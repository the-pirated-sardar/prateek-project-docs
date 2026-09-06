# Personal Ops — User Guide

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`<br>
> **Source scope:** `services/personal-ops/`; Personal Ops paths in `apps/discord-bot/`; and `docs/reviews/capture/{C2,C3}/`<br>
> **Documentation status:** Current

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

## What it does

Personal Ops owns canonical tasks, shows near-term work, proposes deterministic schedules around Google Calendar, and mirrors tasks to Google Tasks without making Google the source of truth.

**Status: IMPLEMENTED · HOSTED DEV.** Real Calendar read/reject/approve/duplicate-approval behavior has been accepted. PROD is not deployed.

## Where I use it

Use Patrick slash commands in Discord, or create tasks/events through Capture.

## Common workflows

### Create a task

- `/task <title>` creates a canonical task.
- Optional fields include duration, earliest start, and due time. Duration is required before the planner can place a task.
- When Google Tasks mirroring is configured, task creation attempts that best-effort mirror automatically; it does not show a second approval prompt.

Example:

    /task title:"Review design" duration:30m earliest:"tomorrow 10am" due:"tomorrow 5pm"

Supported explicit time forms include straightforward relative days, weekdays, named dates, noon/midnight, and exact local ISO-like date/time input. Invalid input is rejected without creating a task.

### Review tasks

- `/today` lists open tasks due today or overdue.
- `/week` lists open tasks due during the next seven days.

### Plan time

1. Run `/plan today`, `/plan tomorrow`, or the supported date form.
2. Review the exact proposed blocks and unscheduled items.
3. Press Approve to apply exactly that proposal, or Reject for zero Calendar changes.
4. If a referenced canonical task changed or the proposal expired, apply fails closed. The service does not re-read Calendar at apply time, so run `/plan` again if external Calendar state may have changed.

### Capture a task naturally

Say “Call the clinic tomorrow afternoon for 20 minutes” through `#capture` or either iOS Shortcut. A high-confidence result can create the task with a duration and soft preferred window. A medium result asks first. Task creation uses the same automatic best-effort Google Tasks mirror when configured.

### Capture an event

Say “Meeting with Sam tomorrow at 3pm.” Patrick creates a separate Calendar proposal. Approve writes the event; Reject or ignore writes nothing.

## Commands, reactions, and controls

| Control        | Effect                                                      |
| -------------- | ----------------------------------------------------------- |
| `/task`        | Create a canonical task, optionally with scheduling fields. |
| `/today`       | List due/overdue open tasks.                                |
| `/week`        | List the next seven days.                                   |
| `/plan`        | Create a point-in-time schedule proposal.                   |
| Approve button | Apply only the exact still-fresh proposal.                  |
| Reject button  | Record rejection; zero Calendar mutation.                   |

## Examples

- A task without duration appears in lists but remains unscheduled.
- A hard meeting from Google Calendar is never moved or shortened by the planner.
- A task due at 3pm is never placed beyond 3pm; if it cannot fit, it is shown as unscheduled.
- A natural-language preferred afternoon can be missed safely; the planner may fall back and marks that the preference was not met.
- Pressing Approve twice does not create two events.

## What happens behind the scenes

```text
canonical tasks + current Calendar snapshot
  → deterministic planner
  → immutable proposal + input fingerprint
  → human decision
  → expiry + canonical-task freshness recheck
  → exact OS-owned Calendar blocks
```

Google Tasks is a best-effort mirror. Editing/deleting it does not silently change the canonical task. Existing Calendar events in the proposal-time snapshot are hard unless they carry a recognized OS ownership tag. Calendar is not fetched again at apply time.

## What statuses mean

| Status             | Meaning                                                          |
| ------------------ | ---------------------------------------------------------------- |
| Open               | Eligible once required planning fields/dependencies allow.       |
| Scheduled          | Has an applied OS-owned Calendar block.                          |
| Done               | Canonical task completed; historical blocks remain history.      |
| Cancelled          | Excluded from planning; mirror may be removed.                   |
| Proposed           | Awaiting decision.                                               |
| Approved           | Decision recorded; apply still must pass freshness checks.       |
| Rejected/Expired   | No mutation.                                                     |
| Applied            | Exact proposal was written.                                      |
| Apply failed       | Stale task/proposal or provider failure; canonical task remains. |
| Mirror sync failed | Google mirror failed; canonical task is still valid.             |

## When something goes wrong

- Proposal stale or Calendar may have changed: run `/plan` again; do not bypass it.
- Task omitted: check duration, dependency, earliest/due bounds, and capacity.
- Google failure: canonical data remains safe; external status records the error.
- Duplicate approval refused: expected fencing, not lost work.
- Never paste OAuth tokens or private Calendar/task contents into public channels.

## Current limitations

- No general task dependency graph or optimizer.
- No apply-time Calendar refresh; a new external event can overlap a previously generated proposal.
- No independent periodic Google Tasks mirror drain.
- Natural-language Calendar update/delete is not available; create-only is wired.
- Some Capture time phrases ask for clarification rather than guessing.
- Current accepted environment is DEV, not PROD.

## Planned improvements

Richer Capture refinements and a future Patrick intent/query router may add conversational task/Calendar operations. Recall is planned as a read side, but neither is current behavior.
