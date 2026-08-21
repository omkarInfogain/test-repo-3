# Step 07 — Write Dev Notes and finalize the story

**Goal:** render the story and mark it ready.

## Actions

1. Write Dev Notes: relevant modules/files, applicable coding + testing standards from `project_context`, and any architectural seams that support testability.
2. Render the full story from `template.md` into `dev_story_location` with the correct story id. When rendering, expand `{{tdd_record_rows}}` to one row per Test Intent behavior ID (matching the Test Intent table's IDs), with empty RED/GREEN/Refactored/Test-file cells for the dev skill to fill.
3. Leave the `File List` section empty — it is owned and populated by `ignis-tdd-dev` at completion, not by story creation.
4. Set Status to `ready_status`. Update `sprint_status_file`: set this story's status to `ready_status` (from config). Match the existing entry format used by the project's `sprint-status.yaml` (do not invent new fields or status strings). If the story is not yet listed, add an entry consistent with existing entries.
5. Report a concise summary: story id, number of ACs, total Test Intent behaviors by level, and confirmation the readiness gate passed.

## Done

The story is ready for `ignis-tdd-dev`.
