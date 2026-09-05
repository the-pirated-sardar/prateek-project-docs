# JobOps

JobOps is the job-discovery and attention-prioritization pipeline. It turns observations from supported public feeds and read-only job-alert email into canonical, explainable job records, then applies conservative eligibility and deterministic ranking before sending bounded Discord notifications.

## Status

**IMPLEMENTED · HOSTED DEV.** Scheduled polling runs on Railway against the DEV database; JobOps does not apply to jobs, contact employers, or make immigration determinations.

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
