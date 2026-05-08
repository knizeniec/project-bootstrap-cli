---
title: Command-First Template Adoption Implementation Plan
status: active
record_class: historical
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: planning
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Command-First Template Adoption Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the prompt-first/skill-first template adoption path with a tool-native command-first initialization workflow backed by mechanical phase gating, adaptive depth, and a single tracked plan file.

**Architecture:** Tool-neutral phase content lives in `project-initialization/` at repo root; each tool dir (`.claude/`, `.copilot/`, `.codex/`) holds thin phase commands that point at it. The full `superpowers` plugin (v5.0.7 from `/home/hexaper/.copilot/superpowers`) is vendored into each tool dir. A tracked plan file at `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` acts as the state machine across runs.

**Tech Stack:** Markdown, GitHub Actions, markdownlint-cli2, Python 3, ripgrep

---

## File Map

**Create:**
- `project-initialization/README.md` — entrypoint and shared contract summary
- `project-initialization/AGENTS.md` — authoring rules for this directory
- `project-initialization/contract.md` — stop rules, concern framing, output format
- `project-initialization/plan-template.md` — plan file scaffold
- `project-initialization/review-checklist.md` — standards + coherence checklist
- `project-initialization/phases/0-triage.md` — routing questions, profile selection, mode assignment, plan init
- `project-initialization/phases/1-intent.md` — intent phase orchestration
- `project-initialization/phases/2-specification.md` — specification phase orchestration
- `project-initialization/phases/3-design.md` — design phase orchestration
- `project-initialization/phases/4-govern-operate.md` — govern & operate phase orchestration
- `project-initialization/phases/5-adapt.md` — adapt phase orchestration
- `project-initialization/phases/6-review.md` — final review phase orchestration
- `project-initialization/artifacts/project-brief.md` — project brief rubric
- `project-initialization/artifacts/business-case.md` — business case rubric
- `project-initialization/artifacts/prd.md` — PRD rubric
- `project-initialization/artifacts/requirements-catalog.md` — requirements catalog rubric
- `project-initialization/artifacts/journeys.md` — user journeys rubric
- `project-initialization/artifacts/acceptance-catalog.md` — acceptance catalog rubric
- `project-initialization/artifacts/solution-design.md` — solution design rubric
- `project-initialization/artifacts/adr.md` — ADR rubric
- `project-initialization/artifacts/c4.md` — C4 views rubric (context / container / component)
- `project-initialization/artifacts/ai-use-policy.md` — AI use policy rubric
- `project-initialization/artifacts/test-strategy.md` — test strategy rubric
- `project-initialization/artifacts/security-baseline.md` — security baseline rubric
- `project-initialization/artifacts/delivery-plan.md` — delivery plan rubric
- `project-initialization/artifacts/language-adaptation.md` — language/stack adaptation rubric
- `project-initialization/artifacts/repo-sync.md` — repository sync rubric
- `project-initialization/profiles/product.md` — product profile artifact defaults
- `project-initialization/profiles/internal-tool.md` — internal tool profile artifact defaults
- `project-initialization/profiles/library.md` — library/utility profile artifact defaults
- `project-initialization/profiles/platform.md` — platform/regulated profile artifact defaults
- `.claude/README.md` — Claude tool dir entrypoint
- `.claude/AGENTS.md` — Claude tool dir authoring rules
- `.claude/commands/init-triage.md` — triage command
- `.claude/commands/init-intent.md` — intent phase command
- `.claude/commands/init-spec.md` — specification phase command
- `.claude/commands/init-design.md` — design phase command
- `.claude/commands/init-govern.md` — govern & operate phase command
- `.claude/commands/init-adapt.md` — adapt phase command
- `.claude/commands/init-review.md` — final review command
- `.claude/agents/init-reviewer.md` — init-specific reviewer agent
- `.copilot/README.md`, `.copilot/AGENTS.md` — Copilot tool dir
- `.copilot/commands/init-{triage,intent,spec,design,govern,adapt,review}.md` — Copilot commands (same content as .claude/)
- `.codex/README.md`, `.codex/AGENTS.md` — Codex tool dir
- `.codex/commands/init-{triage,intent,spec,design,govern,adapt,review}.md` — Codex commands

**Modify:**
- `docs/adr/ADR-001-skill-first-template-adoption.md` — rewrite in place as command-first decision
- `docs/adr/INDEX.md` — update ADR-001 row
- `README.md` — command-first journey, updated layout table, remove prompt sections
- `AGENTS.md` — updated routing and directory inventory
- `CLAUDE.md` — point to `.claude/commands/init-triage.md`
- `CONTRIBUTING.md` — add initialization workflow change guidance
- `CHANGELOG.md` — add unreleased entry
- `docs/00-source-of-truth.md` — update initialization ownership row
- `docs/INDEX.md` — update navigation entries
- `src/AGENTS.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md` — slot guidance updated
- `.github/workflows/ci.yml` — guard new files, drop prompts checks

**Delete:**
- `prompts/` (entire directory)

---

### Task 0: Rewrite ADR-001 and update INDEX

**Files:**
- Modify: `docs/adr/ADR-001-skill-first-template-adoption.md`
- Modify: `docs/adr/INDEX.md`

- [ ] **Step 1: Rewrite ADR-001 in place**

Replace the entire body of `docs/adr/ADR-001-skill-first-template-adoption.md` with:

```markdown
---
title: ADR-001 Command-First Template Adoption
status: active
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# ADR-001: Adopt tool-native commands as the primary template initialization workflow

- Status: Accepted
- Date: 2026-05-07
- Deciders: Template maintainers
- Consulted: Documentation maintainers
- Informed: Contributors and template adopters
- Tags: [Architecture, Delivery]
- Supersedes: None
- Superseded by: None
- Related documents: [../superpowers/specs/2026-05-07-command-first-template-adoption-design.md](../superpowers/specs/2026-05-07-command-first-template-adoption-design.md), [../superpowers/plans/2026-05-07-command-first-template-adoption.md](../superpowers/plans/2026-05-07-command-first-template-adoption.md)

## Context and problem statement

The repository taught adopters to initialize projects through a prompt-first sequence spread across multiple files. That flow required manual prompt selection, had no shared state across runs, and covered only three of the ten numbered documentation domains. A previous draft of this record proposed a skill-first replacement using `skills/project-initialization/` directories; that proposal was revised before any implementation because repo-included skills are not auto-discovered by any of the three supported tools, and the skill-per-tool files duplicated stage logic without mechanical gating.

## Decision drivers

- Give adopters one auto-discoverable starting point per tool after cloning.
- Enforce one-stage-per-run discipline mechanically, not through agent self-discipline.
- Keep stage content in one tool-neutral source; make tool dirs thin wrappers.
- Support adaptive depth: detailed upfront descriptions skip full interview flows.
- Preserve shared review and concern-handling rules across all supported tools.

## Considered options

### Option 1: Keep prompt-first workflow

- Pros: no new tracked surface; keeps current structure.
- Cons: no cross-run state; manual stage selection; shallow domain coverage.

### Option 2: Skill-first (previous draft of this ADR)

- Pros: richer workflow than prompts; shared contract.
- Cons: skills not auto-discovered from repo paths; tool-specific SKILL.md files duplicate stage logic; no mechanical gating.

### Option 3: Tool-native commands (chosen)

- What it is: seven phase commands per tool dir (`.claude/`, `.copilot/`, `.codex/`), each pointing at shared content in `project-initialization/`. A plan file in `docs/superpowers/plans/` is the state machine. Triage writes a tailored artifact roadmap; phase commands honor it.
- Pros: auto-discovered by each tool's native command loader; mechanical gating via plan-file precheck; single source of stage truth; adaptive depth via per-artifact modes.
- Cons: three sets of command files need parity maintenance; vendored `superpowers` plugin must be manually re-synced when upstream changes.

## Decision outcome

Adopt tool-native phase commands in `.claude/`, `.copilot/`, and `.codex/` as the primary template initialization workflow. `prompts/` is removed. The `project-initialization/` directory at repo root is the single source of stage logic, artifact rubrics, profiles, and the shared contract.

### Consequences

- `README.md`, `AGENTS.md`, and `CLAUDE.md` must point to the tool-specific init-triage command.
- The repository must maintain contract parity across the three tool dirs; CI checks enforce this.
- The full `superpowers` plugin (v5.0.7) is vendored into each tool dir and updated manually.

## More information

- An earlier draft of this record (committed 2026-05-07) proposed `skills/project-initialization/` as the primary path. That draft was revised on the same day before any implementation landed. The revision history is visible in git log.
- Vendoring source: `/home/hexaper/.copilot/superpowers` at version 5.0.7.
```

- [ ] **Step 2: Update ADR-001 row in INDEX**

Find the ADR-001 row in `docs/adr/INDEX.md` and replace it with:

```markdown
| [ADR-001](ADR-001-skill-first-template-adoption.md) | Command-First Template Adoption | Accepted | 2026-05-07 | Makes tool-native phase commands the primary adoption workflow; removes prompts/. |
```

- [ ] **Step 3: Lint ADR files**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md
```

Expected: PASS with no errors.

- [ ] **Step 4: Commit**

```bash
cd /home/hexaper/template
git add docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md
git commit -m "docs: rewrite ADR-001 as command-first adoption decision"
```

---

### Task 1: Create project-initialization/ contract layer

**Files:**
- Create: `project-initialization/README.md`
- Create: `project-initialization/AGENTS.md`
- Create: `project-initialization/contract.md`
- Create: `project-initialization/plan-template.md`
- Create: `project-initialization/review-checklist.md`

- [ ] **Step 1: Create project-initialization/AGENTS.md**

```markdown
---
name: Project Initialization Rules
description: Use for work under project-initialization/. Covers phase content, artifact rubrics, profile defaults, and shared contract maintenance.
applyTo: "project-initialization/**"
---

# Project Initialization Rules

Apply these rules when editing files under `project-initialization/`.

## Most important rules

- Keep tool-neutral. Nothing in this directory references `.claude/`, `.copilot/`, or `.codex/` by name.
- One file per artifact rubric. Artifact rubrics in `artifacts/` define questions, gating, and review hooks. Phase files in `phases/` define walk order and orchestration only.
- When changing stage names, stop rules, concern framing, or the output format, update `contract.md` first; then update any phase or artifact files that reference those rules.
- When changing the plan file structure, update `plan-template.md`; then update `phases/0-triage.md` which initializes it.
- Profile defaults in `profiles/` are starting points. Users override them at triage. Do not add tool-specific logic to profiles.
- Run `npx -y markdownlint-cli2 project-initialization/**/*.md` before committing changes here.
```

- [ ] **Step 2: Create project-initialization/README.md**

```markdown
---
title: Project Initialization
status: active
record_class: supporting
audience: [internal]
owner: Template maintainers
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Project Initialization

> **Purpose**: serve as the entrypoint and contract summary for the tool-neutral project initialization workflow.
> **Audience**: contributors maintaining the initialization workflow; AI agents reading phase content.
> **When to update**: update when the phase catalog, artifact list, or shared contract rules change.

## How initialization works

A fresh clone of this template is unspecialized. To initialize it for a specific project, run the tool-native commands in your AI tool's command surface:

- Claude: `.claude/commands/init-triage.md` (invoke as `/init-triage`)
- Copilot: `.copilot/commands/init-triage.md`
- Codex: `.codex/commands/init-triage.md`

Each tool exposes seven phase commands. They must be run in order. State is tracked in `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md`.

## Phases

| Phase | Command | Purpose |
| --- | --- | --- |
| 0. Triage | `/init-triage` | Routing questions, profile selection, artifact roadmap |
| 1. Intent | `/init-intent` | Project brief, optional business case |
| 2. Specification | `/init-spec` | PRD, requirements catalog, user journeys, acceptance catalog |
| 3. Design | `/init-design` | Solution design, ADRs, optional C4 views |
| 4. Govern & Operate | `/init-govern` | AI policy, test strategy, security baseline, delivery plan |
| 5. Adapt | `/init-adapt` | Language adaptation, repository sync |
| 6. Final review | `/init-review` | Cross-doc coherence, entry-doc parity, standards pass |

## This directory

| Path | Responsibility |
| --- | --- |
| `contract.md` | Stop rules, concern framing, output format — all commands follow this |
| `plan-template.md` | Scaffold for the plan file created by triage |
| `review-checklist.md` | Standards + coherence rubric used by `init-reviewer` |
| `phases/` | Per-phase orchestration files |
| `artifacts/` | Per-artifact question rubrics, gating criteria, review hooks |
| `profiles/` | Default artifact activations per project type |
```

- [ ] **Step 3: Create project-initialization/contract.md**

```markdown
# Project Initialization Contract

