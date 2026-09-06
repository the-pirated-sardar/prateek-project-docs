# Platform / Core

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`<br>
> **Source scope:** `packages/`; `apps/discord-bot/`; `ops/`; `supabase/migrations/`; `docs/{ARCHITECTURE,PRIVACY,MODEL_POLICY}.md`; and `docs/adr/`<br>
> **Documentation status:** Current

Platform / Core is the shared foundation beneath Prateek OS. It provides reusable infrastructure and engineering conventions without absorbing domain rules into a central “god service.” Its design is deterministic-first: explicit code, durable state, stable identities, and database constraints remain authoritative, while models are reserved for bounded judgement.

## Status

**IMPLEMENTED.** The accepted platform foundations target hosted DEV rather than Supabase PROD.

## What to read

| Document                                  | Best for                                                                                                         |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | Deep architecture, shared primitives, data and security boundaries, CI/runtime patterns, testing, and tradeoffs. |
| [User Guide](user-guide.md)               | A practical explanation of where the platform appears and how to interpret system-level status and failures.     |
| [OS Study Guide](../../os-study-guide.md) | The broader Prateek OS architecture and the principles shared by every domain.                                   |

## In this system

- Supabase/PostgreSQL persistence, transactions, migrations, constraints, and row-level security;
- Patrick on Discord as an interaction surface, with authorization owned by explicit actor and channel policy;
- small shared packages for database access, permissions, provenance, events, approvals, and other proven primitives;
- deterministic identity, idempotency, leases, fencing, and bounded failure handling;
- Node.js/TypeScript workspace, CI, formatting, testing, and runtime conventions;
- hosted scheduling and local supervision patterns chosen according to workload and privacy needs.

Platform / Core does not provide a general agent, an OS-wide query router, automatic Brain integration, or arbitrary workflow machinery.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
