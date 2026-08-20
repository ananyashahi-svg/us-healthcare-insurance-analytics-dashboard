# Data Model

## US Healthcare Insurance Analytics Dashboard

**Document Version:** 1.0  
**Data Classification:** Synthetic / Portfolio-Safe  
**Model Type:** Analytics / Star Schema  
**Purpose:** Define the logical and analytical data model supporting claims, financial, provider, member, service, and dashboard analytics.

---

## 1. Purpose

This document defines the logical and analytical data model for the US Healthcare Insurance Analytics Dashboard.

The model is designed to support:

- Claims analytics
- Financial analytics
- Allowed Amount analysis
- Claim Amount analysis
- Paid Amount analysis
- Spend analysis
- Provider performance
- Member utilization
- Service utilization
- Geographic analysis
- Time-series analysis
- KPI calculations
- SQL analytics
- Dashboard reporting
- Data-quality validation

The model uses synthetic and portfolio-safe data only.

No real patient, provider, payer, healthcare organization, or claims data is used.

---

## 2. Modeling Approach

The analytical model follows a simplified star-schema approach.

The model contains two primary types of tables.

### Fact Tables

Fact tables contain measurable healthcare insurance business events.

- `fact_claims`
- `fact_claim_lines`

### Dimension Tables

Dimension tables contain descriptive attributes used for filtering, grouping, and analysis.

- `dim_member`
- `dim_provider`
- `dim_service`
- `dim_date`
- `dim_geography`

---

## 3. Analytical Model Overview

The central analytical table is:

`fact_claims`

The fact table connects to the following dimensions:

| Dimension | Primary Key | Foreign Key in Fact | Relationship |
|---|---|---|---|
| dim_member | member_id | member_id | 1-to-many |
| dim_provider | provider_id | provider_id | 1-to-many |
| dim_service | service_id | service_id | 1-to-many |
| dim_date | date_key | date_key | 1-to-many |
| dim_geography | geography_id | geography_id | 1-to-many |

Claim-level records can also connect to:

`fact_claim_lines`

Relationship:

`fact_claims.claim_id → fact_claim_lines.claim_id`

---

## 4. Fact Claims

### Table

`fact_claims`

### Grain

One row represents one unique claim.

This is the primary fact table for executive and financial analytics.

### Key Measures

The table contains:

- Claim Amount
- Allowed Amount
- Member Responsibility
- Paid Amount
- Spend Amount

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| claim_id | VARCHAR | PK | Unique synthetic claim identifier |
| member_id | VARCHAR | FK | Member associated with the claim |
| provider_id | VARCHAR | FK | Provider associated with the claim |
| service_id | VARCHAR | FK | Service associated with the claim |
| date_key | INTEGER | FK | Service date reference |
| claim_status | VARCHAR | | Claim processing status |
| claim_amount | DECIMAL | | Provider-submitted amount |
| allowed_amount | DECIMAL | | Payer-recognized allowed amount |
| member_responsibility | DECIMAL | | Synthetic member responsibility |
| paid_amount | DECIMAL | | Synthetic payer payment |
| spend_amount | DECIMAL | | Analytical payer spend |
| place_of_service | VARCHAR | | Simplified service location |
| geography_id | INTEGER | FK | Geographic reference |

---

## 5. Fact Claims Financial Logic

The financial fields follow defined business rules.

### Claim Amount

`claim_amount`

Represents the provider-submitted or billed amount.

### Allowed Amount

`allowed_amount`

Represents the amount recognized as allowed under the simplified analytical payer model.

Business rule:

`allowed_amount <= claim_amount`

### Member Responsibility

`member_responsibility`

Represents the synthetic portion of the allowed amount assigned to the member.

Business rule:

`member_responsibility <= allowed_amount`

### Paid Amount

`paid_amount`

Represents the synthetic payer payment.

Business rule for applicable paid claims:

`paid_amount = allowed_amount - member_responsibility`

### Spend Amount

`spend_amount`

Represents analytical payer spending.

For the MVP:

`spend_amount = paid_amount`

This definition is intentionally fixed to maintain consistency across SQL and dashboard calculations.

---

## 6. Dim Member

### Table

`dim_member`