All phase commands follow these rules exactly. This file is the single authority for cross-phase behavior.

## Per-run stop rules

1. Read the plan file. If missing, stop — tell the user to run `/init-triage`.
2. Verify the previous phase is `done`. If not, stop — name the incomplete phase.
3. Run all active artifacts end-to-end without intra-phase pauses.
4. Run end-of-phase review before updating the plan.
5. Update the plan in one batch at the end of the run, never incrementally.
6. After the plan update, stop. Do not begin the next phase or read ahead for it. Tell the user the next command.

## Cross-phase fact capture

While a user is providing input for any artifact, classify each statement:
- **Current-artifact fact** — use it to produce the active artifact.
- **Current-phase other-artifact fact** — note it; it will be used when that artifact runs in this phase.
- **Future-phase fact** — write it to `Future-Phase Facts` in the plan under the relevant phase/artifact sub-section, formatted as: `- (captured during Phase N, YYYY-MM-DD): "verbatim or close paraphrase"`.
- **Off-topic** — acknowledge and redirect.

Future-phase facts may promote an artifact's mode at run time. Announce promotions: "I see we captured [X] during Phase N — promoting [artifact] from interview to confirm."

## Concern handling

When a user choice is weak, risky, or inconsistent with project facts already captured, say:

1. **Concern**: what the issue is, stated plainly.
2. **Why it matters here**: one sentence tied to the specific project context.
3. **Stronger option**: one concrete alternative.
4. **Trade-off**: what adopting the stronger option costs.
5. **Acceptable if intentional**: yes or no, with any conditions.

Record the concern in `Concerns And Recommendations` in the plan immediately — do not wait for end-of-phase. Concerns do not block the phase from completing.

## End-of-phase review

After producing all active artifacts, dispatch the `init-reviewer` agent with:
- File paths of all artifacts produced or modified this phase
- Plan file path
- Phase number
- `project-initialization/review-checklist.md`

Apply all critical and important findings before closing the run. Record minor and advisory findings in `Review Findings` and continue.

## End-of-run output format

Output these sections in order:

```
Stage Completed: Phase N — <Phase Name>
Docs Updated: <comma-separated list of files written or modified>
Review Fixes Applied: <list of critical/important findings that were fixed, or "none">
Concerns And Recommendations: <list of five-part concerns raised this run, or "none">
Parked Questions: <questions raised but not fitting any active artifact, or "none">
Next Recommended Step: Run `/<next-command>` to begin Phase N+1 (<Next Phase Name>).
```
```

- [ ] **Step 4: Create project-initialization/plan-template.md**

```markdown
---
title: Project Initialization Plan
status: active
record_class: historical
audience: [internal]
owner: TBD
capability: knowledge
phase: planning
cadence: ad-hoc
last_reviewed: YYYY-MM-DD
---

# Project Initialization Plan

## Goal
<!-- One sentence describing the project, captured during triage. -->

## Profile
- Selected profile: <!-- product | internal-tool | library | platform -->
- Profile rationale: <!-- one-line why triage picked this -->
- Overrides applied: <!-- list any user-requested deviations from profile defaults, or "none" -->

## Phase Roadmap
| Phase | Status | Notes |
| --- | --- | --- |
| 0. Triage | done | |
| 1. Intent | pending | |
| 2. Specification | pending | |
| 3. Design | pending | |
| 4. Govern & Operate | pending | |
| 5. Adapt | pending | |
| 6. Final review | pending | |

<!-- Status values: pending | in-progress | done | active-skipped | fully-skipped | blocked -->

## Artifact Roadmap
| Phase | Artifact | Status | Mode (initial) | Mode (effective) | Output path |
| --- | --- | --- | --- | --- | --- |
<!-- Add one row per artifact. Populate during triage. -->
<!-- Mode values: interview | confirm | extract | skipped -->

## Project Facts
<!-- Stable facts confirmed during triage and updated as phases complete. Re-read at the top of every run. -->

## Future-Phase Facts
<!-- Facts captured out-of-phase. Sub-sectioned by target phase/artifact. -->
<!-- Format each entry: - (captured during Phase N, YYYY-MM-DD): "fact" -->

## Open Questions For Current Phase
<!-- Questions the active phase command must resolve before completing. -->

## Parked Questions For Later
<!-- Questions raised in conversation that don't fit any active artifact. -->

## Concerns And Recommendations
<!-- Five-part advisory entries. Format:
- **Concern (Phase N, YYYY-MM-DD):** description
  - **Why it matters here:** ...
  - **Stronger option:** ...
  - **Trade-off:** ...
  - **Acceptable if intentional:** yes/no + conditions
-->

## Files Updated This Run
<!-- List of files written or modified in the most recent run. -->

## Review Findings
<!-- Severity-classified findings from end-of-phase reviews. -->
<!-- Format: - [fixed|deferred] severity: finding (location) -->

## Next Recommended Step
<!-- Next command to run and why. -->

## Resume Context
<!-- Information needed to resume cleanly in a fresh session. -->
- Active phase:
- Active artifact:
- Last user input:
- Outstanding clarifications:
```

- [ ] **Step 5: Create project-initialization/review-checklist.md**

```markdown
# Project Initialization Review Checklist

Read this file before running any review pass. This is the rubric for the `init-reviewer` agent and for end-of-phase self-review.

## Standards review (run on every artifact produced)

- Frontmatter is present and complete: `title`, `status`, `record_class`, `audience`, `owner`, `capability` are all populated. No field is left as a placeholder string.
- `status` is one of: `draft`, `proposed`, `accepted`, `active`, `superseded`, `archived`.
- `record_class` is one of: `canonical`, `supporting`, `historical`.
- `audience` contains at least one of: `internal`, `manager`, `client`, `end_user`, `auditor`.
- All internal links (`[text](path)`) resolve to existing files.
- No `[TBD: ...]` markers remain in required sections. Optional sections may carry `[TBD: ...]` only if labeled as optional.
- No implementation code, version pins, or feature stubs appear in documentation-only artifacts.
- No content from a later phase has leaked into an earlier-phase artifact (e.g., no stack choices in the project brief).

## Coherence review (run across all artifacts produced in the phase)

- Project name is identical across all artifacts produced so far.
- Target users described in the PRD match the users named in the project brief.
- Deployment target is consistent across PRD, solution design, and any ADRs.
- Tech stack choices in the solution design are covered by accepted ADRs.
- The plan file's `Project Facts` section reflects what was confirmed this phase.
- `README.md` and `AGENTS.md` still accurately describe the repository state. If not, note as `important` so the Adapt phase can fix them.
- Source-of-truth ownership in `docs/00-source-of-truth.md` covers all new canonical docs produced.

## Final review only (Phase 6)

- All produced docs are reachable from `docs/INDEX.md`.
- `README.md` project description matches the brief's one-line summary.
- `AGENTS.md` directory inventory includes all directories added during Adapt.
- `CLAUDE.md` pointer is accurate.
- No slot-template placeholder text (`<!-- slot: ... -->`) remains unfilled in any adapted file.
- All `src/AGENTS.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md` slot guidance reflects the chosen stack.

## Severity guide

- `critical` — artifact cannot serve its purpose: missing required section, factual contradiction with another canonical doc, broken link that blocks further work.
- `important` — quality problem that will cause downstream issues: vague unmeasurable requirement, ambiguous scope boundary, orphaned decision not captured in an ADR.
- `minor` — standards violation that does not block: missing optional frontmatter field, inconsistent heading level, formatting issue.
- `advisory` — concern about a user choice already recorded in `Concerns And Recommendations` in the plan. Do not duplicate.
```

- [ ] **Step 6: Lint all five new files**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/README.md project-initialization/AGENTS.md project-initialization/contract.md project-initialization/plan-template.md project-initialization/review-checklist.md
```

Expected: PASS with no errors.

- [ ] **Step 7: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/README.md project-initialization/AGENTS.md project-initialization/contract.md project-initialization/plan-template.md project-initialization/review-checklist.md
git commit -m "docs: add project-initialization contract layer"
```

---

### Task 2: Create initialization profiles

**Files:**
- Create: `project-initialization/profiles/product.md`
- Create: `project-initialization/profiles/internal-tool.md`
- Create: `project-initialization/profiles/library.md`
- Create: `project-initialization/profiles/platform.md`

- [ ] **Step 1: Create product.md**

```markdown
# Profile: Product

Use for customer-facing or externally shipped products.

## Activation triggers
- Project type = "product"
- Or: project type = other + regulatory posture = light or heavy

## Artifact defaults by phase

### Phase 1 — Intent
| Artifact | Default mode |
| --- | --- |
| project-brief | interview |
| business-case | interview |

### Phase 2 — Specification
| Artifact | Default mode |
| --- | --- |
| prd | interview |
| requirements-catalog | interview |
| journeys | interview |
| acceptance-catalog | interview |

### Phase 3 — Design
| Artifact | Default mode |
| --- | --- |
| solution-design | interview |
| adr | interview |
| c4 | interview — context level only |

### Phase 4 — Govern & Operate
| Artifact | Default mode | Override rule |
| --- | --- | --- |
| ai-use-policy | skipped | activate if AI involvement = core or consumer |
| test-strategy | interview | always active |
| security-baseline | interview | always active |
| delivery-plan | interview | always active |

### Phase 5 — Adapt
| Artifact | Default mode |
| --- | --- |
| language-adaptation | interview |
| repo-sync | extract |
```

- [ ] **Step 2: Create internal-tool.md**

```markdown
# Profile: Internal Tool

Use for tooling built for internal teams, not external customers.

## Activation triggers
- Project type = "internal tool"

## Artifact defaults by phase

### Phase 1 — Intent
| Artifact | Default mode |
| --- | --- |
| project-brief | interview |
| business-case | skipped |

### Phase 2 — Specification
| Artifact | Default mode |
| --- | --- |
| prd | interview |
| requirements-catalog | skipped |
| journeys | interview |
| acceptance-catalog | skipped |

### Phase 3 — Design
| Artifact | Default mode |
| --- | --- |
| solution-design | interview |
| adr | interview |
| c4 | skipped |

### Phase 4 — Govern & Operate
| Artifact | Default mode | Override rule |
| --- | --- | --- |
| ai-use-policy | skipped | activate if AI involvement = core |
| test-strategy | interview | always active |
| security-baseline | skipped | activate if regulatory posture = light or heavy |
| delivery-plan | interview | always active |

### Phase 5 — Adapt
| Artifact | Default mode |
| --- | --- |
| language-adaptation | interview |
| repo-sync | extract |
```

- [ ] **Step 3: Create library.md**

```markdown
# Profile: Library / Utility

Use for reusable packages, libraries, CLIs, and utilities without a primary user-facing product surface.

## Activation triggers
- Project type = "library" or "utility"

## Artifact defaults by phase

### Phase 1 — Intent
| Artifact | Default mode |
| --- | --- |
| project-brief | interview |
| business-case | skipped |

### Phase 2 — Specification
| Artifact | Default mode |
| --- | --- |
| prd | interview — focus on API/interface contract, not user journeys |
| requirements-catalog | skipped |
| journeys | skipped |
| acceptance-catalog | skipped |

### Phase 3 — Design
| Artifact | Default mode |
| --- | --- |
| solution-design | interview |
| adr | interview |
| c4 | skipped |

### Phase 4 — Govern & Operate
| Artifact | Default mode | Override rule |
| --- | --- | --- |
| ai-use-policy | skipped | activate if AI involvement = core |
| test-strategy | interview | always active |
| security-baseline | skipped | activate if regulatory posture ≠ none |
| delivery-plan | skipped | activate if team shape = multi-team |

### Phase 5 — Adapt
| Artifact | Default mode |
| --- | --- |
| language-adaptation | interview |
| repo-sync | extract |
```

- [ ] **Step 4: Create platform.md**

```markdown
# Profile: Platform / Regulated

Use for multi-team platforms, regulated-industry products, or systems with heavy compliance requirements.

## Activation triggers
- Project type = "platform"
- Or: regulatory posture = heavy (regardless of project type)

## Artifact defaults by phase

### Phase 1 — Intent
| Artifact | Default mode |
| --- | --- |
| project-brief | interview |
| business-case | interview |

### Phase 2 — Specification
| Artifact | Default mode |
| --- | --- |
| prd | interview |
| requirements-catalog | interview |
| journeys | interview |
| acceptance-catalog | interview |

### Phase 3 — Design
| Artifact | Default mode |
| --- | --- |
| solution-design | interview |
| adr | interview |
| c4 | interview — context and container levels; component level if team = multi-team |

