---
title: Source-of-Truth Map
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Source-of-Truth Map

> **Purpose**: define where each class of artifact is authoritative, how mirrors behave, and how tool boundaries are declared.
> **Audience**: internal teams and managers deciding where records live and how cross-tool copies should be handled.
> **When to update**: update when authoritative systems, mirror policy, or cross-tool documentation rules change.

## How to use this template

Use this page before deciding whether a document belongs in the repo, in an external system, or as a documented mirror. Keep the authoritative location singular and make mirrors explicit in metadata and prose. If a team wants to split authority across tools, stop and resolve that design first.

- Use this page before importing status or delivery records from another system.
- Use this page before adding `source_of_truth` metadata to a canonical file.
- Cross-check metadata syntax in [04_frontmatter_schema.md](04_frontmatter_schema.md).

## Per-artifact authoritative location

Each artifact class should have one declared home where updates are first made and approvals are interpreted. Repo-native canonical docs are preferred when the content must remain durable, reviewable, and versioned with adjacent records. External systems are acceptable when the operating model needs live workflow data that the repo should only summarize or mirror.

- Project framing: repo canonical docs under `docs/00_governance/`.
- Design decisions: repo canonical docs and ADRs under `docs/03_architecture/` and `docs/adr/`.
- Live work tracking: external tool if needed, with repo summaries only where durable context is required.
- User-facing help: repo docs or product-owned publishing channel, depending on delivery model.

## Mirror policy

Mirrors should exist only to improve discoverability, export, or auditability. A mirror must point back to the authoritative system and must not silently diverge into a second active record. If a mirror starts carrying decisions that do not exist in the source system, it is no longer a mirror and should be reclassified.

- Example policy: keep a client-safe status summary in the repo that mirrors an approved portfolio report.
- Example policy: keep a release checklist in the repo, but link back to the ticketing system for live execution state.
- Example anti-pattern: editing a mirrored roadmap in Markdown while the product tool remains the claimed source.

## Tool-boundary rules

Tool boundaries should be easy to explain in one sentence: what lives here, what lives elsewhere, and why. Put stable narrative, decisions, and cross-functional context in the repo; keep high-churn workflow state in tools built for workflow. When in doubt, choose the location that best preserves reviewable history and clear ownership.

- Repo boundary: canonical guidance, templates, architecture, approval rationale.
- Workflow tool boundary: task state, sprint boards, live issue triage, queue management.
- Evidence boundary: external dashboards or reports may support claims, but the canonical doc should link the specific evidence used.

## How to declare `source_of_truth: mirror_of:<system>`

When a Markdown file mirrors another system, declare that explicitly in frontmatter and explain the mirror intent near the top of the body. Use a stable system label rather than a personal shorthand so automation and reviewers can interpret it consistently. Keep the declaration specific enough to show the boundary without duplicating the entire external workflow model.

- Example frontmatter value: `source_of_truth: mirror_of:jira`
- Example frontmatter value: `source_of_truth: mirror_of:sharepoint`
- Example body note: "This document mirrors the approved client report in Jira and is kept for repo-side context and export."

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md)
- [../00-source-of-truth.md](../00-source-of-truth.md)
