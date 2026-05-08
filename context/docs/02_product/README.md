---
title: Product Templates
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Product Templates

> **Purpose:** orient readers to the canonical product-planning templates for scope, requirements, journeys, backlog policy, and acceptance.
> **Audience:** product, delivery, and assurance roles who need a shared product baseline before design, build, and test work starts.
> **When to update:** update when this folder gains new product artifacts, changes reading order, or changes cross-links to governance, testing, or delivery records.

## How to use this template

Use this landing page to pick the smallest product artifact that answers the current planning question. Start with the PRD, then add catalogs and policies only where they improve traceability, handoff quality, or acceptance clarity.

- Use `01_prd_TEMPLATE.md` to define the problem, users, scope, and release intent.
- Add the requirements, journeys, backlog, and acceptance templates as the product surface becomes more detailed.
- Link to testing and governance artifacts instead of repeating approval, traceability, or evidence detail here.

## Folder purpose

This folder holds the canonical product-planning layer between strategy and implementation. It turns strategic intent into concrete requirements, user outcomes, backlog rules, and acceptance conditions that engineering and QA can verify.

- Typical use: define a feature or product increment before architecture and delivery planning deepens.
- Typical use: give QA and stakeholders a stable link between requirements and acceptance expectations.

## Index of templates

The templates are ordered from broad framing to more operational planning aids. Readers can stop once they have enough structure for the project risk and complexity level.

- [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md) — product problem, users, goals, requirements, and release intent.
- [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) — structured requirement inventory with source, owner, and acceptance links.
- [03_user_journeys_TEMPLATE.md](03_user_journeys_TEMPLATE.md) — persona-centered journeys, pain points, and success measures.
- [04_backlog_policy_TEMPLATE.md](04_backlog_policy_TEMPLATE.md) — intake, prioritization, and tool-boundary rules for backlog management.
- [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) — feature-level Given/When/Then scenarios, UAT pass criteria, and sign-off roles.

## Reading order

Read the PRD first so the more detailed catalogs inherit a clear product narrative. Then move from requirements and journeys into backlog policy and acceptance, using testing and governance docs for verification and traceability.

1. [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md)
2. [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) and [03_user_journeys_TEMPLATE.md](03_user_journeys_TEMPLATE.md)
3. [04_backlog_policy_TEMPLATE.md](04_backlog_policy_TEMPLATE.md)
4. [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md)
5. [../05_testing_acceptance/01_test_strategy_TEMPLATE.md](../05_testing_acceptance/01_test_strategy_TEMPLATE.md) and [../00_governance/12_requirements_traceability_matrix_TEMPLATE.md](../00_governance/12_requirements_traceability_matrix_TEMPLATE.md)

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Requirement count tier | Recommended templates | Skip |
|---|---|---|
| **Tier 1** — ≤10 requirements, single team, no external compliance, straightforward delivery | [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md) — problem, users, goals, requirements, and release intent in one document | [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md), [03_user_journeys_TEMPLATE.md](03_user_journeys_TEMPLATE.md), [04_backlog_policy_TEMPLATE.md](04_backlog_policy_TEMPLATE.md), [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) |
| **Tier 2** — 11–50 requirements, multiple contributors or squads, customer-facing or handoff-dependent delivery | [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md); [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) — structured inventory when requirements exceed the PRD scope; [03_user_journeys_TEMPLATE.md](03_user_journeys_TEMPLATE.md) — when UX or product quality depends on journey clarity; [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) — when QA or UAT needs stable Given/When/Then scenarios | [04_backlog_policy_TEMPLATE.md](04_backlog_policy_TEMPLATE.md) — add only when backlog intake or prioritisation disputes arise |
| **Tier 3** — >50 requirements, or any compliance obligation (regulatory, contractual, or audit trail required) | Full set: all tier-2 templates plus [04_backlog_policy_TEMPLATE.md](04_backlog_policy_TEMPLATE.md) — explicit intake and prioritisation rules for high-volume backlogs; link [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) to [../00_governance/12_requirements_traceability_matrix_TEMPLATE.md](../00_governance/12_requirements_traceability_matrix_TEMPLATE.md) for audit traceability | Nothing — all documents apply |

**Rules of thumb:**
- Promote requirements from the PRD to the requirements catalog when a single requirement needs its own owner, source link, or acceptance reference that would clutter the PRD.
- Add the acceptance catalog when QA and stakeholders need pre-agreed Given/When/Then scenarios before sprint work begins, not after.
- Add the backlog policy when intake volume, prioritisation disputes, or tool-boundary ambiguity cause delivery friction — not as a default on every project.

## Related documents

- [../01_strategy/README.md](../01_strategy/README.md) — strategy artifacts explain why the work matters and what horizon it supports.
- [../05_testing_acceptance/README.md](../05_testing_acceptance/README.md) — testing artifacts define how product requirements and acceptance will be verified.
- [../00_governance/README.md](../00_governance/README.md) — governance artifacts provide approvals, traceability, and control context for product scope.
