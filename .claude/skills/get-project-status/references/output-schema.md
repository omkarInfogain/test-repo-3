# Output Schema Reference

The JSON output MUST conform exactly to this schema. No additional fields. No missing required fields.

## Schema

```json
{
  "currentPhase": {
    "number": 0,
    "name": "string",
    "status": "in_progress | completed"
  },
  "phases": [
    {
      "number": 0,
      "name": "string",
      "status": "completed | in_progress | not_started | skipped",
      "completedAt": "ISO 8601 date string | null",
      "activities": [
        {
          "name": "string (descriptive name of the artifact found)",
          "artifactId": "string | null",
          "status": "draft | in_progress | completed | review | approved | backlog | ready_for_dev | under_review",
          "completedAt": "ISO 8601 date string | null"
        }
      ]
    }
  ],
  "nextSteps": [
    {
      "title": "string (short, concise action label derived from YOUR scan findings, max ~60 chars)",
      "description": "string (specific, actionable — names the actual artifact found/missing and explains why)",
      "priority": "high | medium | low",
      "phase": 0,
      "activityId": "string (exact sidebar activity identifier — see activityId values below)",
      "skillName": "string (the BMAD skill to invoke for this step, e.g. '/bmad-create-architecture')",
      "owner": "string (role or team responsible, e.g. 'Product Manager', 'UX Designer', 'Architect', 'Developer')",
      "dueDate": "ISO 8601 date string | null"
    }
  ],
  "sprintStatus": {
    "name": "string (sprint name)",
    "completion": 0,
    "storiesTotal": 0,
    "storiesDone": 0,
    "daysRemaining": null,
    "velocity": "improving | stable | declining | unknown"
  }
}
```

## Field Rules

### currentPhase
- `number`: 0-5 (the highest phase that is "in_progress")
- `name`: Human-readable phase name — must be one of: Context, Ideation, Definition, Solutioning, Implementation, Quick Flow
- `status`: "in_progress" or "completed"

### phases (array of exactly 6 elements, ordered 0-5)
- `number`: 0-5
- `name`: Phase name (Context, Ideation, Definition, Solutioning, Implementation, Quick Flow)
- `status`: One of "completed", "in_progress", "not_started", "skipped"
- `completedAt`: ISO date string if completed, null otherwise
- `activities`: Array of artifacts found for this phase (empty array if none)

### phases[].activities
- `name`: Descriptive name matching the activity from the SDLC journey map:
  - Phase 0: "Generate Project Context", "Generate Source Tree Analysis", "Generate Project Overview", "Generate Project Architecture", "Generate API Specs", "Generate Development Guide", "Generate Project Documents"
  - Phase 1: "Product Vision", "Market Research", "Vision Brainstorm", "User Journey Brainstorm", "Product Brief"
  - Phase 2: "PRD (Requirements)", "UX Design Spec"
  - Phase 3: "Architecture Document", "Epics & Stories", "Implementation Readiness"
  - Phase 4: "Sprint Planning", "Create Story", "Dev Story", "Code Review", "QA Automation", "Retrospective"
  - Phase 5: "Quick Spec", "Quick Dev"
- `artifactId`: Set to null (reserved for future artifact linking)
- `status`: Current status of the artifact
  - For documents: "draft", "in_progress", "completed", "review", "approved"
  - For stories: "backlog", "ready_for_dev", "in_progress", "under_review", "completed"
- `completedAt`: ISO date if completed, null otherwise

### nextSteps (array of 3-5 items)

**GROUNDING RULE: Every nextStep MUST be derived from files/folders you actually found or confirmed missing during your scan. Do NOT invent artifact names, epic numbers, story numbers, or sprint data. If you did not observe it in a listDirectory or readFile result, do not reference it.**

- `title`: **Required.** Short, scannable label for the step (max ~60 characters). Used as the popup heading and card title. Focus on the action + subject, not the reason.
  - MUST reference an actual filename, folder, or artifact you found/confirmed-missing during scanning.
  - Format: "[Verb] [specific artifact name from your scan]"
  - FORBIDDEN: Generic phrases like "Complete code reviews", "Continue development", "Create stories" without referencing actual files you scanned.
- `description`: **Required.** Full, actionable guidance with role context. Format: "[Action] the [actual artifact name from scan] — [reason based on what you observed]"
  - MUST name specific files/folders from your scan results. If you found `docs/architecture.md` is missing, say that exact path.
- `priority`: "high" for blockers/prerequisites, "medium" for logical next actions, "low" for optional improvements
- `phase`: The phase number (0-5) this step belongs to
- `activityId`: **Required.** The exact sidebar activity identifier for navigation. Must be one of:
  - Phase 0: `generate-project-context`, `generate-source-tree-analysis`, `generate-project-overview`, `generate-project-architecture`, `generate-api-specs`, `generate-development-guide`, `generate-project-docs`
  - Phase 1: `product-vision`, `market-research`, `vision-brainstorm`, `product-brief`
  - Phase 2: `prd`, `ux-design-spec`
  - Phase 3: `architecture`, `epics-and-stories`, `implementation-readiness`
  - Phase 4: `sprint-planning`, `create-story`, `dev-story`, `code-review`, `qa-automation`, `retrospective`
  - Phase 5: `quick-spec`, `quick-dev`
  Choose the activityId that matches the specific activity the step refers to (e.g. a step about code review → `code-review`, NOT `sprint-planning`).
- `skillName`: The BMAD skill path that performs this step. Must be one of:
  - `/bmad-generate-project-context`, `/bmad-document-project`
  - `/bmad-tech-writer`, `/bmad-market-research`, `/bmad-brainstorming`, `/bmad-create-product-brief`
  - `/bmad-create-prd`, `/bmad-create-ux-design`
  - `/bmad-create-architecture`, `/bmad-create-epics-and-stories`, `/bmad-check-implementation-readiness`
  - `/bmad-sprint-planning`, `/bmad-create-story`, `/bmad-dev-story`, `/bmad-code-review`, `/bmad-qa-generate-e2e-tests`, `/bmad-retrospective`
  - `/bmad-quick-spec`, `/bmad-quick-dev`
- `owner`: Role or team responsible for executing this step. Infer from phase and skillName:
  - Phase 0: "Technical Writer" or "Developer"
  - Phase 1: "Product Manager"
  - Phase 2 (PRD): "Product Manager"; Phase 2 (UX): "UX Designer"
  - Phase 3 (Architecture): "Architect"; Phase 3 (Epics/Stories): "Product Manager"
  - Phase 4 (Dev Story/Code Review): "Developer"; Phase 4 (QA): "QA Engineer"; Phase 4 (Sprint Planning): "Scrum Master"
  - Phase 5: "Developer"
- `dueDate`: ISO 8601 date string if a sprint end date or timeline is found in workspace files (e.g. sprint-status.yaml, sprint-plan.md); otherwise null.

### sprintStatus (object or null)
- Set to null if no sprint-status.yaml is found
- `name`: Sprint identifier from sprint-status.yaml (e.g., "Sprint 1")
- `completion`: Percentage 0-100 (storiesDone / storiesTotal * 100, rounded)
- `storiesTotal`: Total story count in the sprint
- `storiesDone`: Stories with status "done" or "completed"
- `daysRemaining`: null (unless sprint end date is found)
- `velocity`: "unknown" unless trend data is available from multiple sprints
