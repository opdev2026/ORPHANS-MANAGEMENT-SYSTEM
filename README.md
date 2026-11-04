# Baitulmaal Orphan Management System (OMS)

A program management module of the Baitulmaal PMO Platform for orphan registration, verification, sponsorship, general support, financial disbursement, field follow-up, and donor reporting.

## What is this project?

The Orphan Management System (OMS) centralizes every operational activity related to orphan beneficiaries across Baitulmaal's international offices and partner organizations.

Partners and country offices register orphans through a controlled intake process. Reviewers verify every record before it becomes an official Orphan Profile with a unique Orphan ID. Verified orphans are matched to available sponsors, supported through general activities, followed up by field teams, and reported on annually to their sponsors after privacy review.

## Repository contents

| Folder / File | Purpose |
|---|---|
| `Overview.md` | Problem, goals, stakeholders, and high-level flow |
| `Ref_documentations/` | Product, business, workflow, technical, data, UI/UX, security, QA, timeline, and risk documentation |
| `Ref_documentations/Workflow-Events.md` | Stakeholder swimlane flowcharts and the notification event catalogue |
| `Ref_documentations/diagrams/` | Swimlane flowcharts as SVG images (one per workflow) |
| `Before review/` | Early project notes and review material |

## Core flow

**Partner / Country Office → Orphan File Reviewer → Program Manager → Sponsorship Coordinator → Field Office → Finance → Sponsor (via approved email)**

Donor Care owns all donor relationship activity and coordinates with OMS without access to orphan case files.

## Main principles

- Only approved partners and country offices may submit orphan intake records.
- Every orphan must pass verification before becoming eligible for sponsorship.
- Every orphan has one unique Orphan ID; all activity is linked to it.
- Sponsor matching is automated, and every manual override is audited.
- Renewal is automatic unless the sponsor requests cancellation through Donor Care.
- Quarterly financial reports require Program Manager approval, then Central Finance approval.
- Transferred funds are never assumed delivered; proof of delivery is mandatory.
- Only approved, privacy-reviewed information is shared with sponsors.
- Termination never deletes sponsorship or financial history.

*Project: Baitulmaal Orphan Management System | Version 1.0 | 11.04.2026*
