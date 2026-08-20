# Data Dictionary

## US Healthcare Insurance Analytics Dashboard

**Document Version:** 1.0  
**Data Classification:** Synthetic / Portfolio-Safe  
**Purpose:** Define the analytical data model and field-level business meaning.

---

## 1. Purpose

This document defines the fields used in the synthetic US healthcare insurance analytics dataset.

The data model is designed to support:

- Claims analytics
- Financial analytics
- Provider performance
- Member utilization
- Service analytics
- KPI calculations
- SQL analysis
- Dashboard reporting
- Data-quality validation

No real patient, provider, payer, or healthcare organization data is used.

---

## 2. Data Model Overview

The analytical model contains the following core entities:

```text
Members
   │
   └──────────────┐
                  ↓
               Claims
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
   Claim Lines           Providers
        │
        ↓
    Services

Claims ───────→ Geography
Claims ───────→ Date
3. Members Table
Table Name

members

Grain

One record per synthetic member.

Field	Data Type	Key	Description
member_id	VARCHAR	PK	Unique synthetic member identifier
member_age	INTEGER		Member age
gender	VARCHAR		Synthetic gender category
state	VARCHAR		Member state
plan_type	VARCHAR		Insurance plan category
enrollment_date	DATE		Synthetic enrollment date
member_status	VARCHAR		Active/inactive member status
3.1 member_id

Example: MEM-000001

Unique identifier for the synthetic member.

Rules:

Must be unique
Cannot be null
Must follow the synthetic ID format
3.2 member_age

Age of the synthetic member at the reference period.

Example: 42

Rules:

0 <= member_age <= 100

For this portfolio dataset, ages will generally represent adult insurance populations.

3.3 gender

Synthetic demographic category.

Example values:

Female
Male
Non-Binary
Unknown

No personally identifying information is represented.

3.4 state

US state associated with the synthetic member.

Example values:

CA, TX, NY, FL, IL

3.5 plan_type

Synthetic insurance plan category.

Example values:

PPO
HMO
EPO
POS
3.6 enrollment_date

Synthetic date representing the beginning of member enrollment.

3.7 member_status

Example values:

Active
Inactive
4. Providers Table
Table Name

providers

Grain

One record per synthetic healthcare provider.

Field	Data Type	Key	Description
provider_id	VARCHAR	PK	Unique synthetic provider identifier
provider_name	VARCHAR		Synthetic provider name
provider_type	VARCHAR		Provider category
specialty	VARCHAR		Provider specialty
state	VARCHAR		Provider state
network_status	VARCHAR		In-network/out-of-network
quality_score	DECIMAL		Synthetic provider performance score
4.1 provider_id

Example: PRV-000001

Unique synthetic provider identifier.

4.2 provider_name

Synthetic provider name.

Example: Provider Group 001

No real healthcare organization names will be used.

4.3 provider_type

Example values:

Hospital
Physician
Clinic
Laboratory
Imaging Center
Specialist
4.4 specialty

Example values:

Primary Care
Cardiology
Orthopedics
Emergency Medicine
Oncology
Radiology
Dermatology
General Surgery
4.5 state

State where the synthetic provider operates.

4.6 network_status

Example values:

In-Network
Out-of-Network
4.7 quality_score

Synthetic analytical score used for provider-performance demonstrations.

Example: 87.5

Range: 0–100

This is an analytical portfolio metric, not a clinical quality rating.

5. Services Table
Table Name

services

Grain

One record per synthetic service type.

Field	Data Type	Key	Description
service_id	VARCHAR	PK	Unique service identifier
service_category	VARCHAR		High-level service category
service_name	VARCHAR		Service description
service_type	VARCHAR		Inpatient/outpatient/professional
specialty	VARCHAR		Associated specialty
5.1 service_id

Example: SRV-000001

5.2 service_category

Example values:

Primary Care
Specialist
Emergency
Inpatient
Outpatient
Laboratory
Imaging
Pharmacy
Surgery
5.3 service_name

Example values:

Primary Care Visit
MRI Scan
Emergency Department Visit
Specialist Consultation
5.4 service_type

Example values:

Professional
Outpatient
Inpatient
6. Claims Table
Table Name

claims

Grain

One record per unique claim.

Field	Data Type	Key	Description
claim_id	VARCHAR	PK	Unique claim identifier
member_id	VARCHAR	FK	Member associated with claim
provider_id	VARCHAR	FK	Provider associated with claim
service_date	DATE		Date healthcare service occurred
claim_received_date	DATE		Date claim was received
service_id	VARCHAR	FK	Service associated with claim
claim_status	VARCHAR		Claim processing status
claim_amount	DECIMAL		Provider-submitted amount
allowed_amount	DECIMAL		Payer-recognized allowed amount
member_responsibility	DECIMAL		Member responsibility
paid_amount	DECIMAL		Payer-paid amount
spend_amount	DECIMAL		Analytical payer spend
place_of_service	VARCHAR		Simplified service location
7. Claim Financial Fields
7.1 claim_amount

Amount submitted/billed by the provider.

Example: $500.00

Business definition:

Provider-submitted amount represented in the synthetic claim.

7.2 allowed_amount

Amount recognized as allowed under the simplified analytical payer model.

Business rule:

allowed_amount <= claim_amount
7.3 member_responsibility

Synthetic portion of the Allowed Amount assigned to the member.

Business rule:

member_responsibility <= allowed_amount
7.4 paid_amount

Synthetic payer payment amount.

Business rule:

paid_amount = allowed_amount - member_responsibility

This relationship applies to claims represented as paid in the synthetic model.

7.5 spend_amount

Analytical spending measure.

For this project:

spend_amount = paid_amount

This definition is fixed for the MVP.

8. Claim Status

The synthetic dataset will use the following statuses:

Status	Description
Paid	Claim represented as paid
Denied	Claim represented as denied
Pending	Claim remains pending
Adjusted	Claim has an adjustment scenario
9. Claim Lines Table
Table Name

claim_lines

Grain

One record per claim service line.

Field	Data Type	Key	Description
claim_line_id	VARCHAR	PK	Unique claim-line identifier
claim_id	VARCHAR	FK	Parent claim
service_id	VARCHAR	FK	Service represented by line
procedure_code	VARCHAR		Synthetic procedure code
units	INTEGER		Number of service units
line_claim_amount	DECIMAL		Line-level submitted amount
line_allowed_amount	DECIMAL		Line-level allowed amount
line_paid_amount	DECIMAL		Line-level paid amount
10. Claim vs Claim Line Grain

This distinction is critical for analytics.

A claim may contain multiple claim lines.

Example:

Claim CLM-000001
    │
    ├── Line 1
    ├── Line 2
    └── Line 3

Therefore, using:

COUNT(claim_id)

on the claim-line table can overcount claims.

For claim-level KPIs, use:

COUNT(DISTINCT claim_id)

when querying claim lines.

11. Date Dimensions

The initial MVP can derive time-based analytics directly from:

service_date
claim_received_date

The dashboard will primarily use:

service_date

for healthcare utilization and financial trend analysis.

A dedicated dim_date table may be introduced in a future version.

12. Geography

Geographic analysis will initially use:

member.state
provider.state

The dashboard can support:

State-level analysis
Regional analysis
Provider geography
Member geography

A dedicated geography dimension may be introduced in a later version.

13. Foreign-Key Relationships

The primary relationships are:

members.member_id
        │
        ↓
claims.member_id




providers.provider_id
        │
        ↓
claims.provider_id




services.service_id
        │
        ↓
claims.service_id




claims.claim_id
        │
        ↓
claim_lines.claim_id
14. Core KPI Mapping
KPI	Primary Source
Total Claims	claims
Claim Amount	claims
Allowed Amount	claims
Paid Amount	claims
Spend	claims
Unique Members	claims
Claims per Member	claims
Spend per Member	claims
Denial Rate	claims
Provider Claims	claims
Provider Spend	claims
Service Utilization	claims / services
15. Data Grain Rules
Entity	Grain
Claims	One row = one unique claim
Claim Lines	One row = one service line within a claim
Members	One row = one member
Providers	One row = one provider
Services	One row = one service definition

Maintaining the correct grain is critical to preventing KPI overcounting.

16. Synthetic Data Principles

All data must be:

Artificially generated
Non-identifiable
Portfolio-safe
Internally consistent
Reproducible
Suitable for SQL analysis

The dataset must not contain:

Real patient names
Real member identifiers
Real addresses
Real medical records
Real claims
Real provider financial information
17. Data Quality Expectations

The dataset should support validation of:

Duplicate Claim IDs
Missing Member IDs
Missing Provider IDs
Missing Service IDs
Invalid dates
Invalid financial relationships
Negative amounts
Invalid statuses
Referential integrity
18. Future Extensions

Potential future fields include:

Diagnosis category
Procedure category
CPT-like synthetic code
ICD-like synthetic code
Network tier
Benefit plan
Deductible status
Copay
Coinsurance
Facility type
Region
Risk category

These are outside the initial MVP unless required for later analytics.

19. Data Dictionary Governance

Any field added or modified must document:

Field name
Business definition
Data type
Grain
Source
Transformation logic
Validation rule
Downstream KPI impact

This ensures consistency between the data layer, SQL, dashboard, and product requirements.

Document Status

Status: Approved for MVP Data Modeling

Next Artifact: Data Model & Star Schema
