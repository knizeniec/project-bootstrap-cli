---
title: Adoption Orchestrator Skill Implementation Plan
status: active
record_class: historical
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Adoption Orchestrator Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the prompt-first template adoption path with repo-included project-initialization skills for Claude and Copilot/Codex, backed by one shared workflow contract, one tracked initialization-plan model, aligned repository entrypoints, and CI guardrails.

**Architecture:** Treat this as one coupled workflow change, not separate mini-projects: the shared contract, tool-specific skill packages, repo entrypoints, legacy prompt fallback, slot references, and CI checks must land together or the adoption flow will drift immediately. Store the shared contract in `skills/project-initialization/README.md`, ship thin tool-specific `SKILL.md` files that preserve the same stage model and stop rules, keep prompts as documented fallback only, and validate the result with markdownlint, docs validation, local link checks, and contract-parity checks.

**Tech Stack:** Markdown, repo-included skill files, GitHub Actions, markdownlint-cli2, lychee, Python 3, ripgrep

---

## File Map

- Create: `skills/AGENTS.md`
  Responsibility: local routing and maintenance rules for tracked repo-included skills.
- Create: `skills/README.md`
  Responsibility: user-facing skill inventory and primary skill-first entrypoint.
- Create: `skills/project-initialization/README.md`
  Responsibility: shared contract for stage names, plan file structure, review loops, concern handling, and stop rules.
- Create: `skills/project-initialization/testing/pressure-scenarios.md`
  Responsibility: reusable RED/GREEN validation prompts that pressure-test stage boundaries, parked-question behavior, and concern handling.
- Create: `skills/project-initialization/testing/review-checklist.md`
  Responsibility: per-stage review passes and final integration review checklist used by both tool variants.
- Create: `skills/project-initialization/claude/SKILL.md`
  Responsibility: Claude-targeted orchestration skill that follows the shared contract and uses reviewer-only subagents.
- Create: `skills/project-initialization/copilot-codex/SKILL.md`
  Responsibility: Copilot/Codex-targeted orchestration skill with the same contract and adapted review mechanics.
- Create: `docs/adr/ADR-001-skill-first-template-adoption.md`
  Responsibility: durable decision record that makes the skill-first adoption workflow binding before implementation lands.
- Modify: `README.md`
  Responsibility: main adopter entrypoint, stage-by-stage journey, top-level layout, and fallback guidance.
- Modify: `AGENTS.md`
  Responsibility: repo-wide routing, main-directory inventory, and global workflow guidance.
- Modify: `CLAUDE.md`
  Responsibility: short Claude-specific pointer to the repo-included initialization skill.
- Modify: `CONTRIBUTING.md`
  Responsibility: contributor expectations when changing the initialization workflow.
- Modify: `CHANGELOG.md`
  Responsibility: visible release note for the new skill-first adoption path.
- Modify: `prompts/README.md`
  Responsibility: legacy prompt inventory and fallback positioning.
- Modify: `docs/00-source-of-truth.md`
  Responsibility: source-of-truth ownership for the new skill-first adoption surface and legacy prompt fallback.
- Modify: `docs/adr/INDEX.md`
  Responsibility: canonical ADR registry entry for the skill-first adoption decision.
- Modify: `docs/INDEX.md`
  Responsibility: navigation coverage for the new skill-first entrypoint and the legacy fallback surface.
- Modify: `src/AGENTS.md`
  Responsibility: slot-template guidance for how initialization fills source-layer rules.
- Modify: `bin/README.md`
  Responsibility: slot-template guidance for repository-sync behavior.
- Modify: `diagrams/README.md`
  Responsibility: slot-template guidance for repository-sync behavior.
- Modify: `examples/README.md`
  Responsibility: slot-template guidance for repository-sync behavior.
- Modify: `.github/workflows/ci.yml`
  Responsibility: required-file enforcement and path-consistency coverage for the new `skills/` surface.

Keep this as one plan because these files define one user-visible workflow surface. Shipping only the skills without the README/AGENTS/CI migration would leave the repo telling adopters to start in the wrong place.

### Task 0: Record The Skill-First Adoption Decision Before Workflow Implementation

**Files:**
- Create: `docs/adr/ADR-001-skill-first-template-adoption.md`
- Modify: `docs/adr/INDEX.md`
- Test: `docs/adr/ADR-001-skill-first-template-adoption.md`, `docs/adr/INDEX.md`

- [ ] **Step 1: Confirm the next ADR slot is available**

Run:

```bash
cd /home/hexaper/template && test ! -f docs/adr/ADR-001-skill-first-template-adoption.md
```

Expected: PASS because `ADR-001` is the next free durable-decision record in the current tree.

- [ ] **Step 2: Create the ADR that makes skill-first adoption the binding repository workflow**

Create `docs/adr/ADR-001-skill-first-template-adoption.md` with exactly this content:

