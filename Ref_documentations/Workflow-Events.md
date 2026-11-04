# OMS — Stakeholder Workflows & Notification Events

**Classification:** Confidential — For Recipients and Authorized Parties Only  
**Version:** 1.0  
**Date:** 11.04.2026  
**Status:** Issued for Review  
**Source:** BRD v1.1 §6–§11 · Workflow & Permission Matrix v1.1 Parts 2 & 5

## 1. How to read the diagrams

Each diagram groups steps by **stakeholder lane** (one box per stakeholder). Every hand-over between stakeholders is a **notification event**.

| Shape | Meaning |
|---|---|
| Rectangle | Workflow step performed by the lane's stakeholder |
| Diamond | Decision |
| ⚡ Hexagon (yellow) | Notification event published by the system |
| Rounded red | End / termination |
| Solid arrow `→` | Workflow sequence |
| Dotted arrow `⇢` | Event emitted / event delivered to recipient |

Event names use the format `EVT-<AREA>-<ACTION>`. The full catalogue is in §8.

---

## 2. End-to-end stakeholder event map

The sequence below shows the main lifecycle of one orphan, from partner onboarding to the annual report reaching the sponsor. Each arrow is an event; the arrow's target is the stakeholder who must act.

```mermaid
sequenceDiagram
    autonumber
    participant PT as Partner / Country Office
    participant CP as Compliance Officer
    participant SYS as OMS System
    participant RV as Orphan File Reviewer
    participant PM as Program Manager
    participant SC as Sponsorship Coordinator
    participant DC as Donor Care
    participant FN as Central Finance
    participant FM as Field Office Manager
    participant FO as Field Officer
    participant SP as Sponsor

    rect rgb(232, 245, 233)
    Note over PT,PM: Phase 1 — Partner compliance, intake & verification
    PT->>SYS: Submit partner agreements
    SYS--)CP: ⚡ EVT-PARTNER-SUBMITTED
    CP->>SYS: Approve partner
    SYS--)PT: ⚡ EVT-PARTNER-APPROVED
    PT->>SYS: Submit intake (form / bulk import)
    SYS--)RV: ⚡ EVT-INTAKE-SUBMITTED
    RV->>SYS: Return for correction (if incomplete)
    SYS--)PT: ⚡ EVT-INTAKE-RETURNED
    PT->>SYS: Resubmit
    SYS--)RV: ⚡ EVT-INTAKE-RESUBMITTED
    RV->>SYS: Complete verification
    SYS--)PM: ⚡ EVT-VERIFICATION-COMPLETED (if no direct approval)
    PM->>SYS: Approve record
    SYS->>SYS: Generate Orphan ID & profile
    SYS--)SC: ⚡ EVT-ORPHAN-APPROVED
    end

    rect rgb(227, 242, 253)
    Note over SYS,SP: Phase 2 — Sponsorship matching
    SYS--)SYS: ⚡ EVT-SPONSOR-AVAILABLE → run matching engine
    SYS--)SC: ⚡ EVT-MATCH-COMPLETED
    SC->>SYS: Manual override (optional, with reason)
    SYS--)PM: ⚡ EVT-MATCH-MODIFIED
    end

    rect rgb(255, 243, 224)
    Note over FN,FM: Phase 3 — Disbursement & quarterly financial report
    FN->>SYS: Allocate & transfer funds
    SYS--)FM: ⚡ EVT-FUNDS-TRANSFERRED
    FM->>SYS: Deliver to family + upload proof
    SYS--)FN: ⚡ EVT-DELIVERY-PROOF-SUBMITTED
    SYS--)FM: ⚡ EVT-QUARTERLY-REPORT-DUE
    FM->>SYS: Submit quarterly report
    SYS--)PM: ⚡ EVT-PROGRAM-APPROVAL-PENDING
    PM->>SYS: Program approval (1st)
    SYS--)FN: ⚡ EVT-FINANCE-APPROVAL-PENDING
    FN->>SYS: Finance approval (2nd)
    SYS--)FM: ⚡ EVT-FINANCIAL-REPORT-FINALIZED
    end

    rect rgb(243, 229, 245)
    Note over SYS,SP: Phase 4 — Annual progress report (July)
    SYS--)FM: ⚡ EVT-ANNUAL-REPORT-DUE
    FM->>SYS: Assign orphans to field officers
    SYS--)FO: ⚡ EVT-PROGRESS-ASSIGNED
    FO->>SYS: Visit, collect updates, upload media, submit
    SYS--)PM: ⚡ EVT-PROGRESS-SUBMITTED
    PM->>SYS: Approve report
    SYS--)PM: ⚡ EVT-PRIVACY-REVIEW-PENDING (authorized reviewer)
    PM->>SYS: Privacy review approved
    SYS-)SP: 📧 Approved donor report (email)
    SYS--)PM: ⚡ EVT-REPORT-SENT / EVT-REPORT-DELIVERY-FAILED
    end

    rect rgb(255, 235, 238)
    Note over SC,SP: Phase 5 — Renewal & termination
    SYS--)SC: ⚡ EVT-SPONSORSHIP-EXPIRING
    SYS--)DC: ⚡ EVT-RENEWAL-NOTIFICATION-DUE
    DC->>SP: Renewal communication (Donor Care channel)
    SP->>DC: Cancellation request (optional)
    DC->>SYS: Record donor decision
    SYS--)SC: ⚡ EVT-SPONSORSHIP-RENEWED or EVT-CANCELLATION-REQUESTED
    SC->>SYS: Update sponsorship status
    SYS--)PM: ⚡ EVT-SPONSORSHIP-TERMINATED
    PM->>SYS: Review eligibility
    SYS--)SC: ⚡ EVT-ORPHAN-RETURNED-TO-POOL
    end
```

