---
title: RFC-000 Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# RFC-NNN: <Title>

> **Purpose:** provide the standard template for a Request for Comments (RFC) — a structured design proposal that invites team review before any binding decision is recorded as an ADR.
> **Audience:** the RFC author, architecture reviewers, and decision-makers evaluating a proposed technical change.
> **When to update:** update this template when RFC authoring expectations, review criteria, or the graduation path to ADR changes.

## How to use this template

Copy this file and rename it `RFC-NNN-short-title.md` where `NNN` is the next available number in the index. Place it in `docs/03_architecture/rfcs/`. Fill every section before marking status `proposed`.

- Mandatory: Title, Status, Context and motivation, Proposal, Alternatives considered (at least two), Drawbacks, Unresolved questions, and Security and privacy implications.
- Optional: Prior art, Adoption plan. Leave sections blank rather than deleting them so reviewers know they were considered.
- Once the RFC is accepted and a binding decision is recorded, create an ADR using [../../adr/ADR-000-template.md](../../adr/ADR-000-template.md) and link it in the Outcome section here.
- Remove this "How to use" block before publishing the RFC for review.

## What not to include

- **Status updates or progress reports** — these belong in the status report template (`docs/07_delivery/04_status_report_TEMPLATE.md`). An RFC is a decision artifact, not a progress tracker.
- **Implementation tickets or task lists** — capture work breakdown in the backlog or implementation plan. The RFC records the reasoning, not the to-do list.
- **Final binding decisions** — once the team accepts the proposal, the durable decision lives in an ADR. The RFC graduates; it does not absorb ADR content. Mark it `accepted` and link the ADR.
- **Operational runbook content** — step-by-step operating procedures belong in runbooks (`docs/06_security_operations/11_runbook_TEMPLATE.md`).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for the rfcs folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `ad-hoc` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

---

## Title

State the proposal in one short, declarative sentence. Aim for something a reviewer can scan in a list and immediately understand. Avoid jargon that hasn't been defined yet.

- Good: "Replace synchronous inter-service HTTP calls with an async event bus."
- Good: "Adopt Postgres as the canonical data store for the accounts domain."
- Avoid: "Investigate options" or "Improve the system" — these are too vague to review.

## Status

Record the current RFC lifecycle state explicitly in this section. Update it as the RFC progresses.

Allowed values and what they mean:
- **Draft** — the author is still shaping the proposal; not ready for formal review.
- **Proposed** — the RFC is ready for structured review; the review window is open.
- **Accepted** — the review closed with approval; see Outcome section for the resulting ADR link.
- **Rejected** — the review closed without approval; see Outcome section for the rationale.
- **Superseded** — replaced by a later RFC or ADR; see Outcome section for the successor reference.

Current status: **Draft**

Review window: YYYY-MM-DD to YYYY-MM-DD
Decision owner: [role]
Reviewers: [roles or teams]

## Context and motivation

Describe the problem, constraints, and forces that make this proposal necessary right now. A reviewer unfamiliar with the background should be able to understand why this RFC exists without reading a separate document.

Write 2–4 paragraphs covering: what is happening today, what is going wrong or about to change, what constraints bound the solution space, and why the team needs a structured decision rather than an ad-hoc one.

- Example: "Today all three services call the accounts API synchronously. Under load, a slowdown in accounts creates cascading latency across the whole system. We need a solution before the Q3 traffic increase."
- Example: "A new regulatory requirement mandates data residency in the EU. Our current multi-region active-active setup does not meet this requirement and must change before the compliance deadline."

## Proposal

State the proposed approach in clear, testable terms. Explain what changes, what it replaces, and what it leaves unchanged. A reader should be able to tell whether the proposal was implemented by reading the system's observable behavior.

Include: the mechanism or design choice, the scope of impact (which components, teams, or interfaces change), key assumptions the proposal depends on, and any phasing or sequencing intended.

- Example: "Introduce an event bus (Kafka) between the order and fulfilment services. The order service publishes `OrderPlaced` events; fulfilment subscribes. No changes to the customer-facing API."
- Example: "Adopt Postgres 16 as the accounts-domain data store. Migrate from the existing DynamoDB table using a dual-write phase over two sprints, validated by reconciliation checksums."

## Alternatives considered

Present at least two realistic alternatives to the proposed approach. For each, explain what it is, its main advantages, and why the team did not choose it. Reviewers use this section to check whether the preferred option was chosen for sound reasons.

