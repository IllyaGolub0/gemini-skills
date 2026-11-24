<!--
Always-on rules template for Gemini CLI, Antigravity, and Gemini Code Assist.

Usage:
1. Copy the sections you need into your project's `GEMINI.md` or your personal
   `~/.gemini/GEMINI.md`, OR
2. Import this file directly in an existing `GEMINI.md` using Gemini CLI memory
   import syntax:
     @path/to/gemini-skills/templates/GEMINI.md
-->

## Technical documentation

For any technical document, spec, or explanatory writeup (design docs, RFCs, ADRs,
API/reference docs, READMEs, test reports), and for technical explanations in chat:
(1) derive, don't assert: decode every symbol, show every number's derivation,
explain each causal step; (2) define every term, convention, and operation before
first use, with no forward references; (3) illustrate geometric/conceptual claims with
figures (computed from real data preferred; draw vectors as vectors); (4) keep it
self-contained, with no load-bearing links to other docs; (5) write so a
non-specialist engineer can follow alongside an expert; (6) render and visually
inspect every figure and PDF before calling it done.

When fixing feedback, sweep the whole document for that *class* of issue in one pass;
never patch only the flagged spot. No em or en dashes as connectives, no status
labels in titles. One idea per sentence, at most 40 words, no semicolon in prose;
every tool and command said what it is at its first use in each chapter; a bare title
page and nominal headings. One writer for the whole text; before delivery, run an
outsider read on each chapter and an audience read on the complete document. Documents
in Ukrainian use the field's vocabulary, not calques.

Activate `writing-technical-docs` (and `reporting-experiments` for measured results,
`documenting-existing-designs` for inherited systems, `typesetting-documents-as-pdf`
for PDF output, `designing-research-studies` for R&D plans, `writing-research-reports`
for theses/R&D reports), or run `/review-doc` for a full audit.

## FPGA, ASIC, and HDL design (optional — remove if not using RTL)

When creating, editing, or reviewing Verilog, SystemVerilog, VHDL, timing
constraints, testbenches, or FPGA synthesis/place-and-route reports:
- Activate `writing-synthesizable-rtl`, `constraining-fpga-designs`, and
  `verifying-fpga-builds` before writing RTL, modifying projects, or reporting build
  status (never report "compile succeeded" without checking constraint warnings,
  inferred resources, and worst-case setup/hold slack).
- Activate the matching HDL discipline skill for Verilog/SystemVerilog rules:
  `avoiding-verilog-races`, `designing-async-fifos`, `crossing-clock-domains`,
  `coding-fsms`, `coding-case-statements`, `using-sv-processes-and-ports`,
  `writing-designer-sva`, or `speeding-up-simulation`.
