# Prateek Brain — User Guide

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `78324967005cccd0b36147e82647f8ef464e8918`<br>
> **Source scope:** `src/cli/`; `src/{ops,observe,semantic,l4,l5,search,claims,context}/`; `package.json`; and `tests/`<br>
> **Documentation status:** Current

[Documentation home](README.md) · [Brain study guide](brain-study-guide.md) · [Status](STATUS.md)

Brain is currently a local developer/operator tool, not a polished consumer application. Commands report controlled results and aggregate health; they should not be copied into public logs when output could contain memory-derived text.

## Setup and safety boundary

Use the repository's supported Node and pinned pnpm toolchain, including `pnpm install --frozen-lockfile`. Install dependencies only in the private implementation checkout, configure the local data root and local Ollama service according to private repository instructions, and never commit environment files, indexes, databases, logs, source inventories, or derived memory.

Examples below use a synthetic root and query:

```sh
node src/cli/brainctl.ts root add /path/to/synthetic-notes --class LOCAL_ONLY --policy AUTOMATIC
node src/cli/brainctl.ts root list
node src/cli/brainctl.ts preflight
```

Root configuration is a security decision. `root add` enables the new root immediately, so review its path, classification, access policy, and ignore/bootstrap policy before running the command. Start narrowly, and do not add a real source merely to test documentation.

## Observe and reconcile

```sh
node src/cli/brainctl.ts reconcile /path/to/synthetic-notes
node src/cli/brainctl.ts observe /path/to/synthetic-notes
node src/cli/brainctl.ts status
```

`reconcile` discovers differences and advances durable work. `observe` performs a bounded reconcile/fold/L1 pass for a root. Background supervision combines observation with semantic, graph, active-memory, and search reconciliation.

If storage is unavailable, the correct result is deferral and an availability signal. Brain does not interpret absence as deletion. Restore the source and reconcile again; unchanged items should not regenerate, while actual changes flow through their dependent layers.

## Local semantic processing

```sh
node src/cli/brainctl.ts model status
node src/cli/brainctl.ts semantic status
node src/cli/brainctl.ts semantic run --max 10
node src/cli/brainctl.ts graph run
node src/cli/brainctl.ts graph status
```

Semantic work uses the configured loopback Ollama model. A missing, unhealthy, timed-out, unauthorized, or resource-deferred model run leaves durable work for retry; it must not create an unvalidated current artifact. `--force` is an explicit operator control and should not be used to bypass resource or privacy policy.

Graph inspection can use an exact synthetic label, and contradiction reports are bounded:

```sh
node src/cli/brainctl.ts graph node "Project Juniper"
node src/cli/brainctl.ts graph contradictions --limit 10
```

## Search

Before first use or after a deliberate rebuild, inspect search health and use bounded backfill/reconciliation:

```sh
node src/cli/brainctl.ts search status
node src/cli/brainctl.ts search backfill --max-batches 2
node src/cli/brainctl.ts search reconcile
node src/cli/brainctl.ts search "fictional project decision" --level l1,l2,l3 --limit 5
```

Search returns derived results with level and provenance context. Treat rank as relevance ordering, not truth. Open the evidence before acting on an inferred, contradicted, uncertain, or historical result.

## Claims and evidence

```sh
node src/cli/brainctl.ts claims list --state current --limit 10
node src/cli/brainctl.ts claims show 'claim:stmt:<synthetic-id>' --security
```

A claim is compiled from current evidence rather than manually curated canonical memory. Read its derivation, support, contradictions, temporal state, and security. A direct claim has stronger provenance than an inference but can still be superseded. H corrections should use the supported H workflow; do not patch derived database rows.

## Context compilation

```sh
node src/cli/brainctl.ts context "What changed in fictional Project Juniper?" --budget 12 --history change
```

The compiler creates a deterministic, bounded evidence packet. History modes distinguish current, historical, and change-oriented retrieval. The packet is not a final answer and does not grant egress permission. Review omitted/budget information and contradiction/supersession sections before passing it to another local tool.

## Health and supervision

```sh
node src/cli/brainctl.ts health
node src/cli/brainctl.ts supervise once
node src/cli/brainctl.ts agents status
```

Health summarizes roots, queues, accounting, semantic/model state, graph/search state, and supervision signals. Investigate sustained stalls, hard failures, accounting gaps, or repeated lock deferrals. A backlog by itself can be normal; loss of progression or safety invariants is not.

The repository can install or remove macOS launch agents, but those commands change local system state. Use them only during an explicitly authorized operational setup or maintenance window. This public guide does not prescribe private labels, paths, or schedules.

## Failure behavior

- **Source unavailable:** defer; do not tombstone or stale descendants.
- **Permission denied:** gateway returns deny or authorization-required and audits the decision.
- **Model unavailable or invalid output:** fail/retry the derived run; do not persist unsupported current memory.
- **Worker crash:** leases expire and work can be reclaimed; generation/swap protects the last valid current projection.
- **SQLite contention:** bounded retries and lock-deferral metrics expose sustained contention.
- **Search drift:** run bounded reconcile/backfill; search remains derived and rebuildable.
- **Contradiction:** preserve both evidence paths and expose the conflict rather than choosing silently.

## What Brain does not currently provide

There is no polished UI, MCP server, Patrick/Discord interface, OS runtime integration, historical importer framework, production embedding index, dedicated graph database, or required cloud-model service. Accepted B2.6 work is an experimental/manual boundary on canonical `main`, not a normal operator command or production dependency; B3 contains the MCP/importer direction. Patrick cannot currently query Brain and would not become an authorization authority if integration is later built.
