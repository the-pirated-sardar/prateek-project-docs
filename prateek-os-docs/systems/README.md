# Prateek OS Systems

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/`; `apps/`; `docs/{ROADMAP,MILESTONES}.md`; `docs/adr/`; `docs/reviews/`; and `supabase/migrations/`<br>
> **Documentation status:** Mixed current/future

This directory is the system-by-system map of Prateek OS. Each system owns a focused domain while reusing a small set of shared platform conventions. Use the directory README for quick orientation, the study guide for engineering depth, and the user guide for practical workflows.

| System                                                   | What it does                                                                      | Implementation status                                   |
| -------------------------------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------- |
| [Platform / Core](platform-core/README.md)               | Supplies shared architecture, infrastructure, security, and engineering patterns. | **IMPLEMENTED · HOSTED DEV (OS3 closed, OS4 active)**   |
| [JobOps](jobops/README.md)                               | Discovers, canonicalizes, prioritizes, and reports relevant jobs.                 | **IMPLEMENTED · HOSTED DEV (Neon data plane)**          |
| [Application Materials](application-materials/README.md) | Prepares evidence-grounded resume and cover-letter packages for human review.     | **DORMANT · RETIREMENT IN J5 (worker paused)**          |
| [Capture](capture/README.md)                             | Preserves and interprets inputs from Discord and mobile entry points.             | **IMPLEMENTED · HOSTED DEV (Core Supabase)**            |
| [Personal Ops](personal-ops/README.md)                   | Owns tasks, deterministic planning, Calendar proposals, and Tasks mirroring.      | **IMPLEMENTED · HOSTED DEV (Core Supabase)**            |
| [Tech News Radar](tech-news-radar/README.md)             | Builds a personalized editorial tech-news stream, Replay Lab, and story pipeline. | **IMPLEMENTED · HOSTED DEV (closed/merged 2026-09-13)** |

## How these directories are organized

Each system directory generally contains:

- `README.md` — quick orientation, status, and navigation;
- `study-guide.md` — architecture, engineering decisions, data model, testing, security, limitations, and tradeoffs;
- `user-guide.md` — practical workflows, controls, statuses, and troubleshooting.

Statuses describe the current public documentation truth, not a promise of production availability. Accepted operation currently targets DEV rather than Supabase PROD. Application Materials retains accepted architecture and recovery evidence, but its worker is dormant, dedicated rehosting is deferred, and owner-facing retirement is scheduled in J5. Tech News Radar is complete, formally closed, and merged to canonical `main` with centralized Railway runtime, Replay Lab, and rolling judgment budget live on hosted DEV. Platform / Core reflects completed OS3 standardization and active OS4 offload/backup work.

For the principles and infrastructure shared across systems, read the [OS Study Guide](../os-study-guide.md). Return to the [Prateek OS documentation home](../README.md) for the overall system map and roadmap.
