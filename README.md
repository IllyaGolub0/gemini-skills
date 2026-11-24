# gemini-skills

Engineering discipline skills, custom commands, always-on rules, and ready-to-paste Gem instruction packs for **Gemini CLI**, **plain Gemini (Web, Gems, AI Studio, Code Assist)**, and **Antigravity**.

The repository packages seventeen skills, one slash command (`/review-doc`), a `GEMINI.md` rules template, and three domain Gem packs covering technical documentation, research design and reporting, PDF typesetting, FPGA/ASIC design, and Verilog/SystemVerilog coding.

| Domain | Contents | What it enforces |
|---|---|---|
| [Technical Documentation & Research](#technical-documentation--research) | 6 skills, `/review-doc` command | Technical documents and chat explanations that derive every claim, define every term before use, and verify every figure. Experiment reports, PDF typesetting, analysis of inherited designs, research designs with falsifiable hypotheses, and theses/R&D reports written from artefacts. |
| [FPGA & ASIC Design](#fpga--asic-design) | 3 skills | RTL that synthesises into the intended hardware (block RAM, DSP, latches, resets, CDC), mandatory SDC/XDC timing constraints on every build, and build reports audited across four gates rather than trusted on "0 errors." |
| [Verilog & SystemVerilog HDL](#verilog--systemverilog-hdl) | 8 skills | Semantic coding rules distilled from Clifford E. Cummings' Sunburst Design papers: race-free coding, async FIFOs, clock-domain crossings, FSM styles, case-statement safety, SV processes/ports, designer SVA bindfiles, and simulation speed. |

---

## Install & Usage

Choose the setup matching how you run Gemini.

### 1. Gemini CLI — Full Extension (Recommended)

Install the entire extension (all 17 skills plus the `/review-doc` command) directly from GitHub:

```bash
gemini extensions install https://github.com/IllyaGolub0/gemini-skills
```

Update later to pull new versions:

```bash
gemini extensions update gemini-skills
```

For local editing and testing, link a local clone instead so changes take effect immediately:

```bash
git clone https://github.com/IllyaGolub0/gemini-skills.git
cd gemini-skills
gemini extensions link .
```

Inside an interactive Gemini CLI session, verify the installation with `/extensions list`, `/skills list`, and `/commands list`.

### 2. Gemini CLI — Individual Skills

Install all skills or pick individual skills at `user` (global) or `workspace` (project-local) scope using `--path`:

```bash
# Install all 17 skills globally (~/.gemini/skills/)
gemini skills install https://github.com/IllyaGolub0/gemini-skills --path skills

# Install a single self-contained skill into the current workspace (.gemini/skills/)
gemini skills install https://github.com/IllyaGolub0/gemini-skills --path skills/writing-technical-docs --scope workspace
```

To link skills from a local clone during development:

```bash
gemini skills link ./skills
```

### 3. Plain Gemini (Web, Gems, Google AI Studio, Code Assist)

You do not need the CLI to apply these rules. Ready-to-paste System Instruction packs live in [`gems/`](gems/):

- [`gems/docs-standards-gem.md`](gems/docs-standards-gem.md): Technical writing, experiment reports, existing design reports, research plans, theses, and PDF typesetting checks
- [`gems/fpga-design-gem.md`](gems/fpga-design-gem.md): Synthesisable RTL, SDC/XDC constraints, and the 4-gate build verification workflow
- [`gems/sunburst-hdl-gem.md`](gems/sunburst-hdl-gem.md): Race-free Verilog/SystemVerilog, async FIFOs, CDC, FSMs, case statements, SV processes/ports, SVA bindfiles, and simulation speed

See [`gems/README.md`](gems/README.md) for step-by-step instructions on creating Gemini Gems, pasting into Google AI Studio, attaching single `SKILL.md` files in plain chat, and configuring Gem Knowledge files.

### 4. Antigravity (IDE, App, and `agy` CLI)

This repository includes a native root [`plugin.json`](plugin.json) alongside `skills/`:

```bash
git clone https://github.com/IllyaGolub0/gemini-skills.git
agy plugin install ./gemini-skills
```

You can also place or link the repository at `~/.gemini/config/plugins/gemini-skills` (global) or `.agents/plugins/gemini-skills` (workspace), or copy selected directories from `skills/` into `.agents/skills/`.

### 5. Always-on Rules (`GEMINI.md`)

On-demand skills can under-trigger when a model treats a plain first-draft request (such as "write me a design doc" or "write up the test results") as something it can answer directly. On the trigger evals bundled with the documentation skills, `writing-technical-docs` fired reliably on review/fix prompts and `/review-doc`, and fired on roughly one in four plain drafting prompts; `reporting-experiments` fired on roughly one in five plain "write up the results" prompts.

To make the rules apply on **every** draft automatically, copy [`templates/GEMINI.md`](templates/GEMINI.md) into your repository's `GEMINI.md` or your global `~/.gemini/GEMINI.md` (or import it in Gemini CLI using `@path/to/gemini-skills/templates/GEMINI.md`):

```markdown
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
```

The full template in [`templates/GEMINI.md`](templates/GEMINI.md) also includes an optional HDL/FPGA rule block.

---

## Technical Documentation & Research

Six skills and the `/review-doc` custom slash command.

| Skill | Fires when | What it does |
|---|---|---|
| `writing-technical-docs` | Writing, reviewing, or fixing a design doc, spec, README, report, or explaining concepts in chat | Enforces thirteen standards: derive rather than assert, define before first use (notation tables with code names, every tool and command defined at first use per chapter), illustrate concepts, self-contained prose, two-audience readability, verified visuals, concrete artefacts shown, generated numbers and cross-references, propagated corrections, zero connective em/en dashes, reader-facing bare title pages and nominal headings, one concrete running example, and named/anchored pointers. Includes sentence rules (one idea, at most 40 words, no semicolon in prose, noun next to pronoun), outsider/audience reader briefs, and a Ukrainian engineering-prose reference. |
| `reporting-experiments` | Writing up measured results: comparisons, benchmarks, A/B tests, sweeps, verification campaigns | Defines the 8-part report shape: derived conditions, derived measurement ceiling, calibrated metrics, configurations table, results with single-variable controls before claimed mechanisms, "what the test does not show", numbered conclusions tied to rows, runnable reproduction block, defect ledgers (`FIX(ID)`), and sequential timestamped runs bound to commit hashes. |
| `typesetting-documents-as-pdf` | Converting Markdown/text to PDF or diagnosing broken PDF layout | Picks the toolchain from document contents, measures widest fenced lines and table rows first (`measure_source.py`), prevents table overflow (`tablewidths.lua`), fixes rigid inline code (`codebreaks.lua`), smart quotes, orphaned headings (`nobottomtitles*`), and silently dropped glyphs. Ships `inspect_pdf.py`, `contact_sheet.py`, `check_section_refs.py`, `check_sentences.py`, `check_word_gaps.py`, `header.tex`, and `build.sh.template`. |
| `documenting-existing-designs` | Describing an inherited codebase or hardware design, block diagrams, partial checkouts | Requires a script-generated inventory before prose, Provenance Tiers (**A** read directly, **B** same-named reference cross-checked, **C** absent source inferred from ports) on every row/block/claim, numbers traced to `file:line`, netlist-derived block diagrams (`references/block-diagrams.md`), anomalies flagged without invented causes, verification against real artefacts with byte coverage accounting, and a closing Verification Statement plus **Not verified** list. |
| `designing-research-studies` | Planning an R&D study, thesis, or benchmark before running experiments | Structures the research design doc (`references/research-design-template.md`): falsifiable hypotheses with refutation criteria, candidate space and numbered sieve thresholds (discards reported as results), checkpoints including a smoke test, record/session splits, condition bands, >= 3 seeds, validation-frozen operating points, shared-code baselines, compute budget with order of cuts, and dated rulings/handoffs. |
| `writing-research-reports` | Writing or revising a thesis, explanatory note, R&D report, paper chapters, or abstract | Builds chapters from artefacts in order (literature review with Gap & Hypotheses tables, reproducible Methods, band-by-band Results with sieve discards, Conclusions/Abstract/Introduction last). Enforces impersonal scientific register, glossary discipline, and a pre-submission checklist (`references/report-structure-and-checklist.md`, `references/literature-review-protocol.md`). |

### `/review-doc [path]`

Defined in [`commands/review-doc.toml`](commands/review-doc.toml) for Gemini CLI. Runs a bounded full-document audit:

1. Reads the complete document first.
2. Runs the chapter-by-chapter outsider read (`skills/writing-technical-docs/references/outsider-reader-brief.md`) and keeps its list separate.
3. Scans every standard and sentence rule, activating `reporting-experiments` for measured results, `documenting-existing-designs` for inherited designs, and `skills/writing-technical-docs/references/ukrainian-engineering-prose.md` for Ukrainian prose.
4. Runs mechanical sweeps (dash grep, Ukrainian terminology grep, `check_sentences.py`, `check_section_refs.py`, `check_word_gaps.py`).
5. Confirms any owner-flagged defect class with 3-5 old/new examples before sweeping, fixes the entire list in one pass by one writer, re-renders visuals, runs a bounded verification, and closes finished documents with an audience read (`skills/writing-technical-docs/references/audience-read-brief.md`) producing five criterion verdicts.

---

## FPGA & ASIC Design

Three self-contained skills, each bundling [`references/quartus-toolchain.md`](skills/constraining-fpga-designs/references/quartus-toolchain.md) with measured compile times, output paths, licensing notes, Apple Silicon container notes, and Quartus log codes.

| Skill | Fires when | What it enforces |
|---|---|---|
| `writing-synthesizable-rtl` | Writing, editing, or reviewing Verilog, SystemVerilog, or VHDL for synthesis | Unconditional block RAM reads (separate handshake block), zero resets on memory arrays, registered DSP inference, latch-free defaults, synchronous FPGA reset discipline, proper CDC structures, and `<=` vs. `=` separation. |
| `constraining-fpga-designs` | Creating projects, adding sources, compiling, or debugging timing in Quartus, Vivado, Libero, Diamond, or iCEcube | Mandatory `.sdc`/`.xdc` before the first compile: `create_clock` / `create_generated_clock`, `derive_clock_uncertainty`, I/O delays, justified `set_false_path`, paired `-setup`/`-hold` multicycles, explicit pin locations, and verifying `.sdc` inclusion in the project file. |
| `verifying-fpga-builds` | Interpreting synthesis, fitter, place-and-route, or bitstream reports | Four mandatory pass gates: (1) Quartus constraint warnings (`332012`, `332144`, `332142`, `332130`, `169085`) and Vivado `check_timing` gaps treated as build failures; (2) resource counts compared against order-of-magnitude estimates; (3) synthesis inference logs verified (`altsyncram`, DSPs); (4) positive setup and hold slack, zero TNS, slow/fast corner margin. |

---

## Verilog & SystemVerilog HDL

Eight skills encoding Clifford E. Cummings' Sunburst Design conference papers. Where a project style guide conflicts on naming or formatting (for example, lowRISC), the project guide wins on naming and formatting, while these skills govern simulation and synthesis semantics.

| Skill | Fires when | Source papers (Sunburst Design) |
|---|---|---|
| `avoiding-verilog-races` | Writing/reviewing `always` blocks, testbenches, clock/reset generators; sim mismatches; `#0` or `#1` fixes | *Nonblocking Assignments in Verilog Synthesis, Coding Styles That Kill!* (SNUG-2000, 1st); *NBA With Delays, Myths & Mysteries* (SNUG-2002, 2nd); *SystemVerilog Event Regions* (SNUG-2006, w/ Salz) |
| `designing-async-fifos` | Designing, reviewing, or debugging dual-clock FIFOs and full/empty flags | *Simulation and Synthesis Techniques for Asynchronous FIFO Design* (FIFO1, SNUG-2002); *...with Asynchronous Pointer Comparisons* (FIFO2, SNUG-2002, 1st, w/ Alfke) |
| `crossing-clock-domains` | Crossing signals, buses, pulses, counters, or resets across async clocks; CDC constraints; gate-sim X-storms | *CDC Design & Verification Techniques Using SystemVerilog* (SNUG-2008, 1st); *Multi-Asynchronous Clock Designs* (SNUG-2001, 3rd) |
| `coding-fsms` | Writing or refactoring FSMs, encodings, registered outputs, or `default` states | *FSM Design & Synthesis using SystemVerilog Part I* (SNUG-2019, 1st); *FSM Fundamentals* (ICU-2002, 2nd); *SystemVerilog 3.0 FSM* (SNUG-2003, 2nd); *Glitch-Free FSM Outputs* (SNUG-2000 Boston, 2nd) |
| `coding-case-statements` | Writing `case`/`casez`/`casex`, priority encoders, decoders; `full_case`/`parallel_case` pragmas | *"full_case parallel_case", the Evil Twins of Verilog Synthesis* (SNUG-1999, 1st) |
| `using-sv-processes-and-ports` | Choosing `always_comb`/`always_ff`/`always_latch`, RTL functions vs. tasks, `.*`/`.name` ports | *SystemVerilog Logic Specific Processes for Synthesis* (SNUG-2016, 2nd); *SystemVerilog Implicit Port Enhancements* (SNUG-2007, 1st) |
| `writing-designer-sva` | Writing RTL assertions, bindfiles vs. inline SVA, assertion labels and macros | *SVA Design Tricks & SVA Bind Files* (SNUG-2009, 1st); *SVA — Bindfiles & Best Known Practices* (SNUG-2016, 1st) |
| `speeding-up-simulation` | Slow SV regressions, testbench logging/randomization/coverage bottlenecks | *Verilog Coding Styles for Improved Simulation Efficiency* (ICU-1997, 1st); *Yikes! Why is My SystemVerilog Still So Slooooow?* (DVCon-2019, 1st, w/ Rose & Sherer) |

---

## Repository Layout

```text
gemini-skills/
├── gemini-extension.json                 # Gemini CLI extension manifest
├── plugin.json                           # Antigravity native plugin manifest
├── commands/
│   └── review-doc.toml                   # /review-doc command for Gemini CLI
├── skills/
│   ├── writing-technical-docs/           # SKILL.md, references/, evals/
│   ├── reporting-experiments/            # SKILL.md, evals/
│   ├── typesetting-documents-as-pdf/     # SKILL.md, scripts/, assets/
│   ├── documenting-existing-designs/     # SKILL.md, references/, evals/
│   ├── designing-research-studies/       # SKILL.md, references/, evals/
│   ├── writing-research-reports/         # SKILL.md, references/, evals/
│   ├── writing-synthesizable-rtl/        # SKILL.md, references/quartus-toolchain.md
│   ├── constraining-fpga-designs/        # SKILL.md, references/quartus-toolchain.md
│   ├── verifying-fpga-builds/            # SKILL.md, references/quartus-toolchain.md
│   ├── avoiding-verilog-races/           # SKILL.md
│   ├── designing-async-fifos/            # SKILL.md
│   ├── crossing-clock-domains/           # SKILL.md
│   ├── coding-fsms/                      # SKILL.md
│   ├── coding-case-statements/           # SKILL.md
│   ├── using-sv-processes-and-ports/     # SKILL.md
│   ├── writing-designer-sva/             # SKILL.md
│   └── speeding-up-simulation/           # SKILL.md
├── gems/                                 # Plain Gemini Web / Gems / AI Studio packs
│   ├── README.md
│   ├── docs-standards-gem.md
│   ├── fpga-design-gem.md
│   └── sunburst-hdl-gem.md
├── templates/
│   └── GEMINI.md                         # Always-on rules template
├── CONTRIBUTING.md
└── README.md
```

---

## Validation

Run before committing or releasing updates:

```bash
# Validate the Gemini CLI extension manifest
gemini extensions validate .

# Validate an individual skill (using Gemini CLI's bundled validator script)
node <path-to-gemini-cli>/bundle/builtin/skill-creator/scripts/validate_skill.cjs skills/<skill-name>

# Validate the Antigravity plugin layout
agy plugin validate .
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding new skills, slash commands, Gem packs, and bumping manifest versions.
