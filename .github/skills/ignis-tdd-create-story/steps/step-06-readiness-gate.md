# Step 06 — Test-first readiness gate

**Goal:** block any story that is not test-ready from reaching `ignis-tdd-dev`.

## Actions

1. Execute the validation checklist at `checklist.md` against the drafted story.
2. Count total Test Intent behaviors. If the count exceeds `max_test_intent_behaviors`, WARN that the story is likely too large to deliver test-first in one iteration and recommend splitting it into smaller vertical slices before handing to `ignis-tdd-dev`. Proceed only after the user acknowledges (or the story is split). This is advisory — it does not by itself HALT the gate.
3. After the checklist passes: if `require_story_preview` is true, present the Acceptance Criteria table and the Test Intent table to the user and ask for explicit approval before writing. **HALT** until approved; if the user requests changes, return to the relevant step. If `require_story_preview` is false (e.g. a guided UI owns review), skip the preview and proceed to finalize.

## HALT

**HALT if** any checklist item fails — an untestable AC, an AC with no Test
Intent behavior, or tasks that are not test-first sequenced. Fix the failing
items (return to the relevant step) before marking the story ready. Do not hand
a non-test-ready story to `ignis-tdd-dev`.

## Next

Proceed to `step-07-finalize.md`.