### Phase 4 — Govern & Operate
| Artifact | Default mode | Override rule |
| --- | --- | --- |
| ai-use-policy | interview | activate if AI involvement = core or consumer |
| test-strategy | interview | always active |
| security-baseline | interview | always active |
| delivery-plan | interview | always active |

### Phase 5 — Adapt
| Artifact | Default mode |
| --- | --- |
| language-adaptation | interview |
| repo-sync | extract |
```

- [ ] **Step 5: Lint profiles**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/profiles/product.md project-initialization/profiles/internal-tool.md project-initialization/profiles/library.md project-initialization/profiles/platform.md
```

Expected: PASS with no errors.

- [ ] **Step 6: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/profiles/
git commit -m "docs: add initialization profiles"
```

---

### Task 3: Create artifact rubrics — Intent phase (project-brief, business-case)

**Files:**
- Create: `project-initialization/artifacts/project-brief.md`
- Create: `project-initialization/artifacts/business-case.md`

- [ ] **Step 1: Create project-initialization/artifacts/project-brief.md**

```markdown
# Artifact Rubric: Project Brief

- Phase: 1 (Intent)
- Output path: `docs/00_governance/00_project_brief.md`
- Template: `docs/00_governance/00_project_brief_TEMPLATE.md`
- Prerequisites: none (first artifact)

## Gating criteria

The project brief is complete when:
- Project name is present and not a placeholder.
- Problem statement is specific: names the problem, not the solution.
- Target users are identified: a qualified description, not "users" or "developers".
- A one-line project summary exists (usable as the `Goal` in the plan file).
- No `[TBD: ...]` markers remain in the executive summary or scope sections.

## Interview mode

Ask in order. Stop when gating criteria are met.

1. "What is the name of this project?"
2. "In one or two sentences: what problem does this project solve, and why does it matter to the people experiencing it?"
3. "Who are the primary users or beneficiaries? Describe them specifically — not 'users' but a qualified group."
4. "What do those people do today, without this project? What makes that inadequate?"
5. "Are there hard constraints — timeline, budget, regulatory, integration, or technical — you must acknowledge upfront?"

If any answer is a placeholder ("TBD", "not sure yet"), ask a follow-up: "What's your best current hypothesis for [that item]? We can mark it as provisional."

## Confirm mode

Present to the user:
```
From what you've shared, here's the draft project brief context:
- Name: [extracted]
- Problem: [extracted]
- Users: [extracted]
- Constraints: [extracted or "none captured yet"]

Anything wrong or missing?
```
Then ask only the questions whose gating criteria are not met.

## Extract mode

Fill `docs/00_governance/00_project_brief_TEMPLATE.md` using `Project Facts` and the current phase's `Future-Phase Facts` sub-section. Where required sections lack captured content, add `[NEEDS-REVIEW: describe what is missing]` so the user can spot gaps at review time. Present the draft and iterate until the user accepts.

## Review hooks

Pass to `init-reviewer`:
- Problem statement names a real problem, not a solution or a category ("build an API" is a solution; "teams cannot track X without manual effort" is a problem).
- Target users are specific enough to derive requirements from (not "developers" or "the business").
- No `[TBD]` or `[NEEDS-REVIEW]` remains in the executive summary, problem statement, or primary users sections.
- Frontmatter is complete per `docs/00_operating_model/04_frontmatter_schema.md`.
- Output path `docs/00_governance/00_project_brief.md` exists and is the file that was written.
```

- [ ] **Step 2: Create project-initialization/artifacts/business-case.md**

```markdown
# Artifact Rubric: Business Case

- Phase: 1 (Intent)
- Output path: `docs/00_governance/01_business_case.md`
- Template: `docs/00_governance/01_business_case_TEMPLATE.md`
- Prerequisites: project-brief (must be done)

## Gating criteria

The business case is complete when:
- Executive summary exists and is non-generic.
- Primary benefit or value driver is named and at least directionally quantified or described.
- Strategic fit is stated (why this project, why now).
- Key investment or cost acknowledgment is present (even if approximate).

## Interview mode

Ask in order:
1. "Who commissioned or is sponsoring this project, and what outcome are they accountable for?"
2. "What is the primary benefit — describe it in terms of time saved, risk reduced, revenue enabled, or cost avoided. Even a rough estimate is useful."
3. "Why is this the right time to build this? What has changed that makes this possible or urgent?"
4. "What is the rough investment expected — team size, duration, or budget range? Approximate is fine."

## Confirm mode

Present captured sponsorship, benefit, timing rationale, and investment facts. Ask only for missing gating criteria.

## Extract mode

Fill `docs/00_governance/01_business_case_TEMPLATE.md` from captured facts. Add `[NEEDS-REVIEW: ...]` for any required section lacking source material.

## Review hooks

- Benefit is concrete: not "improve efficiency" but a direction and magnitude.
- Strategic fit names the organizational context, not just the project.
- Frontmatter complete per schema.
```

- [ ] **Step 3: Lint rubrics**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/artifacts/project-brief.md project-initialization/artifacts/business-case.md
```

Expected: PASS with no errors.

- [ ] **Step 4: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/artifacts/project-brief.md project-initialization/artifacts/business-case.md
git commit -m "docs: add intent phase artifact rubrics"
```

---

### Task 4: Create artifact rubrics — Specification phase

**Files:**
- Create: `project-initialization/artifacts/prd.md`
- Create: `project-initialization/artifacts/requirements-catalog.md`
- Create: `project-initialization/artifacts/journeys.md`
- Create: `project-initialization/artifacts/acceptance-catalog.md`

- [ ] **Step 1: Create project-initialization/artifacts/prd.md**

```markdown
# Artifact Rubric: PRD

- Phase: 2 (Specification)
- Output path: `docs/02_product/01_prd.md`
- Template: `docs/02_product/01_prd_TEMPLATE.md`
- Prerequisites: project-brief

## Gating criteria

The PRD is complete when:
- Problem statement and target users are named and non-generic.
- Scope section has an explicit out-of-scope list with at least three items.
- At least one measurable success metric is defined.
- No `[TBD]` markers remain in required sections.

## Interview mode

1. "The brief names [problem] and [users]. Does the PRD scope match, or are there important differences?"
2. "What is explicitly out of scope for the first release? List at least three things we are not building."
3. "What does success look like in three to six months? Name at least one outcome you could measure."
4. "Are there competing or related products the PRD should acknowledge for differentiation?"
5. "Are there release constraints — regulatory approval, partner dependencies, platform certification — that define hard deadlines?"

## Confirm mode

Present extracted problem, users, scope hints, and metrics. Ask only for what is missing per the gating criteria.

## Extract mode

Fill `docs/02_product/01_prd_TEMPLATE.md` from `Project Facts` and Future-Phase Facts tagged for this artifact. Add `[NEEDS-REVIEW: ...]` where content is absent. Present draft and iterate.

## Review hooks

- Problem statement matches the brief's problem — no scope creep introduced in the PRD.
- Out-of-scope list exists and is not empty.
- Success metrics are measurable: not "improve UX" but "reduce time-to-first-action by 30%".
- Target users in PRD match the brief's user description.
- Frontmatter complete per schema.
```

- [ ] **Step 2: Create project-initialization/artifacts/requirements-catalog.md**

```markdown
# Artifact Rubric: Requirements Catalog

- Phase: 2 (Specification)
- Output path: `docs/02_product/02_requirements_catalog.md`
- Template: `docs/02_product/02_requirements_catalog_TEMPLATE.md`
- Prerequisites: prd

## Gating criteria

The requirements catalog is complete when:
- At least five functional requirements are listed.
- Each requirement has a priority: MUST, SHOULD, COULD, or WON'T.
- No requirement duplicates an acceptance catalog entry (different granularity).
- No requirement contains implementation detail (how, not what).

## Interview mode

1. "List the functional things the system must do — start with MUST requirements, then SHOULD."
2. "For each MUST requirement: what does failure look like? This will drive acceptance criteria later."
3. "Are there non-functional requirements (performance, availability, data retention) the architecture must respect?"
4. "What requirements did you consider and explicitly decide not to include in this release?"

## Confirm mode

Show extracted requirements from PRD and Future-Phase Facts. Ask the user to confirm priority and completeness.

## Extract mode

Pull requirements from PRD scope section and Future-Phase Facts. Structure into the catalog template. Flag any requirement that is vague enough to be unverifiable with `[NEEDS-REVIEW: make testable]`.

## Review hooks

- Each requirement is testable: a person reading it can say "this is met" or "this is not met" unambiguously.
- No requirement says "should be easy to use" or similar subjective statements.
- Priority labels are present on every row.
- Frontmatter complete per schema.
```

- [ ] **Step 3: Create project-initialization/artifacts/journeys.md**

```markdown
# Artifact Rubric: User Journeys

- Phase: 2 (Specification)
- Output path: `docs/02_product/03_user_journeys.md`
- Template: `docs/02_product/03_user_journeys_TEMPLATE.md`
- Prerequisites: prd

## Gating criteria

The user journeys artifact is complete when:
- At least one complete journey exists per primary user type named in the PRD.
- Each journey has a defined start point, at least three steps, and a success outcome.
- Each step names actor, action, and result.
- No step contains implementation detail (no UI element names, API calls, or database terms).

## Interview mode

1. "Who are the distinct user types performing meaningful actions? List them."
2. "For [primary user type]: walk me through the most important thing they do in this system, from start to success."
3. "Where does that journey break down today? What is the failure mode we are trying to eliminate?"
4. "Are there secondary journeys — less frequent but important — for the same user type?"

Repeat questions 2-4 for each user type.

## Confirm mode

Present extracted user types and any journey fragments from Future-Phase Facts. Ask for missing steps or user types.

## Extract mode

Build journeys from PRD user descriptions and any source material provided. Mark incomplete journeys with `[NEEDS-REVIEW: missing steps]`.

## Review hooks

- Actor names in journeys match the PRD's user descriptions.
- No implementation detail: no UI component names, no API endpoint names, no database terms.
- Each journey ends with a named success outcome, not an open-ended state.
- Frontmatter complete per schema.
```

- [ ] **Step 4: Create project-initialization/artifacts/acceptance-catalog.md**

```markdown
# Artifact Rubric: Acceptance Catalog

- Phase: 2 (Specification)
- Output path: `docs/02_product/05_acceptance_catalog.md`
- Template: `docs/02_product/05_acceptance_catalog_TEMPLATE.md`
- Prerequisites: prd, requirements-catalog (if active)

## Gating criteria

The acceptance catalog is complete when:
- Every MUST requirement from the requirements catalog (if active) or PRD scope has at least one acceptance criterion.
- Each criterion is binary: it is either met or not.
- No criterion requires subjective judgment to evaluate.

## Interview mode

For each MUST requirement:
1. "How would you verify that [requirement] is satisfied? Describe a test or observable outcome."
2. "What is the failure case — what would you see if this requirement is NOT met?"

## Confirm mode

Present requirements and ask the user to confirm or add acceptance criteria for each.

## Extract mode

Derive criteria from requirements and any source material. Flag ambiguous criteria with `[NEEDS-REVIEW: not binary — rephrase as pass/fail]`.

## Review hooks

- Every MUST requirement has at least one criterion.
- Criteria are binary: no "should feel fast" or "should look clean".
- Criteria do not duplicate the PRD's out-of-scope list (anti-requirements are not acceptance criteria).
- Frontmatter complete per schema.
```

- [ ] **Step 5: Lint specification rubrics**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/artifacts/prd.md project-initialization/artifacts/requirements-catalog.md project-initialization/artifacts/journeys.md project-initialization/artifacts/acceptance-catalog.md
```

Expected: PASS with no errors.

- [ ] **Step 6: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/artifacts/prd.md project-initialization/artifacts/requirements-catalog.md project-initialization/artifacts/journeys.md project-initialization/artifacts/acceptance-catalog.md
git commit -m "docs: add specification phase artifact rubrics"
```

---

### Task 5: Create artifact rubrics — Design phase

**Files:**
- Create: `project-initialization/artifacts/solution-design.md`
- Create: `project-initialization/artifacts/adr.md`
- Create: `project-initialization/artifacts/c4.md`

- [ ] **Step 1: Create project-initialization/artifacts/solution-design.md**

```markdown
# Artifact Rubric: Solution Design

- Phase: 3 (Design)
- Output path: `docs/03_architecture/01_solution_design.md`
- Template: `docs/03_architecture/01_solution_design_TEMPLATE.md`
- Prerequisites: prd (and requirements-catalog if active)

## Gating criteria