---

## 3. Partner compliance, intake & verification

```mermaid
flowchart TB
    subgraph L_PT["🏢 Partner / Country Office"]
        P1["Submit partner agreements<br/>& compliance documents"]
        P2["Create intake form<br/>or upload bulk Excel"]
        P3["Submit intake"]
        P4["Correct & resubmit"]
    end
    subgraph L_CP["🛡️ Compliance Officer"]
        C1["Review agreements &<br/>compliance requirements"]
        C2{"Approve partner?"}
    end
    subgraph L_SYS["⚙️ OMS System"]
        S1["Activate partner<br/>registration access"]
        S2["Validate data &<br/>detect duplicates"]
        S3{"Reviewer has direct<br/>approval permission?"}
        S4["Generate Orphan ID<br/>& Orphan Profile"]
    end
    subgraph L_RV["🔎 Orphan File Reviewer"]
        R1["Review personal, family<br/>info & documents"]
        R2{"Complete & correct?"}
        R3["Approve record<br/>(Scenario A)"]
    end
    subgraph L_PM["👤 Orphan Program Manager"]
        M1{"Approve forwarded<br/>record? (Scenario B)"}
    end
    subgraph L_SC["🤝 Sponsorship Coordinator"]
        K1["Monitor eligibility &<br/>matching pool"]
    end

    P1 --> C1 --> C2
    C2 -- Yes --> S1
    C2 -- "No / Return" --> P1
    S1 --> P2 --> S2 --> P3 --> R1 --> R2
    R2 -- No --> P4 --> R1
    R2 -- Yes --> S3
    S3 -- Yes --> R3 --> S4
    S3 -- No --> M1
    M1 -- Yes --> S4
    M1 -- "Return" --> P4
    M1 -- "Reject" --> X1(["Rejected<br/>(history kept)"])
    S4 --> K1

    E1{{"⚡ EVT-PARTNER-SUBMITTED"}}
    E2{{"⚡ EVT-PARTNER-APPROVED / REJECTED"}}
    E3{{"⚡ EVT-INTAKE-SUBMITTED"}}
    E4{{"⚡ EVT-INTAKE-RETURNED<br/>(missing documents)"}}
    E5{{"⚡ EVT-VERIFICATION-COMPLETED"}}
    E6{{"⚡ EVT-ORPHAN-APPROVED"}}

    P1 -.-> E1 -.-> C1
    C2 -.-> E2 -.-> P2
    P3 -.-> E3 -.-> R1
    R2 -.-> E4 -.-> P4
    S3 -.-> E5 -.-> M1
    S4 -.-> E6 -.-> K1

    classDef event fill:#FFE8A3,stroke:#D49B00,color:#222
    classDef decision fill:#E8F0FE,stroke:#4A73C9,color:#222
    classDef stop fill:#FDE2E1,stroke:#C0392B,color:#222
    class E1,E2,E3,E4,E5,E6 event
    class C2,S3,R2,M1 decision
    class X1 stop
```

