# agent-rules

Universal rules for AI coding agents (Claude Code, Codex, Cursor, Aider, etc.) working on any of Tal's repos.

Each repo's own `AGENTS.md` references this file and adds only repo-specific details.

---

## STOP AND WAIT — Git & Deploy, No Exceptions

> **NEVER COMMIT OR PUSH WITHOUT ASKING FIRST AND WAITING FOR AN EXPLICIT YES!!!**
> **NEVER COMMIT OR PUSH WITHOUT ASKING FIRST AND WAITING FOR AN EXPLICIT YES!!!**
> **NEVER COMMIT OR PUSH WITHOUT ASKING FIRST AND WAITING FOR AN EXPLICIT YES!!!**

These actions each require a **separate explicit confirmation** — approval of one does NOT authorize the next:

| Action | Rule |
|--------|------|
| `git commit` | Ask "Ready to commit?" — wait for yes |
| `git push` | Ask separately after commit |
| `gh pr create` | Ask separately after push |
| `gh pr merge` | Ask separately — never auto-merge |
| `git branch -d` | Ask separately |
| Deploy / `wrangler deploy` / `adb install` | Only on "עלה לאתר" / "deploy" / "sync" / explicit approval |

> **Do NOT chain steps automatically.** Stop after each step, summarize, and wait.
> **Announcing "מתחיל commit" and proceeding is NOT asking. Ask, then wait for a yes.**

---

## Branching

- **ALL changes in a branch — NEVER directly on `main`.**
- Branch naming: `feature/<description>` or `fix/<description>`
- Branch from `main`: `git checkout main && git pull && git checkout -b feature/<name>`
- Never push a `claude/...` auto-generated worktree branch to origin — rename it first.

---

## After Finishing Code Changes

1. Stop.
2. Summarize what changed and why (not a line-by-line diff — the *why*).
3. Wait for the user's signal before touching git or deploy.

---

## Code Style

- Don't add abstractions beyond what the current task needs. Three similar lines beat a premature helper.
- No comments explaining *what* the code does — only *why* when non-obvious.
- No error handling for scenarios that can't happen.
- No backwards-compatibility shims for removed code — delete it cleanly.
- Don't create planning or analysis documents unless asked — work from conversation context.

---

## Working Style

- Tal ships fast and iterates — keep changes small, share output early, expect quick back-and-forth.
- Don't create transient handoff docs — durable decisions go in `AGENTS.md` or `DESIGN.md`.
- Open work belongs in GitHub Issues, not in this file.
- Match response length to the task — a simple question gets a direct answer, not headers and bullet points.

---

## Secrets

- Never commit API keys, passwords, or tokens.
- Never log or echo secrets in terminal output.
- Secrets live in env files (`.env`, Cloudflare Worker secrets, etc.) — always gitignored.

---

## UI Changes

- After any UI change: open the browser, check the console for errors, confirm the feature works.
- Never say "done" without verifying the UI isn't broken.
- Test the golden path and at least one edge case.

---

## References

- This file lives at: https://github.com/talbz/agent-rules/blob/main/AGENTS.md
- Claude Code global config: `~/.claude/CLAUDE.md`
