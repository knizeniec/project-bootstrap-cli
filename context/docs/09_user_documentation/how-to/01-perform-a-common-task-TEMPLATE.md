---
title: Perform a Common Task How-To Template
status: draft
record_class: canonical
audience: [end_user]
owner: Documentation maintainers
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# Perform a Common Task How-To Template

> **Purpose**: provide a fillable scaffold for guiding an end user through one specific task from goal to verification.
> **Audience**: end users who know the basics and need a direct procedure for a practical outcome.
> **When to update**: update when task flow, prerequisites, common failure points, or verification steps change.

## How to use this template

Copy this file when you need a focused action guide for one repeatable task. Keep the title outcome-based, keep the steps tight, and avoid background explanation unless it directly affects success.

- One page should solve one task well.
- Use numbered steps and exact labels for commands, buttons, fields, or menus.
- Move deep rationale to an explanation page and exact field lists to reference.

## What not to include

- **Conceptual background or tradeoff explanations** — rationale and mental model content belong in explanation pages ([../explanation/01-concept-explanation-TEMPLATE.md](../explanation/01-concept-explanation-TEMPLATE.md)). Keep how-to guides focused on steps, not on why the product works a certain way.
- **First-run guided tutorials with sample data** — onboarding experiences belong in tutorials ([../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md)). A how-to assumes the reader already knows the basics; a tutorial teaches them.
- **Exact option lists, field descriptions, and error codes** — these belong in reference pages ([../reference/01-feature-reference-TEMPLATE.md](../reference/01-feature-reference-TEMPLATE.md)). A how-to may mention a field name; the reference page lists all allowed values.
- **Internal implementation procedures or operational runbooks** — system-level procedures for operators belong in runbooks ([../../06_security_operations/11_runbook_TEMPLATE.md](../../06_security_operations/11_runbook_TEMPLATE.md)). This template targets end users, not internal operations teams.
- **Multiple unrelated tasks in one page** — keep one how-to per distinct user goal. Tasks that belong together in a flow should each have their own page with cross-links.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[end_user]` | This folder targets end users; add `internal` only for dual-purpose content |
| `capability` | `user_docs` | Fixed for this folder |
| `phase` | `n/a` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

## Goal

Start with one sentence that states the task outcome in user language. The reader should know immediately whether this page solves their problem.

- Example: export activity data for one workspace.
- Example: change notification preferences for a team.

## Prerequisites

List the minimum access, setup, or prior state the user needs. Keep prerequisites specific so the user can tell quickly whether they are ready.

- Example: editor or admin role, active workspace, feature enabled.
- Example: CLI installed and logged in, or browser session already authenticated.

## Steps

Write a short ordered procedure that gets the user from the starting state to the desired result. Each step should be actionable and avoid hidden assumptions.

1. Open **[location]**.
2. Choose **[action]**.
3. Enter the required values.

   ```text
   Format: CSV
   Date range: Last 30 days
   Include archived items: No
   ```

4. Confirm the task completes.

   ```bash
   app-cli task run --target example
   ```

## Verification

Show how the user can confirm the task worked without guessing. Good verification checks a visible result, download, state change, or returned output.

- Example: exported file appears with the expected date range and row count.
- Example: settings page now shows the updated value and a success message.

## Troubleshooting

List the most common blockers, the likely cause, and the fastest corrective action. Keep this section practical and short so it stays useful during real work.

- Example: permission denied — confirm the user has the required role.
- Example: empty output — widen the date range or confirm data exists in scope.

## Related how-tos

Link to adjacent procedures that users often need next or instead. This keeps how-to content modular while still supporting real task flows.

- Example: archive the exported report after download.
- Example: share the result with another user or revert the change.

## Related documents

- [README.md](README.md) — explains when to use a how-to instead of another documentation mode.
- [../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md) — use this if the reader first needs guided onboarding.
- [../reference/01-feature-reference-TEMPLATE.md](../reference/01-feature-reference-TEMPLATE.md) — use this for exact options, fields, and errors referenced by the task.
- [../explanation/01-concept-explanation-TEMPLATE.md](../explanation/01-concept-explanation-TEMPLATE.md) — use this for background that explains why the task works this way.
- [../CHANGELOG.md](../CHANGELOG.md) — record notable user-facing task changes in the rolling change history.
