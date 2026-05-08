---
title: Adoption Orchestrator Skill Design
status: draft
record_class: supporting
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: planning
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Adoption Orchestrator Skill Design

> **Purpose**: define a skill-first repository-initialization workflow that replaces the current prompt-first adoption flow with one orchestrated, resumable process.
> **Audience**: template maintainers designing the adoption workflow for Claude and Copilot/Codex style tools.
> **When to update**: update when the initialization workflow, tool-target strategy, or review-loop contract changes.

## Goal

Ship one repository-included orchestration skill that guides adopters from a fresh clone through project initialization in bounded stages. The workflow must be easy to start, friendly to non-experts, curious about missing context, capable of challenging weak choices, and able to stop after each coherent stage without losing continuity.

## Problem

The current prompt system spreads initialization across multiple Markdown prompts with uneven depth and weak continuity. It encourages long back-and-forth sessions, requires the user to choose the next prompt manually, and does not preserve postponed questions or stage state in a structured way. The replacement should keep the adoption flow professional and documentation-first without requiring the user to act like a project manager.

## Desired Outcomes

- A fresh clone has one obvious starting point in `README.md`.
- The repo includes one orchestration skill as the main initialization entrypoint.
- The orchestrator completes one bounded stage per run, then stops cleanly.
- Stage continuity is preserved in a tracked working plan under `docs/superpowers/plans/`.
- The skill asks only the questions needed for the current stage and parks useful later questions instead of expanding the conversation indefinitely.
- After every stage, the skill runs a review/fix loop so changed docs stay aligned with repository standards and one another.
- The skill explicitly raises concerns about risky user choices, explains the risk, and suggests stronger alternatives.
- At plan completion, the skill runs a final integration review across all changed files so the initialized repo fits together.

## Non-Goals

- Do not generate business logic, feature code, or version-pinned implementation dependencies.
- Do not turn the initialization skill into a generic product-management framework.
- Do not rely on ignored local state as the primary memory mechanism.
- Do not require one long session to finish the entire initialization journey.
- Do not keep `docs/superpowers/` as a source of truth after initialization is complete; durable decisions still belong in canonical docs or ADRs.

## Design Summary

The repository ships one skill-first initialization system centered on a single orchestration skill. The orchestrator owns stage selection, question flow, doc writing, review/fix loops, and plan updates. It works against canonical docs plus a living initialization plan stored in `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md`.

Each invocation follows the same pattern:

1. Read the current plan and relevant canonical docs.
2. Continue the active stage or advance to the next stage.
3. Ask only the minimum questions required to complete the current stage well.
4. Write the current stage's canonical docs in one coherent batch.
5. Run a review/fix loop on the changed files.
6. Update the plan with progress, parked questions, concerns, and next steps.
7. Stop cleanly.

## Skill Architecture

### Main entrypoint

Ship one primary orchestration skill in the repository template. `README.md` instructs adopters to start there immediately after cloning and to keep re-running the same skill until the initialization plan is complete.

The orchestrator is responsible for:

- reading repo guidance and canonical templates
- creating and updating the initialization plan
- deciding which stage to run next
- controlling the current-stage question boundary
- writing canonical docs at the end of each stage
- running review/fix loops
- updating `README.md`, `AGENTS.md`, and related repo guidance during the repo-sync stage
- proposing the next step and stopping

### Tool-specific packaging

The workflow is shared, but the repository ships tool-specific versions of the same orchestration skill.

- **Shared contract**: one design and stage model tracked in the repository and used as the authoritative behavior contract.
- **Claude skill**: a repo-included orchestration skill for Claude-style workflows that can invoke subagents and other skills directly.
- **Copilot/Codex skill**: a repo-included orchestration skill for Copilot/Codex-style workflows that follows the same contract, stage model, and plan file while adapting invocation and review mechanics to that tool.

The packaging may differ per tool, but these must stay identical across tool targets:

- stage names and stop conditions
- plan file structure
- concern-handling rules
- review/fix loop contract
- final integration review contract

## Working Plan Design

### File location

Create one living working-record plan file at:

`docs/superpowers/plans/YYYY-MM-DD-project-initialization.md`

This file is the orchestrator's continuity mechanism. It is not canonical project guidance.

### Required contents

The plan file should contain these sections:

- `Goal` — initialize this repo into a project with coherent, professional baseline docs
- `Current Stage` — exactly one active stage
- `Stage Status` — planned, in progress, completed, blocked, or deferred
- `Project Facts` — confirmed facts learned so far
- `Open Questions For Current Stage` — only blockers for the active stage
- `Parked Questions For Later` — grouped by future stage or owning doc area
- `Concerns And Recommendations` — user-choice concerns, rationale, and stronger alternatives
- `Files Updated This Stage` — canonical and supporting files changed in the current run
- `Review Findings` — what the review/fix loop found and what was fixed
- `Next Recommended Step` — the best next stage to run
- `Resume Context` — a short handoff that lets a later invocation restart cleanly

### Plan mutation rules

- The orchestrator may update the plan after every stage.
- The orchestrator may reshape the remaining plan when project facts justify a different path.
- If the plan changes shape, the skill must explain why, preserve completed work, and record the new next step.
- Parked questions must remain visible until answered, intentionally deferred, or made irrelevant.

## Stage Model

The orchestrator should use these stages:

1. **Preflight and orientation**
   - confirm the repo is still an unspecialized template or note current specialization state
   - create the initialization plan
   - inspect the current docs and templates
   - choose the first documentation stage