**Rules:** only approved partners can submit (BR-002) · bulk import never bypasses verification · returned records keep history · reviewer approval is a configurable permission (W-01).

---

## 4. Sponsorship matching, renewal & termination

```mermaid
flowchart TB
    subgraph L_SYS["⚙️ OMS System / Matching Engine"]
        S1{"Available sponsor<br/>exists?"}
        S2["Identify eligible orphans<br/>& apply priorities"]
        S3["Create sponsorship<br/>(Active)"]
        S4["Detect expiry<br/>threshold"]
        S5{"Cancellation<br/>recorded?"}
        S6["Auto-renew &<br/>record new dates"]
        S7["Return orphan to<br/>matching pool"]
        W1["Waiting list"]
    end
    subgraph L_SC["🤝 Sponsorship Coordinator"]
        K1["Review match result"]
        K2["Manual override<br/>(reason required)"]
        K3["Review renewal list"]
        K4["Update sponsorship<br/>to Terminated"]
    end
    subgraph L_DC["💬 Donor Care"]
        D1["Send renewal notification<br/>& follow up with sponsor"]
        D2["Receive cancellation<br/>request, if any"]
        D3["Record & communicate<br/>donor decision"]
    end
    subgraph L_PM["👤 Program Manager / Program Staff"]
        M1{"Orphan still<br/>eligible?"}
    end
    subgraph L_SP["💚 Sponsor"]
        SP1["Receives renewal<br/>communication"]
    end

    S1 -- No --> W1
    S1 -- Yes --> S2 --> S3 --> K1
    K1 -. optional .-> K2 --> S3
    S3 --> S4 --> K3 --> D1 --> SP1 --> D2 --> D3 --> S5
    S5 -- No --> S6 --> S3
    S5 -- Yes --> K4 --> M1
    M1 -- Yes --> S7 --> S1
    M1 -- No --> X1(["Case closed<br/>(history kept)"])

    E1{{"⚡ EVT-SPONSOR-AVAILABLE"}}
    E2{{"⚡ EVT-MATCH-COMPLETED"}}
    E3{{"⚡ EVT-MATCH-MODIFIED"}}
    E4{{"⚡ EVT-SPONSORSHIP-EXPIRING"}}
    E5{{"⚡ EVT-RENEWAL-NOTIFICATION-DUE"}}
    E6{{"⚡ EVT-CANCELLATION-REQUESTED"}}
    E7{{"⚡ EVT-SPONSORSHIP-RENEWED"}}
    E8{{"⚡ EVT-SPONSORSHIP-TERMINATED"}}
    E9{{"⚡ EVT-ORPHAN-RETURNED-TO-POOL"}}

    E1 -.-> S1
    S3 -.-> E2 -.-> K1
    K2 -.-> E3 -.-> M1
    S4 -.-> E4 -.-> K3
    S4 -.-> E5 -.-> D1
    D3 -.-> E6 -.-> K4
    S6 -.-> E7 -.-> K3
    K4 -.-> E8 -.-> M1
    S7 -.-> E9 -.-> K1

    classDef event fill:#FFE8A3,stroke:#D49B00,color:#222
    classDef decision fill:#E8F0FE,stroke:#4A73C9,color:#222
    classDef stop fill:#FDE2E1,stroke:#C0392B,color:#222
    class E1,E2,E3,E4,E5,E6,E7,E8,E9 event
    class S1,S5,M1 decision
    class X1 stop
```

**Rules:** no matching without an available sponsor (BR-008) · priority to orphans whose sponsorship ended, then younger orphans · renewal is the default (BR-012) · Donor Care owns all sponsor communication (BR-013) · termination never deletes history (BR-017).

---

## 5. Disbursement & quarterly financial report

