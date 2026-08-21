# Story {{story_id}}: {{story_title}}

Status: {{status}}
Epic: {{epic_ref}}

## Story

{{story_statement}}

## Scope

**In scope:** {{in_scope}}

**Out of scope:** {{out_of_scope}}

## Acceptance Criteria

<!-- Each AC must be testable: an observable behavior with concrete, checkable values. -->
{{acceptance_criteria}}

<!-- Example:
- **AC-1** — Given a registered user with a valid token, when they GET /profile, then the response is 200 with their email and no password hash.
- **AC-2** — Given an expired token, when they GET /profile, then the response is 401 with error code "token_expired".
-->

## Test Intent  (RED backlog — consumed by ignis-tdd-dev)

<!--
  Contract with ignis-tdd-dev. Every AC must have >= 1 behavior.
  test_level: unit | integration | acceptance
  tea_candidate: yes only for acceptance/e2e behaviors that may delegate to TEA *atdd.
-->

| ID   | AC    | Behavior (what the failing test asserts)          | test_level  | Fixtures / seams / data          | tea_candidate |
|------|-------|---------------------------------------------------|-------------|----------------------------------|---------------|
| T-1  | AC-1  | happy path: valid token returns 200 + profile     | integration | seeded user, valid JWT factory   | no            |
| T-2  | AC-1  | profile response never includes password hash     | unit        | user serializer                  | no            |
| T-3  | AC-2  | expired token returns 401 "token_expired"         | integration | expired JWT fixture              | no            |
| ...  | ...   | ...                                               | ...         | ...                              | ...           |

## Tasks / Subtasks

<!-- Each task = one Test Intent behavior, phrased test-first, in dependency order. -->
{{tasks_subtasks}}

## Dev Notes

{{dev_notes}}

## Dev Agent Record

_Model:_
_Debug Log (clean baseline, RED evidence per behavior, GREEN results, final run):_
_Completion notes:_

### TDD Record

| Behavior | RED (failed first) | GREEN (passed) | Refactored | Test file(s) |
|----------|--------------------|----------------|-----------|--------------|
{{tdd_record_rows}}

## File List

<!-- Leave blank at story creation. Populated by ignis-tdd-dev from `git status` at completion. -->
_Derived from `git status` at completion — includes side-effect files._

## Change Log

| Date | Change | By |
|------|--------|----|
