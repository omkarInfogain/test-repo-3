# Step 02 — Establish a clean test baseline

**Goal:** guarantee a green starting point so a NEW red failure is distinguishable from a pre-existing broken test.

## Actions

1. Run the full suite with `test_command`. Capture exact pass/fail counts and output.
2. Record the clean-baseline result in the Dev Agent Record → Debug Log.

## HALT

**HALT if** the baseline has any failing or erroring tests. Show the failures and
ask the user to (a) fix the baseline, (b) proceed treating those specific
failures as known-pre-existing, or (c) abort. Do not continue until the user chooses.

## Next

Proceed to `step-03-red.md`.
