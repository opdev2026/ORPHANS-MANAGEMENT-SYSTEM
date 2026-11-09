# OMS — Technical Architecture Document

**Classification:** Confidential — For Recipients and Authorized Parties Only  
**Version:** 1.0  
**Date:** 11.04.2026  
**Status:** Issued for Review (proposal)

## 1. System Type

OMS is a module inside the Baitulmaal PMO Platform. It consists of web interfaces, a REST API, a relational database, an event/notification subsystem, a matching engine, scheduled jobs, document storage, and an audit subsystem. It reuses PMO platform services for identity, users, documents, tasks, workflows, and notifications where available (BRD §14.1).

## 2. Recommended Stack

| Layer | Recommended technology |
|---|---|
| Frontend | React + TypeScript (or the PMO platform's frontend stack) |
| API | Node.js + TypeScript with NestJS, or the PMO platform's backend stack |
| Database | PostgreSQL |
| Events / queue | Message queue (e.g. RabbitMQ, Redis Streams, or cloud equivalent) |
| Scheduled jobs | Job scheduler (expiry scan, July cycle, quarter-end, overdue checks) |
| Document storage | S3-compatible object storage with encryption |
| Email | Transactional email service (sender & templates pending P-17) |
| Authentication | PMO platform SSO (OIDC/OAuth2) |
| Containers | Docker |
| Monitoring | Centralized logs + metrics + alerting |

Technology may be substituted to match the PMO platform, provided security, maintainability, and supportability are equivalent.

## 3. Logical Architecture

```text
+----------------+  +----------------+  +----------------+  +----------------+
| Partner /      |  | Program &      |  | Field Office   |  | Finance /      |
| Country Portal |  | Review Console |  | Portal         |  | Compliance     |
+-------+--------+  +-------+--------+  +-------+--------+  +-------+--------+
        |                   |                   |                   |
        +-------------------+---------+---------+-------------------+
                                      |
                               HTTPS / REST API
                                      |
                          +-----------v-----------+
                          | AuthN (PMO SSO)       |
                          | AuthZ (RBAC + scope)  |
                          +-----------+-----------+
                                      |
   +-------------+-------------+------+------+-------------+--------------+
   |             |             |             |             |              |
+--v---+   +-----v----+  +-----v-----+ +-----v-----+ +-----v-----+ +------v------+
|Intake|   |Orphan    |  |Sponsorship| |General    | |Finance &  | |Progress &   |
|Verify|   |Registry  |  |+ Matching | |Support    | |Disburse.  | |Donor Report |
+--+---+   +-----+----+  +-----+-----+ +-----+-----+ +-----+-----+ +------+------+
   |             |             |             |             |              |
   +-------------+------+------+-------------+-------------+--------------+
                        |                         |
                 +------v------+          +-------v--------+      +---------------+
                 | PostgreSQL  |          | Domain Events  +----->| Notification  |
                 +-------------+          | (queue)        |      | in-app/email  |
                        |                 +-------+--------+      +---------------+
                 +------v------+                  |
                 | Audit Log   |          +-------v--------+      +---------------+
                 | (append)    |          | Scheduler      |      | Donor Care    |
                 +-------------+          | expiry / July  |      | exchange      |
                                          +----------------+      +---------------+
```

## 4. Backend Responsibilities

- Enforce RBAC, individual permissions, and country/office/record scopes on every request.
- Enforce workflow state transitions and segregation of duties server-side.
- Generate Orphan IDs.
- Run duplicate detection on intake and bulk import.
- Run the matching engine (event-triggered and/or scheduled).
- Emit a domain event for every workflow transition; the notification service maps events to recipients.
- Run scheduled jobs: expiry threshold scan, July progress cycle, quarter-end report due, overdue follow-up, agreement expiry.
- Generate donor-safe report versions excluding restricted fields.
- Apply approved annual report data to Orphan Profiles, archive previous versions, and flag conflicts for review.
- Evaluate section-level and field-level access consistently for pages, documents, reports, lists, and exports.
- Write append-only audit entries.

## 5. Event-Driven Notifications

Every workflow step publishes a domain event (catalogue in `Workflow-Events.md`). The notification service subscribes, resolves recipients by role and scope, and delivers in-app and email notifications according to configurable rules.

```text
Workflow action --> Domain event --> Notification rules --> Recipients (role + scope) --> In-app / Email
                         |
                         +--> Audit log
                         +--> Matching engine (e.g. EVT-SPONSOR-AVAILABLE)
```

## 6. Database

- PostgreSQL with row-level scope filters (country, office, partner).
- Sensitive columns encrypted at rest.
- No hard deletes for business records.

## 7. Integrations

| Integration | Purpose | Status |
|---|---|---|
| PMO platform (users, documents, tasks, workflow, notifications) | Shared services | Required |
| Donor Care / ERP / CRM | Expiry lists, donor decisions, sponsor references | Method pending (P-16 / W-12) |
| Email service | Report delivery and notifications | Required |
| Donation platforms, payment gateways, external data collection | Future | Out of initial scope |

## 8. Deployment

- Separate development, staging, and production environments.
- Containerized services with infrastructure as code.
- Daily backups with tested restore.

## 9. Security Architecture

See `Security-Privacy.md`.

## 10. Architecture Principles

- Server-side enforcement of every permission and transition.
- Configuration over hardcoding (approval authority, matching parameters, thresholds).
- Event-first: every hand-over between stakeholders is an event.
- Privacy by default for donor-facing output.
- Extensible beneficiary model for Student Sponsorship.
