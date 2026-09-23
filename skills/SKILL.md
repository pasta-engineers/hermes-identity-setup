---
name: hermes-identity-setup
description: "Use when building Hermes identity files."
version: 1.0.0
license: MIT
metadata:
  hermes:
    tags: [hermes, identity, personality, memory, soul, user-profile, setup, onboarding]
---

# Hermes Identity Setup

Hermes is shaped by four markdown layers, each with a **different job and a different writer**. When a user asks to set up / refresh the agent's identity or profile, this skill maps each file, runs a short interview, and routes each answer to the correct file. The goal is improved user-familiarity and agent-purpose, not dumping content everywhere.

## The four files (authoritative summary)

| File | Holds | Writer | Inject timing | Location |
|------|-------|--------|---------------|----------|
| **SOUL.md** | Agent identity: personality, tone, voice, style-avoids | **User** (Hermes seeds a starter if absent; never overwrites existing) | Verbatim as system-prompt slot #1 at session start | `~/.hermes/SOUL.md` ($HERMES_HOME) |
| **USER.md** | User profile: name, role, prefs, communication style, expectations | **Agent** via `memory` tool | Frozen snapshot at session start | `~/.hermes/memories/` |
| **MEMORY.md** | Agent's notes: environment facts, conventions, tool quirks, lessons | **Agent** via `memory` tool | Frozen snapshot at session start | `~/.hermes/memories/` |
| **AGENTS.md** / `.hermes.md` | Project instructions / rules | **User** | Loaded at startup from cwd, parent walk to git root | project dir |

Shorthand: **SOUL.md = who the agent IS** · **USER.md = who YOU are** · **MEMORY.md = what the agent has LEARNED** · **AGENTS.md = what the PROJECT needs.**

Source of truth in the repo: `website/docs/user-guide/which-file-does-what.md`. Linked depth: `features/personality`, `features/memory`, `features/context-files`.

## Hard rules / pitfalls

- **SOUL.md and USER.md never feed each other.** Facts about the user belong in USER.md (agent-written), NOT in SOUL.md. Editing SOUL.md won't populate memory; memory won't change the persona. Editing both is the common mix-up.
- **All identity files are a FROZEN SNAPSHOT at session start.** Edits (to SOUL.md, USER.md, MEMORY.md, or AGENTS.md) apply only from the NEXT session. Tell the user to restart the session — mid-session the model can still act on what's in context, but the injected block shows session-start state.
- **USER.md and MEMORY.md are AGENT-WRITTEN, not hand-edited.** The flow is: user answers → agent saves via the `memory` tool (gated by `write_approval`) → inspect/edit via `hermes journey` (list/edit). Directly editing these by hand is off-pattern; the memory tool manages budget and merging.
- **Memory char budget is small and finite** (per-profile limit, e.g. ~2,200 chars). Teach the user a **Tier 1 (must-always-know) / Tier 2 (nice-to-have)** split and prioritize saves; consolidate/trim stale entries to make room.
- Keep question counts low (target ~12 total or a 4-6 question minimum-viable subset) — the ask is an onboarding interview, not a tax form.

## Workflow

1. Agree on the target files (SOUL.md agent persona / USER.md profile / MEMORY.md always-know facts / a project AGENTS.md).
2. Run a short interview — question scripts and per-file tagging live in `references/identity-files.md`. Each question should carry a "→ goes into <file>" tag.
3. Route answers:
   - SOUL.md answers → user reviews and edits the file (or agent drafts it for user review).
   - USER.md answers → agent saves to the `user` memory target.
   - MEMORY.md answers → agent saves to the `memory` target (consolidate/trim first if near budget).
4. Warn the user once: restart the session for the injected blocks to refresh.

## Support files

- `references/identity-files.md` — the doc master table plus the full interview scripts (SOUL.md / USER.md / MEMORY.md) with per-question file routing.
