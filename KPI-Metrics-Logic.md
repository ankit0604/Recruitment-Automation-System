# KPI Metrics & Logic
## Recruitment Process Automation — emergiTEL Inc.

---

**Document Version:** 1.0  
**Prepared By:** Ankit Prakash Srivastava — Lead Business Analyst  
**In Collaboration With:** Rehan Qazi — Data Analyst  
**Reviewed By:** Akbar Khan — Managing Director  
**Date:** 2024  

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Dashboard Architecture](#2-dashboard-architecture)
3. [Data Sources & Database Schema Reference](#3-data-sources--database-schema-reference)
4. [KPI Definitions — Recruiter Performance](#4-kpi-definitions--recruiter-performance)
5. [KPI Definitions — Pipeline Health](#5-kpi-definitions--pipeline-health)
6. [KPI Definitions — Finance & Payroll](#6-kpi-definitions--finance--payroll)
7. [KPI Definitions — Job Board Distribution](#7-kpi-definitions--job-board-distribution)
8. [Looker Studio Dashboard Specifications](#8-looker-studio-dashboard-specifications)
9. [Calculation Logic Reference](#9-calculation-logic-reference)
10. [Reporting Refresh & Governance](#10-reporting-refresh--governance)

---

## 1. Purpose

This document defines all KPI metrics tracked in the emergiTEL Recruitment Automation system, including their business definitions, calculation logic, data sources, and Looker Studio dashboard specifications.

It serves as:
- The requirements specification for Rehan Qazi (Data Analyst) to build the PostgreSQL views and Looker Studio data connectors
- The reference document for NexGen Solutions to ensure all data required for KPI calculation is captured by the ATS
- The governance document that defines how metrics are measured, refreshed, and interpreted

---

## 2. Dashboard Architecture

```
PostgreSQL Cloud Database
        │
        │  (Looker Studio Data Connector)
        ▼
Google Looker Studio
        │
        ├── Dashboard 1: Recruiter Performance Overview
        ├── Dashboard 2: Candidate Pipeline Health
        ├── Dashboard 3: Finance & Payroll Tracker
        └── Dashboard 4: Job Board Distribution Performance
```

Data flows from the ATS system into the PostgreSQL database in real time as records are created and updated. Looker Studio refreshes at a configurable interval (recommended: every 4 hours for operational dashboards).

---

## 3. Data Sources & Database Schema Reference

The following tables in the PostgreSQL database are the primary sources for KPI computation:

| Table | Key Fields Used in KPIs |
|---|---|
| `candidates` | CandidateID, AssignedRecruiter, Stage, CreatedDate, PlacedDate, Source |
| `job_requisitions` | JobID, Status, CreatedDate, FilledDate, Department, HiringManager |
| `interviews` | InterviewID, CandidateID, JobID, DateTime, Outcome, RecruiterID |
| `offers` | OfferID, CandidateID, JobID, OfferDate, Status, StartDate |
| `placements` | PlacementID, CandidateID, ClientID, JobID, BillRate, PayRate, StartDate, EndDate, RecruiterID |
| `timesheets` | TimesheetID, PlacementID, PeriodStart, TotalHours, Status, ApprovedDate |
| `payroll_records` | PayrollID, PlacementID, TimesheetID, GrossPay, Status, GeneratedDate |
| `job_board_logs` | LogID, JobID, Board, PostedDate, Status, ErrorMessage |
| `users` | UserID, Name, Department, Role |

---

## 4. KPI Definitions — Recruiter Performance

### KPI-RP-01: Total Submissions

| Attribute | Detail |
|---|---|
| **Definition** | Total number of candidates submitted to clients by a recruiter within the selected period |
| **Formula** | `COUNT(CandidateID) WHERE Stage >= 'Submitted to Client' AND AssignedRecruiter = [RecruiterID] AND SubmissionDate BETWEEN [StartDate] AND [EndDate]` |
| **Data Source** | `candidates` table |
| **Granularity** | Per recruiter, per week / month / quarter |
| **Target** | Defined by management per period |
| **Dashboard Visual** | Bar chart — Recruiters on X-axis, Submission Count on Y-axis |

---

### KPI-RP-02: Total Placements

| Attribute | Detail |
|---|---|
| **Definition** | Total number of candidates successfully placed (offer accepted + start date confirmed) by a recruiter within the selected period |
| **Formula** | `COUNT(PlacementID) WHERE RecruiterID = [RecruiterID] AND StartDate BETWEEN [StartDate] AND [EndDate]` |
| **Data Source** | `placements` table |
| **Granularity** | Per recruiter, per month / quarter |
| **Target** | Defined by management |
| **Dashboard Visual** | Stacked bar chart — Recruiter breakdown by placement count |

---

### KPI-RP-03: Submission-to-Placement Conversion Rate

| Attribute | Detail |
|---|---|
| **Definition** | Percentage of submitted candidates that resulted in a placement, per recruiter |
| **Formula** | `(Total Placements / Total Submissions) × 100` |
| **Data Source** | `candidates`, `placements` |
| **Granularity** | Per recruiter, per quarter |
| **Interpretation** | Higher is better; a rate below the team average warrants review |
| **Dashboard Visual** | Gauge chart or percentage card per recruiter |

---

### KPI-RP-04: Time-to-Fill

| Attribute | Detail |
|---|---|
| **Definition** | Average number of calendar days from job requisition creation to placement confirmed, per recruiter |
| **Formula** | `AVG(PlacementStartDate - JobRequisitionCreatedDate) WHERE RecruiterID = [RecruiterID]` |
| **Data Source** | `job_requisitions`, `placements` |
| **Granularity** | Per recruiter, per quarter |
| **Interpretation** | Lower is better; benchmarked against team and industry average |
| **Dashboard Visual** | Line chart — trend over time per recruiter |

---

### KPI-RP-05: Offer Acceptance Rate

| Attribute | Detail |
|---|---|
| **Definition** | Percentage of extended offers that were accepted, per recruiter |
| **Formula** | `(COUNT(offers WHERE Status = 'Accepted') / COUNT(offers WHERE Status IN ('Accepted','Declined'))) × 100` |
| **Filter** | `RecruiterID = [RecruiterID]` |
| **Data Source** | `offers` |
| **Granularity** | Per recruiter, per quarter |
| **Dashboard Visual** | Percentage card with comparison to team average |

---

### KPI-RP-06: Interview-to-Offer Conversion Rate

| Attribute | Detail |
|---|---|
| **Definition** | Percentage of interviews that resulted in an offer being extended |
| **Formula** | `(COUNT(offers) / COUNT(interviews WHERE Outcome = 'Passed')) × 100` |
| **Filter** | Linked to same RecruiterID and same period |
| **Data Source** | `interviews`, `offers` |
| **Dashboard Visual** | Funnel chart showing Interview → Offer → Placement conversion |

---

### KPI-RP-07: Revenue Generated per Recruiter (Gross Billing)

| Attribute | Detail |
|---|---|
| **Definition** | Total gross billing amount attributed to a recruiter's placements within the period |
| **Formula** | `SUM(TotalHours × BillRate) WHERE RecruiterID = [RecruiterID] AND TimesheetStatus = 'Approved'` |
| **Data Source** | `placements`, `timesheets` |
| **Granularity** | Per recruiter, per month / quarter |
| **Dashboard Visual** | Bar chart sorted by revenue descending |

---

## 5. KPI Definitions — Pipeline Health

### KPI-PH-01: Active Candidates by Stage

| Attribute | Detail |
|---|---|
| **Definition** | Count of candidates currently in each stage of the pipeline |
| **Formula** | `COUNT(CandidateID) GROUP BY Stage WHERE Stage != 'Rejected' AND Stage != 'Placed'` |
| **Data Source** | `candidates` |
| **Dashboard Visual** | Horizontal funnel chart showing stage volumes |

---

### KPI-PH-02: Pipeline Drop-Off Rate by Stage

| Attribute | Detail |
|---|---|
| **Definition** | Percentage of candidates who exit the pipeline at each stage without advancing |
| **Formula** | `(COUNT(candidates exiting at Stage X) / COUNT(candidates entering Stage X)) × 100` |
| **Data Source** | `candidates` (using stage history from audit log) |
| **Interpretation** | High drop-off at a specific stage indicates a process or quality issue at that stage |
| **Dashboard Visual** | Funnel chart with percentage drop labels between stages |

---

### KPI-PH-03: Open Requisitions Age

| Attribute | Detail |
|---|---|
| **Definition** | Number of calendar days each open job requisition has been active without a placement |
| **Formula** | `CURRENT_DATE - CreatedDate WHERE Status = 'Open'` |
| **Data Source** | `job_requisitions` |
| **Alert Threshold** | Flag requisitions open for more than 30 days |
| **Dashboard Visual** | Table with job title, days open, hiring manager, recruiter assigned — sorted by age descending |

---

### KPI-PH-04: Candidate Source Effectiveness

| Attribute | Detail |
|---|---|
| **Definition** | Breakdown of placements by candidate source (Indeed, Glassdoor, Wellfound, ZipRecruiter, PostJobFree, Hubstaff Talent, referral, direct) |
| **Formula** | `COUNT(PlacementID) GROUP BY Source` |
| **Data Source** | `candidates`, `placements` |
| **Dashboard Visual** | Pie chart or donut chart |
| **Value** | Identifies highest-ROI sourcing channels to inform job board investment decisions |

---

## 6. KPI Definitions — Finance & Payroll

### KPI-FP-01: Active Placements

| Attribute | Detail |
|---|---|
| **Definition** | Total number of placements currently active (start date passed, no end date or end date in future) |
| **Formula** | `COUNT(PlacementID) WHERE StartDate <= CURRENT_DATE AND (EndDate IS NULL OR EndDate >= CURRENT_DATE)` |
| **Data Source** | `placements` |
| **Dashboard Visual** | Single number card |

---

### KPI-FP-02: Timesheets Pending Approval

| Attribute | Detail |
|---|---|
| **Definition** | Count of submitted timesheets awaiting approver action |
| **Formula** | `COUNT(TimesheetID) WHERE Status = 'Submitted'` |
| **Data Source** | `timesheets` |
| **Alert Threshold** | Any timesheet pending for more than 3 business days |
| **Dashboard Visual** | Number card with alert indicator if above threshold |

---

### KPI-FP-03: Payroll Records Generated (Current Period)

| Attribute | Detail |
|---|---|
| **Definition** | Count of payroll records auto-generated in the current bi-weekly period |
| **Formula** | `COUNT(PayrollID) WHERE GeneratedDate BETWEEN [PeriodStart] AND [PeriodEnd]` |
| **Data Source** | `payroll_records` |
| **Dashboard Visual** | Number card with period label |

---

### KPI-FP-04: Total Gross Payroll (Current Period)

| Attribute | Detail |
|---|---|
| **Definition** | Sum of gross pay across all payroll records in the current period |
| **Formula** | `SUM(GrossPay) WHERE Status != 'Cancelled' AND GeneratedDate BETWEEN [PeriodStart] AND [PeriodEnd]` |
| **Data Source** | `payroll_records` |
| **Dashboard Visual** | Currency card |

---

### KPI-FP-05: Payroll Anomaly Flag

| Attribute | Detail |
|---|---|
| **Definition** | Count of approved timesheets where a payroll record was not generated within 2 hours |
| **Formula** | `COUNT(TimesheetID) WHERE Status = 'Approved' AND NOT EXISTS (SELECT 1 FROM payroll_records WHERE TimesheetID = TimesheetID) AND ApprovedDate < CURRENT_TIMESTAMP - INTERVAL '2 hours'` |
| **Data Source** | `timesheets`, `payroll_records` |
| **Alert** | Any value above 0 triggers an immediate system alert to Admin and Finance |
| **Dashboard Visual** | Alert card — green (0 anomalies) / red (1+ anomalies) |

---

## 7. KPI Definitions — Job Board Distribution

### KPI-JB-01: Jobs Posted per Board (Period)

| Attribute | Detail |
|---|---|
| **Definition** | Count of successful job postings per board within the selected period |
| **Formula** | `COUNT(LogID) WHERE Status = 'Success' GROUP BY Board AND PostedDate BETWEEN [StartDate] AND [EndDate]` |
| **Data Source** | `job_board_logs` |
| **Dashboard Visual** | Bar chart — Board on X-axis, post count on Y-axis |

---

### KPI-JB-02: Posting Failure Rate per Board

| Attribute | Detail |
|---|---|
| **Definition** | Percentage of posting attempts that failed per board |
| **Formula** | `(COUNT(LogID WHERE Status = 'Failed') / COUNT(LogID)) × 100 GROUP BY Board` |
| **Data Source** | `job_board_logs` |
| **Alert Threshold** | Failure rate above 10% for any board within a 7-day rolling window |
| **Dashboard Visual** | Table with board name, total attempts, success count, failure count, failure % |

---

### KPI-JB-03: Source-to-Placement Attribution by Board

| Attribute | Detail |
|---|---|
| **Definition** | Of all placements made, how many originated from each job board as the candidate's source |
| **Formula** | `COUNT(PlacementID) JOIN candidates ON Source = Board GROUP BY Source` |
| **Data Source** | `placements`, `candidates` |
| **Value** | Informs which boards deliver placed candidates — not just applicant volume |
| **Dashboard Visual** | Horizontal bar chart sorted by placements descending |

---

## 8. Looker Studio Dashboard Specifications

### Dashboard 1: Recruiter Performance Overview

**Audience:** Managing Director, Company Owner  
**Refresh:** Every 4 hours

| Widget | KPI | Visual Type | Filter Options |
|---|---|---|---|
| Submissions This Month | KPI-RP-01 | Scorecard | Recruiter, Period |
| Placements This Month | KPI-RP-02 | Scorecard | Recruiter, Period |
| Submissions by Recruiter | KPI-RP-01 | Bar Chart | Period |
| Placement Conversion Rate | KPI-RP-03 | Gauge Chart | Recruiter, Period |
| Time-to-Fill Trend | KPI-RP-04 | Line Chart | Recruiter, Period |
| Offer Acceptance Rate | KPI-RP-05 | Percentage Card | Recruiter, Period |
| Revenue per Recruiter | KPI-RP-07 | Bar Chart (sorted) | Period |

---

### Dashboard 2: Candidate Pipeline Health

**Audience:** Managers, Recruiters  
**Refresh:** Every 4 hours

| Widget | KPI | Visual Type | Filter Options |
|---|---|---|---|
| Pipeline Funnel | KPI-PH-01 | Funnel Chart | Recruiter, Job, Period |
| Stage Drop-Off Rates | KPI-PH-02 | Annotated Funnel | Period |
| Aged Open Requisitions | KPI-PH-03 | Table (sorted) | Recruiter, Department |
| Source Effectiveness | KPI-PH-04 | Donut Chart | Period |

---

### Dashboard 3: Finance & Payroll Tracker

**Audience:** Finance User, Managing Director  
**Refresh:** Every 1 hour

| Widget | KPI | Visual Type | Alert |
|---|---|---|---|
| Active Placements | KPI-FP-01 | Scorecard | None |
| Timesheets Pending | KPI-FP-02 | Alert Card | > 3 days = Red |
| Payroll Records (Current Period) | KPI-FP-03 | Scorecard | None |
| Gross Payroll (Current Period) | KPI-FP-04 | Currency Card | None |
| Payroll Anomaly Flag | KPI-FP-05 | Alert Card | Any value > 0 = Red |

---

### Dashboard 4: Job Board Distribution Performance

**Audience:** Recruiters, Managers  
**Refresh:** Daily

| Widget | KPI | Visual Type | Filter Options |
|---|---|---|---|
| Posts per Board | KPI-JB-01 | Bar Chart | Period |
| Failure Rate per Board | KPI-JB-02 | Table with % column | Period, Board |
| Placements by Source Board | KPI-JB-03 | Horizontal Bar Chart | Period |

---

## 9. Calculation Logic Reference

The following SQL snippets serve as the implementation reference for Rehan Qazi when building the Looker Studio data views in PostgreSQL.

```sql
-- KPI-RP-01: Total Submissions per Recruiter
SELECT
    u.Name AS Recruiter,
    COUNT(c.CandidateID) AS TotalSubmissions
FROM candidates c
JOIN users u ON c.AssignedRecruiter = u.UserID
WHERE c.Stage >= 'Submitted to Client'
  AND c.SubmissionDate BETWEEN :start_date AND :end_date
GROUP BY u.Name;

-- KPI-RP-03: Submission-to-Placement Conversion Rate
SELECT
    u.Name AS Recruiter,
    COUNT(p.PlacementID) AS Placements,
    COUNT(c.CandidateID) AS Submissions,
    ROUND(COUNT(p.PlacementID)::NUMERIC / NULLIF(COUNT(c.CandidateID), 0) * 100, 2) AS ConversionRate
FROM candidates c
JOIN users u ON c.AssignedRecruiter = u.UserID
LEFT JOIN placements p ON c.CandidateID = p.CandidateID
WHERE c.SubmissionDate BETWEEN :start_date AND :end_date
GROUP BY u.Name;

-- KPI-RP-04: Average Time-to-Fill per Recruiter
SELECT
    u.Name AS Recruiter,
    ROUND(AVG(p.StartDate - j.CreatedDate), 1) AS AvgDaysToFill
FROM placements p
JOIN job_requisitions j ON p.JobID = j.JobID
JOIN users u ON p.RecruiterID = u.UserID
WHERE p.StartDate BETWEEN :start_date AND :end_date
GROUP BY u.Name;

-- KPI-FP-05: Payroll Anomaly Detection
SELECT
    t.TimesheetID,
    t.ApprovedDate,
    t.PlacementID
FROM timesheets t
WHERE t.Status = 'Approved'
  AND NOT EXISTS (
      SELECT 1 FROM payroll_records pr WHERE pr.TimesheetID = t.TimesheetID
  )
  AND t.ApprovedDate < NOW() - INTERVAL '2 hours';

-- KPI-JB-02: Job Board Failure Rate
SELECT
    Board,
    COUNT(*) AS TotalAttempts,
    SUM(CASE WHEN Status = 'Success' THEN 1 ELSE 0 END) AS Successes,
    SUM(CASE WHEN Status = 'Failed' THEN 1 ELSE 0 END) AS Failures,
    ROUND(SUM(CASE WHEN Status = 'Failed' THEN 1 ELSE 0 END)::NUMERIC / COUNT(*) * 100, 2) AS FailureRate
FROM job_board_logs
WHERE PostedDate BETWEEN :start_date AND :end_date
GROUP BY Board;
```

---

## 10. Reporting Refresh & Governance

| Dashboard | Refresh Frequency | Owner | Access Level |
|---|---|---|---|
| Recruiter Performance | Every 4 hours | Rehan Qazi | Manager, Admin, Owner |
| Pipeline Health | Every 4 hours | Rehan Qazi | Manager, Recruiter, Admin |
| Finance & Payroll | Every 1 hour | Rehan Qazi | Finance User, Manager, Admin |
| Job Board Distribution | Daily | Rehan Qazi | Recruiter, Manager, Admin |

**Data Governance Rules:**
- All KPI calculations use only approved, timestamped records from the PostgreSQL database
- KPI definitions in this document are the single source of truth — any changes require written approval from the Lead BA and Managing Director
- Historical KPI data is retained indefinitely for year-over-year comparison
- Looker Studio access is controlled via Google account permissions aligned with ATS role assignments

---

*KPI logic reviewed and approved by Rehan Qazi (Data Analyst) and Akbar Khan (Managing Director) prior to dashboard development.*
