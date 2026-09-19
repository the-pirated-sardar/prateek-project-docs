# Prateek Web

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-web`<br>
> **Source baseline:** canonical `main` `007ad31bbfc481ca7621da64cdb7593060aab5c2`<br>
> **Source scope:** `PROJECT_HANDOFF.md`; `docs/`; `package.json`; `astro.config.mjs`; `wrangler.jsonc`; `src/`; `public/`; and `e2e/`<br>
> **Documentation status:** Current

Prateek Web is Prateek's public, recruiter-first portfolio and the presentation layer for selected engineering, creator, and personal work. It is a separate Astro application: it does not run inside Prateek OS and does not depend on the OS runtime.

The site is live on Cloudflare Workers (deployed app SHA `6e57cd6f3787749188de1e5cc3cca49090f55a88` on Worker `133baa05-d748-4d43-8d43-b7a1af87b0b1`). Its current stack is Astro 7, TypeScript, focused React islands, Tailwind CSS 4 as a token engine, repository-owned content, Vitest, and Playwright. W1 is closed. W2 is in progress: W2.5 is complete and deployed; Final W2 production acceptance is pending owner visual signoff on branch `fix/final-w2-production-acceptance` (`eb1b423f3e4d69162f9547dc65f6e724d12c3899`).

## Read next

- [Study Guide](study-guide.md) — architecture, rendering, design system, security, testing, and tradeoffs.
- [User Guide](user-guide.md) — visitor navigation and owner/operator content workflow.
- [Status](STATUS.md) — current deployment, capabilities, debt, and reconciliation triggers.

Further visual refinement, real project assets, and selected external content integrations continue as part of the W2 milestone cycle.
