# Project Pulse dashboard implementation plan

## Objective and scope

Create a lightweight, polished static dashboard for contributors to quickly see
active projects, their owners, current status, recent activity, priority or
risk, and a short contributor-friendly summary. The result is a browser-ready
static app, not a framework application or a server-side service.

The dashboard must display from `app/index.html` when launched. It must not
land on an HTTP server directory listing. Project records are stored in a
local JSON file and rendered into visible project cards.

## Requirements to preserve

- The page title and visible heading use the exact product name **Project
  Pulse**.
- The page references `styles.css` and loads `project-data.json`.
- `app/project-data.json` has a top-level `projects` array.
- Every project includes `name`, `owner`, `status`, `recentActivity`, and
  `priority`; records should also include a concise contributor-friendly
  `summary` so the dashboard meets the brief.
- The UI renders multiple visible cards from the JSON data. Each card uses the
  `project-card` class and visibly presents the required fields.
- Styling includes `.dashboard` and `.project-card` selectors, responsive
  behavior, status badges, readable spacing, `border-radius`, and
  `box-shadow`.
- `.vscode/launch.json` is strict JSON without comments and contains a
  configuration named **Run Project Pulse Dashboard**. It serves from
  `${workspaceFolder}/app` using `python3 -m http.server 5500` and uses a
  `serverReadyAction` URL format that opens
  `http://localhost:%s/index.html`.

## File assignments

| File | Owner | Deliverable |
| --- | --- | --- |
| `docs/project-pulse-plan.md` | Planner | This implementation plan; no application code belongs here. |
| `app/project-data.json` | Coder | Valid, deterministic sample data with a top-level `projects` array and multiple complete project records. |
| `app/index.html` | Coder, using Designer's decisions | Semantic document structure, accessible empty/loading and error regions, a dashboard container, and explicit JavaScript that fetches the JSON and renders project cards. |
| `app/styles.css` | Designer | Polished visual system and responsive CSS, including `.dashboard`, `.project-card`, status badges, priority treatment, focus styles, rounded corners, and shadows. |
| `.vscode/launch.json` | Coder | Comment-free VS Code launch configuration that serves the `app` directory and opens `index.html`. |

The assignments intentionally keep the Designer responsible for visual CSS
only. The Coder may make only minimal, necessary class-name alignment in HTML
after the Designer handoff; any substantive visual change returns to the
Designer.

## Designer responsibilities

1. Define a dashboard hierarchy: title and purpose first, then a concise
   summary/context area, followed by the responsive project-card collection.
2. Specify a restrained visual direction suitable for a contributor tool:
   clear typography, a high-contrast neutral surface, one purposeful accent
   color, and semantic status/priority colors that do not rely on color alone.
3. Provide CSS hooks the Coder can implement consistently, including
   `.dashboard`, `.project-card`, card metadata rows, badge classes, priority
   treatment, loading/error treatment, and an accessible visually-hidden
   utility if needed.
4. Implement responsive rules so cards are readable on narrow screens and
   form multiple columns only when adequate width is available. Avoid clipped
   text, hover-only information, and fixed card heights.
5. Add keyboard-visible focus styles and ensure contrast remains legible for
   body copy, badges, and priority indicators.
6. Report the expected markup/class contract and visual validation checklist
   to the Coder before integration.

## Coder responsibilities

1. Create deterministic sample content in `app/project-data.json`: at least
   three projects, each with `name`, `owner`, `status`, `recentActivity`,
   `priority`, and a short `summary`. Use realistic but static values; do not
   call a remote API.
2. Create semantic `app/index.html` with document language, viewport metadata,
   exact Project Pulse title, stylesheet link, a main dashboard landmark, and
   a named region for the project collection.
3. Reference `project-data.json` in client-side JavaScript. Fetch it from the
   same static origin, verify that `projects` is an array, and render project
   cards from that array rather than hard-coding card content.
4. Use safe DOM APIs such as `textContent` when inserting JSON values. Each
   rendered `project-card` must visibly show name, owner, status,
   recentActivity, priority, and summary; use labels or semantic elements so
   values retain context.