```md
---
title: ADR-001 Skill-First Template Adoption
status: active
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# ADR-001: Adopt repo-included skills as the primary template initialization workflow

- Status: Proposed
- Date: 2026-05-07
- Deciders: Template maintainers
- Consulted: Documentation maintainers
- Informed: Contributors and template adopters
- Tags: [Architecture, Delivery]
- Supersedes: None
- Superseded by: None
- Related documents: [../superpowers/specs/2026-05-07-adoption-orchestrator-skill-design.md](../superpowers/specs/2026-05-07-adoption-orchestrator-skill-design.md), [../superpowers/plans/2026-05-07-adoption-orchestrator-skill-implementation.md](../superpowers/plans/2026-05-07-adoption-orchestrator-skill-implementation.md)

## Context and problem statement

The repository currently teaches adopters to initialize the template through a prompt-first sequence spread across multiple files. That flow is uneven in depth, requires the user to choose the next prompt manually, and does not provide one resumable control surface for staged initialization. The approved design replaces that primary entrypoint with repo-included `project-initialization` skills for Claude and Copilot/Codex while keeping the existing prompt files as a documented fallback.

## Decision drivers

- give adopters one obvious starting point after cloning
- keep initialization bounded to one coherent stage per run
- preserve shared workflow rules across supported AI tools
- keep repository guidance, control surfaces, and CI aligned with the canonical docs created during initialization

## Considered options

### Option 1: Keep the prompt-first workflow as the primary adoption path

- What it is: continue teaching adopters to run `project-bootstrap.md`, `refine-specs.md`, `architecture-baseline.md`, and `language-adaptation.md` in order.
- Pros: no new tracked skill surface; keeps the current structure intact.
- Cons: weak continuity across runs; prompt selection remains manual; stage boundaries and review loops remain inconsistent.

### Option 2: Use repo-included skills as the primary path and keep prompts as fallback

- What it is: ship tool-specific `project-initialization` skills in the repository, backed by one shared contract and one tracked initialization plan, while retaining `prompts/` for tools that cannot use the skills.
- Pros: one clear entrypoint; resumable stage-by-stage workflow; shared concern-handling and review-loop contract across supported tools.
- Cons: adds tracked skill-maintenance work and migration effort across repo entry surfaces.

## Decision outcome

Adopt repo-included `project-initialization` skills as the primary template initialization workflow for supported tools, with `prompts/` retained and documented as the legacy fallback path.

### Consequences

- `README.md`, `AGENTS.md`, `CLAUDE.md`, and related slot guidance must point to the skill-first path first.
- The repository must maintain a shared contract and parity checks across tool-specific skill variants.
- Prompt files remain supported, but they no longer define the primary adoption journey.

## Pros and cons of the options

- Chosen option: it creates one coherent, resumable initialization path that matches the approved design and reduces prompt-selection drift.
- Rejected option: it leaves the current fragmentation in place and keeps the repository advertising a weaker onboarding flow.

## More information

- Update `README.md` and `AGENTS.md` when the entrypoint changes.
- Update `prompts/README.md` so prompt usage stays clearly fallback-only.
- Keep CI checks aligned with the tracked `skills/` surface.
```

- [ ] **Step 3: Register the ADR in the canonical index**

Add this row directly below the `ADR-000` row in `docs/adr/INDEX.md`:

```md
| [ADR-001](ADR-001-skill-first-template-adoption.md) | Skill-First Template Adoption | Proposed | 2026-05-07 | Makes repo-included initialization skills the primary adoption workflow. |
```

- [ ] **Step 4: Lint the ADR files**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md
```

Expected: PASS with no errors.

- [ ] **Step 5: Commit the ADR before changing the workflow surface**

```bash
cd /home/hexaper/template
git add docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md
git commit -m "docs: record skill-first adoption ADR"
```

### Task 1: Create The Tracked Skill Surface And Shared Contract

**Files:**
- Create: `skills/AGENTS.md`
- Create: `skills/README.md`
- Create: `skills/project-initialization/README.md`
- Test: `skills/AGENTS.md`, `skills/README.md`, `skills/project-initialization/README.md`

- [ ] **Step 1: Prove the tracked skill surface does not exist yet**

Run:

```bash
cd /home/hexaper/template && test -f skills/README.md && test -f skills/project-initialization/README.md && test -f skills/AGENTS.md
```

Expected: FAIL because the tracked `skills/` entrypoint and contract docs do not exist yet.

- [ ] **Step 2: Create the local authoring rules for tracked skills**

Create `skills/AGENTS.md` with exactly this content:

```md
---
name: Repository Skill Rules
description: Use for work under skills/. Covers tracked, repo-included skills and shared-contract maintenance.
applyTo: "skills/**"
---

# Repository Skill Rules

Apply these rules when editing files under `skills/`.

## Most Important Rules

- Keep repo-included skills tracked under `skills/`; do not treat ignored tool-local folders as the canonical storage location.
- Keep one shared workflow contract per skill family and keep tool-specific variants aligned with it.
- Tool-specific packaging may differ, but stage names, stop conditions, plan-file sections, review-loop rules, and concern-handling rules must not drift.
- Keep skill descriptions focused on when to use the skill, not a summary of the workflow body.
- Update `README.md`, `AGENTS.md`, `prompts/README.md`, and any affected slot guidance when skill entrypoints or workflow expectations change.
```

- [ ] **Step 3: Create the skill inventory entrypoint and shared project-initialization contract**

Create `skills/README.md` with exactly this content:

```md
# Repository Skills

This directory stores tracked, repo-included skills that guide template adoption and other repeatable workflows.

## Start here

- If you are adopting this template with Claude, start with [project-initialization/claude/SKILL.md](project-initialization/claude/SKILL.md).
- If you are adopting this template with Copilot or Codex, start with [project-initialization/copilot-codex/SKILL.md](project-initialization/copilot-codex/SKILL.md).
- If your tool cannot use these repo-shipped skills, use the legacy fallback flow in [../prompts/README.md](../prompts/README.md).

## Inventory

| Skill | Tool target | Purpose |
| --- | --- | --- |
| [project-initialization/claude/SKILL.md](project-initialization/claude/SKILL.md) | Claude | Preferred entrypoint for stage-by-stage template initialization. |
| [project-initialization/copilot-codex/SKILL.md](project-initialization/copilot-codex/SKILL.md) | Copilot/Codex | Same initialization contract adapted to Copilot/Codex workflows. |

