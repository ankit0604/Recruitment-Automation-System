# Process Flow Diagrams
## Recruitment Process Automation — emergiTEL Inc.

---

**Document Version:** 1.0  
**Prepared By:** Ankit Prakash Srivastava — Lead Business Analyst  
**Reviewed By:** Sonika Salhotra — Recruitment SME  
**Date:** 2025

> Note: All diagrams below are rendered in Mermaid-compatible syntax. GitHub renders Mermaid natively. Use a Mermaid viewer or the GitHub preview to see rendered diagrams.

---

## Table of Contents

1. [Process Index](#1-process-index)
2. [AS-IS: Overall Recruitment Workflow](#2-as-is-overall-recruitment-workflow)
3. [TO-BE: Overall Recruitment Workflow](#3-to-be-overall-recruitment-workflow)
4. [Process 01 — Job Requisition & Multi-Board Posting](#4-process-01--job-requisition--multi-board-posting)
5. [Process 02 — Candidate Submission & Pipeline Management](#5-process-02--candidate-submission--pipeline-management)
6. [Process 03 — Interview Scheduling (Outlook Integration)](#6-process-03--interview-scheduling-outlook-integration)
7. [Process 04 — Offer Management](#7-process-04--offer-management)
8. [Process 05 — Placement & Finance Workflow](#8-process-05--placement--finance-workflow)
9. [Process 06 — Timesheet Approval & Payroll Automation](#9-process-06--timesheet-approval--payroll-automation)
10. [Swimlane: End-to-End Recruitment Lifecycle](#10-swimlane-end-to-end-recruitment-lifecycle)
11. [Process Change Summary](#11-process-change-summary)

---

## 1. Process Index

| Process ID | Process Name | AS-IS Documented | TO-BE Documented |
|---|---|---|---|
| PROC-01 | Job Requisition & Multi-Board Posting | Yes | Yes |
| PROC-02 | Candidate Submission & Pipeline Management | Yes | Yes |
| PROC-03 | Interview Scheduling | Yes | Yes |
| PROC-04 | Offer Management | Yes | Yes |
| PROC-05 | Placement & Finance Workflow | Yes | Yes |
| PROC-06 | Timesheet Approval & Payroll Automation | Yes | Yes |

---

## 2. AS-IS: Overall Recruitment Workflow

The following diagram captures the high-level AS-IS process — how recruitment operations functioned before this system was built.

```mermaid
flowchart TD
    A([Client Sends Job Order]) --> B[Recruiter Creates Job in BreezyHR]
    B --> C[Recruiter Manually Logs into Each Job Board]
    C --> D[Copies Job Description to Each Platform]
    D --> E[Waits for Applications]

    E --> F[Applications Come In via Email / Job Board]
    F --> G[Recruiter Manually Adds Candidate to Excel Sheet]
    G --> H[Recruiter Submits Candidate to Client via Email]

    H --> I{Client Response?}
    I -- Rejected --> J[Manually Update Status in Excel]
    I -- Interview --> K[Recruiter Schedules Interview Manually in Outlook]

    K --> L[Sends Calendar Invite Manually]
    L --> M{Interview Outcome?}
    M -- No Hire --> N[Manually Update Excel — Move to Rejected]
    M -- Hire --> O[Recruiter Extends Offer via Email]

    O --> P{Candidate Response?}
    P -- Declined --> Q[Update Excel — Offer Declined]
    P -- Accepted --> R[Recruiter Notifies Finance Team via Email]

    R --> S[Finance Manually Enters Placement in Spreadsheet]
    S --> T[Candidate Begins Work — Submits Paper/Email Timesheet]
    T --> U[Manager Approves via Email]
    U --> V[Finance Manually Updates Payroll Sheet]
    V --> W([Bi-Weekly Payroll Processed])

    style A fill:#f0f0f0
    style W fill:#f0f0f0
    style J fill:#ffe0e0
    style N fill:#ffe0e0
    style Q fill:#ffe0e0
    style V fill:#ffe0e0
```

**AS-IS Pain Points Highlighted:**
- Manual job posting to each board independently
- No candidate record in a shared system — Excel only
- No link between Outlook calendar events and candidate records
- Finance notification dependent on email communication
- Payroll triggered manually from spreadsheet update

---

## 3. TO-BE: Overall Recruitment Workflow

The following diagram captures the target state with the new system in place.

```mermaid
flowchart TD
    A([Client Sends Job Order]) --> B[Recruiter Creates Job Requisition in ATS]
    B --> C[Selects Target Job Boards]
    C --> D[Single-Click Publish via API]
    D --> E{Posting Status per Board}
    E -- Success --> F[Job Live on Selected Boards]
    E -- Failed --> G[System Notifies Recruiter with Error Detail]

    F --> H[Applications Received — Auto-Captured or Manually Added]
    H --> I[Candidate Record Created in ATS]
    I --> J[Recruiter Submits Candidate to Client within ATS]

    J --> K{Client Response}
    K -- Rejected --> L[Status Auto-Updated: Rejected — Timestamped]
    K -- Interview --> M[Recruiter Schedules Interview in ATS]

    M --> N[MS Graph API Sends Calendar Invite to Candidate + Interviewer]
    N --> O{Interview Outcome Recorded in ATS}
    O -- No Hire --> P[Status Auto-Updated: Not Selected]
    O -- Proceed --> Q[Recruiter Creates Offer Record in ATS]

    Q --> R[Offer Email Sent via Outlook through MS Graph API]
    R --> S{Offer Status}
    S -- Declined --> T[Status Updated: Offer Declined]
    S -- Accepted --> U[Placement Record Auto-Created — Finance Notified]

    U --> V[Candidate Submits Timesheet in ATS]
    V --> W[Approver Notified — Reviews and Approves in ATS]
    W --> X[Payroll Record Auto-Generated — No Manual Step]
    X --> Y([Bi-Weekly Payroll Processed])

    style A fill:#f0f0f0
    style Y fill:#d4edda
    style D fill:#d0e8ff
    style N fill:#d0e8ff
    style R fill:#d0e8ff
    style X fill:#d4edda
```

---

## 4. Process 01 — Job Requisition & Multi-Board Posting

### AS-IS

```mermaid
flowchart LR
    A([Client Request Received]) --> B[Recruiter Creates Job in BreezyHR]
    B --> C[Log into Indeed Canada]
    C --> D[Paste Job Description]
    D --> E[Log into Glassdoor]
    E --> F[Paste Job Description]
    F --> G[Log into Wellfound]
    G --> H[Paste Job Description]
    H --> I[Repeat for ZipRecruiter, PostJobFree, Hubstaff, Website]
    I --> J([7 Separate Manual Postings Complete])
```

**Estimated Time:** 45–60 minutes per job posting  
**Risk:** Inconsistent job descriptions across boards, human error

### TO-BE

```mermaid
flowchart LR
    A([Client Request Received]) --> B[Recruiter Creates Job Requisition in ATS]
    B --> C[Fills Single Job Description Form]
    C --> D[Selects Target Boards via Checkboxes]
    D --> E[Clicks Publish]
    E --> F[ATS Calls APIs for Each Selected Board Simultaneously]
    F --> G{Posting Result}
    G -- All Success --> H([Job Live on All Boards — Confirmation Shown])
    G -- Partial Failure --> I[Success Boards Confirmed — Failed Boards Flagged with Error]
    I --> J[Recruiter Retries Failed Board]
```

**Estimated Time:** 3–5 minutes per job posting  
**Improvement:** 90% reduction in manual effort for job distribution

---

## 5. Process 02 — Candidate Submission & Pipeline Management

### AS-IS

```mermaid
flowchart TD
    A([Candidate Applies / Recruiter Finds Candidate]) --> B[Recruiter Opens Excel Sheet]
    B --> C[Manually Adds Candidate Row]
    C --> D[Emails Resume to Client]
    D --> E{Client Response via Email}
    E -- No Response --> F[Recruiter Follows Up Manually]
    E -- Rejected --> G[Manually Updates Excel: Rejected]
    E -- Move Forward --> H[Manually Updates Excel: Interview Scheduled]
```

### TO-BE

```mermaid
flowchart TD
    A([Candidate Applies / Recruiter Finds Candidate]) --> B[Candidate Record Created in ATS]
    B --> C[Resume Attached — Source Logged — Recruiter Assigned]
    C --> D[Recruiter Submits Candidate to Client — Action Logged in ATS]
    D --> E{Client Response Recorded in ATS}
    E -- Rejected --> F[Status Auto-Updated: Rejected — Timestamp + User Logged]
    E -- Move Forward --> G[Status Updated: Interview Scheduled — Pipeline Advances]
    G --> H[Interview Record Created — Outlook Invite Triggered]
```

---

## 6. Process 03 — Interview Scheduling (Outlook Integration)

### AS-IS

```mermaid
sequenceDiagram
    participant R as Recruiter
    participant O as Outlook (Manual)
    participant C as Candidate
    participant I as Interviewer

    R->>O: Opens Outlook Calendar manually
    R->>O: Creates calendar event
    R->>C: Manually sends email invite to candidate
    R->>I: Manually sends email invite to interviewer
    Note over R: No link to candidate record in BreezyHR
    R->>R: Manually notes interview in Excel sheet
```

### TO-BE

```mermaid
sequenceDiagram
    participant R as Recruiter (ATS)
    participant ATS as ATS System
    participant GRAPH as MS Graph API
    participant C as Candidate
    participant I as Interviewer

    R->>ATS: Opens candidate record — clicks Schedule Interview
    ATS->>ATS: Creates interview record linked to candidate
    ATS->>GRAPH: Sends calendar event creation request
    GRAPH->>C: Calendar invite delivered to candidate
    GRAPH->>I: Calendar invite delivered to interviewer
    ATS->>R: Confirmation shown — interview record saved
    Note over ATS: Reminder auto-sent 24 hrs before via Graph API
```

---

## 7. Process 04 — Offer Management

### AS-IS

```mermaid
flowchart TD
    A([Decision to Extend Offer]) --> B[Recruiter Drafts Offer Manually via Outlook Email]
    B --> C[Sends Offer to Candidate]
    C --> D{Candidate Response via Email}
    D -- Accepted --> E[Recruiter Emails Finance Team with Placement Details]
    D -- Declined --> F[Manually Update Excel: Offer Declined]
    E --> G[Finance Manually Adds Placement to Their Sheet]
```

### TO-BE

```mermaid
flowchart TD
    A([Decision to Extend Offer]) --> B[Recruiter Creates Offer Record in ATS]
    B --> C[ATS Sends Offer Communication via MS Graph API / Outlook]
    C --> D{Offer Status Updated in ATS}
    D -- Accepted --> E[Placement Record Auto-Created in Finance Module]
    D -- Declined --> F[Status Updated: Offer Declined — Candidate Remains in Pipeline for Re-Engagement]
    E --> G[Finance Team Notified Automatically — No Email Required]
```

---

## 8. Process 05 — Placement & Finance Workflow

### AS-IS

```mermaid
flowchart TD
    A([Offer Accepted]) --> B[Recruiter Emails Finance with Start Date, Rate, Client]
    B --> C[Finance Receives Email]
    C --> D[Finance Opens Placement Excel Sheet]
    D --> E[Manually Enters Placement Record]
    E --> F[Manually Enters Bill Rate and Pay Rate]
    F --> G([Placement Record Exists in Spreadsheet])
    G --> H[No Real-Time Visibility for Management]
```

### TO-BE

```mermaid
flowchart TD
    A([Offer Accepted in ATS]) --> B[Placement Record Auto-Generated]
    B --> C[Record Contains: Candidate, Client, Job, Start Date, Bill Rate, Pay Rate, Recruiter]
    C --> D[Finance Module Immediately Shows New Placement]
    D --> E[Finance User Reviews and Confirms Record]
    E --> F[Timesheet Module Activated for Placed Candidate]
    F --> G([Placement Visible in Looker Studio Dashboard in Real Time])
```

---

## 9. Process 06 — Timesheet Approval & Payroll Automation

### AS-IS

```mermaid
flowchart TD
    A([Placed Candidate Completes Work Week]) --> B[Candidate Emails Timesheet to Manager]
    B --> C{Manager Reviews via Email}
    C -- Rejected --> D[Candidate Corrects and Resends Email]
    C -- Approved --> E[Manager Replies Approved via Email]
    E --> F[Finance Team Receives Approval Email]
    F --> G[Finance Opens Payroll Spreadsheet Manually]
    G --> H[Manually Enters Approved Hours]
    H --> I[Manually Calculates Gross Pay]
    I --> J([Payroll Ready for Processing])
    J --> K[Risk: Missed Entry = Missed Payroll Cycle]
```

### TO-BE

```mermaid
flowchart TD
    A([Placed Candidate Completes Work Week]) --> B[Candidate Submits Timesheet in ATS]
    B --> C[Approver Receives Notification in ATS]
    C --> D{Approver Reviews in ATS}
    D -- Rejected with Comments --> E[Candidate Notified — Resubmits Corrected Timesheet]
    D -- Approved --> F[System Automatically Updates Payroll Module]
    F --> G[Payroll Record Generated with Period, Hours, Gross Pay]
    G --> H[Finance User Sees Record in Finance Dashboard]
    H --> I([Bi-Weekly Payroll Runs Without Manual Data Entry])
    I --> J[Looker Studio Reflects Approved Hours in Real Time]
```

**Key Automation Win:** Approval of a timesheet in the ATS is the single trigger that initiates the payroll record — zero manual steps required.

---

## 10. Swimlane: End-to-End Recruitment Lifecycle

The following swimlane maps the complete TO-BE process across all actors.

```mermaid
flowchart TD
    subgraph Client
        CL1([Submit Job Order])
        CL2([Review Candidate Submissions])
        CL3([Confirm Interview])
        CL4([Make Offer Decision])
    end

    subgraph Recruiter
        R1[Create Job Requisition in ATS]
        R2[Publish to Job Boards via API]
        R3[Source & Add Candidates]
        R4[Submit Candidate to Client]
        R5[Schedule Interview in ATS]
        R6[Record Interview Outcome]
        R7[Create Offer in ATS]
        R8[Record Acceptance]
    end

    subgraph ATS_System [ATS System]
        S1[Candidate Record Created]
        S2[Status Updates — Timestamped]
        S3[Outlook Calendar Invite Triggered]
        S4[Placement Record Auto-Created]
        S5[Payroll Record Auto-Generated]
    end

    subgraph Finance
        F1[Review Placement Record]
        F2[Approve Timesheet]
        F3[Monitor Payroll Dashboard]
    end

    subgraph Candidate
        CD1[Applies / Contacted]
        CD2[Attends Interview]
        CD3[Receives Offer]
        CD4[Submits Timesheet]
    end

    CL1 --> R1 --> R2 --> CD1 --> R3 --> S1
    S1 --> R4 --> CL2 --> R5 --> S3 --> CD2
    CD2 --> R6 --> S2 --> CL4 --> R7 --> CD3
    CD3 --> R8 --> S4 --> F1
    F1 --> CD4 --> F2 --> S5 --> F3
```

---

## 11. Process Change Summary

| Process | AS-IS Steps | TO-BE Steps | Manual Steps Eliminated | Key Automation |
|---|---|---|---|---|
| Job Posting | 14+ (log in per board, paste per board) | 4 (create, select, publish, confirm) | 10+ | API-based multi-board distribution |
| Candidate Tracking | 5+ (open Excel, add row, update per event) | 2 (record created, status updated in ATS) | 3+ | ATS pipeline with auto-timestamping |
| Interview Scheduling | 6 (open Outlook, create event, email candidate, email interviewer, note in Excel) | 2 (click schedule, confirm) | 4 | MS Graph API calendar and email |
| Offer Management | 5 (draft email, send, receive response, email finance, update sheet) | 3 (create offer, send via ATS, record response) | 2 | Auto-placement creation on acceptance |
| Placement Record | 4 (receive email, open sheet, enter record, notify) | 0 (fully automated) | 4 | Placement auto-created from offer acceptance |
| Payroll Processing | 5 (receive approval email, open sheet, enter hours, calculate, save) | 0 (fully automated) | 5 | Payroll record auto-generated on timesheet approval |

---

*All process diagrams reflect requirements documented in the BRD and validated with Sonika Salhotra (Recruitment SME) during stakeholder review sessions.*
