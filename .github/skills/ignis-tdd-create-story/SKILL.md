---
name: ignis-tdd-create-story
description: >-
  Use when drafting a story that must be implemented test-first. Produces
  acceptance criteria that are all testable and a per-behavior Test Intent
  backlog (the RED plan) consumed by ignis-tdd-dev. Alternative to
  bmad-bmm-create-story.
module: ignis
phase: 4-implementation
agent: ignis-sm
menu-code: TCS
---

# ignis-tdd-create-story

Draft ONE story so it is implementable test-first. The distinctive output is a
**Test Intent** backlog: for every acceptance criterion, the concrete behaviors
that `ignis-tdd-dev` will drive RED → GREEN → REFACTOR. A readiness gate blocks
any story that is not test-ready.

## Critical rules (apply throughout)

- **This skill REPLACES create-story for this story.** Do not also run stock
  create-story on it. The story must pass the readiness gate (step 6) before it
  is marked ready.
- **Testability is the acceptance bar.** An acceptance criterion that cannot be
  turned into a failing automated test is not done. Rewrite vague criteria
  ("works well", "is fast", "handles errors gracefully") into observable,
  verifiable statements before the story can be marked ready.

## Inputs
- `epics`, `prd`, `architecture` — planning context for the story.
- `config_source` — `{project-root}/_bmad/ignis/config.yaml`.
- `template.md` — the story shape (defines the Test Intent contract section).
- `checklist.md` — the test-first readiness gate.

## Execution

Run the steps in order, loading each shard just-in-time. Every `Action` is
mandatory. Stop immediately on any `HALT` until its condition is resolved.

1. `steps/step-01-orient.md` — load planning inputs, identify the story.
2. `steps/step-02-narrative-scope.md` — story statement + scope.
3. `steps/step-03-testable-acceptance-criteria.md` — testable ACs.
4. `steps/step-04-test-intent-backlog.md` — the RED plan (contract for dev).
5. `steps/step-05-sequence-tasks.md` — test-first task sequencing.
6. `steps/step-06-readiness-gate.md` — HALT gate; run `checklist.md`.
7. `steps/step-07-finalize.md` — Dev Notes, render via `template.md`, set status.

## Output

A story file in `dev_story_location` with testable acceptance criteria and a
populated Test Intent table, at status `ready_status`, with sprint status synced.