## Shared contract

Both tool variants must follow the contract in [project-initialization/README.md](project-initialization/README.md).
```

Create `skills/project-initialization/README.md` with exactly this content:

```md
# Project Initialization Contract

This file is the shared contract for all repo-included `project-initialization` skill variants.

## Shared stages

1. Preflight and orientation
2. Project brief
3. PRD
4. Architecture readiness
5. Architecture baseline
6. Repository sync
7. Final coherence review

Keep one stage per run.

## Working plan

- Path: `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md`
- Required sections:
  - `Goal`
  - `Current Stage`
  - `Stage Status`
  - `Project Facts`
  - `Open Questions For Current Stage`
  - `Parked Questions For Later`
  - `Concerns And Recommendations`
  - `Files Updated This Stage`
  - `Review Findings`
  - `Next Recommended Step`
  - `Resume Context`

## Per-run contract

For each invocation:

1. Read the current plan and relevant canonical docs.
2. Continue the active stage or advance to the next stage.
3. Ask only the questions needed to finish the current stage well.
4. Park unrelated but useful later questions.
5. Update canonical docs in one coherent batch at the end of the stage.
6. Run the review/fix loop.
7. Update the plan.
8. Propose the next step.
9. Stop.

## Review/fix loop

Every stage that changes files must run standards review and coherence review. Check:

- documentation standards
- cross-document consistency
- source-of-truth alignment
- path and link validity
- placeholder discipline
- scope drift or implementation leakage
- whether `README.md`, `AGENTS.md`, index, or source-of-truth updates are also required

## Concern handling

When a user choice is weak, risky, or poorly matched to project facts, the skill must say:

- what the concern is
- why it matters here
- what stronger option may fit better
- what trade-off that stronger option introduces
- whether the original choice is still acceptable if kept intentionally

## End-of-run output

- `Stage Completed`
- `Docs Updated`
- `Review Fixes Applied`
- `Concerns And Recommendations`
- `Parked Questions`
- `Next Recommended Step`

## Validation helpers

- Pressure scenarios: [testing/pressure-scenarios.md](testing/pressure-scenarios.md)
- Review checklist: [testing/review-checklist.md](testing/review-checklist.md)
```

- [ ] **Step 4: Lint the new skill-surface docs**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 skills/AGENTS.md skills/README.md skills/project-initialization/README.md
```

Expected: PASS with no errors.

- [ ] **Step 5: Verify the currently-created links resolve and only intentional forward links remain**

Run:

```bash
cd /home/hexaper/template && python3 - <<'PY'
from pathlib import Path
import re

files = [
    Path("skills/README.md"),
    Path("skills/project-initialization/README.md"),
]
pattern = re.compile(r"\[[^\]]+\]\(([^)#]+)\)")
allowed_missing = {
    "skills/README.md -> project-initialization/claude/SKILL.md",
    "skills/README.md -> project-initialization/copilot-codex/SKILL.md",
    "skills/project-initialization/README.md -> testing/pressure-scenarios.md",
    "skills/project-initialization/README.md -> testing/review-checklist.md",
}
missing = set()
for file in files:
    text = file.read_text()
    for target in pattern.findall(text):
        if target.startswith(("http://", "https://", "mailto:")):
            continue
        resolved = (file.parent / target).resolve()
        if not resolved.exists():
            missing.add(f"{file} -> {target}")
unexpected = sorted(missing - allowed_missing)
if unexpected:
    raise SystemExit("Unexpected unresolved links:\n" + "\n".join(unexpected))
if missing != allowed_missing:
    raise SystemExit("Forward-link set drifted:\n" + "\n".join(sorted(missing)))
print("Only the intentional forward links remain unresolved at Task 1.")
PY
```

Expected: PASS and prints `Only the intentional forward links remain unresolved at Task 1.`.

- [ ] **Step 6: Commit the skill-surface scaffold**

```bash
cd /home/hexaper/template
git add skills/AGENTS.md skills/README.md skills/project-initialization/README.md
git commit -m "docs: add tracked skill workflow contract"
```

### Task 2: Add Pressure Scenarios, Review Checklist, And The Claude Skill

**Files:**
- Create: `skills/project-initialization/testing/pressure-scenarios.md`
- Create: `skills/project-initialization/testing/review-checklist.md`
- Create: `skills/project-initialization/claude/SKILL.md`
- Test: `skills/project-initialization/testing/pressure-scenarios.md`, `skills/project-initialization/testing/review-checklist.md`, `skills/project-initialization/claude/SKILL.md`

- [ ] **Step 1: Create the reusable pressure scenarios and review checklist**

Create `skills/project-initialization/testing/pressure-scenarios.md` with exactly this content:

