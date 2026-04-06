# Nuke

End-of-session housekeeping for LLM coding assistants. Kills dev servers, runs tests, commits, pushes, and updates project docs — in order, with confirmation gates at every destructive step.

One command. Clean exit. Nothing forgotten.

---

## What it does

| Step | What | Skippable |
|------|------|-----------|
| 1 | Kill dev servers started this session | Auto-skips if none found; asks user for port/PID as fallback |
| 2 | Run tests | Skipped with `--quick` |
| 3 | Build check | Skipped with `--quick` |
| 4 | Commit & push | Skipped if working tree is clean |
| 5 | Update CLAUDE.md | Skipped if no CLAUDE.md or no staleness detected |
| 6 | Print summary report | Always |

---

## Safety model

Nuke is designed to clean up, not to break things:

- **Dev servers**: only kills processes it started, or ones you explicitly confirm. Never scans ports and kills arbitrary processes.
- **Tests/build**: on failure, asks whether to commit anyway, stash, or abort. Never decides for you.
- **Commits**: shows diff summary and waits for confirmation. Never stages `.env`, credentials, secrets, or `node_modules`.
- **Push**: only pushes to the existing tracking branch. If there's no upstream, asks before setting one.
- **CLAUDE.md**: updates go in a separate commit, never amends the code commit.

---

## Example output

```
## Session Complete

**Dev server**: killed PID 45231 — next dev
**Tests**: 23 passed
**Build**: clean
**Committed**: a1b2c3d — "feat: add user settings page"
**Pushed**: main → origin/main
**CLAUDE.md**: updated file tree
```

---

## Installation

### Claude Code

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/nuke.md https://raw.githubusercontent.com/PekkaSetala/nuke/main/SKILL.md
```

Then use `/nuke` or `/nuke --quick` in any session.

### Claude.ai (Projects)

1. Open your Project → **Settings** → **Knowledge**
2. Upload `SKILL.md`

### Cursor

Paste the contents of `SKILL.md` into `.cursor/rules/nuke.md`.

### Windsurf / Copilot Chat

Paste into your workspace rules file:

- **Windsurf**: `.windsurfrules`
- **GitHub Copilot**: `.github/copilot-instructions.md`

### Any LLM with a system prompt

Paste `SKILL.md` as part of the system prompt. Nuke has no external dependencies — it's pure instruction.

---

## Usage

```
/nuke              # full cleanup: kill servers, test, build, commit, push, update docs
/nuke --quick      # skip tests and build — just kill servers, commit, push, update docs
```

---

## Files

```
nuke/
├── SKILL.md        # The skill itself — install this
└── README.md       # This file
```

---

## See also

- [Peel](https://github.com/PekkaSetala/peel) — structured problem-solving skill (framing, bottleneck, failure diagnosis)

---

## License

MIT
