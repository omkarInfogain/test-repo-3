---
name: ignis-tdd-dev
description: >-
  Use when implementing an approved story test-first. Drives the story's Test
  Intent backlog through RED → GREEN → REFACTOR with HALT gates. Falls back to
  deriving behaviors from acceptance criteria if no Test Intent section exists.
  Alternative to bmad-bmm-dev-story.
module: ignis
phase: 4-implementation
agent: ignis-dev
menu-code: TDD
---

# ignis-tdd-dev

Implement ONE story test-first. Drives the story's **Test Intent** backlog
(produced by `ignis-tdd-create-story`) one behavior at a time through
RED → GREEN → REFACTOR. Enforcement pattern ported from the Superpowers
`test-driven-development` skill (obra/superpowers, MIT).

## Critical rules (apply throughout)

- **This skill REPLACES dev-story for the selected story.** Do not also run
  dev-story on it. On completion the story ends in `review` status, identical to
  dev-story, so downstream workflows are unaffected.
- **THE IRON LAW: no production code without a failing test first.** If any
  production code is written or modified before a failing test exists for that
  behavior, DELETE it and start the behavior over. Delete means delete — do not
  keep it as "reference" or comment it out. Implement fresh from the test. No
  exceptions.
- **Story file edit permissions.** You may edit ONLY: Tasks/Subtasks checkboxes,
  Dev Agent Record (incl. TDD Record), File List, Change Log, and Status. Do NOT
  edit Acceptance Criteria, Test Intent, story narrative, or Dev Notes.
- **Evidence over assertion.** Test state comes from running the suite and
  reading `git status` — never from memory. Paste real output.

## Execution

Run the steps in order, loading each shard just-in-time. Every `Action` is
mandatory. Stop immediately on any `HALT` until its condition is resolved.
Steps 3–7 form the per-behavior loop.

1. `steps/step-01-orient-backlog.md` — load story + build the RED backlog.
2. `steps/step-02-clean-baseline.md` — establish a green baseline.
3. `steps/step-03-red.md` — write ONE failing test (no production code).
4. `steps/step-04-verify-red.md` — watch it fail for the right reason (MANDATORY).
5. `steps/step-05-green.md` — minimal code to pass.
6. `steps/step-06-refactor.md` — improve while staying green.
7. `steps/step-07-loop.md` — next behavior, or continue.
8. `steps/step-08-completion-verify.md` — git-truth File List + checklist HALT.
9. `steps/step-09-finalize.md` — records, status → review.

## Output

The story with all Test Intent behaviors implemented test-first, full suite
green, File List derived from `git status`, status `review`, sprint status synced.
