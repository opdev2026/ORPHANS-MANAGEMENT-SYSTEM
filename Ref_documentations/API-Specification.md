# OMS — REST API Specification

**Version:** 1.0 | **Date:** 11.04.2026 | **Status:** Issued for Review (proposal)

## 1. API Conventions

Base path:

`/api/v1/orphan-management`

JSON request/response bodies. Lists support `page`, `pageSize`, `sort`, and filters (`country`, `office`, `status`, `period`).

Authentication uses the PMO platform session/token. Every endpoint enforces role, individual permission, and scope.

## 2. Partners & Compliance

`POST /partners` — Partner user / Admin

`POST /partners/{partnerId}/compliance-submissions` — Partner user

`POST /partners/{partnerId}/compliance-submissions/{id}/decision` — Compliance Officer (`APPROVE`, `REJECT`, `RETURN`, comment)

`GET /partners/{partnerId}` — Authorized users

## 3. Intake

`POST /intakes` — Partner / Country Office (creates `DRAFT`)

`PATCH /intakes/{intakeId}` — Creator while `DRAFT` or `RETURNED_FOR_CORRECTION`

`POST /intakes/{intakeId}/documents` — Creator

`POST /intakes/{intakeId}/submit`

`GET /intakes?status=UNDER_REVIEW` — Reviewer queue

## 4. Bulk Import

`GET /bulk-imports/template`

`POST /bulk-imports` — Upload Excel; returns validation report (errors, duplicates, missing fields)

`GET /bulk-imports/{importId}`

`POST /bulk-imports/{importId}/confirm` — Creates intakes for valid rows (idempotent)

## 5. Verification & Approval

`POST /intakes/{intakeId}/review/return` — Reviewer (comments required)

`POST /intakes/{intakeId}/review/verify` — Reviewer; system routes to direct approval or Program Manager

`POST /intakes/{intakeId}/approve` — Reviewer with `orphan.approve` or Program Manager

`POST /intakes/{intakeId}/reject` — Reviewer / Program Manager (comments required)

## 6. Orphans

`GET /orphans` — Scoped list

`GET /orphans/{orphanId}` — Profile (sections filtered by permission)

`PATCH /orphans/{orphanId}` — Authorized country/program users (audited)

`POST /orphans/{orphanId}/eligibility` — Program staff

`GET /orphans/{orphanId}/history` — Sponsorships, activities, disbursements, visits, reports

`GET /orphans/{orphanId}/versions` — Version history (authorized users)

`GET /orphans/{orphanId}/versions/{version}` — Historical version

`GET /orphans/{orphanId}/profile-update-flags`

`POST /orphans/{orphanId}/profile-update-flags/{flagId}/resolve` — Authorized reviewer (`ACCEPT` / `DISCARD`)

## 7. Sponsors & Matching

`GET /sponsors/available`

`POST /matching/runs` — Run engine (or triggered by event/schedule)

`GET /matching/runs/{runId}` — Suggested and completed matches

`POST /matching/assignments` — Manual match (`matching.override`, reason required)

`PATCH /matching/assignments/{sponsorshipId}` — Change/reassign (reason required)

## 8. Sponsorships

`GET /sponsorships?status=EXPIRING_SOON`

`GET /sponsorships/{sponsorshipId}`

`POST /sponsorships/{sponsorshipId}/donor-decisions` — Donor Care (`RENEW`, `CANCEL`, `DISCONTINUED`)

`POST /sponsorships/{sponsorshipId}/status` — Sponsorship Coordinator (after confirmed decision)

## 9. Donor Care Exchange

`GET /donor-care/expiring-sponsorships` — Donor Care (sponsor + sponsorship reference only, no orphan case data)

`GET /donor-care/pending-communications`

## 10. General Support

`POST /activities` · `PATCH /activities/{id}` · `POST /activities/{id}/submit`

`POST /activities/{id}/approve` — Program Manager

`POST /activities/{id}/beneficiaries` — Country/Field office (eligibility rules enforced)

`POST /activities/{id}/completion` — Actual beneficiaries + evidence

`POST /activities/{id}/report/review` — Program Manager

## 11. Finance

`POST /funding-sources` · `POST /allocations` — Finance

`POST /transfers` — Central Finance

`POST /disbursements/{id}/received` · `POST /disbursements/{id}/delivered` — Field Office Manager (evidence required)

`POST /disbursements/{id}/reconcile` — Finance

## 12. Quarterly Financial Reports

`POST /financial-reports` · `PATCH /financial-reports/{id}` · `POST /financial-reports/{id}/submit`

`POST /financial-reports/{id}/program-decision` — Program Manager (`APPROVE` / `RETURN`)

`POST /financial-reports/{id}/finance-decision` — Central Finance (`APPROVE` / `REJECT`; only after program approval)

## 13. Field Visits

`POST /orphans/{orphanId}/visits` · `GET /orphans/{orphanId}/visits`

## 14. Annual Progress Reports

`POST /progress-cycles` — Start July cycle (Program Management / scheduler)

`POST /progress-reports/{id}/assign` — Field Office Manager

`PATCH /progress-reports/{id}` · `POST /progress-reports/{id}/submit` — Field Officer

`POST /progress-reports/{id}/review` · `POST /progress-reports/{id}/approve` — Program staff / Manager

`POST /progress-reports/{id}/privacy-review` — Authorized reviewer

`POST /progress-reports/{id}/send` — Authorized OMS user (idempotent)

`GET /progress-reports/{id}/comparison` — Year-over-year indicators

## 15. Notifications

`GET /notifications` · `POST /notifications/{id}/read`

`GET /notification-rules` · `PUT /notification-rules/{eventCode}` — Admin

## 16. Reports & Audit

`GET /reports/{reportType}` with `format=json|xlsx|pdf`

`GET /audit-logs` — Authorized management / audit users

## 17. Standard Errors

| Code | Meaning |
|---|---|
| 400 | Validation error |
| 401 | Not authenticated |
| 403 | Not permitted (role, permission, or scope) |
| 404 | Not found or not in scope |
| 409 | Invalid state transition, duplicate, or conflicting sponsorship |
| 422 | Business rule violation (e.g. no available sponsor, missing evidence) |
| 429 | Rate limited |

## 18. Idempotency

Bulk-import confirmation, matching runs, transfers, and email sending accept an `Idempotency-Key` header.

## 19. API Security

- All endpoints over HTTPS.
- Responses filter restricted fields by permission.
- Donor Care endpoints never return orphan case data.
- Every state-changing request is audited.

## 20. Access Administration

`GET /permission-groups` · `POST /permission-groups` · `PATCH /permission-groups/{groupId}` — Admin

`PUT /users/{userId}/permissions` — Grant or revoke individual permissions (audited)

`GET /profile-access-rules` · `PUT /profile-access-rules` — Section and field access

Profile, document, report, list, and export responses are filtered by the same access rules.
