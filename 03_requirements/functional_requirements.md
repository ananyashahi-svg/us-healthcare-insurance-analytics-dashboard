# Functional Requirements

## US Healthcare Insurance Analytics Dashboard

**Document Version:** 1.0  
**Status:** MVP Approved  
**Data Classification:** Synthetic / Portfolio-Safe

---

## 1. Purpose

This document defines the functional requirements for the US Healthcare Insurance Analytics Dashboard.

The dashboard will provide business users with a centralized analytical view of:

- Claims volume
- Claims status
- Claim Amount
- Allowed Amount
- Paid Amount
- Spend
- Provider performance
- Service utilization
- Member-level utilization
- Geographic trends

The product uses synthetic data for portfolio demonstration purposes.

---

# 2. User Roles

## 2.1 Business Analyst

The Business Analyst uses the dashboard to:

- Analyze claims trends
- Investigate financial variances
- Identify high-cost providers
- Analyze utilization patterns
- Validate business KPIs
- Support stakeholder reporting

## 2.2 Healthcare Operations User

The Operations User uses the dashboard to:

- Monitor claims activity
- Review spend trends
- Identify unusual patterns
- Compare providers
- Analyze service utilization

## 2.3 Product / Management User

The Product or Management User uses the dashboard to:

- Monitor overall performance
- Track key KPIs
- Identify cost drivers
- Review provider performance
- Support strategic decisions

---

# 3. Functional Requirements

## FR-001 — Dashboard Access

**Requirement**

The system shall provide users with access to the healthcare insurance analytics dashboard.

**Acceptance Criteria**

- Dashboard loads successfully.
- Users can view the main dashboard.
- Dashboard displays the latest available synthetic dataset.
- No real healthcare data is displayed.

---

## FR-002 — KPI Summary

**Requirement**

The dashboard shall display high-level insurance analytics KPIs.

**Required KPIs**

- Total Claims
- Total Claim Amount
- Total Allowed Amount
- Total Paid Amount
- Total Spend
- Unique Members
- Average Claim Amount
- Average Allowed Amount
- Average Spend

**Acceptance Criteria**

- KPI cards are displayed prominently.
- KPI values update when filters are applied.
- KPI definitions remain consistent with the approved KPI dictionary.

---

# 4. Claims Analytics

## FR-003 — Claims Volume

The system shall display total claims volume.

**Acceptance Criteria**

- Total claims are calculated from unique claim IDs.
- Claim count updates based on selected filters.
- Claim-line duplication must not inflate claim counts.

---

## FR-004 — Claims Status

The dashboard shall display claims by status.

**Supported statuses**

- Paid
- Denied
- Pending
- Adjusted

**Acceptance Criteria**

- Users can see the distribution of claims by status.
- Status counts reconcile with the underlying claims dataset.
- Status filters update other dashboard metrics.

---

## FR-005 — Claims Trend

The dashboard shall display claims trends over time.

**Acceptance Criteria**

- Users can view claims by month.
- Users can identify increasing or decreasing claim volume.
- Date filters update the trend.

---

# 5. Financial Analytics

## FR-006 — Claim Amount

The system shall display the total provider-submitted Claim Amount.

**Definition**

