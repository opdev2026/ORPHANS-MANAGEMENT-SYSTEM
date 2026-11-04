# Baitulmaal Orphan Management System — Overview

**Document date:** 11.04.2026  
**Project:** Baitulmaal Orphan Management System (OMS) — PMO Platform Module  
**Source documents:** BRD v1.1, Workflow & Permission Matrix v1.1

---

## 1. Introduction

Baitulmaal manages orphan sponsorship and general support activities across multiple countries, involving central management, country offices, field offices, and partner organizations. Today these activities — intake forms, document verification, sponsor matching, renewals, disbursement, quarterly expenditure reporting, and annual progress reporting — are coordinated across separate tools and manual handovers.

The Orphan Management System brings these activities into one integrated module of the Baitulmaal PMO Platform.

The platform provides:

- Partner compliance review before partners may register orphans.
- Orphan intake through forms and bulk Excel import.
- Verification and configurable approval of every orphan record.
- A permanent Orphan Profile with a unique Orphan ID.
- Automated sponsor matching with audited manual override.
- Sponsorship renewal and termination monitoring, coordinated with Donor Care.
- General support activities with beneficiary targets.
- Fund allocation, quarterly disbursement, and proof of delivery.
- Two-stage quarterly financial report approval.
- Mandatory annual progress reports, privacy-reviewed and emailed to sponsors.
- Event-driven notifications and a full audit trail.

The system is designed around a simple principle: **every orphan has one verified record, and every action on that record is traceable.**

---

## 2. Goals

- Maintain a single, comprehensive digital profile for every orphan.
- Standardize registration, verification, sponsorship, financial reporting, and field follow-up across countries and partners.
- Automate sponsor matching on configurable eligibility and priority rules.
- Provide full traceability from funding source to delivered amount.
- Enforce quarterly financial reporting and annual progress reporting.
- Support Donor Care with accurate sponsorship information and alerts while keeping donor relationships under Donor Care.
- Protect beneficiary privacy in every external communication.
- Provide a scalable foundation for future programs such as Student Sponsorship.

---

## 3. Stakeholders

| Stakeholder | Role | Main responsibilities |
|---|---|---|
| **Central Management** | Oversight | Consolidated performance, statistics, critical issues |
| **Orphan Program Manager** | Program owner | Approvals (records, activities, quarterly reports, progress reports), escalations, matching policy |
| **Orphan File Reviewer** | Verifier | Review intake data and documents, return or approve/forward records |
| **Country Office Manager / Staff** | Country operations | Orphan intake, partner monitoring, coordination with field offices |
| **Partner User** | Data submitter | Intake forms, documents, bulk import, corrections |
| **Field Office Manager** | Field operations & delivery | Receive funds, deliver to families, proof of delivery, quarterly reports |
| **Field Officer** | Beneficiary follow-up | Visits, progress data, photos and videos |
| **Sponsorship Coordinator** | Sponsorship operations | Matching review, manual override, expiry monitoring, status updates |
| **Donor Care User** | Donor relationships | Renewal communication, cancellations, donor inquiries (no orphan file access) |
| **Central Finance Reviewer** | Financial control | Allocation, transfer, second-stage report approval |
| **Compliance Officer** | Partner compliance | Review partner agreements, approve/reject, flag expiry |
| **System Administrator** | Platform administration | Users, roles, structures, workflow and notification configuration |
| **Donor / Sponsor** | External | Funds sponsorship, receives approved reports by email |

---

## 4. High-Level Flow

1. A partner submits agreements; Compliance approves the partner.
2. An approved partner or country office submits an orphan intake (form or bulk import).
3. The Orphan File Reviewer verifies data and documents, returning records for correction where needed.
4. The record is approved directly by the reviewer (if permitted) or forwarded to the Program Manager.
5. The system generates the Orphan ID and Orphan Profile; the orphan enters the matching pool.
6. When a sponsor is available, the matching engine assigns a sponsor; the coordinator may override.
7. Finance allocates and transfers funds; the Field Office Manager delivers them and uploads proof.
8. Each quarter, the field office submits an expenditure report: Program Manager approves, then Central Finance.
9. Each July, field teams collect annual progress; the report is approved, privacy-reviewed, and emailed to the sponsor.
10. Before expiry, OMS alerts Donor Care; Donor Care contacts the sponsor. Renewal is automatic unless cancelled.
11. On termination, the orphan's eligibility is reviewed and the orphan returns to the matching pool.

---

## 5. Strengths and Limitations

**Strengths:**

- One verified record per orphan.
- Configurable approval authority without workflow redesign.
- Automated, auditable matching.
- Clear separation between Orphan Management and Donor Care.
- Financial traceability from allocation to delivery.
- Privacy review before any external sharing.
- Event-driven notifications for every hand-over.

**Limitations:**

- OMS does not replace Donor Care's donor relationship management or existing ERP/CRM.
- OMS records proof of delivery; it does not independently confirm the physical receipt of funds.
- Several business rules (matching weights, deadlines, age transition at 18) are pending confirmation — see `Ref_documentations/Risk-assumption.md`.

---

## 6. Initial Scope Boundary

In scope: the 12 business areas in BRD §3.1 — Orphan Registry, Intake & Verification, Sponsorship Management, Sponsor Matching, General Support, Financial Management, Field Operations, Progress Reporting, Donor Communication Support, Partner Compliance, Notifications, Reports & Analytics.

Out of scope for the initial release:

- Student Sponsorship Management (future expansion; the model must support it).
- Additional beneficiary categories and program types.
- Donor self-service portal.
- Donor Care's full donor relationship management.

*Project: Baitulmaal OMS | Version 1.0 | 11.04.2026*
