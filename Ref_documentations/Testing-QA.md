# OMS — Testing & QA Specification

**Version:** 1.0 | **Date:** 11.04.2026 | **Status:** Issued for Review

## 1. Test Levels

- Unit tests for business rules and state transitions.
- Integration tests for API, database, queue, and email.
- Permission tests per role × module (matrix-driven).
- End-to-end workflow tests per stakeholder journey.
- Security and privacy testing.
- Performance testing for bulk import and matching.
- User acceptance testing with Baitulmaal stakeholders.

## 2. Critical Test Scenarios

### Partner compliance
An unapproved or suspended partner cannot submit an intake. Agreement expiry triggers `EVT-PARTNER-AGREEMENT-EXPIRED`.

### Intake & verification
A submitted intake is not an orphan record. Returned records keep history. A reviewer without `orphan.approve` can only forward to the Program Manager.

### Bulk import
Duplicates and invalid rows are reported; only confirmed valid rows enter verification; no row bypasses review.

### Orphan ID
IDs are unique under concurrent approvals and never change.

### Matching
No match without an available sponsor. Priority ordering is correct. No orphan gets two active sponsorships under concurrent runs. Manual override without a reason or permission is rejected and audited when accepted.

### Renewal & termination
No cancellation → automatic renewal with new dates. Cancellation → termination → orphan returns to pool with priority. History remains.

### General support
Selection outside country rules is blocked. Planned vs. actual counts are recorded.

### Finance
Transferred is not counted as delivered. Delivery without evidence is rejected. Finance cannot approve before Program Manager approval. A Field Office Manager cannot approve their own report.

### Annual progress
July 1 generates a request for every sponsored orphan. Report cannot be sent before approval and privacy review. Delivery failure is recorded and retryable.

### Privacy
Donor versions never include restricted fields. Donor Care cannot reach any orphan endpoint.

### Notifications
Every event in `Workflow-Events.md` reaches the correct role and scope only.

## 3. Security Acceptance

- No critical or high vulnerabilities open at release.
- Cross-country / cross-office access tests pass.
- Upload scanning and file-type restrictions verified.

## 4. UAT

Each stakeholder group executes its journey from `Workflow-Events.md` and signs off.