The solution design is complete when:
- System shape is decided (monolith / services / hybrid) and an ADR exists for it.
- Primary data store is chosen and an ADR exists for it.
- Deployment target is chosen and an ADR exists for it.
- Non-functional requirements from the PRD are addressed (scale, latency, reliability).
- No `[TBD]` in core sections (context, constraints, solution strategy, building blocks).

## Interview mode

Before asking questions, invoke `superpowers:brainstorming` to explore 2-3 architectural approaches. Present the approaches with trade-offs and a recommendation before asking the user to decide.

After the user chooses an approach, ask:
1. "What language and primary framework will you use? If already decided by an ADR, confirm."
2. "What is your primary data store? Relational, document, or other — and which specific technology?"
3. "Where will this system run? Cloud provider and service type (container, serverless, VM, managed service)."
4. "What are the scale characteristics: expected concurrent users, request volume, data volume?"
5. "Are there integration constraints — existing systems, partner APIs, internal platforms this must connect to?"

## Confirm mode

Present extracted stack, deployment, and scale facts. Show which questions were pre-answered. Ask only for missing gating criteria.

## Extract mode

Build the solution design from all captured architecture facts. Mark undecided areas as `[NEEDS-REVIEW: decision required]` rather than silently choosing.

## Review hooks

- Every durable decision (language, framework, data store, deployment target, auth approach) has a corresponding ADR in `docs/adr/`.
- Non-functional requirements from the PRD are addressed — not ignored.
- No implementation code, version pins, or module names appear in the design (those belong in ADRs or implementation docs).
- Scale characteristics match what the PRD implies.
- Frontmatter complete per schema.
```

- [ ] **Step 2: Create project-initialization/artifacts/adr.md**

```markdown
# Artifact Rubric: ADR

- Phase: 3 (Design) — created on demand as decisions arise
- Output path: `docs/adr/ADR-NNN-<decision-slug>.md`
- Template: `docs/adr/ADR-000-template.md`
- Prerequisites: none (created as decisions are made)

## When to create an ADR

Create an ADR whenever a durable decision is made during Design (or any phase) that:
- Cannot easily be reversed once implemented.
- Affects how the codebase or system is structured.
- Would surprise a future contributor if it were changed without explanation.

Common triggers: language choice, framework choice, data store choice, deployment model, auth model, security posture, external service commitment.

## Gating criteria

An ADR is complete when:
- Status is `accepted` or `proposed` (never blank).
- Exactly one decision outcome is stated.
- At least two options were considered.
- Consequences (positive and negative) are listed.

## Interview mode

For each decision identified during the Design phase:
1. "What is the decision in one sentence?"
2. "What options did you consider? List at least two."
3. "What drove the choice — constraint, preference, evidence, or prior art?"
4. "What does this decision cost or risk?"

## ADR index update

After writing each ADR, add a row to `docs/adr/INDEX.md`. Format:
```
| [ADR-NNN](ADR-NNN-slug.md) | Short title | Accepted | YYYY-MM-DD | One-line summary. |
```

## Review hooks

- ADR slug matches the decision (not generic like "ADR-001-tech-choice").
- Status is explicitly set to `accepted` or `proposed`.
- Exactly one decision outcome in the "Decision outcome" section.
- `docs/adr/INDEX.md` row is present.
- Frontmatter complete per schema.
```

- [ ] **Step 3: Create project-initialization/artifacts/c4.md**

```markdown
# Artifact Rubric: C4 Views

- Phase: 3 (Design)
- Output paths:
  - Context level: `docs/03_architecture/02_c4_context.md`
  - Container level: `docs/03_architecture/03_c4_container.md`
  - Component level: `docs/03_architecture/04_c4_component.md`
- Templates: `docs/03_architecture/02_c4_context_TEMPLATE.md`, `03_c4_container_TEMPLATE.md`, `04_c4_component_TEMPLATE.md`
- Prerequisites: solution-design

## Activation levels

The profile and triage overrides determine which levels are activated:
- Context only (default for product profile)
- Context + Container (platform profile or multi-team)
- Context + Container + Component (platform profile + multi-team, or explicit override)

## Gating criteria

A C4 view is complete when:
- Context: system boundary is drawn, external actors are named, relationships are labeled.
- Container: each deployable unit is named with its technology choice; communication paths are labeled.
- Component: internal components of the named container are identified with their responsibilities.

## Interview mode

For context level:
1. "Name the external actors — people or systems that interact with your system."
2. "For each external actor: what does it send to, or receive from, your system?"
3. "Are there external systems your system depends on (data stores, APIs, auth providers)?"

For container level (if activated):
4. "List the deployable units: web app, API server, background worker, database, message queue."
5. "Which container handles which user request types?"

## Confirm mode

Present actors, containers, and communication facts extracted from solution design and Future-Phase Facts. Ask only for missing elements.

## Review hooks

- External actor names match the user types in the PRD.
- All external system dependencies from the solution design appear in the context diagram.
- Container technology choices match the solution design and ADRs.
- Frontmatter complete per schema on each file produced.
```

- [ ] **Step 4: Lint design rubrics**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/artifacts/solution-design.md project-initialization/artifacts/adr.md project-initialization/artifacts/c4.md
```

Expected: PASS with no errors.

- [ ] **Step 5: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/artifacts/solution-design.md project-initialization/artifacts/adr.md project-initialization/artifacts/c4.md
git commit -m "docs: add design phase artifact rubrics"
```

---

### Task 6: Create artifact rubrics — Govern & Adapt phases

**Files:**
- Create: `project-initialization/artifacts/ai-use-policy.md`
- Create: `project-initialization/artifacts/test-strategy.md`
- Create: `project-initialization/artifacts/security-baseline.md`
- Create: `project-initialization/artifacts/delivery-plan.md`
- Create: `project-initialization/artifacts/language-adaptation.md`
- Create: `project-initialization/artifacts/repo-sync.md`

- [ ] **Step 1: Create project-initialization/artifacts/ai-use-policy.md**

```markdown
# Artifact Rubric: AI Use Policy

- Phase: 4 (Govern & Operate)
- Output path: `docs/04_ai_governance/01_ai_use_policy.md`
- Template: `docs/04_ai_governance/` (use first TEMPLATE file found)
- Prerequisites: solution-design

## Gating criteria

The AI use policy is complete when:
- Allowed AI use cases are listed (at minimum: which AI capabilities are used).
- Prohibited uses or limitations are stated.
- Human-in-the-loop requirements are described for any AI-driven decisions.
- Data handling: which user data, if any, is sent to external AI providers.

## Interview mode

1. "Which AI capabilities does this project use — generation, classification, retrieval, or other?"
2. "What data is sent to AI providers? Does it include PII, business-sensitive, or regulated data?"
3. "For AI-driven decisions: which decisions require human review or approval before acting?"
4. "What uses of AI are explicitly prohibited in this project (to prevent misuse or scope creep)?"

## Confirm mode

Present extracted AI involvement facts from triage and Future-Phase Facts. Ask only for policy gaps.

## Review hooks

- Data handling statement covers PII and regulated data — not silent on what is sent externally.
- Prohibited use list exists and is non-empty.
- Human-in-the-loop requirements are named for any decision affecting users.
- Policy aligns with regulatory posture selected at triage.
- Frontmatter complete per schema.
```

- [ ] **Step 2: Create project-initialization/artifacts/test-strategy.md**

```markdown
# Artifact Rubric: Test Strategy

- Phase: 4 (Govern & Operate)
- Output path: `docs/05_testing_acceptance/01_test_strategy.md`
- Template: `docs/05_testing_acceptance/` (use first TEMPLATE file found)
- Prerequisites: solution-design, requirements-catalog (if active)

## Gating criteria

The test strategy is complete when:
- Testing levels are defined per component type (unit, integration, E2E, contract, performance as applicable).
- Automated vs. manual split is stated.
- At least one quality gate for CI/CD is named.
- Coverage expectation is stated (even if directional, e.g., "unit coverage for all business logic").

## Interview mode

1. "What types of testing are most important for this project: unit, integration, end-to-end, contract, performance, security?"
2. "Which tests will be automated and run in CI? Which will remain manual?"
3. "What are the quality gates — what must pass before a PR merges? Before a release?"
4. "Are there components where testing is difficult or expensive? How will you handle them?"

## Confirm mode

Present testing approach facts captured from solution design and Future-Phase Facts. Ask only for missing gating criteria.

## Review hooks

- Testing levels named in the strategy match the component types in the solution design.
- At least one CI gate is named.
- Manual test scope is explicitly bounded — not "everything not automated".
- Frontmatter complete per schema.
```

- [ ] **Step 3: Create project-initialization/artifacts/security-baseline.md**

```markdown
# Artifact Rubric: Security Baseline

- Phase: 4 (Govern & Operate)
- Output path: `docs/06_security_operations/01_security_baseline.md`
- Template: `docs/06_security_operations/` (use first TEMPLATE file found)
- Prerequisites: solution-design

## Gating criteria

The security baseline is complete when:
- Threat surface is identified (what attack vectors are in scope).
- At least three controls are named.
- Data classification is stated (what data is processed and its sensitivity level).
- Incident response owner is named.

## Interview mode

1. "Who are the likely threat actors — external attackers, internal misuse, supply chain?"
2. "What data does this system process? Classify: public, internal, confidential, regulated."
3. "What are the three most important security controls for this system?"
4. "Who is responsible for security incidents — is there an on-call rotation, a security team, or a named owner?"

## Confirm mode

Present data handling and regulatory posture facts from triage and captured tech stack. Ask for controls and ownership.

## Review hooks

- Controls are specific enough to verify (not "follow OWASP" but "input validation on all user-submitted fields").
- Data classification aligns with regulatory posture selected at triage.
- Incident response owner is a role or team, not anonymous.
- Frontmatter complete per schema.
```

- [ ] **Step 4: Create project-initialization/artifacts/delivery-plan.md**

```markdown
# Artifact Rubric: Delivery Plan

- Phase: 4 (Govern & Operate)
- Output path: `docs/07_delivery/01_delivery_plan.md`
- Template: `docs/07_delivery/` (use first TEMPLATE file found)
- Prerequisites: prd, solution-design

## Gating criteria

The delivery plan is complete when:
- At least two phases or milestones are defined.
- Each milestone has a measurable success criterion.
- Team responsibilities are stated (who owns what).
- Dependencies between milestones are noted.

## Interview mode

1. "How do you plan to deliver this: iterative sprints, phased releases, big-bang, or another approach?"
2. "What are the key milestones? For each: what does 'done' mean?"
3. "Who owns each delivery area? Name teams or roles, not individuals."
4. "What are the critical dependencies — things that must happen before a milestone can start?"

## Confirm mode

Present scope and timeline hints from PRD and Future-Phase Facts. Ask for milestone structure and ownership.

## Review hooks

- Milestone success criteria are measurable — not "the feature is complete" but a named, verifiable outcome.
- All PRD MUST requirements are covered by at least one milestone.
- Dependencies are stated directionally (A before B), not implied.
- Frontmatter complete per schema.
```

- [ ] **Step 5: Create project-initialization/artifacts/language-adaptation.md**

```markdown
# Artifact Rubric: Language Adaptation

- Phase: 5 (Adapt)
- Output paths: `src/`, `tests/`, `config/`, `.gitignore`, `.editorconfig`, `src/AGENTS.md`
- Template: n/a — adapts existing slot directories based on canonical docs
- Prerequisites: solution-design, all accepted ADRs

## Gating criteria

Language adaptation is complete when:
- Language/runtime is chosen and reflected in `src/` structure.
- Primary framework is chosen and reflected in `src/` structure.
- Test runner is chosen and reflected in `tests/`.
- `.gitignore` covers the chosen language's runtime artifacts.
- `src/AGENTS.md` slot placeholders are filled with the chosen stack's conventions.

## Interview mode

Read all accepted ADRs first. For any stack decision not yet captured in an ADR:

1. "What language and runtime version will this project use?"
2. "What is the primary framework?"
3. "What test runner and assertion library will you use?"
4. "What package manager: npm, yarn, pip, cargo, go modules, or other?"
5. "What CI platform — GitHub Actions, GitLab CI, or other?"

If an ADR already captures a decision, confirm it rather than re-asking.

## Confirm mode

Present ADR-captured stack decisions. Ask only for what is missing.

## Extract mode

Read ADRs and build adapted directory structure from them. Do not add feature code, dependency version pins not in ADRs, or implementation stubs.

## Review hooks