```md
# Project Initialization Pressure Scenarios

Use these scenarios to validate that a tool-specific skill stays inside one coherent stage, parks later questions, raises concerns clearly, and stops cleanly.

## Scenario 1: Stage-spill temptation

Prompt:

```text
We are still working on the project brief, but here are product requirements, likely integrations, and my preferred stack. Please update everything now so we can move fast.
```

Pass criteria:

- finishes only the current stage
- parks later-stage questions or facts
- does not jump ahead into architecture baseline or repository sync

## Scenario 2: Multi-area answer in one message

Prompt:

```text
Here are answers for the brief, the PRD, and half the architecture all together. Use whatever is helpful and keep going until the repo is ready.
```

Pass criteria:

- extracts only the facts needed for the active stage
- records the rest under `Parked Questions For Later`
- ends with one stage complete and a clear next step

## Scenario 3: Weak user choice

Prompt:

```text
This is a regulated workflow, but I do not want ADRs, and I want the architecture to stay unwritten because we can keep it in chat.
```

Pass criteria:

- names the concern directly
- explains why it matters in this project context
- suggests a stronger alternative
- explains the trade-off
- says whether the original choice is still acceptable if kept intentionally

## Scenario 4: Repo-sync completeness

Prompt:

```text
The docs are done. Please stop after updating the brief and PRD; I do not care whether README or AGENTS match yet.
```

Pass criteria:

- notes the mismatch risk
- explains why repository-sync work is still required before closing initialization
- does not claim the plan is complete while entrypoints are stale
```

Create `skills/project-initialization/testing/review-checklist.md` with exactly this content:

```md
# Project Initialization Review Checklist

Use this checklist after every stage that changes files.

## Standards review

- frontmatter or metadata shape matches the owning surface
- links and paths resolve
- placeholders are explicit and intentional
- no implementation code or version pinning leaked into documentation-only stages

## Coherence review

- changed docs agree with one another
- source-of-truth ownership still points to the right file
- README and AGENTS guidance still match the actual workflow
- the initialization plan reflects what was really completed

## Final integration review

- all completed stages fit together without contradiction
- no slot file still points to the old primary prompt-first entrypoint
- legacy prompt docs are clearly marked as fallback, not primary
- end-of-run output still matches the shared contract
```

- [ ] **Step 2: Run one baseline scenario in a fresh Claude session before the skill exists**

Use a fresh Claude session that has not loaded `skills/project-initialization/claude/SKILL.md`. Paste `Scenario 1: Stage-spill temptation` from `skills/project-initialization/testing/pressure-scenarios.md`.

Expected: capture at least one baseline failure pattern such as stage spill, no parked-question handling, no explicit stop, or no concern framing. Keep the observed failure notes outside the repo and use them to tighten the skill wording.

- [ ] **Step 3: Create the Claude skill using the shared contract and testing docs**

Create `skills/project-initialization/claude/SKILL.md` with exactly this content:

```md
---
name: project-initialization-claude
description: Use when starting or continuing template initialization in this repository with Claude from a fresh or partially specialized clone.
---

# Project Initialization Skill For Claude

## Overview

Use this skill to initialize the repository one coherent stage at a time. Read the shared contract in [../README.md](../README.md), ask only current-stage questions, write canonical docs in one batch at stage end, run review/fix loops, update the tracked initialization plan, propose the next step, and stop.

## When to Use

- right after cloning this template
- when resuming an unfinished initialization run
- when the initialization plan exists but the next stage is not complete

Do not use this skill for feature implementation after initialization is complete.

## Required reads

Before acting, read:

- [../README.md](../README.md)
- [../testing/review-checklist.md](../testing/review-checklist.md)
- the active `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` file, or create one during preflight if none exists

## Claude-specific rules

- You may use subagents only as reviewers, not as owners of the stage.
- If you use review subagents, collect their findings before closing the run.
- Keep one stage per run even if the user gives enough detail for later stages.

## Stage loop

1. Read the initialization plan and relevant canonical docs.
2. Continue the current stage or advance to the next shared stage.
3. Ask only the minimum questions needed to finish the current stage well.
4. Park later-stage questions instead of expanding the run.
5. Write canonical docs in one batch at the end of the stage.
6. Run standards review and coherence review from [../testing/review-checklist.md](../testing/review-checklist.md).
7. Fix objective issues immediately.
8. Record advisory concerns in the plan if they are not blockers.
9. Update the plan and stop.

## Concern handling

When a user choice is weak or risky, say:

- what the concern is
- why it matters here
- what stronger option may fit better
- what trade-off that stronger option introduces
- whether the original choice is still acceptable if kept intentionally

## Output format

- `Stage Completed`
- `Docs Updated`
- `Review Fixes Applied`
- `Concerns And Recommendations`
- `Parked Questions`
- `Next Recommended Step`

## Validation

Pressure-test this skill with [../testing/pressure-scenarios.md](../testing/pressure-scenarios.md).
```

- [ ] **Step 4: Re-run pressure scenarios with the Claude skill loaded**

In a fresh Claude session that loads `skills/project-initialization/claude/SKILL.md`, run `Scenario 1: Stage-spill temptation` and `Scenario 3: Weak user choice` from `skills/project-initialization/testing/pressure-scenarios.md`.

Expected:

- Scenario 1 stays within one stage and parks later questions.
- Scenario 3 uses the full five-part concern framing.

- [ ] **Step 5: Lint the testing docs and Claude skill**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 skills/project-initialization/testing/pressure-scenarios.md skills/project-initialization/testing/review-checklist.md skills/project-initialization/claude/SKILL.md
```

Expected: PASS with no errors.

- [ ] **Step 6: Commit the Claude skill slice**

```bash
cd /home/hexaper/template
git add skills/project-initialization/testing/pressure-scenarios.md skills/project-initialization/testing/review-checklist.md skills/project-initialization/claude/SKILL.md
git commit -m "docs: add Claude initialization skill"
```

### Task 3: Add The Copilot/Codex Skill And Enforce Contract Parity

**Files:**
- Create: `skills/project-initialization/copilot-codex/SKILL.md`
- Test: `skills/project-initialization/copilot-codex/SKILL.md`, `skills/project-initialization/README.md`, `skills/project-initialization/claude/SKILL.md`

- [ ] **Step 1: Create the Copilot/Codex skill with the same shared contract**

Create `skills/project-initialization/copilot-codex/SKILL.md` with exactly this content:

```md
---
name: project-initialization-copilot-codex
description: Use when starting or continuing template initialization in this repository with Copilot or Codex from a fresh or partially specialized clone.
---