5. Handle loading, empty-data, and fetch/shape failures with an informative
   visible message. Keep the error deterministic and avoid a blank page.
6. Create `.vscode/launch.json` as strict JSON with no comments. Configure a
   Python launch that runs `python3 -m http.server 5500`, sets
   `"cwd": "${workspaceFolder}/app"`, has the exact display name **Run Project
   Pulse Dashboard**, and uses `serverReadyAction` to open
   `http://localhost:%s/index.html`.
7. Verify the implementation against the Designer's supplied class contract;
   report any required contract clarification rather than silently changing
   the visual design.

## Implementation sequence and dependencies

### Phase 1 — contract and content model

1. The Planner supplies this plan and the requirements above.
2. The Designer defines the visual hierarchy and HTML/CSS class contract.
3. The Coder confirms the JSON schema and the renderer fields against that
   contract, then creates the sample data.

The Designer's class contract must be available before the Coder finalizes
HTML class names. The data schema can be finalized independently because the
required fields are fixed by the brief.

### Phase 2 — parallel production work

Once the class contract and JSON schema are agreed:

- **Designer:** implement `app/styles.css`.
- **Coder:** implement `app/project-data.json`, semantic/rendering logic in
  `app/index.html`, and `.vscode/launch.json`.

These tracks can run concurrently because their file ownership does not
overlap. The Coder should use the agreed selector names; the Designer should
not edit HTML, data, or launch configuration.

### Phase 3 — sequential integration

After both parallel tracks finish:

1. Coder integrates the final CSS class names with the rendered markup, making
   only contract-alignment edits in `app/index.html`.
2. Coder runs JSON and markup/reference checks, then starts the launch
   configuration.
3. Designer reviews the running dashboard at desktop and narrow viewport
   widths, returning only visual/accessibility adjustments to `styles.css`.
4. Coder reruns all validation after any returned CSS changes.

Browser preview must be sequential after the app files and launch
configuration exist. Final acceptance must be sequential after the integrated
review; do not declare success based only on isolated file checks.

## Concrete validation expectations

### Static checks

Run the following checks from the repository root:

1. Confirm all four assigned implementation files exist.
2. Run `python3 -m json.tool app/project-data.json` and
   `python3 -m json.tool .vscode/launch.json`; both commands must exit
   successfully.
3. Inspect `app/project-data.json` to confirm one top-level `projects` array,
   at least three records, and all required fields on every record.
4. Inspect `app/index.html` to confirm it includes `Project Pulse`, links
   `styles.css`, references `project-data.json`, and produces
   `project-card` elements from the projects data.
5. Inspect `app/styles.css` to confirm `.dashboard`, `.project-card`,
   `border-radius`, and `box-shadow` are present, along with responsive and
   keyboard-focus rules.
6. Inspect `.vscode/launch.json` to confirm the exact configuration name,
   `cwd` of `${workspaceFolder}/app`, the prescribed Python server command,
   and a server-ready URL that ends in `/index.html`.

### Runtime and accessibility checks

1. In VS Code Run and Debug, select **Run Project Pulse Dashboard** and start
   it.
2. Confirm the browser opens `http://localhost:5500/index.html` (or the
   server-ready equivalent), not the server root directory listing.
3. Confirm all sample projects render as separate visible cards and that each
   card exposes owner, status, recent activity, priority, and summary.
4. Narrow the browser viewport and confirm the layout reflows without
   horizontal scrolling, clipped content, or illegible badges.
5. Use keyboard Tab navigation to confirm any interactive/focusable elements
   show a visible focus indicator. Verify headings, landmarks, labels, and
   status/priority text convey meaning without color alone.
6. Temporarily test the renderer's unavailable or malformed-data path, or
   inspect it directly, to confirm a user-facing error is shown rather than a
   silent blank dashboard. Restore valid project data before final review.
7. Stop the preview server after verification.

## Completion criteria and handoff

The work is complete only when all JSON parses, the four assigned files match
their responsibilities, the launch configuration opens `index.html`, and the
runtime dashboard is polished, responsive, accessible, and populated from its
JSON data. The final handoff should list files changed, validation performed,
and any residual browser-specific risk. No staging, commit, or push is part of
this plan.
