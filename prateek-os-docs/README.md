# Prateek OS

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-os`<br>
> **Source baseline:** accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (derived from canonical `main` `e82862bbbaee4cc7a847897b49ad05c23ceab387`)<br>
> **Source scope:** `docs/{PROJECT,ARCHITECTURE,PRIVACY,MODEL_POLICY,ROADMAP,MILESTONES}.md`; `docs/source/active/`; `docs/adr/`; `docs/reviews/`; `services/`; `apps/`; `packages/`; and `supabase/migrations/`<br>
> **Documentation status:** Mixed current/future

Prateek OS is a private personal operating system made of focused services rather than one all-powerful agent. It currently supports low-friction capture, personal planning, job discovery and ranking, and tech news radar.

The design is deterministic-first: databases, explicit rules, stable identities, and state machines remain authoritative. Raw inputs retain provenance; derived classifications and scores are rebuildable. High-consequence mutations such as Calendar writes have explicit approval gates and fail closed at those boundaries. Patrick, the Discord identity, is the interaction surface—not an authorization principal or source of truth. Structured live state is split across Core Supabase (Capture, Personal Ops, Patrick Core state), dedicated News Radar Supabase, and JobOps Neon; `@prateek-os/backup-foundation` provides the accepted backup/restore foundation under active OS4 development (production scheduling, retention, NAS destination, restore drills, and any optional cold offload remain unfinished).

> **Public documentation baseline — 2026-09-19.** Reconciled against accepted cleanup HEAD `d67e5cf543e62636a6dfa0ee96d35835028e1e9d`. Tech News Radar (N1) is **COMPLETE and merged to canonical `main`** (2026-09-13). OS3 System Audit & Standardization is **CLOSED** (2026-09-14); OS4 Supabase Offload & Backup Foundation is **IN PROGRESS** (Three-Plane hosted data cutover landed on `main` at `e82862b`; backup foundation CLI exists; source-table retirement, production backup deployment, and closeout open). See the concise [documentation status](STATUS.md).

## What this documentation contains

Start with the [systems index](systems/README.md) for quick navigation or the [OS Study Guide](os-study-guide.md) for the shared architecture.

### Interaction layer

**[Patrick](patrick/README.md)** is the canonical human-facing interaction identity/layer of Prateek OS. **Current:** Discord-first; hosted as persistent Patrick Gateway on Railway (`patrick-gateway`) under distributed lease coordination. **Future direction:** a coherent multi-surface interaction layer. Patrick is not a domain subsystem or authorization principal; see the [Patrick Study Guide](patrick/study-guide.md) and [Product Vision](patrick/vision.md).

### Domain systems

| System                | What it does                                                                              | Study Guide                                           | User Guide                                         | Status                                                  |
| --------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------- |
| Platform / Core       | Shared monorepo, database, security, runtime, CI, and engineering patterns                | [Study](systems/platform-core/study-guide.md)         | [Use](systems/platform-core/user-guide.md)         | **IMPLEMENTED · HOSTED DEV (OS3 closed, OS4 active)**   |
| JobOps                | Discovers, deduplicates, ranks, and notifies about jobs                                   | [Study](systems/jobops/study-guide.md)                | [Use](systems/jobops/user-guide.md)                | **IMPLEMENTED · HOSTED DEV (Neon data plane)**          |
| Application Materials | Produces evidence-grounded resume and cover-letter packages                               | [Study](systems/application-materials/study-guide.md) | [Use](systems/application-materials/user-guide.md) | **DORMANT · RETIREMENT IN J5 (worker paused)**          |
| Capture               | Preserves and interprets Discord/mobile inputs, with deterministic and model-backed paths | [Study](systems/capture/study-guide.md)               | [Use](systems/capture/user-guide.md)               | **IMPLEMENTED · HOSTED DEV (Core Supabase)**            |
| Personal Ops          | Owns tasks, deterministic planning, approval-gated Calendar writes, and a Tasks mirror    | [Study](systems/personal-ops/study-guide.md)          | [Use](systems/personal-ops/user-guide.md)          | **IMPLEMENTED · HOSTED DEV (Core Supabase)**            |
| Tech News Radar       | Builds a personalized editorial news desk, Replay Lab, and story-selection pipeline       | [Study](systems/tech-news-radar/study-guide.md)       | [Use](systems/tech-news-radar/user-guide.md)       | **IMPLEMENTED · HOSTED DEV (closed/merged 2026-09-13)** |

## Current system map

```mermaid
flowchart LR
    U[Owner / authorized users] --> P[Patrick Gateway on Railway]
    M[iOS Shortcuts] --> API[Capture API]
    P --> C[Capture]
    API --> C
    C --> PO[Personal Ops]
    JO[JobOps] --> P
    JO -. queue .-> AM[Application Materials\nDORMANT]
    PO --> G[Google Calendar / Tasks]
    N1[Tech News Radar] --> P
    C & PO --> DB1[(Core Supabase)]
    JO --> DB2[(JobOps Neon)]
    N1 --> DB3[(News Radar Supabase)]
    DB1 & DB2 & DB3 -. backup / restore foundation .-> BF[@prateek-os/backup-foundation\nOS4 IN PROGRESS]
```

Patrick is shown above as the interaction layer connecting people to capabilities, not as another domain system. See the [Patrick documentation](patrick/README.md).

Implemented systems run against hosted **DEV**, not Supabase PROD. Patrick Gateway runs persistently on Railway (`patrick-gateway`) under `@prateek-os/runtime-coordination` distributed leases (`patrick:discord-gateway`). JobOps scheduling, News Radar scheduling, and Capture API run as independent Railway processes. The Application Materials worker is dormant (generation paused, dedicated rehosting deferred, and owner-facing retirement scheduled in J5).

## Roadmap / what's next

- **N1 — Tech News Radar:** complete, closed, and merged to canonical `main` on 2026-09-13 (`1b6e746`/`586d736`). Centralized Patrick runtime, Replay Lab, and rolling judgment budget are live on hosted DEV.
- **OS3 — System Audit & Standardization:** complete, closed on 2026-09-14 (`os3-final-closeout.md`).
- **OS4 — Supabase Offload, Backup Foundation & Quota Remediation:** active / in progress. Three-Plane Hosted Activation landed on `main` at `e82862b` (Three-Plane hosted data cutover and backup-foundation CLI); Core source-table retirement, production backup deployment, and milestone closeout open.
- **R1 — Reference Library:** next OS milestone; saved-media-reference system; scope still unfrozen; distinct from RC1 Recall.
- **J5 — JobOps Health, Feedback & Application-Materials Retirement:** scheduled immediately after R1.
- **J3 — JobOps CRM / outreach / follow-ups / interview preparation:** deferred without planned date (TBD).
- **Application Materials:** dedicated rehosting is deferred without planned date; owner-facing retirement scheduled in J5.
- The separate [Prateek Brain](../prateek-brain-docs/README.md) track runs in its own repository (`prateek-brain`): B1 and B2 are complete; B3 is in progress (B3.0–B3.6 complete, B3.7 next). Patrick does not currently read from it.
- The public portfolio is documented separately as [Prateek Web](../prateek-web-docs/README.md): W1 is complete; W2.5 is deployed; Final W2 owner visual acceptance is pending.

## Documentation philosophy

Implementation repositories remain authoritative for code and current engineering state. This public repository explains the architecture, user workflows, tests, tradeoffs, and reusable lessons. Repository paths are included as provenance even when the implementation repository is private. Security-sensitive configuration, personal data, private source inventories, full production prompts, and exact private preference profiles are intentionally omitted.
