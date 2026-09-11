# Cross-repo dependency map

This page maps the primary AMRIT frontend repositories to the backend services they depend on in local development.

The port numbers below follow the canonical AMRIT repository table in the upstream platform README and the local development guidance in this docs set. `Common-API` is listed separately because it is a shared backend used by multiple service lines rather than a dedicated one-to-one backend for a single UI.

## Direct UI to API mappings

| UI repository | Local dev port | Primary API repository | API port | Shared backend dependency |
| --- | --- | --- | --- | --- |
| Inventory-UI | 4201 | Inventory-API | 8086 | Common-API (8083) |
| MMU-UI | 4202 | MMU-API | 8087 | Common-API (8083) |
| TM-UI | 4203 | TM-API | 8089 | Common-API (8083) |
| HWC-UI | 4204 | HWC-API | 8085 | Common-API (8083) |
| ADMIN-UI | 4205 | Admin-API | 8082 | Common-API (8083) |
| HWC-Scheduler-UI | 4206 | Scheduler-API | 8088 | Common-API (8083) |
| HWC-Inventory-UI | 4207 | Inventory-API | 8086 | Common-API (8083) |
| Scheduler-UI | 4208 | Scheduler-API | 8088 | Common-API (8083) |
| ECD-UI | 4209 | ECD-API | 8084 | Common-API (8083) |
| Helpline1097-UI | 4210 | Helpline1097-API | 8090 | Common-API (8083) |
| Helpline104-UI | 4211 | Helpline104-API | 8091 | Common-API (8083) |

## Shared components and services

| Shared repository | Role | Port |
| --- | --- | --- |
| Common-UI | Shared UI component library used across service lines | - |
| Common-API | Shared backend services consumed by multiple modules | 8083 |

## Backend services that do not have a dedicated UI row here

These are documented in the AMRIT platform inventory, but they are not one-to-one frontend modules in the dependency map above.

| API repository | Port | Notes |
| --- | --- | --- |
| BeneficiaryID-Generation-API | 8092 | Backend service for beneficiary ID generation and management |
| FHIR-API | 8093 | ABDM / FHIR integration service |
| Identity-API | 8094 | Identity and beneficiary management service |
| Identity-1097-API | 8095 | Identity service instance for the 1097 profile |

## Reading the map

Use the table above as a fast dependency checklist when you are wiring a frontend feature to its backend.

1. Start at the UI repository and confirm which API it calls in the browser or environment configuration.
2. Use the API repository to confirm the backend port and the relevant shared dependency on `Common-API`.
3. If the feature spans multiple service lines, expect the UI to depend on more than one API, with shared utility calls often routed through `Common-API`.

## Related docs

* [System architecture overview](system-architecture-overview.md)
* [API Guide](api-guide.md)
* [Codebase structure](../developer-guide/codebase-structure.md)
