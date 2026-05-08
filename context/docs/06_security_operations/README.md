---
title: Security And Operations Templates
status: draft
record_class: canonical
audience: [internal, manager]
owner: operations-lead
capability: operations
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Security And Operations Templates

> **Purpose**: orient readers to the canonical templates for security, service operations, resilience, support, and early-live governance.
> **Audience**: internal teams and managers responsible for security posture, operational readiness, service acceptance, and recovery.
> **When to update**: update when control expectations, support models, runbook structure, or service-acceptance workflows change.

## How to use this template

Use this landing page to assemble the smallest operations document set that still supports secure, supportable service delivery. Start with baseline and threat work, then add operating, support, acceptance, and recovery controls.

- Start with the security baseline and threat model for control expectations.
- Use the operating, support, and runbook templates to define who runs the service.
- Link release and post-launch decisions back to these canonical operations records.

## Folder purpose

This folder holds the canonical templates for securing, operating, supporting, accepting, and reviewing a live service. Use it when a product or platform needs clear ownership for controls, on-call, support, resilience, and incident learning.

- Typical use: define baseline controls, trust boundaries, and access rules before go-live.
- Typical use: capture service acceptance, incident response, backup, and post-launch learning for operated services.

## Index

The templates below move from preventive controls into live operation and early-life governance. Keep the cross-links between release readiness, runbooks, and post-incident learning intact.

- [01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md) — baseline control expectations for authentication, authorization, encryption, logging, network, secrets, and vulnerability management.
- [02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md) — system decomposition, trust boundaries, STRIDE analysis, mitigations, and residual risk.
- [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md) — service ownership, on-call, escalation, change windows, capacity, and cost accountability.
- [04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md) — support tiers, intake, SLA, knowledge flow, and delivery handover.
- [05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md) — identity sources, roles, least privilege, review, and offboarding.
- [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) — incident lifecycle, communication, and learning expectations.
- [07_backup_and_recovery_TEMPLATE.md](07_backup_and_recovery_TEMPLATE.md) — data classes, cadence, retention, restore drills, and RPO or RTO.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — go-live acceptance checks and conditional acceptance rules.
- [09_post_launch_review_TEMPLATE.md](09_post_launch_review_TEMPLATE.md) — early-live outcomes, issues, benefits, and follow-up actions.
- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — central pointer list for component or service runbooks.
- [11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md) — per-procedure or per-component operational runbook for on-call use.
- [12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md) — structured incident learning, timeline, root cause, and follow-up actions.

## Reading order

Read the preventive control documents first, then the operating and support model, then the resilience and acceptance controls, and finally the review and runbook index. This keeps control design, readiness, and learning connected.

1. [01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md) and [02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md)
2. [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md), [04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md), and [05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md)
3. [07_backup_and_recovery_TEMPLATE.md](07_backup_and_recovery_TEMPLATE.md), [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md), and [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md)
4. [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) and [09_post_launch_review_TEMPLATE.md](09_post_launch_review_TEMPLATE.md)
5. [11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md) (one per critical operation) and [12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md) (after first significant incident)

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Service maturity | Recommended templates | Skip |
|---|---|---|
| **Pre-launch** — service not yet in production; no live traffic; initial control design and threat review required before go-live | [01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md) — authentication, authorisation, encryption, logging, and secrets baseline; [02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md) — trust boundaries and STRIDE analysis; [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — go-live acceptance checks | [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md), [04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md), [05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md), [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md), [07_backup_and_recovery_TEMPLATE.md](07_backup_and_recovery_TEMPLATE.md), [09_post_launch_review_TEMPLATE.md](09_post_launch_review_TEMPLATE.md), [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md), [11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md), [12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md) |
| **Live** — service is in production with real users; on-call ownership needed; incidents have occurred or are likely; SLA obligations exist | All pre-launch templates plus [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md) — service ownership and on-call; [04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md) — tiers, intake, and SLA; [05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md); [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md); [07_backup_and_recovery_TEMPLATE.md](07_backup_and_recovery_TEMPLATE.md) — data classes, cadence, and restore drills; [09_post_launch_review_TEMPLATE.md](09_post_launch_review_TEMPLATE.md); [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) | [11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md), [12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md) — add once first runbook or first postmortem is needed |
| **Regulated** — external audit obligation; formal evidence of controls required; data protection, financial, health, or safety regulation applies | Full set: all live templates plus [11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md) — per-component runbooks for each critical operation; [12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md) — structured learning and follow-up for every significant incident | Nothing — all documents apply |

**Rules of thumb:**
- Always start with the security baseline and threat model before writing any other control document — they define the trust assumptions everything else inherits.
- Add runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)) when on-call engineers cannot safely perform an operation from memory alone; one runbook per critical or risky procedure.
- Add the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)) after the first significant incident, not at service launch — it is only useful once there is a failure to learn from.

## Related documents

- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — consumes service-acceptance and operational-readiness decisions before go-live.
- [../07_delivery/08_cutover_and_rollback_TEMPLATE.md](../07_delivery/08_cutover_and_rollback_TEMPLATE.md) — operationalizes the release and fallback sequence supported by these templates.
- [../08_references/01_standards_register_TEMPLATE.md](../08_references/01_standards_register_TEMPLATE.md) — holds the external standards and frameworks these control templates may reference.
