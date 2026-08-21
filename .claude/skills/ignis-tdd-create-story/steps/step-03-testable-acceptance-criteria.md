# Step 03 — Author TESTABLE acceptance criteria

**Goal:** every acceptance criterion is an observable behavior a test could assert.

## Actions

1. For each acceptance criterion, write it as an observable behavior. Prefer Given/When/Then, or a clear "when X, the system does Y (measurable)" statement.
2. Attach concrete, checkable values: exact outputs, status codes, thresholds with numbers and units, specific error messages/types. No adjectives standing in for assertions.

## HALT

**HALT if** any criterion cannot be expressed as something a test could assert.
Rewrite it until it can. If the underlying requirement is genuinely unmeasurable,
flag it to the user and ask for a measurable definition before continuing. Do not
emit an untestable acceptance criterion.

## Output

Populate `acceptance_criteria` in the template.

## Next

Proceed to `step-04-test-intent-backlog.md`.
