---
title: References Index
status: draft
record_class: canonical
audience: [internal, manager]
owner: references-maintainer
capability: references
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# References Index

> **Purpose**: provide a compact, generation-friendly index for the reference artifacts in this folder.
> **Audience**: internal teams and managers who need a fast path to standards, glossary terms, policy mappings, and reusable lessons.
> **When to update**: update when reference files are added, renamed, or reclassified.

## How to use this template

Use this page as the folder-level index, not as the place to restate the contents of every artifact. Keep titles, paths, and one-line scopes stable so future frontmatter-based index generation can replace or extend this file cleanly.

- Keep each row aligned to the file title and current path.
- Prefer stable summaries over long prose.
- Use [../INDEX.md](../INDEX.md) for repo-wide navigation and this page for folder-local routing.

## Folder records

| Path | Title | Scope | Status |
| --- | --- | --- | --- |
| [README.md](README.md) | Reference Templates | Folder purpose, reading order, and artifact chooser. | draft |
| [01_standards_register_TEMPLATE.md](01_standards_register_TEMPLATE.md) | Standards Register Template | Adopted, adapted, and reference-only standards with rationale and target areas. | draft |
| [02_glossary_TEMPLATE.md](02_glossary_TEMPLATE.md) | Glossary Template | Canonical terms, definitions, examples, and source links. | draft |
| [03_decision_rights_matrix_TEMPLATE.md](03_decision_rights_matrix_TEMPLATE.md) | Decision Rights Matrix Template | RACI or DACI decision authority model tied to governance and change control. | draft |
| [04_policy_mappings_TEMPLATE.md](04_policy_mappings_TEMPLATE.md) | Policy Mappings Template | External policies and standards mapped to internal artifacts and evidence. | draft |
| [05_lessons_learned_index_TEMPLATE.md](05_lessons_learned_index_TEMPLATE.md) | Lessons Learned Index Template | Reusable lessons linked to reviews, owners, and action status. | draft |

## Generated-index notes

This file is intentionally structured so a future frontmatter-driven generator can rebuild it with minimal manual cleanup. Keep file paths concrete, keep summaries short, and avoid embedding volatile project-specific details here.

- Good future source fields: `title`, `status`, `audience`, `capability`, `phase`, and `last_reviewed`.
- Good future groupings: by record type, by audience, and by related lifecycle phase.
- Avoid: duplicated rationale that already belongs in the file being indexed.

## Root navigation pointer

This folder is one part of the larger documentation system. Use the root index when you need to move from references into governance, product, architecture, delivery, testing, operations, or archive spaces.

- Root index: [../INDEX.md](../INDEX.md)
- Root landing page: [../README.md](../README.md)
- Operating-model entrypoint: [../00_operating_model/README.md](../00_operating_model/README.md)

## Related documents

- [README.md](README.md) — folder purpose and reading order.
- [../INDEX.md](../INDEX.md) — root navigation map this index points back to.
- [../README.md](../README.md) — root entrypoint for the canonical docs tree.
- [../00_operating_model/03_source_of_truth_map.md](../00_operating_model/03_source_of_truth_map.md) — explains why this folder stays a reference layer rather than owning delivery or policy execution.
