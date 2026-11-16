# Pre-Automation Audit

An [OpenCode](https://opencode.ai) skill that runs a pre-flight audit on a
codebase **before** an agent is allowed to automate or refactor it end-to-end.

The rule it enforces: no gate, no automation.

## Why

Agent output is only worth as much as the verification behind it. Without a
gate, three things go wrong at once:

- **Unverified claims.** "It works" is a summary, not an artifact.
- **Token waste.** Work that a script could do gets re-prompted on every run.
- **Non-reproducible changes.** Hidden clock, network, or ordering dependencies
  make a change pass once and fail later.

This skill front-loads the cheap work — a sweep, a baseline, a lint run — so the
expensive step is small and checkable.

## When it fires

Before an agent automates, refactors, or migrates a codebase end-to-end. Not for
single-file edits or questions about code.

## How it works

Four gates, each of which must leave evidence behind.

| Gate | Question | Evidence required |
| ---- | -------- | ----------------- |
| **0. Plain code** | Can a script, codemod, or CI job do this instead? | One line per remaining task on why AI is needed |
| **1. Anti-patterns** | What is already wrong or fragile here? | `file:line — pattern — risk` list |
| **2. Artifacts** | What does it actually do right now? | Baseline command, exit code, stdout/stderr, output files |
| **3. Static checks** | Does it still hold together? | Formatter, linter, types, tests — raw output |

Gate 0 exists because the cheapest automation is the one you never prompt for.
If every task is scriptable, the skill tells the agent to stop and write code
instead.

## Hard rules

These are constraints in the skill, not suggestions:

- **No AI-generated comments.** No added comments, docstrings, or banners.
  Existing comment density is the ceiling; the default is zero.
- **Trust through verification.** Every claim is backed by a command, output,
  diff, or log path.
- **Deterministic by design.** Fewest branches, fewest guesses. Pinned versions,
  sorted collections, fixed seeds and timezones, idempotent steps. An unavoidable
  guess is a stop-and-ask, not an inline assumption.
- **Smallest reversible step.** One concern per change, baseline artifact each
  time.
- **Prefer plain code.** A deterministic script beats a repeated prompt.

## Install

Clone straight into the global skills directory:

```sh
git clone https://github.com/hakeemadli/pre-automation-audit \
  ~/.config/opencode/skills/pre-automation-audit
```

No configuration edit is needed — OpenCode discovers
`~/.config/opencode/skills/<id>/SKILL.md` automatically. The skill ID is
`pre-automation-audit`.

To scope it to one project instead, clone into that project's
`.opencode/skills/pre-automation-audit`.

## Usage

Ask for it explicitly:

```text
@pre-automation-audit assess this repo before you start the refactor
```

The agent loads `SKILL.md`, runs the gates, fills in
`references/gate-checklist.md`, and reports in this order:

```text
Gate 0:      verdict + why AI is needed
Anti-patterns: file:line list, or "none found"
Baseline:    commands + artifact paths
Checks:      each command, pass/fail
Plan:        ordered steps + verification command each
Risks:       anything still guessed
```

## Layout

```text
pre-automation-audit/
├── SKILL.md                     # gate workflow and hard rules
└── references/
    └── gate-checklist.md        # fill-in report, loaded at the gate
```

`references/` is not loaded automatically; the agent reads it when `SKILL.md`
tells it to.

## Editing

`SKILL.md` is the source of truth and is intentionally short so it stays cheap
to advertise. Load-bearing detail belongs in `references/`.

If you keep a copy in `~/.config/opencode/skills/`, resync after edits:

```sh
rsync -a --exclude .git ./ ~/.config/opencode/skills/pre-automation-audit/
```

## License

MIT — see [LICENSE](LICENSE).
