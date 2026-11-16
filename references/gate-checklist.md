# Gate Checklist

Fill this in before automation starts. Use `none` instead of leaving a field
blank. Every entry needs evidence — a command, an output, a diff, or a path.

## 0. Plain-code feasibility

| Task | Scriptable without AI? | How / why AI is required |
| ---- | ---------------------- | ------------------------ |
|      |                        |                          |

## 1. Anti-patterns found

| # | Location | Pattern | Risk | In scope? |
| - | -------- | ------- | ---- | --------- |
|   |          |         |      |           |

## 2. Baseline artifacts

| Artifact | Command | Path | Exit code |
| -------- | ------- | ---- | --------- |
|          |         |      |           |

Inputs identified:

Outputs identified:

## 3. Static checks

| Check | Command | Result | Notes |
| ----- | ------- | ------ | ----- |
| Formatter (check) | | | |
| Linter | | | |
| Types / compile | | | |
| Tests | | | |
| Repo-specific | | | |

## Determinism audit

- [ ] Versions locked; no floating dependencies introduced.
- [ ] Ordering is explicit (sorted output, stable traversal).
- [ ] Seeds, locale, and timezone fixed where relevant.
- [ ] No network, clock, or ambient environment dependency in the changed path.
- [ ] Steps are idempotent and safe to re-run.

## Approval

- Ready for automation: yes / no
- Outstanding guesses:
- Blocked on:
