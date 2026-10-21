# Project Pulse implementation plan

## Goal and scope

Build a polished, accessible, responsive static dashboard that helps contributors understand projects, owners, status, recent activity, priorities, and short contributor-friendly summaries.

This document is a plan only; implementation requires subsequent Orchestrator assignments. The Planner inspected `.github/project-pulse-brief.md`, `.github/steps/2-step.md`, `.github/steps/3-step.md`, `.github/workflows/2-step.yml`, `.github/workflows/3-step.yml`, `.github/agents/`, and `docs/agent-team.md`. The Planner reported that `app/` was empty. Preserve the existing `.vscode/tasks.json`.

Assume fictional sample data, vanilla JavaScript inline in `app/index.html`, and a proposed `summary` data field. Do not introduce a framework, backend, authentication, filtering, or external assets.

## File assignments and responsibilities

| Exclusive owner | File | Responsibility |
| --- | --- | --- |
| Coder | `app/index.html` | Semantic layout, data fetching, safe rendering, and accessible loading, empty, and error behavior. |
| Designer | `app/styles.css` | Responsive layout, typography, spacing, cards, badges, priorities, contrast, and focus styling. |
| Coder | `app/project-data.json` | Valid, deterministic fictional project fixtures. |
| Coder | `.vscode/launch.json` | Strict JSON preview configuration that serves the app and opens the dashboard. |

The Designer reviews HTML but sends requested markup changes to the Coder; the Designer does not edit HTML. The Coder does not edit CSS. The Orchestrator coordinates assignments, contract approval, dependencies, and integrated acceptance validation.

## Phase 1: Agree on the implementation contract

**Dependency:** None. Do not implement during this phase.

- Designer defines visual hierarchy, responsive behavior, status and priority presentation, and loading, empty, warning, and error states.
- Coder defines the data schema and invalid-record handling.
- Agree on `.dashboard`, `.project-card`, badge, and state styling hooks.
- Use a top-level `projects` array. Each record must contain nonempty strings for `name`, `owner`, `status`, `recentActivity`, and `priority`. Propose a `summary` string for the contributor-friendly summary and obtain approval.
- Agree on status and priority vocabularies, including neutral styling for unknown values.
- Agree on invalid-record behavior; the recommendation is to omit invalid records and display a visible warning.

**Gate:** The Orchestrator approves the contract before dependent work begins.

## Phase 2: Create data, styles, and launch support

**Dependency:** Approved Phase 1 contract.

### Coder: project fixtures

Create `app/project-data.json` with multiple fictional projects covering varied statuses and priorities. Validate JSON syntax and the agreed field requirements before HTML work depends on the data.

### Designer: dashboard styling

Create `app/styles.css` using the agreed hooks and states. Include `.dashboard` and `.project-card` selectors, `border-radius`, `box-shadow`, a responsive grid, readable spacing, and non-color status cues. Cover typography, badges, priorities, focus visibility, contrast, and long content.

### Coder: runnable preview

Create `.vscode/launch.json` as strict JSON without comments:

- Configuration name: `Run Project Pulse Dashboard`.
- Working directory (`cwd`): `${workspaceFolder}/app`.
- Command: `python3 -m http.server 5500`.
- `serverReadyAction` opens `http://localhost:%s/index.html`.
- Select a launch type actually supported in the environment; do not assume a Python debugger is installed.
- Ensure the server output, readiness pattern, captured port, and output buffering work together so the browser opens reliably.
- Open `index.html`, not a directory listing, and preserve `.vscode/tasks.json`.

**Acceptance:** Fixtures parse and meet the agreed schema; CSS covers the agreed hooks and states; the launch configuration is strict JSON.

## Phase 3: Implement the data-driven dashboard

**Dependencies:** Approved Phase 1 contract and validated project data from Phase 2. Remaining CSS work may overlap only while the agreed hooks remain stable.

Coder creates `app/index.html` with:

- Exact document title and main heading `Project Pulse`.
- Relative references to `styles.css` and `project-data.json`.
- One visible `.project-card` per valid project record.
- Project name, owner, status, recent activity, priority, and the agreed summary.
- Semantic headings, labeled metadata, safe text insertion, and accessible status messages.
- Loading, empty, fetch-failure, malformed-data, and JavaScript-disabled states.
- Explicit handling of invalid records according to the approved contract.
- Inline JavaScript only, with no extra implementation files.

Render from the JSON data; do not duplicate a hardcoded dataset in HTML or JavaScript.

## Phase 4: Integrate, review, and validate

**Dependency:** All implementation artifacts are available.

- Coder performs behavior and runtime review.
- Designer performs visual and accessibility review and makes CSS-only fixes.
- Coder makes any required markup or behavior changes.
- Orchestrator checks integrated acceptance against the brief and this plan.
- Exercise the actual named launch configuration, fix failures, and rerun affected checks.
- Report blockers rather than treating unverified behavior as complete.
- Do not stage, commit, or push; Git operations remain under learner control.

## Parallel work decisions

- After contract approval, Designer CSS work and Coder JSON/launch work can proceed in parallel because file ownership is disjoint and the shared contract is stable.
- HTML waits for validated data and agreed hooks. It may overlap remaining CSS work only under the stable contract.
- Contract changes require coordination and approval before dependent work continues.
- Integrated browser review waits for all artifacts.
- Never allow concurrent overlapping edits to the same file.

## Validation expectations

### Static and data checks

- Run `python3 -m json.tool` on both `app/project-data.json` and `.vscode/launch.json`.
- Check the project schema separately; JSON syntax validation alone is insufficient.
- Verify required HTML/CSS hooks, exact title, relative asset/data references, and launch fields.
- Confirm the rendered card count and displayed values match valid project records.

### Runtime and robustness checks

- Test an empty `projects` array, missing or non-array `projects`, malformed JSON, failed requests, invalid records, unknown status values, and long strings.
- Confirm markup-like strings render as text, not executable code or interpreted markup.
- Confirm accessible loading, empty, warning, and error messages, plus a useful JavaScript-disabled message.

### Visual and accessibility checks

- Review at 320px, 768px, and desktop viewport widths, plus 200% zoom, without horizontal overflow or clipping.
- Check heading order, keyboard access and visible focus for interactive elements, readable contrast, and status meaning without reliance on color.
- Confirm a polished responsive card layout, readable spacing, clear hierarchy, and understandable priorities.

### Launch and integration checks

- Run **Run Project Pulse Dashboard** and confirm it automatically opens `/index.html`.
- Verify styles and data load successfully, the dashboard appears instead of a directory listing, and there are no console errors.
- Stop and restart the preview to check cleanup; stop the server after validation.
- Report an occupied port or Codespaces port-forwarding issue explicitly.

Workflow keyword checks are not sufficient dashboard validation. `scripts/validate-exercise.sh` includes template-specific tracked-file checks and is not a dashboard acceptance suite.

## Open implementation contract decisions and risks

- Confirm fictional fixtures versus any real-data requirement.
- Approve the proposed `summary` field.
- Confirm a supported launch type and working readiness detection.
- Approve invalid-record behavior; omitting invalid records with a visible warning is recommended.
- Treat the Python HTTP server as development-only, not production hosting.

These are decisions for the implementation contract, not blockers to saving this plan.
