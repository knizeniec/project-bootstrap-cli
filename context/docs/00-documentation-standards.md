---
title: Documentation Standards
status: active
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Documentation Standards

> **Purpose**: define writing, ownership, and maintenance rules for the active docs system.
> **Audience**: documentation maintainers, contributors, and managers reviewing doc quality.
> **When to update**: update when the canonical docs model, metadata rules, or maintenance workflow changes.

## Scope

These standards apply to the control spine and the active numbered tree under `docs/`.

- Active canonical docs: [README.md](README.md), [Architecture.md](Architecture.md), [00-documentation-standards.md](00-documentation-standards.md), [00-source-of-truth.md](00-source-of-truth.md), [INDEX.md](INDEX.md), [00_operating_model/](00_operating_model/README.md), and [00_governance/](00_governance/README.md) through [09_user_documentation/](09_user_documentation/README.md)
- Durable decisions: [adr/](adr/README.md)
- Working history only: [superpowers/](superpowers/README.md)
- Evidence only: [99_archive/](99_archive/README.md)

## Core rules

- Keep one active documentation system only. Current guidance belongs in the control spine or the numbered tree.
- Keep one canonical owner per concern. Use [00-source-of-truth.md](00-source-of-truth.md) to name that owner.
- Keep documents single-purpose. A document should mainly explain, instruct, reference, or record a decision.
- Prefer short headings, short paragraphs, small tables, and links over duplicated prose.
- State assumptions, unknowns, and approval conditions explicitly.
- Archive material can support a claim but cannot become the current rule by implication.
- [superpowers/](superpowers/README.md) may describe how work was done, but it cannot define the active product, architecture, governance, delivery, operations, or user-documentation baseline.

## Structure rules

- Create new canonical content inside the numbered tree.
- Keep cross-cutting rules in the control spine instead of repeating them across area folders.
- Use [00_operating_model/](00_operating_model/README.md) for lifecycle, audience, schema, naming, and tailoring rules.
- Use folder landing pages for area-level orientation and keep detailed navigation in [INDEX.md](INDEX.md).
- Store durable implementation decisions in [adr/](adr/README.md).
- Store proposal-stage design discussion in [03_architecture/rfcs/](03_architecture/rfcs/README.md); graduate accepted proposals to ADRs.
- Store external standards, vendor references, and framework citations in [08_references/](08_references/README.md).
- Use named `_TEMPLATE.md` files in canonical areas for reusable bootstrap starters.

## Template conventions

Every canonical `*_TEMPLATE.md` file must carry two standard sections:

- `## What not to include` — lists content that belongs in a different artifact, preventing scope creep and cross-contamination between templates.
- `## Frontmatter quick reference` — shows the minimum valid frontmatter block for that template with the expected `capability`, `record_class`, and `audience` values.

When creating a new template, include both sections. When reviewing an existing template, verify both are present before approving.

## Folder README conventions

Every capability folder `README.md` must carry a `## Which template should I use?` decision table with at least three tiers (for example: small / cross-component / new system; single-stage / multi-stage / regulated; pre-launch / live / regulated). Each tier row should name recommended templates with links, templates to skip, and explicit rules of thumb below the table. This table is the primary template-selection mechanism; contributors should be directed to read it before copying any template.

## Worked-examples convention

Filled, realistic examples that illustrate complete frontmatter and section content for key templates are stored in [_examples/](../_examples/). Current filled examples follow the "Helio" fictional scenario and cover the PRD, solution design, and AI use policy. Use these as known-good references, not as project documents. When a new high-traffic template is added, consider adding a corresponding filled example.

## Required metadata

Maintained Markdown documents should start with YAML frontmatter validated by the docs schema.

```yaml
---
title: "Document title"
status: active
record_class: canonical
audience: [internal]
owner: "Role or team"
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---
```

- Use only values allowed by [00_operating_model/04_frontmatter_schema.md](00_operating_model/04_frontmatter_schema.md).
- Active documents must not retain starter placeholders in required fields.
- Add `superseded_by` only when `status: superseded`.
- Add `source_of_truth` when the schema requires it, especially for `capability: execution` records.
- Current valid `capability` values include `strategy` and `ai_governance` in addition to the base set; see the schema doc for the full enum.

## Update checklist

1. Update the canonical owner first.
2. Update [README.md](README.md), [Architecture.md](Architecture.md), [00-source-of-truth.md](00-source-of-truth.md), and [INDEX.md](INDEX.md) when the structure or ownership map changes.
3. Update [00_operating_model/](00_operating_model/README.md) when lifecycle, audience, schema, naming, or tailoring rules change.
4. Update [adr/](adr/README.md) when a durable implementation decision changes.
5. Keep archive and working-history readmes aligned when their handling rules change.
6. Run link and schema validation when links or metadata change.

## External basis

Reference standards, governance frameworks, and style guides from [08_references/INDEX.md](08_references/INDEX.md).

## Related documents

- [README.md](README.md) — root entry point for contributors using the docs system.
- [Architecture.md](Architecture.md) — describes the structure and role of each documentation surface.
- [00-source-of-truth.md](00-source-of-truth.md) — maps each concern to one active owner.
- [00_operating_model/04_frontmatter_schema.md](00_operating_model/04_frontmatter_schema.md) — defines the validated metadata contract.
- [INDEX.md](INDEX.md) — provides the hand-maintained navigation map.
