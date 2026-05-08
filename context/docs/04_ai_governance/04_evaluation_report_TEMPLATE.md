---
title: Evaluation Report Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: ai-governance
capability: ai_governance
phase: execution
cadence: per-release
last_reviewed: 2026-05-07
---

# Evaluation Report Template

> **Purpose:** record the measured results, regressions, failure modes, and recommendation for a specific AI evaluation run or evaluation cycle.
> **Audience:** AI governance, engineering, QA, and decision-makers who need evidence before approving deployment or change.
> **When to update:** update for each material evaluation run, model revision, prompt revision, or release decision.

## How to use this template

Use one report per evaluation cycle that could influence a ship, hold, rollback, or re-review decision. Keep summary metrics in this document and link to deeper raw evidence if the run produces large artifacts.

- Use consistent suite names, metric names, and comparison baselines across reports.
- Record both good results and important failures so review is credible.
- Remove sample metrics before publishing a real evaluation record.

## What not to include

- **Prompt content or prompt versioning** — prompts belong in the prompt registry ([05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md)). Reference prompt IDs here; do not paste prompt text.
- **Dataset raw data or schema detail** — data specifics belong in the dataset card ([03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md)). Reference the dataset version here by ID or name.
- **Policy approval or use-case decisions** — approved use boundaries belong in the AI use policy ([01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md)). The evaluation report provides evidence; the policy makes the governing decision.
- **Incident response narratives** — live incidents belong in the postmortem template ([../06_security_operations/12_incident_postmortem_TEMPLATE.md](../06_security_operations/12_incident_postmortem_TEMPLATE.md)) and the AI risk register ([06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md)).
- **Full benchmark dumps or raw CSV results** — link to stored evidence artefacts rather than embedding large result tables. This report captures the decision-relevant summary.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `ai_governance` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Evaluation date

Record when the evaluation happened and, if useful, the time window or environment used. The date anchors change history and supports later regression analysis.

- Example bullets: run date, environment, evaluator, release candidate ID.
- Example prompt: when and under what evaluation context were these results produced?

## Model+version

Identify the exact model, prompt set, or deployment variant under evaluation. This should map cleanly to the model card and prompt registry.

- Example bullets: model name, model version, prompt set version, feature flag, provider surface.
- Example prompt: what exact AI configuration produced these results?

## Eval suites run

List the benchmark suites, safety checks, scenario packs, or manual review protocols used. The suite list tells readers what evidence exists and what blind spots may remain.

- Example bullets: quality benchmark, safety suite, adversarial prompts, offline replay, human review batch.
- Example prompt: what evaluation methods were used to judge this model?

## Metrics

Summarize the headline scores that matter for the intended use and policy thresholds. Include both task-quality metrics and any relevant safety, compliance, or human-review metrics.

- Example bullets: accuracy, F1, BLEU, precision/recall, safety pass rate, hallucination rate.
- Example prompt: what numbers matter most for the release decision?

## Regression vs prior

Compare this result set with the previous accepted baseline or release candidate. A deployment decision is usually about change, not just absolute quality.

- Example bullets: improved metrics, degraded metrics, unchanged suites, newly introduced regressions.
- Example prompt: how does this run compare with the last accepted state?

## Failure modes

Describe the most important ways the model still fails, especially those with user, safety, or compliance impact. Keep the list actionable and tied to mitigations or go/no-go reasoning.

- Example bullets: hallucinated citations, prompt injection susceptibility, poor minority-language handling, unsafe refusal gaps.
- Example prompt: where does the system still fail in ways decision-makers should care about?

## Recommendation

State whether the evidence supports ship, ship with conditions, hold, or rollback. Be explicit about any required follow-up actions or controls.

- Example bullets: ship conditions, hold blockers, monitoring requirements, re-test triggers.
- Example prompt: what decision should this evidence drive right now?

## Related documents

- [02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md) — the model card summarizes the stable characteristics and recommendation supported by this report.
- [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md) — dataset cards explain the data sources and preprocessing behind the reported results.
- [05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md) — prompt versions used in evaluation should be traceable through the prompt registry.
- [../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md) — use the evidence index to link this report into broader project verification records.
