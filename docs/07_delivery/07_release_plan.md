---
title: Documentation Development Tool - Release Plan
status: active
record_class: canonical
audience: [internal, manager]
owner: release-manager
capability: execution
phase: execution
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

<!-- markdownlint-disable MD013 -->

## Release scope

Phase 1 release includes:

- Q&A flow for project context capture.
- Small-scope file necessity assessment.
- Documentation generation and adaptation for core docs.
- Built-in documentation quality review feedback.

Deferred:

- Advanced template customization.
- API-first remote workflow support.
- Enterprise-grade governance packs.

Release candidate label: `RC-1`

## RC-1 scope boundaries

Committed release scope:

- Small-scope project flow (Q&A, assessment, generation, review, readiness reporting).
- RC1 release rehearsal and UAT-lite sign-off.

Additional validation scope (non-blocking exploratory coverage):

- Medium and large classification assessment evidence.
- Extended performance and security checks beyond minimum release gates.

## Environments

- Dev: developer verification and iteration.
- CI: automated validation and markdown/frontmatter checks.
- Staging-like workspace: final end-to-end rehearsal.
- Production: initial public/organization release target.

## Release window and freeze

- Preferred release window: weekday 10:00-14:00 local time.
- Change freeze starts 24 hours before production release.
- Only Sev-1 fixes are allowed during freeze; all others are deferred.

## Sequence

1. Freeze release scope and confirm open-risk list.
2. Run final verification suite and update evidence index.
3. Execute go/no-go checkpoint with product, QA, and release owners.
4. Publish release artifacts and update docs references.
5. Monitor first-use period and log incidents/issues.

## Go/no-go checklist

| Check | Owner | Required state |
| --- | --- | --- |
| Evidence matrix updated | QA lead | P0 rows passed or risk-accepted; P1 exceptions documented |
| Readiness tracker updated | Release manager | Test, Release, and Ops at least Amber with dated notes |
| Open Sev-1 defects | Engineering lead | 0 |
| Open Sev-2 defects | Product owner | Mitigation and acceptance documented |
| Support rota confirmed | Operations lead | Next 7 days staffed |
| Incident channel prepared | Incident manager | Active channel and escalation contacts verified |

Gate interpretation notes:

- A P1 exception is valid only when owner, linked issue ID, and target date are present.
- Go/no-go cannot proceed while any P0 evidence row is `Fail`.
- UAT-LITE-001 and REL-REHEARSAL-001 must be linked in the evidence index before final go/no-go.
- Readiness notes must include the latest update date and remaining blocker summary.
- Build Amber exception policy: release may proceed only if Engineering lead signs Amber exception with explicit risk statement in [go-no-go decision record](../evidence/rc1/go-no-go-decision.md).

## Go/no-go decision record

The canonical decision log for RC1 is:

- [../evidence/rc1/go-no-go-decision.md](../evidence/rc1/go-no-go-decision.md)

This record must include decision owners, timestamp, accepted risks, and follow-up actions.

## Rollback

Rollback triggers:

- Critical workflow failure in Q&A, generation, or review modules.
- Sev-1 issue detected during release window.

Rollback actions:

- Revert to previous stable release tag.
- Restore last validated docs outputs for internal users.
- Communicate rollback status and expected recovery timeline.

Rollback decision owner during active incident: incident commander.

Rollback decision owner outside active incident: release manager.

Maximum rollback decision time target: 20 minutes from Sev-1 confirmation.

## Incident decision authority

- During an active incident, the incident commander has sole decision authority for containment, rollback, and recovery sequencing.
- The incident manager owns incident preparedness and channel readiness before incidents, but does not override active-incident decisions.
- The release manager owns normal release sequencing and go/no-go decisions when no incident is active.

## Communications plan

- Pre-release: notify internal stakeholders of scope and release window.
- During release: provide status updates in release channel at each gate.
- Post-release: publish summary including known issues and support path.

Communication channels:

- Primary: team release channel.
- Escalation: incident channel.
- Summary artifact: release summary note linked in this file after each release.

Release summary artifact path:

- `docs/evidence/rc1/release-summary-rc1.md`
- [../evidence/rc1/release-summary-rc1.md](../evidence/rc1/release-summary-rc1.md)

## Success criteria

- Critical workflows complete successfully in production-like checks.
- Verification evidence index shows required checks executed.
- No Sev-1 incidents in first 24 hours after release.
- Support model and incident-response pathways are active and staffed.

## First 24-hour monitoring approach

- Check key workflow health at T+15m, T+1h, T+4h, and T+24h.
- Record incidents, user-reported issues, and mitigations in a single release log.
- If Sev-1 occurs, move to incident response workflow immediately.

## Related documents

- [06_readiness_tracker.md](06_readiness_tracker.md)
- [../05_testing_acceptance/03_verification_evidence_index.md](../05_testing_acceptance/03_verification_evidence_index.md)
- [../06_security_operations/04_support_model.md](../06_security_operations/04_support_model.md)
- [../06_security_operations/06_incident_response.md](../06_security_operations/06_incident_response.md)
- [../evidence/rc1/go-no-go-decision.md](../evidence/rc1/go-no-go-decision.md)
- [../evidence/rc1/support-contact-roster.md](../evidence/rc1/support-contact-roster.md)
- [../evidence/rc1/release-summary-rc1.md](../evidence/rc1/release-summary-rc1.md)

<!-- markdownlint-enable MD013 -->
