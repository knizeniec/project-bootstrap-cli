---
title: Naming and Structure
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Naming and Structure

> **Purpose**: define folder and file naming conventions, ownership expectations, and the one-artifact-one-job rule.
> **Audience**: internal teams and managers adding or reorganizing canonical documentation.
> **When to update**: update when naming conventions, area ownership, or structural rules change.

## How to use this template

Use this page before adding new canonical files or folders so the docs tree stays predictable. Favor small, purpose-driven files over broad catch-all notes. If a proposed name or location feels ambiguous, resolve that here before creating the artifact.

- Check this page before adding a new numbered area or template.
- Use [04_frontmatter_schema.md](04_frontmatter_schema.md) alongside this page when creating a new canonical file.
- Use [01_lifecycle_map.md](01_lifecycle_map.md) to confirm the artifact's role in the operating flow.

## Naming conventions

Names should tell readers what the artifact does without requiring tribal knowledge. Use numeric prefixes only at the top-level area or ordered sequence level, and keep filenames noun-based rather than status-based. Avoid vague or disposable terms because they accumulate duplicate content quickly.

- Use top-level ordered names such as `00_governance/`, `01_strategy/`, `07_delivery/`.
- Use noun-based artifact names such as `release_plan`, `decision_rights_matrix`, `source_of_truth_map`.
- Do not use names like `notes`, `misc`, `stuff`, or `final`.

## Folder ownership

Each numbered area should have a clear concern and an obvious landing page. Folder ownership is about canonical responsibility, not about exclusive team access. If a document spans concerns, place it with the primary owner and link from related areas.

- Governance owns control and traceability records.
- Architecture owns technical structure and durable implementation decisions.
- Operating model owns cross-cutting operating rules for how the documentation system works.

## One-artifact-one-job rule

Each canonical artifact should do one main job well: explain, instruct, decide, record, or reference. This keeps navigation simple and reduces the chance that a single file becomes both process manual and project log. If a file needs multiple jobs, split it and connect the parts with links.

- Good split: separate lifecycle rules from a project-specific delivery plan.
- Good split: separate frontmatter schema from export profile guidance.
- Anti-pattern: one document that mixes governance policy, architecture design, and weekly status.

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [03_source_of_truth_map.md](03_source_of_truth_map.md)
- [../INDEX.md](../INDEX.md)
