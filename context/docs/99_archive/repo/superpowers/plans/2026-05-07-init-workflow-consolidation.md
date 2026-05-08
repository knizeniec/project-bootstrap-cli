---
title: Init Workflow Consolidation Implementation Plan
status: active
record_class: historical
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: planning
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Init Workflow Consolidation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the seven phase-specific initialization commands with one resume-aware `/init` command that reads the existing plan file, shows status, routes to the correct phase or artifact, and supports revisiting completed work without adding new state files.

**Architecture:** Keep `project-initialization/` as the single source of phase and artifact logic, then replace the thin per-phase wrappers in `.claude/commands/`, `.copilot/commands/`, and `.codex/commands/` with one thin `/init` wrapper per tool. Add a shared parser/rendering contract inside the new command/skill text, teach the plan template to record artifact revisits, and extend the session-start hook plus parity script so status visibility and cross-surface consistency stay mechanical.

**Tech Stack:** Markdown, Bash hooks, JSON settings, Python 3 parity script, ripgrep, markdownlint-cli2, lychee

---

## File Map

**Create:**
- `.claude/commands/init.md` — Claude command entrypoint for the consolidated `/init` workflow
- `.copilot/commands/init.md` — Copilot command entrypoint for the consolidated `/init` workflow
- `.codex/commands/init.md` — Codex command entrypoint for the consolidated `/init` workflow

**Modify:**
- `project-initialization/plan-template.md` — add `Last Revisited` column to the Artifact Roadmap table
- `project-initialization/README.md` — replace seven-command guidance with `/init`, status block, revisit flow, and hook behavior
- `project-initialization/contract.md` — replace next-command language with `/init` routing guidance
- `project-initialization/phases/0-triage.md` — update duplicate-plan stop text and next-step output to `/init`
- `project-initialization/phases/1-intent.md` — update plan-missing guidance and any re-run guidance to `/init`
- `project-initialization/phases/2-specification.md` — update plan-missing guidance and any re-run guidance to `/init`
- `project-initialization/phases/3-design.md` — update plan-missing guidance and any re-run guidance to `/init`
- `project-initialization/phases/4-govern-operate.md` — update plan-missing guidance and any re-run guidance to `/init`
- `project-initialization/phases/5-adapt.md` — update plan-missing guidance and any re-run guidance to `/init`
- `project-initialization/phases/6-review.md` — update plan-missing guidance and rerun guidance to `/init`
- `project-initialization/artifacts/repo-sync.md` — stop referring to `.claude/commands/init-triage.md`; point to the consolidated init entrypoint instead
- `.claude/settings.json` — ensure SessionStart registration still points at the updated status-aware hook
- `.claude/hooks/session-start` — inject the `/init` status block when an in-progress initialization plan exists
- `.copilot/hooks/hooks.json` — ensure SessionStart registration still points at the updated status-aware hook
- `.copilot/hooks/session-start` — inject the `/init` status block when an in-progress initialization plan exists
- `.codex/hooks.json` — ensure SessionStart registration still points at the updated status-aware hook
- `.codex/hooks/session-start` — inject the `/init` status block when an in-progress initialization plan exists
- `scripts/check-init-parity.py` — replace seven-command checks with consolidated `/init` and hook registration assertions
- `README.md` — replace the seven-step adoption narrative with `/init` resume/revisit guidance
- `AGENTS.md` — replace the ordered `/init-*` workflow reference with `/init`
- `CHANGELOG.md` — add a breaking-change entry for the consolidated init workflow
- `.claude/README.md` — update tracked asset description if command/hook behavior changes are documented there
- `.copilot/README.md` — update tracked asset description if command/hook behavior changes are documented there
- `.codex/README.md` — update tracked asset description if command/hook behavior changes are documented there
- `.agents/README.md` — document any Codex runtime-surface note affected by the consolidated workflow change
- `.github/copilot-instructions.md` — keep Copilot’s repo instructions aligned if they mention the initialization workflow
- `docs/00-source-of-truth.md` — update assistant-tooling ownership notes only if they explicitly mention the old `/init-*` surface

**Delete:**
- `.claude/commands/init-triage.md`
- `.claude/commands/init-intent.md`
- `.claude/commands/init-spec.md`
- `.claude/commands/init-design.md`
- `.claude/commands/init-govern.md`
- `.claude/commands/init-adapt.md`
- `.claude/commands/init-review.md`
- `.copilot/commands/init-triage.md`
- `.copilot/commands/init-intent.md`
- `.copilot/commands/init-spec.md`
- `.copilot/commands/init-design.md`
- `.copilot/commands/init-govern.md`
- `.copilot/commands/init-adapt.md`
- `.copilot/commands/init-review.md`
- `.codex/commands/init-triage.md`
- `.codex/commands/init-intent.md`
- `.codex/commands/init-spec.md`
- `.codex/commands/init-design.md`
- `.codex/commands/init-govern.md`
- `.codex/commands/init-adapt.md`
- `.codex/commands/init-review.md`

