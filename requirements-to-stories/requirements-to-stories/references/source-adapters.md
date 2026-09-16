# Source Adapters — reading requirements from anywhere

The pipeline's first job is to turn *whatever the person gives you* into **clean, structured text** (headings, paragraphs, lists, tables preserved). Everything downstream works on that text. This file explains how each source is handled and how to add a new one.

## Principle: detect → extract → normalise

1. **Detect** the source type (file extension, a URL's host, or pasted text).
2. **Extract** the text and tables using the right method for that type.
3. **Normalise** into a simple structure: a title, ordered sections with headings, and any tables as Markdown. Keep the **original terminology and IDs** intact — never rename.

If a source can't be reached, **don't guess** — ask the person to paste the content or upload the file.

---

## Uploaded documents (always works, no connectors needed)

| Format | How to extract |
|---|---|
| Word `.docx` | Read text, headings and tables. (In an assistant with a docx capability, use it; otherwise convert to text.) |
| PDF | Extract text; for a scanned/image PDF, OCR it first. Preserve tables. |
| Excel `.xlsx` / `.csv` | Read each sheet; treat rows as records. Good when requirements are already a list. |
| Text / Markdown | Use directly. |
| Image / screenshot | OCR or read the image to recover the requirement text. |

This is the **safe default source** — anyone who downloads the skill can use it with nothing connected.

## Live sources (use the user's own connected tools)

These depend on the tools the person already has connected in their assistant. The skill uses them **if present**; if not, it falls back to "export/paste/upload".

| Source | Preferred path | Fallback |
|---|---|---|
| **Confluence** | Read the page via a connected Confluence/Atlassian tool. | Ask the person to paste the page or export it to PDF/Word and upload. |
| **SharePoint** | Read the file via a connected SharePoint/Microsoft Graph tool. | Ask them to download the file and upload it. |
| **Azure DevOps** | Read the work item / wiki page / query via a connected ADO tool. | Ask them to copy the work item text or export a query to CSV. |
| **Jira** | Read the issue / filter via a connected Jira tool. | Ask them to paste the issue or export to CSV. |

**Do not build or assume credentials.** The skill never stores secrets or logs in on the person's behalf — it uses whatever connection the person has already authorised in their assistant, or falls back to upload/paste.

## Reading an existing backlog (enhance mode)

When the job is to **extend** a backlog rather than build a new one, the tracker itself is a second source. Before generating, read the current state from the connected Jira/ADO tool:

- Pull the existing **epics and stories** in the target project/board with their **keys, titles and parent links**.
- Use this to (a) slot new stories under the right existing epic, (b) detect and **avoid duplicates** — if a requirement is already covered, say so instead of recreating it, and (c) find **dependencies on existing tickets** to propose as links (see `output-adapters.md`).
- If the tracker can't be read (no connected tool), ask the person to export the current backlog to CSV or paste the epic/story list, and proceed from that.

Keep the existing terminology and IDs — never renumber or rename what's already there.

## Never assume structure

Requirements pages differ enormously between teams — some are neat BRDs, some are meeting notes, some are a table. **Read what's actually there**, then (in the main workflow) *propose* how you'll decompose it and *confirm* that mapping before generating. Do not hardcode expected headings.

---

## How to add a new source adapter

To support another source (e.g. Google Docs, Notion, a wiki):

1. Add a row to the tables above describing the **preferred path** (which connected tool reads it) and the **fallback** (how the person supplies it manually).
2. Make sure your extraction preserves **headings, lists, tables, and original terminology/IDs**.
3. Everything after extraction is unchanged — the analysis, decomposition, generation and quality gate all operate on the normalised text, so no downstream file needs to change.

That's the whole point of the adapter pattern: **new sources plug in at the front; the BA logic never changes.**