```mermaid
flowchart TB
    subgraph L_FN["💰 Central Finance"]
        F1["Record funding source<br/>& allocation"]
        F2["Transfer funds to<br/>Field Office Manager"]
        F3["Review evidence &<br/>reconcile"]
        F4{"Finance approval<br/>(2nd)"}
    end
    subgraph L_FM["📍 Field Office Manager"]
        G1["Record amount received"]
        G2["Deliver funds to<br/>orphan / family"]
        G3["Upload proof of delivery<br/>& delivered amount"]
        G4["Prepare & submit<br/>quarterly report"]
        G5["Correct report"]
    end
    subgraph L_PM["👤 Program Manager"]
        M1{"Program approval<br/>(1st)"}
    end
    subgraph L_SYS["⚙️ OMS System"]
        S1["Link transaction to<br/>Orphan ID"]
        S2["Quarter-end<br/>schedule"]
        S3["Update consolidated<br/>reporting (Finalized)"]
    end

    F1 --> F2 --> G1 --> G2 --> G3 --> S1 --> F3
    S2 --> G4 --> M1
    M1 -- Approve --> F4
    M1 -- Return --> G5 --> G4
    F4 -- Approve --> S3
    F4 -- Reject --> G5

    E1{{"⚡ EVT-FUNDS-TRANSFERRED"}}
    E2{{"⚡ EVT-DELIVERY-PROOF-SUBMITTED"}}
    E3{{"⚡ EVT-QUARTERLY-REPORT-DUE"}}
    E4{{"⚡ EVT-PROGRAM-APPROVAL-PENDING"}}
    E5{{"⚡ EVT-FINANCE-APPROVAL-PENDING"}}
    E6{{"⚡ EVT-FINANCIAL-REPORT-RETURNED / REJECTED"}}
    E7{{"⚡ EVT-FINANCIAL-REPORT-FINALIZED"}}

    F2 -.-> E1 -.-> G1
    G3 -.-> E2 -.-> F3
    S2 -.-> E3 -.-> G4
    G4 -.-> E4 -.-> M1
    M1 -.-> E5 -.-> F4
    F4 -.-> E6 -.-> G5
    S3 -.-> E7 -.-> G4

    classDef event fill:#FFE8A3,stroke:#D49B00,color:#222
    classDef decision fill:#E8F0FE,stroke:#4A73C9,color:#222
    class E1,E2,E3,E4,E5,E6,E7 event
    class M1,F4 decision
```

**Rules:** transferred ≠ delivered · proof of delivery is mandatory (BR-022) · Program Manager first, Finance second, both mandatory (BR-019, BR-020) · a Field Office Manager cannot approve their own report.

---

## 6. General support activity

```mermaid
flowchart TB
    subgraph L_PS["🗂️ Orphan Program Staff"]
        A1["Create activity: objectives,<br/>country/office, target, rules"]
        A2["Submit for approval"]
    end
    subgraph L_PM["👤 Program Manager"]
        M1{"Approve activity?"}
        M2{"Accept activity<br/>report?"}
    end
    subgraph L_FO["📍 Country / Field Office"]
        B1["Select beneficiaries<br/>from registry"]
        B2["Execute activity"]
        B3["Record actual beneficiaries<br/>& upload evidence"]
    end
    subgraph L_SYS["⚙️ OMS System"]
        S1["Check country-specific<br/>eligibility & frequency"]
        S2["Link participation to<br/>each Orphan Profile"]
    end

    A1 --> A2 --> M1
    M1 -- No --> A1
    M1 -- Yes --> B1 --> S1 --> B2 --> B3 --> M2
    M2 -- Return --> B3
    M2 -- Accept --> S2 --> X1(["Closed"])

    E1{{"⚡ EVT-ACTIVITY-SUBMITTED"}}
    E2{{"⚡ EVT-ACTIVITY-DUE"}}
    E3{{"⚡ EVT-ACTIVITY-REPORT-SUBMITTED"}}

    A2 -.-> E1 -.-> M1
    M1 -.-> E2 -.-> B1
    B3 -.-> E3 -.-> M2

    classDef event fill:#FFE8A3,stroke:#D49B00,color:#222
    classDef decision fill:#E8F0FE,stroke:#4A73C9,color:#222
    classDef stop fill:#DFF5E1,stroke:#2E7D32,color:#222
    class E1,E2,E3 event
    class M1,M2 decision
    class X1 stop
```

---

## 7. Annual progress report & donor email (July cycle)

