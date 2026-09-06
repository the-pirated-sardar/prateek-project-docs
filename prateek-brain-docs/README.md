# Prateek Brain

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `78324967005cccd0b36147e82647f8ef464e8918` (includes B2.6 acceptance records; corrected B2.6 implementation `2197ec7`); B2.7 in-progress evidence snapshot `721bd74ef9d6ad2a423626691361ad57d7755ad4`<br>
> **Source scope:** `README.md`, `docs/`, `migrations/`, `src/`, `tests/`, and `package.json`<br>
> **Documentation status:** Mixed current/future

Prateek Brain is a separate, local-first personal-memory system. It observes authorized sources, creates privacy-treated and provenance-rich derived memory, supports local search and evidence-backed context assembly, and keeps original material outside the public documentation layer.

It is separate from [Prateek OS](../prateek-os-docs/README.md) because its workload and trust boundary are different: Brain owns local indexing, semantic derivation, graphing, retrieval, and memory policy; Prateek OS owns structured/live domain systems and any future integration contract. Patrick does not currently consume Brain and is not an authorization authority.

At a high level, canonical originals remain untouched; L0 registers them; L1–L5 are rebuildable derived layers; and H records durable human authority. B1 is closed. B2.0–B2.6 are accepted; B2.6 accepted its corrected experimental boundary at `2197ec7`, with acceptance records at `7832496`, now included in local canonical `main`. It adds no production dependency or live external workflow. B2.7 has recorded implementation-side scale/closeout evidence on an unmerged feature snapshot, with independent closeout review pending and no production-code change. B3 remains planned.

## Read next

- [Brain Study Guide](brain-study-guide.md) — comprehensive architecture, privacy, retrieval, resilience, and future boundaries.
- [User Guide](user-guide.md) — the current developer/operator surface.
- [Status](STATUS.md) — canonical and in-progress state.
- [Components](components/README.md) — focused guides for the implemented memory foundation and retrieval intelligence.
