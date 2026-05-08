---
title: RFC Index Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# RFC Index Template

> **Purpose**: track architecture RFCs that are under discussion before they become accepted ADRs or are closed.
> **Audience**: architects, engineers, reviewers, and decision-makers participating in technical review.
> **When to update**: update when RFCs are opened, reviewed, accepted, rejected, or superseded.

## How to use this template

- Keep this as the quick navigation layer for `rfcs/`.
- Link accepted durable outcomes to the matching ADR when one is created.
- Keep lifecycle wording consistent with the RFC README.

## What not to include

- **Full RFC content** — the complete RFC lives in the `rfcs/` directory. This index is a navigation layer only.
- **Accepted binding decisions** — once an RFC graduates, the binding decision belongs in an ADR ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Update this index to link the ADR; do not merge ADR content into the RFC index.
- **Architecture design or implementation detail** — design belongs in the RFC body and ultimately the solution design ([01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md)).
- **Delivery status or implementation tracking** — progress tracking belongs in the delivery plan and status reports.
- **Design review meeting notes** — meeting notes belong in separate documents; this index records lifecycle state, not review history.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## RFC list

| RFC | Title | Status | Date | Outcome link |
| --- | --- | --- | --- | --- |
| RFC-001 | Example proposal | proposed | YYYY-MM-DD | ADR-001 or closure note |

## Graduation rules

- Use RFCs for open design exploration and multi-party review.
- Move durable, binding implementation decisions into `../adr/`.

## Related documents

- [rfcs/README.md](rfcs/README.md) — RFC lifecycle and review expectations.
- [rfcs/RFC-000-template.md](rfcs/RFC-000-template.md) — default RFC authoring template.
- [11_adr_index_TEMPLATE.md](11_adr_index_TEMPLATE.md) — where accepted outcomes are mirrored into the architecture view.
- [../adr/ADR-000-template.md](../adr/ADR-000-template.md) — target format for durable decisions.
