# agent-rules

Universal rules for AI coding agents (Claude Code, Codex, Cursor, Aider, etc.) working on any of Tal's repos.

Each repo's `AGENTS.md` references this file and adds only repo-specific details (tech stack, deploy command, secrets, local run instructions).

---

## ⛔ STOP AND WAIT — Git & Deploy, No Exceptions

> **NEVER COMMIT OR PUSH WITHOUT ASKING FIRST AND WAITING FOR AN EXPLICIT YES!!!**
> **NEVER COMMIT OR PUSH WITHOUT ASKING FIRST AND WAITING FOR AN EXPLICIT YES!!!**
> **NEVER COMMIT OR PUSH WITHOUT ASKING FIRST AND WAITING FOR AN EXPLICIT YES!!!**

These actions each require a **separate explicit confirmation** in the very next user message:

| Action | Rule |
|--------|------|
| `git commit` | Ask "Ready to commit?" — wait for yes |
| `git push` | Ask separately after commit approval |
| `gh pr create` | Ask separately after push approval |
| `gh pr merge` | Ask separately — NEVER auto-merge |
| `git branch -d` | Ask separately |
| Any deploy command | Only on "עלה לאתר" / "deploy" / "sync" or explicit yes |

> **Announcing intent ("מתחיל commit", "I'll now push") and proceeding is NOT asking. ASK. WAIT. ACT.**
> **Approval for one step does NOT authorize the next.** Commit ≠ push. Push ≠ deploy.
> **After finishing code changes: stop, summarize what changed and why, wait for signal.**

---

## SDLC Flow — Always Follow This Order

```
main → feature branch → commits → push → PR → review → merge main → deploy
```

1. **Branch from `main`**: `git checkout main && git pull && git checkout -b feature/<description>`
2. Make changes, commit (wait for approval per commit)
3. Push branch (wait for approval)
4. **Open PR**: `gh pr create` → share the URL → wait for approval to merge
5. **Merge to `main`** only on explicit "yes / merge it"
6. **Deploy only from `main`** — NEVER deploy a feature branch directly
7. Delete feature branch after merge

**One branch per feature or fix.** Don't pile unrelated changes onto a long-lived branch. Rename `claude/...` auto-generated worktree branches before the first push.

---

## Branching Rules

- **ALL changes in a feature branch — NEVER directly on `main`.**
- Naming: `feature/<description>` or `fix/<description>`
- Keep branches focused — one feature or fix per branch
- Never push `claude/...` auto-generated branch names to origin

---

## Code Style

- **No abstractions beyond what the task needs.** Three similar lines beat a premature helper.
- **No comments explaining *what* the code does** — only *why* when the reason is non-obvious (a hidden constraint, a workaround, a subtle invariant).
- **No error handling for scenarios that can't happen.** Trust internal code and framework guarantees. Only validate at system boundaries (user input, external APIs).
- **No backwards-compatibility shims** for removed code — delete it cleanly.
- **No feature flags** or half-finished implementations — either ship it or don't.
- **No planning/analysis documents** unless the user asks — work from conversation context.
- **Prefer editing existing files** over creating new ones.

---

## Feature Documentation

Each repo should have a `DESIGN.md` listing its features, data model, and a regression checklist.

- **Read it before starting any change** — know what already exists.
- **Check the regression checklist before pushing** — run through it manually or note which items apply.
- **Update `DESIGN.md`** when adding or removing a feature.

## After Finishing Changes

1. **Stop.**
2. Summarize what changed and *why* (not line-by-line — the reasoning).
3. **Wait** for the user's signal before any git action.
4. If the changes are UI-visible, verify in the browser before reporting done.

---

## UI Changes

- **After every UI change: open the browser, check the console for errors, confirm the feature works.**
- **Never say "done" without verifying the UI isn't broken.**
- Test the golden path and at least one edge case.
- TypeScript inside template literal strings is NOT stripped by tsc — it reaches the browser as invalid JS and breaks everything. Use plain JS casts or avoid casts inside template strings.
- If an HTML element is removed, remove ALL matching `getElementById`/`querySelector` calls and event listeners, and all state-restore lines that reference it.

---

## Secrets

- **Never commit API keys, passwords, or tokens.**
- **Never echo or log secrets** in terminal output.
- Secrets live in `.env` files or platform-specific secret stores (Cloudflare secrets, etc.) — always `.gitignore`d.
- Double-check `git status` / `git diff` before any commit if `.env` files are nearby.

---

## Testing

- Run the test suite before pushing if one exists.
- Never write change-detector tests (tests that fail whenever expected-to-change data updates — model catalogs, version numbers, list lengths). Write behavioral invariants instead.
- Tests must not write to real user directories — use temp dirs.
- Always run the full suite before pushing, not just the test for the changed file.

---

## Working Style

- Tal ships fast and iterates — keep changes small, share output early, expect quick back-and-forth.
- Don't create transient handoff docs — durable decisions go in `AGENTS.md` or `DESIGN.md`, open work goes in GitHub Issues.
- Match response length to the task — a simple question gets a direct answer, not headers and sections.
- When stuck: say so clearly, state what you know and what you don't, ask the one question that unblocks you.
- **Never give up on the right solution.**

---

## Reference

- This file: https://github.com/talbz/agent-rules/blob/main/AGENTS.md
- Claude Code global config: `~/.claude/CLAUDE.md`
