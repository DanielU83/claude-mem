# Memory (claude-mem stand-in)

A hand-maintained replacement for a memory tool. Drop this file (or its contents) into any project as `CLAUDE.md` — Claude Code reads it automatically and treats it as persistent context. Update it whenever a session teaches you something worth keeping.

## Operating constraints

- **No local installs.** Recent laptop theft — do not install developer tooling, SDKs, or auth credentials on personal machines. Anything that needs a long-running process must run in a cloud environment I control.
- **Default surface is Claude Code on the web** (claude.ai/code). Sessions run in ephemeral containers; anything not committed to git is lost when the container is reclaimed.
- **Anything stateful needs to live elsewhere** — a Codespace, a small VM, or committed back to a repo branch. Don't assume `~/`, `/tmp`, or any path outside the working tree survives a session.

## Working style preferences

- Prefer **action over questions** when intent is reasonably clear; ask only when a choice is irreversible or genuinely ambiguous.
- Default to **concise output**. Multi-paragraph explanations only when the topic warrants it.
- When proposing options, lead with a recommendation and the main tradeoff — don't enumerate every possibility.
- **Trust but verify** any tool I haven't run before (incl. `npx <pkg> install` from untrusted sources). Sandboxed first, then commit.

## Claude Code on the web — known gotchas

- **Ephemeral filesystem.** Only the cloned repo + branch pushes persist. `~/.config`, `~/.claude*`, `/usr/local`, etc. are wiped between sessions.
- **Restricted outbound network.** Some package postinstall scripts (e.g. `tree-sitter-cli` downloading native binaries) hang indefinitely instead of failing fast. If a `bun install` / `npm install` is silent for > 2 minutes, kill it and check whether a child process is fetching a remote binary.
- **No `gh` CLI.** Use the GitHub MCP tools (`mcp__github__*`) for PRs, issues, comments, CI status.
- **Don't poll with `sleep`** for external events (CI, PR comments). The harness wakes the session on its own when events arrive.
- **Commit before the container dies.** Even WIP. Push to a feature branch — losing 20 minutes of work to a reclaim is a self-inflicted wound.

## claude-mem specifically (skipped, but FYI)

- Tried `npx claude-mem@latest install` on 2026-05-16 — hung ~10 min on `tree-sitter-cli` postinstall, killed it.
- Would not have persisted anyway: web container is ephemeral, and claude-mem's DB lives at `~/.claude-mem/claude-mem.db`.
- If I ever want persistent memory: stand up a long-lived dev VM or Codespace, install there once, then use that as my coding environment.

## Best practices for Claude Code sessions

- **Keep this file (or a project-specific `CLAUDE.md`) up to date.** It is the memory.
- **One file per repo** at the root, named `CLAUDE.md`. Project-specific facts, conventions, build commands, gotchas.
- **Don't repeat the obvious.** No "this is a TypeScript project" — Claude can see that. Write down the things a new contributor would only learn by being burned.
- **Capture decisions, not narratives.** "We use `pnpm` not `npm` because of workspace hoisting issues" > "We had a discussion and decided to use pnpm."
- **Record dead ends.** "Tried X, didn't work because Y" prevents Claude from suggesting X again next session.
- **Commit messages and PR descriptions are also memory.** Keep them descriptive — they're searchable forever.

## Tooling notes (extend over time)

_Add learnings here as they come up. Examples below — replace with real ones._

- `gstack` (YC's CEO's stack tool): _TBD — verify what it is and whether it has the same "must run locally" constraint as claude-mem._
- _Tool X: gotcha Y — workaround Z._

## When updating this file

- Keep entries short and concrete.
- Prefer "X because Y" over prose.
- Prune anything that turns out to be wrong — stale memory is worse than no memory.
