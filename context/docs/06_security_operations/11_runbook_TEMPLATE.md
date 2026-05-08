---
title: Runbook Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: operations-lead
capability: operations
phase: execution
cadence: per-release
last_reviewed: 2026-05-07
---

# Runbook Template

> **Purpose:** provide the standard structure for a single operational runbook — a step-by-step procedure that an operator or on-call engineer can follow to complete a specific task safely and repeatably.
> **Audience:** operations engineers, on-call responders, and release managers who execute or approve operational procedures; managers who need to confirm runbook coverage exists.
> **When to update:** review and update this template before each release that changes operational procedures, and after any incident reveals a gap in the documented steps.

## How to use this template

Copy this file and rename it to describe the specific procedure: `11_<topic>_runbook_<projectname>.md`. Register the new runbook in the runbook index ([10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md)) immediately.

- Mandatory: Purpose, Trigger, Procedure (with at least one step), Verification, Owner and escalation.
- Optional: Pre-checks and Post-actions if the procedure is short and self-contained enough that these are obvious.
- Write every step as an imperative command ("Run X", "Verify Y", "Notify Z") rather than a description ("You should run X"). Operators follow steps under time pressure — precision matters.
- Remove all example bullets and placeholder text before publishing. A runbook with unreplaced placeholders will mislead the operator who runs it for the first time.

## What not to include

- **General operational policy or principles** — these belong in the operating model (`docs/06_security_operations/03_operating_model_TEMPLATE.md`). A runbook is procedure, not policy.
- **Incident narrative or timeline** — post-incident story-telling belongs in the postmortem (`docs/06_security_operations/12_incident_postmortem_TEMPLATE.md`). A runbook tells you what to do; a postmortem tells you what happened.
- **One-off scripts not yet formalized** — do not paste draft scripts that have not been tested in a non-production environment. Link to a version-controlled script instead, or formalize the script before publishing the runbook.
- **Root-cause analysis or architectural reasoning** — these belong in an ADR or the solution design. A runbook operator should not need to understand the architectural rationale to execute the steps safely.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

---

## Purpose

State what this runbook achieves in one or two sentences. A reader should understand the runbook's goal before opening the Procedure section.

Write this as an outcome: "This runbook restores the API service to a healthy state after an OOM-triggered crash" rather than "This runbook describes what to do." Also state which environment(s) the runbook applies to.

- Example: "This runbook safely drains and replaces a degraded application node in the production Kubernetes cluster without service interruption."
- Example: "This runbook restores the reporting database from a point-in-time backup after data corruption is confirmed. Applies to production only."

## Trigger

Describe the conditions that should cause an operator to open this runbook. Be specific about symptoms, alert names, and request types so that the right runbook is chosen under pressure.

A good trigger description prevents the runbook from being used inappropriately. Include both the signal (what the operator sees) and the confirmed condition (what they must verify before proceeding).

- Example: "Open this runbook when PagerDuty fires alert `api-pod-crashloop` and the pod has restarted more than three times in ten minutes."
- Example: "Open this runbook when the data integrity check in the nightly job reports a non-zero mismatch count AND the on-call lead has confirmed a restore is needed."
- Example: "Open this runbook when a release manager requests a post-deployment rollback within the release window."

## Pre-checks

List the preconditions that must be true and the access and tooling that must be ready before the operator begins the procedure. Failing to verify these before starting is a common source of mid-procedure failures.

Organize pre-checks into three groups: access (permissions, VPN, credentials), environment (expected state of the system before the procedure starts), and tooling (CLI tools, dashboards, communication channels that must be open).

- Access example: "Confirm you have `kubectl` cluster-admin access to the `prod` context. Run `kubectl auth can-i delete pod --all-namespaces` and expect `yes`."
- Environment example: "Confirm the database is in `running` state and no other migration job is active. Run `make db-status` and check for `healthy`."
- Tooling example: "Open the runbook incident channel in Slack (`#ops-runbook`), post that you are starting this procedure, and keep the channel open for handoff."

## Procedure

Present the complete set of steps in strict order. Each step must be numbered, include the exact command or action required, state the expected outcome, and specify what to do if the step fails.

Write for an operator who has the right access and basic familiarity with the tooling but has never run this particular procedure before. Do not assume context that lives only in someone's head.

