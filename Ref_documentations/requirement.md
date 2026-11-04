# OMS — Product Requirements Document (PRD)

**Classification:** Confidential — For Recipients and Authorized Parties Only  
**Version:** 1.0  
**Date:** 11.04.2026  
**Status:** Issued for Review

## 1. Product Vision

Establish a unified, automated, and transparent orphan management environment that lets Baitulmaal manage beneficiary information, sponsorship operations, financial activity, and program impact through a single integrated system — with clear responsibility separation between Orphan Management, Donor Care, Finance, Field Offices, and Partners.

## 2. Target Users

| User | Primary need |
|---|---|
| Central Management | Consolidated performance and statistics |
| Orphan Program Manager | Approve records, activities, reports; manage escalations |
| Orphan File Reviewer | Verify intake data and documents |
| Country Office Manager / Staff | Register orphans, coordinate country operations |
| Partner User | Submit orphan data and documents |
| Field Office Manager | Deliver funds, submit quarterly reports |
| Field Officer | Record visits and annual progress |
| Sponsorship Coordinator | Supervise matching, renewals, terminations |
| Donor Care User | Receive expiry/renewal information; report donor decisions |
| Central Finance Reviewer | Allocate, transfer, and approve financials |
| Compliance Officer | Approve partners and monitor agreements |
| System Administrator | Configure users, roles, structures, and rules |

## 3. Functional Requirements

### FR-01 Partner Compliance
The system shall allow partners to submit agreements and compliance documents, and allow Compliance Officers to approve or reject them with comments. Only approved partners may submit orphan intake. Expired agreements shall trigger notifications.

### FR-02 Orphan Intake Form
Authorized country office staff and approved partners shall create intake records containing personal information, family information, vulnerability assessment, and supporting documents (BRD §6.2). Submitting an intake does not create an approved orphan record.

### FR-03 Bulk Import
The system shall support bulk import through an approved Excel template, with field mapping, validation, duplicate detection, missing-information identification, error reporting, and review before submission. Bulk import shall not bypass verification.

### FR-04 Verification
Every intake shall enter a review queue. The reviewer shall check personal information, family information, and documents, and may return the record for correction. Returned records retain their submission history.

### FR-05 Configurable Approval
After verification the system shall check the reviewer's approval permission: a reviewer with direct authority approves the record (Scenario A); otherwise the record is forwarded to the Program Manager (Scenario B). Both scenarios shall work without a workflow change.

### FR-06 Orphan Profile and ID
On approval, the system shall generate a unique, immutable Orphan ID and a profile that references personal and family information, documents, verification history, sponsorships, support participation, disbursements, field visits, progress reports, and status history.

### FR-07 Duplicate Detection
The system shall detect probable duplicate orphans on intake and bulk import (name, date of birth, guardian, location, document numbers) and require a reviewer decision.

### FR-08 Sponsor Matching
The matching engine shall run only when an available sponsor exists, select from verified and eligible orphans, apply configured priorities (younger orphans first; orphans whose sponsorship ended first), and create the assignment. Unmatched eligible orphans remain on the waiting list.

### FR-09 Manual Override
Authorized users shall be able to create, change, or reassign matches. Every change shall record previous assignment, new assignment, sponsor reference, reason, user, and timestamp. No additional Program Manager approval is required by default.

### FR-10 Sponsorship Records
Each sponsorship shall link one orphan to one sponsor, with type (monthly/annual), start and end dates, payment frequency, amount, currency, and status. An orphan shall not have conflicting active sponsorships.

### FR-11 Renewal Monitoring
The system shall identify sponsorships approaching expiry (configurable threshold), alert Orphan Management, and make the renewal list available to Donor Care. Renewal is the default outcome unless Donor Care records a cancellation.

### FR-12 Termination and Re-Matching
On a confirmed cancellation or discontinuation from Donor Care, the system shall update the sponsorship status, notify responsible staff, trigger eligibility review, and return eligible orphans to the matching pool. History shall never be deleted.

### FR-13 General Support Activities
Program staff shall create activities with objectives, country/office, target beneficiaries, and eligibility rules. The Program Manager approves activities. Field offices select beneficiaries from the registry, record actual beneficiaries, and upload evidence. Country-specific participation limits shall be enforced.

### FR-14 Funding Sources and Allocation
The system shall distinguish individual sponsorship donations, general donations, restricted donations, and other contributions, and shall trace each allocation to its source. Sponsorship and general support funds shall not be mixed without traceability.

### FR-15 Disbursement Tracking
The system shall track Allocated, Transferred, Delivered, Pending, and Reconciled amounts per orphan and period. Field Office Managers shall upload proof of delivery (receipts, transfer confirmations, signed acknowledgments). Approved disbursements appear in the Orphan Profile.

### FR-16 Quarterly Financial Report
Field offices shall submit a quarterly expenditure report. The Program Manager approves first; Central Finance approves second. Rejected or returned reports require comments and keep their history. A submitter cannot approve their own report.

### FR-17 Field Visits
Field officers shall record visits with date, notes, photos, videos, documents, and next follow-up date. Overdue follow-ups shall trigger notifications.

### FR-18 Annual Progress Report
Each July the system shall generate a reporting list of all sponsored orphans, support assignment to field teams, collect educational, health, and social progress with media, and route the report for review and Program Manager approval. Year-over-year comparison shall show improved, declined, unchanged, and missing indicators.

### FR-19 Privacy Review and Email Delivery
Before any report is sent to a sponsor, an authorized reviewer shall complete a privacy review. Restricted data (national IDs, detailed addresses, contact information, identity documents, detailed medical records) shall be excluded. The system shall send the approved report by email and record delivery status.

### FR-20 Donor Care Coordination
OMS shall share expiry lists and renewal follow-up requirements with Donor Care and receive confirmed donor decisions, without giving Donor Care access to orphan profiles or case data.

### FR-21 Notifications
The system shall generate role-targeted in-app and email notifications for the events in `Workflow-Events.md`, with configurable rules and thresholds.

### FR-22 Audit
The system shall log every critical action (user, role, action, record, previous and new values, timestamp, reason, decision, comments). Audit history is not editable by operational users.

### FR-23 Reports & Analytics
The system shall provide orphan statistics, sponsorship, general support, financial, and progress reports (BRD §12) filtered by country, office, and period, and exportable by authorized users.

### FR-24 Configuration
Administrators shall configure countries, offices, partners, roles, individual permissions, country/office access scopes, matching parameters, eligibility rules, activity categories, sponsorship types, notification rules, reporting periods, approval workflows, and document categories.

## 4. Out of Scope

- Student Sponsorship (future; data model must allow it).
- Donor self-service portal.
- Donor relationship management (remains in Donor Care / ERP / CRM).
- Payment gateway integration (future).
