# Capture

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`<br>
> **Source scope:** `services/capture/`; `apps/capture-api/`; Capture paths in `apps/discord-bot/`; `packages/llm-router/`; `supabase/migrations/`; `docs/adr/0012-c3-natural-language-interpretation.md`; and `docs/reviews/capture/`<br>
> **Documentation status:** Current

Capture is the write-side ingress layer for Prateek OS. It preserves what an authorized user sent through Discord or an iOS entry point before attempting interpretation, then routes safe, typed actions to the domain that owns them. Capture owns ingress and provenance; it does not become the task planner or Calendar authority.

## Status

**IMPLEMENTED · HOSTED DEV.** Deterministic Capture, mobile ingress, and bounded natural-language interpretation have accepted Discord and iPhone workflows. PROD is not deployed.

## What to read

| Document                                  | Best for                                                                                                                          |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Transactional persistence, deterministic/model paths, confidence policy, action handoff, security, testing, and failure recovery. |
| [User Guide](user-guide.md)               | Discord and Shortcut workflows, explicit prefixes, confirmations, event proposals, statuses, and troubleshooting.                 |
| [OS Study Guide](../../os-study-guide.md) | Shared provenance, canonical-state, approval, and model-boundary principles.                                                      |

## In this system

- Discord `#capture`, a mobile Action Button, and a Share Sheet entry point;
- canonical-first storage of raw text, supported attachments, time, and provenance;
- deterministic interpretation for explicit commands and a bounded natural-language path for judgement;
- schema validation, deterministic time resolution, and code-owned confidence bands;
- automatic execution only for safe high-confidence actions, confirmation for medium confidence, and safe unknown outcomes otherwise;
- event-proposal and canonical-task handoff to Personal Ops.

Unsupported or ambiguous content is preserved rather than guessed. Consequential Calendar writes remain proposal-gated by Personal Ops.

## Related systems

- [Personal Ops](../personal-ops/README.md) owns the tasks and Calendar proposals created from Capture actions.
- [Platform / Core](../platform-core/README.md) supplies shared provenance, persistence, permissions, and Discord patterns.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
