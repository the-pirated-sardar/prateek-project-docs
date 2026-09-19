# Application Materials

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/application-materials/`; Application Materials paths in `apps/discord-bot/` and `ops/macos/`; `supabase/migrations/`; `docs/adr/{0007,0009,0010}-*.md`; and `docs/reviews/application-materials/`<br>
> **Documentation status:** Current

Application Materials is the preparation pipeline for tailored resumes and cover letters. It combines a canonical JobOps record with verified personal evidence, uses bounded structured generation, and produces deterministic documents for private delivery and required human review. It never submits an application automatically.

## Status

**DORMANT · RETIREMENT SCHEDULED IN J5.** The accepted persistence, recovery, validation, and approval architecture remains documented, but the local worker is intentionally not installed or running. Dedicated rehosting is deferred; full retirement of the subsystem is planned under J5. Requests can queue; processing is paused.

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
