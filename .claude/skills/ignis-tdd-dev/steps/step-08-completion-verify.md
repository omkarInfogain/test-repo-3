# Step 08 — Completion verification via git truth

**Goal:** verify completeness from tooling, not memory.

## Actions

1. Run `git status --porcelain` and `git diff --name-only` (include untracked). List EVERY changed/added file into the story File List.
2. Run `test_command` one final time; confirm the whole suite is green with pristine output.
3. Confirm every Test Intent behavior has RED evidence and a GREEN result recorded.
4. Execute the validation checklist at `checklist.md`.

## HALT

**HALT if** any checklist box cannot be truthfully checked (e.g. a behavior whose
test never failed first) — TDD was skipped for at least one behavior. Do NOT mark
complete. Return to `step-03-red.md` for the offending behavior.

## Next

Proceed to `step-09-finalize.md`.
