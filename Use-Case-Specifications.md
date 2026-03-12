# Use Case Specifications
## Recruitment Process Automation — emergiTEL Inc.

---

**Document Version:** 1.0  
**Prepared By:** Ankit Prakash Srivastava — Lead Business Analyst  
**Reviewed By:** Sonika Salhotra — Recruitment SME  
**Date:** 2025

---

## Table of Contents

1. [Use Case Overview](#1-use-case-overview)
2. [Actor Definitions](#2-actor-definitions)
3. [Use Case Diagram Summary](#3-use-case-diagram-summary)
4. [UC-01 — Create and Publish Job Requisition](#4-uc-01--create-and-publish-job-requisition)
5. [UC-02 — Add and Manage Candidate Record](#5-uc-02--add-and-manage-candidate-record)
6. [UC-03 — Submit Candidate to Client](#6-uc-03--submit-candidate-to-client)
7. [UC-04 — Schedule Interview via Outlook Integration](#7-uc-04--schedule-interview-via-outlook-integration)
8. [UC-05 — Record Interview Outcome](#8-uc-05--record-interview-outcome)
9. [UC-06 — Create and Manage Offer](#9-uc-06--create-and-manage-offer)
10. [UC-07 — Confirm Placement](#10-uc-07--confirm-placement)
11. [UC-08 — Submit and Approve Timesheet](#11-uc-08--submit-and-approve-timesheet)
12. [UC-09 — Auto-Generate Payroll Record](#12-uc-09--auto-generate-payroll-record)
13. [UC-10 — View Recruiter Performance Dashboard](#13-uc-10--view-recruiter-performance-dashboard)
14. [UC-11 — Manage Users and Roles](#14-uc-11--manage-users-and-roles)

---

## 1. Use Case Overview

| Use Case ID | Use Case Name | Primary Actor | Priority |
|---|---|---|---|
| UC-01 | Create and Publish Job Requisition | Recruiter | High |
| UC-02 | Add and Manage Candidate Record | Recruiter | High |
| UC-03 | Submit Candidate to Client | Recruiter | High |
| UC-04 | Schedule Interview via Outlook Integration | Recruiter | High |
| UC-05 | Record Interview Outcome | Recruiter | High |
| UC-06 | Create and Manage Offer | Recruiter | High |
| UC-07 | Confirm Placement | Recruiter / System | High |
| UC-08 | Submit and Approve Timesheet | Candidate / Manager | High |
| UC-09 | Auto-Generate Payroll Record | System | High |
| UC-10 | View Recruiter Performance Dashboard | Manager / Admin | Medium |
| UC-11 | Manage Users and Roles | Admin | Medium |

---

## 2. Actor Definitions

| Actor | Type | Description |
|---|---|---|
| Recruiter | Primary | Manages candidate pipeline, job postings, interviews, and offers |
| Manager | Primary | Reviews candidate submissions, approves timesheets, accesses reports |
| Finance User | Primary | Reviews placement records, monitors payroll data |
| Admin | Primary | Manages users, roles, and system configuration |
| Candidate | External | Submits timesheets, receives interview invites and offer communications |
| ATS System | System | Executes automated actions — status updates, payroll triggers, notifications |
| MS Graph API | External System | Delivers Outlook calendar invites and email communications |
| Job Board APIs | External System | Receives and publishes job postings via public REST APIs |
| Looker Studio | External System | Connects to database and renders KPI dashboards |

---

## 3. Use Case Diagram Summary

```
+-----------------------------------------------+
|              ATS System Boundary               |
|                                                 |
|  Recruiter ----> UC-01: Create & Publish Job   |
|             ----> UC-02: Manage Candidates     |
|             ----> UC-03: Submit to Client      |
|             ----> UC-04: Schedule Interview    |
|             ----> UC-05: Record Outcome        |
|             ----> UC-06: Create Offer          |
|             ----> UC-07: Confirm Placement     |
|                                                 |
|  Candidate  ----> UC-08: Submit Timesheet      |
|  Manager    ----> UC-08: Approve Timesheet     |
|             ----> UC-10: View Dashboard        |
|                                                 |
|  System     ----> UC-09: Generate Payroll      |
|                   (triggered automatically)     |
|                                                 |
|  Admin      ----> UC-11: Manage Users/Roles    |
|                                                 |
+-----------------------------------------------+
         |                      |
         v                      v
  Job Board APIs         MS Graph API
  (UC-01 extends)        (UC-04, UC-06 extends)
```

---

## 4. UC-01 — Create and Publish Job Requisition

| Field | Detail |
|---|---|
| **Use Case ID** | UC-01 |
| **Use Case Name** | Create and Publish Job Requisition |
| **Primary Actor** | Recruiter |
| **Secondary Actors** | Job Board APIs (Indeed Canada, Glassdoor, Wellfound, ZipRecruiter, PostJobFree, Hubstaff Talent, emergiTEL Website) |
| **Trigger** | A client submits a job order or an internal hiring need is identified |
| **Preconditions** | Recruiter is authenticated; Job board API credentials are configured |
| **Postconditions** | Job requisition is saved in ATS; Job is published to selected boards; Confirmation status shown per board |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Recruiter | Navigates to Job Requisitions module and clicks "Create New Job" |
| 2 | Recruiter | Fills in: Job Title, Department, Location, Job Type, Description, Required Skills, Compensation Range, Hiring Manager |
| 3 | Recruiter | Selects target job boards from a checklist (one or more of the 7 boards) |
| 4 | Recruiter | Sets requisition status to "Open" and clicks "Publish" |
| 5 | ATS System | Calls the public REST API for each selected job board simultaneously |
| 6 | ATS System | Receives success or error response from each board's API |
| 7 | ATS System | Displays posting confirmation screen showing status per board (Success / Failed) |
| 8 | ATS System | Saves the requisition record with status "Open" and posting timestamp |

**Alternative Flows:**

| Step | Condition | Action |
|---|---|---|
| 6a | One or more board APIs return an error | ATS displays which boards failed with the error message; successful boards are confirmed; recruiter can retry failed boards individually |
| 4a | Recruiter saves as "Draft" instead of publishing | Requisition saved without API calls; can be published later |

**Exception Flows:**

| Step | Condition | Action |
|---|---|---|
| 5a | API credential for a board has expired | System flags the board as requiring re-authentication; Admin is notified |
| 5b | Job board API is unavailable | System logs the failure; recruiter is notified; retry option presented |

**Business Rules:**
- A job cannot be published without at least one target board selected
- Compensation range is optional but recommended
- Job descriptions must be at least 100 characters to meet minimum board submission requirements
- Duplicate job titles with identical locations and open status will prompt a conflict warning

---

## 5. UC-02 — Add and Manage Candidate Record

| Field | Detail |
|---|---|
| **Use Case ID** | UC-02 |
| **Use Case Name** | Add and Manage Candidate Record |
| **Primary Actor** | Recruiter |
| **Trigger** | A candidate applies via job board or is sourced directly by a recruiter |
| **Preconditions** | Recruiter is authenticated; At least one open job requisition exists |
| **Postconditions** | Candidate record created in ATS with a unique ID; Record linked to associated job requisition |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Recruiter | Navigates to Candidates module and clicks "Add Candidate" |
| 2 | Recruiter | Enters: Full Name, Email, Phone, Source (job board / referral / direct), Resume upload |
| 3 | Recruiter | Links candidate to one or more open job requisitions |
| 4 | Recruiter | Sets initial stage to "New" |
| 5 | ATS System | Checks for duplicate records based on email address |
| 6 | ATS System | Saves candidate record with CreatedDate and AssignedRecruiter |
| 7 | Recruiter | Can add notes, update stage, or advance pipeline from this record |

**Alternative Flows:**

| Step | Condition | Action |
|---|---|---|
| 5a | Duplicate email detected | System displays a warning: "A candidate with this email already exists." Recruiter can view existing record or override to create a new linked entry |
| 3a | No open requisition exists | Recruiter can save the candidate record without a linked requisition; association can be made later |

**Business Rules:**
- Email address is a required field and is the primary deduplication key
- All stage transitions are logged with timestamp and acting user — no silent edits permitted
- Candidate records are never hard-deleted; they are archived

---

## 6. UC-03 — Submit Candidate to Client

| Field | Detail |
|---|---|
| **Use Case ID** | UC-03 |
| **Use Case Name** | Submit Candidate to Client |
| **Primary Actor** | Recruiter |
| **Secondary Actors** | MS Graph API (Outlook) |
| **Trigger** | Recruiter determines the candidate is a match for the open role and ready for client review |
| **Preconditions** | Candidate record exists and is in "New" or "Reviewed" stage; Job requisition is Open |
| **Postconditions** | Candidate stage updated to "Submitted to Client"; Submission timestamp recorded; Email sent to client via Outlook |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Recruiter | Opens candidate record and clicks "Submit to Client" |
| 2 | Recruiter | Selects the client contact from the system and reviews submission email draft |
| 3 | Recruiter | Confirms send |
| 4 | ATS System | Calls MS Graph API to send the email via Outlook |
| 5 | ATS System | Updates candidate stage to "Submitted to Client" |
| 6 | ATS System | Logs the submission action with timestamp and recruiter ID |
| 7 | ATS System | Logs the outbound email to the Email Log module |

**Business Rules:**
- Resume must be attached before submission is permitted
- Only recruiters assigned to the job or managers can trigger a submission

---

## 7. UC-04 — Schedule Interview via Outlook Integration

| Field | Detail |
|---|---|
| **Use Case ID** | UC-04 |
| **Use Case Name** | Schedule Interview via Outlook Integration |
| **Primary Actor** | Recruiter |
| **Secondary Actors** | MS Graph API, Candidate, Interviewer |
| **Trigger** | Client or internal team requests an interview following candidate submission |
| **Preconditions** | Candidate is in "Submitted to Client" stage; MS Graph API is configured and authenticated |
| **Postconditions** | Interview record created in ATS; Calendar invites sent to candidate and interviewer(s) via Outlook; Candidate stage updated to "Interview Scheduled"; Reminder scheduled for 24 hours prior |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Recruiter | Opens candidate record and clicks "Schedule Interview" |
| 2 | Recruiter | Selects interview type (phone / video / in-person), date, time, and adds interviewer(s) from the system |
| 3 | Recruiter | Enters the video link or meeting location |
| 4 | Recruiter | Clicks "Confirm and Send" |
| 5 | ATS System | Creates an interview record linked to the candidate and job |
| 6 | ATS System | Calls MS Graph API to create a calendar event and send invites to candidate and interviewer(s) |
| 7 | ATS System | Updates candidate stage to "Interview Scheduled" |
| 8 | ATS System | Schedules an automated 24-hour reminder via MS Graph API |
| 9 | ATS System | Logs email and calendar action in Email Log |

**Alternative Flows:**

| Step | Condition | Action |
|---|---|---|
| 6a | MS Graph API returns an error | ATS logs the failure; Recruiter is notified to send manually; Interview record is still saved in ATS |

**Business Rules:**
- An interview cannot be scheduled without at least one interviewer assigned
- Multiple interviews can be scheduled for the same candidate on the same job (e.g., first round, second round)
- Rescheduling creates a new interview record and cancels the previous calendar event via the API

---

## 8. UC-05 — Record Interview Outcome

| Field | Detail |
|---|---|
| **Use Case ID** | UC-05 |
| **Use Case Name** | Record Interview Outcome |
| **Primary Actor** | Recruiter |
| **Trigger** | Interview has concluded and the client has provided feedback |
| **Preconditions** | Interview record exists with status "Scheduled" |
| **Postconditions** | Interview record updated with outcome and notes; Candidate stage updated automatically |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Recruiter | Opens the interview record from the candidate profile |
| 2 | Recruiter | Selects outcome: Passed / Not Selected / Pending Further Rounds |
| 3 | Recruiter | Enters feedback notes |
| 4 | Recruiter | Clicks "Save Outcome" |
| 5 | ATS System | Updates interview record with outcome and timestamp |
| 6 | ATS System | Auto-updates candidate stage based on outcome mapping |

**Outcome-to-Stage Mapping:**

| Interview Outcome | Candidate Stage |
|---|---|
| Passed | Interview Completed — Advance to Offer |
| Not Selected | Rejected |
| Pending Further Rounds | Interview Scheduled (next round) |

---

## 9. UC-06 — Create and Manage Offer

| Field | Detail |
|---|---|
| **Use Case ID** | UC-06 |
| **Use Case Name** | Create and Manage Offer |
| **Primary Actor** | Recruiter |
| **Secondary Actors** | MS Graph API, Candidate |
| **Trigger** | Client decides to extend an offer to the candidate |
| **Preconditions** | Candidate interview outcome recorded as "Passed"; Recruiter has offer details from client |
| **Postconditions** | Offer record created in ATS; Offer communication sent via Outlook; Candidate stage updated to "Offer Extended" |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Recruiter | Opens candidate record and clicks "Create Offer" |
| 2 | Recruiter | Enters: Offer Date, Start Date, Compensation, Type (Contract / Permanent), Client Name |
| 3 | Recruiter | Reviews auto-populated offer email draft |
| 4 | Recruiter | Clicks "Send Offer" |
| 5 | ATS System | Calls MS Graph API to send offer email via Outlook |
| 6 | ATS System | Creates offer record with status "Sent" |
| 7 | ATS System | Updates candidate stage to "Offer Extended" |
| 8 | Recruiter | Upon receiving candidate response, updates offer status in ATS |
| 9a | ATS System | If Accepted: Offer status set to "Accepted" — triggers UC-07 (Confirm Placement) |
| 9b | ATS System | If Declined: Offer status set to "Declined" — candidate stage updated accordingly |

---

## 10. UC-07 — Confirm Placement

| Field | Detail |
|---|---|
| **Use Case ID** | UC-07 |
| **Use Case Name** | Confirm Placement |
| **Primary Actor** | ATS System (automated), Recruiter (verification) |
| **Secondary Actors** | Finance User |
| **Trigger** | Offer status is updated to "Accepted" in UC-06 |
| **Preconditions** | Offer record exists with status "Accepted" |
| **Postconditions** | Placement record automatically created in Finance Module; Finance team notified; Timesheet module activated for candidate |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | ATS System | Detects offer acceptance event |
| 2 | ATS System | Automatically creates a Placement record pre-populated with: Candidate, Client, Job, Start Date, Bill Rate, Pay Rate, Recruiter |
| 3 | ATS System | Updates candidate stage to "Placed" |
| 4 | ATS System | Notifies Finance User of the new placement |
| 5 | Finance User | Reviews placement record for accuracy |
| 6 | Finance User | Confirms or adjusts rates if required |
| 7 | ATS System | Activates timesheet submission for the placed candidate |

---

## 11. UC-08 — Submit and Approve Timesheet

| Field | Detail |
|---|---|
| **Use Case ID** | UC-08 |
| **Use Case Name** | Submit and Approve Timesheet |
| **Primary Actor** | Candidate (submitter), Manager / Finance User (approver) |
| **Trigger** | A work week is completed by a placed candidate |
| **Preconditions** | Candidate has an active placement record; Candidate account is provisioned |
| **Postconditions** | Timesheet record updated with approval status; Payroll record auto-generated (triggers UC-09) |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Candidate | Logs into the system and navigates to Timesheet module |
| 2 | Candidate | Enters daily hours for the work period |
| 3 | Candidate | Clicks "Submit Timesheet" |
| 4 | ATS System | Creates timesheet record with status "Submitted" and notifies the approver |
| 5 | Manager | Reviews submitted timesheet within the ATS |
| 6 | Manager | Clicks "Approve" |
| 7 | ATS System | Updates timesheet status to "Approved" with timestamp and approver ID |
| 8 | ATS System | Triggers UC-09 (Auto-Generate Payroll Record) |

**Alternative Flows:**

| Step | Condition | Action |
|---|---|---|
| 6a | Manager rejects the timesheet | Manager enters rejection reason; ATS notifies candidate; candidate corrects and resubmits |

---

## 12. UC-09 — Auto-Generate Payroll Record

| Field | Detail |
|---|---|
| **Use Case ID** | UC-09 |
| **Use Case Name** | Auto-Generate Payroll Record |
| **Primary Actor** | ATS System (fully automated) |
| **Trigger** | Timesheet status is updated to "Approved" in UC-08 |
| **Preconditions** | Approved timesheet record exists; Associated placement record is active |
| **Postconditions** | Payroll record created in Finance Module with all required data; Finance User can review and export |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | ATS System | Detects timesheet approval event |
| 2 | ATS System | Retrieves placement record (Pay Rate, Period) |
| 3 | ATS System | Calculates: Gross Pay = Total Approved Hours × Pay Rate |
| 4 | ATS System | Creates PayrollRecord with: PlacementID, TimesheetID, Period, Gross Pay, Status: "Pending Processing", GeneratedDate |
| 5 | ATS System | Notifies Finance User that a new payroll record is available |
| 6 | Finance User | Reviews and confirms the payroll record |

**Business Rules:**
- This process requires zero manual intervention — it is fully system-driven
- If a payroll record is not generated within 2 hours of timesheet approval, the system flags the anomaly and alerts the Admin
- Payroll records are read-only once confirmed by Finance — any adjustments require a new revision record with an audit trail

---

## 13. UC-10 — View Recruiter Performance Dashboard

| Field | Detail |
|---|---|
| **Use Case ID** | UC-10 |
| **Use Case Name** | View Recruiter Performance Dashboard |
| **Primary Actor** | Manager, Admin |
| **Secondary Actor** | Google Looker Studio, PostgreSQL Database |
| **Trigger** | Manager wants to review recruiter KPIs or prepare a performance report |
| **Preconditions** | User is authenticated with Manager or Admin role; Looker Studio is connected to the database |
| **Postconditions** | Dashboard renders with current data; User can filter by date range, recruiter, job type |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Manager | Navigates to the Reporting module or accesses Looker Studio link |
| 2 | Looker Studio | Connects to PostgreSQL database and loads the latest data |
| 3 | Manager | Selects filters: date range, recruiter name, department, job type |
| 4 | Looker Studio | Renders KPI widgets: submissions, placements, time-to-fill, offer acceptance rate, revenue per recruiter |
| 5 | Manager | Drills into individual recruiter performance or exports the view as a PDF |

---

## 14. UC-11 — Manage Users and Roles

| Field | Detail |
|---|---|
| **Use Case ID** | UC-11 |
| **Use Case Name** | Manage Users and Roles |
| **Primary Actor** | Admin |
| **Trigger** | A new team member joins, a role changes, or a user must be deactivated |
| **Preconditions** | Admin is authenticated |
| **Postconditions** | User account created, updated, or deactivated; Appropriate role assigned; Change logged in audit trail |

**Main Flow:**

| Step | Actor | Action |
|---|---|---|
| 1 | Admin | Navigates to User Management module |
| 2 | Admin | Clicks "Add User" and enters: Name, Email, Department, Role |
| 3 | Admin | Assigns role: Admin / Manager / Recruiter / Finance / Viewer |
| 4 | ATS System | Creates account and sends welcome email with login credentials via MS Graph API |
| 5 | Admin | To deactivate: selects user and clicks "Deactivate" |
| 6 | ATS System | Revokes access; user data retained for audit purposes |

**Business Rules:**
- Only Admin role can create, modify, or deactivate user accounts
- A deactivated user's historical records remain intact and attributed to them
- Role changes take effect on next login

---

*Use cases are written to the level of detail required for UAT test case derivation. Each use case maps directly to one or more test cases in the UAT Plan document.*
