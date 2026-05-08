---
title: Tailoring Matrix
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Tailoring Matrix

> **Purpose**: define the three tailoring profiles, the expected artifact set for each, and how a project declares its chosen profile.
> **Audience**: internal teams and managers selecting the right documentation depth for a project.
> **When to update**: update when tailoring profiles, selection criteria, or required artifact sets change.

## How to use this template

Use this matrix when choosing how much documentation a project needs. Start with risk, external scrutiny, and delivery complexity, then choose the lightest profile that still preserves control and clarity. Record the chosen profile in the project README or equivalent operating baseline.

- Begin with [01_lifecycle_map.md](01_lifecycle_map.md) to understand phase expectations.
- Use [04_frontmatter_schema.md](04_frontmatter_schema.md) to keep chosen artifacts metadata-complete.
- Revisit the profile if scope, assurance needs, or release risk changes materially.

## Three profiles

The operating model provides `minimum`, `standard`, and `high-assurance` profiles. Profiles control expected evidence depth, not the meaning of the lifecycle phases themselves. Choose one default profile per project so readers do not need to infer the burden from scattered clues.

- `minimum`: for low-risk, fast-moving internal work.
- `standard`: for most planned internal delivery with moderate coordination needs.
- `high-assurance`: for regulated, client-exposed, safety-sensitive, or heavily audited work.

## Per-profile canonical artifact list

Each profile should name the smallest coherent set of canonical artifacts needed to manage the work responsibly. Projects may add more artifacts, but should not drop the baseline set without recording why. Keep the list stable enough to use in kickoffs and audits.

- `minimum`: project brief, core requirements or scope note, implementation plan, release/handoff record.
- `standard`: brief, requirements, solution design, ADRs as needed, delivery plan, implementation plan, RAID/change records, readiness and release records.
- `high-assurance`: all standard artifacts plus stronger traceability, decision rights, audit evidence, stage gates, and explicit approvals.

## Criteria for selection

Profile choice should be driven by consequence and coordination, not by personal comfort with process. Consider how much harm a bad outcome could cause, how many teams are involved, and how much external assurance is required. If two profiles seem plausible, start with the lighter one and document the trigger for moving up.

- Consider risk: user harm, service impact, cost exposure, or compliance impact.
- Consider complexity: number of teams, interfaces, vendors, or rollout stages.
- Consider assurance: audit need, client commitments, or board-level visibility.

## How to declare a project's profile in its README

The project README or equivalent landing page should state the active tailoring profile in plain language and point to any local deviations. Keep the declaration near the top so reviewers know what evidence standard to expect. If the profile changes, note the date and reason.

- Example line: `Tailoring profile: standard`
- Example line: `Tailoring profile: high-assurance (client-regulated rollout)`
- Example note: `Deviation: no separate roadmap; delivery milestones are owned in the implementation plan.`

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [08_decision_rights_matrix.md](08_decision_rights_matrix.md)
- [README.md](README.md)
