# Step 03 — RED: write ONE failing test for the next behavior

**Goal:** express the next behavior as a single failing test. **NO PRODUCTION CODE MAY BE WRITTEN OR MODIFIED IN THIS STEP.**

## Actions

1. Select the next un-done behavior from the RED backlog (respect dependency order).
2. **If** `prefer_tea_atdd` is true AND the behavior is `tea_candidate` AND the `bmad-tea` module is installed: delegate RED to TEA — invoke `bmad-tea-testarch-atdd` to generate the failing acceptance test(s), then go to `step-04-verify-red.md`. Do not write production code.
3. Otherwise author exactly ONE minimal test for this behavior:
   - clear, behavior-describing name;
   - exercises real code paths (mocks only where a real dependency is genuinely unavailable);
   - asserts the specific observable behavior from the Test Intent row, at the stated `test_level`;
   - uses the named fixtures / seams / test data.
4. Mark the corresponding Task/Subtask checkbox as in-progress.

## HALT

**HALT if** you wrote or edited ANY production/source file in this step — Iron Law
violated. DELETE that production change entirely and redo this step writing only
the test.

## Next

Proceed to `step-04-verify-red.md`.
