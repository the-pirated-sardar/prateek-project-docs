# Prateek Web — User Guide

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-web`<br>
> **Source baseline:** canonical `main` `007ad31bbfc481ca7621da64cdb7593060aab5c2`<br>
> **Source scope:** `src/pages/`; `src/data/`; `src/layouts/`; `src/components/`; `src/lib/auth/`; `src/middleware.ts`; `package.json`; `PROJECT_HANDOFF.md`; and `docs/`<br>
> **Documentation status:** Current

[Documentation home](README.md) · [Technical study guide](study-guide.md) · [Status](STATUS.md)

## Visitor guide

The homepage introduces Prateek's software-engineering identity, highlights the Prateek OS flagship, and links into three top-level areas:

- **Work:** portfolio directory, professional experience, skills, and case studies. Prateek OS has a long-form system tour; other projects use appropriately shorter case-study depths.
- **Creator:** the Pirated Sardar creator identity, production capabilities, and clearly labeled placeholder media/analytics surfaces awaiting real integrations.
- **Personal:** art and writing presented with a distinct warm editorial treatment. Some content remains an honest placeholder or empty state.

The fixed navigation dock supports pointer, keyboard, and touch use. The Resume surface opens the public PDF in a new tab. Contact and professional links are shown only where verified in the site data.

### Protected case studies

A protected page shows a locked teaser and password dialog. If you have authorization, enter the separately supplied password. Success creates a two-hour, route-specific secure session and reloads the page. Without valid authorization, protected content is not included in the response. This is a shared-password reference gate rather than per-user identity; the inspected endpoint has no application-level per-client rate limiter, so production use also depends on upstream abuse controls and strong secret handling. This documentation never publishes credentials.

## Owner/operator guide

### Where content lives

Current content is TypeScript data, not MDX or a CMS:

| Content                                  | Primary area            |
| ---------------------------------------- | ----------------------- |
| Site identity and public links           | `src/data/site.ts`      |
| Work projects and case-study hierarchy   | `src/data/projects.ts`  |
| Prateek OS public chapters/status        | `src/data/prateekOs.ts` |
| Skills and project relationships         | `src/data/skills.ts`    |
| Creator content and metrics placeholders | `src/data/creator.ts`   |
| Personal art/writing placeholders        | `src/data/personal.ts`  |
| Routes and page composition              | `src/pages/`            |

Keep claims factual, preserve the recruiter-first hierarchy, and use explicit empty/placeholder states instead of inventing metrics, screenshots, dates, or testimonials. Adding an MDX file alone does not publish content; MDX is installed but is not the current content authority.

### Local workflow

With the repository's pinned package manager and supported Node version:

```sh
pnpm install
pnpm dev
pnpm build
pnpm preview
```

The repository instructions prefer Astro's background development-server mode when available. Do not place production secrets in committed files. The ordinary site builds without protected-route or analytics configuration; those features fail safely when optional settings are absent.

Before proposing a content or design change, run the full checks documented in the [Study Guide](study-guide.md#10-build-validation-and-deployment). For a focused content edit, at minimum run type checking, content/unit tests, formatting, and a production build; route or interaction changes also require the relevant Playwright coverage and visual inspection.

### Adding or changing a case study

1. Update the typed project record and ensure the status is truthful.
2. Use the existing case-study hierarchy and components unless the content genuinely requires a new pattern.
3. Replace illustrative assets only with approved public material, keeping alt text/captions accurate.
4. Add or update route, content-integrity, navigation, responsive, and accessibility tests.
5. For a protected route, add server-side allowlist/configuration and validate that locked HTML contains no gated content. Never ship a credential or private body in client JavaScript.

### Deployment

Deployment targets Cloudflare Workers and is manual. The high-level flow is: pass quality gates, build, deploy through the authorized Cloudflare workflow, smoke-test canonical and redirect routes, then update the source handoff with both the deployed application SHA and current `main` SHA. Do not assume that a documentation commit means production changed.

### Current limitations and planned evolution

Real Creator analytics/media, some project imagery, Personal art/writing, and a final branded social asset remain future inputs. The design-system page has known low-priority enforcement and focus-detour debt. W1 is closed; W2 is in progress: W2.5 is deployed, and final visual acceptance is pending owner review on branch `fix/final-w2-production-acceptance` (`eb1b423f3e4d69162f9547dc65f6e724d12c3899`).
