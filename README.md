# Prateek Project Documentation

> **Last updated:** 2026-09-19<br>
> **Source repository:** Multiple<br>
> **Source baseline:** `prateek-os@d67e5cf543e62636a6dfa0ee96d35835028e1e9d` (accepted cleanup HEAD); `prateek-brain@507be98f51ebece7d7c36d593786df39f0523e73` (canonical `main`); `prateek-web@007ad31bbfc481ca7621da64cdb7593060aab5c2` (canonical `main`)<br>
> **Source scope:** repository navigation and project documentation homes<br>
> **Documentation status:** Current

This repository contains public supporting documentation for Prateek's technical projects. Deep architecture, operational lessons, and system blueprints can live here without turning implementation repositories into documentation archives.

Some implementation repositories may remain private temporarily or permanently. These documents keep their public engineering value available while deliberately omitting private data, security-sensitive details, and personal operating rules. They are also working engineering references: systems of this size are too broad for any reasonable developer to hold every boundary and failure mode in memory.

Some projects may later receive cleaned, public, forkable releases. Until then, this repository provides case studies and practical blueprints for developers building similar systems.

## Projects

- [Prateek OS](prateek-os-docs/README.md) — a structured personal operating system with focused domain services, deterministic-first execution, multi-plane data architecture, and Patrick as its interaction layer. **Current, with OS4 in progress.**
- [Prateek Web](prateek-web-docs/README.md) — the public recruiter-first portfolio and project-presentation website. **Live; W1 closed, W2.5 deployed, W2 overall in progress.**
- [Prateek Brain](prateek-brain-docs/README.md) — a separate local-first memory, indexing, retrieval, and context architecture. **B1 and B2 complete; B3 in progress.**

## Documentation maintenance

See [Maintenance](MAINTENANCE.md) for the source-baseline and reconciliation policy used throughout this repository.
