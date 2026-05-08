---
title: "Helio — Invoice Category Classifier: AI Use Policy"
status: active
record_class: canonical
audience: [internal, manager]
owner: ai-governance
capability: ai_governance
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

> **EXAMPLE — not a real project.** This is a worked example based on the fictional "Helio" project. Use it as a reference for tone, depth, and cross-link patterns when filling [the corresponding template](../04_ai_governance/01_ai_use_policy_TEMPLATE.md). Anonymise / replace all values when adapting.

# Helio — Invoice Category Classifier: AI Use Policy

## Scope

This policy covers the invoice-category classification use case within the Helio invoicing API. Specifically, it applies to:

- The `category-suggester` sidecar component that calls an LLM to suggest an invoice line-item category during invoice creation.
- The rule-based fallback classifier that activates when the LLM is unavailable.
- The audit and monitoring workflows that track classification suggestions, human overrides, and model drift.

This policy does not cover any other AI or ML use on the Helio project, nor does it govern AI use by the upstream billing system or the B2B customer systems that consume Helio's API. Any new AI use case introduced into Helio requires a separate approval under the standard AI intake process before deployment.

## Approved use cases

| ID | Use case | Owner | Notes |
| --- | --- | --- | --- |
| AI-001 | Invoice category suggestion: the classifier suggests one of 12 predefined invoice-line categories (e.g. `software-licence`, `professional-services`, `infrastructure`) for each new invoice. Output is advisory; a human billing administrator must accept or override before the invoice is issued. | Priya Saxena (Product Lead) | Assistive only. Confidence score must be surfaced to the user. Automatic acceptance is prohibited (see AI-R-001). Model version must be logged per invoice (see audit requirements). |

## Restricted or prohibited use cases

| ID | Restriction | Reason | Notes |
| --- | --- | --- | --- |
| AI-R-001 | Automatic acceptance of category suggestions without human review | Human-in-the-loop is a design requirement (PRD FR-006). Removing it would change the risk classification to medium or high and require a new approval. | Enforced at the API level: the invoice creation endpoint does not accept a `category_accepted: true` flag without a recorded actor ID. |
| AI-R-002 | Use of classification output for credit decisions, payment authorisation, or contract enforcement | Classification is advisory financial categorisation, not a decision with legal or financial consequence. Using it for credit or authorisation would require a materially different risk assessment. | Blocked by design: the `category` field is metadata only; no downstream payment or authorisation flow reads it. |
| AI-R-003 | Sending customer PII to the LLM provider | The EU data-residency constraint and contractual obligations with the LLM provider prohibit sending identifying data. Only `line_items[].description` and `line_items[].amount` are included in classifier requests. | Enforced by the `category-suggester` request builder. Validated by automated test in the CI pipeline (`test_no_pii_in_classifier_request`). |
| AI-R-004 | Use of a non-EU LLM endpoint | EU data-residency requirement (NFR-004 in the PRD). All inference must occur on EU-hosted endpoints. | Enforced by network policy: `category-suggester` is only permitted to reach the approved EU endpoint (VPC egress allowlist). Any provider endpoint change requires a new approval and IaC change. |

## Data and model boundaries

**Permitted input data.** The classifier receives only invoice line-item text: the `description` string (max 500 characters) and the numeric `amount` field. No customer name, address, email, tax ID, or any other personally identifying field is included in the request payload.

**Prohibited data.** All PII fields present in the invoice record (customer name, VAT number, billing address, contact email) must not be sent to the LLM provider. The `category-suggester` enforces this by constructing a minimal request object from a strict allowlist of fields; it does not pass the full invoice record.

**Approved model.** Claude Haiku Helio-1 hosted on the provider's EU inference endpoint (`eu-api.anthropic-fictional.example/v1`). Model version is pinned in the `category-suggester` configuration and must not be updated without a documented change review and a new entry in the prompt registry ([05_prompt_registry_helio.md](../04_ai_governance/05_prompt_registry_helio.md)).

**Fallback classifier.** When the LLM is unavailable (circuit breaker open), the rule-based fallback classifier applies keyword matching against the 12 categories. Fallback output uses the same response schema with `source: "fallback"` and `confidence: null`. Billing administrators see a distinct indicator when the suggestion came from the fallback.

**Retention.** The classifier request payload (line-item text + amount) is not stored by the LLM provider beyond the inference call (governed by the provider contract). Helio stores the suggested category label, the confidence score, the model version, and whether the suggestion was accepted or overridden — in the invoice audit log only.

**Logging.** Every classifier invocation is logged with: `invoice_id`, `classifier.source` (`llm` | `fallback` | `unavailable`), `classifier.model_version`, `classifier.confidence`, `category_suggested`, `category_accepted` (boolean), `actor_id` (if overridden). Logs are retained for 7 years in line with the invoice audit log retention policy.

## Approval workflow

New AI use cases in Helio follow the organisation's standard AI intake process:

1. **Triage (Priya Saxena):** the product lead logs the proposed use case in the AI intake form and assigns a draft risk class using the standard risk matrix.
2. **Risk assessment (Priya Saxena + Joe Kwan):** the product lead and lead engineer review the use case against the data, model, and deployment boundaries. A supporting document (model card or evaluation report) must be linked.
3. **Approval by risk class:**
   - Low risk (assistive, human-in-the-loop, no PII): Priya Saxena approves; Lena Rosen (Sponsor) is notified.
   - Medium risk: Lena Rosen approves; legal is consulted.
   - High risk: board-level review required before deployment.
4. **Evidence linkage:** the approved use case is added to this policy table with a link to the evaluation report and model card. The ADR for the model choice ([ADR-0002](../adr/ADR-0002-llm-provider-choice.md)) records the decision rationale.
5. **Re-review triggers:** model version change; confidence threshold adjustment; new category labels; any change to the data sent to the classifier; a measured drop in precision/recall below threshold.

