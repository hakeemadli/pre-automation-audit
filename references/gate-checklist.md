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

## 1a. Patch smells avoided

| Smell | Where it would go | Rejected? | If kept: why the cause is out of reach |
| ----- | ----------------- | --------- | -------------------------------------- |
|       |                   |           |                                        |

## 1b. Reproduce and name the cause

| Item | Value |
| ---- | ----- |
| Reproduction command | |
| Failing artifact path | |
| Failing line quoted | |
| Cause (one sentence) | |
| Fails before the change? | yes / no |

If the cause cannot be named here, the task is investigation, not a fix.

## 2. Baseline artifacts

| Artifact | Command | Path | Exit code |
| -------- | ------- | ---- | --------- |
|          |         |      |           |

Inputs identified:

Outputs identified:

## 2a. Cause evidence

| Check | Result |
| ----- | ------ |
| Same reproduction re-run, unchanged | pass / fail |
| Original trigger still produces the symptom? | no / yes |
| Error swallowed, downgraded, or retried into silence? | no / yes |
| Any test, check, or assertion weakened or skipped? | no / yes |
| Symptom suppressed without fixing the cause? | no / yes |

Every `yes` in the last four rows blocks the change.

## Determinism audit

- [ ] Versions locked; no floating dependencies introduced.
- [ ] Ordering is explicit (sorted output, stable traversal).
- [ ] Seeds, locale, and timezone fixed where relevant.
- [ ] No network, clock, or ambient environment dependency in the changed path.
- [ ] Steps are idempotent and safe to re-run.

## 3. Static checks

| Check | Command | Result | Notes |
| ----- | ------- | ------ | ----- |
| Formatter (check) | | | |
| Linter | | | |
| Types / compile | | | |
| Tests | | | |
| Repo-specific | | | |

## Approval

- Cause fixed, not hidden: yes / no
- Ready for automation: yes / no
- Outstanding guesses:
- Blocked on:
