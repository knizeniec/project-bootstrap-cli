---
title: Init Workflow Consolidation — Design
status: draft
record_class: spec
audience: [internal, contributors]
owner: TBD
capability: knowledge
phase: design
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Init Workflow Consolidation — Design

> **Purpose**: replace the seven phase-specific init commands with a single resume-aware `/init` command that reads the existing plan file, routes to the correct phase or artifact, and supports targeted revisits to completed work.
> **Audience**: contributors maintaining the project-initialization workflow and the per-tool command surfaces.
> **When to update**: update if the phase model, plan-file schema, or supported tool surfaces change.

## Problem

The current initialization workflow exposes seven slash commands (`/init-triage`, `/init-intent`, `/init-spec`, `/init-design`, `/init-govern`, `/init-adapt`, `/init-review`). They must be invoked in order, one per phase. This produces two recurring frictions:

1. **Recall burden.** The user has to remember which phase finished last and which command runs next. The plan file records this in `Next Recommended Step`, but the user has to open the file to see it.
2. **No targeted revisit path.** When a user finishes a phase and later decides an artifact (e.g. the project brief) is too thin, the only option is to re-run the whole phase command, which re-walks every artifact in that phase.

A third, latent friction: each tool surface (Claude, Copilot, Codex) carries seven near-identical command files, multiplying maintenance cost.

## Goals

- One verb the user has to remember: `/init`.
- Status of the in-progress initialization is visible without opening the plan file.
- Targeted revisits to completed phases or individual artifacts are first-class, not an awkward re-run.
- No new state files; the existing plan file remains the sole source of truth.
- Parity across `.claude/`, `.copilot/`, `.codex/` surfaces.

## Non-Goals

- Full automation across phases. Phases are interview-driven by design; a `--auto` flag was considered and rejected.
- Tab-completion of `/init <name>` arguments. Tool-specific; can ship later.
- Telemetry on revisit frequency. Out of scope.
- Migration grace period for old command names. The user explicitly chose a hard break (no deprecation aliases).

## Architecture

### Command surface

Each tool exposes exactly one command: `/init`.

| Invocation | Behavior |
| --- | --- |
| `/init` (no plan exists) | Treat as Phase 0 start. Run triage. |
| `/init` (plan exists, phases incomplete) | Print status block, propose next action, wait for `[Enter] continue · revisit · show plan · q`. |
| `/init` (all phases done) | Print "initialization complete" + offer revisit menu. |
| `/init <phase-or-artifact-name>` | Power-user direct jump. Bypasses the status block. Examples: `/init spec`, `/init project-brief`. |

The status block printed by the no-args path:

```
Project initialization — 2026-05-07
  ✓ Phase 0  Triage          done  2026-05-05
  ✓ Phase 1  Intent          done  2026-05-06
  ▶ Phase 2  Specification   next
    Phase 3  Design
    Phase 4  Govern & Operate
    Phase 5  Adapt
    Phase 6  Final review

Next action: run Phase 2 — Specification (PRD, requirements, journeys, acceptance).

[Enter] continue · revisit · show plan · q
```

The `revisit` path opens an interactive menu of completed phases and the artifacts under them. Selecting an artifact dispatches to that artifact's rubric in append/extend mode (the artifact rubric file decides what append/extend means in context — typically: re-read existing content, ask only what is missing or weak, never blank-overwrite).

### State model

The plan file at `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` remains the sole source of truth. No new files, no separate state cache.

One schema change to `project-initialization/plan-template.md`: the **Artifact Roadmap** gains one column.

Before:
```
| Phase | Artifact | Status | Mode (initial) | Mode (effective) | Output path |
```

After:
```
| Phase | Artifact | Status | Mode (initial) | Mode (effective) | Output path | Last Revisited |
```

`Last Revisited` is `YYYY-MM-DD` after the first revisit and empty otherwise. Revisiting an artifact does **not** reset the artifact's `Status` or the parent phase's `Status` — both remain `done`. The column is the audit trail.

The `/init` parser tolerates the absence of this column in older plan files (treats it as empty for every artifact).

### Control flow

`/init` with no arguments:

1. Locate the plan file (glob `docs/superpowers/plans/*-project-initialization.md`).
2. If absent: announce "no plan found — running Phase 0 (Triage)" and dispatch `project-initialization/phases/0-triage.md`.
3. If present: parse the Phase Roadmap. Identify the first phase whose status is not `done` — that is the "next" phase.
4. If every phase is `done`: announce completion and offer the revisit menu.
5. Otherwise: render the status block, prompt for `[Enter] / revisit / show plan / q`.
6. On Enter: dispatch to the phase rubric `project-initialization/phases/<N>-<name>.md`, applying the per-run stop rules from `project-initialization/contract.md`.
7. On `revisit`: open the revisit menu (see below).
8. On `show plan`: print the plan file path and the Phase Roadmap, Resume Context, and Next Recommended Step sections.
9. On `q`: exit silently.

`/init <name>` with an argument:

