# Identity files: doc map + interview scripts

Condensed from `website/docs/user-guide/which-file-does-what.md` (source of truth).

## Master table (verbatim detail)

- **SOUL.md** — agent's primary identity (personality, tone, communication style, what to avoid stylistically). User writes it; Hermes seeds a starter file if absent, never overwrites existing. Inject: system-prompt slot #1 at session start. Location: `~/.hermes/SOUL.md` (or `$HERMES_HOME/SOUL.md`) — never the working directory.
- **USER.md** — user profile (name, role, preferences, communication style, expectations). Agent writes via `memory` tool (gate with `write_approval`; edit via `hermes journey edit`). Inject: frozen snapshot at session start. Location: `~/.hermes/memories/`.
- **MEMORY.md** — agent's personal notes (environment facts, project conventions, tool quirks, things learned). Agent writes via `memory` tool (same gating/editing). Inject: frozen snapshot at session start. Location: `~/.hermes/memories/`.
- **AGENTS.md** — project instructions/conventions/architecture, cwd-only, loaded at startup; nested copies discovered progressively.
- **.hermes.md** / **HERMES.md** — Hermes-specific project rules, highest priority, parent walk to git root. One project context file per session (`.hermes.md` → AGENTS.md → CLAUDE.md → .cursorrules); SOUL.md always loaded independently (not in that chain).

## Interview scripts

### SOUL.md — who the agent IS (personality, tone, voice)
1. What should your agent feel like? (formal assistant / sharp technical colleague / calm mentor)
2. Tone and style — direct & terse vs warm; jargon vs plain; emoji/formatting?
3. Language behavior — default language, when to switch (technical terms, code, docs)
4. How to handle not knowing — say plainly / ask follow-ups / labeled assumption and proceed
5. One "never" stylistically (no filler, no apologizing, no restating the request) — stronger than N preferences

Goal, purpose, and boundaries also live in SOUL.md (the "who the agent is" layer):
6. What is this agent FOR — general assistant / coding pair / study companion / startup co-founder; rank them
7. Top 3-5 jobs handed most often (where it should invest effort)
8. Take initiative vs wait — proactive flags (updates, security, follow-ups) or strict execution
9. Line never to cross (destructive commands w/o approval, fabricating results, skipping tests)

### USER.md — who YOU are (profile for familiarity)
10. Who are you in one paragraph? (name, role, experience level, deep domains)
11. Answer sizing — one-liner for simple asks vs full walkthroughs; when to go deep (learning / high-stakes)
12. Taste in corrections — blunt vs framed with alternatives
13. Current goals — career track, studies, startup ideas (lets agent connect tasks to bigger goals)

### MEMORY.md — what you want the agent to ALWAYS know
Environment & setup:
1. Your machine(s) at a glance — OS, hardware, roles; first thing I should know
2. Key paths on disk — vaults, repos, dotfiles, servers; which must never be lost
3. Tools used daily — editors/CLIs/services + access (env vars, auth files, ports)

Conventions & workflows:
4. Default technical habits — formatter/linter, package manager, test runner (every task inherits)
5. Your "done" definition — tests pass, docs updated, commit style, branch naming
6. Recurring rhythms — weekly reviews, cadence, deadlines to track across sessions
7. Standing rules with no other home — "never touch config.yaml by hand", "use hermes config set", "don't touch other profile"

Lessons & hygiene:
8. Things corrected on an agent more than once (top 2-3 memory killers)
9. What should never be remembered (inverse — keeps budget clean)

## Minimum-viable subsets
- Identity: #1, #2, #4 (agent persona), #6–#9 (purpose), #10, #11 (profile)
- Memory: #1, #4, #5, #8
Each answer should carry a "→ <file>" tag for routing.
