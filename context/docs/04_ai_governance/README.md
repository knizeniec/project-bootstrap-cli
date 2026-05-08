---
title: AI Governance Templates
status: draft
record_class: canonical
audience: [internal, manager]
owner: ai-governance
capability: ai_governance
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# AI Governance Templates

> **Purpose:** orient readers to the canonical templates used to govern AI use, model evidence, prompts, and AI-specific risk controls.
> **Audience:** internal teams and managers responsible for approving, evaluating, operating, or assuring AI-enabled capabilities.
> **When to update:** update when AI governance templates, reading order, or cross-links to testing and security evidence change.

## How to use this template

Use this landing page to choose the right AI-governance artifact for the question at hand: policy, evidence, prompt control, or risk review. Start with the AI use policy, then add model, dataset, evaluation, prompt, and risk records as the project’s AI surface grows.

- Use the policy first to define allowed use and approval boundaries.
- Add model, dataset, and evaluation records when a system depends on a specific AI asset or measured behavior.
- Link to testing and security artifacts instead of duplicating control evidence.

## Folder purpose

This folder holds the governance layer for AI-enabled behavior, from policy and approval through evidence and ongoing risk management. Use it to make AI decisions reviewable and to keep operational, security, and quality links visible.

- Typical use: approve an AI feature before release with clear constraints and evidence requirements.
- Typical use: maintain traceable records for model changes, prompt changes, and AI-specific risks.

## AI-governance index

The templates move from policy to asset description to measured evidence and operational risk. Readers can stop early for low-risk cases, but higher-assurance work should use the full set.

- [01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md) — allowed use, restricted use, approval workflow, and oversight rules.
- [02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md) — model details, intended use, limitations, and recommendations.
- [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md) — dataset structure, sourcing, preprocessing, and bias notes.
- [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md) — measured performance, regressions, failure modes, and ship/hold recommendation.
- [05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md) — prompt inventory with owners, versions, and evaluation links.
- [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) — AI-specific risk inventory, mitigations, and review cadence.

## Reading order

Read the policy first so downstream evidence and risk records inherit clear guardrails. Then move from model and dataset description into evaluation, prompt control, and risk review, using security and quality artifacts for broader assurance evidence.

1. [01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md)
2. [02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md) and [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md)
3. [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md)
4. [05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md) and [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md)
5. [../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md) and [../06_security_operations/01_security_baseline_TEMPLATE.md](../06_security_operations/01_security_baseline_TEMPLATE.md)

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| AI risk level | Recommended templates | Skip |
|---|---|---|
| **Assistive** — AI produces suggestions that a human always reviews and approves before action; no automated decision path; low-stakes domain | [01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md) — define allowed use, restricted use, and approval boundaries | [02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md), [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md), [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md), [05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md), [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) |
| **Decision-support** — AI output directly informs a consequential decision; output is visible to end users or clients; domain carries moderate risk of harm or error | [01_ai_use_policy_TEMPLATE.md](01_ai_use_policy_TEMPLATE.md); [02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md) — describe model, intended use, and limitations; [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md) — measured performance and ship or hold recommendation; [05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md) — versioned prompt inventory when prompts drive user-facing output | [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md) — add only if the team owns the training or fine-tuning dataset; [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) — add when risk surface extends beyond the model card |
| **Autonomous** — AI takes action without per-action human review; decisions have legal, safety, financial, or compliance consequences; regulatory or contractual obligation to evidence AI controls | Full set: all decision-support templates plus [03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md) — dataset sourcing, preprocessing, and bias notes; [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) — AI-specific risk inventory, mitigations, and review cadence | Nothing — all documents apply |

**Rules of thumb:**
- Promote from assistive to decision-support as soon as AI output is shown directly to an end user or client without an intermediate human review step.
- Add the evaluation report ([04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md)) before every production release that changes model, prompt, or dataset — not only at initial launch.
- Keep the prompt registry ([05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md)) even when the model card is skipped; unversioned prompts are the most common source of untracked AI behaviour change.

## Related documents

- [../05_testing_acceptance/README.md](../05_testing_acceptance/README.md) — testing artifacts hold evaluation evidence, UAT records, and quality gates linked from AI governance docs.
- [../06_security_operations/README.md](../06_security_operations/README.md) — security and operations artifacts provide the baseline controls that AI systems must inherit.
- [../02_product/README.md](../02_product/README.md) — product artifacts explain which user and business outcomes the governed AI capability is meant to support.
