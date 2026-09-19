# Tech News Radar — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/news-radar/`; N1 paths in `apps/discord-bot/`; `supabase/migrations/`; `docs/adr/0013-n1-tech-news-radar.md`; `docs/adr/0019-n1-centralized-patrick-runtime-and-replay-readiness.md`; and `docs/reviews/news-radar/N1/`<br>
> **Documentation status:** Current

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

> **IMPLEMENTED · HOSTED DEV.** Formally closed and merged into canonical `main` on 2026-09-13 (`1b6e746`/`586d736`). Centralized Patrick runtime on Railway (`patrick-gateway`), Replay Lab, and durable rolling judgment budget are live on hosted DEV. Structured data is housed in a dedicated News Radar Supabase project under the OS4 multi-plane architecture.

## What it does

Tech News Radar turns bounded public tech-news sources into a deduplicated, clustered, personalized editorial stream. It separates a broad browsable firehose from a curated radar, rare breaking alerts, a daily digest, and an editorial story-selection desk.

## Where I will use it

| Channel          | Use                                                                        | Current status          |
| ---------------- | -------------------------------------------------------------------------- | ----------------------- |
| `#tech-firehose` | What N1 is seeing after validation/dedupe, before personalized suppression | **Hosted DEV · Active** |
| `#tech-radar`    | Primary concise curated story stream                                       | **Hosted DEV · Active** |
| `#tech-breaking` | Rare major interrupt/promotions                                            | **Hosted DEV · Active** |
| `#tech-digest`   | Daily compact editorial checkpoint                                         | **Hosted DEV · Active** |
| `#tech-desk`     | Pipeline status and released pointer outlines                              | **Hosted DEV · Active** |

## Common workflows (hosted DEV)

### Read and react

- ✅ on Firehose/Radar/Breaking: positive preference evidence.
- ❌: bounded negative preference evidence. It does not delete the story, blacklist a topic, or override globally major news.

### Submit a missed story

Pasting a URL in `#tech-radar` or `#tech-breaking` is intended to store it, treat it as positive evidence, associate it when possible, and record an honest coverage-gap diagnosis. X/Reddit URLs may be stored but are not directly fetched.

### Flag important/covering

The `/news-flag` control marks canonical story/topic state such as explicitly important or already being covered. It is separate from ✅/❌ preference semantics.

### Manage sources

The `/news-source` command lists/enables/disables bounded catalog sources for the authorized owner. The handler is registered and active on hosted DEV.

## Commands / reactions / controls

The controls are ✅/❌ preference reactions, 🎬/🧵 editorial-selection reactions, `/news-source`, `/news-flag`, and `/news-pipeline`. They are active on hosted DEV.

## Editorial Story Pipeline

### Select stories

- Add 🎬 to an eligible Firehose/Radar/Breaking story to create/use the **primary** pipeline.
- Add 🧵 to select into the current **secondary-active** pipeline.
- If there is no secondary-active pipeline, 🧵 does nothing and should be removed; it never creates the primary.
- Remove your reaction to remove only that selection.

### Commands

The hosted-DEV `/news-pipeline` command defines:

| Command                     | Intended result                                                                 |
| --------------------------- | ------------------------------------------------------------------------------- |
| `status`                    | Show primary, secondary, and stashes with counts.                               |
| `stash [name]`              | Preserve primary selections and free the primary slot.                          |
| `secondary-activate <name>` | Make a dormant stash the active secondary; preserve/demote the prior secondary. |
| `reset-primary`             | Fully clear primary selections/reactions and leave a tombstone.                 |
| `reset-secondary`           | Fully clear the secondary-active pipeline.                                      |
| `trim-primary`              | Remove only old primary selections using a deterministic cutoff.                |
| `release-primary`           | Freeze and release primary into a pointer outline.                              |
| `release-secondary`         | Freeze and release secondary into a pointer outline.                            |

### Stash, secondary, reset, trim, release

- **Stash** saves the current rundown and allows a new first 🎬 to start another primary.
- **Secondary activation** lets 🧵 add stories to one chosen stash while 🎬 continues feeding primary.
- **Reset** is destructive to active selections but leaves durable history; primary and secondary reset independently.
- **Trim** is primary-only and removes selections older than a resolved cutoff, based on when selected.
- **Release** groups related selected evidence into story arcs and produces a pointer outline: real links, grounded synopsis, sure-to-mention points, optional points, and follow-ups. It is not a spoken script.

If optional model assistance fails or invents a link, release falls back to a deterministic evidence-preserving outline. A failure before artifact persistence leaves the pipeline intact for retry. Duplicate releases are fenced.

## Examples

- React 🎬 on three related updates and two unrelated stories: release should group the three updates into one arc and retain the other two separately.
- Stash the primary as `launch-week`; the next 🎬 creates a new primary.
- Activate `launch-week` as secondary, then use 🧵 to add follow-ups while 🎬 feeds the new primary.
- Paste a missed story in Radar: N1 should record preference and diagnose whether it was unseen, rejected, under-ranked, or already surfaced—without inventing a cause.

## What happens behind the scenes

```text
bounded public feeds
  → normalize + canonicalize + strong dedupe
  → developing-story clustering
  → deterministic features/filters
  → optional grounded editorial judgement
  → idempotent Discord surfaces
  → replay-safe feedback and story selections
```

Canonical evidence remains even if ranking/model/delivery fails. Full article bodies are not stored merely for convenience.

## What statuses mean

| Status           | Meaning                                                                |
| ---------------- | ---------------------------------------------------------------------- |
| Firehose         | Accepted ingestion evidence before personalized editorial suppression. |
| Radar            | Curated concise story.                                                 |
| Breaking         | Rare major development or explicit owner escalation.                   |
| Stashed          | Preserved dormant editorial selection set.                             |
| Active primary   | Current 🎬 target.                                                     |
| Active secondary | Current 🧵 target.                                                     |
| Released         | Immutable outline artifact persisted; selections cleared terminally.   |
| Reset            | Terminally cleared pipeline with durable history.                      |

## When something goes wrong

The intended safe behavior is:

- source failure is isolated and retried/cooldown-managed;
- duplicate story delivery is suppressed per surface;
- ungrounded/model failure keeps canonical evidence and falls back;
- ineffective 🧵 is removed;
- stale/duplicate release is fenced;
- failed release before persistence keeps all selections.

## Current limitations

- Hosted DEV was activated, corrected, and reverified; batched owner acceptance remains pending.
- No direct X or Reddit integration/scraping.
- Known source coverage gaps and small evaluation corpus.
- Digest scheduling and an initial Breaking promotion have runtime evidence, but quality needs longer owner observation.
- No owner-accepted real `#tech-desk` release.
- No auto-script, auto-post, or auto-publication.

## Planned improvements / closeout work

The current milestone needs real owner feedback/source/flag/manual-submission acceptance, digest-quality observation, one bounded real editorial-pipeline release, final milestone review, merge decision, and closeout reconciliation.
