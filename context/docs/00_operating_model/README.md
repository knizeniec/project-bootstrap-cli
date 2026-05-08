---
title: Operating Model
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Operating Model

> **Purpose**: orient readers to the operating-model layer that explains how this documentation system is structured, governed, tailored, and consumed.
> **Audience**: internal teams and managers shaping or adopting the documentation model.
> **When to update**: update when lifecycle rules, metadata rules, audience profiles, or operating-model navigation change.

## How to use this template

Use this folder as the operating handbook for the docs system before creating or tailoring area-specific artifacts. Read the lifecycle map first, then the role and source-of-truth maps, then the schema and naming rules. Keep this landing page short and use it to route readers to the canonical detail pages below.

- Start with [01_lifecycle_map.md](01_lifecycle_map.md) to understand phases and evidence.
- Then read [02_role_audience_map.md](02_role_audience_map.md) to match readers to docs.
- Use [04_frontmatter_schema.md](04_frontmatter_schema.md) before creating or editing canonical records.

## What this folder is

This folder defines the operating rules for the documentation system rather than project-specific content. It explains lifecycle stages, roles, metadata, naming, tailoring, and decision rights so other docs can stay consistent. Treat these pages as the common operating layer for the numbered tree.

- Example use: check the lifecycle map before deciding which artifacts are needed for a new initiative.
- Example use: check the source-of-truth map before mirroring content from Jira, GitHub, or a wiki.
- Example use: check the naming rules before adding a new canonical artifact.

## Reading order

The recommended reading order moves from orientation to rules to tailoring. That keeps readers from applying metadata or naming rules without understanding the lifecycle and audience model first. Teams can then branch into profile-specific guidance once the baseline is clear.

1. [01_lifecycle_map.md](01_lifecycle_map.md)
2. [02_role_audience_map.md](02_role_audience_map.md)
3. [03_source_of_truth_map.md](03_source_of_truth_map.md)
4. [04_frontmatter_schema.md](04_frontmatter_schema.md)
5. [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md)
6. [06_naming_and_structure.md](06_naming_and_structure.md)
7. [07_tailoring_matrix.md](07_tailoring_matrix.md)
8. [08_decision_rights_matrix.md](08_decision_rights_matrix.md)

## Pointers to lifecycle map and role map

The lifecycle map is the main entry point for deciding what evidence a project should maintain at each phase. The role and audience map is the main entry point for deciding who should read, approve, or receive each class of document. Use both together when tailoring a project baseline.

- Lifecycle anchor: [01_lifecycle_map.md](01_lifecycle_map.md)
- Role and audience anchor: [02_role_audience_map.md](02_role_audience_map.md)
- Metadata anchor: [04_frontmatter_schema.md](04_frontmatter_schema.md)

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Tailoring profile | Recommended templates | Skip |
|---|---|---|
| **Minimum** — single team, no external compliance, internal tooling or prototype | [01_lifecycle_map.md](01_lifecycle_map.md) — understand phases and evidence; [04_frontmatter_schema.md](04_frontmatter_schema.md) — apply consistent metadata | [02_role_audience_map.md](02_role_audience_map.md), [03_source_of_truth_map.md](03_source_of_truth_map.md), [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md), [06_naming_and_structure.md](06_naming_and_structure.md), [07_tailoring_matrix.md](07_tailoring_matrix.md), [08_decision_rights_matrix.md](08_decision_rights_matrix.md) |
| **Standard** — multi-team or customer-facing product, moderate stakeholder reporting, no regulatory requirement | All minimum templates plus [02_role_audience_map.md](02_role_audience_map.md) — match readers to docs; [03_source_of_truth_map.md](03_source_of_truth_map.md) — avoid duplication with tools; [06_naming_and_structure.md](06_naming_and_structure.md) — ensure consistency | [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md), [07_tailoring_matrix.md](07_tailoring_matrix.md), [08_decision_rights_matrix.md](08_decision_rights_matrix.md) |
| **High-assurance** — regulated environment, external audit trail, multi-team with distinct ownership and approval paths | Full set: all standard templates plus [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md) — define audience-gated exports; [07_tailoring_matrix.md](07_tailoring_matrix.md) — record tailoring decisions explicitly; [08_decision_rights_matrix.md](08_decision_rights_matrix.md) — authority and approval mapping | Nothing — all documents apply |

**Rules of thumb:**
- Promote a single tailoring decision to [07_tailoring_matrix.md](07_tailoring_matrix.md) only when an auditor or a new team member needs to understand why an artifact was added or removed.
- Add [08_decision_rights_matrix.md](08_decision_rights_matrix.md) as soon as approval authority is split across more than one team or organisation boundary.
- Keep [04_frontmatter_schema.md](04_frontmatter_schema.md) in every profile — consistent metadata is the minimum contract that makes other docs findable.

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [02_role_audience_map.md](02_role_audience_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [../00-source-of-truth.md](../00-source-of-truth.md)
