# Tech News Radar

Tech News Radar is the N1 editorial tech-news system. Its intended job is to turn bounded public-source observations into a deduplicated, clustered, personalized editorial stream, while preserving evidence and keeping editorial selection distinct from automatic publication.

## Status

**IN PROGRESS — LOCAL / NOT YET FORMALLY ACCEPTED.** The documentation describes a coherent local implementation snapshot. No hosted migration, live Discord registration, scheduled live runtime, or owner acceptance is claimed, and the workflows are not usable today.

## What to read

| Document                                  | Best for                                                                                                                                  |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| [Study Guide](study-guide.md)             | In-progress architecture, canonicalization, clustering, editorial ranking, feedback, delivery, pipeline design, testing, and limitations. |
| [User Guide](user-guide.md)               | Intended channels, reactions, controls, Editorial Story Pipeline workflows, and current non-live constraints.                             |
| [OS Study Guide](../../os-study-guide.md) | Shared canonical-state, model-boundary, scheduling, and delivery principles.                                                              |

## In this system

- a broad Firehose after validation and deduplication;
- a concise editorial Radar, rare Breaking surface, and daily Digest;
- developing-story clustering and deterministic features before optional grounded editorial judgement;
- replay-safe feedback that records bounded preference evidence;
- missed-story submission and coverage-gap diagnosis;
- an Editorial Story Pipeline for primary/secondary selection, stashing, trimming, resetting, and evidence-grounded pointer-outline release.

These are locally implemented or intended N1 surfaces, not hosted/live capabilities. Release produces an editorial outline rather than a spoken script or automatic publication.

## Related systems

- [Platform / Core](../platform-core/README.md) supplies the shared persistence, security, scheduling, and Discord conventions N1 follows.

## Navigation

[All systems](../README.md) · [Prateek OS documentation](../../README.md)
