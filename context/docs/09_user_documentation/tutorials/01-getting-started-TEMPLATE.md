---
title: Getting Started Tutorial Template
status: draft
record_class: canonical
audience: [end_user]
owner: Documentation maintainers
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# Getting Started Tutorial Template

> **Purpose**: provide a fillable tutorial scaffold for teaching a new user one complete, successful end-to-end workflow.
> **Audience**: end users who are learning the product for the first time and need a guided path.
> **When to update**: update when onboarding flow, prerequisites, sample steps, or verification checkpoints change.

## How to use this template

Copy this file when you need a first-run tutorial that can be followed without prior product knowledge. Keep the path linear, use real commands or UI actions, and make every step observable.

- Keep the learner moving toward one visible outcome.
- Include exact commands, button labels, sample values, or screenshots where they remove ambiguity.
- Remove optional branches unless they are essential to finishing the lesson.

## What not to include

- **Reference material for exact fields, options, or error codes** — these belong in reference pages ([../reference/01-feature-reference-TEMPLATE.md](../reference/01-feature-reference-TEMPLATE.md)). A tutorial may name a field; the reference page lists all allowed values and error states.
- **Conceptual explanations or design tradeoffs** — rationale and mental model content belong in explanation pages ([../explanation/01-concept-explanation-TEMPLATE.md](../explanation/01-concept-explanation-TEMPLATE.md)). Tutorials focus on observable steps and outcomes, not on why things work a certain way.
- **Day-two or advanced task procedures** — once the reader has completed the tutorial, specific follow-on tasks belong in how-to guides ([../how-to/01-perform-a-common-task-TEMPLATE.md](../how-to/01-perform-a-common-task-TEMPLATE.md)). Tutorials should end at the first success state and link forward.
- **Multiple independent tutorials in one page** — each tutorial teaches one complete workflow. If two workflows are not meaningfully connected, they each deserve their own tutorial page.
- **Internal onboarding for team members** — staff onboarding belongs in the project initiation document ([../../00_governance/02_project_initiation_document_TEMPLATE.md](../../00_governance/02_project_initiation_document_TEMPLATE.md)) or internal runbooks. This template targets end users of the product, not project contributors.

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

## What you'll build

Start with one short paragraph describing the finished outcome the learner will achieve. This section should motivate the exercise and give the reader a picture of success before they begin.

- Example: create your first workspace, invite a teammate, and send a test update.
- Example: connect an account, run one workflow, and confirm the result appears in the dashboard.

## Prerequisites

List only what the learner truly needs before starting, including accounts, access, versions, and sample data. Keep the list short so the tutorial still feels approachable.

- Example: active account, browser version, test environment URL, starter credentials.
- Example: local CLI installed and authenticated with `tool login`.

## Steps

Write the tutorial as a numbered sequence with no missing transitions. Each step should say what to do, what to enter, and what the learner should expect to see before moving on.

1. Open `[entry point]` and select **[action]**.
2. Enter the sample values below, then choose **Continue**.

   ```text
   Name: Example workspace
   Region: Test
   Visibility: Private
   ```

3. Run the first command or confirm the first screen state.

   ```bash
   app-cli example create --name "Example workspace"
   ```

4. Repeat until the learner reaches one clear end state.

## Verifying

Show the learner how to confirm the tutorial worked. Verification should rely on visible output, a page state, or a simple follow-up command.

- Example: the dashboard shows one active workspace and no setup warnings.
- Example command:

  ```bash
  app-cli example list
  ```

## What you learned

Summarize the skills or concepts the learner practiced so the lesson feels complete. Keep the recap short and tied to the tutorial goal.

- Example: created a workspace, confirmed access, and ran the first successful action.
- Example: learned where to find status, settings, and next-step actions.

## Next steps

Route the learner to the most useful follow-on material instead of extending the tutorial indefinitely. Link to a how-to for the next practical task, a reference page for exact syntax, and an explanation page for deeper understanding.

- Example: move to a task guide for day-two operations.
- Example: read the feature reference for all available options.

## Related documents

- [README.md](README.md) — explains when a guided tutorial is the right documentation mode.
- [../how-to/01-perform-a-common-task-TEMPLATE.md](../how-to/01-perform-a-common-task-TEMPLATE.md) — use this when the reader already knows the basics and needs a specific task.
- [../reference/01-feature-reference-TEMPLATE.md](../reference/01-feature-reference-TEMPLATE.md) — use this for exact commands, parameters, fields, and errors.
- [../explanation/01-concept-explanation-TEMPLATE.md](../explanation/01-concept-explanation-TEMPLATE.md) — use this for background and tradeoffs behind the tutorial workflow.
- [../release-notes/2026-05-07-template-bootstrap.md](../release-notes/2026-05-07-template-bootstrap.md) — record user-visible onboarding changes by release.
