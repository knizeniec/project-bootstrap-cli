---
title: Dataset Card Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: ai-governance
capability: ai_governance
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

# Dataset Card Template

> **Purpose:** document the source, structure, handling, and governance-relevant characteristics of a dataset used for AI training, tuning, evaluation, or monitoring.
> **Audience:** AI governance, engineering, QA, security, and management roles reviewing data suitability and risk.
> **When to update:** update when dataset composition, collection, preprocessing, licensing, supported tasks, or risk characteristics change.

## How to use this template

Create one dataset card per meaningful dataset or dataset version that materially affects model behavior or evaluation. Keep the content governance-focused and link to storage or technical references rather than embedding raw schema dumps.

- Use stable dataset names and version references.
- Summarize only the fields and handling characteristics that matter for review.
- Remove sample rows and placeholder citations before publishing.

## What not to include

- **Raw data samples or data exports** — never include actual dataset content in a governance document. Describe the data structure and handling; link to controlled storage.
- **Model evaluation results** — measured outcomes from running a model on this dataset belong in the evaluation report ([04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md)). This card describes the dataset, not the model's performance on it.
- **Model training infrastructure or hyperparameters** — infrastructure details belong in the deployment or model card. The dataset card covers the data, not the training setup.
- **Prompt content or inference configuration** — prompt management belongs in the prompt registry ([05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md)).
- **Live risk tracking or incident records** — AI risks belong in the AI risk register ([06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md)); dataset-related concerns raised there should link back to this card by ID.

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

## Dataset summary

Describe what the dataset is, why it exists, and how large or representative it is at a high level. A reader should quickly understand whether this is training data, evaluation data, monitoring data, or another class of artifact.

- Example bullets: purpose, size, time span, domain, version, freshness.
- Example prompt: what is this dataset for, and what broad shape does it have?

## Supported tasks

State the tasks or analyses this dataset is appropriate for, plus any known unsupported uses. This helps prevent reuse in contexts it was never designed to support.

- Example bullets: classification, retrieval, summarization, safety evaluation, drift monitoring.
- Example prompt: what work can this dataset support responsibly?

## Languages

List the languages present or supported, along with any material imbalance or exclusion. This is especially important when language coverage affects user equity or performance expectations.

- Example bullets: primary languages, minority coverage, unsupported locales, translation assumptions.
- Example prompt: how does language coverage shape expected model behavior?

## Structure

Summarize the dataset fields, types, and notable schema rules in a compact way. Include only the structure needed for governance review and downstream consumers to understand what the data contains.

- Example bullets: key fields, label types, free-text presence, sensitive fields, annotation format.
- Example prompt: what are the important columns or objects and what do they represent?

## Data sources

Record where the data came from and under what authority or agreement it is used. Provenance should be clear enough that reviewers can assess reuse risk and trustworthiness.

- Example bullets: internal source systems, public corpus, vendor feed, consent basis, contract reference.
- Example prompt: where did this data originate and why is use permitted?

## Collection process

Describe how the data was gathered or assembled, including any sampling or selection logic that might matter. This is where hidden bias or incompleteness often begins.

- Example bullets: sampling method, collection period, annotator process, inclusion/exclusion rules.
- Example prompt: how did the data get from source to dataset?

## Preprocessing

Summarize major cleaning, normalization, labeling, filtering, or de-identification steps. Keep enough detail that reviewers can see what transformations may affect behavior or risk.

- Example bullets: deduplication, redaction, labeling rules, balancing, tokenization, chunking.
- Example prompt: what material transformations changed the data before use?

## Bias considerations

Call out representational gaps, labeling risks, or other fairness concerns that matter for intended use. The goal is not perfection but explicit acknowledgement and mitigation planning.

- Example bullets: skewed populations, noisy labels, missing contexts, proxy-variable risk.
- Example prompt: what could make this dataset systematically misleading or unfair?

## Licensing

State the license, usage rights, and important restrictions for the dataset. If multiple sources apply, explain the strictest practical boundary.

- Example bullets: license type, redistribution rules, commercial-use limits, retention constraints.
- Example prompt: what legal or contractual conditions govern this dataset?

## Citations

List the canonical references a reviewer or later maintainer should follow to understand the dataset origin or methodology. Keep links stable and avoid ad hoc references.

- Example bullets: source paper, vendor documentation, collection protocol, internal approval record.
- Example prompt: which references best explain and justify this dataset?

## Related documents

- [02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md) — the model card should summarize the parts of this dataset that materially affect model behavior.
- [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md) — evaluation reports should identify which dataset versions were used and for what purpose.
- [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) — data risks identified here should be tracked and owned in the risk register.
- [01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md) — dataset use must stay within the policy’s data and model boundaries.
