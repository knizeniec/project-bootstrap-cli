---
title: Audience and Export Profiles
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Audience and Export Profiles

> **Purpose**: define export bundles, show how audience tags flow into them, and set the default redaction rules.
> **Audience**: internal teams and managers preparing internal, client, or assurance-oriented document packages.
> **When to update**: update when audience tags, export bundles, or redaction rules change.

## How to use this template

Use this page when deciding which canonical docs are safe and useful to package for different readers. Start from audience tags in frontmatter, then apply the profile rules here rather than making ad hoc export decisions. Document any exceptions explicitly in the project-level export note or release packet.

- Use this page before sharing docs outside the delivery team.
- Validate audience tags in [04_frontmatter_schema.md](04_frontmatter_schema.md).
- Cross-check lifecycle completeness in [01_lifecycle_map.md](01_lifecycle_map.md).

## The three export profiles

The export model keeps packaging simple by using three named bundles: `internal-full`, `client-bundle`, and `auditor-bundle`. Profiles describe default inclusion behavior, not legal access control on their own. Keep the names stable so automation and manual reviews can use the same vocabulary.

- `internal-full`: broad internal context, including planning, design, and operational detail.
- `client-bundle`: externally shareable scope, progress, commitments, and approved evidence.
- `auditor-bundle`: control, traceability, approvals, and evidence needed for assurance review.

## Which audience values flow to which profile

Audience tags help determine whether a document is a candidate for a given export profile. A profile can include documents tagged for multiple audiences, but it should never ignore explicit audience intent. When a document serves both internal and external readers, review its body for mixed-sensitivity content before export.

- `internal-full`: typically includes `internal`, `manager`, and selected `auditor` documents.
- `client-bundle`: typically includes `client` and selected `manager` documents, plus carefully reviewed shared summaries.
- `auditor-bundle`: typically includes `auditor`, `manager`, and selected `internal` documents with traceability value.

## Redaction rules

Redaction should remove sensitive detail while preserving enough context for the exported document to remain meaningful. Prefer publishing a purpose-built summary over heavy inline redaction when the original is deeply internal. Keep the reason for any major omission obvious to the recipient.

- Redact personal data, credentials, secrets, and internal-only operational tactics.
- Redact negotiation notes, unapproved options, and informal commentary from client-facing bundles.
- Preserve identifiers, approval dates, and traceability links needed for audit or decision review.

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [02_role_audience_map.md](02_role_audience_map.md)
- [03_source_of_truth_map.md](03_source_of_truth_map.md)
