# SDLC Phase Reference

Compact phase→artifact mapping for the get-project-status skill.

---

## Configuration-Driven Folder Paths

The skill MUST read `_bmad/bmm/config.yaml` to resolve actual folder locations:

| Config Key | Purpose | Default |
|------------|---------|---------|
| `planning_artifacts` | Phases 1-3 planning docs + Solutioning architecture | `_bmad-output/planning-artifacts` |
| `implementation_artifacts` | Phase 4 sprint/story files | `_bmad-output/implementation-artifacts` |
| `project_knowledge` | Phase 0 project documents (existing system docs) | `docs` |
| `output_folder` | General output folder (brainstorming, lifecycle-status) | `_bmad-output` |

---

## Phase 0: Context (Foundation & Onboarding)

| Activity | Activity ID | Artifact Indicators |
|----------|-------------|---------------------|
| Generate Project Context | `generate-project-context` | project-context.md |
| Generate Project Documents | `generate-project-documents` | architecture overviews (of existing system), functional specifications, API contracts, component catalogues, source-tree analysis, and any other project documentation |

**Scan Location:** `{project_knowledge}/` folder (configured in config.yaml, default: `docs/`)
**Output Location:** project-context.md at `{output_folder}/project-context.md` or `**/project-context.md`

**CRITICAL DISTINCTION:** Architecture documents in `{project_knowledge}/` describe the EXISTING system (Phase 0 — Context). Architecture documents in `{planning_artifacts}/` describe the system being DESIGNED (Phase 3 — Solutioning). Do not conflate them.

---

## Phase 1: Ideation (Discovery & Vision)

| Activity | Activity ID | Artifact Indicators |
|----------|-------------|---------------------|
| Product Vision | `product-vision` | product-vision.md |
| Market Research | `market-research` | market-research.md, market-research-*.md |
| Vision Brainstorm | `vision-brainstorm` | brainstorming outputs (vision-*) |
| User Journey Brainstorm | `user-journey-brainstorm` | brainstorming outputs (user-journey-*) |
| Product Brief | `product-brief` | product-brief.md |

**Scan Locations:** `{output_folder}/brainstorming/`, `{planning_artifacts}/`, `{project_knowledge}/`
**Phase Exit:** Product Brief consolidates all research → feeds into Definition.

---

## Phase 2: Definition (Requirements & Design)

| Activity | Activity ID | Artifact Indicators |
|----------|-------------|---------------------|
| PRD (Requirements) | `prd` | prd.md, addendum.md, decision-log.md |
| UX Design Spec | `ux-design-spec` | ux-spec.md, ux-design.md |

**Scan Locations:** `{planning_artifacts}/`
**Phase Exit:** PRD validated + UX coverage. **PRD is a hard prerequisite for Phase 3.**

---

## Phase 3: Solutioning (Architecture & Breakdown)

| Activity | Activity ID | Artifact Indicators |
|----------|-------------|---------------------|
| Architecture Document | `architecture` | architecture.md |
| Epics & Stories | `epics-and-stories` | epics.md, epic-*.md, stories list files |
| Implementation Readiness | `implementation-readiness` | implementation-readiness.md, readiness-report.md |

**Scan Locations:** `{planning_artifacts}/` ONLY
**DEDUPLICATION RULE:** If architecture document (or any file) was already found in `{project_knowledge}/` folder and classified under Phase 0, it MUST NOT be listed again under Phase 3. Phase 3 architecture only counts if a SEPARATE architecture document exists  in `{planning_artifacts}/`.
**Prerequisites:** Phase 2 PRD must exist.
**Phase Exit:** Implementation Readiness passes. **Architecture + Epics are hard prerequisites for Phase 4.**

---

## Phase 4: Implementation (Sprint Execution)

| Activity | Activity ID | Artifact Indicators |
|----------|-------------|---------------------|
| Sprint Planning | `sprint-planning` | sprint-status.yaml |
| Create Story | `create-story` | story-*.md files |
| Dev Story | `dev-story` | story implementation evidence (code changes) |
| Code Review | `code-review` | code-review outputs |
| QA Automation | `qa-automation` | e2e test files, QA outputs |
| Retrospective | `retrospective` | retrospective.md, retro-*.md |

**Scan Locations:** `{implementation_artifacts}/`
**Prerequisites:** Phase 3 Architecture + Epics & Stories must exist.
**Sprint Status Parsing:** Read `sprint-status.yaml` for story counts. Valid statuses: backlog, ready-for-dev, in-progress, review, done.

---

## Phase 5: Quick Flow (Parallel Track)

| Activity | Activity ID | Artifact Indicators |
|----------|-------------|---------------------|
| Quick Spec | `quick-spec` | spec-*.md |
| Quick Dev | `quick-dev` | quick-dev outputs, implementation from quick specs |

**Scan Locations:** `{output_folder}/`, `{planning_artifacts}/`
**Note:** Parallel track — does NOT affect Phases 0-4 status. Both can be in_progress simultaneously.
**Prerequisite:** Only project-context.md (from Phase 0) is recommended.

---

## Phase Gate Prerequisites (Blocker Rules)

| Gate | Prerequisite | Blocker If Missing |
|------|-------------|-------------------|
| Phase 2 → Phase 3 | PRD (prd.md) | Architecture work invalid without requirements |
| Phase 3 → Phase 4 | Architecture doc + Epics & Stories | Implementation has no technical guidance or work breakdown |
