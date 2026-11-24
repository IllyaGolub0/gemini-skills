# Contributing to `gemini-skills`

This repository is structured as a **Gemini CLI extension**, an **Antigravity plugin**, a collection of standalone **Agent Skills** (`skills/*`), and ready-to-paste **Gemini Gem packs** (`gems/*`).

---

## 1. Choose the Right Artifact

| Knowledge or Automation | Format | Location |
|---|---|---|
| Repeatable practice, checklist, or workflow | **Agent Skill** (`SKILL.md`) | `skills/<skill-name>/SKILL.md` |
| User-invoked shortcut (`/name`) | **Gemini CLI Custom Command** | `commands/<name>.toml` |
| Long reference manual or checklist supporting a skill | **Reference file** | `skills/<skill-name>/references/` |
| Deterministic helper script run by a skill | **Script** | `skills/<skill-name>/scripts/` |
| Static template or header asset | **Asset** | `skills/<skill-name>/assets/` |
| Trigger evaluation queries | **Eval set** | `skills/<skill-name>/evals/` |
| Instruction pack for Gemini Web / Gems / AI Studio | **Gem pack** | `gems/<domain>-gem.md` |
| Always-on workspace or personal rules | **Rules template** | `templates/GEMINI.md` |

Gemini CLI and Antigravity auto-discover `skills/` and `commands/` (Gemini CLI). Files inside `references/`, `scripts/`, and `assets/` are read **only** when a `SKILL.md` names them, so always link them using paths relative to that `SKILL.md` (for example, `references/quartus-toolchain.md`). Keep every skill completely self-contained inside `skills/<skill-name>/` so `gemini skills install ... --path skills/<skill-name>` works standalone.

---

## 2. Add or Update a Skill

Create `skills/<skill-name>/SKILL.md` with valid YAML frontmatter:

```markdown
---
name: <skill-name>
description: "Use when ... List concrete trigger contexts and user phrasings in third person without summarizing the skill body."
---

# <Title>

Explain the guidance and the *reasoning* behind each rule (avoid bare MUST/NEVER rules
without the "why"). Keep SKILL.md focused and under ~500 lines; move long checklists,
templates, or vendor lookup tables into references/.
```

### Frontmatter Rules (Verified by Gemini CLI Validator & Parser)

1. **`name`**: Lowercase hyphen-case (`^[a-z0-9-]+$`), matching the `skills/<skill-name>` directory name.
2. **`description`**: Must be a **single-line** string (`validate_skill.cjs` rejects multiline YAML block scalars like `>-`) and **at most 1024 characters**.
3. **YAML Quoting**: Wrap `description` in double quotes (`"..."`, escaping inner quotes as `\"`) whenever it contains `: ` or embedded quotation marks so YAML parsers handle it cleanly.
4. **Triggering**: Write in the third person starting with `"Use when..."`, include concrete phrasings that should trigger activation, and do **not** summarize the procedural workflow inside `description` (an agent that sees a summary in `description` may follow the summary instead of activating the skill).

No manifest edit is needed when adding a skill under `skills/` — it is auto-discovered. If the skill fits one of the three domain Gem packs in `gems/`, update the matching `gems/<domain>-gem.md` summary as well.

---

## 3. Add a Custom Slash Command (Gemini CLI)

Place a TOML file at `commands/<command-name>.toml` (subdirectories map to `:` namespaces, such as `commands/docs/audit.toml` -> `/docs:audit`):

```toml
description = "Short one-line summary shown in /help and /commands list."

prompt = """
Instructions for Gemini when the command runs.
Target argument: {{args}}
"""
```

- Use `{{args}}` to inject arguments passed after `/<command-name>`.
- Use `@{relative/path}` to inline workspace files or `!{shell command}` for shell output injection when needed.
- Run `/commands reload` inside Gemini CLI to pick up changes without restarting.

---

## 4. Authoring Skills With `skill-creator`

In Gemini CLI or Antigravity, you can ask the built-in `skill-creator` skill to scaffold a new skill under `skills/<skill-name>/` and draft trigger/non-trigger evaluation queries in `skills/<skill-name>/evals/trigger-evals.json`. Keep `.log` files ignored via `.gitignore`.

Note: on-demand skills can under-trigger on generic first-draft prompts where the model believes it can answer directly. For rules that must apply on every draft regardless of skill activation, update [`templates/GEMINI.md`](templates/GEMINI.md).

---

## 5. Local Testing & Validation Checklist

Link the local checkout to test skills and `/review-doc` in real tasks before committing:

```bash
# Link as a Gemini CLI extension (live edits)
gemini extensions link .

# Or link only the skills directory
gemini skills link ./skills
```

Before pushing, verify every manifest, skill, command, and script:

1. **Gemini CLI extension validation**:
   ```bash
   gemini extensions validate .
   ```
2. **Gemini CLI skill validation** (must pass on every directory in `skills/`):
   ```bash
   # Locate validate_skill.cjs in your @google/gemini-cli installation
   node <path-to-gemini-cli>/bundle/builtin/skill-creator/scripts/validate_skill.cjs skills/<skill-name>
   ```
3. **Antigravity plugin validation**:
   ```bash
   agy plugin validate .
   ```
4. **Syntax & link checks**:
   - Confirm `commands/*.toml` parse cleanly as TOML.
   - Confirm every relative `references/`, `scripts/`, and `assets/` path mentioned in `SKILL.md` exists inside that skill's directory.
   - Run `python3 -m py_compile` on any Python helpers you added or edited.
5. **Version bump**:
   - Bump `"version"` (semver) in both [`gemini-extension.json`](gemini-extension.json) and [`plugin.json`](plugin.json) on every release so installed copies detect updates via `gemini extensions update gemini-skills`.