```mermaid
flowchart TB
    subgraph L_SYS["⚙️ OMS System"]
        S1["July 1: generate reporting<br/>list of sponsored orphans"]
        S2["Generate donor version<br/>(restricted fields removed)"]
        S3["Send email &<br/>record delivery status"]
    end
    subgraph L_PM["👤 Program Management"]
        M1["Initiate reporting cycle"]
        M2{"Report complete<br/>& approved?"}
    end
    subgraph L_FM["📍 Field Office Manager"]
        G1["Assign orphans<br/>to field officers"]
    end
    subgraph L_FO["🧑‍🌾 Field Officer"]
        O1["Visit orphan & family"]
        O2["Record education, health,<br/>social progress + media"]
        O3["Submit progress report"]
    end
    subgraph L_PR["🛡️ Authorized Privacy Reviewer"]
        V1{"Sensitive data<br/>removed?"}
    end
    subgraph L_SP["💚 Sponsor"]
        SP1["Receives approved<br/>annual report by email"]
    end

    S1 --> M1 --> G1 --> O1 --> O2 --> O3 --> M2
    M2 -- Return --> O2
    M2 -- Approve --> S2 --> V1
    V1 -- No --> S2
    V1 -- Yes --> S3 --> SP1

    E1{{"⚡ EVT-ANNUAL-REPORT-DUE"}}
    E2{{"⚡ EVT-PROGRESS-ASSIGNED"}}
    E3{{"⚡ EVT-PROGRESS-SUBMITTED"}}
    E4{{"⚡ EVT-PROGRESS-RETURNED"}}
    E5{{"⚡ EVT-PRIVACY-REVIEW-PENDING"}}
    E6{{"⚡ EVT-REPORT-SENT / DELIVERY-FAILED"}}
    E7{{"⚡ EVT-FOLLOWUP-OVERDUE"}}

    S1 -.-> E1 -.-> G1
    G1 -.-> E2 -.-> O1
    O3 -.-> E3 -.-> M2
    M2 -.-> E4 -.-> O2
    S2 -.-> E5 -.-> V1
    S3 -.-> E6 -.-> M1
    E7 -.-> O1

    classDef event fill:#FFE8A3,stroke:#D49B00,color:#222
    classDef decision fill:#E8F0FE,stroke:#4A73C9,color:#222
    class E1,E2,E3,E4,E5,E6,E7 event
    class M2,V1 decision
```

**Rules:** mandatory for every sponsored orphan (BR-024) · cycle starts in July (BR-025) · only approved, privacy-reviewed content is sent (BR-028) · Donor Care handles any donor follow-up about the report.

---

## 8. Notification event catalogue

