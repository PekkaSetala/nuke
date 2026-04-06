# Contributing to Nuke

Nuke is a prompt-based skill, not a codebase. Contributing means proposing changes to how an LLM is instructed to clean up a session — which requires testing against real workflows, not just reading the diff.

The bar: **test it, document it, show your work.**

---

## What can be contributed

**Step improvements**
Tightening instructions, fixing edge cases (e.g. dev servers on non-standard ports), improving safety checks, making steps more robust on smaller models.

**New platform installation instructions**
If you've verified Nuke works on a platform not listed in the README, add the install path.

**Testing results**
Run Nuke at the end of a real session, document what happened — which steps fired, whether the output was correct, where it fell short. Testing reports are valuable even without a code change.

**Bug reports**
If Nuke consistently misbehaves (kills wrong processes, skips steps it shouldn't, generates bad commit messages), open an issue with a reproducible example.

**New steps**
High bar — see below.

---

## How to propose a change

### For small changes (wording, formatting, installation instructions)

Open a PR directly. In the PR description, explain what problem the change solves and whether you tested it.

### For substantive changes (step logic, safety model, output format)

1. Open an issue first and describe what you want to change and why
2. Get rough agreement before investing in a full PR
3. Include test results in the PR (see Testing below)

### For new steps

Open an issue before writing anything. New steps need a clear answer to:
- What session cleanup task does this address that the existing steps don't?
- What's the safety model? (What could go wrong, and how does the step prevent it?)
- Where does it go in the execution order?

Nuke's value is in being predictable and safe. A new step that introduces risk or ambiguity will be declined.

---

## Testing standard

Changes to `SKILL.md` must be tested in at least one real end-of-session scenario.

A test report should include:

- **Model tested**: e.g. Claude Sonnet 4.5, GPT-4o
- **Session context**: what was running (dev server, framework, etc.)
- **Expected behavior**: which steps should fire, what the output should look like
- **Actual behavior**: what actually happened
- **Pass/fail and why**

---

## Style

`SKILL.md` instructions should be:

- **Explicit.** The model must know exactly what to do at each step. No "use your judgment" where a rule is possible.
- **Short.** Every sentence costs context window.
- **Safe by default.** When in doubt, ask the user rather than act.

---

## What won't be accepted

- Changes that haven't been tested
- New steps without a safety analysis
- Instructions that are vague or skip confirmation gates
- Anything that could kill processes, delete files, or push code without explicit user confirmation
