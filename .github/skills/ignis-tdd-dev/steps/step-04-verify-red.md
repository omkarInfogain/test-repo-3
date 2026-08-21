# Step 04 — Verify RED: watch the test fail for the right reason

**MANDATORY. Never skip.** If you didn't watch the test fail, you don't know it tests the right thing.

## Actions

1. Run the new test with `test_command`; capture exact output.
2. Confirm the failure reason is "feature not implemented," not an unrelated defect.
3. Paste the failing output into the TDD Record / Debug Log as RED evidence for this behavior.

## HALT

- **HALT if** the test PASSES — a test passing before any code proves nothing. The test is wrong (asserts existing behavior, or is a no-op). Rewrite it and return to `step-03-red.md`.
- **HALT if** the test ERRORS instead of failing (import/syntax error, typo, missing fixture) — not a real RED. Fix the scaffolding so it FAILS on a missing-feature assertion, then re-run.

## Next

Proceed to `step-05-green.md`.
