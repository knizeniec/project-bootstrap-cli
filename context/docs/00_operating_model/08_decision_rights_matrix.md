---
title: Decision Rights Matrix
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Decision Rights Matrix

> **Purpose**: provide a reusable RACI/DACI-style template for assigning decision rights across common project decision classes.
> **Audience**: internal teams and managers defining who decides, who contributes, and who must be informed.
> **When to update**: update when the default decision classes, role model, or instantiation guidance changes.

## How to use this template

Use this page to define who is accountable before a project enters active delivery. Pick either a RACI or DACI interpretation and keep that choice consistent within the project. Record concrete names or team labels in project-specific copies rather than relying on generic roles during live decisions.

- Start with the roles in [02_role_audience_map.md](02_role_audience_map.md).
- Align decision timing with [01_lifecycle_map.md](01_lifecycle_map.md).
- Keep metadata rules aligned with [04_frontmatter_schema.md](04_frontmatter_schema.md).

## RACI/DACI matrix template

The matrix should be easy to scan and limited to real decision points that recur during the lifecycle. Rows represent decision classes and columns represent roles. Add notes only when a project's reality materially differs from the default operating model.

| Decision class | Executive | PM | Product | Engineer | QA | Ops | Compliance | Client/Sponsor |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Funding | A | R | C | I | I | I | C | C |
| Scope | C | R | A | C | C | I | I | C |
| Design | I | C | C | A | C | C | I | I |
| Release | I | R | C | C | C | A | C | I |
| Operations | I | C | I | C | I | A | C | I |

- Example interpretation: `A` = accountable approver, `R` = responsible driver.
- Example interpretation: `C` = consulted before decision, `I` = informed after decision.
- Example adaptation: swap to DACI labels if the organization already uses that model.

## Decision classes

Decision classes should reflect durable categories that appear on most projects. Keep them broad enough to be reusable but specific enough that roles can be assigned without debate every week. Add project-local rows only when the baseline set misses a genuinely important class.

- Funding: spend, staffing, and material commercial commitments.
- Scope: requirements inclusion, change control, and acceptance boundaries.
- Design: architecture, interfaces, security posture, and implementation approach.
- Release: go-live readiness, rollout timing, rollback criteria, and comms readiness.
- Ops: support model, incident response ownership, and live-service guardrails.

## Role columns

Use role columns that match the common operating model, then replace them with named owners in project-specific instances. Avoid creating one-off columns for every individual contributor because the matrix becomes unreadable quickly. If a role is absent on a project, record `N/A` or merge it intentionally into another accountable role.

- Core columns: Executive, PM, Product, Engineer, QA, Ops, Compliance, Client/Sponsor.
- Optional local column: End User representative when user sign-off is formalized.
- Optional local note: vendor or partner role when external delivery responsibility is material.

## How to instantiate per project

Instantiate the matrix by copying the template into the project's governance area and replacing generic roles with actual accountable positions or team names. Keep the first version lightweight, then refine only where ambiguity causes real delays or risk. Review the matrix whenever the tailoring profile or governance structure changes.

- Example step: copy the matrix into a project governance baseline or PID.
- Example step: add named approvers for funding, scope, release, and exception handling.
- Example step: link the instantiated matrix from the project README and approval records.

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [02_role_audience_map.md](02_role_audience_map.md)
- [07_tailoring_matrix.md](07_tailoring_matrix.md)
