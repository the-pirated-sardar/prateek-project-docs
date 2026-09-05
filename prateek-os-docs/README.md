# Prateek OS

Prateek OS is a private personal operating system made of focused services rather than one all-powerful agent. It currently supports low-friction capture, personal planning, job discovery and ranking, and application-material preparation. Tech News Radar is being built as the next subsystem.

The design is deterministic-first: databases, explicit rules, stable identities, and state machines remain authoritative. Raw inputs retain provenance; derived classifications and scores are rebuildable. High-consequence mutations such as Calendar writes have explicit approval gates and fail closed at those boundaries. Patrick, the Discord identity, is the interaction surface—not an authorization principal or source of truth. Supabase/PostgreSQL stores structured live state, while hosted and local runtimes are used according to workload and privacy needs.

> **Public documentation baseline — 2026-09-05.** Canonical `main` was observed at `76edfe8635c5abf075c12e47eaa39b70f1b1bce5`. N1 was separately inspected on the clean, committed `feat/n1-tech-news-radar` branch at `9693f07e4dd787173583e080d4a5beab7b576ce4`. All N1 implementation described here is **IN PROGRESS — LOCAL / NOT YET FORMALLY ACCEPTED** unless explicitly stated otherwise. See the concise [documentation status](STATUS.md).

## What this documentation contains

Start with the [systems index](systems/README.md) for quick navigation or the [OS Study Guide](os-study-guide.md) for the shared architecture.

| System                | What it does                                                                              | Study Guide                                           | User Guide                                         | Status                               |
| --------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------- | -------------------------------------------------- | ------------------------------------ |
| Platform / Core       | Shared monorepo, database, security, runtime, CI, and engineering patterns                | [Study](systems/platform-core/study-guide.md)         | [Use](systems/platform-core/user-guide.md)         | **IMPLEMENTED**                      |
| JobOps                | Discovers, deduplicates, ranks, and notifies about jobs                                   | [Study](systems/jobops/study-guide.md)                | [Use](systems/jobops/user-guide.md)                | **IMPLEMENTED · HOSTED DEV**         |
| Application Materials | Produces evidence-grounded resume and cover-letter packages                               | [Study](systems/application-materials/study-guide.md) | [Use](systems/application-materials/user-guide.md) | **IMPLEMENTED · RUNTIME PAUSED**     |
| Capture               | Preserves and interprets Discord/mobile inputs, with deterministic and model-backed paths | [Study](systems/capture/study-guide.md)               | [Use](systems/capture/user-guide.md)               | **IMPLEMENTED · HOSTED DEV**         |
| Personal Ops          | Owns tasks, deterministic planning, approval-gated Calendar writes, and a Tasks mirror    | [Study](systems/personal-ops/study-guide.md)          | [Use](systems/personal-ops/user-guide.md)          | **IMPLEMENTED · HOSTED DEV**         |
| Tech News Radar       | Builds a personalized editorial news desk and story-selection pipeline                    | [Study](systems/tech-news-radar/study-guide.md)       | [Use](systems/tech-news-radar/user-guide.md)       | **IN PROGRESS · LOCAL / UNACCEPTED** |

## Current system map

```mermaid
flowchart LR
    U[Owner / authorized users] --> P[Patrick on Discord]
    M[iOS Shortcuts] --> API[Capture API]
    P --> C[Capture]
    API --> C
    C --> PO[Personal Ops]
    JO[JobOps] --> P
    JO --> AM[Application Materials]
    PO --> G[Google Calendar / Tasks]
    C & JO & AM & PO --> DB[(Supabase PostgreSQL)]
    N1[Tech News Radar\nIN PROGRESS] -. planned Discord surfaces .-> P
```

Implemented systems run against hosted **DEV**, not Supabase PROD. JobOps scheduling runs on Railway; Patrick and the optional Application Materials worker use independently supervised local runtimes. Application Materials generation is presently paused even though its accepted persistence, recovery, validation, and approval architecture remains documented.

## Roadmap / what's next

- **N1 — Tech News Radar:** active, local, not yet formally accepted. Source ingestion, canonicalization, clustering, editorial evaluation, Discord wiring, and the Editorial Story Pipeline exist in the inspected snapshot; hosted and owner acceptance remain.
- **R1 — Reference Library, Y1 — YouTube Analyst, CR1 — Creator Intelligence, W2, PI1, OS1, PX1, OS2:** scheduled future OS-track milestones.
- **J3 — JobOps CRM / outreach / follow-ups / interview preparation:** scheduled late because current JobOps is operationally sufficient.
- **C4 — Capture Refinement & Extensions** and **RC1 — Recall:** scheduled at the roadmap tail; C4 is deliberately unfrozen, while Recall is defined only as a future provenance-preserving read side over existing systems.
- The separate Brain track has its own repository and lifecycle. Patrick does not currently read from it. General News Radar, price monitoring, call capture, physical-item archiving, and other concepts remain future/unscheduled.

## Documentation philosophy

Implementation repositories remain authoritative for code and current engineering state. This public repository explains the architecture, user workflows, tests, tradeoffs, and reusable lessons. Repository paths are included as provenance even when the implementation repository is private. Security-sensitive configuration, personal data, private source inventories, full production prompts, and exact private preference profiles are intentionally omitted.
