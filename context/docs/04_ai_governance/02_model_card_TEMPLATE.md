---
title: Model Card Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: ai-governance
capability: ai_governance
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

# Model Card Template

> **Purpose:** document the identity, intended use, measured behavior, limitations, and recommendations for a specific model version.
> **Audience:** AI governance, engineering, QA, security, and approving managers who need a concise but reviewable description of a model in use.
> **When to update:** update when the model version, intended use, evaluation results, limitations, or deployment recommendation changes.

## How to use this template

Create one model card per model version or materially distinct deployment configuration. Keep the content decision-relevant and link out to evaluation evidence, datasets, and policy records instead of repeating long technical background.

- Use stable model identifiers and version references.
- Summarize rather than duplicate training or evaluation detail that already lives elsewhere.
- Remove example prompts and placeholder caveats before publishing.

## What not to include

- **Training infrastructure or compute detail** — infrastructure details belong in the deployment template ([../03_architecture/07_deployment_TEMPLATE.md](../03_architecture/07_deployment_TEMPLATE.md)) or model provider documentation. The model card summarises governance-relevant attributes, not training setup.
- **Prompt content or prompt engineering notes** — managed prompts belong in the prompt registry ([05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md)).
- **User-facing release notes or product announcements** — release notes belong in the release plan ([../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md)) and user documentation.
- **Raw evaluation data or full benchmark results** — detailed metrics belong in the evaluation report ([04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md)). The model card summarises the headline recommendation supported by that report.
- **Live incident records** — AI incidents belong in the AI risk register ([06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md)) and postmortem template ([../06_security_operations/12_incident_postmortem_TEMPLATE.md](../06_security_operations/12_incident_postmortem_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `ai_governance` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Model details

Record the model name, version, type, provider, and any deployment-specific notes needed to identify exactly what is being governed. A reviewer should be able to distinguish this model from related variants without reading another file first.

- Example bullets: model family, version tag, provider, hosting mode, notable configuration.
- Example prompt: what exact model instance is this card about?

## Intended use

Describe the approved tasks, users, and operating context for this model. Keep the statement bounded so that “possible” use does not become “approved” use by implication.

- Example bullets: supported tasks, expected user groups, supported languages, operational constraints.
- Example prompt: what should this model be used for, and what should it not be relied on for?

## Training data summary

Summarize the training or fine-tuning data sources at the level needed for governance review. Focus on provenance, recency, representativeness, and major exclusions rather than reproducing raw dataset documentation.

- Example bullets: data origin, date range, broad composition, major gaps, sensitive-data handling notes.
- Example prompt: what should a reviewer know about the data that shaped this model?

## Evaluation results

Summarize the headline metrics and the evaluation suites that support deployment decisions. Keep the detail high signal and point to the full evaluation report for raw evidence.

- Example bullets: primary metrics, safety metrics, benchmark suites, threshold outcomes, regression result.
- Example prompt: what evidence shows this model is acceptable for its intended use?

## Ethical considerations

Call out fairness, misuse, safety, transparency, or accountability concerns that matter for the chosen use case. Keep the discussion tied to practical implications and mitigations.

- Example bullets: bias risk, automation risk, explainability limits, human-review expectations.
- Example prompt: what responsible-use concerns should decision-makers know before approval?

## Limitations

Describe known boundaries, weak spots, unsupported tasks, or confidence limits. Reviewers should be able to see where the model is expected to fail or degrade.

- Example bullets: unsupported domains, poor edge-case handling, drift sensitivity, prompt fragility.
- Example prompt: where does this model stop being dependable?

## Caveats

List operational warnings that may not fit neatly into limitations, such as vendor dependencies, cost spikes, or monitoring blind spots. This helps teams plan safe rollout and support.

- Example bullets: rate-limit exposure, region constraints, rollback complexity, audit-log gaps.
- Example prompt: what practical warnings should operators and approvers see immediately?

## Recommendations

State the deployment recommendation and any conditions attached to it. Be explicit about whether the conclusion is ship, ship with guardrails, hold, or retire.

- Example bullets: ship conditions, hold reasons, re-evaluation triggers, required follow-up controls.
- Example prompt: what action should the reader take after reviewing this card?

## Related documents

- [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md) — the dataset card provides the source and preprocessing detail summarized here.
- [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md) — the evaluation report contains the detailed metrics, regressions, and failure analysis backing this card.
- [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) — risks identified here should be tracked and owned in the AI risk register.
- [01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md) — intended use and recommendations must stay within the policy’s approved boundaries.
