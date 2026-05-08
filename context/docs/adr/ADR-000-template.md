---
title: ADR-000 Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# ADR-000: [Short decision title]

- Status: [Proposed | Accepted | Superseded | Archived]
- Date: YYYY-MM-DD
- Deciders: [Role or names with approval authority]
- Consulted: [Roles or teams consulted before the decision]
- Informed: [Roles or teams that need visibility]
- Tags: [Architecture | Security | Data | Delivery | Operations | Product]
- Supersedes: [ADR reference or `None`]
- Superseded by: [ADR reference or `None`]
- Related documents: [Solution design, RFC, issue, benchmark, incident, or `None`]

## What not to include

- **Status updates or progress reports** — use the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)) for ongoing delivery progress. An ADR records a decision, not how the work is going.
- **Multiple unrelated decisions in one ADR** — keep one ADR per decision. If an architectural change requires separate decisions on data storage, authentication, and deployment, create separate ADRs for each.
- **Proposals still under active debate** — use an RFC ([../03_architecture/rfcs/RFC-000-template.md](../03_architecture/rfcs/RFC-000-template.md)) while the proposal is open for comment. Graduate to an ADR once the team has reached a binding conclusion.
- **Implementation tasks or backlog items** — track implementation work in the backlog and delivery tooling, not in the ADR. The ADR records the decision; the backlog records the work that follows.
- **Detailed test or verification evidence** — evidence that a decision was implemented correctly belongs in the verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for the ADR folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Context and problem statement

Describe the problem, constraints, assumptions, and forces that make this decision necessary.

## Decision drivers

- [Driver 1]
- [Driver 2]
- [Driver 3]

## Considered options

### Option 1: [Name]

- What it is: [Short description]
- Pros: [Benefits]
- Cons: [Drawbacks]

### Option 2: [Name]

- What it is: [Short description]
- Pros: [Benefits]
- Cons: [Drawbacks]

## Decision outcome

State the selected option in clear, testable terms.

### Consequences

- [Expected benefit]
- [Trade-off or cost]
- [Risk, limit, or follow-up action]

## Pros and cons of the options

Summarize the trade-off comparison that mattered most.

- Chosen option: [Why it won]
- Rejected option: [Why it lost]

## More information

- Link to [../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md) when this decision shapes the main design.
- Link to [../03_architecture/rfcs/RFC-000-template.md](../03_architecture/rfcs/RFC-000-template.md) when the ADR formalizes an RFC outcome.
- Link implementation, verification, benchmark, incident, or migration evidence here.