### Grain

One row per synthetic member.

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| member_id | VARCHAR | PK | Unique synthetic member identifier |
| member_age | INTEGER | | Member age |
| gender | VARCHAR | | Synthetic demographic category |
| state | VARCHAR | | Member state |
| plan_type | VARCHAR | | Insurance plan category |
| enrollment_date | DATE | | Synthetic enrollment date |
| member_status | VARCHAR | | Active or inactive member status |

### Example Plan Types

- PPO
- HMO
- EPO
- POS

---

## 7. Dim Provider

### Table

`dim_provider`

### Grain

One row per synthetic healthcare provider.

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| provider_id | VARCHAR | PK | Unique synthetic provider identifier |
| provider_name | VARCHAR | | Synthetic provider name |
| provider_type | VARCHAR | | Provider category |
| specialty | VARCHAR | | Provider specialty |
| state | VARCHAR | | Provider state |
| network_status | VARCHAR | | In-network or out-of-network |
| quality_score | DECIMAL | | Synthetic analytical provider score |

### Example Provider Types

- Hospital
- Physician
- Clinic
- Laboratory
- Imaging Center
- Specialist

### Example Specialties

- Primary Care
- Cardiology
- Orthopedics
- Emergency Medicine
- Oncology
- Radiology
- Dermatology
- General Surgery

### Quality Score

`quality_score`

Represents a synthetic analytical score used for portfolio demonstration.

Range:

`0-100`

This is not a real clinical quality rating.

---

## 8. Dim Service

### Table

`dim_service`

### Grain

One row per synthetic service definition.

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| service_id | VARCHAR | PK | Unique service identifier |
| service_category | VARCHAR | | High-level service category |
| service_name | VARCHAR | | Service description |
| service_type | VARCHAR | | Inpatient, outpatient, or professional |
| specialty | VARCHAR | | Associated specialty |

### Example Service Categories

- Primary Care
- Specialist
- Emergency
- Inpatient
- Outpatient
- Laboratory
- Imaging
- Pharmacy
- Surgery

### Example Services

- Primary Care Visit
- MRI Scan
- Emergency Department Visit
- Specialist Consultation
- Laboratory Test
- Surgical Procedure

---

## 9. Dim Date

### Table

`dim_date`

### Grain

One row per calendar date.

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| date_key | INTEGER | PK | Numeric date key |
| full_date | DATE | | Calendar date |
| day | INTEGER | | Day of month |
| month | INTEGER | | Month number |
| month_name | VARCHAR | | Month name |
| quarter | INTEGER | | Quarter number |
| year | INTEGER | | Calendar year |
| week | INTEGER | | Week number |
| day_of_week | VARCHAR | | Day name |

### Primary Analytical Date

The dashboard will primarily use:

`service_date`

for utilization and financial trend analysis.

---

## 10. Dim Geography

### Table

`dim_geography`

### Grain

One row per geographic entity.

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| geography_id | INTEGER | PK | Geography identifier |
| state | VARCHAR | | US state |
| region | VARCHAR | | US geographic region |
| market | VARCHAR | | Simplified market grouping |

### Geographic Analysis

The model supports:

- State-level analysis
- Regional analysis
- Provider geography
- Member geography
- Spend by geography
- Claims by geography

---

## 11. Fact Claim Lines

### Table

`fact_claim_lines`

### Grain

One row represents one service line within a claim.

A single claim may contain multiple claim lines.

### Fields

| Field | Data Type | Key | Description |
|---|---|---|---|
| claim_line_id | VARCHAR | PK | Unique claim-line identifier |
| claim_id | VARCHAR | FK | Parent claim identifier |
| service_id | VARCHAR | FK | Service represented by the line |
| procedure_code | VARCHAR | | Synthetic procedure code |
| units | INTEGER | | Number of service units |
| line_claim_amount | DECIMAL | | Line-level submitted amount |
| line_allowed_amount | DECIMAL | | Line-level allowed amount |
| line_paid_amount | DECIMAL | | Line-level paid amount |

---

## 12. Claim-Level vs Claim-Line Grain

This distinction is critical for accurate analytics.

### Claim-Level Grain

`1 row = 1 unique claim`

Use for:

