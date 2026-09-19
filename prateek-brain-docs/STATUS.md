# Prateek Brain Status

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-brain`<br>
> **Source baseline:** canonical `main` `507be98f51ebece7d7c36d593786df39f0523e73`<br>
> **Source scope:** Git refs; `docs/{PROJECT,ROADMAP,B1_SCOPE,B2_SCOPE,B3_SCOPE}.md`; `docs/adr/`; `docs/reviews/`; `migrations/`; `src/`; and `tests/`<br>
> **Documentation status:** Current

- **Repository truth:** canonical `main` is at `507be98f51ebece7d7c36d593786df39f0523e73` (where B3.6 is complete and the WhatsApp importer is merged).
- **B1 — Foundation:** closed. Observation, privacy treatment, provenance, L0–L5/H, differential regeneration, access control, local-model processing, broad-index operations, and supervision are accepted.
- **B2 — Retrieval and intelligence:** closed. B2.0–B2.7 accepted and closed out. Includes runtime hardening, SQLite FTS5 search, evidence-backed claims, deterministic context compilation, and the B2.5 architecture decision to defer production embeddings and retain SQLite for graph storage.
- **B3 — MCP and importer framework:** in progress. B3.0 through B3.6 are complete and merged to canonical `main`. B3.7 is next and not started. Operational historical-corpus rollout on `ops/b3-historical-corpus-rollout` (`f3f9caa`) is tracked separately as operational work and does not block B3 milestone progression.
- **Inter-project boundary:** no current Patrick, Discord, or polished end-user UI integration exists. Brain remains a separate, local-first system.
- **Runtime:** source authority records local background observation/indexing under macOS supervision. Public documentation intentionally omits source inventories, private counts, paths, memory contents, and machine details.
- **Next reconciliation trigger:** B3.7 completes or closes; B4 begins; an access-policy or schema contract changes; or accepted runtime/deployment status changes.
