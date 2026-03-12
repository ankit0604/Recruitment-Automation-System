# User Acceptance Testing (UAT) Plan & Test Cases
## Recruitment Process Automation — emergiTEL Inc.

---

**Document Version:** 1.0  
**Prepared By:** Ankit Prakash Srivastava — Lead Business Analyst  
**UAT Lead (Execution):** Sonika Salhotra — Recruitment SME  
**Date:** 2024  

---

## Table of Contents

1. [UAT Objectives](#1-uat-objectives)
2. [UAT Scope](#2-uat-scope)
3. [UAT Entry and Exit Criteria](#3-uat-entry-and-exit-criteria)
4. [UAT Environment & Access](#4-uat-environment--access)
5. [Roles and Responsibilities](#5-roles-and-responsibilities)
6. [Test Schedule](#6-test-schedule)
7. [Defect Classification](#7-defect-classification)
8. [Defect Management Process](#8-defect-management-process)
9. [Test Cases — User Management](#9-test-cases--user-management)
10. [Test Cases — Job Requisition & Multi-Board Posting](#10-test-cases--job-requisition--multi-board-posting)
11. [Test Cases — Candidate Management](#11-test-cases--candidate-management)
12. [Test Cases — Interview Scheduling](#12-test-cases--interview-scheduling)
13. [Test Cases — Offer Management](#13-test-cases--offer-management)
14. [Test Cases — Placement & Finance](#14-test-cases--placement--finance)
15. [Test Cases — Timesheet & Payroll Automation](#15-test-cases--timesheet--payroll-automation)
16. [Test Cases — Email Integration (MS Graph API)](#16-test-cases--email-integration-ms-graph-api)
17. [Test Cases — Dashboard & Reporting](#17-test-cases--dashboard--reporting)
18. [UAT Defect Log](#18-uat-defect-log)
19. [UAT Sign-Off](#19-uat-sign-off)

---

## 1. UAT Objectives

The purpose of UAT is to validate that the emergiTEL Recruitment Automation system meets the business requirements defined in the BRD and delivers the expected outcomes across all defined use cases. UAT is the final verification gate before go-live approval.

Specifically, UAT will confirm:
- All functional requirements (FR-UM through FR-RD) are correctly implemented
- The system behaves as expected under normal operating conditions and defined exception scenarios
- Integrations with job boards, MS Graph API (Outlook), and Looker Studio are functioning correctly
- Automated workflows (payroll trigger, placement creation) execute without manual intervention
- Role-based access controls are correctly enforced
- End users (recruiters, managers, finance team) can complete their core workflows without assistance

---

## 2. UAT Scope

### In Scope

- User management and role-based access
- Job requisition creation and multi-board job posting
- Candidate add, update, and pipeline management
- Interview scheduling with Outlook integration
- Offer creation and management
- Placement auto-creation from offer acceptance
- Timesheet submission and approval
- Payroll record auto-generation
- Email notifications via MS Graph API
- Looker Studio dashboard accuracy

### Out of Scope for UAT

- Performance/load testing (separate test phase)
- Penetration/security testing (separate engagement)
- Post-MVP features not yet developed

---

## 3. UAT Entry and Exit Criteria

### Entry Criteria (UAT begins when all are met)

- [ ] NexGen Solutions has delivered the UAT environment with all MVP features deployed
- [ ] All unit and integration tests by the development team are complete with no open critical defects
- [ ] Test data has been loaded in the UAT environment (sample candidates, jobs, users)
- [ ] UAT test accounts are provisioned for all roles (Admin, Recruiter, Manager, Finance, Candidate)
- [ ] MS Graph API is configured and connected in the UAT environment
- [ ] All 7 job board API connections are active in the UAT environment (sandbox/test mode where available)
- [ ] Looker Studio is connected to the UAT database
- [ ] This UAT Plan has been reviewed and approved by the Lead BA

### Exit Criteria (UAT closes when all are met)

- [ ] 100% of test cases executed
- [ ] All Critical and High severity defects resolved and re-tested
- [ ] No more than 3 open Medium severity defects, each with a documented remediation plan and timeline
- [ ] Low severity defects documented and scheduled for post-go-live release
- [ ] UAT sign-off obtained from Sonika Salhotra (Recruitment SME) and Aneela Zaib (Company Owner)

---

## 4. UAT Environment & Access

| Item | Detail |
|---|---|
| **Environment** | Staging / UAT (separate from production) |
| **URL** | To be provided by NexGen Solutions |
| **Database** | PostgreSQL — UAT instance (pre-loaded with test data) |
| **Looker Studio** | Connected to UAT database — separate from production report |
| **Email Integration** | MS Graph API connected to a test Outlook account |
| **Job Board APIs** | Sandbox / test endpoints where available; limited live posting for verification |
| **Access Provisioned By** | NexGen Solutions (Admin credentials) |
| **UAT Duration** | 2 weeks |

---

## 5. Roles and Responsibilities

| Name | Role | UAT Responsibility |
|---|---|---|
| Ankit Prakash Srivastava | Lead BA | UAT plan owner, test case author, defect triage, final review |
| Sonika Salhotra | Recruitment SME / UAT Lead | Execute recruiter and manager test cases, log defects, sign-off |
| Rehan Qazi | Data Analyst | Validate dashboard KPI accuracy, verify database records post-automation |
| NexGen Solutions | Development Vendor | Defect resolution, environment support, re-testing after fixes |
| Aneela Zaib | Company Owner | Final UAT sign-off authority |

---

## 6. Test Schedule

| Phase | Activity | Owner | Duration |
|---|---|---|---|
| Preparation | Environment setup, test data load, access provisioning | NexGen Solutions | 3 days |
| Execution — Wave 1 | User Management, Job Requisition, Candidate Management | Sonika + Ankit | 3 days |
| Execution — Wave 2 | Interview, Offer, Placement, Finance, Timesheet, Payroll | Sonika + Ankit | 4 days |
| Execution — Wave 3 | Email integration, Dashboard, Role-based access, Edge cases | Ankit + Rehan | 2 days |
| Defect Resolution | NexGen Solutions fixes Critical/High defects | NexGen Solutions | 3 days |
| Re-Testing | Regression on fixed defects | Sonika + Ankit | 1 day |
| Sign-Off | Final review and stakeholder sign-off | Ankit + Aneela | 1 day |

---

## 7. Defect Classification

| Severity | Definition | Resolution SLA |
|---|---|---|
| Critical | System crash, data loss, core workflow completely broken, payroll not triggering | Fix before go-live — blocks sign-off |
| High | Major feature not working as specified, incorrect data displayed, integration failure | Fix before go-live — blocks sign-off |
| Medium | Feature partially working, workaround available, non-critical field issue | Fix scheduled before go-live or in first patch |
| Low | Minor UI issues, labelling errors, cosmetic defects | Scheduled for post-go-live release |

---

## 8. Defect Management Process

1. Tester identifies a defect during test case execution
2. Defect is logged in the Defect Log (Section 18) with: ID, test case reference, description, severity, screenshots, steps to reproduce
3. Lead BA reviews and confirms severity classification
4. Defect is communicated to NexGen Solutions with reproduction steps
5. NexGen Solutions resolves and marks as "Ready for Re-Test"
6. Lead BA or SME retests in UAT environment
7. If resolved — defect marked "Closed"; if not — defect reopened
8. All Critical and High defects must be Closed before exit criteria are met

---

## 9. Test Cases — User Management

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-UM-01 | Create a new user with Recruiter role | 1. Log in as Admin. 2. Navigate to User Management. 3. Click Add User. 4. Enter name, email, role = Recruiter. 5. Save. | User account created; welcome email sent; user appears in list with correct role | | |
| TC-UM-02 | Log in as newly created Recruiter | 1. Use credentials from welcome email. 2. Log in. | Recruiter dashboard loads; only Recruiter-accessible modules are visible | | |
| TC-UM-03 | Verify Finance user cannot access Recruitment pipeline | 1. Log in as Finance user. 2. Attempt to navigate to Candidates module. | Access denied; appropriate message displayed | | |
| TC-UM-04 | Deactivate a user | 1. Log in as Admin. 2. Select an active Recruiter. 3. Click Deactivate. | User account deactivated; user cannot log in; historical records remain intact | | |
| TC-UM-05 | Audit log captures user creation | 1. Create a user. 2. Navigate to Audit Log. | Audit entry exists for user creation with timestamp and Admin user ID | | |

---

## 10. Test Cases — Job Requisition & Multi-Board Posting

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-JR-01 | Create a job requisition as a Recruiter | 1. Log in as Recruiter. 2. Navigate to Job Requisitions. 3. Click Create New Job. 4. Fill all required fields. 5. Set status to Open. 6. Click Save. | Job requisition saved with status Open; appears in requisition list | | |
| TC-JR-02 | Publish job to all 7 boards simultaneously | 1. Open a job requisition. 2. Select all 7 boards. 3. Click Publish. | Job posted to all 7 boards; confirmation screen shows Success status for each board | | |
| TC-JR-03 | Verify partial posting failure handling | 1. Simulate a failure for one board (test mode). 2. Publish to all 7. | Successful boards confirmed; failed board flagged with error message; recruiter notified | | |
| TC-JR-04 | Edit and re-publish a job | 1. Open an existing Open requisition. 2. Edit job description. 3. Re-publish to selected boards. | Updated description posted to selected boards; previous posting overwritten | | |
| TC-JR-05 | Close a job requisition | 1. Open an Open requisition. 2. Set status to Closed. 3. Save. | Requisition status updated to Closed; if API supports it, job removed from active boards | | |
| TC-JR-06 | Attempt to publish without selecting a board | 1. Create a job. 2. Leave board selection empty. 3. Click Publish. | System validation error: "At least one job board must be selected" | | |

---

## 11. Test Cases — Candidate Management

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-CM-01 | Add a new candidate manually | 1. Log in as Recruiter. 2. Navigate to Candidates. 3. Click Add Candidate. 4. Fill name, email, phone, source, link to a job. 5. Upload resume. 6. Save. | Candidate record created; stage = New; recruiter assigned; resume linked | | |
| TC-CM-02 | Duplicate candidate detection | 1. Add a candidate with an email that already exists in the system. | System displays warning: "A candidate with this email already exists"; prevents duplicate creation | | |
| TC-CM-03 | Advance candidate stage | 1. Open a candidate record. 2. Update stage from New to Submitted to Client. 3. Save. | Stage updated; transition logged with timestamp and recruiter ID in audit trail | | |
| TC-CM-04 | Search candidates by stage and recruiter | 1. Navigate to Candidate list. 2. Apply filter: Stage = Interview Scheduled, Recruiter = [name]. | Only matching candidates displayed | | |
| TC-CM-05 | Verify candidate record is not hard-deleted | 1. Attempt to delete a candidate record. | System does not allow hard delete; record can only be archived | | |

---

## 12. Test Cases — Interview Scheduling

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-IS-01 | Schedule an interview from candidate record | 1. Open a candidate in Submitted to Client stage. 2. Click Schedule Interview. 3. Select type = Video, enter date/time, add interviewer, add video link. 4. Confirm. | Interview record created; candidate stage updated to Interview Scheduled | | |
| TC-IS-02 | Outlook calendar invite sent to candidate | 1. Complete TC-IS-01. 2. Check the test candidate's Outlook inbox. | Calendar invite received by candidate with correct date, time, and video link | | |
| TC-IS-03 | Outlook calendar invite sent to interviewer | 1. Complete TC-IS-01. 2. Check the assigned interviewer's Outlook inbox. | Calendar invite received by interviewer | | |
| TC-IS-04 | 24-hour reminder sent automatically | 1. Schedule an interview for a future date. 2. Wait until 24 hours before (or simulate in test). | Reminder email sent to candidate and interviewer via Outlook | | |
| TC-IS-05 | Email log captures interview scheduling action | 1. Complete TC-IS-01. 2. Navigate to Email Log for this candidate. | Entry exists for calendar invite action with timestamp | | |
| TC-IS-06 | Schedule second-round interview | 1. Record first interview outcome as Pending Further Rounds. 2. Schedule a new interview. | New interview record created; candidate stage remains Interview Scheduled | | |

---

## 13. Test Cases — Offer Management

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-OM-01 | Create an offer record | 1. Open a candidate with interview outcome = Passed. 2. Click Create Offer. 3. Enter offer date, start date, compensation, type = Contract. 4. Click Send Offer. | Offer record created with status Sent; candidate stage updated to Offer Extended | | |
| TC-OM-02 | Offer email sent via Outlook | 1. Complete TC-OM-01. 2. Check test candidate's email inbox. | Offer email received with correct offer details | | |
| TC-OM-03 | Mark offer as Accepted | 1. Open an offer with status Sent. 2. Update status to Accepted. 3. Save. | Offer status = Accepted; placement record auto-created (verify TC-PF-01) | | |
| TC-OM-04 | Mark offer as Declined | 1. Open an offer with status Sent. 2. Update status to Declined. 3. Save. | Offer status = Declined; candidate stage updated to Offer Declined | | |
| TC-OM-05 | Verify placement not created on declined offer | 1. Complete TC-OM-04. 2. Navigate to Finance module / Placements. | No placement record created for the declined candidate | | |

---

## 14. Test Cases — Placement & Finance

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-PF-01 | Placement record auto-created on offer acceptance | 1. Complete TC-OM-03 (offer accepted). 2. Navigate to Finance module. | Placement record exists with correct: Candidate, Client, Job, Start Date, Bill Rate, Pay Rate, Recruiter | | |
| TC-PF-02 | Finance user notified of new placement | 1. Complete TC-PF-01. 2. Log in as Finance user. | Notification visible; new placement appears in finance module | | |
| TC-PF-03 | Candidate stage updated to Placed | 1. Complete TC-OM-03. 2. Return to candidate record. | Candidate stage = Placed | | |
| TC-PF-04 | Finance user can view and export placement data | 1. Log in as Finance user. 2. Navigate to Placements. 3. Apply date filter. 4. Export to CSV. | Filtered placements displayed; CSV export successful with all expected columns | | |
| TC-PF-05 | Timesheet module activated for placed candidate | 1. Complete TC-PF-01. 2. Log in as Candidate user linked to the placement. | Timesheet submission option is available for the active placement period | | |

---

## 15. Test Cases — Timesheet & Payroll Automation

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-TP-01 | Candidate submits timesheet | 1. Log in as Candidate. 2. Navigate to Timesheet. 3. Enter daily hours for the week. 4. Click Submit. | Timesheet record created with status Submitted; approver notified | | |
| TC-TP-02 | Approver receives and reviews timesheet notification | 1. Complete TC-TP-01. 2. Log in as Manager. | Notification visible; timesheet accessible for review | | |
| TC-TP-03 | Approver approves timesheet | 1. Complete TC-TP-02. 2. Click Approve. | Timesheet status = Approved; timestamp and approver ID logged | | |
| TC-TP-04 | Payroll record auto-generated on approval | 1. Complete TC-TP-03. 2. Navigate to Finance module — Payroll. | Payroll record created with correct: Period, Total Hours, Gross Pay (Hours × Pay Rate), Status = Pending Processing | | |
| TC-TP-05 | Gross pay calculation accuracy | 1. Create a timesheet for 40 hours. 2. Placement pay rate = CAD 25/hr. 3. Approve. 4. Check payroll record. | Gross Pay = 40 × 25 = CAD 1,000.00 | | |
| TC-TP-06 | Approver rejects timesheet with comments | 1. Submit a timesheet with incorrect hours. 2. Log in as Manager. 3. Click Reject, enter reason. | Timesheet status = Rejected; candidate notified; candidate can resubmit | | |
| TC-TP-07 | Payroll anomaly flag triggers when record not generated | 1. Approve a timesheet. 2. Simulate system failure preventing payroll record creation. 3. Wait 2 hours (or simulate time). | System generates a Payroll Anomaly flag; Admin and Finance user notified | | |
| TC-TP-08 | No manual step required between timesheet approval and payroll record | 1. Complete TC-TP-03. 2. Monitor Finance module without taking any action. | Payroll record appears automatically within the expected window — zero manual action required | | |

---

## 16. Test Cases — Email Integration (MS Graph API)

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-EI-01 | Interview calendar invite delivered via Outlook | 1. Schedule an interview (TC-IS-01). 2. Check candidate and interviewer Outlook accounts. | Calendar events appear in both Outlook calendars | | |
| TC-EI-02 | Offer email delivered via Outlook | 1. Send an offer (TC-OM-01). 2. Check candidate Outlook inbox. | Offer email received with correct formatting and offer details | | |
| TC-EI-03 | Email log linked to candidate record | 1. Send a submission email. 2. Navigate to the candidate record. 3. View email log. | Email log entry visible within the candidate record with correct metadata | | |
| TC-EI-04 | Recruiter can manually trigger email from candidate record | 1. Open a candidate record. 2. Click Send Email. 3. Write and send message. | Email sent via Outlook; entry logged in Email Log | | |
| TC-EI-05 | Email failure handled gracefully | 1. Simulate MS Graph API failure. 2. Trigger an email send. | System logs the failure; user notified; action can be retried | | |

---

## 17. Test Cases — Dashboard & Reporting

| TC ID | Test Case Description | Test Steps | Expected Result | Pass / Fail | Defect ID |
|---|---|---|---|---|---|
| TC-DR-01 | Looker Studio connects to UAT database | 1. Open Looker Studio dashboard. | Dashboard loads without connection errors; data visible | | |
| TC-DR-02 | Recruiter submission count accurate | 1. Add 3 candidates as Recruiter A and submit to client. 2. Open Dashboard 1. 3. Filter by Recruiter A. | Submissions = 3 | | |
| TC-DR-03 | Placement count reflects ATS placements | 1. Create 2 placements for Recruiter B. 2. Open Dashboard 1. 3. Filter by Recruiter B. | Placements = 2 | | |
| TC-DR-04 | Payroll anomaly flag shows on dashboard | 1. Trigger TC-TP-07 scenario. 2. Open Dashboard 3. | Alert card shows anomaly in red | | |
| TC-DR-05 | Job board failure rate calculated correctly | 1. Post jobs with 2 failures out of 10 attempts on Indeed. 2. Open Dashboard 4. | Indeed failure rate = 20% | | |
| TC-DR-06 | Manager role can access all dashboards | 1. Log in as Manager. 2. Navigate to all 4 dashboards. | All dashboards accessible | | |
| TC-DR-07 | Recruiter role cannot access Finance dashboard | 1. Log in as Recruiter. 2. Attempt to access Finance & Payroll dashboard. | Access denied; appropriate message shown | | |

---

## 18. UAT Defect Log

| Defect ID | TC Reference | Description | Severity | Status | Assigned To | Resolution Notes | Closed Date |
|---|---|---|---|---|---|---|---|
| DEF-001 | | | | Open | | | |
| DEF-002 | | | | Open | | | |

*This log will be populated during UAT execution. All defects are logged by the tester, reviewed by the Lead BA, and assigned to NexGen Solutions for resolution.*

---

## 19. UAT Sign-Off

### Sign-Off Statement

By signing below, the signatory confirms that:
- UAT has been conducted in accordance with this UAT Plan
- All exit criteria defined in Section 3 have been met
- The system is approved for deployment to the production environment
- Any outstanding Low severity defects have been reviewed and accepted as known items with documented remediation timelines

### Sign-Off Log

| Name | Role | Signature | Date | Notes |
|---|---|---|---|---|
| Sonika Salhotra | Recruitment SME / UAT Lead | __________________ | __________ | |
| Ankit Prakash Srivastava | Lead Business Analyst | __________________ | __________ | |
| Rehan Qazi | Data Analyst | __________________ | __________ | Dashboard KPIs validated |
| Aneela Zaib | Company Owner | __________________ | __________ | Final Go-Live Approval |

---

### Post-UAT Actions Before Go-Live

| Action | Owner | Target Date |
|---|---|---|
| Production environment provisioned by NexGen Solutions | NexGen Solutions | TBD |
| Data migration from BreezyHR (active records only) completed | NexGen Solutions + Ankit | TBD |
| Production MS Graph API credentials configured | IT / NexGen Solutions | TBD |
| Production job board API credentials configured | Ankit / NexGen Solutions | TBD |
| Looker Studio connected to production database | Rehan Qazi | TBD |
| Recruiter team onboarding / training session conducted | Ankit + Sonika | TBD |
| Go-live date communicated to all stakeholders | Akbar Khan | TBD |

---

*UAT Plan Version 1.0 — Prepared by Ankit Prakash Srivastava, Lead Business Analyst, emergiTEL Inc.*
