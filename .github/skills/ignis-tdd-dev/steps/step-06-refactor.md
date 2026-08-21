# Step 06 — REFACTOR: improve design while staying green

**Goal:** improve the code without changing behavior.

## Actions

1. Improve the new code and its immediate surroundings: names, duplication, helpers, types. Do NOT change observable behavior and do NOT add new behavior (new behavior requires a new RED test).
2. Re-run `test_command` after each meaningful refactor; keep the suite green.

## Refactor acceptance bar

A behavior's refactor is complete only when: no duplication introduced by this
behavior remains; linting/static analysis passes on changed files; no dead or
unreachable branches were added; names and structure reflect the behavior. The
full suite stays green throughout. Do not add new behavior (that requires a new
RED test).

## HALT

**HALT if** any test fails during refactor — revert the last refactor step or fix
it until green before continuing.

## Next

Proceed to `step-07-loop.md`.
