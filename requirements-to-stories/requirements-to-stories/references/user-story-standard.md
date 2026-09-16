# User Story Standard (generic)

This is the default, vendor-neutral standard the skill applies. It is deliberately not tied to any one company's template.

> **House-standard overlay:** if a `references/house-standard.md` file exists in this skill copy, apply it **on top of** this standard, and let it win on any conflict. That's how a team layers its own template, Definition of Ready/Done, mandatory fields and terminology without forking the skill.

---

## The story sentence

```
As a [role / persona],
I want to [capability],
so that [outcome / value].
```

- The **role** is a specific actor, not "user" wherever a more precise role exists (e.g. "returning customer", "call-centre agent", "billing system").
- The **capability** is one coherent thing the role can do.
- The **value** is the real reason — the benefit, not a restatement of the capability.

## What a *complete* story contains

A one-line story sentence is not a story. A complete story has these sections, each as its own heading (or explicitly marked N/A):

1. **Background / Context** — why this exists, what problem it solves, links to the source requirement.
2. **Story sentence** — as above.
3. **Design / screens** — links or references, if the story is UI-relevant.
4. **Business Rules** — the rules that govern behaviour, stated separately from the AC.
5. **Acceptance Criteria** — testable conditions (Gherkin or rule-based; see below).
6. **Out of Scope** — what this story explicitly does *not* cover.
7. **References** — source requirement (doc heading, page section, work item ID), related stories, decisions.

## INVEST — check before calling a story done

- **I**ndependent — can be delivered without depending on another unfinished story where possible.
- **N**egotiable — describes the need, not a locked-down solution.
- **V**aluable — delivers value to a user or the business.
- **E**stimable — the team could size it.
- **S**mall — fits comfortably in a sprint.
- **T**estable — you can prove it's done.

If a story fails INVEST (usually **Small** or **Independent**), split it and say how.

---

## Acceptance criteria

Two formats. Choose per story; a story can mix if needed.

### Gherkin (preferred for interactive / UI / workflow stories)

```
Scenario: [short name of the case]
  Given [context / precondition]
  When [action / trigger]
  Then [expected, observable outcome]
```

- Write **one scenario per case**, not one giant scenario.
- Always include the **happy path** *and* the **known edge/failure cases** relevant to the story type (invalid input, empty/boundary, permission denied or revoked, timeout/offline/retry, duplicate/concurrent, stale/expired, cancel mid-flow).
- `Then` must be **observable and testable** — a state, a message, a stored value, a call made — not "it works".
- Use `And` / `But` to add steps; keep each step atomic.

**Example**

```
Scenario: Customer views balance with a settled account
  Given a logged-in customer with a settled account
  When they open the account summary
  Then the current balance is shown as $0.00
  And no "amount due" prompt is displayed

Scenario: Balance service is unavailable
  Given a logged-in customer
  When they open the account summary
  And the balance service does not respond within 3 seconds
  Then a "balance temporarily unavailable, try again shortly" message is shown
  And the failure is logged with a correlation ID
```

### Rule-based (useful for backend / business-rule-heavy stories)

Plain, numbered, testable conditions:

```
AC1: A duplicate submission with the same idempotency key returns the original result and does not create a second record.
AC2: Amounts are rounded to 2 decimal places using half-up rounding.
AC3: A request from a role without the "approve" permission is rejected with a 403 and logged.
```

## Good AC is: **C-T-R-S-C**

Clear, Testable, Relevant, Specific, Complete (happy path + known edge cases). If any AC could pass while the feature is actually broken, it isn't specific enough.

---

## Epics, features, stories

- **Epic** — a large body of work / a business capability. Carries the *business value* (baseline → target).
- **Feature** — a coherent slice of an epic (optional layer; use it when an epic is big).
- **Story** — a single, deliverable, testable increment.

Value lives at the **epic** level (what outcome, measured how). Stories inherit that context via traceability.

## Traceability

Every story links to:
- its **epic** (and feature, if used), and
- its **source requirement** — the doc heading, Confluence section, or work-item ID it came from.

Populate the tracker's structured link field, not just prose in the description.