The current use case (AI-001) is classified as **low risk** and was approved by Priya Saxena on 2026-04-15, with notification to Lena Rosen.

## Evaluation and safety

**Pre-deployment evaluation.** Before the classifier is deployed to production, a labelled test set of 1,000 invoice samples (drawn from anonymised historical invoices) must be evaluated. Required thresholds: precision ≥0.85 and recall ≥0.80 across all 12 categories. Results are recorded in [04_evaluation_report_helio_classifier.md](../04_ai_governance/04_evaluation_report_helio_classifier.md).

**Confidence threshold.** Category suggestions with a confidence score below 0.6 are surfaced to the billing administrator with a visible low-confidence indicator. The administrator cannot accept a low-confidence suggestion without actively confirming it (an extra confirmation step is required in the UI). This threshold is reviewed as part of the monthly drift review and may be adjusted by Priya Saxena without a new policy approval, provided it does not drop below 0.4.

**Human override.** Every invoice must be reviewed by a billing administrator before issue. The `category_accepted` audit field is mandatory; an invoice cannot transition to `issued` status until a human actor has explicitly accepted or overridden the category.

**Adversarial and edge-case testing.** Before GA, Diego Reyes (QA Lead) must run the classifier against a set of adversarial inputs: very short descriptions (<5 words), mixed-language descriptions, and descriptions that span multiple categories. Pass criteria: no unexpected errors; confidence score is not inflated for ambiguous inputs (tested by checking that ambiguous inputs produce confidence ≤0.5).

**Post-deployment monitoring:**
- Weekly: automated precision/recall measurement on a labelled sample of 50 recent invoices (sampled randomly from the previous week's production invoices with accepted or overridden categories). Alert if precision or recall drops below 0.80.
- Monthly: drift review meeting (Priya Saxena, Diego Reyes, Joe Kwan). Agenda: review weekly metrics trend, review override rate by category, assess cost against the €500/month ceiling, decide whether model version or threshold adjustment is needed.
- Quarterly: bias review by category (do any customer segments or invoice types see systematically lower accuracy?). Conducted by Diego Reyes with sign-off from Lena Rosen.
- Cost ceiling: LLM inference cost is monitored via the platform cost dashboard. An alert fires if monthly spend exceeds €400 (warning) or €500 (hard ceiling, triggers automatic fallback-only mode until the next month).

## Compliance and external references

- **EU AI Act (2024/1689).** The category classifier falls within the "AI systems used in the area of work" definition. Given the assistive classification (human-in-the-loop, low risk, no automated decision-making), it is assessed as a limited-risk system. The transparency requirement (users know they are interacting with AI output) is met by the confidence-score display in the UI.
- **GDPR (Regulation 2016/679).** No personal data is sent to the LLM provider. Audit log retention (7 years) is consistent with financial record-keeping obligations under applicable EU law.
- **Internal AI Acceptable Use Policy v2.3.** This project policy is a specialisation of the organisation-level AI AUP. Any conflict is resolved in favour of the more restrictive control.
- **LLM Provider Data Processing Agreement (DPA).** The provider's EU DPA prohibits training on customer data and guarantees data does not leave the EU inference region. Reference: Contract ref HC-2025-0412.

## Owners and approvers

| Role | Name | Responsibility |
| --- | --- | --- |
| Policy owner | Priya Saxena (Product Lead) | Keeps this policy current; approves low-risk AI use cases; triggers monthly drift review. |
| Technical approver | Joe Kwan (Lead Engineer / PM) | Reviews model and data boundary changes; owns the `category-suggester` implementation. |
| QA approver | Diego Reyes (QA Lead) | Signs off pre-deployment evaluation report and quarterly bias review. |
| Sponsor sign-off | Lena Rosen (VP Finance Ops) | Notified of all AI use case approvals; must approve medium- and high-risk uses. |
| SRE owner | Anna Mirov (SRE) | Owns the network policy enforcing EU-endpoint-only egress and cost-ceiling alerting. |
| Next policy review | 2026-06-07 | Monthly cadence; earlier if a re-review trigger fires. |

## Related documents

- [filled_prd_example.md](filled_prd_example.md) — worked example PRD for Helio; FR-005, FR-006, NFR-006, and NFR-007 define the product requirements this policy governs.
- [filled_solution_design_example.md](filled_solution_design_example.md) — worked example solution design; sections 4, 8, and 10 describe the technical implementation of the classifier and its controls.
- [../04_ai_governance/01_ai_use_policy_TEMPLATE.md](../04_ai_governance/01_ai_use_policy_TEMPLATE.md) — the template this example was derived from.
- [../04_ai_governance/02_model_card_helio_classifier.md](../04_ai_governance/02_model_card_helio_classifier.md) — model card for Claude Haiku Helio-1; describes training data, known limitations, and provider SLA.
- [../04_ai_governance/04_evaluation_report_helio_classifier.md](../04_ai_governance/04_evaluation_report_helio_classifier.md) — pre-deployment evaluation results (precision/recall by category).
- [../04_ai_governance/05_prompt_registry_helio.md](../04_ai_governance/05_prompt_registry_helio.md) — registered prompts used by the `category-suggester`; version-controlled and reviewed on each model update.
- [../adr/ADR-0002-llm-provider-choice.md](../adr/ADR-0002-llm-provider-choice.md) — architectural decision record for LLM provider selection.
- [../06_security_operations/01_security_baseline_helio.md](../06_security_operations/01_security_baseline_helio.md) — security baseline defining the control floor the classifier implementation must meet.
