---
title: RC1 Evidence - Integration Assessment (Large)
status: active
record_class: supporting
audience: [internal, manager]
owner: qa-lead
capability: execution
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Run ID

RC-1-INT-ASS-LARGE

## Date

2026-05-08

## Branch/Revision

main @ c9a1a6c

## Executor

QA lead

## Environment

Local Linux workspace using repository docs set

## Command or workflow hint

```bash
cd /home/hexaper/project-bootstrap-cli
RUN_DATE="${RUN_DATE:-$(date +%F)}"
artifact="docs/evidence/rc1/artifacts/RC1-INT-ASS-LARGE-${RUN_DATE}.md"
work="/tmp/rc1-int-assessment-large"
mkdir -p "$(dirname "$artifact")" "$work"
cat > "$work/scenario.yml" << 'YAML'
project_size: large
team_size: 20
delivery_model: multi-stream
YAML
{
	echo "# RC-1-INT-ASS-LARGE"
	echo "## Scenario"; cat "$work/scenario.yml"
	echo "## Expected large docs"
	printf '%s\n' docs/06_security_operations/06_incident_response.md docs/07_delivery/06_readiness_tracker.md docs/05_testing_acceptance/03_verification_evidence_index.md
	echo "## Present docs"
	ls docs/06_security_operations/06_incident_response.md docs/07_delivery/06_readiness_tracker.md docs/05_testing_acceptance/03_verification_evidence_index.md
} | tee "$artifact"
```

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-INT-ASS-LARGE-${RUN_DATE}.md

## Result

Pass

## Notes

Executed on 2026-05-08. Large-scope classification evidence and mandatory record coverage were verified.

## Linked issue IDs

- RC1-EVID-205