- No feature code added — only structural scaffolding.
- `.gitignore` covers the runtime's artifact patterns.
- `src/AGENTS.md` slot guidance names the actual language and framework, not placeholder text.
- Adapted structure matches what the solution design described.
- Frontmatter in `src/AGENTS.md` is complete per schema.
```

- [ ] **Step 6: Create project-initialization/artifacts/repo-sync.md**

```markdown
# Artifact Rubric: Repository Sync

- Phase: 5 (Adapt)
- Output paths: `README.md`, `AGENTS.md`, `CLAUDE.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md`
- Template: n/a — updates existing files to match canonical docs
- Prerequisites: language-adaptation (must be done first)

## Gating criteria

Repository sync is complete when:
- `README.md` project name and description match the project brief.
- `AGENTS.md` directory inventory includes all new directories and removes template-only guidance.
- All slot READMEs (`bin/`, `diagrams/`, `examples/`) describe their role in the specific project.
- No entry surface still references the template's default initialization guidance.

## Mode: extract only

Repo sync always runs in extract mode — it reads canonical docs and updates entry surfaces to match. It does not ask questions.

Steps:
1. Read `docs/00_governance/00_project_brief.md` for project name and one-line summary.
2. Update `README.md`: replace template description with project name and summary; update the layout table to reflect actual directories in use; remove the initialization journey diagram (it is no longer needed post-init).
3. Update `AGENTS.md`: update directory inventory; update routing guidance to reference project-specific docs instead of template guidance.
4. Update slot READMEs to describe their actual role in this project.
5. Verify `CLAUDE.md` still accurately points to `.claude/commands/init-triage.md` (no change needed post-init, but confirm).

## Review hooks

- No template boilerplate remains in README.md or AGENTS.md.
- `README.md` project description matches the brief verbatim or closely paraphrased.
- Slot READMEs are project-specific, not generic template guidance.
- No broken links introduced in any updated file.
```

- [ ] **Step 7: Lint govern and adapt rubrics**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/artifacts/ai-use-policy.md project-initialization/artifacts/test-strategy.md project-initialization/artifacts/security-baseline.md project-initialization/artifacts/delivery-plan.md project-initialization/artifacts/language-adaptation.md project-initialization/artifacts/repo-sync.md
```

Expected: PASS with no errors.

- [ ] **Step 8: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/artifacts/ai-use-policy.md project-initialization/artifacts/test-strategy.md project-initialization/artifacts/security-baseline.md project-initialization/artifacts/delivery-plan.md project-initialization/artifacts/language-adaptation.md project-initialization/artifacts/repo-sync.md
git commit -m "docs: add govern and adapt phase artifact rubrics"
```

---

### Task 7: Create phase files

**Files:**
- Create: `project-initialization/phases/0-triage.md`
- Create: `project-initialization/phases/1-intent.md`
- Create: `project-initialization/phases/2-specification.md`
- Create: `project-initialization/phases/3-design.md`
- Create: `project-initialization/phases/4-govern-operate.md`
- Create: `project-initialization/phases/5-adapt.md`
- Create: `project-initialization/phases/6-review.md`

- [ ] **Step 1: Create project-initialization/phases/0-triage.md**

```markdown
# Phase 0: Triage

## Purpose

Produce the plan file that all subsequent phase commands rely on. Write the Phase Roadmap, Artifact Roadmap, and initial Project Facts. Do not produce any canonical documentation.

## Precheck

If `docs/superpowers/plans/*-project-initialization.md` already exists, stop: "An initialization plan already exists. Run the next phase command, or delete the plan file and re-run `/init-triage` to start over."

## Step 1 — Routing questions

Ask these five questions in order. Do not skip any.

1. **Project type** (pick one): product, internal tool, library/utility, platform/regulated, other (describe).
2. **AI involvement** (pick one): none — no AI features in the product itself; consumer — calls third-party AI APIs; core — AI model behavior is central to the product.
3. **Regulatory posture** (pick one): none — standard software product; light — general data privacy (GDPR, CCPA); heavy — industry-specific regulation (HIPAA, PCI, financial services, government).
4. **Team shape** (pick one): solo, small team (2-5 people), multi-team.
5. **Existing description** (free text, optional): "If you have anything written about this project — a README draft, a requirements doc, an email thread, a design note — paste it here. The more you share, the fewer questions I'll need to ask in later phases. Or leave blank."

## Step 2 — Profile selection

Map type + posture to profile:

| Project type | Regulatory posture | Profile |
| --- | --- | --- |
| product | any | product |
| internal tool | none or light | internal-tool |
| internal tool | heavy | platform |
| library/utility | none or light | library |
| library/utility | heavy | platform |
| platform/regulated | any | platform |
| other | none or light | internal-tool (note the edge case) |
| other | heavy | platform |

Read the chosen profile from `project-initialization/profiles/<profile>.md`.

## Step 3 — Fact extraction

If the user pasted a project description, run an extraction pass:
- Pull project name, one-line summary, primary users, deployment hints, stack hints, integration hints into `Project Facts`.
- Pull architecture or stack details into `Future-Phase Facts / Phase 3 — solution-design`.
- Pull governance or compliance mentions into `Future-Phase Facts / Phase 4 — security-baseline` or `Phase 4 — ai-use-policy`.
- Pull testing mentions into `Future-Phase Facts / Phase 4 — test-strategy`.
- Ignore content that does not fit any artifact.

## Step 4 — Artifact Roadmap construction

Start from the profile's artifact defaults. Apply rule-based prunes:

- AI involvement = none → skip `ai-use-policy` regardless of profile.
- Regulatory posture = none → skip `security-baseline` and `delivery-plan` if profile = library.
- Team shape = solo → skip `delivery-plan` unless profile = product or platform.
- Any artifact for which all required content was extracted → set initial mode to `confirm` or `extract` per these rules:
  - `extract`: sufficient facts captured to draft the artifact without questions.
  - `confirm`: partial facts; command will show extracted content and ask targeted gap questions.
  - `interview`: no usable facts; full question flow.

Show the proposed Artifact Roadmap to the user with one-line mode reasoning per artifact. Example: "PRD is `confirm` because the pasted description gave us users and goals but not scope boundaries."

Ask: "Any artifact you want to activate, deactivate, or change the mode for?" Apply changes and record them under `Overrides applied` in the plan.

## Step 5 — Write the plan file

Initialize `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` from `project-initialization/plan-template.md`. Populate:
- `Goal` — one sentence from project name and summary.
- `Profile` — selected profile and rationale.
- `Phase Roadmap` — all phases as `pending` (Phase 0 = `done`).
- `Artifact Roadmap` — all rows with initial and effective mode (same at this point), status `pending` or `skipped`.
- `Project Facts` — everything extracted in Step 3.
- `Future-Phase Facts` — sub-sections for any future-phase facts extracted in Step 3.

## Output

```
Stage Completed: Phase 0 — Triage
Docs Updated: docs/superpowers/plans/YYYY-MM-DD-project-initialization.md
Review Fixes Applied: none
Concerns And Recommendations: <list or "none">
Parked Questions: <list or "none">
Next Recommended Step: Run `/init-intent` to begin Phase 1 (Intent).
```
```

- [ ] **Step 2: Create project-initialization/phases/1-intent.md**

```markdown
# Phase 1: Intent

## Purpose

Produce the project brief and, if activated, the business case. Establish the "why" before moving to specifications.

## Artifact walk order

1. `project-brief` (always active)
2. `business-case` (if active in Artifact Roadmap)

For each, load `project-initialization/artifacts/<name>.md` and follow the mode assigned in the plan.

## Phase-specific guidance

- Do not discuss technical stack, architecture, or implementation in this phase. If the user raises them, capture as Future-Phase Facts for Phase 3 and redirect: "That's a great design decision — I've noted it for Phase 3 (Design). For now, let's focus on the problem and who it's for."
- After producing the project brief, update `Project Facts` with confirmed project name and one-line summary before moving to the business case.

## Future-phase fact scope

Route captured statements to:
- Specification hints (requirements, scope, user needs) → `Future-Phase Facts / Phase 2 — prd`
- Stack, architecture, deployment → `Future-Phase Facts / Phase 3 — solution-design`
- Compliance, security, AI use → `Future-Phase Facts / Phase 4` (appropriate artifact)
- Testing approach → `Future-Phase Facts / Phase 4 — test-strategy`

## End-of-phase review

Dispatch `init-reviewer` with:
- Files produced: `docs/00_governance/00_project_brief.md` and `docs/00_governance/01_business_case.md` (if produced)
- Phase number: 1
- Checklist: `project-initialization/review-checklist.md`
```

- [ ] **Step 3: Create project-initialization/phases/2-specification.md**

```markdown
# Phase 2: Specification

## Purpose

Produce the PRD and active specification artifacts. Define what the product does and for whom, verifiably.

## Prerequisite check

Before starting, read `docs/00_governance/00_project_brief.md`. If it has unresolved `[TBD]` or `[NEEDS-REVIEW]` markers in the problem statement, users, or one-line summary sections, stop and tell the user: "The project brief has unresolved gaps that will create ambiguity in the PRD. Resolve them first, or override to continue anyway."

## Artifact walk order

1. `prd` (always active)
2. `requirements-catalog` (if active)
3. `journeys` (if active)
4. `acceptance-catalog` (if active)

For each, load `project-initialization/artifacts/<name>.md` and follow the mode assigned in the plan.

## Phase-specific guidance

- Architecture choices that emerge here belong in Future-Phase Facts for Phase 3, not in the PRD itself.
- If the user describes a solution in answer to a requirements question, gently redirect: "That sounds like an architectural choice — I've noted it for Phase 3. In the requirements, let's describe what the system must do, not how."
- The PRD's out-of-scope list is load-bearing. If the user is reluctant to define it, raise a concern using the five-part frame.

## Future-phase fact scope

Route to:
- Architecture choices → `Future-Phase Facts / Phase 3 — solution-design`
- Security or compliance specifics → `Future-Phase Facts / Phase 4 — security-baseline`
- AI use specifics → `Future-Phase Facts / Phase 4 — ai-use-policy`
- Delivery timeline hints → `Future-Phase Facts / Phase 4 — delivery-plan`

## End-of-phase review

Dispatch `init-reviewer` with all specification files produced and phase number 2.
```

- [ ] **Step 4: Create project-initialization/phases/3-design.md**

```markdown
# Phase 3: Design

## Purpose

Make durable technical decisions traceable and documented. Produce the solution design and ADRs for every durable decision.

## Artifact walk order

1. `solution-design`
2. `adr` (created on demand as decisions arise during solution-design — multiple ADRs may result)
3. `c4` (if active — run after solution-design is complete)

For each, load `project-initialization/artifacts/<name>.md` and follow the mode assigned in the plan.

## Phase-specific guidance

- Before running `solution-design` in interview mode, invoke `superpowers:brainstorming` to explore 2-3 architectural approaches. Present approaches with trade-offs and a recommendation. Wait for the user to choose before writing the solution design.
- Every time a durable decision is made during solution-design — language, framework, data store, auth model, deployment target, external service commitment — immediately create an ADR for it using `project-initialization/artifacts/adr.md`. Do not defer ADR creation to end-of-phase.
- Update `docs/adr/INDEX.md` after each ADR is written.
- Check Future-Phase Facts for this phase before starting. Any architecture, stack, or deployment facts captured in earlier phases should be acknowledged: "I see we noted [X] during Phase 1 — confirming this before including it in the solution design."

## Future-phase fact scope

Route to:
- Test approach from design decisions → `Future-Phase Facts / Phase 4 — test-strategy`
- Security implications of architectural choices → `Future-Phase Facts / Phase 4 — security-baseline`
- Deployment decisions relevant to language adaptation → `Future-Phase Facts / Phase 5 — language-adaptation`

## End-of-phase review

Dispatch `init-reviewer` with solution design, all ADRs produced, C4 files (if produced), updated `docs/adr/INDEX.md`, and phase number 3.
```

- [ ] **Step 5: Create project-initialization/phases/4-govern-operate.md**

```markdown
# Phase 4: Govern & Operate

## Purpose

Make operational, quality, and risk decisions traceable. Produce the active govern & operate artifacts per the profile.

## Artifact walk order

1. `ai-use-policy` (if active)
2. `test-strategy`
3. `security-baseline` (if active)
4. `delivery-plan` (if active)

For each, load `project-initialization/artifacts/<name>.md` and follow the mode assigned in the plan.

## Phase-specific guidance

- Check Future-Phase Facts for this phase first. Facts from Specification (data classification, compliance requirements) and Design (deployment target, tech stack) are often pre-populated here.
- If the user says "we don't need a test strategy," raise a concern (five-part frame). Test strategy is always active in non-library profiles.
- If security baseline is skipped but regulatory posture is light or heavy, raise a concern.

