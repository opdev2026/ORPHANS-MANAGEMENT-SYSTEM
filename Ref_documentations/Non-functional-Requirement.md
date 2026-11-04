# OMS — Non-Functional Requirements

**Version:** 1.0 | **Date:** 11.04.2026 | **Status:** Issued for Review

## 1. Performance

- Responsive screens and list queries under normal operating load.
- Bulk import and matching runs must handle large batches without blocking other users.

## 2. Availability

- High availability for users across all operating countries.
- Planned maintenance announced in advance.

## 3. Scalability

- Multiple countries, offices, and partners.
- Growth in orphans, sponsorships, media, and reports without redesign.
- Additional beneficiary programs (Student Sponsorship).

## 4. Security

See `Security-Privacy.md`.

## 5. Privacy

- Data minimization in every external output.
- Sensitive fields masked by default in the UI for users without the right permission.

## 6. Reliability

- Notifications and emails delivered through a retrying queue.
- No partial bulk imports: confirmation is atomic per batch.
- Scheduled jobs are restart-safe and idempotent.

## 7. Auditability

- Every critical action recorded (Workflow Matrix Part 7).
- Audit log append-only and retained per policy.

## 8. Usability

- Consistent workflow screens with clear pending-action indicators.
- Structured forms with validation.
- Arabic and English interface with RTL support (to be confirmed).
- Usable on tablets and mobile for field officers, with tolerance for weak connectivity.

## 9. Maintainability

- Configurable rules (approval, matching, thresholds, notifications) without code changes.
- Automated tests for workflows and permissions.

## 10. Observability

- Centralized logs, metrics, and alerting for API, jobs, queue, and email failures.

## 11. Disaster Recovery

- Regular encrypted backups.
- Restore procedure documented and tested.
