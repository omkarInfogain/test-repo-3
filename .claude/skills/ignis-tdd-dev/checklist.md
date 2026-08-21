# ignis-tdd-dev — Definition of Done

Run against the story at completion (step 8). Every box must be **truthfully**
checkable. HALT gate: if any box fails, TDD was skipped for a behavior — return
to RED for it.

## Test-first discipline (non-negotiable)

- [ ] Every Test Intent behavior has a corresponding test.
- [ ] For every behavior, the test was written **before** its production code.
- [ ] Each test was **run and watched fail** before implementation (RED evidence recorded per behavior).
- [ ] Each test failed because the **feature was missing** — not a typo/import/fixture error.
- [ ] No production code exists that was written before its failing test.

## Coverage of the backlog

- [ ] Every acceptance criterion's behaviors (happy path, edge cases, error paths) from Test Intent are implemented.
- [ ] Behaviors were implemented at the `test_level` specified in Test Intent.
- [ ] Any `tea_candidate` behaviors were driven RED-first (inline or via TEA *atdd).

## Green + refactor

- [ ] Minimal code written to pass (no speculative features — YAGNI).
- [ ] Full suite passes 100% with a **pristine** run (no new warnings vs. baseline).
- [ ] No previously-passing test regressed.
- [ ] Refactoring changed structure, not behavior, and the suite stayed green.
- [ ] Refactor met the bar: no new duplication, lint/static analysis clean on changed files, no dead branches, suite green.

## Test quality

- [ ] Tests exercise real code paths; mocks only where a real dependency was unavailable.
- [ ] Tests assert observable behavior, not incidental implementation detail.

## Story hygiene

- [ ] File List derived from `git status` / `git diff` (includes side-effect files).
- [ ] TDD Record + Dev Agent Record record model, RED/GREEN evidence, completion notes.
- [ ] Change Log updated; Status set to `review`; sprint status synced.
- [ ] Only permitted story sections were edited.

---

**Can't check all boxes? You skipped TDD. Start over for the affected behavior.**

_Final rule: production code exists ⇒ a test for it existed and failed first._