## Future-phase fact scope

Route to:
- Stack-specific test tooling choices → `Future-Phase Facts / Phase 5 — language-adaptation`
- Deployment and infra specifics → `Future-Phase Facts / Phase 5 — language-adaptation`

## End-of-phase review

Dispatch `init-reviewer` with all govern & operate artifacts produced and phase number 4.
```

- [ ] **Step 6: Create project-initialization/phases/5-adapt.md**

```markdown
# Phase 5: Adapt

## Purpose

Specialize the repository structure to match all canonical docs produced in phases 1-4.

## Artifact walk order

1. `language-adaptation`
2. `repo-sync`

For each, load `project-initialization/artifacts/<name>.md` and follow the mode assigned in the plan.

## Phase-specific guidance

- Read all accepted ADRs before running `language-adaptation`. ADRs are the source of truth for stack decisions. If an ADR contradicts something the user says in this phase, raise a concern.
- Do not add feature code, implementation stubs, or dependency version pins beyond what an accepted ADR explicitly requires. If unsure, err on the side of leaving a slot placeholder.
- `docs/09_user_documentation/` is deliberately not touched in this phase. It is produced post-initialization alongside features.
- After `language-adaptation` is complete, run `repo-sync` — it reads the brief and updates `README.md`, `AGENTS.md`, and slot READMEs to match the actual project.

## End-of-phase review

Dispatch `init-reviewer` with all adapted files and phase number 5. The review is especially important here: scope drift (feature code appearing in an initialization pass) is the most common failure.
```

- [ ] **Step 7: Create project-initialization/phases/6-review.md**

```markdown
# Phase 6: Final Review

## Purpose

Validate that all produced artifacts are consistent, complete, and properly linked. Declare initialization complete or identify what must be fixed.

## Precheck

All phases 1-5 must be `done`. If any are not, stop and list the incomplete phases.

## Review steps

Run these in order using `init-reviewer` with the full artifact list:

**1. Cross-doc coherence**
- Project name is identical in: brief, PRD, solution design, README.
- Target users described in the PRD match the brief's user description.
- Deployment target is consistent across PRD, solution design, ADRs, and language adaptation.
- Tech stack choices in solution design are covered by accepted ADRs.

**2. Entry-doc parity**
- `README.md` project description matches the brief's one-line summary.
- `AGENTS.md` directory inventory includes all directories created or adapted during Phase 5.
- `CLAUDE.md` pointer is accurate.

**3. Completeness check**
- All active artifacts in the Artifact Roadmap are `done`.
- All produced docs are reachable from `docs/INDEX.md`.
- No `[TBD]` or `[NEEDS-REVIEW]` markers remain in any produced doc.

**4. Standards pass**
- All produced docs pass markdownlint.
- All produced docs pass the docs validator: `PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs docs/_examples`

## Verdict

- **Pass**: All findings resolved or `advisory`. Mark Phase 6 `done`. Declare initialization complete. Suggest commit: `"docs: complete project initialization"`.
- **Concerns remain**: Leave Phase 6 `in-progress`. List what must be fixed before declaring complete. The user runs `/init-review` again after fixes.

## Output

```
Stage Completed: Phase 6 — Final Review
Docs Updated: <plan file Review Findings section>
Review Fixes Applied: <list or "none">
Concerns And Recommendations: <list or "none">
Parked Questions: <list or "none">
Next Recommended Step: <commit suggestion if passing, or fix list if not>
```
```

- [ ] **Step 8: Lint all phase files**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 project-initialization/phases/0-triage.md project-initialization/phases/1-intent.md project-initialization/phases/2-specification.md project-initialization/phases/3-design.md project-initialization/phases/4-govern-operate.md project-initialization/phases/5-adapt.md project-initialization/phases/6-review.md
```

Expected: PASS with no errors.

- [ ] **Step 9: Commit**

```bash
cd /home/hexaper/template
git add project-initialization/phases/
git commit -m "docs: add initialization phase orchestration files"
```

---

### Task 8: Create .claude/ commands and init-reviewer agent

**Files:**
- Create: `.claude/AGENTS.md`
- Create: `.claude/README.md`
- Create: `.claude/commands/init-triage.md`
- Create: `.claude/commands/init-intent.md`
- Create: `.claude/commands/init-spec.md`
- Create: `.claude/commands/init-design.md`
- Create: `.claude/commands/init-govern.md`
- Create: `.claude/commands/init-adapt.md`
- Create: `.claude/commands/init-review.md`
- Create: `.claude/agents/init-reviewer.md`

- [ ] **Step 1: Create .claude/AGENTS.md**

```markdown
---
name: Claude Tool Directory Rules
description: Use for work under .claude/. Covers commands, agents, and vendored superpowers maintenance.
applyTo: ".claude/**"
---

# Claude Tool Directory Rules

## Most important rules

- Commands in `.claude/commands/` must stay thin. Logic lives in `project-initialization/`. If a command grows beyond ~25 lines of body text, the logic belongs in a phase file, not the command.
- `.claude/skills/superpowers/` and `.claude/agents/superpowers/` are vendored from `/home/hexaper/.copilot/superpowers` at version 5.0.7. Do not edit them directly. When upstream changes, re-sync all three tool dirs together.
- Keep `.claude/`, `.copilot/`, `.codex/` in parity for the seven `init-*` commands. If you change a command in one tool dir, make the equivalent change in the other two.
- CI runs a parity check. Do not break it.
```

- [ ] **Step 2: Create .claude/README.md**

