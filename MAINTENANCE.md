# Documentation Maintenance

> **Last updated:** 2026-09-19<br>
> **Source repository:** `prateek-project-docs`<br>
> **Source baseline:** cross-repository reconciliation tracking `prateek-os@d67e5cf543e62636a6dfa0ee96d35835028e1e9d`, `prateek-brain@507be98f51ebece7d7c36d593786df39f0523e73`, and `prateek-web@007ad31bbfc481ca7621da64cdb7593060aab5c2`<br>
> **Source scope:** all public Markdown, project indexes, metadata convention, links, and publication-safety checks<br>
> **Documentation status:** Current

Implementation repositories are authoritative. This public repository is an explanatory and reference layer: it summarizes accepted architecture, operator behavior, tradeoffs, and clearly labeled future direction without becoming a competing product specification.

## Required metadata

Every Markdown document records a compact header near its title:

- **Last updated:** the date the public document was substantively reconciled with its authority, not the feature's implementation or commit date.
- **Source repository:** the authoritative implementation repository, `Multiple` when genuinely cross-repository, or this docs repository for repository-only policy.
- **Source baseline:** an exact canonical-main SHA for current behavior. An exact feature snapshot may be used for intentionally in-progress documentation, but must not be presented as canonical.
- **Source scope:** stable directories and authority areas whose changes could invalidate the document.
- **Documentation status:** normally `Current`, `In progress`, `Planned`, `Historical`, `Vision`, or `Mixed current/future`.

## Reconciliation workflow

```text
read metadata
    ↓
fetch current source SHA
    ↓
diff recorded baseline → current baseline
    ↓
constrain diff to Source scope
    ↓
inspect new authority / implementation changes
    ↓
update only affected documentation
    ↓
record new date + SHA
    ↓
audit + publish
```

Start by recovering current source `main`, its relationship to the checked-out branch, and working-tree state. Do not assume that the checked-out branch, a remote-tracking ref, and canonical `main` are equal. For a current document, compare its recorded canonical baseline with the newly observed canonical baseline. For an in-progress document, compare the recorded feature snapshot with the exact new feature snapshot while continuing to identify canonical `main` separately.

Constrain the comparison to the document's Source scope, for example:

```sh
git diff <recorded-baseline>..<new-baseline> -- <source-paths>
```

A document needs reconciliation when that bounded diff changes an implementation, contract, test, status, deployment record, or authority that could invalidate one of its claims. Inspect relevant new ADRs, reviews, migrations, milestones, tests, and operational evidence. Reconcile implementation, hosted-DEV, deployed/live, accepted, merged, planned, and vision language explicitly; an accepted feature snapshot does not become canonical merely because its review passed.

Only affected public documents normally need updates; a whole-repository rewrite is usually unnecessary. In-progress work stays labeled in progress until its source authority accepts it, and its baseline changes to canonical `main` only when canonical history actually includes it. After edits, repeat formatting, relative-link and anchor validation, Mermaid checks, metadata coverage, and the full public-safety scan before publication. That safety pass must recheck secrets, credentials, private identifiers and paths, personal-memory examples, source inventories, prompts, scoring details, and deployment/account identifiers rather than assuming an earlier redaction remains sufficient.
