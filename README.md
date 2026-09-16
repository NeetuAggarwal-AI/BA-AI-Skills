# BA-AI-Skills

**Reusable AI Agent Skills for Business Analysis & Product Ownership** — for BAs, POs, and any developer, tester, architect or engineer wearing the BA/PO hat.

These are packaged, reusable skills that encode *experienced* BA thinking — not just output formatting. They help you do real analysis work: turn raw requirements into well-formed epics and user stories, write solid acceptance criteria, map dependencies, and keep everything traceable — while surfacing assumptions, gaps and open questions instead of inventing detail.

Each skill is an [Agent Skill](https://docs.claude.com): a `SKILL.md` file of instructions plus supporting references, templates and examples, designed to be used by an AI assistant that supports skills.

---

## Why this exists

Most AI "story generators" happily invent requirements to look complete. That's the opposite of good BA practice. Every skill here is built on a set of guardrails:

- Never invent requirements — missing detail becomes a labelled assumption or an open question.
- Identify actors, business rules, source-of-truth systems, dependencies and integration impacts.
- Separate business requirements from system behaviour.
- Cover edge cases and failure scenarios, not just the happy path.
- Preserve the organisation's own terminology.
- Keep full traceability from source requirement → epic → story → acceptance criteria.

The result is output you can actually take into a refinement session, not a first draft you have to unpick.

---

## The vision

The long-term goal is a set of skills that chain together into a full specification pipeline:

```
Workshop transcript
   → Workshop analysis
   → Requirements extraction
   → Gap analysis
   → Business rules
   → Open questions
   → Epics / Features
   → User stories
   → Gherkin acceptance criteria
   → API / integration requirements
   → Process models
   → Specification / BRD
   → Jira / ADO backlog
```

Each skill does one job well and produces output the next one can pick up — so you can run a single step, or the whole pipeline.

---

## Skills

| Skill | What it does | Status |
|---|---|---|
| [requirements-to-stories](./requirements-to-stories) | Turn requirements from **any source** (Word/PDF/Excel/image, Confluence, SharePoint, ADO, Jira, or pasted text) into epics and user stories with Gherkin acceptance criteria, and deliver them to **any destination** (Word, Markdown, spreadsheet, Jira or ADO). Includes an epic standard, an "enhance existing backlog" mode, and dependency mapping. | ✅ Available |
| Requirements Discovery | Structure discovery: elicitation questions, stakeholder and scope framing. | 🔜 Planned |
| Workshop Transcript Analysis | Turn a raw workshop/meeting transcript into structured findings, decisions and actions. | 🔜 Planned |
| Requirement Gap Analysis | Compare requirements against a target and surface what's missing or ambiguous. | 🔜 Planned |
| Gherkin AC Generator | Generate/expand Gherkin acceptance criteria for a given story. | 🔜 Planned |
| API / Integration Analysis | Draw out API, payload and integration requirements from a feature. | 🔜 Planned |
| Source-to-Target Mapping | Build and validate data/field mappings between systems. | 🔜 Planned |
| Process Modelling | Produce BPMN/UML-style process models from described flows. | 🔜 Planned |
| BRD / Specification Generation | Assemble a structured BRD/spec from gathered requirements. | 🔜 Planned |
| Backlog Quality Review | Review an existing backlog against a quality gate and Definition of Ready. | 🔜 Planned |

---

## How to use a skill

1. Open the skill's folder (e.g. [`requirements-to-stories`](./requirements-to-stories)) and read its `SKILL.md`.
2. Point an AI assistant that supports Agent Skills at the folder.
3. Give it a requirements source and ask for what you need — e.g. *"Turn this document into a backlog"* or *"Write Gherkin acceptance criteria for this feature."*

Skills are **vendor-neutral** and work with zero setup: live sources (Confluence, SharePoint, ADO, Jira) and tracker destinations use whatever tools *you* have connected, and fall back to file-upload in / document out when nothing is connected. Teams can add a `house-standard.md` overlay to apply their own templates and Definition of Ready/Done without forking.

---

## License

[MIT](./requirements-to-stories/LICENSE) — free to use, adapt and share, with attribution retained.

---

*Built and maintained by Neetu Aggarwal — applying AI and agentic workflows to modern Business Analysis and Product Ownership.*