## Notes Before Execution

- The design spec names tool-surface parity for `.claude/`, `.copilot/`, and `.codex/`, but this repository also has Codex’s repo-native skill surface in `.agents/skills/`. Keep that directory in sync if the implementation introduces any new init-specific skill artifacts or references.
- The current repo does **not** contain `init-*` skills under `.claude/skills/`, `.copilot/skills/`, `.codex/skills/`, `.agents/skills/`, or `.opencode/skills/`; do not invent extra deletes unless implementation actually creates a new `init` skill surface.
- The spec says the phase and artifact rubrics stay authoritative. Do not duplicate interview logic inside the new command wrappers; they should route into `project-initialization/phases/*.md` and `project-initialization/artifacts/*.md`.

---

### Task 1: Add the shared revisit audit field to the plan template

**Files:**
- Modify: `project-initialization/plan-template.md`
- Test: `project-initialization/plan-template.md`

- [ ] **Step 1: Write the failing change to the template table**

Replace the Artifact Roadmap header block in `project-initialization/plan-template.md`:

```markdown
## Artifact Roadmap

| Phase | Artifact | Status | Mode (initial) | Mode (effective) | Output path | Last Revisited |
| --- | --- | --- | --- | --- | --- | --- |

<!-- Add one row per artifact. Populate during triage. -->
<!-- Mode values: interview | confirm | extract | skipped -->
<!-- Last Revisited: YYYY-MM-DD after a revisit completes; blank otherwise -->
```

- [ ] **Step 2: Verify the old header is gone and the new one is present**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
text = Path('project-initialization/plan-template.md').read_text()
assert '| Phase | Artifact | Status | Mode (initial) | Mode (effective) | Output path | Last Revisited |' in text
assert '| Phase | Artifact | Status | Mode (initial) | Mode (effective) | Output path |\n' not in text
print('PASS: plan template includes Last Revisited column')
PY
```

Expected: `PASS: plan template includes Last Revisited column`

- [ ] **Step 3: Commit**

```bash
git add project-initialization/plan-template.md
git commit -m "docs: add revisit audit field to init plan template"
```

---

### Task 2: Rewrite the tool-neutral initialization docs and phase copy for `/init`

**Files:**
- Modify: `project-initialization/README.md`
- Modify: `project-initialization/contract.md`
- Modify: `project-initialization/phases/0-triage.md`
- Modify: `project-initialization/phases/1-intent.md`
- Modify: `project-initialization/phases/2-specification.md`
- Modify: `project-initialization/phases/3-design.md`
- Modify: `project-initialization/phases/4-govern-operate.md`
- Modify: `project-initialization/phases/5-adapt.md`
- Modify: `project-initialization/phases/6-review.md`
- Modify: `project-initialization/artifacts/repo-sync.md`
- Test: `project-initialization/**/*.md`

- [ ] **Step 1: Rewrite `project-initialization/README.md` for the consolidated command**

Replace the “How initialization works” and “Phases” sections with:

```markdown
## How initialization works

A fresh clone of this template is unspecialized. To initialize it for a specific project, run `/init` in your AI tool's command surface.

- Claude: `.claude/commands/init.md`
- Copilot: `.copilot/commands/init.md`
- Codex: `.codex/commands/init.md`

The command reads `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` as its sole source of truth.

- If no plan exists, `/init` starts Phase 0 (Triage).
- If a plan exists and phases remain incomplete, `/init` shows the current roadmap, names the next phase, and lets the user continue, revisit completed work, or show the plan.
- If all phases are complete, `/init` reports completion and offers revisit options.
- Power users can jump directly with `/init <phase-or-artifact-name>`.

## Phases

| Phase | Routed by `/init` to | Purpose |
| --- | --- | --- |
| 0. Triage | `project-initialization/phases/0-triage.md` | Routing questions, profile selection, artifact roadmap |
| 1. Intent | `project-initialization/phases/1-intent.md` | Project brief, optional business case |
| 2. Specification | `project-initialization/phases/2-specification.md` | PRD, requirements catalog, user journeys, acceptance catalog |
| 3. Design | `project-initialization/phases/3-design.md` | Solution design, ADRs, optional C4 views |
| 4. Govern & Operate | `project-initialization/phases/4-govern-operate.md` | AI policy, test strategy, security baseline, delivery plan |
| 5. Adapt | `project-initialization/phases/5-adapt.md` | Language adaptation, repository sync |
| 6. Final review | `project-initialization/phases/6-review.md` | Cross-doc coherence, entry-doc parity, standards pass |

