# Patrick — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `apps/discord-bot/`; `services/{capture,personal-ops,jobops,application-materials,news-radar}/`; current command/approval records in `docs/reviews/`; and `docs/reviews/news-radar/N1/`<br>
> **Documentation status:** Current

[Patrick overview](README.md) · [Technical study guide](study-guide.md) · [Product vision](vision.md)

> **CURRENT GUIDE:** Patrick is currently encountered mainly through Discord. Chat, conversational voice, calls, Recall-backed questions, and general cross-system intent routing are future capabilities. Capture's accepted iOS voice Shortcut is a bounded Capture ingress, not a conversational Patrick voice surface.

## What Patrick is today

Patrick is the human-facing identity of Prateek OS on Discord. Different channels, slash commands, buttons, and reactions connect to different underlying systems. Patrick makes them feel related, but each system still owns its instructions, data, permissions, and approval rules.

The Discord username “Patrick” is a presentation setting. The Discord application/project and platform remain named Prateek OS. A message to Patrick is not automatically a command, and conversational wording never overrides an approval gate.

## Front door

| What I want to do                            | Where/how I do it                                                                     | Detailed guide                                                          |
| -------------------------------------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Capture an idea, link, note, file, or task   | Post naturally in the configured Capture channel; exact prefixes are also allowed.    | [Capture](../systems/capture/user-guide.md)                             |
| Create a task with explicit hard fields      | Use `/task` with optional duration, earliest, and due values.                         | [Personal Ops](../systems/personal-ops/user-guide.md)                   |
| Inspect today's or this week's tasks         | Use `/today` or `/week`.                                                              | [Personal Ops](../systems/personal-ops/user-guide.md)                   |
| Propose a schedule                           | Use `/plan`, review the exact proposal, then Approve or Reject.                       | [Personal Ops](../systems/personal-ops/user-guide.md)                   |
| Propose a Calendar event in natural language | Describe it in the Capture channel, then decide the separate proposal.                | [Capture](../systems/capture/user-guide.md)                             |
| Review JobOps results                        | Read the configured new/hot job channels and inspect the actual posting.              | [JobOps](../systems/jobops/user-guide.md)                               |
| Request application materials                | React 📄 on a persisted JobOps message or use `/prepare`; generation is paused.       | [Application Materials](../systems/application-materials/user-guide.md) |
| Use Tech News Radar                          | Read `#tech-radar`, react ✅/❌ or 🎬/🧵, and manage pipelines with `/news-pipeline`. | [Tech News Radar](../systems/tech-news-radar/user-guide.md)             |

Channel names and availability are system configuration, not universal Patrick behavior. Use the linked guide for the accepted details and current status of each capability.

## Natural-language Capture

In the configured Capture channel, write an ordinary message such as:

- “I should make a video about local AI.”
- “Call the clinic tomorrow afternoon for 20 minutes.”
- “Meeting with Sam tomorrow at 3pm.”

Patrick passes that message to Capture. Capture first preserves the raw input, then uses bounded interpretation. Deterministic code validates the proposed structure and selects the outcome:

- high confidence can create the supported saved item or task;
- medium confidence produces a separate confirmation;
- low confidence/provider failure safely records an unknown result without an action;
- an event creates a separate Calendar proposal even when semantic confidence is high.

For exact no-model behavior, use a supported prefix such as `idea:`, `reference:`, `task:`, or `note:`. The [Capture guide](../systems/capture/user-guide.md) lists the full accepted syntax and attachment behavior.

Natural-language Capture is not open-ended Patrick chat. Every eligible message in the Capture channel enters Capture; Patrick does not currently decide that a sentence is instead a question for Recall or a command for another system.

## Confirmations and proposals

Patrick may post a separate message with Approve and Reject controls.

- A Capture confirmation asks whether a medium-confidence interpretation should produce its bounded domain action.
- A Calendar event proposal shows the exact event before any Calendar write.
- A `/plan` proposal shows exact task blocks before Personal Ops applies them.

Rejecting or ignoring a Calendar proposal creates no Calendar event. Expired, stale, or duplicate decisions fail closed. Do not treat natural conversation as a substitute for pressing the applicable decision control.

## Notifications and reactions

Patrick surfaces output produced by domain systems:

- JobOps posts eligible/ranked job notifications. 📄 requests an idempotent Application Materials job; it does not apply.
- Application Materials can acknowledge an existing/queued request and privately deliver status/artifacts when available. The generation worker is currently paused, so a newly queued request will not produce a package today.
- Capture uses reactions such as ✅ for a safely completed pipeline, 🔁 for replay without duplication, 🚫 for unauthorized input, and ⚠️ for unsafe/invalid input.

Notification text is a presentation of domain state, not the canonical record. If Discord delivery fails, the underlying system may still retain its authoritative result.

## Authorization and privacy

Patrick is not an authorization principal. The real Discord actor must be authorized, the channel/control must be eligible, and the target system's policy still applies. Calendar changes remain approval-gated. JobOps never applies to a job. Application Materials always require human review and manual application.

Do not paste tokens, credentials, private Brain contents, private messages, or sensitive Calendar/task/Capture data into public or inappropriate Discord channels.

## What Patrick cannot do yet

Patrick currently cannot:

- hold general open-ended conversations across OS systems;
- classify a message system-wide as Capture, Query, or Command;
- answer questions through Recall;
- read or consume Prateek Brain;
- continue one authoritative interaction across Discord, chat, voice, and calls;
- provide conversational voice or telephone interaction as Patrick surfaces;
- infer permission from natural language or bypass approval;
- treat Tech News Radar's hosted-DEV acceptance surface as a merged/formally accepted capability;
- restore the paused Application Materials generation worker merely by accepting a request.

Those boundaries are intentional. See the [Product Vision](vision.md) for clearly labelled future direction rather than current instructions.
