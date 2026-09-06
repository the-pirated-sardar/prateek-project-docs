# Prateek Web

> **Last updated:** 2026-09-05<br>
> **Source repository:** `prateek-web`<br>
> **Source baseline:** `71c77c0673420925d705b48b193713e7c3c6a374`<br>
> **Source scope:** `PROJECT_HANDOFF.md`; `docs/w1/`; `package.json`; `astro.config.mjs`; `wrangler.jsonc`; `src/`; `public/`; and `e2e/`<br>
> **Documentation status:** Current

Prateek Web is Prateek's public, recruiter-first portfolio and the presentation layer for selected engineering, creator, and personal work. It is a separate Astro application: it does not run inside Prateek OS and does not depend on the OS runtime.

The W1 site is live on Cloudflare Workers. Its current stack is Astro 7, TypeScript, focused React islands, Tailwind CSS 4 as a token engine, repository-owned content, Vitest, and Playwright. The deployed application baseline and the newer repository tip are deliberately tracked separately in [Status](STATUS.md).

## Read next

- [Study Guide](study-guide.md) — architecture, rendering, design system, security, testing, and tradeoffs.
- [User Guide](user-guide.md) — visitor navigation and owner/operator content workflow.
- [Status](STATUS.md) — current deployment, capabilities, debt, and reconciliation triggers.

Further visual refinement, real project assets, and selected external content integrations are future work. They do not overwrite the behavior of the live W1 site described here.
