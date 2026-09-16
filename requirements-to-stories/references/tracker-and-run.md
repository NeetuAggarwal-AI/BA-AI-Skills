# Tracker & Run Mechanics

Generic practices for running the pipeline reliably against a tracker (Jira, Azure DevOps, or any similar tool). These are vendor-neutral — they describe *how to behave*, not any one team's format. (A team's specific ticket format lives in its own `house-standard.md` overlay, not here.)

---

## 1. Resolve before you ask

Don't open with a list of questions. First work out what you already know, then ask once for only what's genuinely missing.

Look in three places, in order:

1. **What the person just said** — a source link, a tracker/project key, or a board URL in the request wins over anything stored.
2. **Any settings already saved for this work** — if the workspace/project holds notes or a saved configuration from a previous run (source location, target board, naming, the role noun, an ID pattern), reuse it rather than re-asking.
3. **The surrounding context** — the project/workspace description, instructions, or attached docs often already state the source, the target board, and output conventions.

Treat anything you find as a **candidate, not a fact**. If two sources or two boards are named, ask which — never guess between them. Then ask once, showing what you found next to what's missing, so the person is *confirming*, not retyping:

> I have the source (the requirements doc) and the target board. Still needed: the role noun for the "As a ___" line. Which should it be?

If the person confirms settings that are worth reusing next time, you may offer to remember them so a repeat run asks less — only if the person wants that.

---

## 2. Discover the tracker hierarchy before promising one

Before proposing an Epic → Feature → Story tree, check what levels the **target tracker actually supports** — many projects have only two tiers, not three. Don't silently flatten a three-level plan into a two-level board.

If there's no middle (Feature) tier, offer a choice rather than deciding alone:

1. **Two-tier** — Epic → Story; the feature grouping becomes a label/component/tag.
2. **Shift down** — each Feature becomes an Epic; the source section becomes the grouping label.
3. **Sub-tasks** — Epic → Story → Sub-task, if the third level is task breakdown rather than product features.

Confirm the choice once, and carry it through the rest of the run.

---

## 3. Idempotent creation & safe re-runs

This is the concrete mechanism behind **enhance-existing-backlog** mode: a run must be safe to repeat without creating duplicates.

- **Tag what you create.** When items are created, give them a consistent marker (a label/tag tied to the source, e.g. a source identifier) so a later run can recognise them.
- **On any run, look before you create.** Search the target for that marker (and match on title/source-ID where possible). If matching items already exist, this is a re-run or an enhancement — **create only what's new, update what changed, and leave the rest alone.** Never blind-create against a source that was processed before.
- **Report the reconciliation** in the dry-run: what's new, what already exists, what will be updated.

---

## 4. Incremental create, with failure recovery

When the dry-run is approved and you write to the tracker:

- Create **parents before children** (epics first, capture their IDs, then stories linked to them; then any approved dependency links).
- Create **incrementally and report progress**, rather than all-at-once with no visibility.
- If a create fails partway, **name exactly which items were created and which were not**, so a re-run (protected by the idempotency in §3) resumes cleanly instead of duplicating.
- Leave workflow/status fields (e.g. "Ready", sprint, assignee, estimate) **unset unless asked** — those record human decisions, not something to set automatically.

---

These four practices apply on top of the golden rule in `output-adapters.md`: **nothing is written to a tracker — tickets or links — until the person approves the dry-run.**
