---
name: Pre-Automation Audit
description: Use before an agent automates or refactors a codebase end-to-end. Runs a pre-flight gate on anti-patterns, artifact verification (logs/IO), and linting, and enforces deterministic, high-trust, low-token execution.
---

# Pre-Automation Audit

Use this skill before an agent touches a codebase end-to-end. Automation is only
allowed after every gate below has **recorded evidence**. No gate, no automation.

The point is not to slow work down. It is to replace "the agent says it works"
with artifacts you can re-run, and to shrink the surface where guessing can
happen.

## Gate 0: Can this be done without an agent?

Ask first, before reading or editing code.

If the goal is reachable with a script, codemod, migration tool, formatter, or
CI job:

- Use that. Write or reuse normal code.
- Reserve the agent only for the parts that genuinely need judgement.
- For each remaining task, state in one line why AI is required.

Token spend drops fastest by shrinking the AI surface, not by shortening prompts.
A deterministic script that runs a thousand times costs nothing after the first
write; the same work as a prompt costs every time and drifts.

If every task is scriptable, stop here. Do not automate with an agent.

## Gate 1: Anti-pattern sweep

Enumerate before changing anything. Do not fix during the sweep — the sweep
produces the map, the plan decides what to touch.

Look for:

- Duplicated logic, copy-paste modules, dead code, shadow configs.
- Hidden global state, implicit ordering, non-idempotent scripts.
- Non-deterministic paths: clock, randomness, network, environment, filesystem
  or map ordering, locale, timezone.
- Untested entrypoints, swallowed exceptions, silent fallbacks.
- Guessed behavior: TODO/`assumed`, magic constants, untyped boundaries.

Record each as `file:line — pattern — risk`. If the sweep is large, write it to a
file and reference that path instead of pasting it back.

## Gate 2: Artifact verification (logs / IO)

Prove current behavior from artifacts, not from reading code.

- Capture a **baseline run**: exact command, exit code, stdout, stderr, files
  produced, log paths.
- Identify the real inputs and outputs: files, DB rows, HTTP payloads, log lines.
- Diff after every change against that baseline.
- A change is done only when the artifact diff is the intended one and nothing
  else moved.

Never report success from memory of what the code should do. If you cannot
produce an artifact, say what is missing instead of asserting a result.

## Gate 3: Linting and static checks

Run what the repo already defines, in this order, and record raw output:

1. Formatter (check-only).
2. Linter.
3. Type checker / compiler.
4. Test suite.
5. Repo-specific checks (pre-commit, CI config).

Do not introduce a new toolchain as part of automation. If a check is missing,
state that it is missing rather than inventing one or silently skipping it.

## Execution rules

These are hard constraints, not preferences.

- **No AI-generated comments.** Do not add comments, docstrings, banners, or
  explanatory blocks. Match existing comment density; default to zero. A comment
  is allowed only when the user asks for it, or when preserving an existing one.
- **Trust comes from verification.** Every claim is backed by an artifact:
  command, output, diff, or log path. No "should work", no "likely".
- **Deterministic by design.** Prefer the approach with the fewest branches and
  the fewest guesses. Pin versions, sort collections, fix seeds and timezones,
  make steps idempotent and re-runnable. If a guess is unavoidable, stop and ask
  instead of encoding it.
- **Smallest reversible step.** One concern per change, baseline artifact each
  time, so a failure is localized and undoable.
- **Prefer plain code.** If a deterministic script solves it, write the script
  rather than prompting repeatedly.

## Stop conditions

Stop and ask the user when:

- A gate has no artifact to show.
- Two approaches are equally valid and the choice changes public behavior.
- Verification needs credentials, network access, or data you do not have.
- The repo's checks cannot run at all.

## Output format

Report in this order, terse:

1. `Gate 0:` verdict, plus why AI is needed for what remains.
2. `Anti-patterns:` list, or `none found`.
3. `Baseline:` commands and artifact paths.
4. `Checks:` each command with pass or fail.
5. `Plan:` ordered steps, each with its verification command.
6. `Risks:` anything still guessed.

Then read `references/gate-checklist.md` and fill it in before starting work.
