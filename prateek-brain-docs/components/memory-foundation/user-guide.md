# Memory Foundation — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `507be98f51ebece7d7c36d593786df39f0523e73`<br>
> **Source scope:** `src/cli/brainctl.ts`; `src/{l0,observe,semantic,l4,l5,ops,policy,gateway}/`; `docs/SECURITY_MODEL.md`; and `tests/`<br>
> **Documentation status:** Current

[Component home](README.md) · [Study guide](study-guide.md) · [Brain operator guide](../../user-guide.md)

## Practical workflow

Use only approved synthetic or private local sources. This public example intentionally contains no real inventory:

```sh
node src/cli/brainctl.ts root add /path/to/synthetic-notes --class LOCAL_ONLY --policy AUTOMATIC
node src/cli/brainctl.ts preflight
node src/cli/brainctl.ts observe /path/to/synthetic-notes
node src/cli/brainctl.ts model status
node src/cli/brainctl.ts semantic run --max 10
node src/cli/brainctl.ts graph run
node src/cli/brainctl.ts status
node src/cli/brainctl.ts graph status
node src/cli/brainctl.ts health
```

`root add` enables the root immediately. Review the path, classification, access policy, and ignore/bootstrap rules before adding it. Treat preflight failures as blockers. Aggregate status is safe for local diagnosis, but do not paste source lists, labels, paths, model context, or memory-derived output into a public issue.

## Reading status

- A pending semantic queue may simply mean local processing is draining.
- `temporarily_unavailable` means retry when storage returns; do not disable/delete the root to “fix” it.
- Sustained lack of queue progress, hard health failures, or accounting gaps require investigation.
- A model readiness failure should defer semantic work while deterministic observation continues where safe.
- Graph contradictions are information to review, not database corruption.

## Correcting memory

Use supported H instructions and question-resolution flows. Never edit L2–L5 tables or blob output directly: the next regeneration would erase the patch and its provenance would be false. H controls are durable and can target the appropriate layer.

## Supervision

`brainctl supervise once` runs a bounded cycle. `brainctl agents status` inspects configured macOS supervision. Installing/uninstalling agents changes local machine state and requires an explicit operator decision. If supervision is stopped, durable queues remain; resume and reconcile rather than rebuilding blindly.

## Limitations

This component has no graphical source browser or review UI. Root setup, H correction ergonomics, and health investigation are CLI/developer workflows. Search and context workflows are documented in [Retrieval Intelligence](../retrieval-intelligence/README.md).