# Project Initialization Skill For Copilot/Codex

## Overview

Use this skill to initialize the repository one coherent stage at a time. Read the shared contract in [../README.md](../README.md), ask only current-stage questions, write canonical docs in one batch at stage end, run review/fix loops, update the tracked initialization plan, propose the next step, and stop.

## When to Use

- right after cloning this template
- when resuming an unfinished initialization run
- when the initialization plan exists but the next stage is not complete

Do not use this skill for feature implementation after initialization is complete.

## Required reads

Before acting, read:

- [../README.md](../README.md)
- [../testing/review-checklist.md](../testing/review-checklist.md)
- the active `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md` file, or create one during preflight if none exists

## Copilot/Codex-specific rules

- If the tool cannot launch reviewer subagents, run the standards review and coherence review as separate explicit review passes in the same session.
- Keep one stage per run even if the user gives enough detail for later stages.
- Preserve the exact shared stage names, plan sections, concern framing, and output format.

## Stage loop

1. Read the initialization plan and relevant canonical docs.
2. Continue the current stage or advance to the next shared stage.
3. Ask only the minimum questions needed to finish the current stage well.
4. Park later-stage questions instead of expanding the run.
5. Write canonical docs in one batch at the end of the stage.
6. Run standards review and coherence review from [../testing/review-checklist.md](../testing/review-checklist.md).
7. Fix objective issues immediately.
8. Record advisory concerns in the plan if they are not blockers.
9. Update the plan and stop.

## Concern handling

When a user choice is weak or risky, say:

- what the concern is
- why it matters here
- what stronger option may fit better
- what trade-off that stronger option introduces
- whether the original choice is still acceptable if kept intentionally

## Output format

- `Stage Completed`
- `Docs Updated`
- `Review Fixes Applied`
- `Concerns And Recommendations`
- `Parked Questions`
- `Next Recommended Step`

## Validation

Pressure-test this skill with [../testing/pressure-scenarios.md](../testing/pressure-scenarios.md).
```

- [ ] **Step 2: Run pressure scenarios with the Copilot/Codex skill loaded**

In a fresh Copilot or Codex session that uses `skills/project-initialization/copilot-codex/SKILL.md`, run `Scenario 2: Multi-area answer in one message` and `Scenario 4: Repo-sync completeness` from `skills/project-initialization/testing/pressure-scenarios.md`.

Expected:

- Scenario 2 parks later-stage material instead of consuming it all.
- Scenario 4 refuses to close initialization before repository-sync updates are complete.

- [ ] **Step 3: Verify that the shared contract, stop rule, and concern/output strings appear in all three contract files**

Run:

```bash
cd /home/hexaper/template && python3 - <<'PY'
from pathlib import Path

required = [
    "Preflight and orientation",
    "Project brief",
    "PRD",
    "Architecture readiness",
    "Architecture baseline",
    "Repository sync",
    "Final coherence review",
    "docs/superpowers/plans/YYYY-MM-DD-project-initialization.md",
    "Keep one stage per run",
    "what the concern is",
    "why it matters here",
    "what stronger option may fit better",
    "what trade-off",
    "whether the original choice is still acceptable if kept intentionally",
    "Stage Completed",
    "Docs Updated",
    "Review Fixes Applied",
    "Concerns And Recommendations",
    "Parked Questions",
    "Next Recommended Step",
]
files = [
    Path("skills/project-initialization/README.md"),
    Path("skills/project-initialization/claude/SKILL.md"),
    Path("skills/project-initialization/copilot-codex/SKILL.md"),
]
shared = Path("skills/project-initialization/README.md").read_text()
for phrase in ["standards review", "coherence review"]:
    if phrase not in shared:
        raise SystemExit(f"skills/project-initialization/README.md missing {phrase}")
for path in files:
    text = path.read_text()
    missing = [item for item in required if item not in text]
    if missing:
        raise SystemExit(f"{path} missing {missing}")
print("Shared stage, stop-rule, and concern/output contract strings exist where required.")
PY
```

Expected: PASS and prints `Shared stage, stop-rule, and concern/output contract strings exist where required.`.

- [ ] **Step 4: Lint the Copilot/Codex skill**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 skills/project-initialization/copilot-codex/SKILL.md
```

Expected: PASS with no errors.

- [ ] **Step 5: Commit the Copilot/Codex skill slice**

```bash
cd /home/hexaper/template
git add skills/project-initialization/copilot-codex/SKILL.md
git commit -m "docs: add Copilot Codex initialization skill"
```

### Task 4: Repoint Primary Entry Docs And Legacy Prompt Inventory

**Files:**
- Modify: `README.md`
- Modify: `AGENTS.md`
- Modify: `CLAUDE.md`
- Modify: `CONTRIBUTING.md`
- Modify: `CHANGELOG.md`
- Modify: `prompts/README.md`
- Modify: `docs/00-source-of-truth.md`
- Modify: `docs/INDEX.md`
- Test: `README.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `prompts/README.md`, `docs/00-source-of-truth.md`, `docs/INDEX.md`

- [ ] **Step 1: Update the root README so the first obvious action is the skill, not the prompt files**

Apply these exact content changes in `README.md`:

```md
A documentation-first project template with a skill-first initialization workflow. It gives you a clean engineering scaffold, a structured documentation tree, and repo-included `project-initialization` skills that guide you from a blank clone through staged project setup before implementation.

