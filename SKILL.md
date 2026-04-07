# Nuke — End-of-Session Housekeeping

**Flags**: `--quick` (skip test+build), `--yolo` (skip test+build+confirmations+CLAUDE.md)

**Rules**: Batch all independent ops in one message. Don't ask questions answerable from history. Combine git commands.

## 1. Preflight (parallel, one message)

Kill any dev server YOU started this session (by PID from history). No server started = skip silently, don't ask.

Parallel in same message: kill server + `git status` + `git diff --stat` + `git log --oneline -5`

Don't scan ports or kill other workspaces' processes.

## 2. Test + Build (parallel) — skip if --quick/--yolo

Run test and build in parallel. Both pass → proceed. Either fails → report, ask: commit anyway / stash / abort.

## 3. Commit & Push

Clean tree → skip to step 4.

Changes exist:
1. One-line summary from diff --stat (already have it)
2. Draft commit message matching repo style from log (already have it). Fallback: conventional commits.
3. `--yolo`: stage + commit + push immediately, skip to step 5.
4. Otherwise: show message, ask ONE question: "Commit and push? [y/n/commit only/edit message]"
5. Stage files. Never stage `.env*`, credentials, secrets, `node_modules`.
6. Commit + push. Auto-set upstream to `origin/<branch>` if missing.

## 4. CLAUDE.md — skip if --yolo

Only if CLAUDE.md exists. Quick scan: listed paths still exist? Commands match package.json? Fix only obviously wrong refs. Separate commit (`docs: update CLAUDE.md`).

## 5. Summary

```
## Session Complete
**Dev server**: killed PID 12345 (or: none)
**Tests**: 16 passed (or: skipped)
**Build**: clean (or: skipped)
**Committed**: abc1234 — "feat: X" (or: clean)
**Pushed**: main → origin/main (or: up to date)
**CLAUDE.md**: updated (or: no changes)
```
