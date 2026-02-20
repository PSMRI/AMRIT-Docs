# To Do/Future Enhancements

#### 1. ABHA Generation Consent Persistence

* The **ABHA generation consent ID** must be persisted in database tables.
* Currently, whether the patient has **accepted or rejected ABDM consent** is **not stored anywhere in the system**.
* Only UI-level display exists; no backend or database tracking is implemented.
* Design and implement proper storage to maintain consent status and consent ID for audit and compliance purposes.

#### 2. ABHA Generation via Biometric – Device Upgrade Issue

* **ABHA generation through biometric authentication is not working** after the biometric device version upgrade.
* The **device invocation / integration code** exists in the `commonAPI` module.
* All **installation and upgrade documents** have already been shared via the mail chain.
* Investigate compatibility issues with the upgraded device SDK/driver and update the integration accordingly.

#### 3. Incorrect Role Capture in FHIR Bundles (Doctor / Nurse / Lab Technician)

* **Nurse, Doctor, Lab Technician (and Pharmacist)** roles are not being captured correctly in FHIR bundles.
* Required changes:
  * Add new columns in `t_benvisitdetails` table to capture **who performed the visit** (doctor, nurse, lab technician, pharmacist, etc.).
  * Jira ticket **AMM-2015** has been created with detailed requirements.
* Once DB changes are completed:
  * Update the **SP logic**, specifically `FHIR_R_Practitioner`.
  * Ensure the **doctor/practitioner name and role** are fetched from the updated table instead of the current logic.

#### 4. Missing SNOMED CT (SCT) Codes for Medication & Immunization

* **MedicationRequest** and **Immunization** resources currently **do not have SNOMED CT (SCT) codes**.
* SCT codes are presently available only for:
  * Diagnosis
  * Chief complaints
  * Similar clinical entities
* If SCT code mapping is implemented:
  * Update the **FHIR bundle stored procedures** to include SCT codes for MedicationRequest and Immunization resources.
* Without SCT codes, generating these FHIR bundles meaning not maintaining the standards.

#### 5. FHIR Bundle Fallback Handling & Processed Flag Redesign

* Improve and standardize **fallback handling** in the common `ServiceImpl` used for:
  * FHIR bundle creation
  * Saving bundles to MongoDB
* Redesign the **processed flag** logic:
  * When a required bundle is **not generated or partially generated**, the flag should be set appropriately (e.g., false or failed).
  * Fallback scenarios must be clearly identifiable and traceable.

#### 7. ABHA Address Linking Issue – Link Token & Facility Mapping

* An issue exists while saving **ABHA address linking information**.
* Current behavior:
  * In `GenerateTokenAbdmResponses` collection, the **link token is saved only against the ABHA address**.
* Problem:
  * If the **same ABHA address** is linked in **multiple facilities**, using the same link token causes failures.
* Required fix:
  * Save the **link token mapped to both**:
    * ABHA address
    * **ABDM Facility ID**
* This ensures the same ABHA address can be linked independently across multiple facilities without conflict.