```text
Claim Amount = SUM(claim_amount)
FR-007 — Allowed Amount

The system shall display the total Allowed Amount.

Definition

Allowed Amount = SUM(allowed_amount)

Business Rule

allowed_amount <= claim_amount
FR-008 — Paid Amount

The system shall display total payer Paid Amount.

Definition

Paid Amount = SUM(paid_amount)
FR-009 — Spend

The system shall display total analytical payer Spend.

For the MVP:

Spend = SUM(spend_amount)

The portfolio definition is:

spend_amount = paid_amount
FR-010 — Financial Trend

The dashboard shall display financial trends over time.

Users shall be able to analyze:

Claim Amount
Allowed Amount
Paid Amount
Spend

by month.

6. Provider Analytics
FR-011 — Provider Performance

The dashboard shall provide provider-level analytics.

Users shall be able to compare providers using:

Claim Volume
Total Spend
Average Spend per Claim
Allowed Amount
Paid Amount
Synthetic Quality Score
FR-012 — Top Providers

The dashboard shall identify high-volume and high-spend providers.

Acceptance Criteria

Users can identify:

Top providers by claims
Top providers by spend
Top providers by average claim cost
FR-013 — Provider Filtering

Users shall be able to filter analytics by provider.

Filtering by provider shall update relevant:

KPI cards
Claims trends
Financial metrics
Service metrics
7. Service Analytics
FR-014 — Service Utilization

The dashboard shall display utilization by service category.

Example categories:

Primary Care
Specialist
Emergency
Inpatient
Outpatient
Laboratory
Imaging
Pharmacy
Surgery
FR-015 — Service Spend

Users shall be able to analyze Spend by service category.

Acceptance Criteria

Service categories can be compared.
Spend values reconcile with claim-level data.
Users can identify high-spend services.
8. Member Analytics
FR-016 — Unique Members

The dashboard shall display the number of unique members represented in the selected dataset.

Definition

Unique Members = COUNT(DISTINCT member_id)
FR-017 — Claims per Member

The dashboard shall support claims-per-member analysis.

Definition

Claims per Member =
Total Claims / Unique Members
FR-018 — Spend per Member

The dashboard shall support spend-per-member analysis.

Definition

Spend per Member =
Total Spend / Unique Members
9. Geographic Analytics
FR-019 — State-Level Analysis

Users shall be able to analyze claims and spend by US state.

Supported dimensions:

Member State
Provider State

Users shall be able to compare:

Claims
Spend
Allowed Amount
Claim Amount

by state.

10. Dashboard Filters
FR-020 — Date Filter

Users shall be able to filter the dashboard by:

Year
Quarter
Month
Date range
FR-021 — Provider Filter

Users shall be able to filter by:

Provider
Provider Type
Specialty
Network Status
FR-022 — Member Filter

Users shall be able to filter by:

Member State
Plan Type
Member Status
Age Group
FR-023 — Service Filter

Users shall be able to filter by:

Service Category
Service Type
Specialty
FR-024 — Claim Status Filter

Users shall be able to filter by:

Paid
Denied
Pending
Adjusted
11. Drill-Down
FR-025 — Provider Drill-Down

Users shall be able to select a provider and view:

Provider claims
Provider spend
Average claim cost
Service mix
Network status
Synthetic quality score
FR-026 — Service Drill-Down

Users shall be able to select a service category and view:

Claim volume
Spend
Allowed Amount
Average cost
Provider distribution
12. Data Validation
FR-027 — Financial Validation

The system shall support validation of:

allowed_amount <= claim_amount

and:

member_responsibility <= allowed_amount

For paid claims:

paid_amount =
allowed_amount - member_responsibility
FR-028 — Referential Integrity

The dataset shall maintain valid relationships between:

members → claims
providers → claims
services → claims
claims → claim_lines
FR-029 — Duplicate Detection

The system shall identify duplicate:

Claim IDs
Member IDs
Provider IDs
Service IDs

where uniqueness is expected.

13. Data Quality
FR-030 — Missing Data

The system shall identify missing required fields.

Required identifiers include:

Claim ID
Member ID
Provider ID
Service ID
FR-031 — Invalid Values

The system shall identify:

Negative financial amounts
Invalid claim statuses
Invalid dates
Invalid member ages
Invalid foreign-key references
14. Export & Reporting
FR-032 — Dashboard Export

The final dashboard should support exporting analytical results where the selected BI platform allows it.

Possible outputs include:

CSV
Excel
PDF
Dashboard screenshots

This capability is considered dependent on the selected visualization platform.

15. Performance Requirements
FR-033 — Dashboard Performance

The dashboard should load the primary KPI view within an acceptable analytical reporting threshold.

Target:

Initial dashboard load <= 5 seconds

for the synthetic MVP dataset under normal local conditions.

FR-034 — Filter Performance

Applying standard dashboard filters should return updated results without significant delay.

Target:

Filter response <= 3 seconds

for the MVP dataset under normal local conditions.

16. Security & Privacy
FR-035 — Synthetic Data Only

The dashboard shall use synthetic data.

The project must not contain:

PHI
PII
Real patient information
Real member information
Real claims
Real provider financial information
17. Auditability
FR-036 — KPI Traceability

Each dashboard KPI shall be traceable to:

Source Field
     ↓
Transformation
     ↓
Calculation
     ↓
Dashboard KPI

Example:

claims.paid_amount
        ↓
SUM(paid_amount)
        ↓
Total Spend
        ↓
Dashboard KPI
18. Functional Requirement Summary
ID	Requirement	Priority
FR-001	Dashboard Access	Must Have
FR-002	KPI Summary	Must Have
FR-003	Claims Volume	Must Have
FR-004	Claims Status	Must Have
FR-005	Claims Trend	Must Have
FR-006	Claim Amount	Must Have
FR-007	Allowed Amount	Must Have
FR-008	Paid Amount	Must Have
FR-009	Spend	Must Have
FR-010	Financial Trend	Must Have
FR-011	Provider Performance	Must Have
FR-012	Top Providers	Must Have
FR-013	Provider Filtering	Should Have
FR-014	Service Utilization	Must Have
FR-015	Service Spend	Must Have
FR-016	Unique Members	Must Have
FR-017	Claims per Member	Should Have
FR-018	Spend per Member	Should Have
FR-019	Geographic Analysis	Should Have
FR-020	Date Filter	Must Have
FR-021	Provider Filter	Must Have
FR-022	Member Filter	Should Have
FR-023	Service Filter	Must Have
FR-024	Claim Status Filter	Must Have
FR-025	Provider Drill-Down	Should Have
FR-026	Service Drill-Down	Should Have
FR-027	Financial Validation	Must Have
FR-028	Referential Integrity	Must Have
FR-029	Duplicate Detection	Must Have
FR-030	Missing Data Detection	Must Have
FR-031	Invalid Value Detection	Must Have
FR-032	Export & Reporting	Should Have
FR-033	Dashboard Performance	Should Have
FR-034	Filter Performance	Should Have
FR-035	Synthetic Data Privacy	Must Have
FR-036	KPI Traceability	Must Have
Document Status

Status: Approved for MVP Development

Next Artifact: User Stories & Acceptance Criteria
