# Tech News Radar — User Guide

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

> **IN PROGRESS — LOCAL / NOT YET FORMALLY ACCEPTED. None of the workflows below should be treated as usable today.** They describe the intended, locally implemented N1 snapshot. N1 documentation reflects an in-progress implementation snapshot and will be reconciled after milestone closeout.

## What it does

Tech News Radar is intended to turn bounded public tech-news sources into a deduplicated, clustered, personalized editorial stream. It will separate a broad browsable firehose from a curated radar, rare breaking alerts, a daily digest, and an editorial story-selection desk.

## Where I will use it

| Channel          | Intended use                                                               | Current status                             |
| ---------------- | -------------------------------------------------------------------------- | ------------------------------------------ |
| `#tech-firehose` | What N1 is seeing after validation/dedupe, before personalized suppression | **IN PROGRESS; not live**                  |
| `#tech-radar`    | Primary concise curated story stream                                       | **IN PROGRESS; not live**                  |
| `#tech-breaking` | Rare major interrupt/promotions                                            | **IN PROGRESS; not live**                  |
| `#tech-digest`   | Daily compact editorial checkpoint                                         | **IN PROGRESS; not live**                  |
| `#tech-desk`     | Pipeline status and released pointer outlines                              | **IN PROGRESS; no real release delivered** |

## Common workflows (intended)

### Read and react

- ✅ on Firehose/Radar/Breaking: positive preference evidence.
- ❌: bounded negative preference evidence. It does not delete the story, blacklist a topic, or override globally major news.

### Submit a missed story

Pasting a URL in `#tech-radar` or `#tech-breaking` is intended to store it, treat it as positive evidence, associate it when possible, and record an honest coverage-gap diagnosis. X/Reddit URLs may be stored but are not directly fetched.

### Flag important/covering

The intended `/news-flag` control marks canonical story/topic state such as explicitly important or already being covered. It is separate from ✅/❌ preference semantics.

### Manage sources

The intended `/news-source` command lists/enables/disables bounded catalog sources for the authorized owner. The live handler/registration has not been accepted.

## Commands / reactions / controls (intended)

The intended controls are ✅/❌ preference reactions, 🎬/🧵 editorial-selection reactions, `/news-source`, `/news-flag`, and `/news-pipeline`. Every one remains **IN PROGRESS / NOT LIVE**. Their semantics are described below so the design can be reviewed without implying present availability.

## Editorial Story Pipeline (intended)

### Select stories

- Add 🎬 to an eligible Firehose/Radar/Breaking story to create/use the **primary** pipeline.
- Add 🧵 to select into the current **secondary-active** pipeline.
- If there is no secondary-active pipeline, 🧵 does nothing and should be removed; it never creates the primary.
- Remove your reaction to remove only that selection.

### Commands

The local `/news-pipeline` command defines:

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

Today, the correct interpretation is simply: N1 is not active. Do not expect messages, commands, reactions, digests, or releases to work.

After future activation, intended safe behavior is:

- source failure is isolated and retried/cooldown-managed;
- duplicate story delivery is suppressed per surface;
- ungrounded/model failure keeps canonical evidence and falls back;
- ineffective 🧵 is removed;
- stale/duplicate release is fenced;
- failed release before persistence keeps all selections.

## Current limitations

- No live/hosted/owner-accepted N1 workflow.
- No direct X or Reddit integration/scraping.
- Known source coverage gaps and small evaluation corpus.
- No proven live daily digest or breaking quality.
- No real `#tech-desk` release.
- No auto-script, auto-post, or auto-publication.

## Planned improvements / closeout work

The current milestone still needs hosted DEV deployment, live recurring operation, Discord registration and surface checks, real feedback/source/flag/manual-submission acceptance, digest proof, one bounded real editorial-pipeline release, final independent review, and closeout reconciliation. This guide must be updated from “intended” to “current” only for behavior that passes those gates.