### Status and revisit behavior

`/init` without arguments renders a status block derived from the plan file, then offers:

```text
[Enter] continue · revisit · show plan · q
```

- `continue` runs the next incomplete phase.
- `revisit` opens the completed-phase and artifact menu.
- `show plan` prints the plan path plus the roadmap and resume sections.
- `q` exits without changes.

Revisiting a completed artifact updates `Last Revisited` in the Artifact Roadmap but keeps both artifact and phase status as `done`.

### SessionStart visibility

Each tool's SessionStart hook checks for an in-progress project initialization plan on session open.

- No plan file: hook stays silent.
- In-progress plan: hook prints the status block and `Run /init to continue.`
- All phases done: hook stays silent.
```

- [ ] **Step 2: Rewrite `project-initialization/contract.md` stop and output rules**

Make these exact replacements in `project-initialization/contract.md`:

```markdown
## Per-run stop rules

1. Read the plan file. If missing, stop — tell the user to run `/init`.
2. Verify the previous phase is `done` unless `/init <name>` explicitly routed into a revisit of a completed phase or artifact. If a required prior phase is incomplete, stop — name the incomplete phase.
3. Run all active artifacts end-to-end without intra-phase pauses.
4. Run end-of-phase review before updating the plan.
5. Update the plan in one batch at the end of the run, never incrementally.
6. After the plan update, stop. Do not begin the next phase or read ahead for it. Tell the user to run `/init` to continue.

## End-of-run output format

Output these sections in order:

```text
Stage Completed: Phase N — <Phase Name>
Docs Updated: <comma-separated list of files written or modified>
Review Fixes Applied: <list of critical/important findings that were fixed, or "none">
Concerns And Recommendations: <list of five-part concerns raised this run, or "none">
Parked Questions: <questions raised but not fitting any active artifact, or "none">
Next Recommended Step: Run `/init` to continue. The command will route to the next incomplete phase or revisit target.
```
```

- [ ] **Step 3: Rewrite phase and artifact copy that still points at `/init-*`**

Apply these exact text replacements:

```markdown
project-initialization/phases/0-triage.md
- Replace: "Run the next phase command, or delete the plan file and re-run `/init-triage` to start over."
- With: "Run `/init` to continue, revisit a completed item, or delete the plan file and re-run `/init` to start over."
- Replace the output line with: "Next Recommended Step: Run `/init` to continue to Phase 1 (Intent)."

project-initialization/phases/1-intent.md
- Replace: "No initialization plan found. Run `/init-triage` first."
- With: "No initialization plan found. Run `/init` first."

project-initialization/phases/2-specification.md
- Replace any "Run `/init-triage` first" or phase-specific rerun instruction with "Run `/init` first" or "Run `/init` again after fixes."

project-initialization/phases/3-design.md
- Replace any "Run `/init-triage` first" or phase-specific rerun instruction with "Run `/init` first" or "Run `/init` again after fixes."

project-initialization/phases/4-govern-operate.md
- Replace any "Run `/init-triage` first" or phase-specific rerun instruction with "Run `/init` first" or "Run `/init` again after fixes."

project-initialization/phases/5-adapt.md
- Replace any "Run `/init-triage` first" or phase-specific rerun instruction with "Run `/init` first" or "Run `/init` again after fixes."

project-initialization/phases/6-review.md
- Replace any "Run `/init-triage` first" or "The user runs `/init-review` again after fixes" language with `/init` equivalents.

