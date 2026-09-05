# Personal Ops

Personal Ops owns canonical tasks and deterministic time planning. It provides Discord task views and planning commands, creates approval-gated Google Calendar proposals, and can mirror canonical task state to Google Tasks without making either Google surface the source of truth.

## Status

**IMPLEMENTED · HOSTED DEV.** Task, planning, Calendar approval, and configured Tasks-mirroring workflows have accepted real-world evidence. PROD is not deployed.

## What to read

| Document                                  | Best for                                                                                                                         |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Canonical task design, deterministic planning, proposal lifecycle, Calendar/Tasks integration, testing, security, and tradeoffs. |
| [User Guide](user-guide.md)               | Using `/task`, `/today`, `/week`, and `/plan`, reviewing proposals, and troubleshooting Calendar or mirror failures.             |
| [OS Study Guide](../../os-study-guide.md) | Shared state-machine, approval, idempotency, and integration principles.                                                         |

## In this system

- canonical tasks with duration, scheduling bounds, status, and provenance;
- `/task`, `/today`, `/week`, and `/plan` Discord commands;
- a deterministic planner that respects hard Calendar occupancy and task constraints;
- plan-first, mutate-second Calendar proposals with expiry, approval, and replay fencing;
- event proposals handed off from Capture;
- best-effort Google Tasks mirroring of canonical task state.

Calendar occupancy is based on the proposal-time snapshot. Applying an approved proposal checks proposal and canonical-task freshness, but it does **not** perform a fresh live Calendar-conflict read. When configured, Google Tasks mirroring is an automatic best-effort downstream sync and does **not** require separate owner approval.

## Related systems

- [Capture](../capture/README.md) supplies natural-language task and event proposals.
- [Platform / Core](../platform-core/README.md) supplies shared persistence, approval, and integration patterns.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
