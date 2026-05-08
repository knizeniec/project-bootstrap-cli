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

## Per-readiness-domain register

| Domain | Status | Owner | Gating evidence link |
| --- | --- | --- | --- |
| Scope | Green | Product owner | [PRD](../02_product/01_prd.md) and [Project Brief](../00_governance/00_project_brief.md) approved for baseline scope. |
| Design | Green | Architecture owner | [Solution Design](../03_architecture/01_solution_design.md) defines modular architecture and constraints. |
| Build | Red | Engineering lead | Blocked: backlog not created, module sequencing not finalized. Recovery: finalize implementation backlog and confirm integration test coverage. |
| Test | Amber | QA lead | Strategy and evidence index created: [Test Strategy](../05_testing_acceptance/01_test_strategy.md), [Verification Evidence Index](../05_testing_acceptance/03_verification_evidence_index.md). Remaining: execute suites and replace pending evidence rows with run artifacts. |
| Release | Amber | Release manager | [Release Plan](../07_delivery/07_release_plan.md) created with sequence, rollback, and comms. Remaining: run release rehearsal and complete go/no-go evidence. |
| Ops | Amber | Operations lead | [Support Model](../06_security_operations/04_support_model.md) and [Incident Response](../06_security_operations/06_incident_response.md) created. Remaining: confirm on-call staffing and run incident drill. |

## Notes and recovery actions

- Build to Green:
  - Finalize module backlog and implementation sequencing.
  - Confirm integration test coverage for Q&A, assessment, generation, and review modules.
- Test to Green:
  - Execute integration and end-to-end suites.
  - Replace pending evidence index rows with concrete run IDs and artifact links.
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