- Total Claims
- Claim Amount
- Allowed Amount
- Paid Amount
- Spend
- Denial Rate

### Claim-Line Grain

`1 row = 1 service line`

Use for:

- Service utilization
- Procedure analysis
- Units
- Line-level financial analysis
- Service-level trends

---

## 13. Relationship Summary

| Parent Table | Child Table | Relationship |
|---|---|---|
| dim_member | fact_claims | 1-to-many |
| dim_provider | fact_claims | 1-to-many |
| dim_service | fact_claims | 1-to-many |
| dim_date | fact_claims | 1-to-many |
| dim_geography | fact_claims | 1-to-many |
| fact_claims | fact_claim_lines | 1-to-many |

---

## 14. Primary and Foreign Keys

### Primary Keys

Each table contains a unique primary identifier.

Examples:

- `member_id`
- `provider_id`
- `service_id`
- `date_key`
- `geography_id`
- `claim_id`
- `claim_line_id`

### Foreign Keys

The primary relationships are:

`fact_claims.member_id → dim_member.member_id`

`fact_claims.provider_id → dim_provider.provider_id`

`fact_claims.service_id → dim_service.service_id`

`fact_claims.date_key → dim_date.date_key`

`fact_claims.geography_id → dim_geography.geography_id`

`fact_claim_lines.claim_id → fact_claims.claim_id`

`fact_claim_lines.service_id → dim_service.service_id`

---

## 15. Star Schema Rationale

A star-schema-oriented model was selected because the primary use case is analytical reporting.

### Performance

Common aggregations are straightforward.

Example:

