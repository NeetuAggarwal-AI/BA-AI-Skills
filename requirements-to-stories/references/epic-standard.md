# Epic Standard (generic)

How to detail an **epic** to a consistent standard — the counterpart to `user-story-standard.md`. An epic carries the *business value and outcome*; its child stories inherit that context via traceability.

> **House-standard overlay:** if `references/house-standard.md` exists, apply it on top of this standard and let it win on any conflict.

## Core sections — always include

1. **Epic ID / title / status / owner** — the identity and who owns it (PO).
2. **Epic statement** — the outcome-framed sentence:
   *In order to [outcome], as [organisation/role], we want [capability], so that [value].*
3. **Background / Context** — why this exists, the problem it solves, key systems involved.
4. **Business value & outcome (measurable)** — the value quantified as **baseline → target**, with *how it's measured*. This is what makes an epic more than a big story.
5. **Scope — in scope / out of scope** — what this epic covers and, explicitly, what it does not.
6. **Key business rules (epic-level)** — the rules that govern the whole capability, stated separately from any one story's AC.
7. **Source-of-truth / data** — which system owns each piece of data (read vs write).
8. **Dependencies** — upstream (what must come first), downstream (who consumes the result), cross-system, and external teams. Note any **blocked-by** relationship to another epic/story so it can be linked (see `output-adapters.md` → dependency mapping).
9. **Child features / stories** — the decomposition: the list of stories (and features, if used) that deliver this epic.
10. **Traceability** — source requirement (doc heading / page section / work-item ID), parent (if any), blocked-by / related links.

## Include where relevant

- **Hypothesis framing** — an alternative to the epic statement for discovery/experiment epics: *"We believe [change] will [outcome]. We'll know we're right when [metric]."*
- **Success metrics & telemetry** — the specific events/logging that make the value measurable from day one (call it out separately when the instrumentation itself is work).
- **Actors / personas** — when the epic spans several roles or systems.
- **Risks & constraints (epic-level NFRs)** — reliability, security/compliance (e.g. PCI), performance, privacy — the non-functionals that apply across the whole epic, with target values.
- **Assumptions** — labelled, to be confirmed.
- **Open questions** — what's unresolved; these block full detailing of the child stories.
- **Definition of Done (epic-level)** — when the *whole* epic is considered delivered (all stories done + UAT, telemetry live, downstream validated).

## Rules

- **Value is mandatory and must be measurable.** "Improve the experience" is not an epic value; "reduce payment calls by 40% in 3 months" is. If the number isn't known yet, capture it as an **open question**, don't invent it.
- **Don't fabricate targets, metrics or rules.** Same guardrail as everywhere in this skill — unknowns become open questions or labelled assumptions.
- **Keep epic and story layers distinct.** Epic-level business rules and NFRs are the ones that span stories; story-specific detail lives on the story, not repeated on the epic.
- **Every child story traces back to this epic.**

---

## Worked example (fictional — "Northwind Utilities")

# EPIC — E3: Online Bill Payment

**Epic ID:** NU-EP-03 · **Status:** Draft · **Owner (PO):** [name] · **Target release:** v1

**Epic statement**
In order to reduce payment-related contact-centre calls and give customers a faster way to pay, as Northwind Utilities, we want residential customers to be able to pay their bill by card in the self-service portal, so that customers can settle their account without calling or posting a cheque.

**Background / Context**
Today customers can only pay by phone or cheque, which drives call volume and slows collections. Balances/bills are owned by BillMaster; payments are processed by the existing PayLink gateway. Finance has flagged historic duplicate-charge complaints, so payment reliability is a first-class concern.

**Business value & outcome (measurable)**

| Measure | Baseline | Target | How measured |
|---|---|---|---|
| Payment-related calls | [1,800/mo] | −40% in 3 months | Contact-centre call tags |
| Card-payment adoption | 0% | 25% of eligible payments in 3 months | Portal payment events |
| Avg days to payment after bill issue | [12 days] | −3 days | BillMaster settlement data |

*(Targets are placeholders to confirm with the sponsor — see Open Questions.)*

**Success metrics & telemetry** — payment events (initiated/succeeded/failed/declined) logged with correlation IDs; duplicate-charge incidents tracked (target: zero).

**In scope** — pay full outstanding balance by card; confirmation with reference number; duplicate-submission protection; payment result reflected against the account.
**Out of scope (v1)** — partial/instalment payments; direct debit or other methods; business customers; stored cards.

**Actors** — registered residential customer (payer); BillMaster (source of truth); PayLink (gateway); Finance/collections (downstream).

**Key business rules (epic-level)**
- BR1: Payments processed only through PayLink.
- BR2: Payment result must be reflected against the account in BillMaster.
- BR3: Only the full outstanding balance can be paid in v1.
- BR4: A duplicate submission must never result in a second charge.

**Source-of-truth / data** — balance & bill → BillMaster (read); payment → PayLink; settlement record → BillMaster (write).

**Dependencies**
- Upstream: **blocked by NU-EP-01 (Account Access)** — must be registered/logged in to pay.
- Cross-system: PayLink integration contract & idempotency rules confirmed.
- Downstream: Finance reconciliation consumes payment data.
- External team: Payments/PayLink team for contract and test credentials.

**Risks & constraints (epic-level NFRs)**
- Reliability: duplicate-charge risk — mitigated by idempotency (BR4); target zero.
- Security/PCI: card details never stored by the portal (confirm PayLink hosted fields).
- Performance: confirmation shown within [target] seconds.

**Assumptions** — 🅰️ card only in v1; 🅰️ residential customers only; 🅰️ full-balance payment only.

**Open questions** — ❓ PayLink's exact idempotency rules? ❓ confirmed baseline/target numbers? ❓ PCI via PayLink hosted fields?

**Child features / stories**

| ID | Type | Title |
|---|---|---|
| NU-101 | Story | Pay full balance by card (happy path) |
| NU-102 | Story | Prevent duplicate charge on resubmission |
| NU-103 | Story | Handle card declined / payment failure |
| NU-104 | Story | Reflect payment result against account (BillMaster write-back) |
| NU-105 | Story | Payment confirmation + reference number |

**Traceability** — Source: BRD §"Customers can pay their bill by card" + Finance duplicate-charge note. Parent: — (top-level). **Blocked by:** NU-EP-01. Related: NU-EP-02 (View Account).

**Definition of Done (epic-level)** — all child stories done + UAT passed; zero duplicate-charge defects in UAT; payment telemetry live in a dashboard; Finance reconciliation validated end-to-end.