1. Resolve `<name>` against the catalog of phase names (`triage`, `intent`, `spec`, `design`, `govern`, `adapt`, `review`) and artifact names (the union of artifacts listed under `project-initialization/artifacts/`).
2. If a phase: dispatch to the phase rubric. If the phase is already `done`, run in revisit mode (the rubric uses existing content as base, asks only what's weak).
3. If an artifact: dispatch to that artifact's rubric in append/extend mode regardless of phase status. Update `Last Revisited` in the Artifact Roadmap row when done.
4. If no match: print the catalog and stop.

### Revisit menu

Triggered by typing `revisit` at the `/init` prompt. The menu is a flat list:

```
Revisit a completed item:
  1. Phase 0 — Triage (whole phase)
  2. Phase 1 — Intent (whole phase)
     a. project-brief        last revisited: never
     b. business-case        last revisited: 2026-05-06
  3. Phase 2 — Specification (whole phase)
     a. prd                  last revisited: never
     ...
Enter number (e.g. 2a) or q to cancel:
```

Selecting a whole phase runs that phase's rubric in revisit mode. Selecting a specific artifact runs only that artifact's rubric.

### SessionStart hook

A `SessionStart` hook installed in each tool's settings checks for `docs/superpowers/plans/*-project-initialization.md` on session open. Behavior:

- If the file is absent, the hook is silent.
- If the file is present and at least one phase is not `done`, the hook prints the status block followed by `Run /init to continue.`.
- If every phase is `done`, the hook is silent (initialization is over; no need to nag).

The hook is read-only — it never modifies the plan file. It is purely a visibility surface.

### Phase logic stays put

`project-initialization/phases/*.md` and `project-initialization/artifacts/*.md` are not modified by this design beyond the plan-template column change. The phase rubrics already encode interview prompts, mode selection, and end-of-phase review dispatch. The new `/init` command and its skill are pure routing; they do not duplicate phase content.

### Skill consolidation

The `init-triage`, `init-intent`, `init-spec`, `init-design`, `init-govern`, `init-adapt`, `init-review` skills (one per phase) are deleted and replaced by a single `init` skill in each tool surface. The new skill contains:

- The argument-parsing rules above.
- The status-block render template.
- The revisit-menu render template.
- Pointers to `project-initialization/phases/` and `project-initialization/artifacts/` for actual phase logic.
- The contract reference (`project-initialization/contract.md`) for per-run stop rules.

Net change: −7 skill files, +1 skill file per tool surface.

## Migration

This is a hard breaking change. No deprecation aliases.

### Files deleted

- `.claude/commands/init-triage.md`
- `.claude/commands/init-intent.md`
- `.claude/commands/init-spec.md`
- `.claude/commands/init-design.md`
- `.claude/commands/init-govern.md`
- `.claude/commands/init-adapt.md`
- `.claude/commands/init-review.md`
- `.claude/skills/init-triage.md` (and the other six init-* skills, if present as separate files; otherwise their entries in whatever skill index file exists)
- Equivalent files under `.copilot/commands/` and `.codex/commands/`
- Equivalent skill files under each tool surface

### Files added

- `.claude/commands/init.md`
- `.claude/skills/init/SKILL.md` (or whichever path the skill system expects in this template)
- `.copilot/commands/init.md` + skill equivalent
- `.codex/commands/init.md` + skill equivalent
- A small SessionStart hook script + registration entry in each tool's settings file

### Files modified

- `project-initialization/plan-template.md` — add `Last Revisited` column to Artifact Roadmap.
- `project-initialization/README.md` — replace the seven-row command table with the single `/init` entry; explain status block, revisit, and the SessionStart hook.
- `project-initialization/contract.md` — update the "Tell the user the next command" rule to reference `/init` (or remove the explicit naming and let `/init` self-route).
- `project-initialization/phases/*.md` — replace any references to the next phase command with `/init`.
- `README.md` — update the adoption sequence narrative.
- `AGENTS.md` — update the line that lists `/init-triage` first, then `/init-intent` through `/init-review`.
- Any other doc surfaced by `grep -r '/init-' .` — update to `/init` or remove.
- `CHANGELOG.md` — new entry under the next version, marked `BREAKING`.

### Parity script

`scripts/` already contains a parity script that asserts equivalent tool-surface files exist across `.claude/`, `.copilot/`, `.codex/`. Extend it to:

- Assert `init.md` exists in all three command directories.
- Assert none of the seven old `init-*.md` files exist in any of the three.
- Assert the `init` skill exists in all three skill directories.
- Assert the SessionStart hook is registered in each tool's settings.

## Testing

- Run the extended parity script. Must pass.
- Run `lychee` against the docs. No stale links to `/init-<phase>` commands.
- Manual smoke test in a fresh clone:
  1. Run `/init` with no plan file → Phase 0 (Triage) runs.
  2. Run `/init` again → status block shows Phase 0 done, prompts to continue to Phase 1.
  3. Hit Enter → Phase 1 runs.
  4. Open a fresh session → SessionStart hook prints the status block and `Run /init to continue.`.
  5. Run `/init revisit` → menu lists completed phases and artifacts. Pick `project-brief` → only that artifact's rubric runs in append mode. Confirm `Last Revisited` is updated; phase status remains `done`.
  6. Run `/init spec` directly → Phase 2 runs without showing status block.
  7. Complete all phases. Run `/init` → "initialization complete" + revisit menu offered.

## Risks and trade-offs

- **Hard break for downstream users with muscle memory.** Mitigated only by clear `CHANGELOG.md` entry and an updated README. Accepted by user.
- **Status-block parser must be resilient.** The plan file is human-edited markdown. The parser must tolerate whitespace variation, missing optional sections, and the absence of the new `Last Revisited` column. Implementation should treat any unparseable row as `pending`.
- **Revisit mode semantics depend on artifact rubrics.** This design says "revisit mode = append/extend, do not blank-overwrite," but each artifact rubric must honor this. The artifact rubric files in `project-initialization/artifacts/` already mention modes (interview / confirm / extract); revisit mode reuses `confirm` semantics for already-present content. No rubric content changes are required by this design, but if a rubric is found to fully overwrite on re-run, that is a follow-up bug, not a blocker.
- **SessionStart hook noise.** The hook prints on every session open while a plan is active. For a long initialization that's many sessions of the same status block. Acceptable: it disappears the moment the final phase is `done`.

## Open questions

None remaining at design time. All routing, granularity, migration, and parity decisions are pinned by the brainstorm dialogue above.