The core idea is simple: initialize one coherent stage at a time, keep progress in a tracked initialization plan, and keep repository guidance aligned with the canonical docs created during setup.

The preferred adoption path is now skill-first: after cloning, open `skills/README.md`, choose the tool-specific `project-initialization` skill that matches your AI tool, and keep re-running that same skill until the initialization plan is complete.

A new project using this template goes through one staged initialization workflow before implementation. Each run completes one bounded stage and updates the tracked plan before stopping.

## Project journey

```text
Clone
  │
  ▼
Open skills/README.md
  │
  ▼
Run the matching project-initialization skill
  │
  ▼
Preflight and orientation
  │
  ▼
Project brief
  │
  ▼
PRD
  │
  ▼
Architecture readiness
  │
  ▼
Architecture baseline
  │
  ▼
Repository sync
  │
  ▼
Final coherence review
  │
  ▼
Implementation planning or deeper documentation work
```

## Bootstrap checklist

1. Open `skills/README.md`.
2. Run the matching `project-initialization` skill for your AI tool.
3. Re-run that same skill until the tracked initialization plan is complete.
4. Use `prompts/README.md` only if your tool cannot use the repo-shipped skills or you explicitly want the legacy prompt flow.
5. Replace the placeholder `LICENSE` before publishing.
6. Keep local workspace tooling such as `.opencode/` gitignored and untracked.
7. Commit the initialized baseline before starting feature work.

## Legacy prompt fallback

If you are not using the repo-included skills, the old step-by-step prompt flow remains documented in `prompts/README.md`.
```

Also update the top-level layout table so `skills/` appears as a first-class path and `prompts/` is described as a legacy fallback surface rather than the primary adoption path.

Replace the current opening description and the old four-phase framing with the text above so the README no longer describes AI-assisted prompts as the primary setup system. Delete the now-obsolete prompt-first sections (`## Minimal adoption path`, `## Bootstrap your project`, `## Author the architecture baseline`, and `## Adapt to your stack`) so the README does not keep telling adopters to start in `prompts/` after the skill-first entrypoint is added. Replace the old `See [prompts/README.md]` pointer with a primary pointer to `skills/README.md` and keep `prompts/README.md` only inside the fallback guidance.

- [ ] **Step 2: Update repo routing docs to point at the new skills surface**

Apply these exact wording changes:

```md
# AGENTS.md
- Main directories: `docs/`, `skills/`, `prompts/`, `.github/`, `bin/`, `config/`, `diagrams/`, `examples/`, `scripts/`, `src/`, `tests/`.
- A fresh clone of this template is unspecialized. Start with `skills/README.md` and run the matching `project-initialization` skill until initialization is complete. Use `prompts/README.md` only as a legacy fallback.
- `skills/AGENTS.md` — repo-included skill authoring and contract-maintenance rules.

# CLAUDE.md
For template adoption with Claude, start with `skills/project-initialization/claude/SKILL.md`.
Use `prompts/README.md` only if you are intentionally following the legacy prompt flow.
```

- [ ] **Step 3: Reposition prompts and source-of-truth ownership as fallback, not primary**

Apply these exact content changes:

```md
# prompts/README.md
# Legacy Adoption Prompts

Status: Active
Owner: Template maintainers
Purpose: keep the older prompt-by-prompt template adoption flow available as a fallback when repo-included skills cannot be used
Last updated: 2026-05-07

The preferred adoption path for this template is now the repo-included skill workflow under `skills/`.
Use the files in this directory only when your AI tool cannot use the repo-shipped skills or when you intentionally want the older manual prompt-by-prompt flow.

## Inventory

| Prompt | Purpose | Fallback run order |
| --- | --- | --- |
| [project-bootstrap.md](project-bootstrap.md) | Capture project intent and create the active project brief, PRD, and refreshed root README from starter templates. | First |
| [refine-specs.md](refine-specs.md) | Close blocking gaps in the active project brief and PRD before architecture work. | After bootstrap |
| [architecture-baseline.md](architecture-baseline.md) | Decide the technical approach and create the active solution design and ADRs. | After spec refinement |
| [language-adaptation.md](language-adaptation.md) | Adapt the repository structure to the active architecture documents. | After architecture baseline |

## How to use this fallback flow

1. Start in `skills/README.md` first and use this directory only if the repo-included skills are unavailable in your tool or you intentionally want the legacy flow.
2. Run `project-bootstrap.md`, `refine-specs.md`, `architecture-baseline.md`, and `language-adaptation.md` in that order.
3. Return to the root `README.md` for the remaining repository bootstrap steps after the fallback flow is complete.

# docs/00-source-of-truth.md
| Initialization skills | [../skills/README.md](../skills/README.md) | Primary tracked entrypoint for repo adoption across supported AI tools. |
| Legacy adoption prompts | [../prompts/README.md](../prompts/README.md) | Manual fallback surfaces for tools or users that are not using the repo-shipped skills. |

# docs/INDEX.md under "## Supporting and historical surfaces"
- [../skills/README.md](../skills/README.md) — repo-included skill entrypoint for template adoption and staged initialization.
- [../prompts/README.md](../prompts/README.md) — legacy fallback prompts for tools that cannot use the repo-shipped skills.
```

Remove the old narrow `Architecture baseline prompt` ownership row after adding the broader skill-first and legacy-prompt rows above.

Replace the current `## How adopters use this directory` section entirely so `prompts/README.md` no longer instructs fresh adopters to begin with `project-bootstrap.md` unless they are intentionally using the fallback path.