project-initialization/artifacts/repo-sync.md
- Replace: "Verify `CLAUDE.md` still accurately points to `.claude/commands/init-triage.md`"
- With: "Verify repository entry surfaces now point users at the consolidated `/init` workflow instead of any retired `/init-*` command."
```

- [ ] **Step 4: Run targeted search to prove no stale `/init-*` references remain in `project-initialization/`**

Run:

```bash
rg -n '/init-(triage|intent|spec|design|govern|adapt|review)' project-initialization
```

Expected: no output

- [ ] **Step 5: Lint the tool-neutral docs**

Run:

```bash
npx -y markdownlint-cli2 project-initialization/**/*.md
```

Expected: PASS with no errors

- [ ] **Step 6: Commit**

```bash
git add project-initialization/README.md project-initialization/contract.md project-initialization/plan-template.md project-initialization/phases/*.md project-initialization/artifacts/repo-sync.md
git commit -m "docs: route project initialization guidance through init"
```

---

### Task 3: Replace per-phase command wrappers with one `/init` wrapper per tool

**Files:**
- Create: `.claude/commands/init.md`
- Create: `.copilot/commands/init.md`
- Create: `.codex/commands/init.md`
- Delete: `.claude/commands/init-triage.md`
- Delete: `.claude/commands/init-intent.md`
- Delete: `.claude/commands/init-spec.md`
- Delete: `.claude/commands/init-design.md`
- Delete: `.claude/commands/init-govern.md`
- Delete: `.claude/commands/init-adapt.md`
- Delete: `.claude/commands/init-review.md`
- Delete: `.copilot/commands/init-triage.md`
- Delete: `.copilot/commands/init-intent.md`
- Delete: `.copilot/commands/init-spec.md`
- Delete: `.copilot/commands/init-design.md`
- Delete: `.copilot/commands/init-govern.md`
- Delete: `.copilot/commands/init-adapt.md`
- Delete: `.copilot/commands/init-review.md`
- Delete: `.codex/commands/init-triage.md`
- Delete: `.codex/commands/init-intent.md`
- Delete: `.codex/commands/init-spec.md`
- Delete: `.codex/commands/init-design.md`
- Delete: `.codex/commands/init-govern.md`
- Delete: `.codex/commands/init-adapt.md`
- Delete: `.codex/commands/init-review.md`
- Test: `.claude/commands/init.md`
- Test: `.copilot/commands/init.md`
- Test: `.codex/commands/init.md`

- [ ] **Step 1: Write the new consolidated wrapper text in `.claude/commands/init.md`**

Create `.claude/commands/init.md` with this exact content:

```markdown
---
description: "Run the consolidated project initialization workflow — start triage, resume the next phase, or revisit a completed phase or artifact."
---

You are running the consolidated `/init` workflow for this repository.

## Argument handling

- No argument: inspect `docs/superpowers/plans/*-project-initialization.md`.
  - If no plan exists, announce `No initialization plan found — running Phase 0 (Triage).` and follow `project-initialization/phases/0-triage.md` exactly.
  - If a plan exists and at least one phase is not `done`, render the status block below, then wait for `[Enter] continue · revisit · show plan · q`.
  - If all phases are `done`, print `Project initialization is complete.` and offer the revisit menu.
- One argument: resolve it against phase names (`triage`, `intent`, `spec`, `design`, `govern`, `adapt`, `review`) and artifact names from `project-initialization/artifacts/`. A phase argument runs the corresponding phase rubric; an artifact argument runs only that artifact in revisit mode.
- Unknown argument: print the supported phase names plus the artifact catalog and stop.

## Status block template

Render this format from the plan's Phase Roadmap:

```text
Project initialization — <plan date>
  ✓ Phase 0  Triage          done  <date or blank>
  ✓ Phase 1  Intent          done  <date or blank>
  ▶ Phase 2  Specification   next
    Phase 3  Design
    Phase 4  Govern & Operate
    Phase 5  Adapt
    Phase 6  Final review

Next action: run Phase <N> — <Phase Name> (<artifacts summary>).

[Enter] continue · revisit · show plan · q
```

## Revisit menu template

If the user chooses `revisit`, render completed phases and their artifacts in a flat menu:

```text
Revisit a completed item:
  1. Phase 0 — Triage (whole phase)
  2. Phase 1 — Intent (whole phase)
     a. project-brief        last revisited: never
     b. business-case        last revisited: 2026-05-06
Enter number (e.g. 2a) or q to cancel:
```

Selecting a whole phase runs that phase rubric in revisit mode. Selecting an artifact runs only that artifact's rubric in append/extend mode and updates `Last Revisited` in the Artifact Roadmap when the artifact run completes.

## Routing rules

- Always treat `project-initialization/plan-template.md` and the active plan file as the only state source.
- Always follow `project-initialization/contract.md` for per-run stop rules.
- Always route into `project-initialization/phases/<N>-*.md` for full-phase work.
- Always route into `project-initialization/artifacts/<name>.md` for artifact-only revisits.
- Never duplicate phase interview logic in this wrapper.
- If the existing plan lacks the `Last Revisited` column, treat it as blank for every artifact.
```

- [ ] **Step 2: Copy the same wrapper to Copilot and Codex surfaces**

Create `.copilot/commands/init.md` and `.codex/commands/init.md` with the same body as `.claude/commands/init.md`.

- [ ] **Step 3: Remove the retired per-phase wrappers**

Delete these files:

```text
.claude/commands/init-triage.md
.claude/commands/init-intent.md
.claude/commands/init-spec.md
.claude/commands/init-design.md
.claude/commands/init-govern.md
.claude/commands/init-adapt.md
.claude/commands/init-review.md
.copilot/commands/init-triage.md
.copilot/commands/init-intent.md
.copilot/commands/init-spec.md
.copilot/commands/init-design.md
.copilot/commands/init-govern.md
.copilot/commands/init-adapt.md
.copilot/commands/init-review.md
.codex/commands/init-triage.md
.codex/commands/init-intent.md
.codex/commands/init-spec.md
.codex/commands/init-design.md
.codex/commands/init-govern.md
.codex/commands/init-adapt.md
.codex/commands/init-review.md
```

- [ ] **Step 4: Verify each tool surface has only `init.md` left in its commands directory for this workflow**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
for tool in ('.claude', '.copilot', '.codex'):
    names = sorted(p.name for p in Path(tool, 'commands').glob('init*.md'))
    assert names == ['init.md'], f'{tool}: {names}'
print('PASS: consolidated init wrapper present across all tool surfaces')
PY
```

Expected: `PASS: consolidated init wrapper present across all tool surfaces`

- [ ] **Step 5: Commit**

```bash
git add .claude/commands/init.md .copilot/commands/init.md .codex/commands/init.md .claude/commands .copilot/commands .codex/commands
git commit -m "feat: consolidate init command surfaces"
```

---

### Task 4: Make the SessionStart hooks surface `/init` status instead of only skill bootstrap text

**Files:**
- Modify: `.claude/hooks/session-start`
- Modify: `.copilot/hooks/session-start`
- Modify: `.codex/hooks/session-start`
- Modify: `.claude/settings.json`
- Modify: `.copilot/hooks/hooks.json`
- Modify: `.codex/hooks.json`
- Test: `.claude/hooks/session-start`
- Test: `.copilot/hooks/session-start`
- Test: `.codex/hooks/session-start`

- [ ] **Step 1: Add shared status-block helper logic to `.claude/hooks/session-start`**

Insert this shell block after plugin-root setup and before the existing legacy-skills warning logic:

```bash
REPO_ROOT="$(git rev-parse --show-toplevel 2>/dev/null || pwd)"

find_init_plan() {
  rg --files "${REPO_ROOT}/docs/superpowers/plans" | rg 'project-initialization\.md$' | head -n 1
}

build_init_status_message() {
  local plan_path="$1"
  [ -n "$plan_path" ] || return 0

  python3 - "$plan_path" <<'PY'
from pathlib import Path
import re
import sys

path = Path(sys.argv[1])
text = path.read_text()
phase_block_match = re.search(r"## Phase Roadmap\n\n((?:\|.*\n)+)", text)
if not phase_block_match:
    raise SystemExit(0)

rows = []
for line in phase_block_match.group(1).splitlines():
    if not line.startswith('| ') or line.startswith('| ---'):
        continue
    parts = [part.strip() for part in line.strip('|').split('|')]
    if len(parts) != 3:
        continue
    rows.append(parts)

if not rows:
    raise SystemExit(0)

next_index = next((i for i, row in enumerate(rows) if row[1] != 'done'), None)
if next_index is None:
    raise SystemExit(0)

date_match = re.search(r'(\d{4}-\d{2}-\d{2})-project-initialization\.md$', path.name)
plan_date = date_match.group(1) if date_match else path.name
lines = [f'Project initialization — {plan_date}']
for idx, (phase, status, notes) in enumerate(rows):
    label = phase.split('. ', 1)[1]
    if idx < next_index:
        suffix = notes or ''
        lines.append(f'  ✓ {phase:<8} {label:<15} done  {suffix}'.rstrip())
    elif idx == next_index:
        lines.append(f'  ▶ {phase:<8} {label:<15} next')
    else:
        lines.append(f'    {phase:<8} {label}')

phase, _, notes = rows[next_index]
label = phase.split('. ', 1)[1]
summary = notes if notes else 'continue the initialization workflow'
lines.extend(['', f'Next action: run {phase} — {label} ({summary}).', '', 'Run /init to continue.'])
print('\\n'.join(lines))
PY
}
```

- [ ] **Step 2: Append the rendered init status to the injected SessionStart context**

Replace the current `session_context=` assignment in all three hook scripts with this structure:

```bash
plan_path="$(find_init_plan || true)"
init_status_message="$(build_init_status_message "$plan_path" || true)"

session_context="<EXTREMELY_IMPORTANT>\nYou have superpowers.\n\n**Below is the full content of your 'superpowers:using-superpowers' skill - your introduction to using skills. For all other skills, use the 'Skill' tool:**\n\n${using_superpowers_escaped}\n\n${warning_escaped}"

if [ -n "$init_status_message" ]; then
  init_status_escaped=$(escape_for_json "$init_status_message")
  session_context="${session_context}\n\n<project-init-status>${init_status_escaped}</project-init-status>"
fi

session_context="${session_context}\n</EXTREMELY_IMPORTANT>"
```

- [ ] **Step 3: Keep hook registration explicit and non-async**

Ensure these tracked settings files contain this exact registration block:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|clear|compact",
        "hooks": [
          {
            "type": "command",
            "command": "<existing per-tool run-hook command>",
            "async": false
          }
        ]
      }
    ]
  }
}
```

Use the existing per-tool command string already present in:

```text
.claude/settings.json
.copilot/hooks/hooks.json
.codex/hooks.json
```

- [ ] **Step 4: Test the hook helper directly against an in-progress plan**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
sample = Path('docs/superpowers/plans/2099-01-01-project-initialization.md')
sample.write_text('''# Project Initialization Plan\n\n## Phase Roadmap\n\n| Phase | Status | Notes |\n| --- | --- | --- |\n| 0. Triage | done | 2099-01-01 |\n| 1. Intent | pending | project-brief, business-case |\n| 2. Specification | pending | |\n| 3. Design | pending | |\n| 4. Govern & Operate | pending | |\n| 5. Adapt | pending | |\n| 6. Final review | pending | |\n''')
print(sample)
PY
.claude/hooks/session-start | rg 'Project initialization|Run /init to continue\.'
rm docs/superpowers/plans/2099-01-01-project-initialization.md
```

Expected: output contains both `Project initialization` and `Run /init to continue.`

- [ ] **Step 5: Commit**

```bash
git add .claude/hooks/session-start .copilot/hooks/session-start .codex/hooks/session-start .claude/settings.json .copilot/hooks/hooks.json .codex/hooks.json
git commit -m "feat: surface init status in session-start hooks"
```

---

### Task 5: Extend the parity script for the consolidated command and hook registration

**Files:**
- Modify: `scripts/check-init-parity.py`
- Test: `scripts/check-init-parity.py`

- [ ] **Step 1: Replace the existing parity script body with consolidated checks**

Set `scripts/check-init-parity.py` to this exact content:

```python
#!/usr/bin/env python3
"""
Check that the consolidated init workflow exists across .claude/, .copilot/, .codex/
and that retired phase wrappers are gone. Run from repo root.
"""

from pathlib import Path
import json
import sys

TOOL_DIRS = [".claude", ".copilot", ".codex"]
RETIRED = [
    "init-triage.md",
    "init-intent.md",
    "init-spec.md",
    "init-design.md",
    "init-govern.md",
    "init-adapt.md",
    "init-review.md",
]

COMMAND_TEXT_CHECKS = [
    "project-initialization/phases/",
    "project-initialization/artifacts/",
    "project-initialization/contract.md",
    "[Enter] continue · revisit · show plan · q",
    "Last Revisited",
]

HOOK_FILES = {
    ".claude": Path(".claude/settings.json"),
    ".copilot": Path(".copilot/hooks/hooks.json"),
    ".codex": Path(".codex/hooks.json"),
}

HOOK_COMMAND_SNIPPETS = {
    ".claude": '.claude/hooks/run-hook.cmd" session-start',
    ".copilot": '${CLAUDE_PLUGIN_ROOT}/hooks/run-hook.cmd" session-start',
    ".codex": '.codex/hooks/run-hook.cmd" session-start',
}

errors = []

for tool in TOOL_DIRS:
    init_path = Path(tool) / "commands" / "init.md"
    if not init_path.exists():
        errors.append(f"MISSING: {init_path}")
        continue

    content = init_path.read_text()
    for phrase in COMMAND_TEXT_CHECKS:
        if phrase not in content:
            errors.append(f"MISSING PHRASE in {init_path}: {phrase!r}")

    for retired in RETIRED:
        retired_path = Path(tool) / "commands" / retired
        if retired_path.exists():
            errors.append(f"RETIRED FILE STILL PRESENT: {retired_path}")

for tool, path in HOOK_FILES.items():
    if not path.exists():
        errors.append(f"MISSING HOOK REGISTRATION: {path}")
        continue
    data = json.loads(path.read_text())
    hooks = data.get("hooks", {}).get("SessionStart", [])
    if not hooks:
        errors.append(f"NO SessionStart hook in {path}")
        continue
    command_blob = json.dumps(hooks)
    if HOOK_COMMAND_SNIPPETS[tool] not in command_blob:
        errors.append(f"SessionStart command mismatch in {path}")

if errors:
    print("Parity check FAILED:")
    for error in errors:
        print(f"  {error}")
    sys.exit(1)

print("Parity check PASSED: consolidated init workflow present across all tool surfaces.")
```

- [ ] **Step 2: Run the parity check**

Run:

```bash
python3 scripts/check-init-parity.py
```

Expected: `Parity check PASSED: consolidated init workflow present across all tool surfaces.`

- [ ] **Step 3: Commit**

```bash
git add scripts/check-init-parity.py
git commit -m "test: update init parity checks for consolidated workflow"
```

---

### Task 6: Update the top-level docs and assistant asset docs for the breaking workflow change

**Files:**
- Modify: `README.md`
- Modify: `AGENTS.md`
- Modify: `CHANGELOG.md`
- Modify: `.claude/README.md`
- Modify: `.copilot/README.md`
- Modify: `.codex/README.md`
- Modify: `.agents/README.md`
- Modify: `.github/copilot-instructions.md`
- Test: `README.md`
- Test: `AGENTS.md`
- Test: `CHANGELOG.md`

- [ ] **Step 1: Replace the README initialization journey and checklist text**

Update `README.md` with these exact replacements:

```markdown
## Project journey

A new project using this template goes through seven phases before implementation, but the user only needs to remember one command: `/init`.

```text
Clone
  │
  ▼
/init
  Starts Phase 0 (Triage) if no plan exists.
  Otherwise shows status, resumes the next phase,
  or lets the user revisit a completed phase or artifact.
  State lives in docs/superpowers/plans/YYYY-MM-DD-project-initialization.md
```

`/init` routes into these phases:

| Phase | Routed target | Output |
| --- | --- | --- |
| 0 | `project-initialization/phases/0-triage.md` | plan file in `docs/superpowers/plans/` |
| 1 | `project-initialization/phases/1-intent.md` | `docs/00_governance/00_project_brief.md` and optional `01_business_case.md` |
| 2 | `project-initialization/phases/2-specification.md` | active specification artifacts |
| 3 | `project-initialization/phases/3-design.md` | `docs/03_architecture/01_solution_design.md`, ADRs, optional C4 views |
| 4 | `project-initialization/phases/4-govern-operate.md` | govern and operate artifacts per profile |
| 5 | `project-initialization/phases/5-adapt.md` | language-specific scaffold and repository sync updates |
| 6 | `project-initialization/phases/6-review.md` | final review pass and completed plan |

## Bootstrap checklist

1. Clone this repository.
2. Run `/init` in your AI tool to start Phase 0 (Triage) and write the initialization plan.
3. Run `/init` again whenever you want to continue; it will resume the next incomplete phase automatically.
4. Use `/init <phase-or-artifact-name>` if you need to revisit a completed phase or artifact directly.
5. Replace the placeholder `LICENSE` with your chosen license before publishing.
6. Keep machine-specific overrides local-only. The assistant directories `.claude/`, `.copilot/`, `.codex/`, and `.opencode/` are part of the tracked template contract; only `settings.local.json` and similar machine-specific files should stay untracked.
7. Commit the bootstrap state as one or two clean commits before starting feature work.

## Initialize your project

Run `/init` in Claude Code, GitHub Copilot Chat, Codex, or any tool that supports per-project slash commands. The command:

- Starts triage if no plan exists.
- Reads the existing plan file and shows a status block when initialization is already in progress.
- Lets the user continue, revisit completed work, or print the plan path and resume context.
- Supports direct jumps with `/init <phase-or-artifact-name>`.
```

- [ ] **Step 2: Update root workflow references and the breaking-change note**

Make these exact edits:

```markdown
AGENTS.md
- Replace: "run `/init-triage` first, then `/init-intent` through `/init-review` in order"
- With: "run `/init`; it will start triage, then resume the next incomplete initialization phase on subsequent runs"

CHANGELOG.md
- Under `## [Unreleased]`, add:

### Changed

- Consolidated the seven phase-specific project-initialization commands into a single resume-aware `/init` workflow.

### Removed

- Removed the legacy `/init-triage`, `/init-intent`, `/init-spec`, `/init-design`, `/init-govern`, `/init-adapt`, and `/init-review` command surfaces.
```

- [ ] **Step 3: Update assistant-asset docs that mention the workflow surface**

Apply these exact edits:

```markdown
.claude/README.md
- Append: "- `commands/init.md` — consolidated initialization command surface"

.copilot/README.md
- Append: "- `commands/init.md` — consolidated initialization command surface kept in parity with the sibling tool directories"

.codex/README.md
- Append: "- `commands/init.md` — consolidated initialization command surface mirrored with the sibling tool directories"

.agents/README.md
- Append: "- The consolidated `/init` workflow is command-driven; keep any Codex repo-native skill or documentation references aligned with the tracked command surfaces."

.github/copilot-instructions.md
- Append: "- When documenting repository bootstrap, refer to the consolidated `/init` workflow rather than any retired `/init-*` command."
```

- [ ] **Step 4: Search the repo for stale `/init-*` references outside the design spec and historical records**

Run:

```bash
rg -n '/init-(triage|intent|spec|design|govern|adapt|review)' README.md AGENTS.md CHANGELOG.md .claude .copilot .codex .agents .github project-initialization
```

Expected: no output

- [ ] **Step 5: Lint the changed Markdown docs**

Run:

```bash
npx -y markdownlint-cli2 README.md AGENTS.md CHANGELOG.md project-initialization/**/*.md .claude/README.md .copilot/README.md .codex/README.md .agents/README.md .github/copilot-instructions.md
```

Expected: PASS with no errors

- [ ] **Step 6: Commit**

```bash
git add README.md AGENTS.md CHANGELOG.md .claude/README.md .copilot/README.md .codex/README.md .agents/README.md .github/copilot-instructions.md project-initialization
git commit -m "docs: document consolidated init workflow"
```

---

### Task 7: Run the final verification matrix and manual command smoke tests

**Files:**
- Test: `.claude/commands/init.md`
- Test: `.copilot/commands/init.md`
- Test: `.codex/commands/init.md`
- Test: `.claude/hooks/session-start`
- Test: `.copilot/hooks/session-start`
- Test: `.codex/hooks/session-start`
- Test: `scripts/check-init-parity.py`
- Test: `README.md`
- Test: `project-initialization/**/*.md`

- [ ] **Step 1: Run automated verification**

Run:

```bash
python3 scripts/check-init-parity.py && npx -y markdownlint-cli2 README.md AGENTS.md CHANGELOG.md project-initialization/**/*.md .claude/README.md .copilot/README.md .codex/README.md .agents/README.md .github/copilot-instructions.md && lychee README.md project-initialization/README.md docs/superpowers/specs/2026-05-07-init-workflow-consolidation-design.md
```

Expected: parity check passes, markdownlint passes, and lychee reports no stale links to retired `/init-*` commands.

- [ ] **Step 2: Manually smoke-test the command behavior through its surface**

Run these interactive checks in a real command-capable tool surface for at least one harness (Claude, Copilot, or Codex):

```text
1. Fresh clone or clean test repo state: no `docs/superpowers/plans/*-project-initialization.md` file present.
2. Invoke `/init` with no arguments.
   Expected: announces no plan exists and routes into Phase 0 (Triage).
