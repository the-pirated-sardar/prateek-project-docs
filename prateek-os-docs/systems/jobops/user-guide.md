# JobOps — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/jobops/`; JobOps paths in `apps/discord-bot/`; `ops/`; and `docs/reviews/jobops/`<br>
> **Documentation status:** Current

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

## What it does

JobOps automatically discovers Canadian job postings, keeps one canonical record per strongly identified posting, applies a conservative eligibility gate, calculates an explainable LAND priority score, and posts useful results to Discord.

**Status: IMPLEMENTED · HOSTED DEV.** The scheduler runs on Railway against the dedicated JobOps Neon PostgreSQL database plane under OS4. JobOps never applies, sends outreach, or makes an immigration determination.

## Where I use it

- `#jobs-new`: all newly discovered jobs that pass hard eligibility and have a current ranking.
- `#jobs-hot`: the highest-priority eligible subset with the strongest location class.
- `#errors`: persistent source/scheduler incidents and recovery notices.
- Terminal: operator-only health, ingest, rebuild, and demand commands.

## Common workflows

1. Scan `#jobs-hot` first for time-sensitive top-priority opportunities.
2. Continue through `#jobs-new` for the broader eligible stream.
3. Read the LAND bucket/components as a triage aid, then inspect the actual posting.
4. React 📄 if you want an Application Materials request queued. Generation is currently paused; see that guide.
5. Apply manually. JobOps never submits anything.

## Commands, reactions, and controls

There are no routine JobOps slash commands. The main user control is 📄 on a persisted job notification.

Operator terminal commands include:

| Command                           | Purpose                                                               |
| --------------------------------- | --------------------------------------------------------------------- |
| `pnpm jobops:sources`             | List configured source state.                                         |
| `pnpm jobops:ingest -- [options]` | Run a bounded collection pass.                                        |
| `pnpm jobops:health`              | Read-only scheduler/source health.                                    |
| `pnpm jobops:demand`              | Read-only company-demand summaries.                                   |
| `pnpm jobops:backfill`            | Rebuild current intelligence/rankings without Discord sends.          |
| `pnpm jobops:reclassify`          | Reapply eligibility under the runner lease, without sending messages. |

## Examples

- A Canadian junior/intermediate role may appear in `#jobs-new`; if it also reaches the top recommendation bucket and strongest location group, it appears in `#jobs-hot` once.
- The same posting seen in an ATS feed and an email can produce two observations but one canonical job.
- A role with unknown location is stored but does not notify.
- A provider outage may produce one `#errors` incident, then enter cooldown and later send one recovery message.

## What happens behind the scenes

```text
source becomes due
  → bounded fetch
  → normalize
  → strong-identity dedupe
  → persist canonical job + observation + provenance
  → hard eligibility
  → deterministic LAND ranking
  → independent #jobs-new / #jobs-hot delivery
```

The five-minute cron is a dispatcher, not a promise that every source is polled every five minutes. Sources have staggered absolute schedules. One database lease prevents overlapping runs.

## What statuses and terms mean

| Term/status     | Meaning                                                |
| --------------- | ------------------------------------------------------ |
| `APPLY NOW`     | Highest priority, subject to additional guards.        |
| `APPLY TODAY`   | Strong daily-queue candidate.                          |
| `STRETCH`       | Worth considering, possibly after bounded preparation. |
| `OPPORTUNISTIC` | Lower immediate fit but potentially useful.            |
| `IGNORE`        | Hard-gated or low strategic value; still retained.     |
| Observation     | One sighting from one source.                          |
| Canonical job   | Stable deduplicated job identity.                      |
| Cooldown        | Temporary source pause after repeated failure.         |

Exact private weights and personal ranking configuration are not published here. Scores are deterministic priorities, not predictions.

## When something goes wrong

- Missing job: it may have failed the eligibility gate, not yet been observed, or be outside current source coverage.
- Duplicate-looking posts: strong identity may be insufficient to merge safely; the system prefers a false split to a false merge.
- Source error: persistent failures appear in `#errors`; cooldown and later retry are automatic.
- No PDF after 📄: the Application Materials worker is dormant with retirement scheduled in J5. The request may still be queued.
- Never paste OAuth tokens, database keys, or email contents into Discord for diagnosis.

## Current limitations

- Coverage does not include every employer/platform.
- Unknown locations are deliberately suppressed.
- LAND and probable NOC/TEER are heuristics requiring human judgement.
- A future “upgrade” ranking mode and JobOps CRM/outreach features are not implemented.
- Current operation is hosted DEV, not PROD.

## Planned improvements

J5 is scheduled after R1; J3 (CRM, outreach preparation, follow-ups, and interview preparation) is deferred without a planned date. Consequential outreach will still require approval; automated sending/applying is not current behavior.
