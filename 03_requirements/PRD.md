# Product Requirements Document (PRD)

## US Healthcare Insurance Analytics Dashboard

**Version:** 1.0
**Status:** Draft
**Product Type:** Healthcare Insurance Analytics & Decision Support
**Data:** Synthetic / Portfolio-Safe
**Primary Users:** Business / Operations, Provider Network, Healthcare Data Analysts

---

# 1. Product Overview

The **US Healthcare Insurance Analytics Dashboard** is an analytics and decision-support product designed to provide a centralized view of healthcare insurance claims, spending, utilization, and provider performance.

The product transforms synthetic claims-related data into standardized KPIs, interactive analytics, and drilldown capabilities that enable stakeholders to understand healthcare cost and utilization patterns.

The product is designed around the following analytical flow:

```text
Data
 ↓
KPI
 ↓
Trend
 ↓
Breakdown
 ↓
Drilldown
 ↓
Insight
 ↓
Business Decision
```

---

# 2. Product Vision

> **Enable healthcare insurance stakeholders to understand claims and healthcare cost performance through reliable, standardized, and accessible analytics.**

---

# 3. Product Mission

The product will provide a trusted analytics experience that allows stakeholders to:

* Monitor healthcare insurance performance
* Understand claims trends
* Analyze healthcare spending
* Compare Claim Amount and Allowed Amount
* Evaluate provider performance
* Analyze utilization
* Investigate changes through drilldowns
* Make data-driven decisions

---

# 4. Product Goals

## Goal 1 — Centralize Analytics

Provide a single dashboard experience for core healthcare insurance analytics.

## Goal 2 — Standardize KPIs

Create consistent definitions and calculation logic for core healthcare insurance metrics.

## Goal 3 — Enable Self-Service Analysis

Allow business users to explore claims and spending without requiring a separate report for every question.

## Goal 4 — Improve Provider Visibility

Provide provider-level analytics for cost, claims, and utilization.

## Goal 5 — Improve Data Confidence

Ensure dashboard KPIs are supported by documented calculations and validation checks.

---

# 5. Target Users

### Primary Users

**Business / Operations Manager**

Focus:

* Overall performance
* Claims
* Spend
* Utilization
* Trends

**Provider Network Manager**

Focus:

* Provider performance
* Provider spend
* Utilization
* Provider comparison

**Healthcare Data Analyst**

Focus:

* Detailed investigation
* KPI validation
* Claims analysis
* Data quality

---

# 6. Product Value Proposition

The dashboard provides:

### For Business Leaders

**"A single view of healthcare claims and cost performance."**

### For Provider Network Teams

**"A consistent way to compare provider cost and utilization patterns."**

### For Analysts

**"A validated analytical layer for investigating claims and KPI changes."**

---

# 7. Core Product Capabilities

The MVP will contain the following capabilities:

### 7.1 Executive Dashboard

Provides:

* Total Claims
* Total Spend
* Total Allowed Amount
* Total Claim Amount
* Unique Members
* Claims per Member
* Spend per Member
* Key trends

### 7.2 Claims Analytics

Provides:

* Claims volume
* Claims trends
* Claim status
* Claim Amount
* Allowed Amount
* Average Claim Amount
* Average Allowed Amount

### 7.3 Spend Analytics

Provides:

* Total Spend
* Spend trends
* Spend by service
* Spend by provider
* Spend by geography

### 7.4 Provider Analytics

Provides:

* Provider ranking
* Provider claims
* Provider spend
* Average Allowed Amount
* Utilization
* Provider comparison

### 7.5 Utilization Analytics

Provides:

* Claims per Member
* Services per Member
* Service frequency
* Utilization trends

### 7.6 Interactive Analysis

Provides:

* Date filters
* Provider filters
* Service filters
* Geography filters
* Claim status filters
* Network filters
* Drilldowns
# 8. MVP Feature Requirements

## 8.1 Executive Overview Dashboard

The Executive Overview will provide a high-level summary of healthcare insurance performance.

### Core KPIs

- Total Claims
- Total Claim Amount
- Total Allowed Amount
- Total Spend
- Unique Members
- Claims per Member
- Spend per Member
- Denial Rate

### Visualizations

- Monthly claims trend
- Monthly spend trend
- Spend by service category
- Spend by provider
- Spend by geography
- Claim status distribution

### User Outcome

Users should be able to answer:

> "What is happening with claims, healthcare spending, and utilization?"

within a few minutes of opening the dashboard.

---

## 8.2 Claims Analytics

The Claims Analytics page will provide detailed analysis of claims activity.

### Capabilities

- Claims trend analysis
- Claim status analysis
- Claim Amount analysis
- Allowed Amount analysis
- Average claim cost
- Claims by service
- Claims by provider
- Claims by geography

### User Outcome

Users should be able to identify:

- Changes in claim volume
- Changes in claim status
- High-cost services
- High-volume providers
- Major claim drivers

---

## 8.3 Spend Analytics

The Spend Analytics page will focus on healthcare cost patterns.

### Capabilities

- Total Spend
- Spend trend
- Spend by service category
- Spend by provider
- Spend by geography
- Spend per member
- Average spend per claim

### User Outcome

Users should be able to determine:

- Where healthcare spend is concentrated
- Which services drive spend
- Which providers contribute most to spend
- Whether spend is increasing or decreasing

---

## 8.4 Provider Performance

