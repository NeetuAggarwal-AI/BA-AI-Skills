# Quality Gate & Definition of Ready (generic)

Run this on **every epic and story** — whether drafting or reviewing. For a review, mark each item **pass** or **gap**. For a draft, either close the gap now or capture it as an **open question**. Never hide a gap to make the output look finished.

The gate makes you *ask the question*; it never hardcodes the *answer*. Targets and values come from stakeholders and context, not from the model.

## The gate

1. **Structure complete** — Background, story sentence, Business Rules, Acceptance Criteria, Out of Scope, References are each present (or explicitly N/A) as their **own sections**, not buried in notes.

2. **INVEST + estimable** — meets INVEST and is small enough to size. If not, split it and say how.

3. **Value & measurable success** — business value is quantified at the **epic** level (baseline + target), and there's an **observable success measure / telemetry** captured *as* AC — how we'll prove it works once live. The target says what "good" looks like; the telemetry is the logging/instrumentation that lets us measure it.

4. **NFRs — considered every time, asked only where relevant.** Glance across the NFR categories — performance/responsiveness, security, privacy & data handling/retention, accessibility, reliability/availability, scalability, usability, compliance, localisation, observability/telemetry. Decide which genuinely apply to *this* story and ask only those, for their real target values. Don't blanket-ask all of them; equally, don't silently drop them — if none apply, say so briefly.

5. **Edge / failure coverage beyond happy-path + one negative.** Check the failure modes for the story type: invalid/empty/boundary input, permission or auth denied *or later revoked*, offline/timeout/retry, duplicate/concurrent, stale/expired, cancel or interruption mid-flow, unsupported version/environment.

6. **Traceability** — linked to its epic **and** to the source requirement. Structured link field populated, not left blank with content only in the description.

7. **Scope boundaries explicit** — where a behaviour starts and stops is stated (what's in vs out), not left implied.

8. **Internal consistency** — Background, Business Rules, AC and Notes agree with each other; flag any contradiction.

## Definition of Ready — quick checklist (before a story enters a sprint)

- [ ] Meets INVEST
- [ ] Estimated
- [ ] Design signed off (if UI-relevant)
- [ ] Downstream/impacted services and dependencies known — no external roadblocks
- [ ] AC is clear and testable (happy path + edge cases)
- [ ] NFRs defined where applicable
- [ ] Business value stated and tied to an objective
- [ ] Supporting context attached (mockups, architecture notes, business rules)

If a story fails several of these, **don't just flag it — help close the gaps**: ask the clarifying questions, draft the missing AC, and note what's still open.

## Marking gaps, never hiding them

- Where a required section has no source material yet, keep the heading and mark it `TBC — [what's pending]` — don't delete the heading and don't invent content to fill it.
- An item that doesn't pass the gate still goes in the output, marked **NOT READY** with the specific gap named. A hidden gap is worse than a visible one.
