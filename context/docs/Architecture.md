---
title: Documentation Architecture
status: active
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Documentation Architecture

> **Purpose**: define the structure of the active documentation system and the role of each documentation surface.
> **Audience**: maintainers and contributors shaping the repository-wide docs model.
> **When to update**: update when the docs tree, document classes, or control-spine responsibilities change.

This repository keeps one active documentation system rooted under `docs/`. Canonical content lives in the control spine plus the numbered areas from [00_operating_model/](00_operating_model/README.md) through [09_user_documentation/](09_user_documentation/README.md).

[docs/adr/](adr/README.md) stores durable architecture decisions. [superpowers/](superpowers/README.md) stores working history. [99_archive/](99_archive/README.md) stores evidence and retired material.

## Active model

```text
docs/
|- README.md
|- Architecture.md
|- 00-documentation-standards.md
|- 00-source-of-truth.md
|- INDEX.md
|- 00_operating_model/
|- 00_governance/
|- 01_strategy/
|- 02_product/
|- 03_architecture/
|- 04_ai_governance/
|- 05_testing_acceptance/
|- 06_security_operations/
|- 07_delivery/
|- 08_references/
|- 09_user_documentation/
|- 99_archive/
|- adr/
`- superpowers/
```

## Document classes

| Class | Surface | Role |
| --- | --- | --- |
| Control spine | [README.md](README.md), [Architecture.md](Architecture.md), [00-documentation-standards.md](00-documentation-standards.md), [00-source-of-truth.md](00-source-of-truth.md), [INDEX.md](INDEX.md) | Entry, structure, standards, ownership, and navigation for the active docs system. |
| Operating layer | [00_operating_model/](00_operating_model/README.md) | Lifecycle, audience, schema, naming, tailoring, and decision-rights guidance for the whole tree. |
| Active domain docs | [00_governance/](00_governance/README.md) through [09_user_documentation/](09_user_documentation/README.md) | Canonical documentation by capability area. |
| Starter templates | Named `_TEMPLATE.md` files in canonical areas | Reusable bootstrap surfaces for project-specific documentation. |
| ADRs | [adr/](adr/README.md) | Durable implementation and architecture decisions. |
| Working history | [superpowers/](superpowers/README.md) | Specs, plans, and tracked work records that do not define the active baseline. |
| Archive/evidence | [99_archive/](99_archive/README.md) | Historical evidence and retired material kept for traceability only. |

## Anchoring

The numbered domain folders are a custom convention chosen for this template. They are not a one-to-one mapping to a single published standard.

Adopters who must align with arc42 can map the folders as follows:

| Folder | arc42 alignment |
| --- | --- |
| `00_operating_model/` | Project-specific operating wrapper around lifecycle, audience, and metadata rules |
| `00_governance/` | Quality goals, stakeholders, constraints (arc42 §1, §2) |
| `01_strategy/` | Strategic context (arc42 §1) |
| `02_product/` | Requirements and quality goals (arc42 §1, §10) |
| `03_architecture/` | Solution strategy, building blocks, runtime, deployment, crosscutting (arc42 §4-§8) |
| `04_ai_governance/` | Quality requirements and risks specific to AI (arc42 §10, §11) |
| `05_testing_acceptance/` | Quality scenarios and verification evidence (arc42 §10) |
| `06_security_operations/` | Cross-cutting concepts and runtime view for operations (arc42 §6, §8) |
| `07_delivery/` | Deployment view and risk or technical debt framing (arc42 §7, §11) |
| `08_references/` | Glossary and external references (arc42 §12) |
| `09_user_documentation/` | User-facing guidance outside the core arc42 structure |
| `adr/` | Architecture decisions (arc42 §9) |

Adopters who prefer this template's taxonomy without arc42 alignment can ignore the table above.

## Change rules

- If the documentation structure changes, update [README.md](README.md), [Architecture.md](Architecture.md), [00-documentation-standards.md](00-documentation-standards.md), [00-source-of-truth.md](00-source-of-truth.md), and [INDEX.md](INDEX.md) together.
- If lifecycle, audience, or metadata rules change, update [00_operating_model/](00_operating_model/README.md) in the same patch.
- If canonical ownership changes, update [00-source-of-truth.md](00-source-of-truth.md) in the same patch.
- If a durable implementation decision changes, update the relevant ADR in [adr/](adr/README.md).
- If archive or working-history handling changes, update the matching README in [99_archive/](99_archive/README.md) or [superpowers/](superpowers/README.md).

## Related documents

- [README.md](README.md) — root navigation for the docs system.
- [00-documentation-standards.md](00-documentation-standards.md) — writing and maintenance rules.
- [00-source-of-truth.md](00-source-of-truth.md) — canonical ownership map.
- [00_operating_model/README.md](00_operating_model/README.md) — operating layer for lifecycle and schema rules.
- [09_user_documentation/README.md](09_user_documentation/README.md) — end-user documentation surface.
