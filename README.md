# Recruitment Process Automation — emergiTEL Inc.

**Project Type:** Internal Product Development | Business Process Automation  
**Organization:** emergiTEL Inc.  
**Lead Business Analyst:** Ankit Prakash Srivastava  
**External Vendor:** NexGen Solutions, Hyderabad  
**Project Status:** MVP Delivered — Active Development  

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Stakeholders](#stakeholders)
- [Solution Summary](#solution-summary)
- [System Architecture Overview](#system-architecture-overview)
- [Key Integrations](#key-integrations)
- [Repository Structure](#repository-structure)
- [Methodology](#methodology)
- [Challenges & Gap Analysis](#challenges--gap-analysis)
- [MVP Scope](#mvp-scope)
- [Technology Stack](#technology-stack)
- [Project Outcomes](#project-outcomes)
- [Contact](#contact)

---

## Project Overview

emergiTEL Inc. is a Canadian staffing and technology solutions company operating across multiple verticals. As the organization scaled, its recruitment operations grew increasingly dependent on fragmented, manual workflows layered on top of a third-party Applicant Tracking System (BreezyHR) that offered only limited functionality.

This project documents the end-to-end business analysis and vendor-managed development of a **proprietary Applicant Tracking and Recruitment Automation System** — purpose-built to replace BreezyHR, eliminate manual data handling, automate job distribution across multiple job boards, and integrate finance and payroll workflows into a single operational platform.

---

## Problem Statement

Despite subscribing to BreezyHR as an ATS platform with calendar integration, the majority of recruitment operations at emergiTEL remained manual:

- Candidate submission tracking was maintained in separate spreadsheets
- Interview status, offer status, and placement status updates required manual entry
- Finance-related records (timesheets, payroll triggers) were tracked in Excel sheets updated by hand
- Even after a placed candidate's timesheet was approved, payroll teams had to manually update records to initiate bi-weekly payroll processing
- Data was siloed across tools with no unified source of truth
- BreezyHR subscription costs represented a recurring overhead with diminishing return on investment as usage outgrew the platform's native capabilities

The decision was made by emergiTEL's leadership to build a **proprietary ATS** — eliminating vendor dependency, securing candidate and client data internally, and enabling custom automation tailored to the company's exact workflows.

---

## Objectives

1. Replace BreezyHR with a fully owned internal ATS
2. Automate candidate lifecycle tracking from submission through placement
3. Automate bi-weekly payroll triggers upon timesheet approval
4. Enable single-click job posting across multiple job boards via public APIs
5. Integrate Outlook email communication via Microsoft Graph API
6. Provide real-time recruiter performance visibility via Looker Studio dashboards
7. Reduce manual data entry and associated error rates
8. Secure proprietary recruitment and client data within emergiTEL's own infrastructure

---

## Stakeholders

| Name | Role | Organization | Responsibility |
|---|---|---|---|
| Aneela Zaib | Company Owner | emergiTEL Inc. | Executive Sponsor — Final approvals, strategic direction |
| Akbar Khan | Managing Director | emergiTEL Inc. | Project Champion — Operational oversight, stakeholder alignment |
| Sonika Salhotra | Recruitment SME | emergiTEL Inc. | Subject Matter Expert — Process validation, UAT lead |
| Rehan Qazi | Data Analyst | emergiTEL Inc. | Database design, Looker Studio KPI dashboard setup |
| Ankit Prakash Srivastava | Lead Business Analyst | emergiTEL Inc. | Requirements elicitation, BRD, process design, UAT coordination |
| NexGen Solutions | External Development Vendor | Hyderabad, India | System architecture, development, integration, delivery |

---

## Solution Summary

The system delivers a unified recruitment operations platform with the following core modules:

- **Candidate Management** — Full applicant lifecycle tracking from sourcing to placement
- **Job Requisition Management** — Create, manage, and post job openings centrally
- **Multi-Board Job Distribution** — One-click publishing to seven job boards via public APIs
- **Interview & Offer Pipeline** — Automated status progression with Outlook calendar and email integration
- **Placement & Onboarding Tracker** — Placement confirmation triggers downstream finance workflows
- **Timesheet & Payroll Automation** — Approved timesheets auto-trigger bi-weekly payroll records
- **Finance Module** — Replaces manual finance sheets with structured, auditable data
- **Recruiter Performance Dashboard** — Looker Studio dashboards connected to a structured database maintained by the Data Analyst

---

## System Architecture Overview

```
┌────────────────────────────────────────────────────────────────────────┐
│                        emergiTEL ATS Platform                          │
│                                                                        │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────────────────┐   │
│  │  Job        │   │  Candidate   │   │  Interview / Offer /     │   │
│  │  Requisition│──▶│  Pipeline    │──▶│  Placement Tracker       │   │
│  └─────────────┘   └──────────────┘   └──────────┬───────────────┘   │
│                                                   │                    │
│                                          ┌────────▼──────────┐        │
│                                          │  Finance & Payroll │        │
│                                          │  Automation Module │        │
│                                          └────────┬──────────┘        │
│                                                   │                    │
│  ┌──────────────────────────────────────┐  ┌─────▼──────────────┐    │
│  │  Job Board Distribution Engine       │  │  Reporting Layer    │    │
│  │  (7 Boards via Public API)           │  │  Looker Studio      │    │
│  └──────────────────────────────────────┘  └────────────────────┘    │
└────────────────────────────────────────────────────────────────────────┘
         │                                    │
         ▼                                    ▼
  Outlook Email API              PostgreSQL / Cloud Database
  (MS Graph API)                 (Managed by Rehan Qazi)
```

---

## Key Integrations

### Job Board APIs (Public)

| Job Board | API Type | Purpose |
|---|---|---|
| Indeed Canada | Public REST API | Job posting distribution |
| Glassdoor | Public REST API | Job posting distribution |
| Wellfound (AngelList) | Public REST API | Job posting distribution |
| ZipRecruiter | Public REST API | Job posting distribution |
| PostJobFree | Public REST API | Job posting distribution |
| Hubstaff Talent | Public REST API | Job posting distribution |
| emergiTEL Website | Internal CMS API | Synchronized job listing |

### Communication Integration

| Tool | API | Purpose |
|---|---|---|
| Microsoft Outlook | Microsoft Graph API | Email notifications, interview scheduling, offer communications |

### Reporting & Analytics

| Tool | Purpose |
|---|---|
| Google Looker Studio | Real-time recruiter performance dashboards |
| PostgreSQL (Cloud) | Structured data storage — candidates, jobs, timesheets, placements |

---

## Repository Structure

```
Recruitment-Process-Automation/
│
├── README.md                          # Project overview (this file)
│
├── BRD/
│   └── Business-Requirements-Document.md    # Full BRD with functional & non-functional requirements
│
├── Process-Diagrams/
│   └── Process-Flow-Diagrams.md             # AS-IS and TO-BE process flows, swimlane diagrams
│
├── Use-Cases/
│   └── Use-Case-Specifications.md           # Detailed use cases with actors, flows, exceptions
│
├── KPI-Logic/
│   └── KPI-Metrics-Logic.md                 # KPI definitions, formulas, dashboard logic
│
└── UAT/
    └── UAT-Plan-and-Test-Cases.md            # UAT strategy, test cases, sign-off log
```

---

## Methodology

This project followed a hybrid **Agile-Waterfall** approach:

- **Discovery Phase** — Stakeholder interviews, AS-IS process mapping, gap analysis
- **Requirements Phase** — BRD authoring, use case documentation, stakeholder sign-off
- **Design Phase** — TO-BE process design, data flow design, integration architecture review
- **Development Phase** — Vendor-managed (NexGen Solutions) with BA oversight via sprint reviews
- **UAT Phase** — Coordinated by Lead BA, executed by Recruitment SME and recruiter team
- **Deployment Phase** — Phased rollout beginning with MVP core modules
- **Monitoring Phase** — KPI dashboard go-live, recruiter adoption tracking

---

## Challenges & Gap Analysis

### Key Challenges Identified During Discovery

**1. No Single Source of Truth**  
Candidate data was distributed across BreezyHR, Excel sheets, and individual recruiter email inboxes. There was no authoritative system of record. During stakeholder interviews, recruiters reported regularly working from outdated spreadsheet versions.

**2. Manual Status Propagation**  
Every status change — from interview scheduled to offer extended to placement confirmed — required a recruiter to manually update two to three separate files. This introduced latency and inconsistency in reporting.

**3. Finance and Recruitment Disconnect**  
The finance team had no live visibility into placement or timesheet status. Payroll processing was triggered manually, creating delays and occasional missed payroll cycles when communication broke down between recruitment and finance.

**4. BreezyHR Limitations**  
BreezyHR's API exposed limited data for downstream automation. Custom workflow logic, finance integration, and multi-board job distribution were not feasible within the platform's constraints.

**5. Job Posting Fragmentation**  
Recruiters manually logged into each job board individually to post openings. With seven active boards, a single job posting required significant repetitive effort and introduced inconsistency in job descriptions across platforms.

**6. Data Security Concern**  
As emergiTEL's client base grew, leadership identified risk in storing sensitive candidate and client data within a third-party SaaS platform over which they had no data governance control.

**7. Email Communication Not Tracked in ATS**  
All candidate and client communications via Outlook were completely disconnected from BreezyHR. There was no audit trail of what was communicated, when, and by whom.

### Gap Analysis Summary

| Area | AS-IS State | Gap | TO-BE State |
|---|---|---|---|
| Candidate Tracking | Manual spreadsheets | No automation, high error rate | Automated pipeline in ATS |
| Interview Scheduling | Manual calendar entries | Not linked to candidate record | Outlook API integrated scheduling |
| Finance / Payroll | Manual sheet updates | Delay, errors, missed cycles | Auto-trigger on timesheet approval |
| Job Distribution | Manual per-board login | Time-consuming, inconsistent | Single-click multi-board posting |
| Reporting | Ad hoc Excel summaries | No real-time data | Live Looker Studio dashboard |
| Email Tracking | Outlook inbox only | No ATS visibility | MS Graph API integration |
| Data Security | BreezyHR cloud | Third-party dependency | Proprietary hosted database |

---

## MVP Scope

The MVP was scoped to deliver the highest-impact modules first to demonstrate value and enable parallel migration from BreezyHR:

**In MVP:**
- Candidate pipeline management (Submission → Interview → Offer → Placement)
- Job requisition creation and management
- Multi-board job distribution (all 7 boards)
- Outlook email integration via MS Graph API
- Basic finance module (placement and timesheet tracking)
- Payroll trigger automation on timesheet approval
- Looker Studio dashboard (core recruiter KPIs)

**Post-MVP Roadmap:**
- Advanced analytics and forecasting
- Client portal for job order submission
- Mobile-responsive recruiter interface
- Document management (contracts, offer letters)
- Automated reference check workflows

---

## Technology Stack

| Layer | Technology |
|---|---|
| Backend | To be confirmed with NexGen Solutions |
| Database | PostgreSQL (Cloud-hosted) |
| Email Integration | Microsoft Graph API (Outlook) |
| Job Board Integration | Public REST APIs (per board) |
| Reporting | Google Looker Studio |
| Version Control | GitHub |
| Documentation | Markdown (this repository) |

---

## Project Outcomes

- Eliminated dependency on BreezyHR — full subscription cost saved from go-live
- Reduced manual data entry across recruitment and finance operations
- Unified candidate lifecycle data into a single platform
- Finance team gained live visibility into placements and timesheet approvals
- Payroll processing time reduced through automated trigger workflow
- Recruiters able to post to 7 job boards in a single action
- Recruiter performance became measurable and visible in real time via dashboard
- Data security risk addressed through proprietary data storage

---

## Contact

**Ankit Prakash Srivastava**  
Lead Business Analyst — emergiTEL Inc.  
Project documentation maintained in this repository.

---

*This repository serves as the official project documentation archive for the emergiTEL Recruitment Process Automation initiative.*