3. Complete enough of triage to write the plan file, then invoke `/init` again.
   Expected: status block shows Phase 0 done and Phase 1 as next.
4. Press Enter at the prompt.
   Expected: routes into Phase 1 without asking for `/init-intent`.
5. Open a fresh session.
   Expected: SessionStart prints the status block and `Run /init to continue.`.
6. Invoke `/init` and choose `revisit`.
   Expected: completed phases and their artifacts appear in the flat revisit menu.
7. Select one artifact such as `project-brief`.
   Expected: only that artifact rubric runs, artifact status remains `done`, and `Last Revisited` updates.
8. Invoke `/init spec` directly.
   Expected: Phase 2 runs immediately without rendering the status block.
9. Complete all phases and invoke `/init` once more.
   Expected: reports initialization complete and offers revisit options instead of a next-phase prompt.
```

- [ ] **Step 3: Capture verification evidence in the final implementation summary commit**

```bash
git status --short
```

Expected: no unexpected files beyond the intended workflow changes and verification-safe artifacts.

- [ ] **Step 4: Commit**

```bash
git add .
git commit -m "test: verify consolidated init workflow end to end"
```

---

## Self-Review

### Spec coverage

- One remembered verb (`/init`) is covered by Task 3 and Task 6.
- Visible in-progress status without opening the plan file is covered by Task 4 and Task 7.
- Targeted revisits to completed phases or artifacts are covered by Task 3 and Task 7.
- No new state files, with the plan file as sole source of truth, is preserved by Task 1 through Task 4.
- Parity across `.claude/`, `.copilot/`, and `.codex` is enforced by Task 3, Task 5, and Task 7.
- The repository’s real Codex runtime-surface note (`.agents/README.md`) is included so the migration does not leave repo-native guidance stale.

### Placeholder scan

- No `TODO`, `TBD`, or “implement later” placeholders remain.
- Every code-edit step includes concrete text, commands, or file operations.
- Verification steps name exact commands and expected output.

### Type and naming consistency

- Uses one consolidated command name throughout: `/init`.
- Uses one revisit audit field name throughout: `Last Revisited`.
- Uses the actual phase file names already present in `project-initialization/phases/`.
