# Step 05 — GREEN: write the minimal code to pass

**Goal:** make the failing test pass with the simplest possible code.

## Actions

1. Write the SIMPLEST production code that makes the failing test pass. Add nothing the test does not require (YAGNI).
2. Run the full suite with `test_command`; capture output.
3. Record the GREEN result and mark the behavior green in the TDD Record.

## HALT

- **HALT if** the target test still fails — keep working on the CODE (never weaken the test) until it passes.
- **HALT if** any previously-passing test now fails (regression) — fix it before continuing.
- **If** output has warnings/errors not present in the clean baseline — resolve them; GREEN means a pristine run, not merely a passing assertion.

## Next

Proceed to `step-06-refactor.md`.
