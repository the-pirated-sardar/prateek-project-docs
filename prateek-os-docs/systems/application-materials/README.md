# Application Materials

Application Materials is the preparation pipeline for tailored resumes and cover letters. It combines a canonical JobOps record with verified personal evidence, uses bounded structured generation, and produces deterministic documents for private delivery and required human review. It never submits an application automatically.

## Status

**IMPLEMENTED, BUT RUNTIME PAUSED / NOT RELIABLE END TO END.** The accepted persistence, recovery, validation, and approval architecture remains documented, but the worker is intentionally not installed or running. Requests can queue; processing is paused pending repair.

## What to read

| Document                                  | Best for                                                                                                                                     |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Evidence boundaries, queue and lease design, generation, journaling, deterministic rendering, validation, recovery, security, and tradeoffs. |
| [User Guide](user-guide.md)               | Requesting materials, interpreting request states, and understanding what the paused runtime means in practice.                              |
| [OS Study Guide](../../os-study-guide.md) | Shared model, approval, persistence, and local-runtime architecture.                                                                         |

## In this system

- idempotent requests tied to canonical JobOps notification identity;
- a durable queue with claims, heartbeats, reclaim, and recovery-safe invocation journaling;
- evidence-grounded structured resume and cover-letter generation;
- deterministic LaTeX/PDF production and bounded validation/repair;
- fact, identity, page, geometry, typography, and overflow checks;
- private delivery ending in human review and manual application.

Deterministic checks reduce risk but do not replace careful review. Provider usage telemetry may be unavailable, and the current generation path is not reliable enough for active use.

## Related systems

- [JobOps](../jobops/README.md) provides the canonical job and notification identity.
- [Platform / Core](../platform-core/README.md) provides persistence, security, and runtime conventions.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
