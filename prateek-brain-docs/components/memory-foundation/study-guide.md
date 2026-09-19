# Memory Foundation — Study Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `507be98f51ebece7d7c36d593786df39f0523e73`<br>
> **Source scope:** `src/{l0,l1,l2,l3,l4,l5,h,observe,watch,policy,gateway,grants,audit,provenance,queue,model,semantic,ops}/`; `migrations/`; `docs/adr/`; and `tests/`<br>
> **Documentation status:** Current

[Component home](README.md) · [User guide](user-guide.md) · [Brain-wide architecture](../../brain-study-guide.md)

## Responsibility and data model

The foundation owns the path from source registration through active memory. SQLite stores registry rows, security overrides, H instructions, immutable audit records, durable work, artifact generations, semantic objects/anchors, graph evidence, and L5 generations. Content-addressed local blobs store derived text. Forward-only migrations make the schema reproducible.

Source and artifact identities are stable across processing runs. A source move can update its locator without becoming a new memory; a duplicate remains a distinct source unless strong evidence says otherwise. Exactly one current generation per applicable source/layer is protected by transactions and indexes, while superseded generations preserve history/provenance.

## Observation algorithm

Roots are explicitly registered. Local roots combine watcher events with reconciliation; mounted roots reconcile. Discovery is bounded and root-confined, does not follow symlinks, and records unsupported/opaque files without pretending they were semantically understood.

Observation compares identity, size/time metadata, availability, and content evidence. Durable change events coalesce before fold. Large-root walks checkpoint their frontier, and missing detection waits until a full cycle completes. This prevents a time-bounded partial scan from declaring unvisited files gone.

Mounted-source trust expires on a bounded schedule, prompting selective re-verification. A missing mount or unreadable placeholder produces deferred state—not deletion. Only positive, repeated missing evidence can tombstone a source.

## Privacy and access

Bootstrap ignore/classification policy is deterministic and reviewable. Root defaults, inherited rules, and source overrides resolve into independent classification and access-policy axes. Grants provide scoped runtime authorization but never override a classification ceiling or absolute deny.

L1 performs deterministic treatment before semantic processing. Its output retains source linkage and inherits the restrictive posture. Every protected register/read/model/egress-style operation goes through the gateway and appends an audit decision. The model receives only an authorized treated representation.

## Semantic pipeline and model use

Text is chunked deterministically with offsets. The local model proposes bounded L2 JSON; schemas, relation contracts, enum bounds, and exact-quote anchors validate it. Unsupported objects/relationships are dropped. Cross-chunk merging is deterministic and preserves contradictions.

L3 summaries are hierarchical, but each atomic statement requires L2 grounding. Numeric and evidence checks prevent a fluent summary from introducing unsupported facts. Source roles exclude metadata/templates/system artifacts from authored semantic memory.

The model is a replaceable local judgment service. Deterministic code controls input, timeouts, retries, validation, provenance, and persistence. A failed model run cannot mutate canonical content or become the current projection.

## Graph, active memory, and H

L4 folds current semantic evidence into a conservative SQLite graph. Nodes, edges, edge evidence, contradictions, and supersessions are separate records. Person identity never merges on a weak label match. Relation contracts constrain what each object type may assert.

H4 can confirm/reject/correct/merge/mark-distinct/supersede through durable directives. Corrections influence regeneration rather than hand-editing derived output. Machine-generated H questions let ambiguity reach the operator, and resolution atomically records the instruction and queues the affected regeneration.

L5 ranks a bounded working set. Its current generation changes only through an atomic build/swap, so failure preserves the prior set. H5 pin, suppress, and boost affect selection while keeping the underlying evidence intact.

## Failure modes and recovery

| Failure                    | Designed behavior                                        |
| -------------------------- | -------------------------------------------------------- |
| Watcher gap                | Reconciliation catches missed changes.                   |
| Root unavailable           | Defer and report availability; do not stale descendants. |
| Partial large scan         | Resume from durable frontier; delay missing sweep.       |
| Worker crash               | Expire/reclaim leases and re-drive idempotent work.      |
| Model timeout/invalid JSON | Record bounded failure; no current invalid artifact.     |
| Refresh crash              | Preserve prior L5 generation; clean abandoned build.     |
| SQLite lock contention     | Bounded retry and sustained-deferral observability.      |
| Real contradiction         | Preserve competing evidence and expose it.               |

## Tests and performance

Tests cover migrate-from-zero, atomic H updates, policy inheritance, grants, append-only audit, path confinement, source identity, watcher/reconcile convergence, mounted-source outage/return, deterministic L1 treatment, chunking, anchors, grounding, graph evidence and identity, L5 budget/swap, multiprocess queue races, injected process death, and supervision health.

Synthetic golden data protects extraction and graph quality without exposing personal memory. Fake model clients make failure cases deterministic; separate accepted evidence covers the loopback local model. Performance work includes set-based coalescing, selective event emission, content-size limits, cached glob compilation, schema indexes, bounded drains, resource probes, and resumable traversal.

## Tradeoffs

SQLite and the filesystem minimize operational surface and keep data local, but demand careful single-host concurrency and backup discipline. Extensive provenance costs storage, but enables trust and regeneration. Conservative identity and relation validation can reduce recall; the system intentionally prefers missing a link to fabricating one. Keeping generations supports audit/recovery but creates retention debt that must be addressed only with a provenance-safe policy.
