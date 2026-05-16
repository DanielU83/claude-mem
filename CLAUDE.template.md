# CLAUDE.md

> Hand-maintained memory file. Rename to `CLAUDE.md` and place at the root of any project — Claude Code reads it automatically as persistent context.
>
> Structure and rules are modeled on what `claude-mem` chooses to retain when compressing a session. Treat each section like a database with strict schemas, not a journal.

---

## How to use this file

- **Update at end of session**, not as you go. Compress to the things future-you (or future-Claude) will need.
- **One fact per line.** Self-contained, no pronouns referring to other entries.
- **Anchor with file paths** (`src/foo/bar.ts:42`) so entries are greppable.
- **Prefer terse over complete.** If you have to choose, drop the entry. Less is more.
- **Prune on contact.** If you read an entry and it's stale or wrong, fix it or delete it the same session.

---

## Project context

_Slow-changing facts about the project. Update only when reality changes._

- **What it is:** _one sentence._
- **Stack:** _languages, frameworks, key services._
- **Entry points:** _main binaries, dev server commands, primary scripts._
- **Where state lives:** _database, cache, config files — full paths._
- **External dependencies that matter:** _APIs, queues, anything that can break independently._

---

## Build & run

_Exact commands, in copy-paste form. No prose around them._

```bash
# install
…
# dev
…
# test
…
# lint / typecheck
…
# deploy / ship
…
```

---

## Decisions

_Architectural or convention choices, each with the **why**. Format: `Decision — because Reason.` No essays._

- _Example:_ Use `pnpm` over `npm` — workspace hoisting bugs in npm 10 broke our monorepo builds.
- _Example:_ SQLite over Postgres for local cache — zero-config, single-file backup, performance is fine at our scale.

---

## Gotchas

_Things that bite. Each entry: **symptom → cause → fix**. File-path anchors where relevant._

- _Example:_ Worker hangs on startup → port 37700 already bound by stale process → `npm run worker:stop` then restart.
- _Example:_ `tree-sitter-cli` postinstall hangs in restricted sandboxes → no fast-fail on blocked CDN → kill the install, set `SKIP_DOWNLOAD=1`.

---

## Discoveries

_Durable findings from debugging or code archaeology. Things that took effort to learn and you'd hate to learn twice._

- _Example:_ `src/services/worker-service.ts:142` — the `start` command is idempotent only if `worker.pid` is fresh; stale pidfile causes double-binding.
- _Example:_ Chroma's embedding endpoint silently truncates inputs > 8192 tokens — we chunk in `src/services/sync/ChromaSync.ts:88`.

---

## Dead ends

_Approaches that were tried and failed. Prevents re-trying the same wrong path next session._

- _Example:_ Tried using `bun:sqlite` for the main DB — incompatible with FTS5 extensions we depend on, reverted to `better-sqlite3`.
- _Example:_ Tried running the worker as a systemd unit — Codespaces don't expose systemd, switched to a supervised child process.

---

## Patterns / conventions

_Repeated solutions worth codifying. House style._

- _Example:_ Hook scripts dispatch to one unified worker via `bun-runner.js`; new hooks add a subcommand, never a new script.
- _Example:_ All user-facing strings live in `src/i18n/`. No inline literals in components.
- _Example:_ Privacy: wrap any user-sensitive context in `<private>…</private>` before it reaches a tool.

---

## File-path map

_Where the important things live. Update when you move files._

- _Entry point:_ `…`
- _Config:_ `…`
- _Tests:_ `…`
- _Background worker:_ `…`
- _Build scripts:_ `…`

---

## Session log

_End-of-session summary, one entry per working session. Newest on top. Format below — keep each session to ~5 lines._

### YYYY-MM-DD — _one-line goal_

- **Investigated:** _what was looked at._
- **Learned:** _the new fact(s)._
- **Changed:** _files modified, in a sentence._
- **Next:** _the obvious next step, if any._
- **Open questions:** _things still unresolved (keep short; promote to Gotchas/Discoveries if they outlive the session)._

---

## Anti-patterns when updating this file

_Borrowed directly from how `claude-mem` filters its own observations._

- **No meta-narration.** Write what the project does, not what you (or Claude) did about it. ✗ "I analyzed the worker and decided…" ✓ "Worker uses port 37700; collisions auto-resolve via `uid % 100`."
- **No chronological prose.** ✗ "First we tried X, then Y, finally Z." ✓ Three separate entries under Decisions / Dead ends.
- **No vague hedging.** ✗ "might affect performance" ✓ "increases p95 latency by ~40 ms on the hot path (`src/api/search.ts:91`)."
- **No "skip" notes.** If an entry isn't worth keeping, delete it — don't leave `~~struck-through~~` lines or `// TODO revisit` markers.
- **No subagent details.** Capture outcomes, not the nested tool calls that produced them.
- **Type ≠ concept.** A `Decision` entry belongs under Decisions; don't also tag the word "decision" inside it. Sections are the taxonomy.

---

## When this file gets long

- **Archive, don't append forever.** Move resolved Gotchas, completed Next steps, and old Session log entries to `docs/CLAUDE-archive.md`.
- **Promote.** A Discovery that's referenced repeatedly becomes a Pattern. A Pattern that's universal becomes part of Project context.
- **Rule of thumb:** if `CLAUDE.md` exceeds ~300 lines, you're hoarding. Compress.
