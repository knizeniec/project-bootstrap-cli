---
title: Documentation Development Tool - Readiness Tracker
status: active
record_class: canonical
audience: [internal, manager]
owner: release-manager
capability: execution
phase: monitoring
cadence: weekly
last_reviewed: 2026-05-08
source_of_truth: repo
---

<!-- markdownlint-disable MD013 -->

## Per-readiness-domain register

| Domain | Status | Owner | Gating evidence |
| --- | --- | --- | --- |
| Scope | Green | Product owner | [PRD](../02_product/01_prd.md), [Project Brief](../00_governance/00_project_brief.md) |
| Design | Green | Architecture owner | [Solution Design](../03_architecture/01_solution_design.md) |
| Build | Red | Engineering lead | Implementation backlog and integration coverage evidence |
| Test | Amber | QA lead | [Test Strategy](../05_testing_acceptance/01_test_strategy.md), [Verification Evidence Index](../05_testing_acceptance/03_verification_evidence_index.md) |
| Release | Amber | Release manager | [Release Plan](../07_delivery/07_release_plan.md) |
| Ops | Amber | Operations lead | [Support Model](../06_security_operations/04_support_model.md), [Incident Response](../06_security_operations/06_incident_response.md) |

## Ownership handoff note

For delivery-to-operations handoff ownership and backup assignments, use the ownership matrix in [../06_security_operations/04_support_model.md#ownership-matrix](../06_security_operations/04_support_model.md#ownership-matrix).

## Notes and recovery actions

- Build to Green:
- Finalize module backlog and implementation sequencing.
- Confirm integration test coverage for Q&A, assessment, generation, and review modules.
- Test to Green:
- Execute integration and end-to-end suites.
- Replace pending evidence rows with concrete run IDs and artifact links.
- Release to Green:
- Run release rehearsal against staged environment.
- Validate go/no-go criteria against updated acceptance evidence.
- Ops to Green:
- Confirm support rota and escalation coverage.
- Complete one incident simulation and publish follow-up actions.

## Related documents

- [../00_governance/00_project_brief.md](../00_governance/00_project_brief.md)
- [../01_strategy/01_product_vision.md](../01_strategy/01_product_vision.md)
- [../02_product/01_prd.md](../02_product/01_prd.md)
- [../03_architecture/01_solution_design.md](../03_architecture/01_solution_design.md)
- [../05_testing_acceptance/01_test_strategy.md](../05_testing_acceptance/01_test_strategy.md)
- [../05_testing_acceptance/03_verification_evidence_index.md](../05_testing_acceptance/03_verification_evidence_index.md)
- [07_release_plan.md](07_release_plan.md)
- [../06_security_operations/04_support_model.md](../06_security_operations/04_support_model.md)
- [../06_security_operations/06_incident_response.md](../06_security_operations/06_incident_response.md)

<!-- markdownlint-enable MD013 -->
