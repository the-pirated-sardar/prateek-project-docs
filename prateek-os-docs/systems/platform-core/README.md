# Platform / Core

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `packages/`; `apps/discord-bot/`; `ops/`; `supabase/migrations/`; `docs/{ARCHITECTURE,PRIVACY,MODEL_POLICY}.md`; and `docs/adr/`<br>
> **Documentation status:** Current

Platform / Core is the shared foundation beneath Prateek OS. It provides reusable infrastructure and engineering conventions without absorbing domain rules into a central “god service.” Its design is deterministic-first: explicit code, durable state, stable identities, and database constraints remain authoritative, while models are reserved for bounded judgement.

## Status

**IMPLEMENTED · HOSTED DEV.** Standardized through completed OS3 audit (2026-09-14). OS4 Supabase Offload & Backup Foundation is currently active/in progress: Three-Plane Hosted Activation landed on `main` at `e82862b` (Phase B1 remote transport / backup-foundation CLI and initial hosted offload); Core source-table retirement and milestone closeout open.

## What to read

| Document                                  | Best for                                                                                                         |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Deep architecture, shared primitives, data and security boundaries, CI/runtime patterns, testing, and tradeoffs. |
| [User Guide](user-guide.md)               | A practical explanation of where the platform appears and how to interpret system-level status and failures.     |
| [OS Study Guide](../../os-study-guide.md) | The broader Prateek OS architecture and the principles shared by every domain.                                   |

## In this system

- multi-plane persistence across Core Supabase, dedicated News Radar Supabase, and JobOps Neon, with `@prateek-os/backup-foundation` providing backup/restore validation under OS4;
- centralized Patrick Gateway on Railway (`patrick-gateway`) under `@prateek-os/runtime-coordination` distributed lease protection;
- small shared packages for database access, permissions, provenance, events, approvals, and other proven primitives;
- deterministic identity, idempotency, leases, fencing, and bounded failure handling;
- Node.js/TypeScript workspace, CI, formatting, testing, and runtime conventions;
- hosted scheduling and local supervision patterns chosen according to workload and privacy needs.

Platform / Core does not provide a general agent, an OS-wide query router, automatic Brain integration, or arbitrary workflow machinery.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
