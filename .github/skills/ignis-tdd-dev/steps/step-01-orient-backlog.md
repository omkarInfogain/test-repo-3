# Step 01 — Orient, load context, build the RED backlog

**Goal:** load the story and turn its Test Intent into an ordered backlog.

## Actions

1. If a story file was provided, load it. Otherwise read `sprint_status_file`, select the next `ready-for-dev` story, and load it from `dev_story_location`.
2. Resolve and load `project_context` (coding + testing standards) if it exists. Resolve the glob against the real tree — do not skip because it is a pattern.
3. Parse the story's **Test Intent** table into the RED backlog: an ordered list of behaviors, each with its AC, `test_level`, fixtures/seams, and `tea_candidate` flag.
4. Set the story Status to `in-progress` and update `sprint_status_file`.

## HALT

**HALT if** no `ready-for-dev` story exists — report there is nothing to implement and stop.

## Fallback

**If** the story has no Test Intent section: derive behaviors from the Acceptance
Criteria (happy path + any stated edge/error cases). Note in the Debug Log that
the backlog was derived, not authored — prefer re-drafting via
`ignis-tdd-create-story`.

## Next

Proceed to `step-02-clean-baseline.md`.
