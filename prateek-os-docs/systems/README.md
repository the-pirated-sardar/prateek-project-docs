# Prateek OS Systems

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** canonical `main` `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`; N1 in-progress feature snapshot `c111af6770bc2197ce35b2cb76179356911b39ae` (corrected code `2bd34e3`)<br>
> **Source scope:** `services/`; `apps/`; `docs/{ROADMAP,MILESTONES}.md`; `docs/adr/`; `docs/reviews/`; and `supabase/migrations/`<br>
> **Documentation status:** Mixed current/future

This directory is the system-by-system map of Prateek OS. Each system owns a focused domain while reusing a small set of shared platform conventions. Use the directory README for quick orientation, the study guide for engineering depth, and the user guide for practical workflows.

| System                                                   | What it does                                                                      | Implementation status                                         |
| -------------------------------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| [Platform / Core](platform-core/README.md)               | Supplies shared architecture, infrastructure, security, and engineering patterns. | **IMPLEMENTED**                                               |
| [JobOps](jobops/README.md)                               | Discovers, canonicalizes, prioritizes, and reports relevant jobs.                 | **IMPLEMENTED · HOSTED DEV**                                  |
| [Application Materials](application-materials/README.md) | Prepares evidence-grounded resume and cover-letter packages for human review.     | **IMPLEMENTED, BUT RUNTIME PAUSED / NOT RELIABLE END TO END** |
| [Capture](capture/README.md)                             | Preserves and interprets inputs from Discord and mobile entry points.             | **IMPLEMENTED · HOSTED DEV**                                  |
| [Personal Ops](personal-ops/README.md)                   | Owns tasks, deterministic planning, Calendar proposals, and Tasks mirroring.      | **IMPLEMENTED · HOSTED DEV**                                  |
| [Tech News Radar](tech-news-radar/README.md)             | Builds a personalized editorial tech-news stream and story-selection pipeline.    | **IN PROGRESS · HOSTED DEV / OWNER ACCEPTANCE PENDING**       |

## How these directories are organized

Each system directory generally contains:

- `README.md` — quick orientation, status, and navigation;
- `study-guide.md` — architecture, engineering decisions, data model, testing, security, limitations, and tradeoffs;
- `user-guide.md` — practical workflows, controls, statuses, and troubleshooting.

Statuses describe the current public documentation truth, not a promise of production availability. Accepted operation currently targets DEV rather than Supabase PROD. Application Materials retains accepted architecture and recovery evidence, but its worker is paused and generation is not currently reliable end to end. Tech News Radar was activated on hosted DEV from an unmerged feature snapshot; its corrections passed delta review and hosted reverification, while owner acceptance and milestone closeout remain pending.

For the principles and infrastructure shared across systems, read the [OS Study Guide](../os-study-guide.md). Return to the [Prateek OS documentation home](../README.md) for the overall system map and roadmap.
