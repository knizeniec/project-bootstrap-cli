---
title: RC1 Evidence - Integration Generation
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

RC-1-INT-GEN

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
artifact="docs/evidence/rc1/artifacts/RC1-INT-GEN-${RUN_DATE}.md"
out="/tmp/rc1-int-generation"
rm -rf "$out"
mkdir -p "$(dirname "$artifact")" "$out"
rsync -a --include='*/' --include='*.md' --exclude='*' docs/ "$out/docs/"
{
	echo "# RC-1-INT-GEN generated docs set"
	echo "## Generated files"
	find "$out/docs" -type f -name '*.md' | sort
	echo "## File count"
	find "$out/docs" -type f -name '*.md' | wc -l
} | tee "$artifact"
```

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-INT-GEN-${RUN_DATE}.md

## Result

Planned

## Notes

Planned generation run to verify required sections are emitted and naming conventions are preserved.

## Linked issue IDs

- RC1-EVID-206
