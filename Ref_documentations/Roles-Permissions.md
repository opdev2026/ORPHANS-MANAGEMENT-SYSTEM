# OMS — Roles & Permissions Matrix

**Version:** 1.0 | **Date:** 11.04.2026 | **Source:** Workflow & Permission Matrix v1.1, Part 3

## 1. Roles

| Code | Role |
|---|---|
| CM | R01 Central Management |
| PM | R02 Orphan Program Manager |
| RV | R03 Orphan File Reviewer |
| CO | R04 Country Office Manager |
| PT | R05 Partner User |
| FM | R06 Field Office Manager |
| FO | R07 Field Officer |
| SC | R08 Sponsorship Coordinator |
| DC | R09 Donor Care User |
| FN | R10 Central Finance Reviewer |
| CP | R11 Compliance Officer |
| AD | R12 System Administrator |

**Legend:** V View · C Create · E Edit · S Submit · R Review · A Approve · J Reject · X Export · M Manage · — No access

## 2. Main Module Permissions

| Module / Function | CM | PM | RV | CO | PT | FM | FO | SC | DC | FN | CP | AD |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Orphan Registry | V | V/E | V | V/E | V/C/E* | V | V* | V | — | V* | — | M |
| Intake Forms | V | V | V/R | V/C/E | C/E/S | V | — | — | — | — | — | M |
| Bulk Import | V | V | V | V/C/S | C/S | V/C/S | — | — | — | — | — | M |
| Document Verification | V | V | V/R/A* | V | V/C | V | V/C* | — | — | — | V | M |
| Orphan Profile | V | V/E | V | V/E* | V* | V/E* | V* | V | — | V* | — | M |
| Sponsorship Records | V | V/E | V | V | — | V | — | V/E* | — | V* | — | M |
| Sponsor Matching | V | V | — | V | — | — | — | V/E/M* | — | — | — | M |
| Sponsorship Renewal | V | V | — | V | — | V | — | V/E | Donor-side | — | — | M |
| Sponsorship Termination | V | V | — | V | — | — | — | V/E | Donor-side | — | — | M |
| General Support Activities | V | V/E/A | — | V/E | — | V/C/E | V* | — | — | V* | — | M |
| Beneficiary Selection | V | V/A | — | V/E | — | V/E | V* | — | — | — | — | M |
| Financial Records | V | V | — | V* | — | V/C/E | — | V* | — | V/E | — | M |
| Quarterly Financial Reports | V | V/R/A | — | V | — | V/C/E/S | — | — | — | V/R/A/J | — | M |
| Disbursement Evidence | V | V | — | V | — | V/C/S | V* | — | — | V/R | — | M |
| Field Visits | V | V | — | V | — | V | V/C/E/S | — | — | — | — | M |
| Annual Progress Reports | V | V/R/A | V/R | V | — | V/C/E/S | V/C/E/S | V | — | — | — | M |
| Donor Report Preparation | V | V/A | — | V | — | V | V/C | — | — | — | — | M |
| Email Delivery | V | V/A | — | — | — | — | — | — | — | — | — | M |
| Compliance | V | V | — | V | V/S | — | — | — | — | — | V/E/A/J | M |
| Reports & Analytics | V/X | V/X | V* | V/X | V* | V/X | V* | V/X | Donor reports | V/X | V/X | M |
| User Management | — | — | — | — | — | — | — | — | — | — | — | M |
| System Settings | — | — | — | — | — | — | — | — | — | — | — | M |

`*` V* = limited to assigned records; A* = direct approval only when explicitly granted; E/M* = matching permissions configurable per user.  
"Donor-side" = donor records and communication only, never orphan operational records.

## 3. Configurable Individual Permissions

| Permission | Default | Purpose |
|---|---|---|
| `orphan.approve` | Off for RV, On for PM | Scenario A (direct reviewer approval) vs. Scenario B |
| `matching.override` | Configurable for SC | Manual match / reassign |
| `report.privacy_review` | Pending W-06 | Authorized privacy reviewer |
| `report.send_email` | PM / authorized OMS user (W-09) | Send approved donor reports |

## 4. Rules

- Review and approval are separate permissions; reviewing never implies approving.
- Administrators manage configuration but receive no business approval authority or unrestricted sensitive data access by default.
- Donor Care has no access to orphan profiles, family data, documents, field reports, or case functions.
- Two users with the same role may have different scopes (country, office, assigned records).
- Permissions must be enforced by the backend.
