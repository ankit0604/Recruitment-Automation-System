# Business Requirements Document (BRD)
## Recruitment Process Automation — emergiTEL Inc.

---

**Document Version:** 1.0  
**Status:** Approved  
**Prepared By:** Ankit Prakash Srivastava — Lead Business Analyst  
**Reviewed By:** Sonika Salhotra (Recruitment SME), Akbar Khan (Managing Director)  
**Approved By:** Aneela Zaib (Company Owner)  
**Date:** 2025  

---

## Table of Contents

1. [Document Purpose](#1-document-purpose)
2. [Business Context](#2-business-context)
3. [Business Objectives](#3-business-objectives)
4. [Scope](#4-scope)
5. [Stakeholder Register](#5-stakeholder-register)
6. [Current State (AS-IS)](#6-current-state-as-is)
7. [Future State (TO-BE)](#7-future-state-to-be)
8. [Functional Requirements](#8-functional-requirements)
9. [Non-Functional Requirements](#9-non-functional-requirements)
10. [Integration Requirements](#10-integration-requirements)
11. [Data Requirements](#11-data-requirements)
12. [Assumptions and Constraints](#12-assumptions-and-constraints)
13. [Dependencies](#13-dependencies)
14. [Risks](#14-risks)
15. [Acceptance Criteria](#15-acceptance-criteria)
16. [Sign-Off](#16-sign-off)

---

## 1. Document Purpose

This Business Requirements Document (BRD) captures the business needs, objectives, stakeholder expectations, and functional requirements for the development of emergiTEL Inc.'s proprietary Recruitment Process Automation system.

The document serves as the contractual reference between emergiTEL Inc. and the external development vendor, NexGen Solutions (Hyderabad), and as the baseline for UAT validation and project sign-off.

---

## 2. Business Context

emergiTEL Inc. is a Canadian-based staffing and technology solutions company. The company manages a continuous high-volume recruitment pipeline across multiple industry verticals. Until this initiative, the core technology supporting recruitment was BreezyHR — a subscription-based, third-party ATS.

As the organization scaled, BreezyHR's limitations became a significant operational bottleneck. The platform offered only basic applicant tracking and calendar integration, leaving the majority of critical workflows — candidate tracking, interview progression, placement confirmation, timesheet management, and payroll processing — to be handled manually via disconnected Excel spreadsheets and email chains.

emergiTEL's leadership determined that continued investment in BreezyHR presented diminishing return and increasing risk. The decision was taken to develop a fully proprietary system that would consolidate all recruitment and finance workflows, secure candidate data within the company's own infrastructure, and automate the manual processes that were eroding team productivity and data accuracy.

---

## 3. Business Objectives

| ID | Objective |
|---|---|
| BO-01 | Replace BreezyHR with a fully owned, internally operated ATS |
| BO-02 | Automate the end-to-end candidate lifecycle from submission to placement |
| BO-03 | Automate payroll processing triggers upon timesheet approval to eliminate manual finance sheet updates |
| BO-04 | Enable single-click job distribution to seven job boards via public APIs |
| BO-05 | Integrate email and calendar communication via Microsoft Outlook (MS Graph API) |
| BO-06 | Provide real-time recruiter performance visibility through a structured database and Looker Studio dashboards |
| BO-07 | Eliminate third-party data dependency and secure all candidate and client data within emergiTEL's infrastructure |
| BO-08 | Reduce manual data entry by a minimum of 70% across recruitment and finance operations |

---

## 4. Scope

### 4.1 In Scope

- Full candidate lifecycle management (sourcing, submission, interview, offer, placement)
- Job requisition creation and management
- Multi-board job posting automation (Indeed Canada, Glassdoor, Wellfound, ZipRecruiter, PostJobFree, Hubstaff Talent, emergiTEL website)
- Microsoft Outlook integration for email notifications and interview scheduling
- Timesheet submission and approval workflow
- Automated payroll trigger on timesheet approval
- Finance module (placement records, billing tracking)
- Recruiter performance KPI dashboard (Looker Studio)
- Cloud-hosted PostgreSQL database design and setup
- User authentication and role-based access control

### 4.2 Out of Scope

- Mobile application (post-MVP)
- Client portal for external job order submission (post-MVP)
- Integration with accounting software (e.g., QuickBooks) — future phase
- Automated reference checking — future phase
- AI-based candidate matching or resume scoring — future phase

---

## 5. Stakeholder Register

| ID | Name | Role | Organization | Influence | Interest | Engagement Strategy |
|---|---|---|---|---|---|---|
| SH-01 | Aneela Zaib | Company Owner | emergiTEL Inc. | High | High | Monthly executive updates, formal approvals |
| SH-02 | Akbar Khan | Managing Director | emergiTEL Inc. | High | High | Bi-weekly steering meetings |
| SH-03 | Sonika Salhotra | Recruitment SME | emergiTEL Inc. | Medium | High | Weekly requirement walkthroughs, UAT lead |
| SH-04 | Rehan Qazi | Data Analyst | emergiTEL Inc. | Medium | High | Database schema reviews, dashboard alignment |
| SH-05 | Ankit Prakash Srivastava | Lead Business Analyst | emergiTEL Inc. | High | High | Day-to-day project ownership |
| SH-06 | NexGen Solutions | Development Vendor | Hyderabad, India | High | Medium | Sprint reviews, change request management |

---

## 6. Current State (AS-IS)

### 6.1 Candidate Tracking

Recruiters maintain individual Excel spreadsheets to track candidates across stages. There is no standardized format, no shared real-time view, and no audit trail of changes. When a candidate's status changes, the recruiter manually updates their own sheet, and a separate summary may or may not be communicated to the manager.

### 6.2 Interview Management

Interview scheduling is done manually via Outlook calendar. There is no link between a scheduled interview and a candidate record in BreezyHR. Interview outcomes are communicated by email and then manually noted in spreadsheets.

### 6.3 Offer and Placement Tracking

Offer letters are sent via Outlook and tracked in a separate sheet. Once an offer is accepted and a candidate is placed, the placement record is manually entered into another Excel file used by the finance team. This introduces a communication dependency between recruitment and finance teams that frequently causes delays.

### 6.4 Timesheet and Payroll

Placed candidates submit timesheets. Upon manager approval, the finance team is notified by email. A team member then manually updates a payroll Excel sheet to include the approved hours and initiate bi-weekly payroll processing. This step has no automation and is entirely dependent on manual communication.

### 6.5 Job Posting

When a new job is created, a recruiter manually logs into each job board individually and copies the job description across all platforms. With seven active boards, this is a repetitive, time-consuming process. Job descriptions can become inconsistent across boards due to manual copy-paste errors.

### 6.6 Reporting

There is no real-time reporting. Ad-hoc Excel summaries are prepared manually when management requests a status update. These reports do not reflect current data and require significant manual compilation time.

---

## 7. Future State (TO-BE)

### 7.1 Candidate Tracking

All candidate records are stored in the central ATS. Recruiters update candidate status within the system. Status changes are timestamped and attributed to the updating recruiter. Managers have live visibility into all pipelines.

### 7.2 Interview Management

Interview scheduling is triggered from within the candidate record. Integration with Outlook via MS Graph API sends calendar invites to candidates and interviewers automatically. Interview outcomes are recorded in the system and update the candidate's stage automatically.

### 7.3 Offer and Placement Tracking

Offer details are recorded within the system. Upon offer acceptance, the placement record is automatically created and made visible to the finance team — no manual communication required.

### 7.4 Timesheet and Payroll

Placed candidates submit timesheets through the system. Upon approver sign-off, the system automatically updates the finance module and generates the payroll record for the bi-weekly run. No manual intervention is required.

### 7.5 Job Posting

A recruiter creates a job requisition in the system and selects target job boards. A single submission posts the job simultaneously to all selected boards via their respective public APIs. The emergiTEL website listing is also updated automatically.

### 7.6 Reporting

Looker Studio dashboards connect directly to the PostgreSQL database. Recruiter KPIs are updated in real time. Management can view performance metrics at any time without requiring manual report compilation.

---

## 8. Functional Requirements

### 8.1 User Management

| ID | Requirement |
|---|---|
| FR-UM-01 | The system shall support role-based access control with the following roles: Admin, Recruiter, Manager, Finance, Viewer |
| FR-UM-02 | Each user shall have a unique login with secure authentication |
| FR-UM-03 | Admin users shall be able to create, deactivate, and assign roles to users |
| FR-UM-04 | All user actions shall be logged with a timestamp and user ID |

### 8.2 Job Requisition Management

| ID | Requirement |
|---|---|
| FR-JR-01 | Users with appropriate roles shall be able to create a new job requisition |
| FR-JR-02 | Each requisition shall capture: job title, department, location, job type, description, required skills, compensation range, hiring manager, target job boards |
| FR-JR-03 | Requisitions shall support statuses: Draft, Open, On Hold, Closed, Filled |
| FR-JR-04 | The system shall allow editing of requisitions before the Open status |
| FR-JR-05 | Closed and Filled requisitions shall be archived and searchable |

### 8.3 Multi-Board Job Distribution

| ID | Requirement |
|---|---|
| FR-JD-01 | Upon publishing a job requisition, the system shall post the job to all selected job boards via their respective public APIs in a single action |
| FR-JD-02 | Supported boards: Indeed Canada, Glassdoor, Wellfound, ZipRecruiter, PostJobFree, Hubstaff Talent, emergiTEL website |
| FR-JD-03 | The system shall display posting confirmation status per board (Success / Failed) |
| FR-JD-04 | Failed postings shall trigger a notification to the posting user with the error detail |
| FR-JD-05 | Job edits shall support re-publishing to selected boards |
| FR-JD-06 | Job closure in the ATS shall trigger removal from active boards where the API supports it |

### 8.4 Candidate Management

| ID | Requirement |
|---|---|
| FR-CM-01 | Recruiters shall be able to manually add candidates or import from job board applications |
| FR-CM-02 | Each candidate record shall capture: full name, contact information, current status, source, assigned recruiter, associated job, resume/CV, notes, and status history |
| FR-CM-03 | Candidate lifecycle stages: New, Submitted to Client, Interview Scheduled, Interview Completed, Offer Extended, Offer Accepted, Offer Declined, Placed, Rejected, On Hold |
| FR-CM-04 | Status transitions shall be timestamped and attributed to the acting user |
| FR-CM-05 | Recruiters shall be able to search and filter candidates by stage, job, recruiter, date range, and keyword |
| FR-CM-06 | Duplicate candidate detection shall be enabled based on email address |

### 8.5 Interview Management

| ID | Requirement |
|---|---|
| FR-IM-01 | Recruiters shall be able to schedule an interview directly from the candidate record |
| FR-IM-02 | The system shall send calendar invites to the candidate and interviewers via MS Graph API (Outlook integration) |
| FR-IM-03 | Interview records shall capture: interview type, date, time, interviewer(s), location or video link, outcome |
| FR-IM-04 | Interview outcomes shall automatically update the candidate's status in the pipeline |
| FR-IM-05 | Reminder notifications shall be sent to interviewers 24 hours before the scheduled interview |

### 8.6 Offer Management

| ID | Requirement |
|---|---|
| FR-OM-01 | Recruiters shall be able to create an offer record linked to a candidate and job requisition |
| FR-OM-02 | Offer records shall capture: offer date, start date, compensation, offer type (contract/permanent), status |
| FR-OM-03 | Offer statuses: Draft, Sent, Accepted, Declined, Withdrawn |
| FR-OM-04 | Upon offer acceptance, the system shall automatically initiate the placement record creation |
| FR-OM-05 | Offer communications shall be sent via Outlook through MS Graph API |

### 8.7 Placement Management

| ID | Requirement |
|---|---|
| FR-PM-01 | Placement records shall be automatically created upon offer acceptance |
| FR-PM-02 | Each placement record shall capture: candidate, client, job, start date, end date (for contracts), bill rate, pay rate, recruiter |
| FR-PM-03 | Finance team users shall have immediate visibility into new placement records |
| FR-PM-04 | Placement records shall link to the timesheet and payroll modules |

### 8.8 Timesheet Management

| ID | Requirement |
|---|---|
| FR-TM-01 | Placed candidates shall be able to submit timesheets through the system |
| FR-TM-02 | Timesheets shall capture: week period, daily hours, overtime, total hours |
| FR-TM-03 | Timesheets shall support statuses: Submitted, Under Review, Approved, Rejected |
| FR-TM-04 | Approvers shall receive a notification when a timesheet is submitted |
| FR-TM-05 | Upon timesheet approval, the system shall automatically update the payroll module with the approved hours and trigger the bi-weekly payroll record |

### 8.9 Finance and Payroll Module

| ID | Requirement |
|---|---|
| FR-FP-01 | The system shall maintain a finance record per placement including billing cycles, approved timesheets, and payroll entries |
| FR-FP-02 | Bi-weekly payroll records shall be auto-generated upon timesheet approval without manual intervention |
| FR-FP-03 | Finance users shall be able to view, filter, and export payroll data |
| FR-FP-04 | The system shall flag any placement where an approved timesheet has not triggered a payroll entry within the expected cycle |

### 8.10 Email Integration (MS Graph API / Outlook)

| ID | Requirement |
|---|---|
| FR-EI-01 | The system shall integrate with Microsoft Outlook via Microsoft Graph API |
| FR-EI-02 | Automated emails shall be sent for: interview scheduling, offer communication, timesheet reminders, placement confirmation |
| FR-EI-03 | Email logs shall be stored and linked to the relevant candidate or placement record |
| FR-EI-04 | Recruiters shall be able to trigger manual email send from within the candidate record |

### 8.11 Reporting and Dashboard

| ID | Requirement |
|---|---|
| FR-RD-01 | All operational data shall be stored in a structured PostgreSQL database |
| FR-RD-02 | Google Looker Studio shall connect to the database to power real-time dashboards |
| FR-RD-03 | Dashboards shall display KPIs as defined in the KPI Logic document |
| FR-RD-04 | Dashboard access shall be role-restricted |

---

## 9. Non-Functional Requirements

| ID | Category | Requirement |
|---|---|---|
| NFR-01 | Performance | Pages and records shall load within 3 seconds under normal usage conditions |
| NFR-02 | Scalability | The system shall support up to 50 concurrent users without degradation |
| NFR-03 | Availability | The system shall maintain 99.5% uptime during business hours (8 AM – 8 PM ET) |
| NFR-04 | Security | All data shall be encrypted in transit (TLS 1.2+) and at rest |
| NFR-05 | Security | Role-based access control shall prevent users from accessing modules outside their designated role |
| NFR-06 | Data Integrity | All status changes shall be logged with timestamps and user attribution — no record shall be hard-deleted |
| NFR-07 | Compliance | The system shall comply with applicable Canadian data privacy regulations including PIPEDA |
| NFR-08 | Maintainability | Code and database schema documentation shall be delivered by NexGen Solutions at project closure |
| NFR-09 | Usability | Core recruiter workflows (add candidate, update status, post job) shall be completable within 3 clicks from the main navigation |
| NFR-10 | Backup | Database backups shall run daily with a 30-day retention period |

---

## 10. Integration Requirements

| ID | Integration | Type | Description |
|---|---|---|---|
| IR-01 | Indeed Canada | REST API (Public) | Job posting and status sync |
| IR-02 | Glassdoor | REST API (Public) | Job posting distribution |
| IR-03 | Wellfound | REST API (Public) | Job posting distribution |
| IR-04 | ZipRecruiter | REST API (Public) | Job posting distribution |
| IR-05 | PostJobFree | REST API (Public) | Job posting distribution |
| IR-06 | Hubstaff Talent | REST API (Public) | Job posting distribution |
| IR-07 | emergiTEL Website | Internal CMS API | Job listing synchronization |
| IR-08 | Microsoft Outlook | Microsoft Graph API | Email and calendar integration |
| IR-09 | Google Looker Studio | Database connector | Dashboard and reporting |
| IR-10 | PostgreSQL | Cloud database | Central data store |

---

## 11. Data Requirements

| Entity | Key Attributes |
|---|---|
| User | UserID, Name, Email, Role, Department, Status, CreatedDate |
| Job Requisition | JobID, Title, Department, Location, Type, Status, PostedDate, ClosedDate, TargetBoards |
| Candidate | CandidateID, Name, Email, Phone, Source, AssignedRecruiter, LinkedJobID, Stage, ResumeURL, CreatedDate |
| Interview | InterviewID, CandidateID, JobID, Type, DateTime, Interviewers, Outcome, Notes |
| Offer | OfferID, CandidateID, JobID, OfferDate, StartDate, Compensation, Type, Status |
| Placement | PlacementID, CandidateID, ClientID, JobID, StartDate, EndDate, BillRate, PayRate, RecruiterID |
| Timesheet | TimesheetID, PlacementID, PeriodStart, PeriodEnd, TotalHours, Status, ApprovedBy, ApprovedDate |
| PayrollRecord | PayrollID, PlacementID, TimesheetID, Period, GrossPay, Status, GeneratedDate |
| EmailLog | LogID, RecipientType, RecipientID, Subject, SentDate, TriggeredBy |
| AuditLog | LogID, UserID, EntityType, EntityID, Action, OldValue, NewValue, Timestamp |

---

## 12. Assumptions and Constraints

### Assumptions

- All seven job boards maintain active public APIs with acceptable rate limits for emergiTEL's posting volume
- Microsoft 365 licenses are available for Outlook/MS Graph API integration
- NexGen Solutions will adhere to the agreed technology stack and deliver code with inline documentation
- Recruiters and finance staff will participate in user acceptance testing prior to go-live
- The cloud hosting environment will be provisioned and managed by NexGen Solutions for the initial phase

### Constraints

- Budget ceiling for MVP delivery is fixed — post-MVP features require separate budgetary approval
- The system must be operational before the BreezyHR subscription renewal date to avoid unnecessary renewal cost
- The emergiTEL website CMS API must be accessible and documented by the internal web team prior to integration development
- Data migration from BreezyHR is limited to active candidates and open job requisitions only

---

## 13. Dependencies

| ID | Dependency | Owner | Impact if Delayed |
|---|---|---|---|
| DEP-01 | Access to all job board API credentials | Ankit / NexGen Solutions | Delays job distribution module |
| DEP-02 | Microsoft 365 tenant access for Graph API setup | IT / Akbar Khan | Delays email integration module |
| DEP-03 | emergiTEL website CMS API documentation | Internal web team | Delays website job sync |
| DEP-04 | PostgreSQL cloud environment provisioning | NexGen Solutions | Blocks all development |
| DEP-05 | Looker Studio access and database connector setup | Rehan Qazi | Delays dashboard delivery |
| DEP-06 | Recruiter availability for UAT | Sonika Salhotra | Delays sign-off and go-live |

---

## 14. Risks

| ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| RK-01 | Job board API rate limits or deprecation | Medium | High | Monitor API status; build retry and fallback logic |
| RK-02 | BreezyHR data migration complexity | Medium | Medium | Limit migration to active records only; manual validation by SME |
| RK-03 | Scope creep from stakeholder requests post-BRD sign-off | High | Medium | Enforce formal change request process; BA owns change log |
| RK-04 | NexGen Solutions delivery delay | Medium | High | Include milestone-based payment schedule; weekly delivery reviews |
| RK-05 | Recruiter adoption resistance | Low | High | Early recruiter involvement in UAT; training sessions pre-launch |
| RK-06 | MS Graph API permissions not granted by IT | Low | High | Escalate to Managing Director early; include in pre-dev checklist |
| RK-07 | Data security breach during transition | Low | Critical | Enforce data encryption, access controls, and penetration testing pre-launch |

---

## 15. Acceptance Criteria

The system shall be considered ready for go-live when all of the following conditions are met:

1. All functional requirements in Section 8 have been developed and verified
2. All UAT test cases defined in the UAT Plan document have passed
3. No critical or high-severity defects remain open
4. Job posting to all 7 boards functions successfully in the staging environment
5. Outlook integration successfully sends calendar invites and emails in staging
6. Payroll auto-trigger fires correctly upon timesheet approval in staging
7. Looker Studio dashboard displays live data accurately
8. Role-based access control is verified across all user roles
9. Non-functional requirements (NFR-01 through NFR-10) have been validated
10. Formal UAT sign-off obtained from Sonika Salhotra (Recruitment SME) and Aneela Zaib (Company Owner)

---

## 16. Sign-Off

| Name | Role | Signature | Date |
|---|---|---|---|
| Aneela Zaib | Company Owner | __________________ | __________ |
| Akbar Khan | Managing Director | __________________ | __________ |
| Sonika Salhotra | Recruitment SME | __________________ | __________ |
| Ankit Prakash Srivastava | Lead Business Analyst | __________________ | __________ |

---

*Version 1.0 — This document is controlled. Any changes after sign-off require a formal Change Request submitted to the Lead Business Analyst.*
