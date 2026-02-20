# Database schema

Here is the list of existing databases in the AMRIT platform.

1\. **db\_identity** -> Stores all beneficiary data.

2\. **db\_1097\_identity** -> Stores 1097 helpline beneficiary data.

3\. **db\_iemr**-> Stores all master & transaction data of 104, 1097, TM, MMU, HWC, ECD service lines.

4\. **db\_reporting** -> Stores All AMRIT service lines reporting related data.

&#x20;![](<../.gitbook/assets/image (3).png>)

## Setting up DB Schema

The **AMRIT DB Service** simplifies the initial setup of database schemas across all AMRIT modules by automating the creation of tables, relationships, and seed data. It ensures consistency and reduces manual effort, making the platform ready for use in various environments. The service leverages Flyway migrations for version control and compatibility. Explore the code and contribute via the [GitHub repository](https://github.com/PSMRI/Amrit-DB/).

***

## Setting up a Dummy Database

Download a pre-configured dummy database to quickly restore and simulate the AMRIT platform's functionality for testing or local development. The zip file includes SQL scripts to populate data for different modules.

{% file src="../.gitbook/assets/AmritMasterData (1).zip" %}

Dummy username and passwords<br>

| Module                              | Username                      | password    |
| ----------------------------------- | ----------------------------- | ----------- |
| 1097                                | 1097User                      | Test@123    |
| ECD Supervisor                      | ECDSupervisor2, ecdsupervisor | Test@123    |
| ECD Associate                       | ECDAssociate                  | Test@123    |
| ECD ANM                             | ECDANM                        | Test@123    |
| ECD MO                              | ECDMedicalOfficer             | Test@123    |
| ECD quality Supervisor              | ECDQualitySupervisor          | Test@123    |
| ECD Quality Auditor                 | ECDQualityAuditor             | Test@123    |
| 104HAO                              | 104hao                        | Test@123    |
| 104MO                               | 104mo                         | Test@123    |
| 104 supervisor                      | 104Supervisor                 | Test@123    |
| 104SIO(service improvement officer) | 104SIO                        | Test@123    |
| 104CO(CounsellingOfficer)           | 104psmri,104co                | Test@123    |
| 104 Surveyor                        | 104Surveyor                   | Test@123    |
| TM(Telemedicine)                    | TMUser                        | Test@123    |
| Admin(ECD, 104)                     | 104ECDAdmin                   | P@ssword@07 |
| Admin(TM,HWC)                       | HWCTMAdmin                    | Test@123    |
| Admin(1097)                         | 1097Admin                     | Test@123    |
| Admin(MMU)                          | MMUAdmin                      | Test@123    |
| HWC                                 | HWCUser                       | Test@123    |
| MMU                                 | MMUUser                       | Test@123    |
| Admin                               | SuperAdmin                    | admin@123   |

## AMRIT DB Architecture

Here is the AMRIT DB Architecture diagram.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## DB Tables

### Beneficiary Registration Tables

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### 104 Tables

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### MMU & TM Tables

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

### 1097 Tables

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Inventory Tables

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### HWC Screening Tables

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

&#x20;
