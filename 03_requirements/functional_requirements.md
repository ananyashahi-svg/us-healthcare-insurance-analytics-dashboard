# Functional Requirements

## US Healthcare Insurance Analytics Dashboard

**Document Version:** 1.0  
**Status:** Draft  
**Data Classification:** Synthetic / Portfolio-Safe

---

# 1. Purpose

This document translates the business and product requirements into detailed functional requirements for the US Healthcare Insurance Analytics Dashboard.

Each requirement describes expected system behavior and provides acceptance criteria that can be used for development, testing, and UAT.

---

# 2. Functional Requirement Structure

Each requirement contains:

- Requirement ID
- Feature
- Description
- Business Rule
- Acceptance Criteria
- Priority

---

# 3. Executive Dashboard

## FR-001 — Display Total Claims

### Description

The system shall display the total number of unique claims within the selected analytical context.

### Business Rule

```text
Total Claims = COUNT(DISTINCT Claim ID)
Acceptance Criteria
KPI displays on the Executive Dashboard.
Each Claim ID is counted once.
Applying valid filters recalculates the KPI.
KPI matches the validated SQL result.
Null Claim IDs are excluded or flagged according to data-quality rules.
Priority

Must Have

FR-002 — Display Total Claim Amount
Description

The system shall display the total submitted/billed Claim Amount.

Business Rule
Total Claim Amount = SUM(Claim Amount)
Acceptance Criteria
KPI displays the aggregated Claim Amount.
KPI responds to applicable filters.
Financial formatting is applied.
KPI reconciles with SQL.
Priority

Must Have

FR-003 — Display Total Allowed Amount
Description

The system shall display the total Allowed Amount independently from Claim Amount.

Business Rule
Total Allowed Amount = SUM(Allowed Amount)
Acceptance Criteria
Allowed Amount is displayed separately.
Claim Amount is not used as a substitute.
KPI responds to filters.
Result reconciles with SQL.
Priority

Must Have

FR-004 — Display Total Spend
Description

The system shall display Total Spend based on the approved project definition.

Business Rule
Spend Amount = Paid Amount
Acceptance Criteria
Total Spend is calculated from Spend Amount.
Spend does not use Claim Amount by default.
Spend responds to dashboard filters.
Spend reconciles with SQL.
Priority

Must Have

4. Claims Analytics
FR-005 — Claims Trend
Description

The system shall display claims volume over time.

Acceptance Criteria

Users can:

View claims by month.
Apply a date range.
Identify increases and decreases.
Hover/select a period to view the associated value.
Priority

Must Have

FR-006 — Claim Status Distribution
Description

The system shall display claims by processing status.

Supported Statuses
Paid
Denied
Pending
Adjusted
Acceptance Criteria
Each supported status is represented.
Users can see claim counts.
Users can see percentages where applicable.
Status filters affect related dashboard components.
Priority

Must Have

FR-007 — Denial Rate
Description

The system shall calculate the percentage of claims with Denied status.

Formula
Denial Rate =
Denied Claims / Total Claims × 100
Acceptance Criteria
Denied claims are correctly identified.
Denominator uses the documented claim population.
Result is displayed as a percentage.
Result is validated against SQL.
Priority

Must Have

5. Financial Analytics
FR-008 — Claim Amount vs Allowed Amount
Description

The system shall provide a comparison between Claim Amount and Allowed Amount.

Acceptance Criteria

Users can:

View both metrics together.
Compare them across time.
Compare them by service.
Compare them by provider.
Priority

Must Have

FR-009 — Average Claim Amount
Description

The system shall calculate the average Claim Amount per unique claim.

Formula
Average Claim Amount =
Total Claim Amount / Total Claims
Acceptance Criteria
Calculation uses unique claims.
KPI responds to filters.
Result is validated against SQL.
Priority

Must Have

FR-010 — Average Allowed Amount
Description

The system shall calculate the average Allowed Amount per unique claim.

Formula
Average Allowed Amount =
Total Allowed Amount / Total Claims
Acceptance Criteria
Calculation uses unique claims.
KPI responds to filters.
Result reconciles with SQL.
Priority

Must Have

6. Provider Analytics
FR-011 — Provider Ranking
Description

The system shall allow providers to be ranked by selected performance metrics.

Supported Metrics
Claims
Spend
Allowed Amount
Average Allowed Amount
Utilization
Acceptance Criteria
Providers are displayed in ranked order.
User can select the metric.
Ranking updates based on active filters.
Provider IDs remain unique.
Priority

Must Have

FR-012 — Provider Comparison
Description

The system shall allow users to compare selected providers.

Comparison Metrics
Claims
Spend
Allowed Amount
Average Allowed Amount
Utilization
Acceptance Criteria
At least two providers can be compared.
Metrics use the same analytical context.
Comparison responds to filters.
Missing values are clearly represented.
Priority

Must Have

FR-013 — Provider Drilldown
Description

Users shall be able to select a provider and investigate provider-level performance.

Drilldown Information
Provider
Specialty
Claims
Claim Amount
Allowed Amount
Paid Amount
Spend
Services
Utilization
Geography
Acceptance Criteria
Selecting a provider opens the provider detail view.
Provider-specific KPIs are recalculated.
User can return to the previous dashboard state.
Priority

Must Have

7. Utilization Analytics
FR-014 — Claims per Member
Description

The system shall calculate the average number of claims per unique member.

Formula
Claims per Member =
Total Claims / Unique Members
Acceptance Criteria
Unique members are counted using Member ID.
Claims are counted using unique Claim IDs.
KPI responds to filters.
Division by zero is prevented.
Priority

Must Have

FR-015 — Spend per Member
Description

The system shall calculate average Spend per unique member.

Formula
Spend per Member =
Total Spend / Unique Members
Acceptance Criteria
Unique Member ID is used.
Spend uses the approved Spend definition.
KPI responds to filters.
Division by zero is handled.
Priority

Must Have

FR-016 — Service Utilization
Description

The system shall display healthcare service volume by service category.

Acceptance Criteria
Service categories are grouped consistently.
Users can sort by utilization.
Filters update results.
Null or unknown service categories are handled.
Priority

Must Have

8. Dashboard Filtering
FR-017 — Date Filter
Description

Users shall be able to filter the dashboard by date or date range.

Acceptance Criteria
Valid dates can be selected.
All applicable visuals respond to the selected period.
The active date range is visible.
Reset returns to the default period.
Priority

Must Have

FR-018 — Provider Filter
Description

Users shall be able to filter analytics by provider.

Acceptance Criteria
Users can select one or more providers.
Applicable dashboard components update.
The active provider filter is visible.
Priority

Must Have

FR-019 — Service Filter
Description

Users shall be able to filter analytics by service category.

Acceptance Criteria
Users can select one or more service categories.
Applicable dashboard components update.
The active service filter is visible.
Priority

Must Have

FR-020 — Claim Status Filter
Description

Users shall be able to filter claims by processing status.

Acceptance Criteria
Users can select supported claim statuses.
Applicable dashboard components update.
The selected status is visible.
Priority

Must Have

FR-021 — Geographic Filter
Description

Users shall be able to filter analytics by supported geographic dimensions.

Acceptance Criteria
Users can select supported geographic values.
Applicable dashboard components update.
The active geographic filter is visible.
Priority

Should Have

FR-022 — Filter Reset
Description

The system shall provide a clear mechanism to remove active filters.

Acceptance Criteria
All filters return to default values.
KPI values return to the default dashboard state.
No stale filters remain active.
Priority

Must Have

9. Drilldown & Detail
FR-023 — KPI Drilldown
Description

Users shall be able to drill from summary metrics into supporting dimensions.

Example
Total Spend
    ↓
Service Category
    ↓
Provider
    ↓
Claim
Acceptance Criteria
Drilldown preserves the selected analytical context.
User can identify the selected dimension.
User can return to the previous level.
Priority

Must Have

FR-024 — Claim Detail
Description

The system shall provide claim-level details.

Fields
Claim ID
Member ID
Provider ID
Service Date
Service Category
Claim Amount
Allowed Amount
Paid Amount
Spend Amount
Claim Status
Acceptance Criteria
Each record displays the correct Claim ID.
Financial values reconcile with the source dataset.
Filters remain applicable.
Duplicate claim display is prevented where claim-level grain is required.
Priority

Must Have

10. Data Quality
FR-025 — Duplicate Claim Detection
Description

The analytical process shall identify duplicate Claim IDs.

Acceptance Criteria
Duplicate IDs can be detected using SQL.
Duplicate records are flagged.
Duplicate records are not silently removed without documentation.
Priority

Must Have

FR-026 — Missing Reference Detection
Description

The system shall identify claims referencing unavailable:

Member IDs
Provider IDs
Service IDs
Acceptance Criteria
Claims with unmatched Member IDs are identified.
Claims with unmatched Provider IDs are identified.
Claims with unmatched Service IDs are identified.
Exceptions are recorded for investigation.
Priority

Must Have

FR-027 — Financial Validation
Description

The system shall identify violations of defined financial rules.

Validation Rules
Allowed Amount <= Claim Amount
Paid Amount <= Allowed Amount
Member Responsibility <= Allowed Amount
Paid Amount + Member Responsibility = Allowed Amount
Spend Amount = Paid Amount
Acceptance Criteria
Each financial rule is evaluated using SQL.
Violating records are identified.
Exception counts are reported.
Valid records are not incorrectly flagged.
Priority

Must Have

11. KPI Reconciliation
FR-028 — SQL Reconciliation
Description

Dashboard KPIs shall be independently validated using SQL.

Acceptance Criteria

For each core KPI:

Dashboard Value = SQL Value

within any explicitly documented rounding tolerance.

Core KPIs
Total Claims
Total Claim Amount
Total Allowed Amount
Total Paid Amount
Total Spend
Unique Members
Claims per Member
Spend per Member
Denial Rate
Priority

Must Have

12. Error & Empty-State Handling
FR-029 — No Data State
Description

If filters return no records, the dashboard shall clearly communicate that no data is available for the selected criteria.

Example
No data available for the selected filters.
Try adjusting the date range or other filters.
Priority

Should Have

FR-030 — Invalid Filter Combination
Description

If a filter combination produces no valid analytical records, the dashboard should display an appropriate empty state rather than misleading zero values.

Acceptance Criteria
The dashboard clearly indicates that no records match the selected criteria.
The system does not display misleading KPI values.
Users are given guidance to adjust filters.
Priority

Should Have

13. Functional Requirement Traceability
Functional Requirement	PRD Feature	Business Requirement
FR-001	KPI Summary	BR-01
FR-002	KPI Summary	BR-01
FR-003	Allowed Amount	BR-04
FR-004	Spend Analytics	BR-03
FR-005	Claims Analytics	BR-02
FR-006	Claim Status	BR-02
FR-007	Denial Rate	BR-02
FR-008	Claim vs Allowed	BR-04
FR-009	Average Claim	BR-02
FR-010	Average Allowed	BR-04
FR-011	Provider Ranking	BR-05
FR-012	Provider Comparison	BR-05
FR-013	Provider Drilldown	BR-05
FR-014	Claims per Member	BR-06
FR-015	Spend per Member	BR-03
FR-016	Service Utilization	BR-06
FR-017–022	Filtering	BR-08
FR-023	Drilldown	BR-09
FR-024	Claim Detail	BR-09
FR-025–027	Data Quality	BR-12
FR-028	KPI Validation	BR-10 / BR-11
FR-029–030	UX / Error Handling	BR-08
14. Functional Definition of Done

A functional requirement is considered complete when:

Requirement behavior is implemented.
Business rules are applied.
Acceptance criteria pass.
Relevant SQL validation is complete.
Dashboard behavior is tested.
Filter behavior is validated.
Edge cases are addressed.
UAT criteria are satisfied.
Requirement traceability is maintained.
