# Cmbio IP configuration

Edit this file once, then copy it into `skills/disclose/references/` and `skills/screen/references/`. Every skill in the plugin reads it. Undecided values say TBD.

## People

| Role | Name | Contact |
|---|---|---|
| IP owner (receives every IDF, assigns numbers, runs the screen) | TBD | TBD |
| Backup IP owner | TBD | TBD |
| QA / DPO (anything touching client data or client-derived results) | Nikolaj | TBD |
| Review panel | IP owner, one senior scientist outside the project, QA/DPO when client data is involved | |

## Entities

Contracting entities are Clinical Microbiomics A/S (global) and CosmosID, Inc. (US). The IDF names the entity the inventor is employed by.

## Language

Interview the inventor in whichever language they prefer, Danish or English. Write the IDF in English unless the inventor asks otherwise, since the patent firm, the US entity and international colleagues all need to read it. The Frascati section may additionally be produced in Danish on request for the auditor.

## Where finished IDFs go

- Inventor sends the confirmed IDF to the IP owner by TBD.
- The IP owner logs it in TBD and assigns the number `IDF-YYYY-NNN`.
- Draft IDFs produced by the skill carry the placeholder `IDF-YYYY-DRAFT` until logged.

## Timing rules

- Disclosure hold: from the moment an IDF is started, the inventor does not present, publish, push publicly or send detailed methods to anyone outside Cmbio without an NDA, until the IP owner confirms a route.
- Urgent flag: any planned external disclosure inside 90 days is flagged at the top of the IDF and the inventor is told to contact the IP owner the same day.
- Panel meets monthly. Batches go to the patent firm in February and September.

## Patent firm

| Field | Value |
|---|---|
| Firm | TBD |
| Contact | TBD |
| Engagement scope | Per-disclosure patentability and route assessment, batched twice a year. No claim drafting until a filing decision is made. |

## Data rules for IDFs

- No client names in the technical sections. Refer to "a sponsor project" and put the contract reference in section E only.
- No raw client data, sequences, sample identifiers or patient-level information. Point to where the data lives.
- No personal data about anyone other than the named inventors.
