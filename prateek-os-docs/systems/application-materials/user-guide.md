# Application Materials — User Guide

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`<br>
> **Source scope:** `services/application-materials/`; Application Materials paths in `apps/discord-bot/` and `ops/macos/`; and `docs/reviews/application-materials/A2/`<br>
> **Documentation status:** Current

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

## What it does

Application Materials prepares a tailored one-page resume PDF and one-page cover-letter PDF for a canonical JobOps posting, using verified evidence and deterministic document checks.

**Current status: IMPLEMENTED, BUT RUNTIME PAUSED / NOT RELIABLE END TO END.** Requests can be queued; the worker is not currently installed/running, so no package will be generated until the repair milestone is completed and the worker is restored.

## Where I use it

- React 📄 on a Patrick job message in `#jobs-new` or `#jobs-hot`.
- Use `/prepare <job_id>` as an authorized fallback.
- When processing is restored, receive private files in `#application-materials`.

## Common workflows

### Reaction workflow

1. Open a persisted job notification.
2. Add 📄.
3. Patrick queues or finds the existing request.
4. Today, processing stops there because the worker is paused.

### `/prepare` fallback

Use `/prepare <job_id>` when you have a canonical job ID or need to redeliver an existing successful package. Repeating the same logical request does not regenerate or duplicate it.

### When runtime is restored

The worker will research the role/company, generate bounded structured drafts, render/validate PDFs, and deliver the selected pair privately. Review every document before using it. Apply manually.

## Commands, reactions, and controls

| Control               | Meaning                              |
| --------------------- | ------------------------------------ |
| 📄 on a real job post | Primary preparation request.         |
| `/prepare <job_id>`   | Authorized fallback or redelivery.   |
| Remove/re-add 📄      | Does not cancel; remains idempotent. |

Both `#jobs-new` and `#jobs-hot` copies of the same job map to one request.

## Examples

- Reacting on a hot-job copy and later on its new-job copy returns the same preparation request.
- Running `/prepare` again after a delivery problem redelivers selected PDFs rather than paying for regeneration.
- Reacting today queues work but produces no PDF while the worker is paused.

## What happens behind the scenes

```text
authorized request
  → durable queue
  → local worker claim + heartbeat
  → verified evidence and bounded research
  → structured generation with request-wide budget
  → deterministic LaTeX/PDF validation and bounded repair
  → private delivery
  → NEEDS_HUMAN_REVIEW
```

The model may draft language, but it cannot authorize facts or own document layout. A crash-safe journal prevents completed or ambiguous paid work from being repeated freely.

## What statuses mean

| Status              | Meaning                                                                                |
| ------------------- | -------------------------------------------------------------------------------------- |
| Queued              | Request exists and awaits a worker.                                                    |
| Claimed/Processing  | A worker owns it under a lease.                                                        |
| Needs human review  | Artifacts were produced but are not approved/submitted.                                |
| Failed              | Processing stopped; automatic expensive retry is not assumed.                          |
| Succeeded/Delivered | Package passed system checks and was privately delivered; still requires human review. |

## When something goes wrong

- No documents today: expected while the worker is paused.
- Duplicate acknowledgement: check the existing request; reactivity is idempotent.
- Delivery failed after generation: use `/prepare` for redelivery; do not regenerate manually unless the recorded state requires it.
- Never treat “succeeded” as “applied,” and never paste private resume evidence or credentials into a public channel.

## Current limitations

- Worker intentionally absent; generation paused.
- Execution backend needs repair/replacement; no replacement provider is selected.
- Automated validation cannot replace human review.
- No submission, recruiter message, or public document link is created.

## Planned improvements

A future maintenance milestone is expected to repair the generation path and reconcile updated master resume/cover-letter sources while preserving the existing evidence, budget, recovery, privacy, and approval contracts.