```markdown
# Claude Tool Directory

This directory contains Claude Code's per-project configuration for this repository.

## Project initialization commands

| Command | Phase | Purpose |
| --- | --- | --- |
| `/init-triage` | 0 | Routing questions, profile, artifact roadmap, plan file |
| `/init-intent` | 1 | Project brief, optional business case |
| `/init-spec` | 2 | PRD, requirements catalog, journeys, acceptance catalog |
| `/init-design` | 3 | Solution design, ADRs, optional C4 views |
| `/init-govern` | 4 | AI policy, test strategy, security baseline, delivery plan |
| `/init-adapt` | 5 | Language adaptation, repository sync |
| `/init-review` | 6 | Final coherence review, declare initialization complete |

Run these commands in order. Each command reads the plan file at `docs/superpowers/plans/*-project-initialization.md` and refuses to run if prerequisites are unmet.

## Vendored superpowers

`skills/superpowers/` and `agents/superpowers/` contain the full superpowers plugin v5.0.7 vendored from `/home/hexaper/.copilot/superpowers`. Available for use in any session in this repository.
```

- [ ] **Step 3: Create .claude/commands/init-triage.md**

```markdown
---
description: "Run Phase 0 (Triage) of project initialization — ask routing questions, select profile, build artifact roadmap, write plan file."
---

You are running **Phase 0 (Triage)** of the project initialization workflow for this repository.

**Before doing anything else:** check whether `docs/superpowers/plans/*-project-initialization.md` already exists. If it does, stop: "An initialization plan already exists. Run the next phase command, or delete the plan file and re-run `/init-triage` to start over."

**Follow `project-initialization/phases/0-triage.md` exactly.** That file defines the routing questions, profile selection matrix, fact extraction rules, mode assignment logic, and plan-file initialization instructions.

**After triage is complete:** write the plan file to `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` using today's date. Use `project-initialization/plan-template.md` as the scaffold.

**Stop after writing the plan file.** Output the end-of-run summary from `project-initialization/contract.md`. Do not begin Phase 1.
```

- [ ] **Step 4: Create .claude/commands/init-intent.md**

```markdown
---
description: "Run Phase 1 (Intent) of project initialization — produce project brief and optional business case."
---

You are running **Phase 1 (Intent)** of the project initialization workflow.

**Precheck:**
1. Find `docs/superpowers/plans/*-project-initialization.md`. If missing, stop: "No initialization plan found. Run `/init-triage` first."
2. Verify Phase 0 (Triage) shows `done` in the Phase Roadmap. If not, stop.
3. If Phase 1 already shows `done`, confirm with the user before re-running.

**Follow `project-initialization/phases/1-intent.md` exactly.** Load artifact rubrics from `project-initialization/artifacts/` for each active artifact. Follow the mode (interview / confirm / extract) recorded in the Artifact Roadmap.

**Follow all cross-phase fact capture rules from `project-initialization/contract.md`.** Any statement about architecture, stack, requirements, or governance belongs in `Future-Phase Facts`, not in the current artifact.

**End-of-phase review:** dispatch the `init-reviewer` agent in `agents/init-reviewer.md` with: file paths produced this phase, plan file path, phase number `1`, checklist path `project-initialization/review-checklist.md`. Apply all critical and important findings before updating the plan.

**Update the plan in one batch:** Phase Roadmap (Phase 1 → done), Artifact Roadmap statuses, Project Facts, Future-Phase Facts, Files Updated This Run, Review Findings, Next Recommended Step, Resume Context.

**Stop.** Output the end-of-run summary from `project-initialization/contract.md`.
```

- [ ] **Step 5: Create .claude/commands/init-spec.md**

```markdown
---
description: "Run Phase 2 (Specification) of project initialization — produce PRD, requirements catalog, user journeys, acceptance catalog."
---

You are running **Phase 2 (Specification)** of the project initialization workflow.

**Precheck:**
1. Find the plan file. If missing, stop and direct to `/init-triage`.
2. Verify Phase 1 (Intent) is `done`. If not, stop.
3. If Phase 2 already shows `done`, confirm with the user before re-running.

**Follow `project-initialization/phases/2-specification.md` exactly.**

**End-of-phase review:** dispatch `agents/init-reviewer.md` with files produced this phase, plan file path, phase number `2`, checklist `project-initialization/review-checklist.md`.

**Update the plan in one batch and stop.** Output end-of-run summary.
```

- [ ] **Step 6: Create .claude/commands/init-design.md**

```markdown
---
description: "Run Phase 3 (Design) of project initialization — produce solution design, ADRs, and optional C4 views."
---

You are running **Phase 3 (Design)** of the project initialization workflow.

**Precheck:**
1. Find the plan file. If missing, stop and direct to `/init-triage`.
2. Verify Phase 2 (Specification) is `done`. If not, stop.
3. If Phase 3 already shows `done`, confirm with the user before re-running.

**Follow `project-initialization/phases/3-design.md` exactly.** This phase invokes `superpowers:brainstorming` before writing the solution design and creates ADRs on demand as decisions arise.

**End-of-phase review:** dispatch `agents/init-reviewer.md` with all files produced this phase (solution design, ADRs, C4 files if any, updated ADR INDEX), plan file path, phase number `3`, checklist path.

**Update the plan in one batch and stop.** Output end-of-run summary.
```

- [ ] **Step 7: Create .claude/commands/init-govern.md**

```markdown
---
description: "Run Phase 4 (Govern & Operate) of project initialization — produce AI policy, test strategy, security baseline, delivery plan."
---

You are running **Phase 4 (Govern & Operate)** of the project initialization workflow.

**Precheck:**
1. Find the plan file. If missing, stop and direct to `/init-triage`.
2. Verify Phase 3 (Design) is `done`. If not, stop.
3. If Phase 4 already shows `done`, confirm with the user before re-running.

**Follow `project-initialization/phases/4-govern-operate.md` exactly.**

**End-of-phase review:** dispatch `agents/init-reviewer.md` with files produced this phase, plan file path, phase number `4`, checklist path.

**Update the plan in one batch and stop.** Output end-of-run summary.
```

- [ ] **Step 8: Create .claude/commands/init-adapt.md**

```markdown
---
description: "Run Phase 5 (Adapt) of project initialization — language adaptation and repository sync."
---

You are running **Phase 5 (Adapt)** of the project initialization workflow.

**Precheck:**
1. Find the plan file. If missing, stop and direct to `/init-triage`.
2. Verify Phase 4 (Govern & Operate) is `done`. If not, stop.
3. If Phase 5 already shows `done`, confirm with the user before re-running.

**Follow `project-initialization/phases/5-adapt.md` exactly.** Read all accepted ADRs before beginning. Do not add feature code, stubs, or version pins beyond what ADRs explicitly require.

**End-of-phase review:** dispatch `agents/init-reviewer.md` with all adapted files, plan file path, phase number `5`, checklist path.

**Update the plan in one batch and stop.** Output end-of-run summary.
```

- [ ] **Step 9: Create .claude/commands/init-review.md**

```markdown
---
description: "Run Phase 6 (Final Review) of project initialization — cross-doc coherence check, declare initialization complete or list what remains."
---

You are running **Phase 6 (Final Review)** of the project initialization workflow.

**Precheck:**
1. Find the plan file. If missing, stop and direct to `/init-triage`.
2. Verify all phases 1-5 are `done`. If any are not, stop and list the incomplete phases.
3. If Phase 6 already shows `done`, confirm with the user before re-running.

**Follow `project-initialization/phases/6-review.md` exactly.** This phase produces no new artifacts — it only validates what was produced in phases 1-5.

Dispatch `agents/init-reviewer.md` with: the complete list of all artifacts produced across all phases, plan file path, phase number `6`, checklist path `project-initialization/review-checklist.md`.

**Verdict:**
- All findings resolved or advisory → mark Phase 6 `done` in the plan. Declare initialization complete. Suggest commit: `"docs: complete project initialization"`.
- Unresolved important or critical findings → leave Phase 6 `in-progress`. List what must be fixed. Tell the user to fix them and re-run `/init-review`.

**Update the plan and stop.** Output end-of-run summary.
```

- [ ] **Step 10: Create .claude/agents/init-reviewer.md**

```markdown
---
name: init-reviewer
description: |
  Use to review documentation artifacts produced during a project initialization phase. Dispatch at end-of-phase with the list of files produced, the plan file path, the phase number, and the review checklist path. Returns structured findings classified by severity. Do not write to files — return findings only for the phase command to apply. Examples: <example>Context: Phase 2 (Specification) just completed producing prd.md and requirements-catalog.md. user: "Phase 2 artifacts complete." assistant: "Dispatching init-reviewer to check the specification artifacts." <commentary>End-of-phase review is required per the initialization contract.</commentary></example> <example>Context: Phase 6 final review needs to check all produced artifacts for cross-doc coherence. user: "All phases complete, running final review." assistant: "Dispatching init-reviewer with the full artifact list for final coherence check." <commentary>Phase 6 dispatches init-reviewer with all artifacts.</commentary></example>
model: inherit
---

You are an initialization documentation reviewer. Review Markdown artifacts produced during project initialization against the supplied review checklist and the project's own plan file.

**You do not write to files.** Return structured findings only. The calling phase command applies fixes.

**For each finding, output exactly this format:**

```
- Severity: [critical | important | minor | advisory]
- Location: <file path and section name or line range>
- Finding: <one-line description of the issue>
- Reason: <one line explaining why it matters for this project>
- Suggested fix: <verbatim text change or diff — concrete and applicable>
```

**Severity definitions (from `project-initialization/review-checklist.md`):**
- `critical` — artifact cannot serve its purpose: missing required section, factual contradiction with another canonical doc, broken link blocking further work.
- `important` — quality problem causing downstream issues: vague non-measurable requirement, ambiguous scope, orphaned decision not in an ADR.
- `minor` — standards violation that does not block: missing optional frontmatter field, formatting issue.
- `advisory` — concern about a user choice already in the plan's Concerns section. Do not duplicate.

**How to review:**

1. Read `project-initialization/review-checklist.md` for the full rubric.
2. Read the plan file to understand what was decided, what was extracted, and what overrides were applied.
3. For each file in the supplied list: check standards (frontmatter, links, no TBD in required sections) and coherence (consistency with other artifacts and project facts).
4. For Phase 6: additionally check cross-doc coherence across all artifacts per the final-review section of the checklist.
5. Do not invent criteria not in the checklist.
6. Do not duplicate concerns already recorded in `Concerns And Recommendations` in the plan.

**Output structure:**

Start with: `## Review: Phase N — <Phase Name>`

Then list all findings. If none: `No findings. All checked artifacts meet the standards and coherence criteria.`
```

- [ ] **Step 11: Lint commands and agent**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 .claude/README.md .claude/AGENTS.md .claude/commands/init-triage.md .claude/commands/init-intent.md .claude/commands/init-spec.md .claude/commands/init-design.md .claude/commands/init-govern.md .claude/commands/init-adapt.md .claude/commands/init-review.md .claude/agents/init-reviewer.md
```

Expected: PASS with no errors.

- [ ] **Step 12: Commit**

```bash
cd /home/hexaper/template
git add .claude/
git commit -m "feat: add .claude commands and init-reviewer agent"
```

---

### Task 9: Vendor superpowers into .claude/

**Files:**
- Create: `.claude/skills/superpowers/` (14 skill directories)
- Create: `.claude/agents/superpowers/code-reviewer.md`
- Create: `.claude/commands/superpowers/` (3 command files)

- [ ] **Step 1: Verify source exists and capture version**

Run:
```bash
cat /home/hexaper/.copilot/superpowers/package.json
ls /home/hexaper/.copilot/superpowers/skills/
ls /home/hexaper/.copilot/superpowers/agents/
ls /home/hexaper/.copilot/superpowers/commands/
```

Expected: version 5.0.7, 14 skill directories, `code-reviewer.md` agent, 3 commands.

- [ ] **Step 2: Copy superpowers skills**

Run:
```bash
mkdir -p /home/hexaper/template/.claude/skills/superpowers
cp -r /home/hexaper/.copilot/superpowers/skills/* /home/hexaper/template/.claude/skills/superpowers/
```

Expected: 14 skill directories appear under `.claude/skills/superpowers/`.

- [ ] **Step 3: Copy superpowers agent**

Run:
```bash
mkdir -p /home/hexaper/template/.claude/agents/superpowers
cp /home/hexaper/.copilot/superpowers/agents/code-reviewer.md /home/hexaper/template/.claude/agents/superpowers/code-reviewer.md
```

- [ ] **Step 4: Copy superpowers commands**

Run:
```bash
mkdir -p /home/hexaper/template/.claude/commands/superpowers
cp /home/hexaper/.copilot/superpowers/commands/* /home/hexaper/template/.claude/commands/superpowers/
```

- [ ] **Step 5: Verify copy**

Run:
```bash
ls /home/hexaper/template/.claude/skills/superpowers/ | wc -l
ls /home/hexaper/template/.claude/agents/superpowers/
ls /home/hexaper/template/.claude/commands/superpowers/
```

Expected: 14 skill directories, `code-reviewer.md`, 3 command files.

- [ ] **Step 6: Commit**

```bash
cd /home/hexaper/template
git add .claude/skills/ .claude/agents/superpowers/ .claude/commands/superpowers/
git commit -m "chore: vendor superpowers v5.0.7 into .claude/"
```

---

### Task 10: Create .copilot/ and .codex/ trees

**Files:**
- Create: `.copilot/AGENTS.md`, `.copilot/README.md`
- Create: `.copilot/commands/init-{triage,intent,spec,design,govern,adapt,review}.md`
- Create: `.copilot/agents/init-reviewer.md`, `.copilot/skills/superpowers/`, `.copilot/agents/superpowers/`, `.copilot/commands/superpowers/`
- Create: `.codex/AGENTS.md`, `.codex/README.md`
- Create: `.codex/commands/init-{triage,intent,spec,design,govern,adapt,review}.md`
- Create: `.codex/agents/init-reviewer.md`, `.codex/skills/superpowers/`, `.codex/agents/superpowers/`, `.codex/commands/superpowers/`

- [ ] **Step 1: Copy .claude/ to .copilot/ and .codex/**

Run:
```bash
cp -r /home/hexaper/template/.claude/ /home/hexaper/template/.copilot/
cp -r /home/hexaper/template/.claude/ /home/hexaper/template/.codex/
```

- [ ] **Step 2: Update .copilot/AGENTS.md**

Replace the first line of the frontmatter `name` field in `.copilot/AGENTS.md`:

```markdown
---
name: Copilot Tool Directory Rules
description: Use for work under .copilot/. Covers commands, agents, and vendored superpowers maintenance.
applyTo: ".copilot/**"
---
```

Also update the body: replace every occurrence of `.claude/` with `.copilot/` in the body text (rules about parity and vendoring still apply — keep those).

- [ ] **Step 3: Update .copilot/README.md**

Replace the first heading and intro paragraph:

```markdown
# Copilot Tool Directory

This directory contains GitHub Copilot's per-project configuration for this repository.
```

Keep the commands table and vendored superpowers note unchanged.

- [ ] **Step 4: Update .codex/AGENTS.md and .codex/README.md**

Repeat Step 2-3 for `.codex/`, replacing `.claude/` and "Claude Tool Directory" with `.codex/` and "Codex Tool Directory".

- [ ] **Step 5: Run parity check — confirm all 21 init-* commands share key phrases**

Run:
```bash
cd /home/hexaper/template && python3 - <<'PY'
from pathlib import Path
import sys

required_phrases = [
    "project-initialization/phases/",
    "project-initialization/contract.md",
    "project-initialization/review-checklist.md",
    "init-reviewer",
    "Update the plan in one batch",
]

tool_dirs = [".claude", ".copilot", ".codex"]
commands = ["init-triage", "init-intent", "init-spec", "init-design", "init-govern", "init-adapt", "init-review"]

errors = []
for tool in tool_dirs:
    for cmd in commands:
        path = Path(f"{tool}/commands/{cmd}.md")
        if not path.exists():
            errors.append(f"MISSING: {path}")
            continue
        text = path.read_text()
        for phrase in required_phrases:
            if phrase not in text and cmd != "init-triage":  # triage has no reviewer dispatch
                if phrase == "init-reviewer" and cmd == "init-triage":
                    continue
                if phrase not in ["init-reviewer", "Update the plan in one batch"] or cmd != "init-triage":
                    pass  # triage exception handled above
            if phrase not in text and not (cmd == "init-triage" and phrase in ["init-reviewer"]):
                errors.append(f"{path}: missing '{phrase}'")

if errors:
    print("FAILURES:")
    for e in errors:
        print(" ", e)
    sys.exit(1)
print(f"Parity check passed: all {len(tool_dirs) * len(commands)} command files contain required phrases.")
PY
```

Expected: `Parity check passed: all 21 command files contain required phrases.`

- [ ] **Step 6: Lint .copilot/ and .codex/ command files**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 .copilot/AGENTS.md .copilot/README.md .copilot/commands/*.md .copilot/agents/init-reviewer.md .codex/AGENTS.md .codex/README.md .codex/commands/*.md .codex/agents/init-reviewer.md
```

Expected: PASS with no errors.

- [ ] **Step 7: Commit**

```bash
cd /home/hexaper/template
git add .copilot/ .codex/
git commit -m "feat: add .copilot and .codex command surfaces"
```

---

### Task 11: Rewrite entry docs and slot READMEs

**Files:**
- Modify: `README.md`
- Modify: `AGENTS.md`
- Modify: `CLAUDE.md`
- Modify: `CONTRIBUTING.md`
- Modify: `CHANGELOG.md`
- Modify: `docs/00-source-of-truth.md`
- Modify: `docs/INDEX.md`
- Modify: `src/AGENTS.md`
- Modify: `bin/README.md`
- Modify: `diagrams/README.md`
- Modify: `examples/README.md`

- [ ] **Step 1: Replace README.md opening and journey sections**

Read `README.md` first. Replace the entire content with:

```markdown
# Universal Repository Template

A documentation-first project template with a command-first initialization workflow. It gives you a clean engineering scaffold, a structured documentation tree, and tool-native `project-initialization` commands that guide you from a fresh clone through staged project setup before implementation.

The core idea is simple: initialize one coherent phase at a time, keep progress in a tracked initialization plan, and keep repository guidance aligned with the canonical docs created during setup.

## Project journey

After cloning, open your AI tool's init command and run the phases in order:

```text
Clone
  │
  ▼
/init-triage  — routing questions, profile, artifact roadmap
  │
  ▼
/init-intent  — project brief, optional business case
  │
  ▼
/init-spec    — PRD, requirements, journeys, acceptance
  │
  ▼
/init-design  — solution design, ADRs, optional C4 views
  │
  ▼
/init-govern  — AI policy, test strategy, security, delivery
  │
  ▼
/init-adapt   — language adaptation, repository sync
  │
  ▼
/init-review  — final coherence check, declare complete
  │
  ▼
Implementation planning or deeper documentation work
```

## Bootstrap checklist

1. Choose your AI tool and open its init commands:
   - **Claude**: run `/init-triage` (commands in `.claude/commands/`)
   - **Copilot**: run the `init-triage` command (commands in `.copilot/commands/`)
   - **Codex**: run the `init-triage` command (commands in `.codex/commands/`)
2. Answer the triage routing questions. Paste any existing project description you have.
3. Run each `/init-*` command in order until `/init-review` declares initialization complete.
4. Replace the placeholder `LICENSE` before publishing.
5. Keep local workspace tooling such as `.opencode/` gitignored and untracked.
6. Commit the initialized baseline before starting feature work.

## Top-level layout

| Path | Purpose |
| --- | --- |
| `project-initialization/` | Tool-neutral phase content, artifact rubrics, profiles, and shared contract. |
| `.claude/` | Claude Code commands, agents, and vendored superpowers. |
| `.copilot/` | Copilot commands, agents, and vendored superpowers. |
| `.codex/` | Codex commands, agents, and vendored superpowers. |
| `src/` | Implementation code and reusable modules. |
| `tests/` | Automated verification. |
| `config/` | Environment, runtime, and deployment configuration templates. |
| `scripts/` | Maintainer automation. |
| `bin/` | Thin user-facing entrypoints or wrappers. |
| `tools/` | Repository-maintained utilities such as documentation validators. |
| `examples/` | Copyable examples for implemented features only. |
| `diagrams/` | Source diagrams supporting active architecture docs. |
| `docs/` | Canonical documentation tree and governance structure. |

## Documentation model

- `docs/README.md`, `docs/Architecture.md`, `docs/00-documentation-standards.md`, `docs/00-source-of-truth.md`, and `docs/INDEX.md` form the control spine.
- `docs/adr/` holds durable architecture decisions.
- `docs/superpowers/` holds dated specs and plans as historical working records.
- `docs/99_archive/` is for retired material.
- `tools/docs_validator/` holds the Python frontmatter validator.
```

- [ ] **Step 2: Update AGENTS.md**

Read `AGENTS.md`. Make these targeted changes:

1. In the main directories line, add `project-initialization/`, `.claude/`, `.copilot/`, `.codex/` and remove `prompts/`.
2. Replace any line referring to "start with `prompts/`" or "run `project-bootstrap.md`" with: "A fresh clone of this template is unspecialized. Start with the init command in your AI tool's tool directory (`.claude/`, `.copilot/`, or `.codex/`) and run `/init-triage` to begin."
3. Add routing entry: "`project-initialization/AGENTS.md` — rules for editing phase content, artifact rubrics, and profiles."

- [ ] **Step 3: Update CLAUDE.md**

Replace the existing content with:

```markdown
# Claude Working Notes

Base rules for any agent live in [AGENTS.md](AGENTS.md). The workflow reference is [README.md](README.md).

## Template initialization

For template adoption with Claude, start with `.claude/commands/init-triage.md` (invoke as `/init-triage`). Run each `/init-*` command in phase order until `/init-review` declares initialization complete.

## Agent rules

See [.claude/AGENTS.md](.claude/AGENTS.md) for rules specific to the `.claude/` tool directory.
```

- [ ] **Step 4: Update CONTRIBUTING.md**

Add under the existing contribution guidelines:

```markdown
## Initialization workflow changes

If a change affects the project initialization workflow:

- Update the relevant files in `project-initialization/` (phase files, artifact rubrics, or profiles).
- Make equivalent changes to the command files in all three tool dirs: `.claude/commands/`, `.copilot/commands/`, `.codex/commands/`.
- Run the CI parity check locally before opening a PR: `python3 scripts/check-init-parity.py` (created in Task 12).
- Update `README.md` if the phase list or bootstrap checklist changes.
```

- [ ] **Step 5: Add CHANGELOG.md entry**

Add under `## [Unreleased]`:

```markdown
### Changed

- Replaced the prompt-first template adoption path with a tool-native command-first initialization workflow. Seven phase commands (`/init-triage` through `/init-review`) in `.claude/`, `.copilot/`, and `.codex/` drive stage-by-stage initialization backed by a tracked plan file. The `prompts/` directory is removed. Phase content and artifact rubrics live in the new `project-initialization/` directory.
```

- [ ] **Step 6: Update docs/00-source-of-truth.md**

Read the file. Replace any row referencing the initialization prompts with:

```markdown
| Initialization workflow | [`project-initialization/README.md`](../project-initialization/README.md) | Primary tracked entrypoint for tool-native command-first template adoption. |
| Initialization commands — Claude | [`.claude/commands/`](../.claude/commands/) | Claude Code phase commands for project initialization. |
| Initialization commands — Copilot | [`.copilot/commands/`](../.copilot/commands/) | Copilot phase commands for project initialization. |
| Initialization commands — Codex | [`.codex/commands/`](../.codex/commands/) | Codex phase commands for project initialization. |
```

Remove any row that pointed to `prompts/README.md` as a primary surface.

- [ ] **Step 7: Update docs/INDEX.md**

Add under the supporting surfaces section:

```markdown
- [`../project-initialization/README.md`](../project-initialization/README.md) — tool-neutral phase content and shared contract for project initialization.
- [`.claude/`](../.claude/), [`.copilot/`](../.copilot/), [`.codex/`](../.codex/) — tool-native command surfaces for initialization.
```

Remove any entry pointing to `prompts/` as a primary surface.

- [ ] **Step 8: Update slot READMEs**

In `src/AGENTS.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md`: replace any sentence referring to `prompts/language-adaptation.md` with: "This is a slot directory. The `project-initialization` Adapt phase (`/init-adapt`) fills or removes this directory during Repository Sync depending on the chosen stack."

- [ ] **Step 9: Lint all modified entry docs**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md docs/00-source-of-truth.md docs/INDEX.md src/AGENTS.md bin/README.md diagrams/README.md examples/README.md
```

Expected: PASS with no errors.

- [ ] **Step 10: Commit**

```bash
cd /home/hexaper/template
git add README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md docs/00-source-of-truth.md docs/INDEX.md src/AGENTS.md bin/README.md diagrams/README.md examples/README.md
git commit -m "docs: move template adoption to command-first workflow"
```

---

### Task 12: Delete prompts/ and update CI

**Files:**
- Delete: `prompts/` (entire directory)
- Modify: `.github/workflows/ci.yml`
- Create: `scripts/check-init-parity.py`

- [ ] **Step 1: Delete prompts/**

Run:
```bash
cd /home/hexaper/template
git rm -r prompts/
```

Expected: `prompts/AGENTS.md`, `prompts/README.md`, `prompts/project-bootstrap.md`, `prompts/refine-specs.md`, `prompts/architecture-baseline.md`, `prompts/language-adaptation.md` all staged for deletion.

- [ ] **Step 2: Update .github/workflows/ci.yml — required governance files**

Read `.github/workflows/ci.yml`. In the `Validate required governance files` step, replace the prompts checks with tool-dir and project-initialization checks:

Remove these lines:
```yaml
          test -f prompts/README.md
          test -f prompts/AGENTS.md
          test -f prompts/language-adaptation.md
          test -f prompts/project-bootstrap.md
          test -f prompts/architecture-baseline.md
          test -f prompts/refine-specs.md
```

Add these lines:
```yaml
          test -f project-initialization/README.md
          test -f project-initialization/contract.md
          test -f project-initialization/plan-template.md
          test -f project-initialization/review-checklist.md
          test -f .claude/commands/init-triage.md
          test -f .claude/commands/init-review.md
          test -f .claude/agents/init-reviewer.md
          test -f .copilot/commands/init-triage.md
          test -f .copilot/commands/init-review.md
          test -f .codex/commands/init-triage.md
          test -f .codex/commands/init-review.md
```

- [ ] **Step 3: Update .github/workflows/ci.yml — canonical path consistency**

In the `Validate canonical path consistency` step, add `project-initialization .claude .copilot .codex` to the path list and remove `prompts` from it.

- [ ] **Step 4: Create scripts/check-init-parity.py**

```python
#!/usr/bin/env python3
"""Check that all three tool dirs have equivalent init-* command files."""
from pathlib import Path
import sys

TOOL_DIRS = [".claude", ".copilot", ".codex"]
COMMANDS = [
    "init-triage",
    "init-intent",
    "init-spec",
    "init-design",
    "init-govern",
    "init-adapt",
    "init-review",
]
REQUIRED_PHRASES = [
    "project-initialization/phases/",
    "project-initialization/contract.md",
]
# init-triage has different structure (no reviewer dispatch, no plan update)
TRIAGE_SKIP = {"init-reviewer", "Update the plan in one batch"}
NON_TRIAGE_PHRASES = [
    "init-reviewer",
    "Update the plan in one batch",
]

errors = []

for cmd in COMMANDS:
    texts = {}
    for tool in TOOL_DIRS:
        path = Path(f"{tool}/commands/{cmd}.md")
        if not path.exists():
            errors.append(f"MISSING: {path}")
            texts[tool] = ""
        else:
            texts[tool] = path.read_text()

    phrases = REQUIRED_PHRASES[:]
    if cmd != "init-triage":
        phrases += NON_TRIAGE_PHRASES

    for phrase in phrases:
        for tool in TOOL_DIRS:
            if phrase not in texts.get(tool, ""):
                errors.append(f"{tool}/commands/{cmd}.md: missing required phrase '{phrase}'")

if errors:
    print("Init parity check FAILED:")
    for e in errors:
        print(f"  {e}")
    sys.exit(1)

total = len(TOOL_DIRS) * len(COMMANDS)
print(f"Init parity check passed: all {total} command files contain required phrases.")
```

- [ ] **Step 5: Run parity check to confirm it passes**

Run:
```bash
cd /home/hexaper/template && python3 scripts/check-init-parity.py
```

Expected: `Init parity check passed: all 21 command files contain required phrases.`

- [ ] **Step 6: Lint CI and parity script**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 .github/workflows/ci.yml 2>/dev/null || echo "ci.yml is YAML not Markdown — skip lint"
python3 -m py_compile scripts/check-init-parity.py && echo "Python syntax OK"
```

Expected: `Python syntax OK`

- [ ] **Step 7: Commit**

```bash
cd /home/hexaper/template
git add .github/workflows/ci.yml scripts/check-init-parity.py
git commit -m "ci: guard command-first workflow, delete prompts/"
```

---

### Task 13: Final integration validation

**Files:** no new files — validation only

- [ ] **Step 1: Run markdownlint across all changed Markdown**

Run:
```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 \
  project-initialization/**/*.md \
  .claude/README.md .claude/AGENTS.md .claude/commands/init-*.md .claude/agents/init-reviewer.md \
  .copilot/README.md .copilot/AGENTS.md .copilot/commands/init-*.md .copilot/agents/init-reviewer.md \
  .codex/README.md .codex/AGENTS.md .codex/commands/init-*.md .codex/agents/init-reviewer.md \
  docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md \
  README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md \
  docs/00-source-of-truth.md docs/INDEX.md \
  src/AGENTS.md bin/README.md diagrams/README.md examples/README.md
