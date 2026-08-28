# ServiceNow ITSM — Enterprise Change Management Lab

**End-to-end Normal Change lifecycle: request → risk assessment → CAB approval → scheduled implementation → validation → closure → operational reporting**

![ServiceNow](https://img.shields.io/badge/Platform-ServiceNow-1F6FEB?logo=servicenow&logoColor=white) ![ITIL 4](https://img.shields.io/badge/Framework-ITIL%204-2EA043?style=flat) ![Change Management](https://img.shields.io/badge/Focus-Change%20Management-8250DF) ![Status](https://img.shields.io/badge/Status-Completed-2EA043)

---

## Overview

This repository documents a hands-on ServiceNow IT Service Management (ITSM) lab in which I managed a production infrastructure change through the complete, governed change lifecycle — from initial request through Change Advisory Board (CAB) approval, scheduled implementation, post-implementation validation, and formal closure. The lab concludes with an operational report built from live incident data.

From a security and governance standpoint, this lab is less about clicking through a ticketing tool and more about demonstrating the control that keeps production environments stable: **nothing changes in production without documented risk analysis, an approval checkpoint, a rollback plan, and an auditable record of what happened.** That control is the backbone of every change management program I'd expect to inherit, audit, or run in an enterprise environment.

**Lab context:** ServiceNow ITSM · Personal Developer Instance (free, no credit card) · aligned to CompTIA A+, Network+, and ITIL 4 Foundation · ~2–3 hours

| | |
|---|---|
| **Scenario** | Upgrade all production Windows servers to Windows Server 2025 |
| **Change type** | Normal Change (CAB required) |
| **Risk / Impact** | Moderate / 1 – High |
| **Outcome** | Closed — Successful, no backout required |

---

## Why This Matters (Security & Governance Perspective)

A production Windows Server upgrade is a legitimate operational need — vendor support, security patching, reliability — but it's also a textbook opportunity for an uncontrolled change to take down a service. This lab exercises the controls that prevent that:

- **Risk-based classification** — the change was scoped as Moderate risk / High impact *before* anything touched production, driving how much scrutiny it received.
- **Segregation of duties** — the person requesting the change is not the sole authority approving it; the CAB reviews scope, risk, and rollback plan independently before authorization.
- **Change freeze / maintenance windows** — implementation is confined to a pre-approved window, limiting blast radius and giving support teams a known point of contact if something goes wrong.
- **Documented rollback** — a tested backout plan exists *before* implementation begins, not improvised after a failure.
- **Independent validation** — post-implementation testing is tracked as a separate task from implementation, so "we made the change" and "we confirmed it worked" aren't the same claim made by the same step.
- **Auditability** — every state transition, approval, and close note is retained in ServiceNow as a permanent record, which is exactly what an auditor or incident responder would pull during a post-mortem or compliance review.

This is the same discipline that underlies change-control requirements in frameworks like ITIL 4, SOC 2, and ISO 27001 — the lab just implements it in a live ITSM platform instead of describing it on paper.

---

## Lab Architecture

The diagram below maps the full change record — governance gate, production target, CAB approval workflow, maintenance window, and both change tasks — to the ServiceNow modules and roles that carry it from creation to closure, alongside the lab's environment, roles, and certification alignment.

<img width="2400" height="2294" alt="architecturediagram" src="https://github.com/user-attachments/assets/da5c0ddb-6a08-4f79-88db-17e8eb058d50" />


**Data flow:** Change Request → Risk / Impact Assessment → CAB Approval → Scheduled Window → Implementation Task → Testing Task → Review → Closed → Operational Reporting

---

## Lab Walkthrough

### 1. Create the Change Request

A Normal Change was created for the production Windows Server 2025 upgrade.

| Field | Value |
|---|---|
| Change Type | Normal |
| Category | Software |
| Short Description | Upgrade all production servers to Windows Server 2025 |
| Impact | 1 – High |
| Risk | Moderate |
| Assignment Group | Change Management |
| Assigned To | Change Manager |

The change description documented the business justification: vendor support, current security updates, system performance, and reliability.

<img width="1906" height="899" alt="01-change-request" src="https://github.com/user-attachments/assets/6c750d23-ee08-4b1f-9840-7a3027d4a5a8" />


### 2. Document Change Planning

The Planning section captured business justification, risk analysis, implementation plan, backout plan, and test plan.

**Business Justification** — Windows Server 2025 provides current vendor support, security enhancements, and improved system reliability, reducing exposure associated with outdated operating systems.

**Risk Analysis** — The change affects production infrastructure; risk was mitigated through an approved maintenance window, controlled per-server rollout, per-server validation, and a documented recovery strategy.

**Backout Plan**
1. Stop further deployment.
2. Restore the affected server using the verified pre-change backup or system snapshot.
3. Confirm services and applications return to their previous working state.
4. Notify the Change Management team.

**Test Plan**
1. Verify the server boots successfully.
2. Confirm network connectivity.
3. Validate critical Windows services.
4. Validate production applications.
5. Confirm users can access required services.
6. Review system and application event logs.
7. Monitor server performance before continuing.

<img width="1896" height="739" alt="02-change-planning" src="https://github.com/user-attachments/assets/79e6ae26-0ccb-4b83-9d01-4d3e8eeac3bf" />


### 3. Schedule the Change

| Schedule Item | Date / Time |
|---|---|
| Planned Start | 2026-08-29 22:00 |
| Planned End | 2026-08-30 02:00 |
| CAB Required | Yes |
| CAB Date / Time | 2026-08-28 14:00 |

A defined maintenance window provides a controlled implementation period and limits disruption to production services.

<img width="1873" height="874" alt="03-change-schedule" src="https://github.com/user-attachments/assets/4cc25c9b-5477-4c16-b194-66ca2ae0c69f" />


### 4. Manage the CAB Approval Workflow

Because the change was configured to require CAB review, it entered the approval process before implementation. This step exercised:

- Change Manager review
- CAB approval requests routed to multiple approvers
- Approval status tracking
- Authorization gated on approval — no scheduling without it

The CAB recommendation covered upgrade scope, implementation plan, business impact, testing procedures, and backout strategy. After approval, ServiceNow progressed the change through Authorization into the Scheduled state.

<img width="997" height="762" alt="04-approval-request" src="https://github.com/user-attachments/assets/827cad94-6a99-4469-b19d-3120d5a727b9" />


<img width="1798" height="619" alt="05-cab-approvals" src="https://github.com/user-attachments/assets/00004972-2414-43c5-a211-7f6c4afecfb1" />

<img width="1906" height="521" alt="06-change-scheduled" src="https://github.com/user-attachments/assets/7d0618ee-d564-4112-acf2-ef348d940cc7" />


### 5. Implement the Change

After approval and scheduling, the change moved into the Implement phase. ServiceNow generated change tasks to separate implementation work from post-implementation validation — the Implementation Task represented execution of the upgrade and was tracked and closed independently.

<img width="1842" height="745" alt="07-implementation-task" src="https://github.com/user-attachments/assets/816fec99-12c9-4116-b213-1d731271372c" />


### 6. Perform Post-Implementation Testing

A separate Testing change task validated the upgraded environment before the parent change progressed to review:

- Server availability
- Network connectivity
- Windows services
- Production application availability
- User access
- Event log review
- Server performance

Both the Implementation and Post-Implementation Testing tasks were closed successfully.

<img width="1823" height="916" alt="08-testing-task" src="https://github.com/user-attachments/assets/21b702a3-07c0-4b36-b65f-eff12d372be2" />


### 7. Review and Close the Change

After implementation and testing, the change progressed to Review. The record captured actual implementation timestamps and confirmed both change tasks were closed. The change was then formally closed with a close code of `Successful`.

> Windows Server 2025 production server upgrade completed successfully. All planned implementation activities were completed and post-implementation testing confirmed server availability, network connectivity, Windows services, and production applications were functioning as expected. No critical issues were identified during validation. The change was completed without requiring the backout plan.

<img width="1920" height="1723" alt="09-change-review" src="https://github.com/user-attachments/assets/9fb016f2-f061-4360-8004-e864585e98c4" />


<img width="1920" height="1563" alt="10-change-closed" src="https://github.com/user-attachments/assets/8dbb7686-df12-4563-b453-2fe1de86f723" />


### 8. Build an Operational Report

A ServiceNow report was created to analyze recent incident activity as a closing exercise in operational reporting.

| Setting | Configuration |
|---|---|
| Report Name | Incident Volume by Priority — Last 30 Days |
| Table | Incident |
| Visualization | Bar chart |
| Filter | Created on Last 30 days |
| Group By | Priority |
| Aggregation | Count |
| Display Data Table | Enabled |

**Results:** 6 incidents in the reporting period, all classified as Priority 4 – Low (100%).

<img width="1886" height="923" alt="11-incident-report" src="https://github.com/user-attachments/assets/4765171a-dc20-4acb-b1d0-7dec8376ccb5" />


The completed report was exported to PDF to demonstrate report distribution and documentation:

```markdown
[View the Incident Volume by Priority Report](reports/incident-volume-by-priority-last-30-days.pdf)
```

---

## Skills Demonstrated

**ServiceNow Administration**
Navigating ITSM records · creating and managing change requests · managing related records and change tasks · working with approval records · building and exporting reports.

**IT Service Management**
Change lifecycle management · CAB governance · risk assessment · business impact analysis · maintenance-window planning · implementation planning · backout planning · post-change validation · change closure.

**Operational Reporting**
Incident data analysis · date-based filtering · priority grouping · count aggregation · bar-chart visualization · report export and stakeholder-ready documentation.

**Certification alignment:** CompTIA A+ · Network+ · ITIL 4 Foundation

---

## Key Takeaways

This lab went beyond opening a single ServiceNow ticket — it required managing a production-change scenario through governance, approval, implementation, testing, review, and closure end to end.

It reinforced the relationship between technical execution and change control: a production change needs clear documentation, defined ownership, risk mitigation, approval checkpoints, validation criteria, and an auditable record of the final outcome. Adding the operational report extended the lab beyond a single change record into visibility across broader IT service activity — the same kind of trend data a security or IT operations team would use to spot systemic issues before they become incidents.

---

## Repository Structure

```text
ServiceNow-ITSM-Change-Management-Lab/
│
├── README.md
│
├── diagrams/
│   └── architecture-diagram.png
│
├── reports/
│   └── incident-volume-by-priority-last-30-days.pdf
│
└── screenshots/
    ├── 01-change-request.png
    ├── 02-change-planning.png
    ├── 03-change-schedule.png
    ├── 04-approval-request.png
    ├── 05-cab-approvals.png
    ├── 06-change-scheduled.png
    ├── 07-implementation-task.png
    ├── 08-testing-task.png
    ├── 09-change-review.png
    ├── 10-change-closed.png
    └── 11-incident-report.png
```

---

## Project Status

**Completed** — the change request progressed successfully from New through Assess, Authorize, Scheduled, Implement, Review, and Closed.
