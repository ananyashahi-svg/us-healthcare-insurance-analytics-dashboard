# Data Model

## US Healthcare Insurance Analytics Dashboard

**Document Version:** 1.0  
**Data Classification:** Synthetic / Portfolio-Safe  
**Model Type:** Analytics / Star-Schema-Oriented

---

# 1. Purpose

This document defines the logical and analytical data model for the US Healthcare Insurance Analytics Dashboard.

The model is designed to support:

- Claims analytics
- Financial analytics
- Provider performance
- Member utilization
- Service utilization
- Time-series analysis
- KPI calculations
- SQL analytics
- Dashboard reporting

The model uses a simplified star-schema-oriented design.

---

# 2. Modeling Approach

The analytical model separates:

### Fact Data

Business events and measurable transactions.

### Dimension Data

Descriptive attributes used to filter, group, and analyze facts.

The primary analytical fact is:

```text
Fact Claims
The supporting dimensions are:

Dim Member
Dim Provider
Dim Service
Dim Date
Dim Geography
3. Conceptual Model
                         ┌─────────────────┐
                         │   Dim Member    │
                         │─────────────────│
                         │ member_id       │
                         │ age             │
                         │ gender          │
                         │ state           │
                         │ plan_type       │
                         └────────┬────────┘
                                  │
                                  │
┌─────────────────┐               │
│  Dim Provider   │               │
│─────────────────│               │
│ provider_id     │               │
│ provider_name   │               │
│ specialty       │               │
│ provider_type   │               │
│ state           │               │
│ network_status  │               │
└────────┬────────┘               │
         │                        │
         │                        │
         ▼                        ▼
              ┌──────────────────────────┐
              │       Fact Claims        │
              │──────────────────────────│
              │ claim_id                 │
              │ member_id                │
              │ provider_id              │
              │ service_id               │
              │ date_key                 │
              │ claim_status             │
              │ claim_amount             │
              │ allowed_amount           │
              │ member_responsibility    │
              │ paid_amount              │
              │ spend_amount             │
              └───────────┬──────────────┘
                          │
             ┌────────────┼─────────────┐
             │            │             │
             ▼            ▼             ▼
     ┌─────────────┐ ┌───────────┐ ┌──────────────┐
     │ Dim Service │ │ Dim Date  │ │ Dim Geography│
     │─────────────│ │───────────│ │──────────────│
     │ service_id  │ │ date_key  │ │ geography_id │
     │ category    │ │ date      │ │ state        │
     │ name        │ │ month     │ │ region       │
     │ type        │ │ quarter   │ │ market       │
     │ specialty   │ │ year      │ │              │
     └─────────────┘ └───────────┘ └──────────────┘
4. Fact Claims
Table

fact_claims

Grain

One row represents one unique claim.

This grain is critical because the majority of executive KPIs are claim-level metrics.

4.1 Fact Claims Fields
Field	Type	Key	Description
claim_id	VARCHAR	PK	Unique claim identifier
member_id	VARCHAR	FK	Member identifier
provider_id	VARCHAR	FK	Provider identifier
service_id	VARCHAR	FK	Service identifier
date_key	INTEGER	FK	Service date reference
claim_status	VARCHAR		Claim status
claim_amount	DECIMAL		Submitted amount
allowed_amount	DECIMAL		Allowed amount
member_responsibility	DECIMAL		Member responsibility
paid_amount	DECIMAL		Payer payment
spend_amount	DECIMAL		Analytical spend
place_of_service	VARCHAR		Service location
5. Dim Member
Table

dim_member

Grain

One row per synthetic member.

Field	Type	Key	Description
member_id	VARCHAR	PK	Unique member identifier
member_age	INTEGER		Member age
gender	VARCHAR		Gender category
state	VARCHAR		Member state
plan_type	VARCHAR		Insurance plan
enrollment_date	DATE		Enrollment date
member_status	VARCHAR		Member status
6. Dim Provider
Table

dim_provider

Grain

One row per synthetic provider.

Field	Type	Key	Description
provider_id	VARCHAR	PK	Unique provider identifier
provider_name	VARCHAR		Synthetic provider name
provider_type	VARCHAR		Provider category
specialty	VARCHAR		Provider specialty
state	VARCHAR		Provider state
network_status	VARCHAR		Network participation
quality_score	DECIMAL		Synthetic analytical score
7. Dim Service
Table

dim_service

Grain

One row per service definition.

Field	Type	Key	Description
service_id	VARCHAR	PK	Service identifier
service_category	VARCHAR		Service category
service_name	VARCHAR		Service name
service_type	VARCHAR		Service type
specialty	VARCHAR		Associated specialty
8. Dim Date
Table

dim_date

Grain

One row per calendar date.

Field	Type	Key	Description
date_key	INTEGER	PK	Numeric date key
full_date	DATE		Calendar date
day	INTEGER		Day of month
month	INTEGER		Month number
month_name	VARCHAR		Month name
quarter	INTEGER		Quarter
year	INTEGER		Calendar year
week	INTEGER		Week number
day_of_week	VARCHAR		Day name
9. Dim Geography
Table

dim_geography

Grain

One row per geographic entity represented in the analytical model.

Field	Type	Key	Description
geography_id	INTEGER	PK	Geography identifier
state	VARCHAR		US state
region	VARCHAR		US geographic region
market	VARCHAR		Simplified market grouping
10. Claim Lines

Claim lines are maintained separately from the claim-level fact.

Table

fact_claim_lines

Grain

One row per claim service line.

Field	Type	Key	Description
claim_line_id	VARCHAR	PK	Claim line identifier
claim_id	VARCHAR	FK	Parent claim
service_id	VARCHAR	FK	Service identifier
procedure_code	VARCHAR		Synthetic procedure code
units	INTEGER		Service units
line_claim_amount	DECIMAL		Line submitted amount
line_allowed_amount	DECIMAL		Line allowed amount
line_paid_amount	DECIMAL		Line paid amount
11. Relationship Model

The main relationships are:

dim_member
     │
     │ 1 : many
     ▼
fact_claims
     ▲
     │ 1 : many
dim_provider

And:

dim_service
     │
     │ 1 : many
     ▼
fact_claims

And:

dim_date
     │
     │ 1 : many
     ▼
fact_claims
12. Relationship Summary
Parent	Child	Relationship
dim_member	fact_claims	1 : Many
dim_provider	fact_claims	1 : Many
dim_service	fact_claims	1 : Many
dim_date	fact_claims	1 : Many
dim_geography	fact_claims	1 : Many
fact_claims	fact_claim_lines	1 : Many
13. Why Star Schema?

A star-schema-oriented model was selected because the primary use case is analytics.

Benefits include:

Performance

Aggregations such as:

SUM(Spend Amount)
COUNT(DISTINCT Claim ID)

are straightforward.

Usability

Business users can understand dimensions such as:

Provider
Service
Member
Date
Geography

without needing to understand transactional source structures.

Consistent KPIs

Centralizing measures in the fact table reduces inconsistent calculations.

Dashboard Compatibility

The model works well with BI tools such as:

Power BI
Tableau
Looker
Excel Power Pivot
14. Fact vs Dimension
Fact Tables

Contain measurable business events.

fact_claims
fact_claim_lines

Examples:

Claim Amount
Allowed Amount
Paid Amount
Spend Amount
Units
Dimension Tables

Contain descriptive attributes.

dim_member
dim_provider
dim_service
dim_date
dim_geography

Examples:

Provider Specialty
Member Plan Type
Service Category
State
Month
Region
15. Grain Management

Grain must be explicitly controlled.

Claim-Level Grain
1 row = 1 claim

Used for:

Total Claims
Claim Amount
Allowed Amount
Paid Amount
Spend
Denial Rate
Claim-Line Grain
1 row = 1 claim service line

Used for:

Service-level analysis
Procedure analysis
Units
Line-level financial analysis
16. Avoiding Double Counting

A common analytical risk is joining claim-level and claim-line-level data without controlling grain.

Example:

1 Claim
   ↓
3 Claim Lines

A direct aggregation could incorrectly produce:

Claim Amount × 3

Therefore:

Claim-level financial KPIs must be calculated from fact_claims unless the analysis specifically requires claim-line data.

17. KPI → Fact Mapping
KPI	Fact / Logic
Total Claims	fact_claims
Total Claim Amount	fact_claims
Total Allowed Amount	fact_claims
Total Paid Amount	fact_claims
Total Spend	fact_claims
Unique Members	fact_claims.member_id
Claims per Member	Claims / Members
Spend per Member	Spend / Members
Denial Rate	Denied Claims / Total Claims
Provider Spend	fact_claims grouped by provider
Service Utilization	fact_claims / fact_claim_lines
Average Allowed Amount	Allowed Amount / Claims
18. Data Flow

The analytical data flow is:

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
19. Data Quality Layer

Before analytical consumption, the data should be validated for:

Primary-key uniqueness
Foreign-key integrity
Missing values
Duplicate claims
Invalid financial relationships
Invalid dates
Invalid claim statuses
Negative amounts
Referential integrity
20. Slowly Changing Dimensions

The MVP does not require full Slowly Changing Dimension implementation.

However, provider and member attributes may change over time in production environments.

Future versions could implement:

SCD Type 2

for attributes such as:

Provider network status
Member plan
Provider specialty

This is outside the MVP.

21. Portfolio Scope

The model intentionally balances realism with simplicity.

Included:

Claim-level fact
Claim-line fact
Member dimension
Provider dimension
Service dimension
Date dimension
Geography dimension
Financial measures
Data-quality controls

Excluded from MVP:

Real payer adjudication engine
Real EDI transactions
Real PHI
Real provider contracts
Real benefit configuration
Production payment processing
22. Design Principle

The data model follows one core principle:

Business metrics must be calculated at the correct grain and from clearly defined source fields.

This ensures that:

Business Requirement
        ↓
KPI Definition
        ↓
Data Model
        ↓
SQL
        ↓
Dashboard

remains consistent and traceable.
