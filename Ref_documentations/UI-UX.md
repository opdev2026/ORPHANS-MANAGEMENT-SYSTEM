# OMS — UI/UX Specification

**Version:** 1.0 | **Date:** 11.04.2026 | **Status:** Issued for Review

## 1. Design Principles

- Every user lands on a "My pending actions" list generated from notifications and tasks.
- Status is always visible as a badge with the next responsible role.
- Sensitive data is masked unless the user has permission.
- Approve / Reject / Return actions always show history and require comments where mandated.
- Arabic and English with RTL support (to be confirmed).

## 2. Management Dashboard (CM, PM)

- KPIs: registered, verified, eligible, sponsored, unsponsored orphans; by country, office, age group.
- Sponsorship: active, expiring, renewed, terminated, available sponsors.
- Finance: allocated vs. transferred vs. delivered vs. pending.
- Reporting: quarterly report status by office; annual report completion.
- Pending approvals queue.

## 3. Partner / Country Office Portal

- Intake form wizard: Personal → Family → Vulnerability → Documents → Review & Submit.
- Bulk import: download template → upload → validation report (errors, duplicates, missing) → confirm.
- Returned applications with reviewer comments highlighted per field.
- Partner compliance status and agreement expiry banner.

## 4. Review Console (RV, PM)

- Queue filtered by country, partner, age, waiting time.
- Side-by-side view: submitted data and document viewer.
- Field-level comment and "Return for correction".
- Duplicate candidates panel.
- "Approve" when permitted, otherwise "Verify & forward to Program Manager".

## 5. Orphan Profile

Tabs: Overview · Family · Documents · Verification history · Sponsorship · General support · Finance · Field visits · Progress reports · Audit.

## 6. Sponsorship Console (SC)

- Matching pool and waiting list.
- Matching run results with suggested matches and priority reasons.
- Manual override dialog with mandatory reason.
- Expiring sponsorships list; Donor Care decision status.

## 7. Donor Care View (DC)

- Expiring sponsorships (sponsor and sponsorship reference only).
- Record donor decision: Renew / Cancel / Discontinued.
- No access to orphan tabs.

## 8. Field Office Portal (FM, FO)

- Funds received, to deliver, delivered; upload proof per orphan.
- Quarterly report builder with auto-filled totals.
- Activity execution: select beneficiaries, record attendance, upload evidence.
- Visit and progress report forms optimized for mobile with photo/video capture.

## 9. Finance & Compliance Consoles

- Finance: allocations, transfers, reconciliation, second-stage report approval.
- Compliance: partner submissions, agreement validity, approve/reject.

## 10. Progress Report & Privacy Review

- Year-over-year comparison view (improved / declined / unchanged / missing).
- Donor preview with restricted fields removed; reviewer checklist.
- Send and delivery status.

## 11. Accessibility

- WCAG 2.1 AA target.
- Keyboard navigation and screen-reader labels.

## 12. UX Safety

- Confirmation dialogs for approvals, terminations, transfers, and sending emails.
- Clear warning when an action affects a donor-facing output.

## 13. Responsive Design

- Desktop-first for review, finance, and management consoles.
- Mobile/tablet-ready for field officers.
