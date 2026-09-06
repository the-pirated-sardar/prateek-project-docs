# Tech News Radar

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** in-progress feature snapshot `c111af6770bc2197ce35b2cb76179356911b39ae` (corrected code `2bd34e3`); canonical `main` `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`<br>
> **Source scope:** `services/news-radar/`; N1 paths in `apps/discord-bot/`; `supabase/migrations/`; `docs/adr/0013-n1-tech-news-radar.md`; and `docs/reviews/news-radar/N1/`<br>
> **Documentation status:** In progress

Tech News Radar is the N1 editorial tech-news system. Its intended job is to turn bounded public-source observations into a deduplicated, clustered, personalized editorial stream, while preserving evidence and keeping editorial selection distinct from automatic publication.

## Status

**IN PROGRESS — HOSTED DEV / CORRECTION VERIFIED / OWNER ACCEPTANCE PENDING.** Hosted DEV was activated from `41007e5`, and recurring surfaces/controls were exercised. Independent review then found user-visible defects; corrections through `2bd34e3` passed delta review and hosted reactivation/reverification, recorded through `c111af6`. N1 is not merged into canonical `main`, deployed to PROD, owner-accepted, or formally closed.

## What to read

| Document                                  | Best for                                                                                                                                  |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | In-progress architecture, canonicalization, clustering, editorial ranking, feedback, delivery, pipeline design, testing, and limitations. |
| [User Guide](user-guide.md)               | Intended channels, reactions, controls, Editorial Story Pipeline workflows, and current non-live constraints.                             |
| [OS Study Guide](../../os-study-guide.md) | Shared canonical-state, model-boundary, scheduling, and delivery principles.                                                              |

## In this system

- a broad Firehose after validation and deduplication;
- a concise editorial Radar, rare Breaking surface, and daily Digest;
- developing-story clustering and deterministic features before optional grounded editorial judgement;
- replay-safe feedback that records bounded preference evidence;
- missed-story submission and coverage-gap diagnosis;
- an Editorial Story Pipeline for primary/secondary selection, stashing, trimming, resetting, and evidence-grounded pointer-outline release.

The hosted-DEV acceptance environment exists and the correction cycle is verified; batched owner acceptance remains before milestone closeout. Release is designed to produce an editorial outline rather than a spoken script or automatic publication.

## Related systems

- [Platform / Core](../platform-core/README.md) supplies the shared persistence, security, scheduling, and Discord conventions N1 follows.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
