---
name: Documentation Rules
description: Use for work under docs/. Covers canonical doc ownership, routing, and documentation workflow guidance.
applyTo: "docs/**"
---

# Documentation Rules

> **Purpose**: route work under `docs/` and keep the documentation system coherent.
> **Audience**: agents and contributors editing documentation in this repository.
> **When to update**: update when doc workflow, folder routing, or information-architecture expectations change.

Apply these rules when editing anything under `docs/`.

## Priorities

- Keep the canonical source of truth in one place and link instead of duplicating.
- Keep docs concise, actionable, and easy to scan.
- Verify doc claims against code or commands before writing them.

## Update rules

- Update docs in the same task when behavior, workflows, architecture, or operations change.
- If a file does not need updates, state why in the final response.
- Keep operational runbooks, architecture decisions, and acceptance criteria easy to discover.

## Canonical doc creation

- When a user asks to create a canonical document, first identify the requested doc type and owning area by checking [00-source-of-truth.md](00-source-of-truth.md), [INDEX.md](INDEX.md), the relevant numbered-area `README.md`, and available `*_TEMPLATE.md` files.
- If the user already named the canonical doc type, do not re-decide it. Map that request to the correct canonical area and template.
- Before naming the new file, ask only the minimum kickoff questions needed to create a usable first draft: what the document is about, what scope it covers, and what context-specific title or subject it should use.
- If a matching template exists, copy it into the owning canonical area and rename it to fit the user context.
- If no matching template exists and the document type is reusable, create a new `*_TEMPLATE.md` file in the correct numbered area first, following neighboring template patterns, then create the requested document from that template.
- Keep [docs/adr/](adr/README.md) as the only exception to root numbered-area ownership. New ADRs should start from `docs/adr/ADR-000-template.md` and follow [docs/adr/README.md](adr/README.md).
- After creating the file, continue consulting the user to fill it section by section. Aim for a first useful version, not a perfect final version.
- Ask focused follow-up questions that replace the most important placeholders first: summary, scope, goals or decision, constraints or drivers, dependencies or risks, approvals or owners, and related documents.
- Accept partial answers. When information is still unknown, record it explicitly as an assumption, open question, or follow-up item instead of blocking the draft.
- When adding a new reusable canonical template, update the owning area landing page plus [README.md](README.md), [INDEX.md](INDEX.md), and [00-source-of-truth.md](00-source-of-truth.md) in the same change.

## Template routing by artifact type

Use these rules to route to the right template or area quickly.

**Architecture proposals and decisions:**
- RFC (proposal not yet decided, needs structured team review): [03_architecture/rfcs/RFC-000-template.md](03_architecture/rfcs/RFC-000-template.md); manage via [03_architecture/rfcs/README.md](03_architecture/rfcs/README.md).
- ADR (decision made, durable): [adr/ADR-000-template.md](adr/ADR-000-template.md); manage via [adr/README.md](adr/README.md).
- Nothing (single-component change, no shared interface): proceed with implementation and reference any existing ADR in commit messages.
- Relationship: RFCs are proposals pre-decision; ADRs are binding records post-adoption. Graduate an RFC to an ADR when the outcome becomes durable implementation direction.

**Operations:**
- Runbook (per-component or per-operation procedure): [06_security_operations/11_runbook_TEMPLATE.md](06_security_operations/11_runbook_TEMPLATE.md); register in [06_security_operations/10_runbook_index_TEMPLATE.md](06_security_operations/10_runbook_index_TEMPLATE.md).
- Incident postmortem (structured learning after a significant incident): [06_security_operations/12_incident_postmortem_TEMPLATE.md](06_security_operations/12_incident_postmortem_TEMPLATE.md).

**Delivery:**
- Migration plan (data or service migration, not greenfield): [07_delivery/09_migration_plan_TEMPLATE.md](07_delivery/09_migration_plan_TEMPLATE.md).

**Worked examples:**
- See [_examples/](../_examples/) for filled, realistic examples using the "Helio" fictional scenario: [filled_prd_example.md](_examples/filled_prd_example.md), [filled_solution_design_example.md](_examples/filled_solution_design_example.md), [filled_ai_use_policy_example.md](_examples/filled_ai_use_policy_example.md).

## Template selection mechanism

Each capability folder `README.md` contains a `## Which template should I use?` decision table with three tiers (e.g. small / cross-component / new system; single-stage / multi-stage / regulated) and rules of thumb. Consult the owning folder README before selecting a template.

## Template structure conventions

Every canonical `*_TEMPLATE.md` file contains two standard sections that every agent should read before filling:
- `## What not to include` — lists content that belongs in a different artifact or a different document altogether.
- `## Frontmatter quick reference` — shows the minimum valid frontmatter for that template type with the expected `capability`, `record_class`, and `audience` values.

## Information architecture

- Organize docs by reader need: tutorial, how-to, reference, explanation.
- Prefer links to source files, ADRs, and canonical docs over repeated prose.
- Keep docs consistent with actual behavior and constraints.
- Treat root `docs/` as the canonical source-of-truth tree.
- Treat [docs/adr/](adr/README.md) as ADR-only support space.
- Treat [docs/00_operating_model/](00_operating_model/README.md) as the canonical operating layer for lifecycle, role, schema, naming, and tailoring guidance.
- Treat [docs/09_user_documentation/](09_user_documentation/README.md) as the end-user documentation area organized by Diátaxis.

## Local routing

- Read [docs/adr/AGENTS.md](adr/AGENTS.md) before editing `docs/adr/**`.
- Read [docs/superpowers/AGENTS.md](superpowers/AGENTS.md) before editing `docs/superpowers/**`.
- For `docs/03_architecture/rfcs/**`, follow [03_architecture/rfcs/README.md](03_architecture/rfcs/README.md).
- For `docs/00_operating_model/**`, follow this file plus that folder's [README.md](00_operating_model/README.md).
- For `docs/09_user_documentation/**`, follow this file plus that folder's [README.md](09_user_documentation/README.md).

## Related documents

- [README.md](README.md) — root docs entry point and contributor path.
- [INDEX.md](INDEX.md) — hand-maintained navigation map across lifecycle, roles, and capabilities.
- [00-source-of-truth.md](00-source-of-truth.md) — maps each concern to one canonical owner.
- [00_operating_model/README.md](00_operating_model/README.md) — operating-layer guidance for lifecycle and metadata.
- [09_user_documentation/README.md](09_user_documentation/README.md) — user-docs area and Diátaxis entry point.
