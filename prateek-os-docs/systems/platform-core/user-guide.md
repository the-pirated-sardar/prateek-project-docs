# Platform / Core — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `packages/`; `apps/discord-bot/`; `ops/`; `supabase/migrations/`; `docs/{ARCHITECTURE,PRIVACY}.md`; and deployment/acceptance records in `docs/reviews/`<br>
> **Documentation status:** Current

[Documentation home](../../README.md) · [Technical study guide](study-guide.md)

## What it does

Platform/Core is the shared foundation behind Patrick and every current Prateek OS system. It provides private storage, identity/provenance, process supervision, and safe control boundaries. It is not a separate app you operate.

## Where I use it

You encounter it through Discord, the iOS Capture Shortcuts, and system-generated status/error messages. Most platform behavior is intentionally invisible when healthy.

## Common workflows

- Use Patrick only in configured Prateek OS channels.
- Treat hosted **DEV** as the current live environment; nothing here implies Supabase PROD is deployed.
- Check `#errors` when a domain service reports a persistent source/runtime problem.
- For operator-only diagnostics, use the domain guide rather than modifying database rows manually.

## Commands, reactions, and controls

Platform/Core has no public slash commands of its own. Commands belong to domains. Authorization is always tied to your real Discord identity and configured channel—not to the Patrick persona.

## Examples

- A command from an unauthorized user is rejected without mutation.
- A Capture message in the wrong channel is ignored.
- Re-delivery of the same Discord event converges on the original record.
- A missing feature-specific channel configuration disables that feature, not every Patrick capability.

## What happens behind the scenes

Patrick maps a Discord event to a typed domain request. The domain writes structured state to PostgreSQL across dedicated database planes (Core Supabase, News Radar Supabase, or JobOps Neon), where constraints/RLS protect it. Long-running or recurring work is claimed with database leases. Railway runs the Patrick Gateway service under distributed lease coordination (`patrick:discord-gateway`) and starts finite JobOps schedule runs; LaunchAgents supervise optional local worker processes.

## What statuses mean

| Status                  | Meaning                                                                  |
| ----------------------- | ------------------------------------------------------------------------ |
| **IMPLEMENTED**         | Code and accepted evidence exist.                                        |
| **HOSTED DEV**          | The accepted runtime targets the development database.                   |
| **RUNTIME PAUSED**      | Architecture/data remain, but a worker is intentionally not running.     |
| **IN PROGRESS · LOCAL** | Feature-branch implementation exists without formal closeout/deployment. |
| **PLANNED/FUTURE**      | Not usable today.                                                        |

## When something goes wrong

Do not paste secrets into Discord or chat. Capture the bounded error/status and consult the relevant domain guide. Failed work normally remains retryable or recorded; bypassing a lease/approval directly in the database is not a supported user action.

## Current limitations

- No general dashboard.
- No ordinary Discord switch for protected core systems.
- No PROD promotion.
- No general Patrick query router or Brain integration.

## Planned improvements

OS4 production backup/restore deployment and remaining verification, future R1 (Reference Library, unfrozen scope), J5, and cross-system query routing are planned or future. They are not current platform capabilities.
