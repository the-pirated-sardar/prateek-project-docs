# Capture — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `services/capture/`; `apps/capture-api/`; Capture paths in `apps/discord-bot/`; `packages/llm-router/`; and `docs/reviews/capture/{C1,C2,C3}/`<br>
> **Documentation status:** Current

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

## What it does

Capture is the low-friction inbox for Prateek OS. It preserves text, links, files, screenshots, shared content, and voice inputs, then either uses explicit deterministic syntax or bounded natural-language interpretation to create a saved item, task, or Calendar proposal.

**Status: IMPLEMENTED · HOSTED DEV.** Discord and both iOS Shortcuts have real accepted workflows. PROD is not deployed.

## Where I use it

- Post in Discord `#capture`.
- Use the iPhone Action Button voice Shortcut.
- Use the iOS Share Sheet for a URL, selected text, image, or file, optionally with a “why saved” note.

## Common workflows

### Natural language (normal path)

Write naturally; slash syntax is not required:

- “Call the clinic tomorrow afternoon for 20 minutes.”
- “Idea for a video comparing local and hosted agents.”
- “Meeting with Sam next Friday at 2pm.”

The model proposes structure; deterministic policy decides whether it is safe to act, ask, or store as unknown.

### Deterministic fallback

Use a prefix when you want exact, free, no-model behavior:

| Prefix                | Outcome                                                  |
| --------------------- | -------------------------------------------------------- |
| `idea:`               | Create an idea item.                                     |
| `reference:` / `ref:` | Create a reference.                                      |
| `note:` / `project:`  | Create a project note.                                   |
| `person:`             | Create a lightweight person item.                        |
| `creator:`            | Create a creator idea.                                   |
| `writing:`            | Create a writing idea.                                   |
| `book:`               | Preserve/classify; no separate executable behavior.      |
| `task:` / `todo:`     | Create a canonical Personal Ops task.                    |
| `buy:` / `purchase:`  | Preserve a purchase action, blocked; no purchase occurs. |

Recognized task phrases such as “remind me to…,” “remember to…,” “need to…,” and “todo…” also use the bounded deterministic path. A bare or lightly annotated link/file becomes a reference. A known prefix plus URL can create both its primary item and a reference.

The deterministic prefix path intentionally understands much less time language than natural-language Capture. Use `/task` in Personal Ops for explicit hard `duration`, `earliest`, and `due` fields.

When a task action succeeds, the canonical task is created first. If Google Tasks mirroring is configured, the system then attempts that downstream mirror automatically and best-effort; there is no separate mirror approval. Calendar writes remain separately proposal-gated.

### Medium-confidence confirmation

Patrick posts a separate message with Approve/Reject buttons. Nothing derived is created until approval. You can reply to that still-open confirmation with a correction. Ignoring it expires the active wait without changing the raw Capture; late approval remains safely fenced by the persisted lifecycle.

### Event proposal

Natural-language events create an exact Google Calendar proposal. Approve writes one event; Reject/ignore writes nothing. This approval is required even when the semantic classification was high confidence.

## Commands, reactions, and controls

No slash command is required for normal Capture. The deterministic prefix is a message syntax, not a slash command.

| Reaction/status | Meaning                                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------------------- |
| ✅              | Raw Capture was accepted and the pipeline ended safely. Check for a separate confirmation/proposal message when relevant. |
| 🔁              | Discord redelivered an already stored message; no duplicate was created.                                                  |
| 🚫              | Actor was not authorized; nothing saved.                                                                                  |
| ⚠️              | Empty/unsafe/oversized attachment input was rejected; nothing saved.                                                      |
| No reaction     | Wrong channel or bot message; intentionally ignored.                                                                      |

## Examples

- `idea: test stale lease ownership` → exact deterministic idea creation, no model call.
- “Save this for my database course” plus URL → model-backed reference interpretation.
- “Pick up milk” may ask for confirmation because a short command can mean different actionable types.
- “Schedule dinner tomorrow at 7” → Calendar proposal, never an automatic write.
- A long message containing several unrelated tasks/ideas → safe `unknown`/diagnostic rather than arbitrary splitting.

## What happens behind the scenes

```text
authorize input
  → preserve exact raw text/files/time/provenance
  → explicit prefix? deterministic rules : bounded model interpretation
  → validate schema and resolve time deterministically
  → HIGH: execute safe domain action
     MEDIUM: ask first
     LOW/provider failure: unknown, no action
  → store every outcome and acknowledge
```

Files remain private. Supported content may be bounded/truncated for interpretation; the original remains unchanged.

## What statuses mean

| Action state      | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| `pending`         | Waiting to run.                                     |
| `executing`       | Held by one worker lease.                           |
| `succeeded`       | Domain result created.                              |
| `blocked`         | Human approval/confirmation required.               |
| `deferred`        | Classification has no executor yet.                 |
| `expired`         | Stored temporal target passed before action.        |
| `retry_pending`   | Transient failure will retry with bounded backoff.  |
| `failed_terminal` | Attempts exhausted; canonical Capture still exists. |

## When something goes wrong

- ⚠️: repost with safe/smaller/fewer files.
- 🚫: ask for explicit authorization; nothing was stored.
- Unknown result: raw input is safe; use a clearer phrase or deterministic prefix.
- Medium result: use its buttons or reply to that confirmation with a correction.
- Provider outage: Capture degrades safely to unknown; it does not lose the raw input.
- Event/task action failure: Patrick can deliver a bounded follow-up; do not bypass the approval state manually.

## Current limitations

- No automatic splitting of several unrelated thoughts.
- No general correction graph for already completed captures.
- No general Patrick question/query routing or Brain integration.
- Purchase approval is not usable.
- Some semantic types are classified only.
- Some ambiguous date/bare-hour language asks rather than guesses.
- Unsupported file content is preserved but not interpreted.

## Planned improvements

C4 is scheduled but deliberately unfrozen. Multi-item segmentation, richer correction/conversation handling, temporal-language refinements, general intent routing, and acknowledgement UX changes remain separate future work—not current behavior.
