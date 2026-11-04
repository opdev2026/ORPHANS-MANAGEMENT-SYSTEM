# OMS — Risks, Assumptions & Dependencies

**Version:** 1.0 | **Date:** 11.04.2026

## 1. Key Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Sensitive beneficiary data exposed to donors | High | Data classification, generated donor version, mandatory privacy review |
| Duplicate or fraudulent orphan records | High | Duplicate detection, document verification, approval workflow |
| Funds transferred but not delivered | High | Separate delivered status, mandatory proof, reconciliation |
| Unauthorized matching changes | High | Permission control, mandatory reason, audit |
| Pending business rules delay development | High | Configurable rules, early decision workshop |
| Poor data quality in migration | Medium | Validation, staged migration, reconciliation reports |
| Weak connectivity in field offices | Medium | Mobile-friendly forms, resumable uploads |
| Donor Care integration unclear | Medium | Defined exchange contract, interim manual process |
| PMO platform dependency delays | Medium | Early integration contract and mocks |
| Email deliverability issues | Medium | Approved sender domain, delivery tracking, retry |

## 2. Assumptions

- Baitulmaal's PMO platform provides SSO and user management.
- Partners and offices are responsible for the accuracy of submitted data.
- Donor Care continues to own donor relationships in its own systems.
- Email is the only donor communication channel for OMS in the initial release.
- Sponsorship amounts and currencies follow approved policies.

## 3. Important Non-Assumption

OMS records evidence of fund delivery; it does not by itself prove the physical receipt of funds. Field verification and audits remain necessary.

## 4. Pending Business Decisions

| ID | Item | Required clarification |
|---|---|---|
| P-01 / W-02 | Matching parameters | Final criteria, weights, priority rules |
| P-02 / W-11 | Matching frequency | Event-triggered or scheduled |
| P-03 / W-03 | Renewal notification | Notification period before expiry |
| P-04 | Renewal cancellation | Cancellation channel and deadline |
| P-05 / W-04 | Financial reporting | Quarterly submission deadlines |
| P-06 | Financial approval | Additional approval levels |
| P-07 | General support | Country-specific eligibility policies |
| P-08 | Activity repetition | Repeated participation rules |
| P-09 | Progress reports | Indicators and templates |
| P-10 | Donor communication | Permitted beneficiary information |
| P-11 | Disbursement | Currencies and payment methods |
| P-12 | Funding allocation | Allocation and restriction policies |
| P-13 | Sponsorship amounts | Monthly/annual values and currency rules |
| P-14 | Age eligibility | Transition at age 18 |
| P-15 | Data migration | Existing sources and requirements |
| P-16 / W-12 | Donor Care integration | Data exchange method |
| P-17 / W-09 | Email delivery | Sender, templates, authorized senders |
| W-01 | Verification approval | Who gets direct approval permission |
| W-05 | General support approval | Extra approval before execution |
| W-06 | Privacy review | Final authorized reviewer |
| W-07 | Country access | Country/office access boundaries |
| W-08 | Bulk import | Separate confirmation before verification queue |
| W-10 | System administration | Central vs. country-level configuration |

## 5. Dependencies

- PMO platform services.
- Email service and sender domain.
- Object storage.
- Donor Care / ERP / CRM interface.
- Legal and privacy review.

## 6. Risk Acceptance

Business owners should explicitly accept residual risks before production deployment.
