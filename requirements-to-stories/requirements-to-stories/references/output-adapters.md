# Output Adapters — delivering stories anywhere

Once the stories exist, deliver them to whatever destination the person wants. The stories themselves are the same object every time; only the rendering/writing differs.

## Principle: one story model, many renderers

Hold each story as a structured object (title, role, capability, value, background, business rules, AC, out-of-scope, references, links to epic/source). Each destination is just a way of **rendering or writing** that object.

---

## Document destinations (always work, no connectors)

| Destination | Notes |
|---|---|
| **Word `.docx`** | The safe default. A clean, review-ready story pack: one section per epic, stories underneath, AC formatted, a summary of assumptions/open questions at the front. |
| **Markdown** | For a wiki, repo, or PR description. |
| **Spreadsheet `.xlsx` / `.csv`** | One row per story (Epic, Story, Role, Value, AC, Priority, Source link). Ideal for bulk review or for a tool's CSV import. |

Default to the **Word document** unless the person asks otherwise — it needs nothing connected and is the most review-friendly.

## Tracker destinations (use the user's connected tools) — dry-run first, always

| Destination | Path |
|---|---|
| **Jira** | Create epics → (features) → stories via a connected Jira tool. |
| **Azure DevOps** | Create epics → features → stories/PBIs via a connected ADO tool. |

### The mandatory flow for any tracker

1. **Dry-run.** Produce the *complete* proposed backlog as a document or on-screen list: the full hierarchy, every story with its AC, and where each will be created (project/board, issue types, parent links).
2. **Review.** The person reads it and edits. Nothing has been created yet.
3. **Approve.** Only on an explicit "yes, create these" do you write to the tracker.
4. **Create + report.** Create the items, then report back the created IDs/links and the epic→story parentage so it's traceable.

**Never write to a tracker silently, and never create tickets in the same step as generating them.** This is the single most important rule for tracker output — it protects the person from a messy board they didn't sign off on.

### Field mapping

- Map to the tracker's **structured fields** (summary/title, description, acceptance criteria field, parent/epic link, labels/components), not just a wall of text in the description.
- Put the **source-requirement link** in a structured link/label field for traceability.
- If the person has a **house-standard overlay** (`references/house-standard.md`) with required fields, honour those on create.

---

## Dependency mapping & linking (detect → propose → create)

New stories often depend on each other, and in **enhance mode** on tickets that already exist. Handle dependencies as a first-class step, with the same protection as ticket creation.

1. **Detect.** Compare each new story/epic against the rest of the new set **and** the existing backlog. Look for: one story consuming what another produces; a story that can't start until an epic/story is delivered; shared data or a shared integration where order matters; a child that needs a parent that doesn't exist yet.
2. **Propose in the dry-run.** Present a small **dependency table** — *from → relationship → to*, plus a **one-line reason** for each — as part of the dry-run the person reviews. Example:

   | From | Relationship | To | Why |
   |---|---|---|---|
   | NU-102 | is blocked by | NU-101 | Duplicate-charge handling builds on the base payment flow |
   | NU-EP-03 | is blocked by | NU-EP-01 | Customers must be able to log in before they can pay |

3. **Create on approval only.** After the person approves, create the links along with the tickets:
   - **Jira** — issue links: *blocks / is blocked by*, *relates to*, *depends on* (per the project's link types).
   - **Azure DevOps** — *Predecessor / Successor* or *Related* work-item links.
   For a **document** destination (Word/Markdown/CSV), render the dependency table instead of creating live links.

**Never create dependency links silently.** A wrong "is blocked by" can quietly stall a sprint and is hard to unpick — so dependencies are always proposed with reasoning and confirmed before they're written, exactly like the tickets themselves.

---

## How to add a new destination

1. Add a row above with either the document renderer or the connected tool it uses.
2. Reuse the same story object — only add rendering/field-mapping logic.
3. If it's a system that persists (a tracker, a wiki), it **must** use the dry-run → approve → create flow. Read-only document exports don't need approval.
