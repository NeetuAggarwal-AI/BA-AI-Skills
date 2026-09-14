# requirements-to-stories

**An AI Agent Skill that turns requirements from any source into well-formed user stories with Gherkin acceptance criteria — and delivers them to any destination.**

Built for Business Analysts, Technical BAs and Product Owners. It encodes experienced BA thinking, not just story formatting: it never invents requirements, and it surfaces assumptions, gaps, open questions, actors, business rules, dependencies, NFRs and edge cases *before* it writes.

```
SOURCE  →  EXTRACT  →  ANALYSE (BA thinking)  →  DECOMPOSE  →  GENERATE  →  DELIVER
```

## What it does

- **Any source in** — an uploaded document (Word, PDF, Excel, image, text), a Confluence / SharePoint / Azure DevOps / Jira link, or pasted notes.
- **Experienced-BA analysis** — actors, business rules, source-of-truth systems, dependencies, open questions and assumptions, made visible before any story is written.
- **Epics + stories, detailed to a standard** — Epic → Feature → Story, with epics detailed to an epic standard (measurable value, scope, rules, dependencies) and stories with Gherkin (or rule-based) AC covering happy path *and* edge/failure cases, checked against INVEST and a quality gate.
- **Two modes** — build a **new backlog**, or **enhance an existing one**: read what's already in the tracker, add new epics/stories without duplicating, and link dependencies to existing tickets.
- **Dependency mapping** — detects dependencies between new items and existing tickets, proposes them (with reasons) in the dry-run, and creates the links (Jira / ADO) only on approval.
- **Any destination out** — a Word document (default, no setup), Markdown, a spreadsheet, or a Jira / Azure DevOps backlog via a **dry-run → approve → create** flow.

## How it's structured

```
requirements-to-stories/
├── SKILL.md                          # the skill: triggers, inputs, outputs, guardrails, workflow
├── README.md                         # this file
├── references/
│   ├── user-story-standard.md        # story format, INVEST, AC formats, house-standard overlay hook
│   ├── epic-standard.md              # how to detail an epic (core vs optional fields) + example
│   ├── quality-gate.md               # quality gate + Definition of Ready
│   ├── story-type-questions.md       # question sets per story type
│   ├── source-adapters.md            # how each source is read (incl. reading an existing backlog)
│   └── output-adapters.md            # destinations; dry-run→approve→create; dependency linking
├── assets/
│   ├── user-story-template.md        # blank fill-in template
│   └── gherkin-cheatsheet.md         # quick Gherkin reference
└── examples/
    ├── 01-input-brief.md             # a fictional requirements brief
    ├── 02-output-analysis-and-stories.md   # the analysis + stories it produces
    └── 03-epic-example.md            # a fully-detailed epic + proposed dependency links
```

## Design principles

- **Vendor- and domain-neutral.** Works for web, mobile, API, data or process change, in any industry.
- **Adapter pattern.** New sources plug in at the front and new destinations at the back; the BA logic in the middle never changes.
- **Dry-run before any tracker write.** Nothing is created in Jira/ADO — tickets *or* dependency links — until you approve the full proposed backlog.
- **House-standard overlay.** A team layers its own template/DoR via a single `references/house-standard.md` file — no fork needed.

## Using it

Point your AI assistant (one that supports Agent Skills / SKILL.md) at this folder, then give it a requirements source and ask for stories — e.g. *"Turn this document into a backlog"* or *"Write Gherkin acceptance criteria from this Confluence page."*

Live sources (Confluence, SharePoint, ADO, Jira) and tracker destinations use whatever tools **you** have connected in your assistant; if none are connected, the skill falls back to upload/paste in and a document out, so it works with zero setup.

## License

MIT — see `LICENSE`.
