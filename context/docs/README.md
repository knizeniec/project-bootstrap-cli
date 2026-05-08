---
title: Documentation
status: active
record_class: supporting
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Documentation

> **Purpose**: serve as the root entry point for the active documentation system under `docs/`.
> **Audience**: contributors, maintainers, and managers orienting themselves in the repository docs.
> **When to update**: update when root navigation, contributor workflow, or canonical folder structure changes.

## Start here fast

- Lifecycle map: [00_operating_model/01_lifecycle_map.md](00_operating_model/01_lifecycle_map.md)
- Role map: [00_operating_model/02_role_audience_map.md](00_operating_model/02_role_audience_map.md)
- Frontmatter schema: [00_operating_model/04_frontmatter_schema.md](00_operating_model/04_frontmatter_schema.md)
- Example artifacts: [manager](_examples/canonical-manager.md), [internal](_examples/canonical-internal.md), [end user](_examples/end-user-doc.md)
- Filled worked examples (realistic, Helio scenario): [_examples/](_examples/) — see `filled_prd_example.md`, `filled_solution_design_example.md`, `filled_ai_use_policy_example.md`
- Contribution path: [How to contribute](#how-to-contribute)

## What this repository documents

This tree is the canonical documentation system for a project built from this template. It covers the operating model, governance, strategy, product, architecture, AI governance, testing, operations, delivery, references, and end-user documentation.

- Control spine: [README.md](README.md), [Architecture.md](Architecture.md), [00-documentation-standards.md](00-documentation-standards.md), [00-source-of-truth.md](00-source-of-truth.md), and [INDEX.md](INDEX.md)
- Canonical capability areas: [00_operating_model/](00_operating_model/README.md) through [09_user_documentation/](09_user_documentation/README.md)
- Example artifacts: [docs/_examples/ manager/internal/end-user seeds](./_examples/canonical-manager.md)
- Supporting or historical areas: [adr/](adr/README.md), [superpowers/](superpowers/README.md), [99_archive/](99_archive/README.md), and repository-specific archived records in [99_archive/repo/README.md](99_archive/repo/README.md)

## Audience model

The docs system uses explicit audience tags so readers can find the right depth and future export bundles can separate internal, manager, client, end-user, and auditor views.

- Audience rules: [00_operating_model/02_role_audience_map.md](00_operating_model/02_role_audience_map.md)
- Export and sharing profiles: [00_operating_model/05_audience_and_export_profiles.md](00_operating_model/05_audience_and_export_profiles.md)
- Metadata contract: [00_operating_model/04_frontmatter_schema.md](00_operating_model/04_frontmatter_schema.md)

## Reading paths

### By lifecycle phase

- Initiation: [00_governance/00_project_brief_TEMPLATE.md](00_governance/00_project_brief_TEMPLATE.md) -> [00_governance/01_business_case_TEMPLATE.md](00_governance/01_business_case_TEMPLATE.md) -> [01_strategy/01_product_vision_TEMPLATE.md](01_strategy/01_product_vision_TEMPLATE.md)
- Planning: [02_product/01_prd_TEMPLATE.md](02_product/01_prd_TEMPLATE.md) -> [03_architecture/01_solution_design_TEMPLATE.md](03_architecture/01_solution_design_TEMPLATE.md) -> [07_delivery/01_delivery_plan_TEMPLATE.md](07_delivery/01_delivery_plan_TEMPLATE.md)
- Execution: [07_delivery/02_implementation_plan_TEMPLATE.md](07_delivery/02_implementation_plan_TEMPLATE.md) -> [00_governance/07_change_control_log_TEMPLATE.md](00_governance/07_change_control_log_TEMPLATE.md) -> [07_delivery/07_release_plan_TEMPLATE.md](07_delivery/07_release_plan_TEMPLATE.md)
- Monitoring: [07_delivery/04_status_report_TEMPLATE.md](07_delivery/04_status_report_TEMPLATE.md) -> [00_governance/06_raid_register_TEMPLATE.md](00_governance/06_raid_register_TEMPLATE.md) -> [05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)
- Closure: [06_security_operations/08_service_acceptance_TEMPLATE.md](06_security_operations/08_service_acceptance_TEMPLATE.md) -> [06_security_operations/09_post_launch_review_TEMPLATE.md](06_security_operations/09_post_launch_review_TEMPLATE.md) -> [99_archive/README.md](99_archive/README.md)

### By role

- Executive or sponsor: [00_operating_model/02_role_audience_map.md](00_operating_model/02_role_audience_map.md) -> [00_governance/00_project_brief_TEMPLATE.md](00_governance/00_project_brief_TEMPLATE.md) -> [01_strategy/02_roadmap_TEMPLATE.md](01_strategy/02_roadmap_TEMPLATE.md)
- PM or delivery lead: [00_operating_model/01_lifecycle_map.md](00_operating_model/01_lifecycle_map.md) -> [07_delivery/01_delivery_plan_TEMPLATE.md](07_delivery/01_delivery_plan_TEMPLATE.md) -> [07_delivery/04_status_report_TEMPLATE.md](07_delivery/04_status_report_TEMPLATE.md)
- Product: [01_strategy/01_product_vision_TEMPLATE.md](01_strategy/01_product_vision_TEMPLATE.md) -> [02_product/01_prd_TEMPLATE.md](02_product/01_prd_TEMPLATE.md) -> [02_product/05_acceptance_catalog_TEMPLATE.md](02_product/05_acceptance_catalog_TEMPLATE.md)
- Engineer: [03_architecture/01_solution_design_TEMPLATE.md](03_architecture/01_solution_design_TEMPLATE.md) -> [adr/README.md](adr/README.md) -> [07_delivery/02_implementation_plan_TEMPLATE.md](07_delivery/02_implementation_plan_TEMPLATE.md)
- QA or compliance: [05_testing_acceptance/01_test_strategy_TEMPLATE.md](05_testing_acceptance/01_test_strategy_TEMPLATE.md) -> [00_governance/12_requirements_traceability_matrix_TEMPLATE.md](00_governance/12_requirements_traceability_matrix_TEMPLATE.md) -> [08_references/01_standards_register_TEMPLATE.md](08_references/01_standards_register_TEMPLATE.md)
- Ops: [06_security_operations/03_operating_model_TEMPLATE.md](06_security_operations/03_operating_model_TEMPLATE.md) -> [06_security_operations/10_runbook_index_TEMPLATE.md](06_security_operations/10_runbook_index_TEMPLATE.md) -> [07_delivery/08_cutover_and_rollback_TEMPLATE.md](07_delivery/08_cutover_and_rollback_TEMPLATE.md)
- End user doc writer: [09_user_documentation/README.md](09_user_documentation/README.md) -> [09_user_documentation/tutorials/README.md](09_user_documentation/tutorials/README.md) -> [09_user_documentation/CHANGELOG.md](09_user_documentation/CHANGELOG.md)

### By capability

- Operating model: [00_operating_model/README.md](00_operating_model/README.md)
- Governance: [00_governance/README.md](00_governance/README.md)
- Strategy: [01_strategy/README.md](01_strategy/README.md)
- Product: [02_product/README.md](02_product/README.md)
- Architecture: [03_architecture/README.md](03_architecture/README.md) — includes RFC subsystem at [03_architecture/rfcs/README.md](03_architecture/rfcs/README.md)
- AI governance: [04_ai_governance/README.md](04_ai_governance/README.md)
- Testing and acceptance: [05_testing_acceptance/README.md](05_testing_acceptance/README.md)
- Security and operations: [06_security_operations/README.md](06_security_operations/README.md) — includes runbook ([11_runbook_TEMPLATE.md](06_security_operations/11_runbook_TEMPLATE.md)) and postmortem ([12_incident_postmortem_TEMPLATE.md](06_security_operations/12_incident_postmortem_TEMPLATE.md))
- Delivery: [07_delivery/README.md](07_delivery/README.md) — includes migration plan ([09_migration_plan_TEMPLATE.md](07_delivery/09_migration_plan_TEMPLATE.md))
- References: [08_references/README.md](08_references/README.md)
- User documentation: [09_user_documentation/README.md](09_user_documentation/README.md)

Each capability folder README contains a `## Which template should I use?` decision table with tier-based guidance and rules of thumb. Consult the relevant folder README before selecting a template.

For the full cross-cutting map, use [INDEX.md](INDEX.md).

## How to use templates

Use `_TEMPLATE.md` files as starters for project-specific canonical records.

1. Find the owning area in [00-source-of-truth.md](00-source-of-truth.md).
2. Copy the closest matching template, for example [03_architecture/01_solution_design_TEMPLATE.md](03_architecture/01_solution_design_TEMPLATE.md).
3. Rename it to fit the project context.
4. Fill in YAML frontmatter using [00_operating_model/04_frontmatter_schema.md](00_operating_model/04_frontmatter_schema.md).
5. Replace placeholders with project facts, assumptions, and links.

Quick examples:

- New product scope: start from [02_product/01_prd_TEMPLATE.md](02_product/01_prd_TEMPLATE.md)
- New technical baseline: start from [03_architecture/01_solution_design_TEMPLATE.md](03_architecture/01_solution_design_TEMPLATE.md)
- Architecture proposal (pre-decision): start from [03_architecture/rfcs/RFC-000-template.md](03_architecture/rfcs/RFC-000-template.md)
- Durable architecture decision: start from [adr/ADR-000-template.md](adr/ADR-000-template.md)
- New delivery control: start from [07_delivery/04_status_report_TEMPLATE.md](07_delivery/04_status_report_TEMPLATE.md)
- Data or service migration: start from [07_delivery/09_migration_plan_TEMPLATE.md](07_delivery/09_migration_plan_TEMPLATE.md)
- Operations runbook: start from [06_security_operations/11_runbook_TEMPLATE.md](06_security_operations/11_runbook_TEMPLATE.md)
- Incident postmortem: start from [06_security_operations/12_incident_postmortem_TEMPLATE.md](06_security_operations/12_incident_postmortem_TEMPLATE.md)
- New user-facing help page: start from [09_user_documentation/tutorials/01-getting-started-TEMPLATE.md](09_user_documentation/tutorials/01-getting-started-TEMPLATE.md)

Each template contains a `## What not to include` section and a `## Frontmatter quick reference` section — read both before filling.

For realistic filled examples of key templates, see [_examples/](_examples/).

## How CI validates docs

Docs quality is checked by automation so navigation, metadata, and links stay reliable.

- Install docs validator: `python -m pip install -e "./tools/docs_validator[dev]"`
- Docs validator tests: `python -m pytest -q tools/docs_validator/tests`
- Frontmatter and cross-file rules on active canonical docs: `python -m docs_validator.cli docs/README.md docs/Architecture.md docs/00-documentation-standards.md docs/00-source-of-truth.md docs/INDEX.md docs/_examples docs/00_operating_model docs/00_governance docs/01_strategy docs/02_product docs/03_architecture docs/04_ai_governance docs/05_testing_acceptance docs/06_security_operations docs/07_delivery docs/08_references docs/09_user_documentation docs/adr`
- Markdown style: `npx -y markdownlint-cli2`
- Link checking: `lychee --config lychee.toml --offline --no-progress './**/*.md'`
- Workflow entrypoint: [../.github/workflows/docs.yml](../.github/workflows/docs.yml)

## How to contribute

- Read [00-documentation-standards.md](00-documentation-standards.md) before changing structure or ownership.
- Check [00-source-of-truth.md](00-source-of-truth.md) before creating a new canonical file.
- Follow the local rules in [AGENTS.md](AGENTS.md) when editing under `docs/`.
- Update cross-links in [INDEX.md](INDEX.md) and the relevant area `README.md` when you add or move canonical docs.
- Keep [superpowers/](superpowers/README.md) and [99_archive/](99_archive/README.md) non-canonical.

## Related documents

- [Architecture.md](Architecture.md) — structure of the docs system.
- [00-documentation-standards.md](00-documentation-standards.md) — writing and maintenance rules.
- [00-source-of-truth.md](00-source-of-truth.md) — canonical ownership map.
- [INDEX.md](INDEX.md) — comprehensive hand-maintained index.
- [00_operating_model/README.md](00_operating_model/README.md) — operating layer for lifecycle, role, and schema guidance.
- [_examples/canonical-manager.md](_examples/canonical-manager.md) — seeded example artifact for quick frontmatter reference.
