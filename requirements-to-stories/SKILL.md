---
name: requirements-to-stories
description: Turn requirements from any source into well-formed user stories with Gherkin acceptance criteria, then deliver them to any destination. Use whenever someone provides requirements — an uploaded document (Word, PDF, Excel, image, text), a Confluence / SharePoint / Azure DevOps / Jira link, or pasted notes — and wants user stories, a backlog, epics and features, acceptance criteria, or a Definition-of-Ready check. Also use for "write stories from this BRD", "turn this doc into a backlog", "generate acceptance criteria", "split this into epics and stories", or when a requirements source URL/file appears with a request for stories or tickets. Applies experienced business-analyst thinking: it never invents requirements, and surfaces assumptions, gaps, open questions, actors, business rules, dependencies, NFRs and edge cases before writing.
license: MIT
---

# Requirements → User Stories

Turn requirements from **any source** into well-formed **user stories with acceptance criteria**, and deliver them to **any destination** — a Word document, Markdown, a spreadsheet, or a Jira / Azure DevOps backlog.

This skill is a **pipeline**, not a one-shot generator:

```
SOURCE  →  EXTRACT  →  ANALYSE (BA thinking)  →  DECOMPOSE  →  GENERATE  →  DELIVER
```

It works in two modes: **new backlog** (build epics and stories from scratch) and **enhance existing backlog** (read what's already in the tracker, add new epics/stories without duplicating, and link dependencies to existing tickets). See "Two modes" below.

It is **product-, domain- and vendor-neutral**. It works for a mobile app, a web portal, an API/integration, a data/reporting change or a process change, across any industry. When the product type changes, the *questions* change — the *standard* (story format, INVEST, good acceptance criteria, traceability) does not.

The most important principle: **this skill encodes how an experienced BA thinks, not just how to format a story.** It does not fill silence with invented detail. Where the source is unclear, it says so and asks.

---

## When to use this skill

Use it whenever someone provides requirements and wants them turned into stories, acceptance criteria, or a backlog — regardless of where the requirements come from or where the stories need to go. Typical triggers:

- "Turn this BRD / spec / document into user stories."
- "Build a backlog from this Confluence page / SharePoint doc / ADO work item."
- "Write Gherkin acceptance criteria for this feature."
- "Split this into epics, features and stories."
- "Is this story ready for a sprint?" (Definition-of-Ready check)
- A requirements file or URL appears alongside a request for stories or tickets.

If the person only wants to *explore* or *elicit* requirements (they don't have them yet), that's discovery, not this skill — help them gather first.

---

## Two modes

**New backlog** — the source becomes a fresh set of epics and stories.

**Enhance existing backlog** — the person already has epics/stories in a tracker and wants to add more (a later phase, a change request, a new feature). In this mode the skill **first reads the current backlog** from the connected tracker, then:
- slots new stories under the **right existing epic** (or proposes a new epic only where none fits);
- **avoids duplicating** tickets that already exist — if something is already covered, it says so instead of recreating it;
- reuses the existing **keys, naming and terminology**; and
- detects **dependencies on already-created tickets** and proposes links (see step 7 and `references/output-adapters.md`).

Detect the mode from the request ("build a backlog" = new; "add to / extend / next phase of the existing backlog" = enhance). If a tracker is connected and the target project already has items, ask which mode is intended before generating.

---

## Inputs it accepts

**Any source** — the skill detects the source type and extracts clean text before doing anything else. See `references/source-adapters.md` for exactly how each is handled and how to add a new one.

| Source | How it's used |
|---|---|
| Uploaded document | Word (.docx), PDF, Excel (.xlsx/.csv), plain text/Markdown, or an image/screenshot of requirements — text (and tables) are extracted. |
| Confluence page | Read via the user's connected Confluence tool if available; otherwise the user pastes the content or exports it. |
| SharePoint document | Read via the user's connected SharePoint/Graph tool if available; otherwise the file is downloaded/uploaded. |
| Azure DevOps | A work item, wiki page, or query — read via the user's connected ADO tool if available. |
| Jira | An issue or filter — read via the user's connected Jira tool if available. |
| Pasted text / meeting notes | Used directly. |

The skill **never assumes a fixed document structure.** Requirements pages differ wildly between teams. It reads what's actually there, proposes how it will decompose it, and confirms that mapping before generating (see workflow step 3).

## Outputs it produces

**Any destination** — see `references/output-adapters.md`.

| Destination | Notes |
|---|---|
| **Word document (.docx)** | Always available, zero setup. A clean, review-ready story pack. This is the safe default. |
| Markdown | For pasting into a wiki, repo, or PR. |
| Spreadsheet (.xlsx/.csv) | One row per story — handy for import or bulk review. |
| **Jira** | Epics/features/stories created via a **dry-run → approve → create** flow. Requires the user's connected Jira tool. |
| **Azure DevOps** | Same dry-run → approve → create flow. Requires the user's connected ADO tool. |

**Golden rule for any tracker destination: dry-run first.** Show the full backlog for approval and only create tickets after the user confirms. Never write to a tracker silently.

---

## Guardrails — the experienced-BA rules (apply on every run)

These are what make the output trustworthy rather than generic. Apply them every time, whatever the source or destination:

1. **Never invent requirements.** If it isn't supported by the source, it doesn't become a requirement. Missing detail becomes an *open question* or a clearly labelled *assumption* — never a silently fabricated fact.
2. **Separate what's given from what's inferred.** Every assumption is flagged as an assumption; every gap is flagged as a gap.
3. **Surface open questions before writing stories.** List what's ambiguous or missing first, so gaps are closed by people, not by the model.
4. **Identify the actors / personas / roles** the requirement involves, and check roles & permissions.
5. **Identify business rules** and state them explicitly, separate from system behaviour.
6. **Identify the source-of-truth system** for each piece of data.
7. **Identify upstream and downstream dependencies** and integration impacts (APIs, data, other teams/systems).
8. **Separate business requirements from system behaviour** — a business need is not the same as a UI behaviour.
9. **Cover edge cases and failure scenarios**, not just the happy path.
10. **Preserve the organisation's own terminology.** Use the source's names for things; don't rename or "tidy" domain terms.
11. **Find gaps before creating stories**, and **avoid duplicate or unnecessary requirements.**
12. **Maintain traceability** — every story links back to its source requirement (page section, doc heading, work item ID).

If a run would violate any of these to look more "complete," stop and ask instead.

---

## Workflow (the steps to follow every time)

**1. Ingest the source (and, in enhance mode, the existing backlog).** Detect the source type and extract clean text and tables. If the source can't be reached (no connected tool, broken link), ask the person to paste it or upload the file rather than guessing. **In enhance mode, also read the current backlog** from the tracker (existing epics, stories and their keys) so new work can be slotted in without duplication. See `references/source-adapters.md`.

**2. Analyse before decomposing.** Read the whole thing first. Produce a short **analysis pass** *before* any stories:
   - Actors/roles involved
   - Business rules found
   - Source-of-truth systems and key data
   - Dependencies & integration impacts
   - **Open questions** (what's ambiguous or missing)
   - **Assumptions** (clearly labelled)
   - Anything that looks like a gap or a contradiction

   Show this to the person. This is the single highest-value part of the skill — it's the BA thinking, made visible.

**3. Propose the decomposition and confirm it.** Suggest an **Epic → Feature → Story** breakdown mapped to the source (which section becomes which epic/story). **Confirm this mapping with the person before generating** — do not assume the source's headings map 1:1 to epics. Adjust to their feedback.

**4. Ask the story-type questions.** Work out what *kind* of story each item is (data entry, workflow/status, integration/API, notification, auth/access, permission/consent, background/async, reporting, install/migration, UI/interaction — they combine) and ask the specific questions that type always needs. Prompt for real values; don't invent them. See `references/story-type-questions.md`.

**5. Detail the epics, then generate the stories + acceptance criteria.**
   - **Epics** — detail each epic to `references/epic-standard.md`: epic statement, background, **measurable business value (baseline → target)**, scope in/out, epic-level business rules, source-of-truth data, dependencies, child stories, and traceability. Value at the epic level is mandatory and must be measurable — if the number isn't known, it's an open question, not an invented figure.
   - **Stories** — use `references/user-story-standard.md`: *As a [role], I want to [capability], so that [value]*; a complete story (Background, story sentence, Business Rules, AC, Out of Scope, References); **Gherkin** AC by default for interactive/UI stories and **rule-based** AC for backend/rule-heavy stories, covering happy path **and** known edge/failure cases; checked against **INVEST**.

**6. Run the quality gate.** Before delivering, run each epic/story through `references/quality-gate.md` (structure complete, INVEST + estimable, value & measurable success, NFRs considered where relevant, edge/failure coverage, traceability, explicit scope, internal consistency). Report each as pass/gap; close gaps or capture them as open questions — don't hide them.

**7. Map dependencies, then deliver.**
   - **Dependencies** — check each new story/epic against the rest of the new set **and** (in enhance mode) against the existing backlog. Where one genuinely depends on another, capture it as a proposed link with a one-line reason ("US-114 *is blocked by* US-090 because it consumes the token US-090 issues"). **Propose these in the dry-run — never create dependency links silently.**
   - **Deliver** — Word doc by default. For Jira/ADO, always **dry-run → approve → create**, and only on approval create the tickets *and* the approved dependency links (Jira issue links such as *blocks / is blocked by / relates to*; ADO Predecessor/Successor/Related). See `references/output-adapters.md`.

---

## Customising for a team (house-standard overlay)

The skill ships **generic on purpose** so anyone can use it. A team that has its own house standard (its own story template, Definition of Ready/Done, mandatory fields, naming) does **not** fork the skill — it adds a single overlay file and points the skill at it:

- Create `references/house-standard.md` in your copy with your format, DoR/DoD, required fields and terminology.
- The skill applies the generic standard **plus** your overlay, with the overlay winning on any conflict.

This keeps the public skill clean while letting each team make it their own. See the note at the top of `references/user-story-standard.md`.

---

## Reference files

- `references/user-story-standard.md` — the generic story format, INVEST, AC formats, worked examples, and the house-standard overlay hook.
- `references/epic-standard.md` — how to detail an epic to a standard (core vs optional fields) + a worked example.
- `references/quality-gate.md` — the quality gate + Definition of Ready checklist.
- `references/story-type-questions.md` — the question sets per story type.
- `references/source-adapters.md` — how each source is ingested, and how to add a new source.
- `references/output-adapters.md` — how each destination works, the dry-run→approve→create flow, and how to add a new destination.
- `assets/user-story-template.md` — blank fill-in story template.
- `assets/gherkin-cheatsheet.md` — quick Gherkin reference.
- `examples/` — worked examples on a fictional domain: input brief → analysis → stories (`01`, `02`), and a fully-detailed epic (`03`).
