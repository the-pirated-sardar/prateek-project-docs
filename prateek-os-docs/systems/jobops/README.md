# JobOps

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/jobops/`; JobOps paths in `apps/discord-bot/`; `supabase/migrations/`; `docs/adr/000{1,2,3,4,5}-*.md`; and `docs/reviews/jobops/`<br>
> **Documentation status:** Current

JobOps is the job-discovery and attention-prioritization pipeline. It turns observations from supported public feeds and read-only job-alert email into canonical, explainable job records, then applies conservative eligibility and deterministic ranking before sending bounded Discord notifications.

## Status

**IMPLEMENTED · HOSTED DEV.** Scheduled polling runs on Railway against the dedicated JobOps Neon PostgreSQL database plane under OS4; JobOps does not apply to jobs, contact employers, or make immigration determinations. J5 is scheduled after R1; J3 is deferred without a planned date.

## What to read

| Document                                  | Best for                                                                                                      |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Architecture, canonical identity, ingestion, ranking, scheduling, delivery, testing, security, and tradeoffs. |
| [User Guide](user-guide.md)               | Reading notifications, using controls, understanding statuses, and handling common failures.                  |
| [OS Study Guide](../../os-study-guide.md) | Shared persistence, scheduling, concurrency, and security patterns.                                           |

## In this system

- ingestion from supported ATS/public providers and bounded read-only Gmail alerts;
- strong canonical job identity with retained observations and provenance;
- conservative eligibility checks and versioned, deterministic ranking intelligence;
- absolute-slot scheduled polling with source health and lease protection;
- independently idempotent Discord delivery for new and high-priority results;
- demand and source intelligence over canonical activity.

Private source inventories and exact ranking weights are intentionally not documented here. Coverage is broad rather than universal, and a ranking remains a triage aid.

## Related systems

- [Application Materials](../application-materials/README.md) prepares review packages from canonical JobOps records; its generation runtime is currently paused.
- [Platform / Core](../platform-core/README.md) supplies shared database, scheduling, security, and delivery conventions.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