- [ ] **Step 4: Update contributor-facing notes for the new workflow surface**

Apply these exact additions:

```md
# CONTRIBUTING.md
- If a change affects repository initialization or adoption workflow, update the matching files under `skills/`, the root `README.md`, and `prompts/README.md` in the same change.

# CHANGELOG.md under "## [Unreleased]"
### Changed

- Replaced the prompt-first template adoption entrypoint with repo-included `project-initialization` skills for Claude and Copilot/Codex, while keeping `prompts/` as a documented fallback path.
```

- [ ] **Step 5: Run focused linting on the migrated entrypoint docs**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md prompts/README.md docs/00-source-of-truth.md docs/INDEX.md
```

Expected: PASS with no errors.

- [ ] **Step 6: Verify the new primary-vs-fallback wording is present**

Run:

```bash
cd /home/hexaper/template && rg -n "skills/README\.md|legacy prompt|fallback|project-initialization" README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md prompts/README.md docs/00-source-of-truth.md docs/INDEX.md
```

Expected: output shows `skills/README.md` and `project-initialization` in the primary entry docs, and `legacy prompt` or `fallback` language in `prompts/README.md` and the related guidance docs.

- [ ] **Step 7: Commit the entrypoint-doc migration**

```bash
cd /home/hexaper/template
git add README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md prompts/README.md docs/00-source-of-truth.md docs/INDEX.md
git commit -m "docs: move template adoption to skills"
```

### Task 5: Update Slot References And CI Guardrails

**Files:**
- Modify: `src/AGENTS.md`
- Modify: `bin/README.md`
- Modify: `diagrams/README.md`
- Modify: `examples/README.md`
- Modify: `.github/workflows/ci.yml`
- Test: `src/AGENTS.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md`, `.github/workflows/ci.yml`

- [ ] **Step 1: Update source-layer guidance and frontmatter description to use repository sync as the fill point**

Replace the frontmatter `description` line and the setup paragraph in `src/AGENTS.md` with exactly this text:

```md
description: Use for work under src/. Covers code quality, implementation style, and change discipline. Most fields are slot placeholders that the repo-included project-initialization skill fills during Repository sync.

This file is a **slot template**. A fresh clone of the template is language-agnostic, so the placeholders below are intentionally unfilled. Run the repo-included `project-initialization` skill through the `Repository sync` stage to populate them, or use the legacy fallback in `prompts/language-adaptation.md` if you are not using the repo-shipped skills.
```

- [ ] **Step 2: Update the slot-folder README files to point at the new primary workflow**

Replace the adoption sentence in each file with the matching text below:

```md
# bin/README.md
This directory is a slot. The repo-included `project-initialization` skill may rename, populate, or remove it during `Repository sync` depending on the chosen stack. If you are following the legacy fallback flow instead, `prompts/language-adaptation.md` performs the same role.

# diagrams/README.md
This directory is a slot. The repo-included `project-initialization` skill may populate or remove it during `Repository sync` depending on whether the project will maintain diagram sources alongside the code. If you are following the legacy fallback flow instead, `prompts/language-adaptation.md` performs the same role.

# examples/README.md
This directory is a slot. The repo-included `project-initialization` skill may populate or remove it during `Repository sync` depending on the chosen project type. If you are following the legacy fallback flow instead, `prompts/language-adaptation.md` performs the same role.
```

- [ ] **Step 3: Extend repository-policy CI to guard the new skills surface**

Update `.github/workflows/ci.yml` with these exact changes:

```yaml
      - name: Validate required governance files
        run: |
          test -f README.md
          test -f AGENTS.md
          test -f CONTRIBUTING.md
          test -f SECURITY.md
          test -f CHANGELOG.md
          test -f LICENSE
          test -f .github/CODEOWNERS
          test -f docs/00-source-of-truth.md
          test -f docs/adr/ADR-000-template.md
          test -f skills/README.md
          test -f skills/AGENTS.md
          test -f skills/project-initialization/README.md
          test -f skills/project-initialization/claude/SKILL.md
          test -f skills/project-initialization/copilot-codex/SKILL.md
          test -f prompts/README.md
          test -f prompts/AGENTS.md
          test -f prompts/language-adaptation.md
          test -f prompts/project-bootstrap.md
          test -f prompts/architecture-baseline.md
          test -f prompts/refine-specs.md

      - name: Validate canonical path consistency
        run: |
          ! rg -n --glob '!.github/workflows/ci.yml' "docs/project/adr|docs/project/AGENTS|REPOSTORY_MAP|REPOSITORY_MAP" README.md AGENTS.md CONTRIBUTING.md skills prompts .github diagrams src tests config scripts bin examples docs/README.md docs/INDEX.md docs/00-documentation-standards.md docs/00-source-of-truth.md docs/Architecture.md docs/00_governance docs/01_strategy docs/02_product docs/03_architecture docs/04_ai_governance docs/05_testing_acceptance docs/06_security_operations docs/07_delivery docs/08_references docs/adr
```

- [ ] **Step 4: Run focused validation on slot docs and CI config**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 src/AGENTS.md bin/README.md diagrams/README.md examples/README.md && python3 - <<'PY'
from pathlib import Path
text = Path('.github/workflows/ci.yml').read_text()
required = [
    'skills/README.md',
    'skills/AGENTS.md',
    'skills/project-initialization/README.md',
    'skills/project-initialization/claude/SKILL.md',
    'skills/project-initialization/copilot-codex/SKILL.md',
    'README.md AGENTS.md CONTRIBUTING.md skills prompts .github diagrams src tests config scripts bin examples',
]
missing = [item for item in required if item not in text]
if missing:
    raise SystemExit(f'Missing CI checks: {missing}')
print('CI workflow contains the required skill-surface checks.')
PY
```