```

Expected: PASS with no errors.

- [ ] **Step 2: Run docs validator**

Run:
```bash
cd /home/hexaper/template && PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs docs/_examples
```

Expected: PASS with 0 errors. (Warning count may change due to updated docs — warnings are acceptable, errors are not.)

- [ ] **Step 3: Run init parity check**

Run:
```bash
cd /home/hexaper/template && python3 scripts/check-init-parity.py
```

Expected: `Init parity check passed: all 21 command files contain required phrases.`

- [ ] **Step 4: Verify no prompts/ references remain in primary entry docs**

Run:
```bash
cd /home/hexaper/template && rg -n "prompts/" README.md AGENTS.md CLAUDE.md CONTRIBUTING.md .github/workflows/ci.yml docs/00-source-of-truth.md docs/INDEX.md 2>&1
```

Expected: no output (no remaining primary references to `prompts/`). References in `CHANGELOG.md` describing the removal are acceptable — check output carefully.

- [ ] **Step 5: Verify new primary dirs appear in entry docs**

Run:
```bash
cd /home/hexaper/template && rg -l "project-initialization\|\.claude.*commands\|init-triage" README.md AGENTS.md CLAUDE.md docs/00-source-of-truth.md
```

Expected: all four files listed in output.

- [ ] **Step 6: Manual adopter walkthrough**

Read these files in order and confirm the path is clear and coherent:

1. `README.md` — does a fresh adopter see the command-first path immediately?
2. `.claude/commands/init-triage.md` — is it self-contained and actionable?
3. `project-initialization/phases/0-triage.md` — does it define the routing questions and plan-file initialization clearly?
4. `.claude/commands/init-intent.md` — does it gate correctly on triage and hand off to the right phase file?
5. `project-initialization/phases/6-review.md` — does it declare a clear pass/fail verdict?

Expected: a first-time adopter could follow these files from clone to initialized repository without ambiguity.

- [ ] **Step 7: Create final integration commit if any fixes were made**

Run:
```bash
cd /home/hexaper/template
git status --short
```

If any files were modified during validation fixes, stage and commit them:

```bash
git add <changed files>
git commit -m "docs: finalize command-first adoption workflow integration"
```

If `git status --short` is empty, skip this step — no commit needed.
