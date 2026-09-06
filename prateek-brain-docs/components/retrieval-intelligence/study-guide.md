# Retrieval Intelligence — Study Guide

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `78324967005cccd0b36147e82647f8ef464e8918`<br>
> **Source scope:** `src/{search,claims,context,l4,l5}/`; `migrations/0009_b2_2_search_baseline.sql`; `docs/{B2_RETRIEVAL_CONTRACT,B2_CLAIMS_CONTRACT,B2_CONTEXT_COMPILER_CONTRACT,B2_BENCHMARK_DESIGN}.md`; `docs/adr/0008-b2.5-embeddings-graph-decision.md`; `docs/reviews/b2/`; and `tests/`<br>
> **Documentation status:** Current

[Component home](README.md) · [User guide](user-guide.md) · [Brain-wide architecture](../../brain-study-guide.md)

## Retrieval pipeline

```mermaid
flowchart LR
  Q[Query] --> T[Taxonomy and filters]
  T --> F[FTS5 candidates]
  F --> A[Live access evaluation]
  A --> R[Deterministic ranking]
  R --> C[Claims compilation]
  C --> G[Graph, H, L5 expansion]
  G --> B[Budgeted context packet]
```

Each stage narrows or enriches evidence without weakening the Memory Foundation's contracts. Results and packets remain derived, provenance-bearing, and security constrained.

## Search index and reconciliation

Migration 0009 adds an FTS5 table, a document mapping table, and per-source index state. The index covers three current derived representations: treated L1 text, L2 object labels, and L3 statement text. Raw originals are not directly indexed, and graph/L5 rows are not duplicated into FTS.

Search reconciliation compares the current L1/L2/L3 artifact identifiers to indexed state. It deletes obsolete units and inserts replacements transactionally, enabling bounded initial backfill and differential maintenance. Source tombstones and restoration converge through the same reconciliation rather than bespoke query-time exceptions.

Candidate rows cache class/policy for filtering efficiency, but a live gateway evaluation is authoritative. This closes a common security bug: an index created under old policy cannot continue leaking a result after access changes.

## Ranking and result contract

Ranking starts from deterministic lexical relevance, then applies explicit structured signals and filters. Results identify source, layer, reference kind, artifact/generation, text/snippet, rank explanation, provenance, security decision, and temporal/history context where applicable.

The query taxonomy includes exact/factual, paraphrase/conceptual, temporal, source-scoped, relationship, contradiction/supersession, and history/change questions. One algorithm need not dominate every class; benchmark reports therefore break quality down by class instead of hiding weakness in one aggregate.

Ranking does not equal confidence and confidence does not equal authority. A high lexical result may be inferred or superseded. Consumers must inspect derivation and evidence.

## Claims compilation

Claims are generated at read time from current semantic statements, graph edges/evidence, contradictions, supersession, and H state. A claim presents a normalized unit with:

- kind and subject/predicate/object or statement text;
- direct, inferred, or human-confirmed derivation;
- support and contradiction references;
- current, superseded, contradicted, or otherwise bounded state;
- temporal validity/uncertainty where evidence supports it;
- provenance and effective security.

Because the view is compiled, it cannot become stale independently from its inputs. The tradeoff is query cost. Current measured scale does not justify a claim table; materialization remains trigger-based future work.

Functional relationship rules and conservative supersession handling reduce false certainty. For example, an inferred “supersedes” relation can be recorded as evidence without automatically retiring its target; stronger direct or human-confirmed evidence is required for the state change.

## Context compiler

The compiler accepts a query, explicit structured-item budget, history mode, and filters. It retrieves evidence, compiles claims, expands relevant graph/H/L5 context, orders candidates by priority, and emits a structured packet with selected and omitted material.

Budgeting is deterministic and whole-item aware. Critical human authority, contradictions, and direct evidence outrank decorative context. Graph emission is priority-aware, not discovery-order dependent. Repeat runs against unchanged state should produce identical packets.

The packet maintains traceability to source/artifact references and keeps contradictions/supersession visible. It is not free-form synthesis and does not resolve ambiguity by generating a plausible story. It is designed as safe input for a later authorized consumer.

## Security and privacy

Search, claims, and context inherit the most restrictive support. The live gateway mediates protected reads; any future egress additionally requires classification permission and an `EGRESS_GRANT`. Higher abstraction never means safer by default.

CLI output can itself contain private derived material. Operators should avoid public terminals/logs and never use real query output as a documentation example. Tests use synthetic names and facts.

## Embeddings and graph decision

B2.5 compared FTS with local embedding and hybrid variants on synthetic query classes. Embeddings improved paraphrase/conceptual recall but left real-corpus scale and adversarial false positives unresolved, so production adoption was deferred. This is a measured “not yet,” not a claim that embeddings have no value.

Representative SQLite graph traversals remained comfortably within the decision threshold, so a dedicated graph database was not adopted. Retaining one local datastore avoids synchronization, policy duplication, and operational complexity.

## Tests

Search tests cover migration/backfill, incremental replacement, tombstone/restore convergence, access-policy changes, deterministic rank, filters, CLI behavior, and benchmark invariants. Claims tests cover direct/inferred distinctions, support, contradiction, supersession, identity conservatism, security, stable IDs, and CLI views. Context tests cover deterministic repeatability, budgets, history modes, graph/H/L5 priority, provenance, omission reporting, security ceilings, and command output.

Benchmark and golden fixtures are synthetic. Model fakes isolate retrieval logic; search/claims/context production behavior is predominantly deterministic and does not require a live model call at query time.

## Failure modes and tradeoffs

- A stale/missing FTS row is repaired by reconcile/backfill; the index is never canonical.
- A denied result is filtered and audited even if its index snapshot is stale.
- A tight budget reports omissions rather than silently truncating evidence into misleading fragments.
- Contradictory evidence remains visible; callers must not collapse it to one answer.
- Lexical search misses some conceptual matches; accepted architecture favors a known deterministic baseline until embedding risks are resolved.
- Query-time claims avoid staleness but may need materialization if measured cost crosses the documented trigger.
- SQLite simplifies local consistency; a new store requires demonstrated scale need and a security/provenance design.