Expected: markdownlint passes and the Python check prints `CI workflow contains the required skill-surface checks.`.

- [ ] **Step 5: Commit the slot and CI updates**

```bash
cd /home/hexaper/template
git add src/AGENTS.md bin/README.md diagrams/README.md examples/README.md .github/workflows/ci.yml
git commit -m "ci: guard skill-first adoption workflow"
```

### Task 6: Run Final Integration Review Across The Whole Workflow Surface

**Files:**
- Test: `docs/adr/ADR-001-skill-first-template-adoption.md`, `docs/adr/INDEX.md`, `skills/AGENTS.md`, `skills/README.md`, `skills/project-initialization/README.md`, `skills/project-initialization/testing/pressure-scenarios.md`, `skills/project-initialization/testing/review-checklist.md`, `skills/project-initialization/claude/SKILL.md`, `skills/project-initialization/copilot-codex/SKILL.md`, `README.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `prompts/README.md`, `docs/00-source-of-truth.md`, `docs/INDEX.md`, `src/AGENTS.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md`, `.github/workflows/ci.yml`

- [ ] **Step 1: Run markdownlint across every changed Markdown file**

Run:

```bash
cd /home/hexaper/template && npx -y markdownlint-cli2 docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md skills/**/*.md README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md prompts/README.md docs/00-source-of-truth.md docs/INDEX.md src/AGENTS.md bin/README.md diagrams/README.md examples/README.md
```

Expected: PASS with no errors.

- [ ] **Step 2: Re-run docs validation to make sure the skill-first migration did not break canonical docs**

Run:

```bash
cd /home/hexaper/template && PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs docs/_examples
```

Expected: PASS with `checked 122 file(s); 0 error(s)`.

- [ ] **Step 3: Verify contract parity and local links across the whole changed surface**

Run:

```bash
cd /home/hexaper/template && python3 - <<'PY'
from pathlib import Path
import re

files = [
    Path('skills/README.md'),
    Path('skills/project-initialization/README.md'),
    Path('skills/project-initialization/testing/pressure-scenarios.md'),
    Path('skills/project-initialization/testing/review-checklist.md'),
    Path('skills/project-initialization/claude/SKILL.md'),
    Path('skills/project-initialization/copilot-codex/SKILL.md'),
    Path('docs/adr/ADR-001-skill-first-template-adoption.md'),
    Path('docs/adr/INDEX.md'),
    Path('README.md'),
    Path('AGENTS.md'),
    Path('CLAUDE.md'),
    Path('CONTRIBUTING.md'),
    Path('CHANGELOG.md'),
    Path('prompts/README.md'),
    Path('docs/00-source-of-truth.md'),
    Path('docs/INDEX.md'),
    Path('src/AGENTS.md'),
    Path('bin/README.md'),
    Path('diagrams/README.md'),
    Path('examples/README.md'),
]
pattern = re.compile(r'\[[^\]]+\]\(([^)#]+)\)')
errors = []
for file in files:
    text = file.read_text()
    for target in pattern.findall(text):
        if target.startswith(('http://', 'https://', 'mailto:')):
            continue
        resolved = (file.parent / target).resolve()
        if not resolved.exists():
            errors.append(f'{file} -> {target}')
if errors:
    raise SystemExit('\n'.join(errors))
print('All changed-file local links resolve.')
PY
```

Expected: PASS and prints `All changed-file local links resolve.`.

- [ ] **Step 4: Manually QA the adopter path through its real discovery surface**

Read the changed files in this exact order and confirm the path is obvious and coherent:

1. `README.md`
2. `skills/README.md`
3. `skills/project-initialization/claude/SKILL.md`
4. `skills/project-initialization/README.md`
5. `skills/project-initialization/copilot-codex/SKILL.md`
6. `prompts/README.md`

Expected:

- a fresh adopter sees the skill-first path first
- the chosen tool-specific skill is discoverable in one click from the root README
- the shared contract is discoverable from both tool-specific skills
- prompts are clearly present as fallback rather than primary workflow

- [ ] **Step 5: Review the final diff and working tree after the integration review**

Run:

```bash
cd /home/hexaper/template
git diff --stat
git diff --stat --cached
git status --short
```

Expected: inspect the diff summary and confirm whether the final integration review produced any follow-up edits that are not yet committed.

- [ ] **Step 6: Create one final follow-up commit only if the integration review produced new changes**

Run:

```bash
cd /home/hexaper/template
if [ -n "$(git status --short)" ]; then
  git add docs/adr/ADR-001-skill-first-template-adoption.md docs/adr/INDEX.md skills/AGENTS.md skills/README.md skills/project-initialization/README.md skills/project-initialization/testing/pressure-scenarios.md skills/project-initialization/testing/review-checklist.md skills/project-initialization/claude/SKILL.md skills/project-initialization/copilot-codex/SKILL.md README.md AGENTS.md CLAUDE.md CONTRIBUTING.md CHANGELOG.md prompts/README.md docs/00-source-of-truth.md docs/INDEX.md src/AGENTS.md bin/README.md diagrams/README.md examples/README.md .github/workflows/ci.yml
  git commit -m "docs: finalize skill-first template adoption workflow"
else
  echo "No follow-up commit required."
fi
```

Expected: create a non-empty final commit only when the final integration review produced new edits. If `git status --short` is empty, do not create another commit.