```sql
SUM(spend_amount)
COUNT(DISTINCT claim_id)
Business Usability

Business users can analyze metrics using familiar dimensions:

Provider
Member
Service
Date
Geography
Plan Type
KPI Consistency

Centralizing financial measures in the fact table reduces inconsistent metric calculations.

Dashboard Compatibility

The model can be consumed by common BI tools such as:

Power BI
Tableau
Looker
Excel Power Pivot
16. Fact vs Dimension
Fact Tables

Facts represent measurable business events.

fact_claims

Measures:

Claim Amount
Allowed Amount
Member Responsibility
Paid Amount
Spend Amount

fact_claim_lines

Measures:

Units
Line Claim Amount
Line Allowed Amount
Line Paid Amount
Dimension Tables

Dimensions describe the business context.

dim_member

Examples:

Age
Gender
State
Plan Type

dim_provider

Examples:

Provider Type
Specialty
State
Network Status

dim_service

Examples:

Service Category
Service Type
Specialty

dim_date

Examples:

Month
Quarter
Year

dim_geography

Examples:

State
Region
Market
17. Grain Management

Grain must be explicitly controlled before calculating KPIs.

Claim Grain

1 row = 1 claim

Primary source:

fact_claims

Claim-Line Grain

1 row = 1 claim service line

Primary source:

fact_claim_lines

Member Grain

1 row = 1 member

Primary source:

dim_member

Provider Grain

1 row = 1 provider

Primary source:

dim_provider

Service Grain

1 row = 1 service definition

Primary source:

dim_service

18. Avoiding Double Counting

A common analytical risk occurs when claim-level data is joined to claim-line data.

Example:

1 Claim → 3 Claim Lines

If the claim amount is joined to all three lines, the claim amount can appear three times.

Therefore:

Claim-level financial KPIs must be calculated from fact_claims unless the analysis specifically requires claim-line data.

For example:

Correct claim count:

COUNT(DISTINCT claim_id)

Incorrect approach when using claim-line data:

COUNT(claim_id)

when multiple lines exist for the same claim.

19. KPI Mapping
KPI	Primary Source	Calculation
Total Claims	fact_claims	COUNT(DISTINCT claim_id)
Claim Amount	fact_claims	SUM(claim_amount)
Allowed Amount	fact_claims	SUM(allowed_amount)
Paid Amount	fact_claims	SUM(paid_amount)
Spend	fact_claims	SUM(spend_amount)
Unique Members	fact_claims	COUNT(DISTINCT member_id)
Claims per Member	fact_claims	Claims / Members
Spend per Member	fact_claims	Spend / Members
Average Claim Amount	fact_claims	Claim Amount / Claims
Average Allowed Amount	fact_claims	Allowed Amount / Claims
Denial Rate	fact_claims	Denied Claims / Total Claims
Provider Claims	fact_claims	Claims grouped by provider
Provider Spend	fact_claims	Spend grouped by provider
Service Utilization	fact_claim_lines	SUM(units)
Average Spend per Claim	fact_claims	Spend / Claims
20. Core Financial Relationships

The simplified financial model uses:

Allowed Amount <= Claim Amount

and:

Member Responsibility <= Allowed Amount

For paid claims:

Paid Amount = Allowed Amount - Member Responsibility

For the MVP:

Spend Amount = Paid Amount

These rules will be validated during synthetic data generation and SQL quality checks.

21. Analytical Data Flow

The analytical flow is:

Synthetic Source Data

↓

Data Validation

↓

Data Transformation

↓

Dimension Tables

↓

Fact Tables

↓

SQL Analytics

↓

Dashboard Dataset

↓

BI Dashboard

22. Data Quality Layer

Before analytical consumption, the following checks should be performed.

Key Validation
Primary-key uniqueness
Foreign-key integrity
Duplicate identifiers
Missing identifiers
Financial Validation
Negative amounts
Allowed Amount greater than Claim Amount
Member Responsibility greater than Allowed Amount
Paid Amount inconsistent with financial rules
Date Validation
Invalid dates
Claim dates before enrollment
Future service dates where not allowed
Invalid date keys
Business Validation
Invalid claim statuses
Invalid provider relationships
Invalid service relationships
Invalid plan types
Invalid network statuses
23. Data Lineage

The model supports the following analytical lineage:

Business Requirement

↓

KPI Definition

↓

Source Field

↓

Transformation Logic

↓

Fact / Dimension

↓

SQL Query

↓

Dashboard Metric

This ensures that every dashboard KPI can be traced back to a defined business field and calculation.

24. Slowly Changing Dimensions

The MVP does not require full Slowly Changing Dimension implementation.

However, in a production environment, some dimension attributes may change over time.

Potential examples include:

Provider network status
Member plan
Provider specialty
Provider location

A future production implementation could use:

SCD Type 2

to preserve historical versions of changing dimension records.

SCD implementation is outside the MVP scope.

25. Analytical Security Considerations

Because this is a healthcare insurance analytics use case, production implementations would require appropriate access controls.

Potential controls include:

Role-based access
Least-privilege access
Data masking
Audit logging
Encryption
Environment separation
PHI/PII controls
Row-level security

The portfolio dataset does not contain real PHI or PII.

26. Portfolio Scope
Included in MVP
Claim-level fact
Claim-line fact
Member dimension
Provider dimension
Service dimension
Date dimension
Geography dimension
Financial measures
KPI mapping
Data-quality rules
Grain definitions
Relationship definitions
Data lineage
Excluded from MVP
Real payer adjudication engine
Real EDI transactions
Real PHI
Real provider contracts
Real benefit configuration
Real payment processing
Production healthcare integrations
Production security implementation
27. Future Extensions

Potential future additions include:

Diagnosis category
Synthetic procedure category
CPT-like synthetic codes
ICD-like synthetic codes
Network tier
Benefit plan
Deductible status
Copay
Coinsurance
Facility type
Region
Risk category
Provider contract attributes
Claim adjustment history

These features are outside the initial MVP.

28. Design Principles

The data model follows these principles:

Define the grain before calculating metrics.
Separate measurable facts from descriptive dimensions.
Centralize financial measures.
Prevent claim and claim-line double counting.
Maintain consistent KPI definitions.
Preserve data lineage from requirement to dashboard.
Validate financial and referential integrity.
Keep synthetic data clearly separated from real healthcare data.
Design the model for analytical scalability.
Keep the MVP simple enough for portfolio demonstration.
29. Final Analytical Traceability

The complete product analytics chain is:

Business Requirement

→ KPI Definition

→ Data Dictionary

→ Data Model

→ Synthetic Dataset

→ SQL

→ Analytics

→ Dashboard

→ Product Metrics

This ensures consistency between the business, data, analytics, and product layers.
