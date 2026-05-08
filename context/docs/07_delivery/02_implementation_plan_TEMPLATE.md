---
title: Implementation Plan Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: implementation-lead
capability: execution
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Implementation Plan Template

> **Purpose**: define how approved work will be executed, validated, cut over, and backed out if needed.
> **Audience**: implementation leads, delivery managers, and managers coordinating the detailed execution approach.
> **When to update**: update when sequencing, environments, cutover assumptions, rollback approach, or runbook references change.

## How to use this template

Use this template once the delivery baseline is approved and the team needs an executable implementation approach. Keep it closer to operational reality than the delivery plan, but still focused on canonical decisions rather than ticket-level tasks.

- Mandatory: approach, sequencing, environments, cutover strategy, rollback, and runbook reference.
- Optional: workstream-specific appendices if the implementation is unusually complex.
- Remove duplicated schedule detail that already lives in stage plans or tooling.

## What not to include

- **Detailed cutover runbook steps** — step-by-step execution belongs in the cutover and rollback template ([08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md)). This plan summarises the strategy; the runbook provides the ordered steps.
- **Product requirements or acceptance criteria** — requirements belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)) and requirements catalog. This plan covers execution approach, not product scope.
- **Architecture decisions or design rationale** — design decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Reference relevant ADRs here; do not re-explain their rationale.
- **Sprint-level backlog or ticket detail** — day-to-day task tracking belongs in delivery tooling. The implementation plan covers canonical decisions about approach and sequence, not individual tasks.
- **Migration data or SQL scripts** — detailed migration content belongs in the migration plan ([09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md)) and version-controlled scripts.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Approach

Describe the overall implementation style and why it suits the initiative. Readers should understand whether the team is executing a phased rollout, a migration wave model, a pilot-first approach, or another pattern.

- Example: pilot-first release followed by staged regional rollout.
- Example: technical migration executed in parallel with business process change.

## Sequencing

Explain the order of the main implementation steps and the rationale behind that order. Good sequencing shows what must happen first, what can run in parallel, and what creates the final release path.

- Example: prepare environments, complete integrations, validate data, rehearse cutover, then release.
- Example: parallel workstreams converge at a readiness gate before launch.

## Environments

List the environments or execution contexts involved and how they will be used. Keep the section focused on decisions that affect readiness, testing, and cutover confidence.

- Example: development, test, staging, rehearsal, production.
- Example: environment entry criteria, access owner, or data-handling notes.

## Cutover strategy

Summarize the intended release transition model without recreating the full runbook. This section should prepare readers for the dedicated cutover document and align expectations early.

- Example: blue-green switch, phased migration, weekend cutover, or pilot release.
- Example: decision point requiring readiness tracker green status before execution.

## Rollback

Explain the rollback philosophy and the conditions that would trigger it. Teams should know whether rollback restores prior state, pauses rollout, or uses feature toggles and staged disablement.

- Example: full rollback if critical transaction failures exceed threshold.
- Example: partial rollback for one region while the rest of the rollout remains live.

## Runbook reference

Point to the operational documents and checklists the implementation depends on. This keeps the implementation plan anchored to the operational acceptance surface instead of assuming it will appear later.

- Example: release plan, cutover runbook, service acceptance checklist, support handover.
- Example: named owner for each referenced runbook or operational procedure.

## Related documents

- [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md) — provides the higher-level execution baseline this plan elaborates.
- [../00_governance/12_requirements_traceability_matrix_TEMPLATE.md](../00_governance/12_requirements_traceability_matrix_TEMPLATE.md) — ties implementation work back to approved requirements and verification.
- [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md) — contains the ordered execution and fallback runbooks referenced here.
