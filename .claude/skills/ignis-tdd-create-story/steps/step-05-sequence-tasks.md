# Step 05 — Sequence Tasks/Subtasks as RED → GREEN → REFACTOR units

**Goal:** turn the Test Intent backlog into an ordered, test-first task list.

## Actions

1. Turn each Test Intent behavior into ONE task, phrased test-first: "Write failing test for &lt;behavior&gt;, implement to green, then refactor to the project refactor bar (see ignis-tdd-dev)." Do not redefine the refactor bar here — it is owned by the dev skill's step-06 / checklist.
2. Order tasks by dependency so earlier greens unblock later behaviors. Group by AC where natural.
3. Add a final task: "Verify full suite green + File List from `git status`."

## Output

Populate `tasks_subtasks` in the template.

## Next

Proceed to `step-06-readiness-gate.md`.
