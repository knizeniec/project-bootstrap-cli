---
title: RC1 Evidence - Integration Review Baseline
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

RC-1-INT-REV

## Date

2026-05-12 (planned)

## Branch/Revision

main @ 95eda1f (planning baseline)

## Executor

QA lead

## Environment

Local Linux workspace using repository docs set

## Command or workflow hint

```bash
cd /home/hexaper/project-bootstrap-cli
RUN_DATE="${RUN_DATE:-$(date +%F)}"
artifact="docs/evidence/rc1/artifacts/RC1-INT-REV-${RUN_DATE}.md"
mkdir -p "$(dirname "$artifact")"
{
	echo "# RC-1-INT-REV baseline findings"
	echo "## markdownlint output"
	git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2 || true
	echo "## docs validator output"
	PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs/evidence/rc1 || true
	echo "## Evidence docs headings check"
	rg -n "^## (Run ID|Result|Notes|Linked issue IDs)$" docs/evidence/rc1/*.md
} | tee "$artifact"
```

Pass/fail interpretation: `|| true` keeps artifact generation running even if checks fail. Treat any markdownlint error output or docs-validator error output in this artifact as a failed check.

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-INT-REV-${RUN_DATE}.md

## Result

Planned

## Notes

Planned baseline review to confirm style, completeness, and evidence-link conventions before RC1 gate.

## Linked issue IDs

- RC1-EVID-207
