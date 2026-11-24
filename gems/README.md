# Using `gemini-skills` in Plain Gemini (Web, Gems, AI Studio, Code Assist)

You do not need Gemini CLI to use the engineering standards and checklists in this repository. Use any of the options below directly in the Gemini web app, Gems, Google AI Studio, or IDE chat.

---

## 1. Gemini Gems (gemini.google.com)

Create a reusable custom Gem per engineering domain:

1. Open [Gemini](https://gemini.google.com) -> **Explore Gems** (Gem Manager) -> **New Gem**.
2. Name the Gem and paste the contents of one of the instruction packs below into **Instructions**:

| Gem Pack | File to Paste into Instructions | Optional Files to Upload as Gem Knowledge |
|---|---|---|
| **Technical Docs & Research** | [`docs-standards-gem.md`](docs-standards-gem.md) | [`ukrainian-engineering-prose.md`](../skills/writing-technical-docs/references/ukrainian-engineering-prose.md), [`outsider-reader-brief.md`](../skills/writing-technical-docs/references/outsider-reader-brief.md), [`audience-read-brief.md`](../skills/writing-technical-docs/references/audience-read-brief.md), [`research-design-template.md`](../skills/designing-research-studies/references/research-design-template.md), [`report-structure-and-checklist.md`](../skills/writing-research-reports/references/report-structure-and-checklist.md), [`literature-review-protocol.md`](../skills/writing-research-reports/references/literature-review-protocol.md), [`block-diagrams.md`](../skills/documenting-existing-designs/references/block-diagrams.md) |
| **FPGA & Timing Design** | [`fpga-design-gem.md`](fpga-design-gem.md) | [`quartus-toolchain.md`](../skills/constraining-fpga-designs/references/quartus-toolchain.md) |
| **Verilog / SystemVerilog HDL** | [`sunburst-hdl-gem.md`](sunburst-hdl-gem.md) | Individual `SKILL.md` files from `skills/` when deep examples or benchmark tables are needed |

3. Save the Gem and use it whenever drafting, reviewing, or debugging in that domain.

---

## 2. Plain Gemini Chat (One-Off Task)

If you only need one specific skill (for example, `writing-technical-docs`, `designing-async-fifos`, or `typesetting-documents-as-pdf`):

1. Attach or paste the corresponding `skills/<skill-name>/SKILL.md` (and any file from `skills/<skill-name>/references/` if needed) directly into your Gemini chat prompt.
2. Ask Gemini to follow the attached skill instructions for your document, code, or build log.

---

## 3. Google AI Studio (aistudio.google.com)

1. Open **System Instructions** in AI Studio.
2. Paste the contents of [`docs-standards-gem.md`](docs-standards-gem.md), [`fpga-design-gem.md`](fpga-design-gem.md), or [`sunburst-hdl-gem.md`](sunburst-hdl-gem.md) (or a specific `skills/<skill-name>/SKILL.md`).

---

## 4. Workspace Rules (`GEMINI.md`) for Gemini Code Assist & IDE Chat

Copy [`../templates/GEMINI.md`](../templates/GEMINI.md) into the root of your repository as `GEMINI.md` (or into `~/.gemini/GEMINI.md` for all personal projects) so the core writing and hardware rules stay active automatically across chats.
