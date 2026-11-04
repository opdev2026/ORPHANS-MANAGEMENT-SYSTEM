# OMS — Data Model Document

**Classification:** Confidential — For Recipients and Authorized Parties Only  
**Version:** 1.0  
**Date:** 11.04.2026  
**Status:** Issued for Review (proposal)

## 1. Conventions

- Timestamps: ISO 8601 UTC.
- Dates: `YYYY-MM-DD`.
- Primary identifiers: UUID v4.
- Orphan public ID: server-generated, immutable, non-sensitive.
- Money: decimal(14,2) + ISO 4217 currency code.
- Fields marked *Sensitive* are restricted and excluded from donor-facing output.
- `beneficiaryType` is included on beneficiary entities to allow future Student Sponsorship (BR-032).

## 2. Country & Office

| Field | Type | Required | Description |
|---|---|---|---|
| officeId | UUID | Yes | Primary key |
| countryCode | string(2) | Yes | ISO 3166-1 alpha-2 |
| type | enum | Yes | CENTRAL, COUNTRY_OFFICE, FIELD_OFFICE |
| parentOfficeId | UUID | No | Field office → country office |
| name | string | Yes | Display name |
| status | enum | Yes | ACTIVE, INACTIVE |

## 3. Partner

| Field | Type | Required | Description |
|---|---|---|---|
| partnerId | UUID | Yes | Primary key |
| name | string | Yes | Legal name |
| countryCode | string(2) | Yes | Operating country |
| complianceStatus | enum | Yes | SUBMITTED, UNDER_REVIEW, RETURNED, APPROVED, REJECTED, ACTIVE, SUSPENDED |
| agreementStart / agreementEnd | date | No | Agreement validity |
| reviewedBy | UUID | No | Compliance Officer |

## 4. User

| Field | Type | Required | Description |
|---|---|---|---|
| userId | UUID | Yes | Primary key |
| email | string | Yes | Login identifier |
| roles | enum[] | Yes | R01–R12 (see `Roles-Permissions.md`) |
| permissionOverrides | string[] | No | Individual grants/restrictions (e.g. `orphan.approve`, `matching.override`) |
| countryScope / officeScope | string[] / UUID[] | No | Record-level access boundaries |
| partnerId | UUID | No | For partner users |
| status | enum | Yes | ACTIVE, LOCKED, DISABLED |

## 5. IntakeApplication

| Field | Type | Required | Description |
|---|---|---|---|
| intakeId | UUID | Yes | Primary key |
| source | enum | Yes | FORM, BULK_IMPORT |
| submittedByUserId | UUID | Yes | Creator |
| partnerId / officeId | UUID | Yes | Origin |
| status | enum | Yes | See `business logic.md` §2 |
| payload | json | Yes | Personal, family, vulnerability data (versioned) |
| version | int | Yes | Incremented on resubmission |
| assignedReviewerId | UUID | No | Reviewer |
| duplicateCandidates | UUID[] | No | Probable duplicates |

## 6. Orphan

| Field | Type | Required | Description |
|---|---|---|---|
| orphanId | UUID | Yes | Internal key |
| publicId | string | Yes | `ORP-EG-2026-000142` |
| beneficiaryType | enum | Yes | ORPHAN (future: STUDENT) |
| fullName | string | Yes | *Sensitive* in full form |
| dateOfBirth | date | Yes | Used for age priority |
| gender | enum | Yes | |
| countryCode / officeId | | Yes | Location scope |
| address | string | No | *Sensitive* |
| educationGrade | string | No | |
| guardian | json | Yes | *Sensitive* (name, ID, contact) |
| siblingsCount | int | No | |
| vulnerability | json | No | Health, social, living, income |
| eligibilityStatus | enum | Yes | PENDING_ELIGIBILITY_REVIEW, ELIGIBLE, NOT_ELIGIBLE, SUSPENDED, CLOSED |
| sponsorshipStatus | enum | Yes | AVAILABLE_FOR_MATCHING, MATCHED, SPONSORED, … |
| photoDocumentId | UUID | No | Approved photo |
| registeredAt | datetime | Yes | |

## 7. Document

| Field | Type | Required | Description |
|---|---|---|---|
| documentId | UUID | Yes | Primary key |
| ownerType / ownerId | enum / UUID | Yes | ORPHAN, INTAKE, PARTNER, DISBURSEMENT, VISIT, REPORT, ACTIVITY |
| category | enum | Yes | BIRTH_CERT, FATHER_DEATH_CERT, GUARDIAN_ID, SCHOOL_CERT, MEDICAL, RECEIPT, TRANSFER_CONFIRMATION, SIGNED_ACK, PHOTO, VIDEO, AGREEMENT, OTHER |
| storageKey | string | Yes | Object storage reference |
| sensitivity | enum | Yes | RESTRICTED, INTERNAL, DONOR_SHAREABLE |
| verificationStatus | enum | No | PENDING, ACCEPTED, REJECTED |

## 8. Sponsor (reference)

| Field | Type | Required | Description |
|---|---|---|---|
| sponsorId | UUID | Yes | Internal key |
| externalDonorRef | string | Yes | Reference in Donor Care / CRM |
| displayName | string | Yes | |
| email | string | Yes | For approved report delivery |
| availableCapacity | int | Yes | Number of orphans that can still be sponsored |

