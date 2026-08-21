# Step 04 — Derive the Test Intent backlog (the RED plan)

**Goal:** produce the section `ignis-tdd-dev` consumes as its RED/GREEN backlog. Be concrete and complete.

## Actions

1. For EACH acceptance criterion, enumerate the behaviors to be driven test-first:
   - the happy path;
   - boundary / edge cases (empty, max, min, off-by-one, unicode, concurrency where relevant);
   - error and failure paths (invalid input, missing dependency, timeout, permission denied).
2. For each behavior record: a short behavior name, the AC it traces to, the test level (`unit` | `integration` | `acceptance`), and any fixtures, seams, or test data needed to make it testable. Frame the `Fixtures / seams / data` column in terms of `detected_test_framework` (e.g. Jest: mock modules / test fixtures; pytest: fixtures / conftest; RSpec: let/factories). If the framework is unknown, keep fixtures framework-neutral and flag them for confirmation during dev.
3. Mark behaviors whose test level is acceptance/e2e with `tea_candidate: yes` so `ignis-tdd-dev` can optionally delegate them to TEA `*atdd` — but only set `tea_candidate: yes` when `tea_available` is true. If TEA is unavailable, set `tea_candidate: no` for all behaviors (they will be authored inline during dev).

## HALT

**HALT if** any acceptance criterion has zero associated behaviors — every AC
needs at least one test behavior. Add them.

## Output

Populate the `test_intent` table in the template.

## Next

Proceed to `step-05-sequence-tasks.md`.
