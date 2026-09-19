# Retrieval Intelligence — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `507be98f51ebece7d7c36d593786df39f0523e73`<br>
> **Source scope:** `src/cli/brainctl.ts`; `src/{search,claims,context,l4,l5}/`; `migrations/0009_b2_2_search_baseline.sql`; and `tests/`<br>
> **Documentation status:** Current

[Component home](README.md) · [Study guide](study-guide.md) · [Brain operator guide](../../user-guide.md)

## Prepare and inspect search

```sh
node src/cli/brainctl.ts search status
node src/cli/brainctl.ts search backfill --max-batches 2
node src/cli/brainctl.ts search reconcile
```

Backfill is bounded and resumable. Reconciliation is safe to repeat because current artifact IDs determine what needs replacement. Investigate persistent queue failure or drift; do not edit FTS tables directly.

## Search safely

Use synthetic examples in shared material:

```sh
node src/cli/brainctl.ts search "Project Juniper decision" --level l1,l2,l3 --limit 5
```

Narrow by level, source, or time only when it matches the question. A result is a lead into evidence. Review its layer, source/artifact provenance, derivation, and state before relying on it.

## Inspect claims

```sh
node src/cli/brainctl.ts claims list --state current --limit 10
node src/cli/brainctl.ts claims show 'claim:stmt:<synthetic-id>' --security
```

Look for support, contradictions, supersession, temporal bounds, direct/inferred status, and security. Claims are regenerated views; correction belongs in H or the underlying source, not in a claim row.

## Compile context

```sh
node src/cli/brainctl.ts context "What changed in fictional Project Juniper?" --budget 12 --history change
```

Choose `current`, `history`, or `change` according to intent. Review the packet's omissions and budget use. A compiled packet is not a final answer or authorization: do not send it to any external model unless the classification ceiling, access policy, and explicit egress authorization all allow that separate action.

## Expected failures and limitations

- No result can mean no indexed matching evidence, a restrictive filter, unavailable derived state, or denied access—not proof the fact is false.
- A contradicted/superseded result is intentionally preserved for history and should not be quoted as current without qualification.
- Tight budgets omit lower-priority context.
- Search is lexical; production embeddings are deferred.
- There is no UI, Patrick integration, MCP endpoint, or cloud search service.
- Accepted B2.6 external comparison/audit seams are experimental/manual on canonical `main` and are not exposed as operational commands or a production workflow.
