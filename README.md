# Nuke

End-of-session housekeeping before you nuke the context window. Kills dev servers, runs tests, commits, pushes, and updates project docs — optimized for speed with parallel execution and minimal confirmation gates.

One command. Clean exit. Nothing forgotten.

---

## What it does

| Step | What | Skippable |
|------|------|-----------|
| 1 | Kill dev servers + git preflight (parallel) | Auto-skips if none found |
| 2 | Run tests + build (parallel) | `--quick` or `--yolo` |
| 3 | Commit & push | Skipped if working tree is clean |
| 4 | Update CLAUDE.md | `--yolo` or no CLAUDE.md |
| 5 | Print summary report | Always |

---

## Flags

| Flag | What it skips | User confirmations | Estimated turns |
|------|--------------|-------------------|----------------|
| *(none)* | — | 1–2 (commit+push, failures) | ~4 |
| `--quick` | Tests, build | 1 (commit+push) | ~3 |
| `--yolo` | Tests, build, CLAUDE.md, all confirmations | 0 | ~2 |

Flags are combinable. `--yolo` implies `--quick`.

---

## Speed optimizations (v2)

- **Parallel preflight**: git status, diff, and log run alongside dev server cleanup in one message
- **Parallel test+build**: tests and build run simultaneously, not sequentially
- **Single confirmation**: commit and push are one question, not two separate prompts
- **No unnecessary questions**: dev server step skips silently if none were started
- **Lightweight CLAUDE.md check**: quick path validation only, no full codebase audit
- **Compressed prompt**: ~1,000 tokens vs ~2,100 in v1 — faster LLM processing

---

## Safety model

Nuke is designed to clean up, not to break things:

- **Dev servers**: only kills processes it started. Never scans ports and kills arbitrary processes.
- **Tests/build**: on failure, asks whether to commit anyway, stash, or abort. Never decides for you.
- **Commits**: shows diff summary and proposed message. Never stages `.env*`, credentials, secrets, or `node_modules`.
- **Push**: auto-sets upstream to `origin/<branch>` if missing. In `--yolo` mode, pushes without asking.
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
**CLAUDE.md**: no changes
```

---

## Installation

### Claude Code

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/nuke.md https://raw.githubusercontent.com/PekkaSetala/nuke/main/SKILL.md
```

Then use `/nuke`, `/nuke --quick`, or `/nuke --yolo` in any session.

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
/nuke --quick      # skip tests and build
/nuke --yolo       # skip everything skippable, no confirmations, just commit and push
```

---

## Files

```
nuke/
├── SKILL.md           # The skill itself — install this
├── README.md          # This file
├── CONTRIBUTING.md    # How to contribute
└── LICENSE            # MIT
```

---

## See also

- [Peel](https://github.com/PekkaSetala/peel) — structured problem-solving skill (framing, bottleneck, failure diagnosis)

---

## License

MIT
