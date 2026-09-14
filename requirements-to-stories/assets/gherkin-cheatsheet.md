# Gherkin Cheat-Sheet

A quick reference for writing acceptance criteria in `Given / When / Then`.

## The shape

```
Scenario: [short, specific name for this case]
  Given [the starting context / precondition]
  When  [the action or trigger]
  Then  [the expected, observable outcome]
```

- **Given** — the world before the action (state, data, role, permissions).
- **When** — the single action or event under test.
- **Then** — what must be observably true afterwards (a state, a message, a stored value, a call made).

Add steps with **And** / **But**:

```
Scenario: Payment succeeds
  Given a customer with a valid saved card
  And an order totalling $50.00
  When they confirm the payment
  Then the order status becomes "Paid"
  And a receipt is emailed to the customer
  But no second charge is made if they refresh the page
```

## Rules of thumb

- **One case per scenario.** Don't cram happy path and errors into one scenario.
- **`Then` must be testable.** "Then it works" is not AC. "Then the balance shows $0.00" is.
- **Name the scenario after the case**, not the feature — e.g. "Balance service unavailable", not "View balance".
- **Cover the failures the story type demands** — invalid/empty input, permission denied/revoked, timeout/offline/retry, duplicate/concurrent, stale/expired, cancel mid-flow.
- **Keep steps atomic** — one fact per step.
- Prefer **concrete example values** over vague ones ("$0.00", "within 3 seconds", "a role without approve permission").

## When *not* to use Gherkin

For backend or heavily rule-based stories, plain numbered **rule-based AC** is often clearer:

```
AC1: A duplicate submission with the same idempotency key returns the original result and creates no second record.
AC2: Amounts round to 2 decimal places, half-up.
```

Use whichever makes the condition easiest to test. A story may mix both.
