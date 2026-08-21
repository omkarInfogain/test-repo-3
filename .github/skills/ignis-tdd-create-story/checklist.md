# ignis-tdd-create-story — Test-First Readiness Gate

Run against the drafted story (step 6). Every box must be truthfully checkable,
or the story is **not** ready for `ignis-tdd-dev`.

## Testable acceptance criteria

- [ ] Every acceptance criterion describes an **observable behavior** (Given/When/Then or a clear "when X → Y").
- [ ] Every criterion has **concrete, checkable values** — exact outputs, status codes, numeric thresholds with units, specific error types/messages.
- [ ] No criterion relies on unmeasurable adjectives ("fast", "robust", "user-friendly") without a defined metric.

## Test Intent backlog (the contract with ignis-tdd-dev)

- [ ] Every acceptance criterion maps to **at least one** Test Intent behavior.
- [ ] No Test Intent behavior exists without a traceable AC reference (every behavior's AC column resolves to a real acceptance criterion).
- [ ] Each behavior lists its **test level** (unit / integration / acceptance).
- [ ] Happy path, edge/boundary cases, and error/failure paths are each represented where applicable.
- [ ] Fixtures, seams, and test data needed to make each behavior testable are named.
- [ ] Acceptance/e2e behaviors are flagged `tea_candidate` if TEA delegation is intended.

## Test-first task sequencing

- [ ] Every task corresponds to one Test Intent behavior and is phrased test-first (failing test → minimal code → refactor).
- [ ] Tasks are ordered by dependency so earlier greens unblock later behaviors.
- [ ] A final task verifies the full suite is green and the File List is derived from `git status`.

## Story hygiene

- [ ] Story statement, in/out-of-scope, and Dev Notes are present.
- [ ] Dev Notes reference the applicable coding + testing standards from project-context.
- [ ] Status set to the configured ready value; sprint status synced.

---

**Any box unchecked ⇒ the story is not test-ready. Fix it before handoff.**
