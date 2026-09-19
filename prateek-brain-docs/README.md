# Prateek Brain

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `507be98f51ebece7d7c36d593786df39f0523e73`<br>
> **Source scope:** `README.md`, `docs/`, `migrations/`, `src/`, `tests/`, and `package.json`<br>
> **Documentation status:** Current

Prateek Brain is a separate, local-first personal-memory system. It observes authorized sources, creates privacy-treated and provenance-rich derived memory, supports local search and evidence-backed context assembly, and keeps original material outside the public documentation layer.

It is separate from [Prateek OS](../prateek-os-docs/README.md) because its workload and trust boundary are different: Brain owns local indexing, semantic derivation, graphing, retrieval, and memory policy; Prateek OS owns structured/live domain systems and any future integration contract. Patrick does not currently consume Brain and is not an authorization authority.

At a high level, canonical originals remain untouched; L0 registers them; L1–L5 are rebuildable derived layers; and H records durable human authority. B1 is closed. B2 is closed (B2.0–B2.7 accepted and closed). B3 is in progress: B3.0–B3.6 are complete and canonical on `main` (including the importer framework and WhatsApp adapter), with B3.7 next and not started. Operational historical-corpus rollout on `ops/b3-historical-corpus-rollout` (`f3f9caa`) is tracked separately as operational work.

## Read next

- [Brain Study Guide](brain-study-guide.md) — comprehensive architecture, privacy, retrieval, resilience, and future boundaries.
- [User Guide](user-guide.md) — the current developer/operator surface.
- [Status](STATUS.md) — canonical and in-progress state.
- [Components](components/README.md) — focused guides for the implemented memory foundation and retrieval intelligence.