The Provider Performance page will allow users to compare providers.

### Capabilities

- Provider ranking
- Provider claims volume
- Provider spend
- Allowed Amount
- Average Allowed Amount
- Average Claim Amount
- Utilization
- Provider specialty
- Network status

### Provider Comparison

Users should be able to select providers and compare:

```text
Claims
Spend
Allowed Amount
Average Cost
Utilization
Service Mix
# 17. Product Metrics

The product will measure success across adoption, usability, data quality, and analytical effectiveness.

## 17.1 Product Adoption Metrics

| Metric | Definition | Target |
|---|---|---:|
| Dashboard Adoption | Percentage of intended users accessing the dashboard | ≥ 80% |
| Monthly Active Users | Unique users accessing the dashboard in a month | Increasing trend |
| Repeat Usage | Percentage of users returning to the dashboard | ≥ 60% |
| Feature Usage | Usage of filters and drilldowns | ≥ 50% of active users |

---

## 17.2 Analytics Effectiveness Metrics

| Metric | Definition | Target |
|---|---|---:|
| KPI Reconciliation Rate | Percentage of KPIs matching validated SQL calculations | ≥ 99% |
| Data Quality Pass Rate | Percentage of records passing defined quality checks | ≥ 98% |
| Drilldown Success Rate | Percentage of investigations successfully reaching relevant detail | ≥ 90% |
| Reporting Turnaround | Time required to obtain standard analytics | < 5 minutes |

---

## 17.3 Business Outcome Metrics

The portfolio project will demonstrate how the dashboard could support:

- Reduced manual reporting effort
- Faster identification of cost drivers
- Improved KPI consistency
- Faster provider performance analysis
- Improved visibility into claims trends

These outcomes are illustrative portfolio targets rather than measured production results.

---

# 18. Feature-Level Acceptance Criteria

## 18.1 Executive Dashboard

### Acceptance Criteria

**AC-EXEC-01**

Given a user opens the Executive Dashboard,

When the dashboard loads,

Then the user should see the defined core KPIs.

**AC-EXEC-02**

Given a user selects a date range,

When the filter is applied,

Then applicable KPIs and visualizations should update consistently.

**AC-EXEC-03**

Given the dashboard displays Total Spend,

When the user validates the KPI against the approved SQL calculation,

Then the values should reconcile within the defined tolerance.

---

## 18.2 Claims Analytics

### Acceptance Criteria

**AC-CLAIM-01**

The user should be able to view total claims for the selected period.

**AC-CLAIM-02**

The user should be able to analyze claims by status.

**AC-CLAIM-03**

The user should be able to analyze Claim Amount and Allowed Amount separately.

**AC-CLAIM-04**

The user should be able to filter claims by relevant dimensions.

---

## 18.3 Spend Analytics

### Acceptance Criteria

**AC-SPEND-01**

The user should be able to view Total Spend for the selected period.

**AC-SPEND-02**

The user should be able to analyze spend by service category.

**AC-SPEND-03**

The user should be able to analyze spend by provider.

**AC-SPEND-04**

The user should be able to view spend trends over time.

---

## 18.4 Provider Analytics

### Acceptance Criteria

**AC-PROV-01**

The user should be able to rank providers by selected metrics.

**AC-PROV-02**

The user should be able to view provider claims volume.

**AC-PROV-03**

The user should be able to view provider spend and Allowed Amount.

**AC-PROV-04**

The user should be able to compare selected providers.

---

## 18.5 Drilldown

### Acceptance Criteria

**AC-DRILL-01**

The user should be able to move from an aggregated KPI to a relevant breakdown.

**AC-DRILL-02**

The user should be able to move from a category to provider-level analysis.

**AC-DRILL-03**

Where claim-level data is available, the user should be able to access claim-level detail.

---

# 19. Data Requirements

The product requires the following core datasets:

| Dataset | Purpose |
|---|---|
| Members | Member population and demographic analysis |
| Providers | Provider attributes and performance |
| Claims | Core healthcare claim transactions |
| Claim Lines | Service-level claim analysis |
| Services | Service and procedure classification |
| Geography | Geographic analysis |
| Date | Time-based analysis |

---

# 20. Key Data Fields

## Claims

- Claim ID
- Member ID
- Provider ID
- Service ID
- Service Date
- Claim Amount
- Allowed Amount
- Paid Amount
- Claim Status
- Network Status

## Members

- Member ID
- Member demographic segment
- Geographic identifier
- Plan identifier

## Providers

- Provider ID
- Provider Name
- Provider Specialty
- Provider Type
- Geographic identifier
- Network Status

## Services

- Service ID
- Service Category
- Service Description
- Procedure Code

---

# 21. Data Grain

The analytical model must clearly distinguish between different levels of data.

### Member Grain

One record represents one member.

### Provider Grain

One record represents one provider.

### Claim Grain

One record represents one claim.

### Claim Line Grain

One record represents one service line within a claim.

### Date Grain

One record represents one calendar date.

This distinction is important to prevent double-counting when calculating claims, Allowed Amount, Paid Amount, and Spend.

---

# 22. KPI Governance

Each KPI must have:

- Business definition
- Calculation formula
- Data source
- Data fields
- Aggregation logic
- Filter behavior
- Owner
- Validation method

Example:

### Total Claims

**Definition:** Number of unique claims in the selected analytical period.

**Formula:**

```text
COUNT(DISTINCT Claim_ID)
