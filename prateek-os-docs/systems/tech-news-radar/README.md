# Tech News Radar

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/news-radar/`; N1 paths in `apps/discord-bot/`; `supabase/migrations/`; `docs/adr/0013-n1-tech-news-radar.md`; `docs/adr/0019-n1-centralized-patrick-runtime-and-replay-readiness.md`; and `docs/reviews/news-radar/N1/`<br>
> **Documentation status:** Current

Tech News Radar is the N1 editorial tech-news system. It turns bounded public-source observations into a deduplicated, clustered, personalized editorial stream, preserving evidence and keeping editorial selection distinct from automatic publication.

## Status

**IMPLEMENTED · HOSTED DEV.** Formally closed and merged into canonical `main` on 2026-09-13 (`1b6e746`/`586d736`). Centralized Patrick runtime on Railway (`patrick-gateway`), Replay Lab, and durable rolling judgment budget are live on hosted DEV. Structured data is housed in a dedicated News Radar Supabase project under the OS4 multi-plane architecture.

## What to read

| Document                                  | Best for                                                                                                                         |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Architecture, canonicalization, clustering, editorial ranking, feedback, delivery, Replay Lab, pipeline design, and limitations. |
| [User Guide](user-guide.md)               | Channel surfaces, reactions, controls, and Editorial Story Pipeline workflows on hosted DEV.                                     |
| [OS Study Guide](../../os-study-guide.md) | Shared canonical-state, model-boundary, multi-plane persistence, scheduling, and delivery principles.                            |

## In this system

- a broad Firehose after validation and deduplication;
- a concise editorial Radar, rare Breaking surface, and daily Digest;
- developing-story clustering and deterministic features before optional grounded editorial judgement;
- durable rolling judgment budget and attempt leasing for chargeable model calls;
- replay-safe feedback that records bounded preference evidence;
- Replay Lab for offline replay without live table or lease mutations;
- missed-story submission and coverage-gap diagnosis;
- an Editorial Story Pipeline for primary/secondary selection, stashing, trimming, resetting, and evidence-grounded pointer-outline release.

Release is designed to produce an editorial outline rather than a spoken script or automatic publication.

## Related systems

- [Platform / Core](../platform-core/README.md) supplies the shared persistence, security, scheduling, and Discord conventions N1 follows.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
