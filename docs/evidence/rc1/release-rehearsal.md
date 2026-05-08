---
title: RC1 Evidence - Release Rehearsal
status: active
record_class: supporting
audience: [internal, manager]
owner: release-manager
capability: execution
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Run ID

RC-1-REL-REHEARSAL

## Date

2026-05-15 (planned)

## Branch/Revision

main @ 95eda1f (planning baseline)

## Executor

Release manager + operations lead

## Environment

Staging-like workspace

## Command or workflow hint

```bash
cd /home/hexaper/project-bootstrap-cli
RUN_DATE="${RUN_DATE:-$(date +%F)}"
artifact="docs/evidence/rc1/artifacts/RC1-REL-REHEARSAL-${RUN_DATE}.md"
work="/tmp/rc1-release-rehearsal"
rm -rf "$work"
mkdir -p "$(dirname "$artifact")" "$work/pre" "$work/post" "$work/rollback"
cp -a docs "$work/pre/docs"
tar -czf "$work/pre/docs-pre.tgz" -C "$work/pre" docs
cp -a "$work/pre/docs" "$work/post/docs"
printf '\nrehearsal_marker: rc1\n' >> "$work/post/docs/07_delivery/07_release_plan.md"
tar -xzf "$work/pre/docs-pre.tgz" -C "$work/rollback"
{
	echo "# RC-1-REL-REHEARSAL"
	echo "## Rehearsal package"; ls -lh "$work/pre/docs-pre.tgz"
	echo "## Rollback parity check"
	diff -qr "$work/pre/docs" "$work/rollback/docs" || true
} | tee "$artifact"
```

Pass/fail interpretation: `diff -qr ... || true` prevents early exit to preserve artifact capture. Treat any `diff` output lines as rollback parity failure; no `diff` output indicates pass.

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-REL-REHEARSAL-${RUN_DATE}.md

## Result

Planned

## Notes

Planned run; record timeline, gate outcomes, rollback decision checkpoint, and follow-up actions.

## Linked issue IDs

- RC1-EVID-211
