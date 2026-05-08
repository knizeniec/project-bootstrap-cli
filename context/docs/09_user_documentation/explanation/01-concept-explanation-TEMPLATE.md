---
title: Concept Explanation Template
status: draft
record_class: canonical
audience: [end_user]
owner: Documentation maintainers
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# Concept Explanation Template

> **Purpose**: provide a fillable scaffold for explaining a product concept, background idea, or design tradeoff to end users.
> **Audience**: end users who need a clearer mental model to use the product confidently or make better decisions.
> **When to update**: update when the concept, rationale, tradeoffs, or recommended mental model changes.

## How to use this template

Copy this file when users need to understand an idea rather than follow instructions. Keep the writing concept-first, structured, and linked to practical docs instead of burying procedures inside the explanation.

- Start from the reader's likely question or misconception.
- Use short examples to clarify the concept.
- Link to how-to and reference pages for action and lookup details.

## What not to include

- **Step-by-step task instructions** — procedures and task steps belong in how-to guides ([../how-to/01-perform-a-common-task-TEMPLATE.md](../how-to/01-perform-a-common-task-TEMPLATE.md)). Explanation pages orient the reader; they do not walk through a task sequence.
- **Exact field references, option lists, or error codes** — these belong in reference pages ([../reference/01-feature-reference-TEMPLATE.md](../reference/01-feature-reference-TEMPLATE.md)). An explanation may mention that fields or options exist, but the canonical list stays in reference.
- **First-run onboarding exercises** — guided learning experiences with sample values and step-by-step verification belong in tutorials ([../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md)).
- **Internal architecture or implementation decisions** — design rationale and ADR-level decisions belong in architecture documents ([../../03_architecture/01_solution_design_TEMPLATE.md](../../03_architecture/01_solution_design_TEMPLATE.md)) and ADRs. Explain user-visible concepts and tradeoffs, not internal system internals.
- **Release notes or change history** — what changed and when belongs in the changelog or release notes. An explanation describes how something works, not its version history.

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

## Background

Open with the context that makes the concept worth understanding. This section should explain what user problem, workflow, or product behavior creates the need for the concept.

- Example: why the product separates personal and shared workspaces.
- Example: why a workflow has draft and published states.

## The concept

Define the concept in plain language, then explain how it behaves in the product. Keep the explanation concrete enough that readers can connect it to what they see on screen.

- Example: roles determine who can view, edit, approve, or release content.
- Example: synchronization means data is queued, confirmed, and only then visible to all collaborators.

## Alternatives and tradeoffs

Compare the chosen concept with the main alternatives a user might expect. Tradeoff language helps users understand why the product behaves a certain way and what constraints come with that choice.

- Example: stricter approval flow improves control but slows immediate publishing.
- Example: shared defaults reduce setup time but may require local overrides for advanced teams.

## Further reading

Point readers to the next useful documents once they understand the idea. Link one page for action, one for exact facts, and one for release or change context if relevant.

- Example: task guide for performing the action governed by this concept.
- Example: reference page listing supported fields, roles, or states.

## Related documents

- [README.md](README.md) — explains when an explanation page is the right documentation mode.
- [../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md) — use this to teach the concept through a guided lesson.
- [../how-to/01-perform-a-common-task-TEMPLATE.md](../how-to/01-perform-a-common-task-TEMPLATE.md) — use this for task steps influenced by the concept.
- [../reference/01-feature-reference-TEMPLATE.md](../reference/01-feature-reference-TEMPLATE.md) — use this for exact fields, options, limits, or errors.
- [../CHANGELOG.md](../CHANGELOG.md) — record concept-level user-facing behavior changes over time.
