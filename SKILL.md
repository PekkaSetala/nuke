# Nuke — End-of-Session Housekeeping

Run through every step below in order. Skip steps that don't apply.

If the user runs `/nuke --quick`, skip Steps 2 and 3.

## Step 1: Kill Dev Server

First, check your conversation history for any `npm run dev`, `npx vite`, `next dev`, or similar commands you executed, and kill those specific processes by PID if they're still running.

If you don't find any session-started processes, ask the user: "Any dev servers running for this project? Give me the port or PID and I'll kill it." If they say no, move on.

Do NOT scan ports and kill arbitrary processes — other workspaces may be using them.

## Step 2: Run Tests

If the project has a test command (`npm test`, `npx vitest run`, etc.), run it.

If tests fail: report the failures, then ask the user whether to commit anyway, stash changes, or abort. Do not decide for them.

## Step 3: Build Check

If the project has a build command (`npm run build`, etc.), run it.

If the build fails: report the error, then ask the user whether to commit anyway, stash changes, or abort. Do not decide for them.

## Step 4: Commit & Push

Check `git status`. If the working tree is clean, skip to Step 5.

If there are staged or unstaged changes:

1. Run `git diff --stat` and show the user a one-line summary of what changed.
2. Wait for confirmation before committing.
3. Draft a commit message matching the repo's existing style. Use `git log --oneline -5` to check — if that fails (new repo, detached HEAD), fall back to conventional commits (`feat:`, `fix:`, `chore:`).
4. Stage relevant files. Never stage `.env`, `.env.*`, credentials, secrets, or `node_modules`.
5. Commit.
6. If there are unpushed commits, push to the remote tracking branch. If there is no upstream, ask whether to set one.

## Step 5: Update CLAUDE.md

If `CLAUDE.md` exists, check whether it's stale:

- File tree accuracy — do listed files still exist? Are new important files missing?
- Pattern descriptions — do they match current behavior?
- Command references — are they still correct?

If updates are needed, make a separate commit (`docs: update CLAUDE.md`). Do not amend the previous commit — it may already be pushed.

If no `CLAUDE.md` exists, skip.

## Step 6: Summary Report

```
## Session Complete

**Dev server**: killed PID 12345 — next dev (or: none started this session)
**Tests**: 16 passed (or: skipped — /nuke --quick)
**Build**: clean (or: skipped)
**Committed**: abc1234 — "feat: description" (or: working tree was clean)
**Pushed**: main → origin/main (or: already up to date)
**CLAUDE.md**: updated file tree (or: no changes needed)
```

Keep it compact. No prose, no filler.