Do not write this as a pure advocacy section for the proposal — engage honestly with the trade-offs. A reviewer who prefers an alternative should find their option documented here.

### Alternative A: [Name]

**What it is:** [Short description.]

**Pros:**
- [Benefit 1]
- [Benefit 2]

**Cons:**
- [Drawback 1]
- [Drawback 2]

**Why not chosen:** [1–2 sentences.]

### Alternative B: [Name]

**What it is:** [Short description.]

**Pros:**
- [Benefit 1]
- [Benefit 2]

**Cons:**
- [Drawback 1]
- [Drawback 2]

**Why not chosen:** [1–2 sentences.]

## Drawbacks

Call out the costs, risks, complexity, or lock-in that the *chosen proposal* introduces. Be honest about what the team is accepting by going this route. Reviewers need to weigh these drawbacks against the expected benefits.

Include: migration or transition effort, new operational burden, vendor or technology lock-in, areas where the proposal is still uncertain, and risks that remain even after a successful implementation.

- Example: "Introducing an event bus adds a new infrastructure dependency. The team will need on-call expertise for Kafka operations that does not exist today."
- Example: "The dual-write migration phase will run for two sprints, during which both read paths must be actively maintained and monitored."

## Unresolved questions

List the open issues that reviewers must help resolve before the RFC can move to `accepted`. Each question should be specific enough that it has a clear answer once resolved.

This section is the primary call to action for reviewers. Keep questions sharp rather than rhetorical.

- Example: "What SLA should the event bus provide? Is at-least-once delivery sufficient, or do we need exactly-once semantics?"
- Example: "Which team owns the Kafka cluster — platform, data, or the accounts domain? This affects the support model."
- Example: "Is there a compliance requirement that prevents storing account events in the shared bus?"

## Security and privacy implications

Describe any new trust boundaries, data sensitivity concerns, access control requirements, or audit obligations introduced by the proposal. Security implications should be reviewed explicitly, not treated as an afterthought.

Cover: new data flows and who can read or write them, changes to authentication or authorization, audit-log requirements, and any regulatory or policy implications.

- Example: "Events on the shared bus will contain account identifiers. Access to the topic must be scoped by service identity using mTLS. See threat model for the updated trust boundary diagram."
- Example: "No new PII is introduced by this proposal. Existing access controls on the accounts API are unchanged."

## Prior art and references

Reference comparable systems, prior internal patterns, external guidance, benchmarks, or prior experiments that informed the proposal. This helps reviewers trust that the proposal is grounded in evidence.

- Example: "The platform team adopted a similar event bus for the payments domain in 2024. See ADR-012 for the decision context."
- Example: "Martin Fowler's LMAX Disruptor paper (link) demonstrates the latency characteristics we modelled our throughput estimates against."

## Adoption plan

Describe how the change will be introduced — phasing, feature flags, migration steps, team enablement, and how existing consumers will be supported during transition. This section is optional at draft stage but required before the RFC moves to `proposed`.

- Example: "Phase 1: introduce the event bus alongside the existing HTTP call (dual-emit). Phase 2: remove the HTTP call after two sprints of parallel validation."
- Example: "A one-page guide and a team demo session will be scheduled before the first consumer onboards."

## Outcome

Fill this section when the RFC closes (status becomes `accepted`, `rejected`, or `superseded`). Leave it blank while the RFC is `draft` or `proposed`.

- **Decision date:** YYYY-MM-DD
- **Outcome:** [Accepted / Rejected / Superseded by RFC-NNN]
- **Rationale summary:** [1–3 sentences on the deciding factors.]
- **Resulting ADR:** [ADR-NNN: Title](../../adr/ADR-NNN-title.md) *(if accepted and a durable decision was recorded)*
- **Follow-up actions:** [Linked tickets or next steps, if any.]

## Related documents

- [README.md](README.md) — RFC process: when to open one, lifecycle states, and graduation rules.
- [../../adr/ADR-000-template.md](../../adr/ADR-000-template.md) — the target format when this RFC's accepted outcome becomes a durable decision.
- [../01_solution_design_TEMPLATE.md](../01_solution_design_TEMPLATE.md) — the architecture baseline that accepted RFCs typically update or extend.
- [../../adr/README.md](../../adr/README.md) — ADR process, authority rules, and index.
