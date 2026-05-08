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
artifact="docs/evidence/rc1/artifacts/RC1-INT-REV-${RUN_DATE}.md"
mkdir -p "$(dirname "$artifact")"
{
	echo "# RC-1-INT-REV baseline findings"
	echo "## markdownlint output"
	git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2 || true
	echo "## docs validator output"
	if [ -d tools/docs_validator/src ]; then
		PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs/evidence/rc1 || true
	else
		echo "docs_validator unavailable in this repository; using markdownlint and heading checks for this run."
	fi
	echo "## Evidence docs headings check"
	grep -RInE "^## (Run ID|Result|Notes|Linked issue IDs)$" docs/evidence/rc1/*.md
} | tee "$artifact"
```

Pass/fail interpretation: `|| true` keeps artifact generation running even if checks fail. Treat any markdownlint error output or docs-validator error output in this artifact as a failed check.

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-INT-REV-${RUN_DATE}.md

## Result

Pass with accepted risk

## Notes

Executed on 2026-05-08. Markdownlint reported repository-wide markdown debt outside the RC1 evidence scope; docs validator module is not present in this repository. Accepted risk for this P1 check with follow-up due 2026-05-15.

Risk decision link: [risk-acceptance-int-rev.md](risk-acceptance-int-rev.md)

## Linked issue IDs

- RC1-EVID-207
