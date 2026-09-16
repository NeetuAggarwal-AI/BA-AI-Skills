# Example output — analysis + backlog (fictional)

> The skill's output for `01-input-brief.md`. Notice it does the **analysis first**, surfaces gaps as **open questions** rather than inventing answers, confirms the decomposition, and only then writes stories with Gherkin AC. Fictional company "Northwind Utilities".

---

## Step 2 — Analysis pass (before any stories)

**Actors / roles**
- Residential customer (registered)
- Prospective customer (no account yet)
- BillMaster (billing system — source of truth for balance & bills)
- CRM (source of truth for contact details)
- PayLink (payment gateway)

**Business rules found**
- BR: BillMaster is the source of truth for balances and bills; the portal is read-only against it.
- BR: Contact details are mastered in CRM, not BillMaster.
- BR: Business customers are out of scope for v1.

**Source-of-truth / data**
- Balance & bills → BillMaster (read).
- Contact details → CRM (read/write).
- Payment → PayLink; payment result must be reflected against the account.

**Dependencies & integration impacts**
- Portal ↔ BillMaster (read balance/bill).
- Portal ↔ CRM (read/write contact details).
- Portal ↔ PayLink (take payment, handle result).
- These are three separate integrations, each with its own failure behaviour.

**Open questions (must be answered by people — not invented)**
- ❓ What should the portal show when **BillMaster is unavailable**? (Raised in the workshop, no decision recorded.)
- ❓ How do customers **register** — self-register, or invite-only? (No decision.)
- ❓ What is the **accessibility policy** the portal must meet (e.g. WCAG 2.1 AA)?
- ❓ What is the **baseline** call volume, and the **target** reduction? ("Reduce calls" has no number.)
- ❓ What are PayLink's rules for **duplicate/idempotent** payments? (Finance flagged past duplicate-charge complaints.)
- ❓ Which contact fields are **mandatory**, and are there validation rules (e.g. address format)?

**Assumptions (clearly labelled — confirm before build)**
- 🅰️ Only residential customers in v1.
- 🅰️ Card is the only payment method in v1 (no direct debit).

---

## Step 3 — Proposed decomposition (confirm before generating)

| Epic | Stories |
|---|---|
| **E1 — Account access** | Register, Log in |
| **E2 — View account** | View current balance, View most recent bill |
| **E3 — Pay a bill** | Pay by card (with duplicate-payment protection) |
| **E4 — Manage contact details** | Update email/phone/address |

*Business value sits at the epic level (E2/E1 drive the call-reduction goal — pending the baseline/target in the open questions).*

> In a real run the skill would pause here for the person to confirm/adjust this mapping before writing stories.

---

## Step 5 — Sample stories (two shown in full)

### E2 · Story: View current balance

**Source requirement:** "Customers can view their current balance" (In scope for v1)

**Background:** Reduces "what's my balance?" calls. Balance is read live from BillMaster (source of truth).

**Story**
As a **registered residential customer**,
I want to **see my current account balance in the portal**,
so that **I don't have to call the contact centre to check it**.

**Business Rules**
- BR1: Balance is read from BillMaster; the portal never calculates or caches it as authoritative.

**Acceptance Criteria**
```
Scenario: Balance shown for a customer in credit
  Given a logged-in residential customer whose BillMaster balance is -$25.00 (in credit)
  When they open the account summary
  Then the balance is shown as "$25.00 in credit"

Scenario: Balance shown for a customer who owes money
  Given a logged-in customer whose BillMaster balance is $80.00
  When they open the account summary
  Then the balance is shown as "$80.00 due"
  And the bill due date is shown

Scenario: BillMaster is unavailable   ← depends on open question
  Given a logged-in customer
  When they open the account summary
  And BillMaster does not respond within [target] seconds
  Then a "balance temporarily unavailable, please try again shortly" message is shown
  And the failure is logged with a correlation ID
```
> ❓ The unavailable-behaviour and the timeout value are **open questions** — the scenario is drafted but flagged, not invented as fact.

**Out of Scope:** Payment (see E3); business customers.

**Success telemetry (AC):** portal balance-view events are logged so the drop in balance-related calls can be measured against the (to-be-confirmed) baseline.

---

### E3 · Story: Pay a bill by card

**Source requirement:** "Customers can pay their bill by card" + finance note on duplicate charges

**Story**
As a **registered residential customer**,
I want to **pay my bill by card in the portal**,
so that **I can settle my account without calling or posting a cheque**.

**Business Rules**
- BR1: Payments are processed through PayLink.
- BR2: A payment result must be reflected against the customer's account.

**Acceptance Criteria**
```
Scenario: Successful card payment
  Given a logged-in customer with an amount of $80.00 due
  When they pay $80.00 by valid card
  Then PayLink returns success
  And the payment is recorded against their account
  And a confirmation with a reference number is shown

Scenario: Duplicate submission is not double-charged   ← from finance's flag
  Given a customer who has just submitted an $80.00 payment
  When the payment request is submitted again with the same idempotency key
  Then the original result is returned
  And no second charge is made

Scenario: Card declined
  Given a logged-in customer
  When PayLink declines the card
  Then a clear "payment declined" message is shown
  And the account balance is unchanged
```
> ❓ The exact idempotency mechanism depends on **PayLink's rules** (open question) — the AC states the required *behaviour*; the implementation detail is confirmed with Finance/PayLink.

**Story type(s):** Integration/API + Data entry → that's why the AC must cover decline, duplicate and success, not just the happy path.

---

## Step 6 — Quality-gate note (illustrative)

- Structure complete ✅ · INVEST ✅ · Traceability ✅ (each links to a brief line)
- Value measurable ⚠️ **gap** — no baseline/target yet (open question)
- NFRs ⚠️ accessibility target unknown (open question); performance target for BillMaster read unknown
- Edge/failure coverage ✅ for the two stories shown

The gaps are **reported, not hidden** — which is exactly the behaviour the skill is designed to produce.
