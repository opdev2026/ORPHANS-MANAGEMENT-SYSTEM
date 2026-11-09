# OMS — Non-Functional Requirements

**Version:** 1.0 | **Date:** 11.04.2026 | **Status:** Issued for Review

## 1. Performance

- Standard screens and list queries: p95 under 2 seconds.
- Orphan profile load: p95 under 3 seconds including media thumbnails.
- Bulk import of 5,000 rows validated within 5 minutes.
- Matching run for 10,000 eligible orphans completes within 2 minutes.

## 2. Availability

- Target availability 99.5% during business hours across operating time zones.
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
- Profile version history retained without permanent deletion.
- Permission changes and sensitive data access recorded.

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

- Daily backups; RPO ≤ 24 hours, RTO ≤ 8 hours (to be confirmed).
- Restore tested at least twice a year.
