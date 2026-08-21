# Get Project Status — Workflow

**Goal:** Scan workspace files via MCP and return SDLC lifecycle status as a single JSON object.

**Mode:** Headless only. No interactive prompts. No markdown fences. No explanation text.

---

## CRITICAL: OUTPUT FORMAT

Your ENTIRE response to the user must be ONLY the raw JSON object. Nothing else.

- Do NOT narrate what you are doing
- Do NOT say "let me scan" or "now I'll check" or ANY conversational text
- Do NOT wrap output in markdown code fences (no ```json)
- Do NOT include any text before or after the JSON
- Do NOT ask follow-up questions
- Do NOT include commentary or explanations
- Perform all scanning steps internally/silently
- The first character of your response must be `{` and the last must be `}`

If you include ANY text outside the JSON object, the application will fail to parse the response.

---

## CONSTRAINTS

- Output MUST be a single valid JSON object conforming to the schema in `./references/output-schema.md`
- All scanning steps are INTERNAL — never surface them in output

## PATH CONVENTIONS

**CRITICAL: All paths passed to `listDirectory` and `readFile` must be workspace-relative.**

The system prompt strips `{project-root}/` before you use any path. This means:
- `{project-root}/docs` → use `docs`
- `{project-root}/_bmad/bmm/config.yaml` → use `_bmad/bmm/config.yaml`
- `{project-root}` (root itself, no trailing slash) → use `"."` (dot)

**Workspace root = `"."`** (a single dot character)
- `"."` correctly resolves to the workspace root — ALWAYS USE THIS for root
- `""` (empty string) is REJECTED by the server — DO NOT USE
- `/` is an absolute path rejected by the server — DO NOT USE

Examples:
  - List workspace root → path: `"."`
  - List docs folder → path: `docs`
  - Read config → path: `_bmad/bmm/config.yaml`

## ALLOWED TOOLS

Use following MCP tools:
- `listDirectory` — to discover workspace structure
- `readFile` — to read specific files
- 'fileSearch' — to search for files by glob pattern
- 'textSearch' — to search for text within files

Do NOT call any other tool. Specifically:
- Do NOT call `getWorkspaceGitStatus` — git status is irrelevant for SDLC phase scanning
- Do NOT call `createFile`, `writeFile`, `editFile`, or `deleteFile`
- Do NOT call any Jira, GitHub, or other integration tools

After completing all scan steps and assembling the JSON, output it immediately and STOP. Do not make any further tool calls.

## TOOL CALL BUDGET

**You have a maximum of 10 tool calls total.** Plan your calls carefully.

- NEVER recursively traverse subdirectories. Only list directories explicitly named in the workflow steps.
- DO NOT list src/, .github/, node_modules/, or any source code directories — they are irrelevant for SDLC phase detection.
- Prefer `fileSearch` with glob patterns (e.g., `**/*.md`) over multiple `listDirectory` calls when looking for specific files.
- Combine information from a single `listDirectory` call — do NOT call listDirectory on every subfolder.
- If a directory was already listed, do NOT list it again.
- Stop scanning as soon as you have enough information to determine phase status (you do NOT need to find every file).

**SEARCH SCOPE RULES:**
- ONLY search/list within paths resolved from config.yaml: `{output_folder}/`, `{project_knowledge}/`, `{planning_artifacts}/`, `{implementation_artifacts}/`
- NEVER search or read files inside `_bmad/bmm/` — that folder contains framework templates and workflow definitions, NOT project artifacts.
- NEVER search or read files inside `_bmad/core/`, `_bmad/_config/`, or `_bmad/_memory/` — those are agent infrastructure.
- If `fileSearch` returns results from `_bmad/bmm/` paths, IGNORE those results entirely.
- The ONLY relevant project output folders are those resolved in Step 1 (e.g., `{output_folder}/planning-artifacts/`, `{output_folder}/implementation-artifacts/`, `{project_knowledge}/`).
---

## SDLC PHASE MODEL (6 Phases)

Full phase→artifact mapping: `./references/sdlc-phases.md`

| Phase | Name | Domain | Key Artifacts |
|-------|------|--------|---------------|
| 0 | Context | Foundation & Onboarding | project context document, project knowledge documents like API contract, project overview, existing architecture |
| 1 | Ideation | Discovery & Vision | product-vision, market-research, brainstorming, product-brief |
| 2 | Definition | Requirements & Design | PRD , UX design spec documents |
| 3 | Solutioning | Architecture & Breakdown | architecture document, epics & stories, implementation-readiness |
| 4 | Implementation | Sprint Execution | sprint-status.yaml, story files, code reviews, QA tests |
| 5 | Quick Flow | Parallel Track | quick specs (spec-*.md), quick dev artifacts |

### Phase Gate Prerequisites (Critical for Blocker Detection)

- **Phase 2 → Phase 3:** A PRD (`prd.md`) is REQUIRED before architecture is valid
- **Phase 3 → Phase 4:** Architecture doc + Epics & Stories are REQUIRED before implementation

---

## EXECUTION

<workflow>

<step n="1" goal="Load workspace configuration">
  <action>Read `_bmad/bmm/config.yaml` to resolve configured folder paths</action>
  <action>Extract these variables (strip `{project-root}/` prefix to get workspace-relative paths):</action>
  - `{planning_artifacts}` — where Phases 1-3 planning docs are stored (e.g., `_bmad-output/planning-artifacts`)
  - `{implementation_artifacts}` — where Phase 4 sprint/story files are stored (e.g., `_bmad-output/implementation-artifacts`)
  - `{project_knowledge}` — where Phase 0 project documents are stored (e.g., `docs`)
  - `{output_folder}` — general output folder (e.g., `_bmad-output`)
  <action>If config.yaml is not found, use defaults: planning_artifacts=`_bmad-output/planning-artifacts`, implementation_artifacts=`_bmad-output/implementation-artifacts`, project_knowledge=`docs`, output_folder=`_bmad-output`</action>
</step>

<step n="2" goal="Discover workspace structure">
  <action>Call listDirectory with path `"."` (dot = workspace root) to identify top-level folders and files</action>
  <action>Look for: {output_folder}/, {project_knowledge}/</action>
  <action>If {output_folder} exists, list its contents including  planning-artifacts/ and implementation-artifacts/</action>
  <action>If {project_knowledge} exists, list its contents for project documents and context documents</action>
  <action>DO NOT list src/, .github/, or any other directories not mentioned above — they are not relevant for SDLC phase detection</action>
</step>

<step n="3" goal="Scan for Phase 0 (Context) artifacts">
  <critical>ALL files found inside {project_knowledge}/ (typically `docs/`) belong to Phase 0 — Context. This may include architecture document,  api contracts document , component inventory document, data model , source tree analysis document of any other document that describes the existing system. They are NOT Solutioning or Definition phase artifacts. Do NOT classify them under any other phase.</critical>

  <action>You may use `fileSearch` or listDirectory to search/list project context file (e.g. project-context.md) in either {project_knowledge} folder or directly in {output_folder} folder.</action>
  <action>All other project documentation can be found in {project_knowledge}/ folder and those documents are  Context phase artifacts.</action>
  <action>If a project context file is found: mark activity "Generate Project Context" with status based on content (has content = completed, stub = draft)</action>
  <action>If ANY project documentation found in {project_knowledge}/: mark activity "Generate Project Documents" with appropriate status. List each file as an individual activity entry under Phase 0.</action>
</step>

<step n="4" goal="Scan for Phase 1 (Ideation) artifacts">
  <action>Use `fileSearch` or listDirectory to find the planning artifacts in {planning_artifacts}/ folder in one call. IMPORTANT: Scope your fileSearch glob to `{planning_artifacts}/**/*.md` — do NOT use a workspace-wide glob like `**/*.md` which returns framework template files from _bmad/bmm/. From the results, identify Phase 1 files: product brief (e.g. product-brief.md, *brief*.md), vision document (e.g. vision.md, *vision*.md),  market research (e.g. market-research*.md, domain-research*.md, *research*.md), brainstorming outputs (e.g. *brainstorm*.md, *session*.md, *summary*.md)</action>
  <action>Map found artifacts to activities: Product Vision, Market Research, Vision Brainstorm, User Journey Brainstorm, Product Brief</action>
</step>

<step n="5" goal="Scan for Phase 2 (Definition) artifacts">
  <action>From the fileSearch results in step 4, identify Phase 2 files: like PRD (e.g. prd.md, *prd*.md or prd/ directory), UX Design spec (e.g.ux-spec.md, ux-design.md, ux-design-specification.md, or any other us design specification variant</action>
  <action>Map to activities: PRD (Requirements), UX Design Spec</action>
  <action>Note: PRD existence is critical for Phase 3 prerequisite checking</action>
</step>

<step n="6" goal="Scan for Phase 3 (Solutioning) artifacts">
  <critical>ONLY files found inside {planning_artifacts}/ count as Solutioning artifacts. If architecture document exists in {project_knowledge}/ (e.g. docs/architecture.md), it was ALREADY classified as Phase 0 in step 3 — do NOT duplicate it here. Phase 3 architecture is ONLY found at {planning_artifacts}/ folder (e.g. {planning_artifacts}/architecture.md).</critical>

  <action>From the fileSearch results in step 4, identify Phase 3 files in {planning_artifacts}/ folder: architecture document, epics in either {planning_artifacts}/ or {planning_artifacts}/epics/ directory, stories in {planning_artifacts}/epic*/ directory or {planning_artifacts}/epics/epic*/stories/ directory, implementation-readiness.md or readiness-report.md</action>
  <action>Map to activities: Architecture Document (only if found in {planning_artifacts}/), Epics & Stories, Implementation Readiness</action>
</step>

<step n="7" goal="Scan for Phase 4 (Implementation) artifacts">
  <action>Use `fileSearch` with glob `{implementation_artifacts}/**` or `listDirectory` on `{implementation_artifacts}/` to find all implementation files. Identify: sprint-status.yaml, story files, code-review outputs, QA test files. Do NOT search outside {implementation_artifacts}/.</action>
 
  <action>If sprint-status.yaml exists at `{implementation_artifacts}/sprint-status.yaml`, read it (1 readFile call) for story counts and completion data to populate sprintStatus</action>
  <action>Map to activities: Sprint Planning, Create Story, Dev Story, Code Review, QA Automation, Retrospective</action>
</step>

<step n="8" goal="Scan for Phase 5 (Quick Flow) artifacts">
  <action>From the fileSearch results in steps 4 and 7, identify Phase 5 files: *spec-*.md (quick specs), quick-dev outputs in {implementation-artifacts}/ or {planning_artifacts}/ folders</action>
  <action>Map to activities: Quick Spec, Quick Dev</action>
</step>

<step n="9" goal="Apply phase status rules">
  <action>For each phase, determine status:</action>
  - Phase has relevant artifacts found → "in_progress" (or "completed" if outputs appear final/approved)
  - Phase has NO artifacts but a LATER phase does → "skipped"
  - Phase has NO artifacts and no later phase is active → "not_started"
  <action>Set currentPhase = highest-numbered phase with status "in_progress"</action>
  <action>If no phases are in_progress, set currentPhase to Phase 0 with status "in_progress"</action>
</step>

<step n="10" goal="Detect prerequisite blockers and generate next steps">
  <critical>EVERY nextStep title and description MUST reference SPECIFIC file names, folder names, or artifact names that YOU ACTUALLY FOUND (or confirmed missing) during steps 2-8. If you did not scan it, do not mention it. NEVER invent epic numbers, story numbers, or artifact names.</critical>

  <action>Check prerequisite rules:</action>
  - Phase 3 active but Phase 2 has NO PRD → HIGH blocker referencing the specific missing file
  - Phase 4 active but Phase 3 missing architecture or epics → HIGH blocker naming what is missing

  <action>Generate 3-5 next steps in priority order:</action>
  1. ALL missing prerequisite blockers first (high priority) — name the ACTUAL missing file/artifact from your scan
  2. Logical next action for the current in_progress phase — reference ACTUAL filenames you found
  3. Recommended optional improvements based on what you OBSERVED is missing

  <action>Each nextStep MUST include ALL of these fields:</action>
  - `title`: Short label (max ~60 chars) derived from YOUR SCAN RESULTS. Reference actual filenames/folders you found. Do NOT use generic phrases or example text.
  - `description`: Full actionable guidance referencing specific files/artifacts from your scan. Format: "[Action] [actual artifact name from scan] — [reason based on scan findings]"
  - `priority`: "high" / "medium" / "low"
  - `phase`: phase number (0-5)
  - `activityId`: Exact sidebar activity identifier. Refer to the activityId list in ./references/output-schema.md — REQUIRED for correct UI navigation.
  - `skillName`: BMAD skill path. Refer to the skill list in ./references/output-schema.md.
  - `owner`: Role responsible. Infer from phase/skill.
  - `dueDate`: ISO date if sprint end date found, otherwise null.
</step>

<step n="11" goal="Assemble and output JSON">
  <action>Construct the JSON object conforming to the schema in ./references/output-schema.md</action>
  <action>Ensure ALL 6 phases (0-5) are present in the phases array</action>
  <action>Output ONLY the JSON object — no surrounding text, no code fences, no narration. First char = '{', last char = '}'</action>
</step>

</workflow>

---

## EDGE CASES

- **Empty workspace:** Return Phase 0 "in_progress" with no activities, nextStep = "Generate project context to establish the foundation for AI agents"
- **Only source code (no planning artifacts):** Phase 0 "in_progress" (if project-context.md exists) or "not_started", Phase 4 "skipped" or "in_progress" if story files found, earlier phases "skipped". NextSteps should recommend creating missing planning docs.
- **Sprint status file exists:** Parse it for story counts to populate sprintStatus. Map story statuses: backlog, ready-for-dev, in-progress, review, done. If parsing fails, set sprintStatus to null.
- **Ambiguous artifact status:** Default to "in_progress" rather than "completed" — avoid false completions.
- **Quick Flow artifacts alongside full phases:** Phase 5 is a parallel track. Its presence does NOT affect the status of Phases 0-4. Both can be "in_progress" simultaneously.