| Event code | Trigger | Published by | Recipient(s) | Required action | Channel |
|---|---|---|---|---|---|
| EVT-PARTNER-SUBMITTED | Partner submits compliance documents | System | Compliance Officer | Review agreements | In-app, Email |
| EVT-PARTNER-APPROVED | Compliance approves partner | Compliance Officer | Partner User, Country Office Manager | Start intake | In-app, Email |
| EVT-PARTNER-REJECTED | Compliance rejects / returns | Compliance Officer | Partner User | Correct documents | In-app, Email |
| EVT-PARTNER-AGREEMENT-EXPIRED | Agreement end date reached | Scheduler | Compliance Officer, Partner User, Country Office Manager | Renew agreement | In-app, Email |
| EVT-BULK-IMPORT-VALIDATED | Excel validated | System | Uploader | Review errors & duplicates, confirm | In-app |
| EVT-INTAKE-SUBMITTED | Intake submitted | System | Orphan File Reviewer | Review application | In-app |
| EVT-INTAKE-RETURNED | Verification incomplete / missing documents | Reviewer | Partner / Country Office | Upload missing documents | In-app, Email |
| EVT-INTAKE-RESUBMITTED | Corrections submitted | System | Orphan File Reviewer | Re-review | In-app |
| EVT-VERIFICATION-COMPLETED | Reviewer finishes verification | System | Reviewer / Program Manager | Approve according to permissions | In-app |
| EVT-ORPHAN-APPROVED | Record approved, Orphan ID generated | System | Sponsorship Coordinator, submitter | Monitor eligibility | In-app |
| EVT-ORPHAN-REJECTED | Record rejected | Reviewer / PM | Submitter | Note decision | In-app |
| EVT-SPONSOR-AVAILABLE | Sponsor capacity becomes available | System | Matching Engine, Sponsorship Coordinator | Run matching | System, In-app |
| EVT-MATCH-COMPLETED | Assignment created (new sponsorship) | Matching Engine | Sponsorship Coordinator | Monitor assignment | In-app |
| EVT-MATCH-MODIFIED | Manual override | Sponsorship Coordinator | Relevant program staff | Review audit history | In-app |
| EVT-SPONSORSHIP-EXPIRING | Configured expiry threshold (P-03) | Scheduler | Orphan Management (SC), Donor Care | Renewal preparation | In-app, Email |
| EVT-RENEWAL-NOTIFICATION-DUE | Expiration approaching | Scheduler | Donor Care | Contact sponsor | In-app, Email |
| EVT-CANCELLATION-REQUESTED | Donor Care records cancellation | Donor Care | Sponsorship Coordinator | Update sponsorship | In-app |
| EVT-SPONSORSHIP-RENEWED | No cancellation by end date / renewal confirmed | System | Sponsorship Coordinator, Donor Care | Note new dates | In-app |
| EVT-SPONSORSHIP-TERMINATED | Cancellation confirmed | Sponsorship Coordinator | Orphan Program Staff | Recheck eligibility | In-app |
| EVT-ORPHAN-RETURNED-TO-POOL | Eligible orphan re-pooled | System | Sponsorship Coordinator | Monitor re-matching | In-app |
| EVT-ACTIVITY-SUBMITTED | Activity submitted for approval | Program Staff | Program Manager | Approve activity | In-app |
| EVT-ACTIVITY-DUE | Activity approved / scheduled | System | Country / Field Office | Select beneficiaries & execute | In-app, Email |
| EVT-ACTIVITY-REPORT-SUBMITTED | Activity evidence submitted | Field Office | Program Manager | Review report | In-app |
| EVT-FUNDS-TRANSFERRED | Transfer recorded | Central Finance | Field Office Manager | Confirm receipt, deliver | In-app, Email |
| EVT-DELIVERY-PROOF-SUBMITTED | Proof uploaded | Field Office Manager | Finance / Program Staff | Review evidence | In-app |
| EVT-QUARTERLY-REPORT-DUE | Quarter-end schedule (P-05) | Scheduler | Field Office Manager | Submit report | In-app, Email |
| EVT-PROGRAM-APPROVAL-PENDING | Quarterly report submitted | System | Program Manager | Review report | In-app |
| EVT-FINANCE-APPROVAL-PENDING | Program approval completed | System | Central Finance | Financial review | In-app |
| EVT-FINANCIAL-REPORT-RETURNED | Program Manager returns | Program Manager | Field Office Manager | Correct report | In-app, Email |
| EVT-FINANCIAL-REPORT-REJECTED | Finance rejects | Central Finance | Field Office Manager, Program Manager | Correct report | In-app, Email |
| EVT-FINANCIAL-REPORT-FINALIZED | Both approvals complete | System | Field Office Manager, Program Manager | — | In-app |
| EVT-ANNUAL-REPORT-DUE | July reporting cycle | Scheduler | Field Office Manager | Collect progress data | In-app, Email |
| EVT-PROGRESS-ASSIGNED | Orphan assigned to field officer | Field Office Manager | Field Officer | Visit & collect data | In-app |
| EVT-PROGRESS-SUBMITTED | Progress report submitted | Field Officer | Program Manager | Review & approve | In-app |
| EVT-PROGRESS-RETURNED | Report returned | Program staff | Field Officer | Correct report | In-app |
| EVT-PRIVACY-REVIEW-PENDING | Report approved, donor version ready | System | Authorized privacy reviewer | Check information | In-app |
| EVT-REPORT-SENT | Email delivery attempted | System | Orphan Management | Monitor delivery status | In-app |
| EVT-REPORT-DELIVERY-FAILED | Email bounced / failed | System | Orphan Management | Retry / correct address via Donor Care | In-app |
| EVT-FOLLOWUP-OVERDUE | Next follow-up date passed | Scheduler | Field Officer, Field Office Manager | Conduct visit | In-app, Email |

## 9. Event payload (standard)

```json
{
  "eventCode": "EVT-INTAKE-SUBMITTED",
  "occurredAt": "2026-11-04T09:30:00Z",
  "actor": { "userId": "uuid", "role": "PARTNER_USER" },
  "record": { "type": "INTAKE", "id": "uuid", "status": "SUBMITTED" },
  "scope": { "countryCode": "EG", "officeId": "uuid", "partnerId": "uuid" },
  "recipients": [{ "role": "ORPHAN_FILE_REVIEWER" }],
  "link": "/intakes/uuid"
}
```

- Recipients are resolved by role **and** scope (country, office, assigned records).
- Payloads sent to Donor Care never contain orphan case data.
- Every event is also written to the audit log.
- Thresholds and recipients are configurable in Notification Rules (Workflow Matrix Part 8).
