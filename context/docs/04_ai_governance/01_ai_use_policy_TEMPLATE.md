---
title: AI Use Policy Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: ai-governance
capability: ai_governance
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# AI Use Policy Template

> **Purpose:** provide the canonical template for defining approved AI use, restricted use, approval gates, and oversight expectations for a project.
> **Audience:** AI governance, product, engineering, security, and management roles who must decide whether an AI use case is allowed and under what controls.
> **When to update:** update when AI use cases, risk boundaries, approval paths, providers, evaluation expectations, or oversight controls change.

## How to use this template

Copy this file when defining the AI governance baseline for a project that uses, embeds, or operates AI-supported behavior. Keep this document policy-level, then link to the model, dataset, evaluation, prompt, risk, security, and evidence artifacts that prove the policy is being followed.

- Keep approved and prohibited use explicit enough that reviewers can make fast decisions.
- Link evaluation evidence and security controls instead of duplicating detailed test or baseline content.
- Remove sample rows and placeholders before publishing a project policy.

## What not to include

- **Model evaluation metrics or test results** — measured evaluation evidence belongs in the evaluation report ([04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md)). This policy sets the bar; the evaluation report shows whether it was met.
- **Model technical detail or training data description** — that belongs in the model card ([02_model_card_TEMPLATE.md](02_model_card_TEMPLATE.md)) and dataset card ([03_dataset_card_TEMPLATE.md](03_dataset_card_TEMPLATE.md)).
- **Prompt registry or prompt text** — managed prompts belong in the prompt registry ([05_prompt_registry_TEMPLATE.md](05_prompt_registry_TEMPLATE.md)).
- **Security infrastructure detail** — control implementation belongs in the security baseline ([../06_security_operations/01_security_baseline_TEMPLATE.md](../06_security_operations/01_security_baseline_TEMPLATE.md)). This policy states what controls are required; the baseline states how they are implemented.
- **Live incident records or breach narratives** — incidents belong in the postmortem template ([../06_security_operations/12_incident_postmortem_TEMPLATE.md](../06_security_operations/12_incident_postmortem_TEMPLATE.md)) and AI risk register ([06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `ai_governance` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Scope

Define the systems, workflows, and model classes covered by this policy, plus any explicit exclusions. A clear scope prevents teams from assuming the policy covers every experimental or adjacent AI use automatically.

- Example bullets: systems in scope, systems out of scope, model classes, operational boundaries.
- Example prompt: where does this policy apply, and where does a separate approval or policy document remain necessary?

## Approved use cases

List the use cases that are explicitly allowed, along with ownership and constraints. Keep the table concrete enough that a reviewer can distinguish “allowed with conditions” from “not yet approved.”

| ID | Use case | Owner | Notes |
| --- | --- | --- | --- |
| AI-001 | [Approved purpose, audience, and data class] | [Role or team] | [Constraints, guardrails, or model class] |
| AI-002 | [Approved purpose] | [Owner] | [Notes] |

## Restricted or prohibited use cases

Record the use cases that are limited, escalated, or outright disallowed, along with the reason. This is where you make high-risk boundaries visible so they are not rediscovered late in delivery.

| ID | Restriction | Reason | Notes |
| --- | --- | --- | --- |
| AI-R-001 | [Disallowed purpose, audience, or data class] | [Risk, regulatory, or contract reason] | [Notes] |
| AI-R-002 | [Restriction] | [Reason] | [Notes] |

## Data and model boundaries

Describe what data may be sent to AI systems, what must never be sent, and which providers or deployment surfaces are approved. Include retention and logging expectations so security and operations teams can align controls early.

- Example bullets: permitted data classes, prohibited data classes, hosting models, retention period, audit logging expectations.
- Example prompt: what boundaries would stop an otherwise useful AI pattern from being acceptable here?

## Approval workflow

Explain how new AI uses are triaged, who approves them, and where the approval trail is recorded. Keep the path simple enough that teams use it, but formal enough that risk ownership is clear.

- Example bullets: triage owner, risk-class-to-approver mapping, required ADR or evidence links, re-review triggers.
- Example prompt: who can approve, who must be consulted, and what evidence is mandatory before use?

## Evaluation and safety

State the minimum evaluation, monitoring, oversight, and adversarial testing expectations for any approved AI use. This section should set the bar that model cards, evaluation reports, prompt reviews, and incident response then support.

- Example bullets: pre-deployment eval suites, safety thresholds, human review points, monitoring metrics, red-team cadence.
- Example prompt: what evidence must exist before shipping, and what checks must continue after launch?

## Compliance and external references

List any internal policies, external frameworks, contractual constraints, or regulatory mappings that matter for this AI use. Keep the list short and point to the canonical references elsewhere.

- Example bullets: internal standards, external frameworks, vendor obligations, customer commitments.
- Example prompt: which external obligations affect this policy and which internal docs translate them into practice?

## Owners and approvers

Name the responsible roles for policy upkeep and sign-off. This makes review and escalation paths explicit instead of relying on informal ownership.

- Example bullets: policy owner, technical approver, security approver, legal or risk approver, next review date.
- Example prompt: who keeps this policy current, and who can block or approve AI use changes?

## Related documents

- [04_evaluation_report_TEMPLATE.md](04_evaluation_report_TEMPLATE.md) — evaluation reports provide the measured evidence required by this policy.
- [06_ai_risk_register_TEMPLATE.md](06_ai_risk_register_TEMPLATE.md) — the risk register tracks ongoing AI-specific risks, mitigations, and review cadence.
- [../06_security_operations/01_security_baseline_TEMPLATE.md](../06_security_operations/01_security_baseline_TEMPLATE.md) — the security baseline defines the control floor AI systems must inherit.
- [../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md) — the evidence index provides a place to link concrete verification records referenced by this policy.
