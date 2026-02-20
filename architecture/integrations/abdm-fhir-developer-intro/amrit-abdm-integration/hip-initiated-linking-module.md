# HIP-Initiated Linking Module

### Overview

This module enables a Health Information Provider (HIP) to initiate linking of a patient’s care-context (hospital visit, OPD record, etc) with the patient’s ABHA (Ayushman Bharat Health Account) as per the ABDM V3 specifications. The system spans UI, FHIR API layer, and HIP service (background processing) and ensures full flow from doctor submission → ABHA link token → care-context linked.

### Scope & Context

* Epic Link: _Upgrade AMRIT ABDM M2 to V3 APIs_
* Service line: Platform
* Environment: All
* Version currently: release-3.9.0 (based on comment on the ticket) AMM-1856
* Tickets:
  * Core Story (API + flow): AMRITAMM-1307
  * UI Changes: AMRITAMM-1811
  * FHIR API Changes: AMRITAMM-1809
  * HIP Service Changes: AMRITAMM-1810
* Dependency on ABDM V3 spec (link token workflow, care-context linking).

### Key Concepts (based on ABDM spec)

* **ABHA Address / ABHA Number**: The unique health account identifier in the ABDM ecosystem.
* **Care Context**: A logical grouping of healthcare records for a patient (visit, prescription, lab result etc) that needs linking with ABHA.
* **Linking Token / RequestId**: In V3 workflows, after authentication/consent, the system issues a token (or requestId) that the HIP uses to link the care context.&#x20;
* **Polling/Async Callback**: Because linking may be asynchronous, HIP may need to poll for token or wait for callback to complete linking.

### Detailed Flow (Module Breakdown)

#### Stage 1: Trigger / Patient Visit Initiation (UI)

* After a doctor submits an encounter in the hospital EMR/portal, the UI triggers a “Link care-context to ABHA” modal. (Ticket 1811)
  * Input fields: ABHA number, Name, Gender, Year of Birth (must match Aadhaar‐registered details).
  * On submit: UI calls `POST /careContext/generateLinkToken` (FHIR layer).
* UI must show a “Linking in progress…” indicator and disable further inputs while waiting for response.
* UI error handling:
  * If name/year mismatch detected → show message: _“Entered details don’t match Aadhaar. Kindly provide Aadhaar-provided name and retry.”_

#### Stage 2: Generate Link Token (FHIR API)

**Endpoint**: `POST /careContext/generateLinkToken`

* Request payload should include: ABHA number, name, gender, yearOfBirth, visitCode, visitCategory, facilityId, facilityName.
* FHIR layer logic:
  * Validate inputs (non-null, ABHA format).
  * Authenticate to ABDM gateway (OAuth2.0/JWT) as required per V3 spec.
  * Call ABDM’s “GenerateLinkToken” API.
    * If HTTP 202 Accepted → capture `requestId` in response.
    * If error (invalid details, duplicate request) → propagate error to UI.
* FHIR returns to UI:

```json
{
  "data": {
    "requestId": "xxxxx-xxx-xxxx-xxxx-xxxxxxxxxxx"
  },
  "statusCode": 200,
  "errorMessage": "Success",
  "status": "Success"
}
```

* Store in data-store (MongoDB or SQL) the mapping: requestId → initial status = “tokenRequested”, timestamp, ABHA number, facility id, etc.
* Logging: record requestId, ABHA number (masked if necessary), timestamp, facility id, retry count = 0.

#### Stage 3: Background Callback Processing (HIP Service)

* The HIP Service (ticket 1810) runs a background job/process that listens for ABDM callbacks (e.g., “on\_generateLink” or equivalent).
* On receiving callback: extract `requestId`, `linkToken`, status (whether token ready).
* Update the data‐store entry (requestId) to status = “tokenReady”, store `linkToken`, timestamp tokenGenerated.

#### Stage 4: Poll & Link Care Context (FHIR API)

**Endpoint**: `GET/POST /careContext/linkCareContext`

* Input: `requestId`
* Logic:
  1. Fetch record by requestId
     * If not found → return `status=pending`, `retry=true`
     * If found but status “tokenRequested” → return `status=pending`, retry
     * If found and status “tokenReady” with `linkToken` → proceed
  2. Use `linkToken` and call ABDM’s LinkCareContext API (send careContext details: each visit/encounter reference, etc)
  3. Evaluate ABDM response:
     * If success → update record status = “linked”, record timestamp, careContextReferences
     * If failure (duplicate, mismatch) → update status = “linkFailed”, record error code/message
  4. Return to UI: success or error
* Polling logic: UI will call this endpoint every \~5 seconds until either success, failure, or retry-limit reached (first try would be instant where next two retries will have an interval of 5 secs)
* Data-store must keep retryCount, lastPolled timestamp, error(s) if any.
* Logging/tracing: record interactions, durations (requestId→token→link), failures for root-cause analysis.

#### Stage 5: UI Finalization

* If FHIR API returns success → UI shows “Care context linked with your ABHA successfully.”
* If FHIR API returns failure with specific error → UI shows user-friendly message (map internal error codes to UX messages).
* If UI retries exceed threshold (e.g., 5 tries) and still “pending” → show fallback: _“We couldn’t link automatically. Please try manual linking or contact staff.”_
* Optional: UI logs this fallback event for system analytics.

### Sequence Diagrams (PlantUML)

#### 1. Full End-to-End Sequence

<figure><img src="../../../../.gitbook/assets/VLFFJXjF3BxlKrWSVlfL3GhboA42aW9HKuX8K2zSd5sJZkATsSvuKbOL5QS-GDLBfVRbU99wPqF-H2eZ8NRip_u-VvuS1q4liUGCSt4URW9vm01xVPVkJzz_m0Z5a1Mu6UnbbQ9DRYhK-zx5nruJLBp-sqwTNGokuMGqW27Mv1Ea2UtK3qOm9hymEIN4ydiRT7BQ_m3dAFG5wuHg0bcMXEw_LM8uWbPizrceU3ERaG-tzkdnzz3OEUJeQJz.png" alt=""><figcaption></figcaption></figure>



#### 2. Polling & Re-try logic sub-flow

<figure><img src="../../../../.gitbook/assets/VLB1RXCn4BtxAvvw0jf2AswLKhMaAOdKqcgJgfTSfhl3refZB_OiRLHLoeaFG1pXoeVbIx2Nx4P1WAMrrZFlZTyypqaGybBFZQ9SdCVD21ImS-wT5RO5NWnl-p7uSaTL4QTpNP6Lc1-ECfnUZtOIEXrKGK9t51YNpfOkXS_UAHNBxH-ZGH_X-ceymPkIBNiIlj1sDd4pjbEx-l8LdZ_L76HKg8IEhCYFd94Dx0LFNNeFGKZgq9F4pNw7EOO.png" alt=""><figcaption></figcaption></figure>

#### 3. Data Store & Background Callback Processing

<figure><img src="../../../../.gitbook/assets/VL1DJy904BtlhvXm1I_64ua6a21Dm12iDnCpxOvqmtQtxcxvYE6_EuijiPZOIsSdx-FD6_c0BjIb5XN_LQugD05witcoXl4gwG5wY0yXCM26dc1fpSR6zNJIvSoJ5RoGswcS8gsTkw3nPBg49moqnoCyejOPvji8lpo4E9lVFB01324ndbm0GtjOABDTqsTDFhoPBt0_Oo87W5c1ptMPaHi07nHgVn_ibao39eRaZf27SO2JQAtOf4t9Jcl.png" alt=""><figcaption></figcaption></figure>