2. **Project brief**
   - capture intent, scope, stakeholders, constraints, success measures, and non-goals
   - write or update the canonical project brief
   - park product-detail and technical-detail questions that are too early

3. **PRD**
   - capture users, workflows, requirements, acceptance shape, and product boundaries
   - write or update the canonical PRD
   - surface architecture-blocking gaps without prematurely solving them

4. **Architecture readiness**
   - review whether the project brief and PRD are strong enough for architecture work
   - ask only blocking clarification questions
   - either mark readiness or revise the docs until they are ready

5. **Architecture baseline**
   - discuss durable technical direction, trade-offs, and structural choices
   - write or update the solution design and ADRs
   - raise concerns when user decisions are weak, risky, or mismatched to the project facts

6. **Repository sync**
   - update `README.md`, `AGENTS.md`, source-of-truth/index files, and related guidance so the repo reflects the initialized project direction
   - align onboarding and routing with the canonical docs

7. **Final coherence review**
   - run the last review/fix loop across all changed files in the initialization plan
   - confirm the plan can be closed cleanly

## Stage Execution Contract

Each invocation should complete at most one coherent stage.

For the active stage, the orchestrator must:

1. read the plan and relevant canonical docs
2. determine whether to continue the current stage or advance to the next one
3. ask only the questions required to finish the current stage well
4. park unrelated but useful questions into later-stage queues
5. update the stage's canonical docs in one coherent batch at the end of the discussion
6. run the review/fix loop
7. update the plan
8. propose the next step
9. stop

The orchestrator should not spill deeply into the next stage after completing the current one.

## Review/Fix Loop Contract

### Per-stage review loop

After every stage that changes files, the orchestrator must run a focused review/fix loop on the changed docs and guidance files.

The loop should check for:

- repository documentation standards
- cross-document consistency
- source-of-truth alignment
- path and link validity
- placeholder and open-question discipline
- scope drift and implementation leakage
- missing `README.md`, `AGENTS.md`, index, or source-of-truth updates triggered by the new docs

### Review subagents

The orchestrator may dispatch subagents for review, but they are reviewers rather than owners. Recommended review passes are:

- **standards review** — documentation rules, formatting, path validity, template hygiene
- **coherence review** — consistency across changed docs, stage outputs, and source-of-truth ownership
- **final integration review** — at plan completion, across all changed files touched during initialization

The orchestrator remains responsible for collecting findings, deciding which fixes are objective, applying those fixes, and recording advisory concerns that should remain visible to the user.

### Fix policy

- Fix objective documentation issues immediately.
- Do not silently override a user choice that is merely risky or suboptimal.
- Record remaining concerns in the plan when they are advisory rather than blocking.

## Concern-Handling Contract

When the user makes a choice that is weak, risky, or poorly matched to the stated goals, the orchestrator should challenge it directly but constructively.

The skill must say:

- what the concern is
- why it matters in this project's context
- what stronger option may fit better
- what trade-off the stronger option introduces
- whether the original choice is still acceptable if kept intentionally

This keeps the workflow useful to non-experts without turning it into a rigid gatekeeper.

## Output Contract For Each Run

At the end of every stage run, the skill should emit the same short handoff format:

- `Stage Completed`
- `Docs Updated`
- `Review Fixes Applied`
- `Concerns And Recommendations`
- `Parked Questions`
- `Next Recommended Step`

This output should be concise enough to end the session cleanly while leaving a clear restart point.

## README And Repository Entry Design

The root `README.md` should present a simple initialization path:

1. clone the template
2. run the project-initialization skill
3. continue re-running the same skill until the initialization plan is complete

The README should explain that:

- the skill works stage by stage rather than all at once
- the skill stores progress and deferred questions in a tracked initialization plan
- the skill may raise concerns about risky choices and suggest stronger alternatives
- the skill updates repository guidance files during repo sync so onboarding stays aligned with the created docs

## Relationship To Canonical Docs And ADRs

- Canonical project facts belong in the root `docs/` tree.
- Durable technical decisions belong in `docs/adr/`.
- The initialization plan and design spec are working-history records under `docs/superpowers/`.
- The orchestrator may use the plan as memory, but it must move any durable guidance into canonical docs or ADRs as part of the appropriate stage.

## Risks And Design Trade-Offs

### Benefits

- one clear entrypoint for adopters
- shorter and more bounded conversations
- better continuity across sessions
- better support for non-expert users
- explicit review/fix discipline after every stage
- easier to challenge questionable project choices without derailing the flow

### Costs

- more orchestration logic than the current prompt files
- per-tool wrapper maintenance for Claude and Copilot/Codex
- extra plan-management discipline to avoid stale parked questions

### Main risk

If the orchestrator becomes too eager to continue into adjacent stages, it will recreate the long, exhausting conversation pattern this design is trying to avoid. The stop condition after each coherent stage is therefore a core product requirement, not a convenience.

## Recommended Implementation Direction

Implement this as a skill-first system with one orchestration contract delivered as repo-included tool-specific skills, then retire or reduce the current prompt-first adoption flow. Keep the repository contract centered on one visible entrypoint, one living initialization plan, one per-stage review/fix loop, and one final integration review.

## Related documents

- [../../README.md](../../README.md) — current template entrypoint that will need to point users to the new skill-first workflow.
- [../../../prompts/README.md](../../../prompts/README.md) — current prompt inventory that this design intends to replace or reduce.
- [../README.md](../README.md) — working-history policy for `docs/superpowers/`.
- [../plans/README.md](../plans/README.md) — plan-file conventions for tracked working plans.
- [../../adr/README.md](../../adr/README.md) — canonical home for durable implementation decisions.
