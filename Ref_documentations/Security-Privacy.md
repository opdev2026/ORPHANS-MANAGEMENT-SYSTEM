# OMS — Security & Privacy Requirements

**Version:** 1.0 | **Date:** 11.04.2026 | **Status:** Issued for Review

## 1. Security Objectives

- Protect orphans' personal, family, health, and identity data.
- Prevent unauthorized intake, approval, matching, or financial actions.
- Guarantee traceability of every fund movement and decision.
- Ensure only approved, privacy-reviewed content reaches sponsors.

## 2. Access Control

- RBAC with individual permission overrides.
- Country-, office-, partner-, and record-level scopes applied to every query.
- Server-side enforcement only; the UI hiding an action is not a control.
- Donor Care isolation from all orphan case data.

## 3. Privileged Accounts

- MFA for administrators, approvers (PM, FN, CP), and reviewers.
- Administrators have no default approval authority or sensitive-data access.
- Permission changes are audited.

## 4. Segregation of Duties

- A submitter cannot be the only approver of their own record or report.
- A Field Office Manager cannot approve their own quarterly report.
- A partner cannot approve its own orphan verification.
- Matching changes require explicit permission and always create an audit record.
- Sponsorship status changes only after a recorded Donor Care decision.
- No external sending before privacy approval.

## 5. Data Classification

| Class | Examples | Donor sharing |
|---|---|---|
| Restricted | National IDs, identity documents, detailed address, contact info, detailed medical records, guardian details | Never (unless approved policy) |
| Internal | Vulnerability assessment, field notes, financial records | No |
| Donor-shareable | Beneficiary reference, first name/age (per policy P-10), education progress, general health and social updates, approved photos | After privacy review |

## 6. Privacy Review Workflow

1. Report prepared.
2. System generates a donor version excluding restricted fields.
3. Authorized reviewer checks text and media for sensitive content.
4. Reviewer approves or returns.
5. Email is sent; delivery status is recorded.

## 7. Threat Areas

- Unauthorized access across countries/offices.
- Insider manipulation of matching or disbursement amounts.
- Fake or duplicate orphan records.
- Leakage of sensitive data in emails, exports, or photo metadata (strip EXIF/GPS).
- Forged proof-of-delivery documents.
- Malicious file uploads (scan uploads, restrict types and size).

## 8. Data Protection

- TLS in transit; encryption at rest for database and document storage.
- Signed, time-limited URLs for documents and media.
- Exports limited by permission and audited.
- Backups encrypted.

## 9. Privacy Governance

- Final list of donor-shareable fields to be approved by Baitulmaal (P-10).
- Retention periods for documents and media to be approved.
- Consent from guardians for photos and media according to country policy.
