# Example — a fully-detailed epic (fictional)

> A worked epic for the fictional "Northwind Utilities" portal, detailed to `references/epic-standard.md`. It shows every section an epic can carry; in practice, the core sections are always included and the rest are used where relevant. It also shows the **blocked-by** dependency that the skill would propose as a link (see `references/output-adapters.md` → dependency mapping).

---

# EPIC — E3: Online Bill Payment

**Epic ID:** NU-EP-03 · **Status:** Draft · **Owner (PO):** [name] · **Target release:** v1

### Epic statement
In order to reduce payment-related contact-centre calls and give customers a faster way to pay, as Northwind Utilities, we want residential customers to be able to pay their bill by card in the self-service portal, so that customers can settle their account without calling or posting a cheque.

### Background / Context
Today customers can only pay by phone or cheque, which drives call volume and slows collections. Balances/bills are owned by BillMaster; payments are processed by the existing PayLink gateway. Finance has flagged historic duplicate-charge complaints, so payment reliability is a first-class concern.

### Business value & outcome (measurable)

| Measure | Baseline | Target | How measured |
|---|---|---|---|
| Payment-related calls | [1,800/mo] | −40% in 3 months | Contact-centre call tags |
| Card-payment adoption | 0% | 25% of eligible payments in 3 months | Portal payment events |
| Avg days to payment after bill issue | [12 days] | −3 days | BillMaster settlement data |

*(Targets are placeholders to confirm with the sponsor — see Open Questions.)*

### Success metrics & telemetry
Payment events (initiated/succeeded/failed/declined) logged with correlation IDs; duplicate-charge incidents tracked (target: zero).

### In scope
Pay full outstanding balance by card; confirmation with reference number; duplicate-submission protection; payment result reflected against the account.

### Out of scope (v1)
Partial/instalment payments; direct debit or other methods; business customers; stored cards.

### Actors
Registered residential customer (payer); BillMaster (source of truth); PayLink (gateway); Finance/collections (downstream).

### Key business rules (epic-level)
- BR1: Payments processed only through PayLink.
- BR2: Payment result must be reflected against the account in BillMaster.
- BR3: Only the full outstanding balance can be paid in v1.
- BR4: A duplicate submission must never result in a second charge.

### Source-of-truth / data
Balance & bill → BillMaster (read); payment → PayLink; settlement record → BillMaster (write).

### Dependencies
- Upstream: **blocked by NU-EP-01 (Account Access)** — must be registered/logged in to pay.
- Cross-system: PayLink integration contract & idempotency rules confirmed.
- Downstream: Finance reconciliation consumes payment data.
- External team: Payments/PayLink team for contract and test credentials.

### Risks & constraints (epic-level NFRs)
- Reliability: duplicate-charge risk — mitigated by idempotency (BR4); target zero.
- Security/PCI: card details never stored by the portal (confirm PayLink hosted fields).
- Performance: confirmation shown within [target] seconds.

### Assumptions
🅰️ card only in v1; 🅰️ residential customers only; 🅰️ full-balance payment only.

### Open questions
❓ PayLink's exact idempotency rules? ❓ confirmed baseline/target numbers? ❓ PCI via PayLink hosted fields?

### Child features / stories

| ID | Type | Title |
|---|---|---|
| NU-101 | Story | Pay full balance by card (happy path) |
| NU-102 | Story | Prevent duplicate charge on resubmission |
| NU-103 | Story | Handle card declined / payment failure |
| NU-104 | Story | Reflect payment result against account (BillMaster write-back) |
| NU-105 | Story | Payment confirmation + reference number |

### Traceability
Source: BRD §"Customers can pay their bill by card" + Finance duplicate-charge note. Parent: — (top-level). **Blocked by:** NU-EP-01. Related: NU-EP-02 (View Account).

### Definition of Done (epic-level)
All child stories done + UAT passed; zero duplicate-charge defects in UAT; payment telemetry live in a dashboard; Finance reconciliation validated end-to-end.

---

### Proposed dependency links (what the skill would show in the dry-run)

| From | Relationship | To | Why |
|---|---|---|---|
| NU-EP-03 | is blocked by | NU-EP-01 | Customers must be able to log in before they can pay |
| NU-102 | is blocked by | NU-101 | Duplicate-charge handling builds on the base payment flow |
| NU-104 | is blocked by | NU-101 | Write-back depends on a successful payment existing |

*These are proposed, not auto-created — they'd be written as Jira/ADO links only after approval.*
