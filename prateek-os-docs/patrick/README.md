# Patrick — Interaction Layer

[Documentation home](../README.md) · [OS study guide](../os-study-guide.md) · [Systems index](../systems/README.md)

Patrick is the canonical human-facing interaction identity of Prateek OS. **Prateek** is the human owner, administrator, and actor; **Prateek OS** is the platform; **Patrick** is how that platform presents itself in direct human interaction; and **Pat** is an informal conversational alias. A useful shorthand is:

```text
Prateek
   ↓
Patrick
   ↓
Prateek OS capabilities
```

That shorthand describes the experience, not a hidden super-service. Underneath Patrick, explicit domain systems still own their data, validation, permissions, failure handling, and consequential actions.

## Status

| Horizon              | Meaning                                                                  |
| -------------------- | ------------------------------------------------------------------------ |
| **CURRENT**          | Discord-first interaction identity/layer.                                |
| **FUTURE DIRECTION** | Coherent multi-surface interaction layer across Prateek OS capabilities. |

## CURRENT — What exists today

Patrick is encountered mainly through the existing Prateek OS Discord bot. The visible Discord username “Patrick” is an account-level presentation setting; the Discord application/project remains **Prateek OS**. The repository does not contain one standalone `patrick` service. Current behavior is distributed across the Discord bot/runtime and the domain services it calls.

Accepted Discord interaction includes:

- natural-language and deterministic Capture messages;
- confirmation and Calendar-proposal buttons;
- Personal Ops task, list, and planning commands;
- JobOps notifications and the reaction that requests Application Materials;
- Application Materials command/status delivery surfaces, while generation is currently paused.

Tech News Radar/N1 has local, in-progress Discord code, but it is not hosted, live, or formally accepted. Its planned commands, reactions, notifications, and editorial surfaces are therefore **not current Patrick behavior**.

Patrick is the interface, not the authority. A conversational request does not create permission. The authenticated actor and role determine access, and each target system preserves its own authorization and approval rules. Patrick is also not canonical storage, a database, Recall, the Brain, a particular language model, or the whole operating system.

## FUTURE DIRECTION — One assistant experience

The long-term product direction is for Patrick to feel like one coherent assistant across Discord, chat, voice, calls, and other appropriate surfaces. “Jarvis-like” is useful only as an interaction/product-experience analogy: one recognizable identity, natural interaction, context-aware routing, continuity, and bounded proactive help across many underlying capabilities. It does **not** mean fictional omniscience, AGI, unrestricted autonomy, universal data access, or bypassing approval.

Future Patrick may classify a request broadly as Capture, Query, or Command and route it to the system that owns the work. Recall may provide provenance-preserving answers over authoritative sources. Explicit Prateek OS ↔ Brain integration may later provide authorized context. None of those integrations exists today, and Patrick could never grant itself Brain access or become an authorization principal.

The architectural goal is **one assistant, not one giant service**: consistent interaction above multiple bounded systems, not one opaque agent with universal credentials.

## Read next

- [Study Guide](study-guide.md) — current implementation, boundaries, future routing architecture, safety, testing, and failure behavior.
- [User Guide](user-guide.md) — the current Discord front door and links to system-specific instructions.
- [Product Vision](vision.md) — the explicitly future-facing, multi-surface Patrick direction.
- [OS Study Guide](../os-study-guide.md) — shared platform architecture and engineering principles.
- [Systems Index](../systems/README.md) — the domain systems behind Patrick.
