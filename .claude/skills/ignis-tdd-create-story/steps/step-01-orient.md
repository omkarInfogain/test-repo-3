# Step 01 — Orient and load planning inputs

**Goal:** understand the story to draft and load the context needed to make it testable.

## Actions

1. Load `config.yaml` from `config_source`. Resolve and hold: `dev_story_location`, `sprint_status_file`, `project_context`, `ready_status`, `test_command`, `test_framework`, `max_test_intent_behaviors`, `require_story_preview`, `tea_enabled`, `prefer_tea_atdd`.

   **HALT if** `config.yaml` cannot be found or is missing `dev_story_location` / `sprint_status_file` / `ready_status` — report the missing keys and stop; downstream steps depend on them.
2. Load the `epics`, `prd`, and `architecture`/tech-spec inputs.
3. Resolve and load `project_context` (coding + testing standards) if it exists. Resolve the glob against the real project tree — do not skip because it is a pattern.
4. Determine the test framework: if `test_framework` is `auto`, infer it from `project_context` and the repo (config files, dependencies, existing test dirs); otherwise use the configured value. Record it as `detected_test_framework` for later steps. If it cannot be determined, note "framework: unknown" and proceed.
5. Determine TEA availability: if `tea_enabled` is `auto`, detect whether the `bmad-tea` module is installed/configured; otherwise use the configured value. Record `tea_available`.
6. Identify the specific story to draft (from the epic backlog or an argument). Capture its parent epic, the user role, the goal, and the value.
7. Determine the next story id/number consistent with `dev_story_location` naming.

## Next

Proceed to `step-02-narrative-scope.md`.