Donor relationship data remains in Donor Care systems; OMS stores only what is needed for matching and reporting.

## 9. Sponsorship

| Field | Type | Required | Description |
|---|---|---|---|
| sponsorshipId | UUID | Yes | Primary key |
| orphanId / sponsorId | UUID | Yes | Link |
| type | enum | Yes | MONTHLY, ANNUAL |
| amount / currency | decimal / string | Yes | |
| paymentFrequency | enum | Yes | |
| startDate / endDate | date | Yes | |
| status | enum | Yes | See `business logic.md` §5 |
| matchedBy | enum | Yes | ENGINE, MANUAL |
| previousSponsorshipId | UUID | No | Renewal chain |

## 10. MatchingChange

| Field | Type | Required | Description |
|---|---|---|---|
| changeId | UUID | Yes | Primary key |
| sponsorId | UUID | Yes | Sponsor reference |
| previousOrphanId / newOrphanId | UUID | No/Yes | Assignments |
| reason | string | Yes | Mandatory |
| changedBy / changedAt | UUID / datetime | Yes | |

## 11. DonorDecision

| Field | Type | Required | Description |
|---|---|---|---|
| decisionId | UUID | Yes | Primary key |
| sponsorshipId | UUID | Yes | |
| type | enum | Yes | RENEW, CANCEL, DISCONTINUED |
| recordedBy | UUID | Yes | Donor Care user |
| effectiveDate | date | Yes | |

## 12. Activity & Participation

| Field | Type | Required | Description |
|---|---|---|---|
| activityId | UUID | Yes | Primary key |
| category | enum | Yes | WINTER_CLOTHING, EID, SEASONAL, RECREATIONAL, EDUCATIONAL, OTHER |
| countryCode / officeId | | Yes | |
| targetBeneficiaries / actualBeneficiaries | int | Yes | |
| status | enum | Yes | See `business logic.md` §8 |
| ActivityParticipant | (activityId, orphanId, attended) | Yes | Linked to profile |

## 13. Funding, Allocation & Disbursement

| Entity | Key fields |
|---|---|
| FundingSource | sourceId, type (SPONSORSHIP, GENERAL, RESTRICTED, OTHER), amount, currency, restrictions |
| Allocation | allocationId, sourceId, purpose (SPONSORSHIP, GENERAL_SUPPORT, ACTIVITY, OFFICE), targetId, amount, period |
| Disbursement | disbursementId, orphanId, allocationId, fieldOfficeId, period, amountTransferred, amountDelivered, deliveredAt, status (PENDING, TRANSFERRED, DELIVERED, RECONCILED), evidenceDocumentIds |

## 14. QuarterlyFinancialReport

| Field | Type | Required | Description |
|---|---|---|---|
| reportId | UUID | Yes | |
| fieldOfficeId / period | UUID / string | Yes | e.g. `2026-Q3` |
| sponsorshipExpenditure / generalSupportExpenditure / activityExpenditure | decimal | Yes | |
| amountsReceived / amountsDelivered / remainingBalance | decimal | Yes | |
| status | enum | Yes | See `business logic.md` §10 |
| ApprovalStep | (stage PROGRAM/FINANCE, decision, userId, comment, at) | Yes | |

## 15. FieldVisit

| Field | Type | Required | Description |
|---|---|---|---|
| visitId | UUID | Yes | |
| orphanId / fieldOfficerId | UUID | Yes | |
| visitDate / nextFollowUpDate | date | Yes / No | |
| notes | text | No | |
| mediaDocumentIds | UUID[] | No | |

## 16. ProgressReport

| Field | Type | Required | Description |
|---|---|---|---|
| progressReportId | UUID | Yes | |
| orphanId / year | UUID / int | Yes | One per sponsored orphan per year |
| education / health / social | json | Yes | Indicators (template pending P-09) |
| status | enum | Yes | See `business logic.md` §11 |
| privacyReviewedBy | UUID | No | |
| donorVersionDocumentId | UUID | No | Generated donor-safe version |

## 17. Notification

| Field | Type | Required | Description |
|---|---|---|---|
| notificationId | UUID | Yes | |
| eventCode | string | Yes | e.g. `EVT-INTAKE-SUBMITTED` (see `Workflow-Events.md`) |
| recipientUserId / recipientRole | UUID / enum | Yes | |
| channel | enum | Yes | IN_APP, EMAIL |
| recordType / recordId | | Yes | Deep link |
| status | enum | Yes | PENDING, SENT, READ, FAILED |

## 18. EmailDelivery

| Field | Type | Required | Description |
|---|---|---|---|
| deliveryId | UUID | Yes | |
| progressReportId / sponsorId | UUID | Yes | |
| status | enum | Yes | QUEUED, SENT, DELIVERED, FAILED |
| attempts / lastError | int / string | Yes / No | |

## 19. AuditLog

| Field | Type | Required | Description |
|---|---|---|---|
| auditId | UUID | Yes | |
| userId / userRole | UUID / enum | Yes | |
| action | string | Yes | |
| recordType / recordId | | Yes | |
| previousValue / newValue | json | No | |
| reason / comment / decision | string | No | |
| at | datetime | Yes | |

Audit records are append-only.