**Step format:**

> **N. [Action verb] [what]**
> Command: `<exact command or action>`
> Expected outcome: [What the operator should see when the step succeeds.]
> If this fails: [Specific rollback or escalation action.]

**Example steps:**

1. **Notify the team** that the procedure is starting.
   Action: Post in `#ops-runbook`: "Starting [runbook name] at [UTC time]. On-call: @[handle]."
   Expected outcome: Message acknowledged by at least one other team member.
   If this fails: Do not proceed without acknowledgement. Page the backup on-call.

2. **Scale down** the affected deployment.
   Command: `kubectl scale deployment/<name> --replicas=0 -n <namespace>`
   Expected outcome: `deployment.apps/<name> scaled` with zero running pods confirmed by `kubectl get pods -n <namespace>`.
   If this fails: Check for PodDisruptionBudget blocking the scale. Run `kubectl describe pdb -n <namespace>` and escalate to platform team.

3. **Perform the corrective action** [replace with the actual step].
   Command: [exact command]
   Expected outcome: [observable result]
   If this fails: Go to [Failure / rollback section](#failure--rollback).

## Verification

Describe how the operator confirms that the procedure succeeded. Verification steps must be observable — the operator should be able to produce evidence (log output, metric reading, API response) that proves the system is in the expected state.

Do not rely on absence of errors as the sole verification signal. Explicitly state what a healthy system looks like after this procedure.

- Example: "Run `curl -sf https://api.example.com/healthz | jq .status` and confirm the response is `"ok"`. Check the Datadog dashboard `API Health Overview` and confirm p99 latency returns below 200 ms within five minutes."
- Example: "Run the reconciliation script: `make reconcile-check ENV=prod`. Expect `0 discrepancies found`. Save the output as evidence and attach it to the incident ticket."

## Post-actions

List the tasks that must be completed after a successful procedure. These are not optional — they maintain the operational record and ensure the team stays informed.

Typical post-actions include: notifying stakeholders, closing or updating the incident ticket, restoring any temporary access grants, updating monitoring thresholds if they were changed during the procedure, and scheduling a follow-up if the procedure revealed a gap.

- Example: "Post in `#ops-runbook` and `#incidents` that the procedure completed successfully. Include the start time, end time, and link to the ticket."
- Example: "Update the runbook index entry (`10_runbook_index_TEMPLATE.md`) with the date this runbook was last exercised."
- Example: "If any step required a workaround, create a follow-up ticket immediately and link it to this runbook's issue tracker entry."

## Failure / rollback

Describe what the operator should do if the procedure fails partway through and a rollback is needed. Be explicit about the rollback sequence, the expected restored state, and when to escalate rather than attempt recovery alone.

This section should be detailed enough that a different operator who was not present at the start can pick up the rollback safely.

- Example rollback step: "Re-scale the deployment to its previous replica count: `kubectl scale deployment/<name> --replicas=<previous-count> -n <namespace>`. Verify pods reach `Running` state."
- Example escalation trigger: "If the database does not return to `healthy` state within 15 minutes of starting rollback, page the database team lead immediately and do not attempt further changes."

## Owner and escalation

Identify who owns this runbook, who should be paged if the procedure fails or the operator needs help, and what the escalation path looks like.

Keep this section role-based rather than name-based so it remains accurate when individuals change.

- Primary owner: [role, e.g., "operations-lead"]
- Secondary / backup: [role]
- Escalation path: [role → role → role, with contact channel]
- Related incident channel: [Slack channel or equivalent]

## Change log

Record every material change to this runbook with a date and a brief description. Operators must be able to see whether the version they are following is current.

| Date | Changed by | Summary of change |
|---|---|---|
| YYYY-MM-DD | [role] | Initial version |
| YYYY-MM-DD | [role] | [What changed and why] |

## Related documents

- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — the central index that registers this runbook and points operators to all available procedures.
- [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) — the incident response lifecycle that this runbook supports during containment and recovery.
- [../07_delivery/08_cutover_and_rollback_TEMPLATE.md](../07_delivery/08_cutover_and_rollback_TEMPLATE.md) — the release-level cutover and rollback plan that may reference this runbook for production execution.
