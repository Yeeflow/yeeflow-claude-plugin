# docs/studies collection 1

This file combines 24 documents from `docs/studies/` for the Claude directory build. A reference to `docs/studies/<name>.md` is the section that starts at the anchor `#<name>` below.

- [advanced-controls-runtime-proof.md](#advanced-controls-runtime-proof)
- [advanced-controls.md](#advanced-controls)
- [agent-access-application-resources-tool.md](#agent-access-application-resources-tool)
- [ai-agent-image-extraction-inputs.md](#ai-agent-image-extraction-inputs)
- [application-planning-capability-coverage.md](#application-planning-capability-coverage)
- [application-settings-header.md](#application-settings-header)
- [application-settings-navigation-layout.md](#application-settings-navigation-layout)
- [application-settings-navigation-menu.md](#application-settings-navigation-menu)
- [application-settings-runtime-test-plan.md](#application-settings-runtime-test-plan)
- [application-user-groups.md](#application-user-groups)
- [container-button-action-runtime-proof.md](#container-button-action-runtime-proof)
- [container-button-action-settings.md](#container-button-action-settings)
- [data-filter-controls-runtime-proof.md](#data-filter-controls-runtime-proof)
- [data-filter-controls.md](#data-filter-controls)
- [data-list-custom-form-fields.md](#data-list-custom-form-fields)
- [data-list-document-library-field-types.md](#data-list-document-library-field-types)
- [data-list-document-library-permissions-notifications.md](#data-list-document-library-permissions-notifications)
- [data-list-field-creation-runtime-proof.md](#data-list-field-creation-runtime-proof)
- [data-list-layoutview-add-form-runtime-fix.md](#data-list-layoutview-add-form-runtime-fix)
- [data-list-public-form-runtime-proof.md](#data-list-public-form-runtime-proof)
- [data-list-public-forms.md](#data-list-public-forms)
- [data-list-workflow-ai-agent-runtime-test-plan.md](#data-list-workflow-ai-agent-runtime-test-plan)
- [data-view-resource-settings.md](#data-view-resource-settings)
- [doc-library-control-form-hosts-runtime.md](#doc-library-control-form-hosts-runtime)

---

<a id="advanced-controls-runtime-proof"></a>

<!-- source: docs/studies/collection-01.md#advanced-controls-runtime-proof -->

# Advanced Controls Runtime Proof

Runtime status: passed for the focused generated package. The user confirmed that `/Users/Renger/Downloads/advanced-controls-runtime-proof.v1.yap` imported successfully, opened the Advanced Controls Runtime Dashboard, rendered the included advanced controls, and showed no missing binding, render, action, or page-break error.

## Generated Package

- App name: `Advanced Controls Runtime Proof`
- Dashboard: `Advanced Controls Runtime Dashboard`
- Data List: `Advanced Control Runtime Items`
- Data List custom forms: `New Item`, `Edit Item`, `View page`
- Manual-test package path: `/Users/Renger/Downloads/advanced-controls-runtime-proof.v1.yap`
- Local ignored generated package: `advanced-controls-runtime-proof.v1.yap`
- Generator: `tools/generators/generate-advanced-controls-runtime-proof.mjs`

The generated `.yap` package, decoded app data, decoded resource JSON, validation JSON, screenshots, and runtime evidence files are not committed. The committed artifacts are limited to the generator script, this proof doc, safe validator/inspector updates, and skill guidance.

## Controls Included

Dashboard controls:

- Tab: one `aktabs` control with two `ak-tabs-tab` children and nested controls.
- Toggle: one `toggle` control with two `toggle-panel` children and nested controls.
- Timer: one safe static future date under `attrs.set.date`.
- Icon list: two safe static links to Yeeflow-owned public URLs.
- Divider: plain and labeled `line` controls.
- Alert: info, success, warning, and error-style variants.
- Progress bar: static numeric percentage values.
- Spacer: `gap` controls with explicit spacing.
- Progress circle: static percentage values.
- Steps bar: static `steps-options` and static current step.
- QR Code: static `https://<yourdomain>.yeeflow.com/<redacted-path> via `attrs["qr-code-link"].customUrl.url`.
- Barcode: static safe values with `CODE128`.
- Embed: iframe with static public `https://<yourdomain>.yeeflow.com/<redacted-path> URL.

Data List `View page` controls:

- Steps bar: field-bound to the current item `Stage` radio field (`Text1`).
- QR Code: static `https://<yourdomain>.yeeflow.com/<redacted-path> URL.
- Barcode: static safe value.
- Embed: iframe with static public Yeeflow URL.
- Document embed: bound to the current item `Attachment` file-upload field (`Text4`) and expected to render a safe empty state until a safe test file is later added.

## Data List Fields

`Advanced Control Runtime Items` includes only safe synthetic fields and one safe sample row:

- `Title` input
- `Stage` radio (`Plan`, `Build`, `Validate`, `Runtime test`)
- `Progress` percent
- `Link URL` hyperlink
- `Barcode Value` input
- `Attachment` file-upload, empty in sample data

## Local Validation Results

All local validation commands completed with zero errors:

| Check | Result | Notes |
| --- | --- | --- |
| `node --check tools/generators/generate-advanced-controls-runtime-proof.mjs` | pass | generator syntax valid |
| `node --check scripts/inspect-advanced-controls.mjs` | pass | inspector syntax valid |
| `node --check validate-yap-package.js` | pass | package validator syntax valid |
| `node validate-yap-package.js advanced-controls-runtime-proof.v1.yap --mode generator --stage final` | pass_with_warnings, 0 errors | warnings are runtime-sensitive field/control support and UI style guidance |
| `node validate-yap-graph.js advanced-controls-runtime-proof.v1.yap --mode generator --stage final` | pass_with_warnings, 0 errors | warnings are runtime-sensitive field support |
| `node scripts/inspect-advanced-controls.mjs advanced-controls-runtime-proof.v1.yap --page "Advanced Controls Runtime Dashboard" --list-form "View page"` | pass, 0 findings | detected 36 advanced controls across dashboard and View page |
| `node scripts/inspect-yap-schema-standard.mjs advanced-controls-runtime-proof.v1.yap` | pass, 0 findings | wrapper/schema standard check passed |
| `node scripts/inspect-app-creation-rules.mjs advanced-controls-runtime-proof.v1.yap` | pass, 0 findings | app creation rule check passed |
| `node scripts/inspect-yap-materialization.mjs advanced-controls-runtime-proof.v1.yap` | pass_with_warnings, 0 errors | warning: no workflow/form resources, expected for this dashboard/list proof |
| `node scripts/inspect-yap-import-readiness.mjs advanced-controls-runtime-proof.v1.yap` | pass_with_warnings, 0 errors | aggregate gate passed; 36 warnings retained as runtime-sensitive notes |
| `node build-yap-wrapper.js .tmp/advanced-controls-runtime-proof.v1.resource.json .tmp/advanced-controls-runtime-proof.roundtrip.yap --title "Advanced Controls Runtime Proof" --description "Focused safe generated package for runtime proof of Yeeflow advanced controls." --validation-mode generator` | pass | wrapper build/round trip passed |
| `git diff --check` | pass | no whitespace errors |

Warnings intentionally retained:

- Percent, hyperlink, file-upload, and several advanced controls remain runtime-sensitive until this generated package is imported and opened.
- The package has no approval/workflow resources by design.
- UI-style warnings do not block this focused proof because the package is a compact control test harness, not a production app shell.

## User Runtime Result

The user confirmed the generated package imported successfully and the `Advanced Controls Runtime Dashboard` opened.

Passed runtime checks:

1. Tab rendered and tab switching worked.
2. Toggle rendered and expand/collapse worked.
3. Timer rendered.
4. Icon list rendered and links appeared.
5. Divider rendered.
6. Alert variants rendered.
7. Progress bar rendered.
8. Spacer created spacing.
9. Progress circle rendered.
10. Steps bar rendered.
11. QR Code rendered.
12. Barcode rendered.
13. Embed rendered safely without breaking the page.
14. Document embed rendered a safe empty state.
15. No missing binding, render, action, or page-break error appeared.

## Manual Runtime Test Instructions

The following checklist is the completed runtime scope for `/Users/Renger/Downloads/advanced-controls-runtime-proof.v1.yap` and can be reused for future retests:

1. The app imports successfully.
2. `Advanced Controls Runtime Dashboard` opens.
3. Tab control renders and tab switching works.
4. Toggle control renders and expand/collapse works.
5. Timer renders using the static future date.
6. Icon list renders and the two safe public links appear.
7. Divider controls render.
8. Alert variants render.
9. Progress bar renders.
10. Spacer creates visible spacing.
11. Progress circle renders.
12. Static Steps bar renders.
13. QR Code renders for `https://<yourdomain>.yeeflow.com/<redacted-path>
14. Barcode renders for safe static values.
15. Embed renders or fails safely without breaking the page.
16. Open `Advanced Control Runtime Items`, open the included sample row, and open `View page` if needed.
17. Field-bound Steps bar renders from the sample row `Stage` value.
18. Document embed renders a safe empty state because `Attachment` is empty. A safe file can be added later to test non-empty preview behavior.
19. No missing binding, render, action, or page-break error appears.

## Proof Boundary

- Runtime proof is limited to `/Users/Renger/Downloads/advanced-controls-runtime-proof.v1.yap`.
- Runtime proof covers rendering and basic interactions for Tab, Toggle, Timer, Icon list, Divider, Alert, Progress bar, Spacer, Progress circle, Steps bar, QR Code, Barcode, Embed safe render, and Document embed empty state.
- Local validation remains a separate readiness gate and is not a substitute for runtime proof on future packages.
- This proof does not claim QR scan behavior.
- This proof does not claim Barcode scan behavior.
- This proof does not claim external iframe content loading beyond safe render.
- This proof does not claim document preview for non-empty files.
- This proof does not claim dynamic value changes.
- This proof does not claim Approval Form/Public Form host behavior because this package does not include those hosts.

---

<a id="advanced-controls"></a>

<!-- source: docs/studies/collection-01.md#advanced-controls -->

# Advanced Controls Export Study

Proof boundary: the schema study is export-backed learning from `/Users/Renger/Downloads/Company Overview (3).yap`. The focused generated runtime package `/Users/Renger/Downloads/advanced-controls-runtime-proof.v1.yap` was later user-confirmed for import/open/render and basic interaction checks for the included controls. Raw `.yap`, screenshots, decoded full payloads, `Resource`, `Sign`, API responses, private IDs, private URLs, sample records, generated packages, and secrets are not committed.

## Source And Scope

Repository branch: `codex/advanced-controls-learning`

Export source: `/Users/Renger/Downloads/Company Overview (3).yap`

Studied Dashboard pages: `Tab controls`, `Toggle controls`, and `Additional controls`.

Studied Data List custom form: `Company Overview` / `View page`.

Inspector added: `scripts/inspect-advanced-controls.mjs`. It inventories advanced controls, redacts IDs and URLs, summarizes attrs/style/binding modes, and emits compatibility warnings for ambiguous historical exports.

## Export Inventory

| Surface | Export-proven controls |
| --- | --- |
| Dashboard / Tab controls | 10 Tab containers and 30 Tab items |
| Dashboard / Toggle controls | 5 Toggle containers and 15 Toggle panels |
| Dashboard / Additional controls | Timer, Icon list, Divider, Alert, Progress bar, Spacer, Progress circle, Steps bar, QR Code |
| Data List / Company Overview / View page | Steps bar, QR Code, Barcode, Embed, Document embed |

Observed internal types: `aktabs`, `ak-tabs-tab`, `toggle`, `toggle-panel`, `timer`, `icon_list`, `line`, `alert`, `progress`, `gap`, `progress-circle`, `steps-bar`, `list-qrcode`, `barcode`, `embed`, and `document-embed`.

## Control Findings

### Tab

Tab usage is export-proven on the Dashboard page `Tab controls`. A Tab container uses `type = "aktabs"` and contains `ak-tabs-tab` children. Tab item titles are stored on the child `label`; active/default behavior is represented with `attrs.isDefault`, while designer-selected state appears as `attrs.isDesignDefault`. Observed container style variants include `attrs.tabs-tabposition` values for top, left, right, and bottom placement, plus `attrs.item` and `attrs.tabs` settings for icon size, typography, icon placement, justification, and alignment. Nested content can be normal containers or data-bound controls such as Collection.

Generation guidance: use Tab when a dashboard or form needs peer content groups without leaving the page, such as overview/details/history, metrics by department/region, or setup/status/activity.

Validation guidance: generated Tab controls must include stable child Tab items, each item should have a title, and each item should contain a content container or intentional nested controls.

### Toggle

Toggle usage is export-proven on the Dashboard page `Toggle controls`. A Toggle container uses `type = "toggle"` and contains `toggle-panel` children. Section titles are stored under `attrs.title.value` on each panel. Container styling is held under `attrs.caption` and `attrs.general`, including background, border radius, spacing, hover shadow, and typography. Nested content can be containers or data-bound Collection controls. No reliable expanded/collapsed runtime behavior was proven in this branch.

Generation guidance: use Toggle for collapsible detail blocks, FAQ-style sections, optional guidance, and grouped panels that should not dominate the initial viewport.

Validation guidance: generated Toggle controls must include panel children with stable ids, titles, and nested content. Default expanded/collapsed settings should be emitted only when the schema is explicit or runtime-proven.

### Timer

Timer dashboard usage is export-proven from `Additional controls`. Timer controls use `type = "timer"`; the observed configuration stores date/countdown settings under `attrs.set`, with one variant also including `attrs.gen` and `attrs.items`. Dynamic date-field behavior was not observed.

Use cases: SLA countdown, due date, deadline, campaign countdown, and task timer. Generated Timer controls need a static date setting or dynamic date binding that resolves to a date/time value.

### Icon List

Icon list dashboard usage is export-proven from `Additional controls`. Icon list controls use `type = "icon_list"` with observed keys `data`, `gen`, `icon`, and `title`. Icon style includes size and color settings under `attrs.icon`. Item internals are partly opaque in the studied export, so generation should keep icon/text/link item shape warning-first until a focused export captures all item variants.

Use cases: quick links, resource navigation, and compact shortcuts.

### Divider

Divider dashboard usage is export-proven from `Additional controls`. Divider controls use `type = "line"`. Observed settings include `line-width`, `width`, `space`, `sketchpicker`, optional `eletype`, `line-text`, and text styling. Use Divider to separate content blocks or add labeled separators.

### Alert

Alert dashboard usage is export-proven from `Additional controls`. Alert controls use `type = "alert"`. Observed variants include basic alerts plus examples whose labels/types correspond to success, warning, and error states. Settings live mainly under `attrs.alert`, with one variant also using `attrs.title` and `attrs.description`.

Use cases: guidance, warnings, validation messages, risk/status communication, and success confirmations.

### Progress Bar

Progress bar dashboard usage is export-proven from `Additional controls`. Progress bar controls use `type = "progress"` with settings such as `attrs.per`, `attrs.bar`, and `attrs.title`. This export proves static progress configuration. Variable-bound and field-bound progress values remain planning guidance unless a later export/runtime test proves the exact binding shape.

### Spacer

Spacer dashboard usage is export-proven from `Additional controls`. Spacer controls use `type = "gap"` with `attrs.space`, and one variant also uses `attrs.common`. Use Spacer for intentional layout rhythm between sections.

### Progress Circle

Progress circle dashboard usage is export-proven from `Additional controls`. Progress circle controls use `type = "progress-circle"`, with percentage/value settings under `attrs.per` and style variants under `attrs.sty` plus `attrs.common`. Absolute value/max and dynamic bindings remain planning guidance until separately proven.

### Steps Bar

Steps bar dashboard usage is export-proven from `Additional controls`; data-list custom-form usage is export-proven from `Company Overview / View page`. Steps bar controls use `type = "steps-bar"`. Static step labels live in `attrs.steps-options[]` as key/value pairs. Current/past icons use `attrs.current-icon` and `attrs.past-icon`. Layout variants use `attrs.layout`, `attrs.text-posi`, `attrs.indicator`, and `attrs.stepstyle`.

Dashboard examples include variable-bound current step patterns. Data List View page examples include current item/list field bindings via `attrs.current-step.variable[]` with `exprType = "list_field"`, plus `attrs.obj-f` pointing to the bound field. Observed field-bound examples use radio/status-like fields.

Use cases: workflow stage, project phase, approval progress, onboarding progress, implementation stage, or lifecycle status.

### QR Code

QR Code dashboard usage is export-proven from `Additional controls`; Data List View page usage is export-proven from `Company Overview / View page`. QR Code controls use `type = "list-qrcode"`. Observed style settings include `attrs.common`, `attrs.qr-code-size`, and `attrs.qr-code-link`. Some exports omit an explicit value/source, which likely represents host-current URL behavior, but current page/current item/current form URL runtime behavior is not proven here.

Use cases: mobile access, record sharing, public page links, form links, and dashboard/page sharing.

### Barcode

Barcode Data List View page usage is export-proven from `Company Overview / View page`. Barcode controls use `type = "barcode"`. Observed settings include `attrs.value`, `attrs.type`, `attrs.textPosition`, `attrs.displayValue`, `attrs.fallback`, and `attrs.barcode` style. The export includes CODE128A variants and dynamic value binding to a temp variable.

Use cases: asset tags, inventory, ticketing, record references, and scan workflows.

### Embed

Embed Data List View page usage is export-proven from `Company Overview / View page`. Embed controls use `type = "embed"` with iframe/code configuration under `attrs.code`. The normalized refs redact the URL and keep only the configuration shape. Static URL mode is export-proven; dynamic URL mode is not observed in this branch.

Use cases: external reports, maps, external dashboards, docs, and videos when iframe loading is acceptable for the target tenant/security context.

### Document Embed

Document embed Data List View page usage is export-proven from `Company Overview / View page`. Document embed controls use `type = "document-embed"`. The document source is bound through `attrs["doc-source"]` with an expression token using `exprType = "list_field"` and a file-upload-compatible value type. Appearance settings include height and unit values under `attrs.appearance`.

Use cases: contract preview, invoice review, project documents, images, presentations, PDFs, Word files, and PowerPoint files when the record has an attachment/file field.

## Host Comparison

Dashboard-observed in this export: Tab, Toggle, Timer, Icon list, Divider, Alert, Progress bar, Spacer, Progress circle, Steps bar, and QR Code.

Data List custom form-observed in this export: Steps bar, QR Code, Barcode, Embed, and Document embed.

Approval Form and Public Form support is product-understanding-backed unless separately export-proven in this branch. Do not generate host-specific runtime claims for Approval Forms or Public Forms from this export alone.

Binding patterns observed: static settings, dashboard temp variables, current item/list field bindings, implicit host URL behavior for QR Code, and iframe/static URL settings for Embed.

## Yeeflow App Builder Guidance

During application planning, consider these controls when they improve navigation, status visibility, record sharing, document review, or dashboard usability. Prefer native controls over custom code for common UI structure: Tabs for peer sections, Toggles for collapsible sections, Divider/Spacer for layout, Alert for communication, Progress controls and Steps bar for status, QR/Barcode for sharing/scanning, Embed for external content, and Document embed for attachment previews.

For generated-final packages, unresolved required bindings, invalid URLs, nonnumeric progress values, unsupported barcode types, unresolved field references, and unsupported host placements should block handoff. Historical exports should warn when the schema is ambiguous or host behavior is implicit.

## Reusable Assets Added

- Inspector: `scripts/inspect-advanced-controls.mjs`
- Normalized refs: `docs/studies/normalized/advanced-controls.bundle.json*.normalized.json`
- Validator/import-readiness updates: `validate-yap-package.js` and `scripts/inspect-yap-import-readiness.mjs`
- Skill guidance: installed repo skills, active `~/.codex/skills`, and plugin mirror skill docs are updated without rebuilding or tagging the plugin.

## Proof Boundary

- Tab control usage is export-proven from dashboard Tab controls.
- Toggle control usage is export-proven from dashboard Toggle controls.
- Timer, Icon list, Divider, Alert, Progress bar, Spacer, Progress circle, Steps bar, and QR Code dashboard usage are export-proven from dashboard Additional controls where observed.
- Steps bar, QR Code, Barcode, Embed, and Document embed data list form usage are export-proven from Company Overview / View page where observed.
- The focused generated package `/Users/Renger/Downloads/advanced-controls-runtime-proof.v1.yap` is user-confirmed runtime-proven for package import, dashboard open, rendering/basic interactions for Tab and Toggle, rendering for Timer, Icon list, Divider, Alert, Progress bar, Spacer, Progress circle, Steps bar, QR Code, Barcode, Embed safe render, Document embed empty state, and absence of missing binding/render/action errors.
- Approval Form/Public Form support is product-understanding-backed unless separately export-proven in this branch.
- QR scan behavior, Barcode scan behavior, external iframe content loading beyond safe render, non-empty document preview, dynamic value changes, and Approval Form/Public Form host behavior are not proven by the runtime package.

---

<a id="agent-access-application-resources-tool"></a>

<!-- source: docs/studies/collection-01.md#agent-access-application-resources-tool -->

# Agent Access Application Resources Tool

Source export: `<downloads>/Spark & AI (1).yap`

Classification: export-proven tool structure only.

## Tool Identity

The `Dental Aligner Label Info` Agent includes one component:

- `Components[].Type = 2`
- `Components[].SubType = 10`
- name: `Application resource access`

This is the currently proven shape for the Agent-side app-resource tool in this export.

## Tool Scope

The tool `Settings.Data.Value` points to the current application/listset, not to a single list:

- `Settings.Data.AppID = <local app id>`
- `Settings.Data.ListSetID = <local app/listset id>`
- `Settings.Data.Value = <same local app/listset id>`

Selectable resources are then constrained inside `Settings.resources`.

## Resource Configuration

Observed `Settings.resources` shape:

```json
{
  "dataLists": {
    "items": [
      { "id": "<stock-box-list-id>", "permissions": 11 },
      { "id": "<category-list-id>", "permissions": 8 }
    ]
  }
}
```

This export proves the app-resource tool can be scoped to multiple local data lists through a nested `resources.dataLists.items[]` object, not only through a flat array.

## Proven Intended Behavior

The tool description instructs the Agent to:

1. update the `Stock Box` item whose ID comes from input variable `stock_box_item_id`
2. write extracted values into target fields on that item
3. check the `Category` list
4. map the extracted category code to a `Category` row
5. use that row ID as the lookup value for `Stock Box.Category Code`
6. return mapped category details through Agent output variables

That means the update logic is described in the tool contract and prompt, not in a separate workflow-side `Update item` node.

## What Is Proven vs Inferred

Export-proven:

- tool lives in Agent `Components[]`
- component type/subtype
- current-app/listset scope in `Settings.Data.Value`
- nested `resources.dataLists.items[]`
- local data-list permissions integers
- description-driven update intent

Inferred but not execution-proven:

- exact runtime request payload sent by the tool
- exact field-by-field update call shape
- exact meaning of permission values `11` and `8`
- whether the tool performs lookup resolution internally or through model reasoning plus tool calls

## Validation Guidance

- Accept app-resource tools where `Settings.Data.Value` equals the current app/listset ID.
- Accept `Settings.resources` as an object.
- Validate `resources.dataLists.items[]` entries resolve to included lists.
- Treat unresolved list IDs as blockers in generator final mode.
- Treat live tool execution as runtime-sensitive whenever updates are possible.

---

<a id="ai-agent-image-extraction-inputs"></a>

<!-- source: docs/studies/collection-01.md#ai-agent-image-extraction-inputs -->

# AI Agent Image Extraction Inputs

Source export: `<downloads>/Spark & AI (1).yap`

Classification: export-proven Agent structure only.

## Agent

- Name: `Dental Aligner Label Info`
- Resource module: `Data.OtherModules[]` with `Type = "Agents"`
- Resource type: `Type = 0`
- Published: `true`
- Tool count: `1`

## Prompt Role

The Agent prompt is specialized for dental-aligner label extraction and explicitly tells the Agent to:

- inspect the input label image
- extract structured fields from left/right/bottom label zones
- return `N/A` instead of inventing missing values
- use the application-resource access tool to update the target `Stock Box` item

The prompt also instructs the Agent to derive a `Category Code` and resolve a matching `Category` list item for the lookup field.

## Input Variables

Observed `Settings.InputVariables[]`:

| ID | Type | Purpose |
| --- | --- | --- |
| `label_image` | `img` | input dental label image |
| `stock_box_item_id` | `text` | originating Stock Box row ID |

## Output Variables

Observed `Settings.OutputVariables[]`:

| ID | Type | Purpose |
| --- | --- | --- |
| `Category Code` | `text` | extracted category code |
| `Category ID` | `text` | mapped Category row ID |

This export does not use workflow-side `outputVariables[]`; the Agent appears to rely on its own tool side effects to update the list row.

## Expected Extracted Fields

The prompt names these extracted business values:

- Label Name
- Product_Step
- Product Name
- Doctor Name
- Produced/Registered
- Label Number
- Box Info
- Ref Number
- Lot Number
- Date
- extraction confidence
- notes

The host `Stock Box` list contains matching or adjacent target fields for many of these values.

## Image Extraction Boundaries

This branch intentionally did not run live image extraction because:

- the source export belongs to a real business app
- the list already contains records
- the Agent tool can update list items
- executing the flow would risk real-record mutation and live AI usage

The structure is therefore export-proven, not execution-proven.

---

<a id="application-planning-capability-coverage"></a>

<!-- source: docs/studies/collection-01.md#application-planning-capability-coverage -->

# Application Planning Capability Coverage

This study audits whether recent Yeeflow platform learning is visible during application planning, before package generation starts. It is a planning-coverage milestone: it does not add new package validation behavior, generate a baseline package, or claim new runtime proof.

## Planning Rule

Every full Yeeflow application build plan should include a `Capability Coverage Plan` before implementation. The plan should choose the capabilities that belong in v1, explicitly exclude capabilities that are irrelevant, mark partially proven or unproven capabilities honestly, assign each selected capability to the responsible skill, and define validation plus runtime-test boundaries.

The planning phase should avoid both under-planning and feature stuffing. A capability is considered when it is relevant to the business process, not automatically included in every app.

For newly learned capabilities, also apply `docs/studies/collection-02.md#new-capability-runtime-gate-process`. Planning may select export-proven or validator-backed capabilities, but it must not describe them as runtime-proven unless a focused runtime baseline passed for the selected host/scope.

## Coverage Matrix

| Capability | Learned status | Source docs or skill location | Consider during app planning | Planning questions or decision criteria | Implementation skill responsible | Validation/runtime test requirements | Safety boundary |
|---|---|---|---|---|---|---|---|
| Data lists | runtime-proven | `yeeflow-application-builder`, `yeeflow-application-generator`, data-list generator docs | yes | What business records, master data, transaction lists, lookup lists, and follow-up/task lists are needed? | `yeeflow-data-list-generator`, `yeeflow-application-generator` | Validate child lists, app graph, field IDs, lookup targets, sample rows, custom forms, materialization. Runtime-test list open and data visibility. | Do not create placeholder lists without v1 purpose. Preserve tenant/private data boundaries. |
| Approval forms | runtime-proven | `yeeflow-approval-form-generator`, approval form control and design docs | yes | Are approvals, submissions, task pages, workflow panels, or controlled status changes needed? | `yeeflow-approval-form-generator`, `yeeflow-application-generator` | Validate form Def, workflow, ContentList targets, task assignment, publish/open/submit/review when scoped. | Do not hardcode tenant-specific users; require safe assignment strategy. |
| Dashboards | runtime-proven for proven controls; partial for some chart/report shapes | `yeeflow-dashboard-generator`, dashboard UI/UX docs | yes | Which operational pages, queues, KPIs, summaries, and drill-downs support the app's v1 workflow? | `yeeflow-dashboard-generator`, `yeeflow-application-generator` | Validate Type `103` ownership, layout JSON, data bindings, ReportIds/exts. Runtime-test render and data-bound values or empty states. | Do not count static KPI/mockup text as runtime proof. |
| Document libraries | runtime-proven for Type `16`, multiple libraries, root folders, simple custom fields/views | `docs/studies/document-library-*`, `yeeflow-application-generator` | yes when document management is relevant | Are file repositories, generated root folders, custom document metadata, views, or document custom forms needed? | `yeeflow-application-generator`, document-library rules in validator | Validate `ListModel.Type = 16`, default fields, Type `0` views, folder rows, `SimplePortal = null`, ReplaceIds. Runtime-test library open/folder render. | Do not fake document uploads or commit file payloads/private documents. |
| Doc library controls | runtime-proven on dashboards for root/static folder display; document-library custom-form host runtime-proven; approval/data-list hosts partial | `docs/studies/doc-library-control-*`, `yeeflow-dashboard-generator` | yes when dashboards or forms must expose library files | Should users see library contents from dashboards or forms? Should the control be root-bound or folder-bound? Are caption/search/add needed? | `yeeflow-dashboard-generator`, relevant form generator | Validate `type = "document-library"`, target Type `16` library, fields, folder path, caption layout. Runtime-test per host. | Do not claim approval-form live or data-list custom-form runtime proof until focused tests pass. |
| Custom forms | runtime-proven for standard data-list custom forms and many approval controls; partial for advanced hosts | Data-list, approval-form, custom-form docs | yes | Which resources need New/Edit/View forms, request pages, task pages, or document custom forms? | `yeeflow-data-list-generator`, `yeeflow-approval-form-generator`, `yeeflow-application-generator` | Validate layout/control bindings, field mappings, action rules, publish/open behavior. | Avoid unsupported controls or mark as required proof/deferred. |
| Data list workflows | import-proven and designer-shape learned; runtime execution remains scoped | `docs/studies/data-list-workflow-*`, app generator | yes when list events should automate work | Which list trigger is needed: new item, field/status change, or manual? Is `Setting.NewTrigger = true` required? | `yeeflow-application-generator`, workflow action docs | Validate `FlowMappings[]`, `DefResource` designer metadata, trigger settings, action references. Runtime-test open/designer first. | Do not run row mutation or AI execution without safe sandbox scope. |
| Approval workflows | runtime-proven for standard app approval flows | Approval-form generator docs, workflow action references | yes | What business stages, branches, assignments, ContentList writes, and audit outputs are required? | `yeeflow-approval-form-generator`, `yeeflow-expression-generator` | Validate workflow graph, actions, expressions, assignments, ContentList mappings. Runtime-test submit/review branches. | Avoid tenant-specific user assignment unless explicitly mapped and safe. |
| Scheduled workflows | import/open/designer runtime-proven for safe baseline; execution scope partially documented | `docs/studies/scheduled-workflow-*`, app generator | yes for timed automation | Is recurrence needed? Weekly or every X working days? Timezone? Should QueryData, AI Assistant, or Send email run? | `yeeflow-application-generator`, package validator | Validate `WorkflowType = 3`, schedule `Settings`, variables, `QueryData`, `AI`, `MailTask`. Runtime-test import/open/designer/config only unless execution is explicitly safe. | Do not trigger schedules, send email, or call live AI unless safe recipient/data/scope is approved. |
| QueryData workflow action | export-proven; used in scheduled workflow baseline | `docs/studies/collection-03.md#workflow-query-data-action` | yes when workflows need list reads | Which list is queried? What filters and output variables are needed? | App generator, expression generator | Validate target list references, filters, output variables, workflow graph. Runtime-test configuration; execution only with safe data. | Avoid querying tenant/private data in tests. |
| AI Assistant workflow action | export-proven in scheduled and data-list workflows; execution runtime-sensitive | `docs/studies/workflow-ai-assistant-*`, app generator | yes when AI should summarize, classify, score, draft, or recommend | Which Agent is called? What input/output variables are mapped? Are image/file inputs needed? | App generator, AI Agent skills | Validate Agent reference, variable maps, image/file value types, output variables. Runtime-test non-executing config first. | Do not execute live AI or mutate rows unless sandbox scope is approved. |
| Send email workflow action | export-proven; safe baseline uses reserved test recipient | `docs/studies/collection-03.md#workflow-send-email-action` | yes when notifications or drafts are required | Is email send required, or should app generate drafts/review tasks? Who receives mail? Are subject/body expression-based? | App generator, expression generator | Validate `MailTask`, recipient, subject/body, variables. Runtime-test config; send only in safe scope. | Never generate real recipients by default; no accidental email execution. |
| AI Agents | import-proven as app-contained resources; workflow invocation export-proven; runtime execution scoped | `docs/studies/ai-agent-*`, generated Agent skills, app generator | yes when reusable AI work is needed | Which reusable Agents are needed? What inputs/outputs/tools? Should they read/update app resources? | AI Agent template/operator skills, app generator | Validate OtherModules, `Publisher: 0`, settings JSON, components/tools, input variables, local resource access. Runtime-test config visibility before execution. | Keep generated IDs within `System.Int64`; no secrets, real credentials, or destructive tool calls. |
| Copilots | import-proven as app-contained Type `1`; chat/runtime behavior not fully proven for generated apps | `docs/studies/copilot-*`, generated Copilot skills | yes when conversational UX is part of app | Does the app need a Copilot? Quick prompts? Local tools? Connected Agents? Image uploads or attachments? | Copilot template/operator skills, app generator | Validate OtherModules, suggestions, components, tools, `Publisher: 0`, local references. Runtime-test config and safe chat only when scoped. | Do not overclaim generated Copilot runtime until smoke tested. |
| Access application resources tool | export-proven/import-learned for local list access | `docs/studies/collection-01.md#agent-access-application-resources-tool`, app generator | yes when AI must read/update app lists | Which lists can the AI access? Read only or update/create? Is same-row update needed through item ID input? | App generator, AI Agent/Copilot skills | Validate compact `{ id, permissions }` entries and list references. Runtime-test read/update only in disposable sandbox. | Use least privilege; avoid destructive update/delete by default. |
| Image/file input extraction | export-proven for Agent workflow mapping; runtime execution sensitive | `docs/studies/collection-01.md#ai-agent-image-extraction-inputs`, `docs/studies/collection-02.md#workflow-ai-assistant-image-input` | yes when images/files drive AI decisions | Which fields hold images/files? Should values use `icon-upload` or `file-upload`? What is extracted? | App generator, AI Agent skills | Validate input variable type `img`, field mappings, Agent prompts/tools. Runtime-test with harmless files only. | Do not upload private images or execute real analysis without approval. |
| Copilot image upload scenario | partial/import-planning only unless focused runtime proof exists | Copilot app-resource docs and runtime plans | yes when user-facing Copilot should accept images | Is image upload central to user value or should it be deferred? Which Agent/tool handles it? | Copilot skills, AI Agent skills, app generator | Validate Copilot/Agent dependency graph; runtime-test only with harmless sandbox files. | Mark as partial until focused generated runtime proof exists. |
| Application navigation menu | export-proven | `docs/studies/collection-01.md#application-settings-navigation-menu`, app generator | yes | Should navigation be default or custom? Which resources are top-level? Which are grouped? Are custom labels/icons needed? | `yeeflow-application-generator`, `yeeflow-application-builder` | Validate root `LayoutView.sort[]`, references, depth, group labels, icons/no-icon. Runtime-test menu render. | Groups are top-level only; no nested groups. |
| Navigation groups | export-proven | Application settings docs | yes | Are business-facing groups clearer than raw resource list? What child resources belong together? | App generator | Validate `Type: "classes"` with `list[]`, max depth 2, required `Title`. Runtime-test group/child render. | Do not nest groups. |
| Navigation layout | export-proven | `docs/studies/collection-01.md#application-settings-navigation-layout` | yes | Use `default`, `left`, `onheader`, or `none`? Does the menu fit in header? | App generator | Validate `attrs["navigator-menu"].position`; runtime-test selected layout. | Use only proven values. |
| Header height/title visibility | export-proven for `height: 46` and `hideTitle: true`; default 56 is product-known/export-observed by absence | `docs/studies/collection-01.md#application-settings-header` | yes when shell appearance matters | Should app title show? Is small header needed? Are additional heights required? | App generator | Validate `attrs.appearance`, boolean `hideTitle`, header height type. Runtime-test title/header render. | Do not overclaim unobserved height variants. |
| User groups | export-proven for group records; member assignment unproven | `docs/studies/collection-01.md#application-user-groups` | yes for role/permission planning | Are app user groups needed for permissions, pickers, or future access control? Are members needed? | App generator, builder | Validate `Data.AppGroups[]`, `{ ID, Name, Description }`, ReplaceIds. Runtime-test groups page only with placeholder groups. | Do not generate members, real users, emails, or tenant identities until proven. |
| Custom code controls/actions | runtime-proven only by specific scripts/hosts | Custom code generator skill, app generator | yes for non-native interactions | Can native controls/actions solve it? Which host needs custom code? What parameters/read/write behavior? | `yeeflow-custom-code-generator`, app/form/dashboard generators | Validate script presence, params, host bindings, custom-code inspection. Runtime-test render/query/writeback. | Avoid custom code for simple native behavior; no unsupported runtime APIs. |
| Connections / HTTP API / OAuth | export-proven as sensitive dependencies; generation remains credential-sensitive | AI Agent/Copilot and connection docs | yes for integration-heavy apps | Is external connectivity required in v1? Can it be a post-import configuration step? Are credentials available safely? | App generator, Agent/Copilot skills, feature learning when unknown | Validate references and redaction; runtime-test only with safe credentials/sandbox. | Never commit credentials, tokens, OAuth secrets, or tenant connection IDs. |
| OpenAPI/REST tools | partial/export-known, credential-sensitive | Agent/Copilot tool docs | yes when external APIs are part of AI/tool design | Is OpenAPI tool required? Is schema known? What operations are safe? | AI Agent/Copilot skills | Validate tool schema/references; runtime-test non-destructive operations only. | Defer execution unless credentials and endpoints are safe. |
| Safe runtime testing and deferred execution | runtime-process proven | `yeeflow-runtime-test-orchestrator`, runtime checklist docs | yes | What should be import-proven, runtime-proven, render-only, blocked, or not tested? Which actions must not execute? | Runtime test orchestrator, package validator | Validate before build/runtime; document scope and labels. | Do not run AI/email/external/destructive actions without explicit safe scope. |

## Capability Coverage Plan Template

Every full app plan should include:

```text
Capability Coverage Plan

Selected capabilities:
- <capability>: <why it is needed in v1, responsible skill, proof target>

Intentionally excluded capabilities:
- <capability>: <why it is not relevant or is better deferred>

Deferred or unproven capabilities:
- <capability>: <proof gap, fallback, required learning/runtime test>

Runtime test boundary:
- Import/open:
- Resource rendering:
- Workflow/AI/email/external execution:
- Deferred execution:

Safety boundary:
- Private data:
- Credentials/tokens:
- Generated packages/artifacts:
- Human review requirements:

Skill and validator dependencies:
- <skill/validator/doc>: <use>
```

## Planning Self-Check

Before generating a package, Codex should confirm:

- the plan considered the current learned capability set without forcing irrelevant features into the app
- every selected capability has a responsible specialized skill or documented generator rule
- unproven, partial, or runtime-sensitive capabilities are marked honestly
- validation commands and runtime proof boundaries are named before build/runtime test
- no generated plan depends on real users, tenant IDs, emails, credentials, tokens, private documents, image binaries, raw exports, decoded payloads, or generated package artifacts being committed

---

<a id="application-settings-header"></a>

<!-- source: docs/studies/collection-01.md#application-settings-header -->

# Yeeflow Application Settings: Header

## Scope

This study inspects header settings across:

- `<downloads>/Tenant Service Portal v2.yap`
- `<downloads>/Tenant Service Portal v3.yap`
- `<downloads>/Tenant Service Portal v4.yap`
- `<downloads>/Tenant Service Portal v5.yap`
- `<downloads>/Tenant Service Portal v6.yap`

## Export-Proven Location

Application header appearance is stored in:

```text
Data.Item.ListModel.LayoutView -> attrs.appearance
```

## Export-Proven Fields

v2-v5:

```json
{
  "bgc": "var(--c--primary-light)",
  "color": "var(--c--primary)"
}
```

v6:

```json
{
  "bgc": "var(--c--primary-light)",
  "color": "var(--c--primary)",
  "height": 46,
  "hideTitle": true
}
```

## Findings

- Default 56 px header height is represented by omission in v2-v5 and by the product UI default.
- Small header height is export-proven as `height: 46`.
- Larger header height was not present in the five exports; it remains product-known but not export-proven here.
- Title visibility off is export-proven as `hideTitle: true`.
- Title visibility on/default is represented by omission in v2-v5.
- Header color fields are `bgc` and `color`.
- `onheader` navigation layout did not require a different header field shape in v4.

## Validator Rules Added

- `appearance.height`, when present, must be a positive number.
- `46` and omitted/default 56 px are documented as export-proven.
- unstudied positive heights warn rather than hard-fail.
- `appearance.hideTitle`, when present, must be boolean.

---

<a id="application-settings-navigation-layout"></a>

<!-- source: docs/studies/collection-01.md#application-settings-navigation-layout -->

# Yeeflow Application Settings: Navigation Layout

## Scope

This study compares:

- `<downloads>/Tenant Service Portal v2.yap`
- `<downloads>/Tenant Service Portal v3.yap`
- `<downloads>/Tenant Service Portal v4.yap`
- `<downloads>/Tenant Service Portal v5.yap`

## Export-Proven Field

Navigation menu layout is stored in the parsed root layout JSON:

```text
Data.Item.ListModel.LayoutView -> attrs["navigator-menu"].position
```

## Comparison Table

| Export | Expected layout | Setting field | Stored value | Additional related settings | Generator rule |
| --- | --- | --- | --- | --- | --- |
| v2 | Horizontal/default | `attrs["navigator-menu"].position` | `default` | `bgc`, `color` | Use `default` for horizontal navigation under the header. |
| v3 | Vertical | `attrs["navigator-menu"].position` | `left` | `bgc`, `color` | Use `left` for left-panel vertical navigation. |
| v4 | On header | `attrs["navigator-menu"].position` | `onheader` | `bgc`, `color` | Use `onheader` when the menu should sit inside the header. |
| v5 | None | `attrs["navigator-menu"].position` | `none` | `bgc`, `color` | Use `none` when no navigation menu should render. |

## Default Behavior

The studied "default/horizontal" export stores `position: "default"` explicitly. Missing `position` was not proven in this export set, so generators should write `default` rather than relying on omission.

## Validator Rules Added

Allowed export-proven values:

- `default`
- `left`
- `onheader`
- `none`

Unknown values are hard errors for generated final packages and warnings in less certain contexts.

---

<a id="application-settings-navigation-menu"></a>

<!-- source: docs/studies/collection-01.md#application-settings-navigation-menu -->

# Yeeflow Application Settings: Navigation Menu

## Scope

This study uses these read-only exports:

- `<downloads>/Tenant Service Portal v2.yap`
- `<downloads>/Tenant Service Portal v6.yap`

Raw exports and decoded payloads stayed outside git under ignored `tmp/application-settings-study/`.

## Export-Proven Location

Application navigation is stored on the root application/listset:

- `Data.Item.ListModel.LayoutView` is a JSON string.
- Parsed `LayoutView.sort[]` stores navigation menu items.
- Parsed `LayoutView.attrs["navigator-menu"]` stores navigation appearance and layout mode.
- Parsed `LayoutView.sortVer` was `1` in all five studied exports.

The root application shell remains:

- `Data.Item.ListModel.Type = 1024`
- child data-list resources in `Data.Childs[]`
- dashboard/page resources in `Data.Item.Layouts[]` with `Type = 103`
- approval forms/workflows in `Data.Forms[]` with `WorkflowType = 2` and `ListID = 0`

## Menu Item Shapes

### Top-Level Resource

Export-proven single resource item shape:

```json
{
  "AppID": 41,
  "ListID": "<RESOURCE_ID>",
  "ListSetID": "<APP_LISTSET_ID>",
  "Type": 103,
  "Title": "Tenant Home",
  "Icon": "fa-regular fa-grid-2",
  "DisplayName": "Tenant Home"
}
```

For data lists, `Type` is `1`; for approval forms, `Type` is `105`. Dashboard/page resources use `Type = 103` and reference `Data.Item.Layouts[].LayoutID`.

### Top-Level Group

Export-proven group shape:

```json
{
  "ID": "<GROUP_ID>",
  "AppID": 41,
  "ListSetID": "<APP_LISTSET_ID>",
  "Type": "classes",
  "Title": "Workbenches",
  "Icon": "fa-regular fa-layer-plus",
  "list": []
}
```

Group rules:

- `Type = "classes"` identifies a custom group.
- `Title` is the group display text and should always be present.
- `ID` is a group-local/menu ID.
- Child items are stored in `list[]`.
- No nested group was present; product rule is maximum depth 2.

### Child Resource Inside Group

Child resources inside a group reuse the same resource item shape:

```json
{
  "AppID": 41,
  "ListID": "<RESOURCE_ID>",
  "ListSetID": "<APP_LISTSET_ID>",
  "Type": 1,
  "IsHidden": false,
  "Title": "Lease Profiles",
  "Icon": "fa-regular fa-table-list"
}
```

## Display Text

Export-proven behavior:

- `DisplayName` stores custom navigation text when configured.
- Resource items may omit `DisplayName`; Yeeflow falls back to the resource `Title`.
- Groups use `Title` as required display text.
- `DisplayName: ""` was observed on hidden process menu items, not on generated app resource items.

## Icons And No-Icon

Export-proven behavior:

- Icon-enabled items use `Icon` with a string such as `fa-regular fa-grid-2`, `fa-regular fa-table-list`, or `fa-regular fa-pen-field`.
- No-icon items in v6 use `Icon: ""`.
- `null` icon was not export-proven for navigation items and should be treated as runtime-sensitive.

## Visibility And Ordering

Export-proven behavior:

- Ordering is array order in `LayoutView.sort[]` and child `list[]`.
- `IsHidden: false` marks visible menu items.
- `IsHidden: true` marks hidden resources that remain in the root sort array.
- Group items in the studied export did not use `IsHidden`.

## Observed Non-Goal Menu Types

The v2/v6 exports also include:

- process/task shortcuts: `Type = "process"` with `Path` such as `/p/claim`
- custom link: `Type = "link"` with `DisplayName` and `url`

These are observed but not promoted as generated application-resource menu rules in this branch.

## Validator Rules Added

- `LayoutView` must parse as JSON.
- navigation groups use `Type = "classes"`.
- group title/display text is required.
- groups cannot be nested.
- only groups should contain `list[]`.
- depth greater than two is a hard error for generated final packages.
- resource items may omit `DisplayName`.
- missing resource custom text falls back to `Title`.
- `Icon: ""` is accepted as the export-proven no-icon shape.
- invalid layout position values are hard errors for generated final packages.

---

<a id="application-settings-runtime-test-plan"></a>

<!-- source: docs/studies/collection-01.md#application-settings-runtime-test-plan -->

# Application Settings Runtime Test Plan

## Local Gate

Runtime testing must not start until all local gates pass:

- syntax checks for modified JS/MJS scripts
- JSON parse checks for normalized references
- `validate-yap-package` on any generated baseline
- `validate-yap-graph` on any generated baseline
- app settings navigation/header/user-group validation
- resource reference validation
- `ReplaceIds` scope checks
- wrapper round trip if a `.yap` is generated
- `git diff --check`
- safety scan for raw exports, decoded payloads, real users, emails, secrets, and tokens

## Safe Baseline Scope

Suggested package name:

- `application-settings-navigation-baseline.v1.yap`

Safe baseline should use:

- fresh IDs
- no private users
- no real emails
- no credentials
- no external connections
- empty placeholder app user groups only if generated safely

## Runtime Cases

Test at `https://<yourdomain>.yeeflow.com/` only after local validation passes:

- package imports
- app opens
- horizontal/default menu renders
- vertical menu renders, if variant generated
- on-header menu renders, if variant generated
- none layout hides the menu, if variant generated
- custom display text appears
- resource-name fallback appears when `DisplayName` is omitted
- icons render where configured
- no-icon items render without an icon where `Icon: ""`
- group menu appears
- group children appear
- no nested group exists
- header default height renders
- export-proven small header height renders if generated
- title hidden state works if generated
- user groups page opens if empty placeholder groups are generated

## Stop Conditions

Stop runtime testing if:

- generated validation fails on unknown settings structure
- package would expose real tenant/user data
- app groups require real members
- user/member values cannot be safely redacted
- import/runtime behavior suggests settings are tenant-bound or user-bound

---

<a id="application-user-groups"></a>

<!-- source: docs/studies/collection-01.md#application-user-groups -->

# Yeeflow Application User Groups

## Scope

This study uses:

- `<downloads>/Tenant Service Portal v6.yap`

The export includes two application user groups named `User Group 1` and `User Group 2`.

## Export-Proven Location

Application user groups are stored in:

```text
Data.AppGroups[]
```

Export-proven group shape:

```json
{
  "ID": "<GROUP_ID>",
  "Name": "User Group 1",
  "Description": null
}
```

## ReplaceIds Behavior

v6 has two more `Resource.ReplaceIds[]` entries than v2-v5, matching the two `Data.AppGroups[]` IDs. This indicates generated app group IDs should be included in `ReplaceIds` for new `.yap` import packages.

## Member Structure

The v6 `.yap` export did not include group member lists, member IDs, emails, or user display values in `Data.AppGroups[]`.

Export-proven:

- group ID
- group name
- nullable description
- group IDs participate in `ReplaceIds`

Not export-proven:

- user/member storage location
- member ID field names
- member display/email fields
- group permission references
- user picker references to app groups
- whether group IDs are app-local after import or tenant-global after runtime materialization

## Redaction Policy

Generated or documented references must not include:

- tenant IDs
- real user IDs
- email addresses
- personal names
- credentials, tokens, secrets, or private identifiers

Use placeholders such as:

- `<GROUP_ID>`
- `<REDACTED_USER_ID>`
- `<REDACTED_USER_EMAIL>`
- `<REDACTED_TENANT_ID>`

## Generator Recommendations

- It is safe to generate empty application user groups using `Data.AppGroups[]` with fresh IDs, `Name`, and `Description`.
- Include generated group IDs in `Resource.ReplaceIds[]`.
- Do not generate member assignments until a member-bearing export is studied.
- Do not embed real users, emails, or tenant-local identities in generated packages.

## Validator Recommendations

- validate `Data.AppGroups[]` as an array.
- validate each group has `ID` and `Name`.
- warn if group IDs are not in `ReplaceIds`.
- hard-fail generated packages that embed email-like values in `AppGroups`.
- warn on any member/user-looking fields because member schema is not yet export-proven.

## Runtime-Test Limitation

Runtime import of placeholder user groups may be safe only if groups are empty. Any test involving members should stop until a safe, redacted member-bearing export proves the schema.

---

<a id="container-button-action-runtime-proof"></a>

<!-- source: docs/studies/collection-01.md#container-button-action-runtime-proof -->

# Container/Button Action Runtime Proof

## Summary

This focused runtime proof validates a small generated package for representative Dashboard Container/Button action navigation. It builds on the export-learning from `AP Approval Demo v3.yap`, but uses a synthetic package instead of the source export.

- Branch: `codex/container-button-action-runtime-proof`
- Package: `container-button-action-runtime-proof.v1.yap`
- Downloads copy: `<downloads>/container-button-action-runtime-proof.v1.yap`
- App: `Container Button Action Runtime Proof`
- Dashboard: `Action Runtime Dashboard`
- Target dashboard: `Action Target Dashboard`
- Data list target: `Action Runtime Requests`
- Approval form target: `Action Runtime Approval`

## Generated Actions

The package contains representative Container/Button action settings:

| Action | Target | Open behavior | Proof result |
| --- | --- | --- | --- |
| Link | Safe `about:blank` URL | Default link behavior | User-confirmed package test passed |
| Add list item | `Action Runtime Requests` | Modal/pop-up | User-confirmed package test passed |
| Add list item | `Action Runtime Requests` | Slide-in | User-confirmed package test passed |
| Open dashboard | `Action Target Dashboard` | Full-page/target | User-confirmed package test passed |
| Open dashboard | `Action Target Dashboard` | Modal/pop-up | User-confirmed package test passed |
| Open approval form | `Action Runtime Approval` | Slide-in | User-confirmed package test passed |

The Open dashboard actions include concrete `PageID` values. Add list item and Open approval form actions reference included package resources rather than private links.

## Local Validation

The regenerated package passed local validation with zero errors:

- Container/Button action inspector: `pass`, 8 controls, 0 findings
- Package validator: `pass_with_warnings`, 0 errors
- Graph validator: `pass_with_warnings`, 0 errors
- Materialization inspector: `pass`, 0 errors, 0 warnings
- Aggregate import-readiness: `pass_with_warnings`, 0 errors
- Wrapper/package copies matched by SHA-256

Warnings were limited to existing runtime-sensitive schema/design-system cautions and optional inspector warnings, not unresolved action targets.

## Runtime Result

Manual Yeeflow testing was performed with the regenerated package. The user confirmed:

- the previous approval form publish issue was fixed
- the generated `.yap` worked
- the focused test passed

This proves representative import/open/navigation behavior for the generated package only. It does not prove every action variant or every business scenario.

## Approval Form `pageUrl` Fix

The first generated package exposed this publish error for the approval form target:

```text
process request pageUrl is null key:CBAR
```

The fixed package regenerated the approval form target with a complete request-page shape:

- `DefResource.pageurls[]` entry has outer `type = 1`
- `DefResource.pageurls[]` entry has outer `pagetype = 1`
- embedded `formdef.id` equals the page ID
- embedded `formdef.pagetype = 1`
- embedded `formdef.name` and `formdef.title` are populated
- embedded `formdef.filterVars` and `formdef.tempVars` are arrays
- Start node properties mirror the request page across `taskurl`, `taskUrl`, and `TaskUrl`

This is a generated-package readiness rule for approval forms opened from dashboard actions. The fix is user-confirmed for this package. Workflow execution, approval routing, and submission behavior remain out of scope.

## Import-Readiness Hardening

Aggregate import-readiness now treats the Pivot Table inspector's default missing `Dashboard` page finding as optional when the package is not a Pivot Table package. Container/Button action validation still runs and still fails generated-final packages with unresolved action targets.

This prevents unrelated packages from failing import-readiness solely because they do not have a literal dashboard named `Dashboard`, while preserving hard checks for Container/Button action targets.

## Not Observed

The successful manual test did not show:

- missing `PageID` or target ID errors
- missing action target errors
- approval form `pageUrl is null` publish error after regeneration
- dashboard render crash

## Proof Boundary

- AP Approval Demo v3 Container/Button action schema remains export-proven.
- This generated package is user-confirmed for representative import/open/navigation of Link, Add list item, Open dashboard, and Open approval form actions.
- Modal/pop-up, slide-in, and full-page/target behaviors are covered only as representative generated-package runtime proof.
- Save, submit, workflow execution, approval routing, data mutation, permissions, security behavior, cross-app targets, form-action binding, external sensitive navigation, and all open-mode/size combinations are not proven.

---

<a id="container-button-action-settings"></a>

<!-- source: docs/studies/collection-01.md#container-button-action-settings -->

# Container and Button Action Settings

Source export path: `<downloads>/AP Approval Demo v3.yap`

Target dashboard studied: `AP Approval Dashboard`

Proof labels:

- Export-proven: decoded `.yap` schema from the target dashboard.
- UI-reference-backed: screenshot showing the Builder Execute type menu.
- Product/user-understanding-backed: the product intent and business-selection guidance supplied with the task.
- Validator-backed: checks added in this branch.
- Runtime-proven: not claimed in this pass.

## Summary

Container and Button controls can be actionable dashboard controls. In the target export, the Button control serializes as `type = "action_button"`, while cards/blocks serialize as `type = "container"`. Both use the same dashboard Action settings model under `attrs`.

The export proves these action type codes on Container and Button-like controls:

| Action type code | Action name | Export field pattern | Proof |
| --- | --- | --- | --- |
| `2` | Link | `attrs["action-type"] = "2"` plus `attrs.link` | export-proven |
| `5` | Add list item | `attrs["action-type"] = "5"` plus `attrs.data.list` | export-proven |
| `6` | Open dashboard | `attrs["action-type"] = "6"` plus `attrs.data.page` | export-proven |
| `8` | Open approval form | `attrs["action-type"] = "8"` plus `attrs.data.form` | export-proven |
| `1` | Action / form action binding | UI menu and existing form-action knowledge; not found in this dashboard export | UI-reference-backed/product-backed, not export-proven here |

The screenshot shows the Execute type options `Action`, `Link`, `Add list item`, `Open dashboard`, and `Open approval form`; the screenshot is visual/UI reference only.

## Dashboard Inventory

Inspector command:

```text
node scripts/inspect-container-button-actions.mjs "<downloads>/AP Approval Demo v3.yap" --page "AP Approval Dashboard"
```

Inventory result:

| Page | Controls | Container actions | Button actions | Action types found | Target resolution | Proof |
| --- | ---: | ---: | ---: | --- | --- | --- |
| `AP Approval Dashboard` | 56 | 40 | 16 | Link, Add list item, Open dashboard, Open approval form | 55 resolve locally; 1 Open dashboard sample omits `PageID` | export-proven |

Action counts:

| Action type | Count | Container count | Button count | Target type |
| --- | ---: | ---: | ---: | --- |
| Link | 2 | 1 | 1 | URL/expression URL |
| Add list item | 24 | 15 | 9 | Data list |
| Open dashboard | 14 | 10 | 4 | Type `103` dashboard page |
| Open approval form | 16 | 14 | 2 | Approval form `ProcKey` |

Open behavior counts:

| Open mode label | Export `op` value | Count | Notes |
| --- | --- | ---: | --- |
| Default | omitted / empty | 17 | Export leaves `op` unset on several actions. |
| Pop-up window | `modal` | 21 | Often combined with `modalsize`. |
| Slide in | `slide` | 7 | Found on Open dashboard and Open approval form. |
| Full page | `target` | 5 | Found on Add list item, Open dashboard, and Open approval form. |
| New window | `new` | 6 | Found on Add list item, Open dashboard, and Open approval form. |

Size settings found:

| Size field | Values found | Notes |
| --- | --- | --- |
| `attrs.modalsize` | `0`, `1`, `2`, `3`, `9` | Export-proven size enum values. |
| `attrs.cusize` | `{ "w": number }`, `{ "w": number, "wu": "vw" }` | Used with custom size, especially `modalsize = 9`. |

Target scope:

- Current-application targets are export-proven through matching `ListSetID` / `ListID` / `PageID` / `ProcKey` values inside the same package.
- Cross-application targets were not found in this export. Treat cross-app schema as product-understanding-backed until a focused export proves the exact serialized identity requirements.
- The target dashboard contains one Open dashboard sample with `attrs.data.page` missing `PageID`; this is documented as an export finding and promoted to a generated-final hard error.

## Schema Patterns

### Shared Control Shape

The actionable control is a dashboard child node:

```json
{
  "type": "container",
  "attrs": {
    "action-type": "5"
  }
}
```

Button actions use the same `attrs` action block while the control type is `action_button`:

```json
{
  "type": "action_button",
  "attrs": {
    "button-style": "2",
    "action-type": "5"
  }
}
```

Generation rule: treat `container`, `button`, and `action_button` as sharing the same Action settings model unless a future export proves a difference.

### Action / Form Action Binding

The Builder menu includes `Action` as an Execute type, and existing form-action learning uses action-button references to front-end form actions. This target dashboard did not include action type `1`.

Validation rule: generated Action/form-action bindings should include an action reference such as `control_action`, `control-action`, or the export-proven field name from the host being generated. Because this dashboard did not export action type `1`, unknown dashboard-specific variants should warn first in compatibility mode.

### Link

Export pattern:

```json
{
  "action-type": "2",
  "link": {
    "opentype": true,
    "url": null,
    "variable": "[expression array]"
  }
}
```

The sample uses a variable expression for `Application:Application URL`; no literal external URL is preserved in normalized refs.

Generation guidance:

- Use Link for external documentation, help pages, websites, internal URLs, and direct deep links.
- Prefer structural Yeeflow actions instead of raw links when the target is a known Yeeflow resource in the package.
- Redact literal URLs in docs and normalized refs.

Validation rules:

- Link actions need `attrs.link`.
- The link should contain either a literal URL or a URL expression variable.
- Generated-final validation should fail missing Link targets.

### Add List Item

Export pattern:

```json
{
  "action-type": "5",
  "data": {
    "list": {
      "AppID": "[AppID]",
      "ListSetID": "[ListSetID]",
      "ListID": "[ListID]"
    }
  },
  "layout": "[LayoutID]",
  "passvalues": [
    {
      "Name": "Text1",
      "Value": "[expression array]"
    }
  ],
  "queryParams": [
    {
      "name": "KeyInfo",
      "value": "[expression]"
    }
  ],
  "op": "modal",
  "modalsize": 2
}
```

Generation guidance:

- Use Add list item for quick-create experiences such as Add task, Create request, Upload document, Add vendor, or New invoice.
- Target a Data List or Document Library structurally with `attrs.data.list`.
- Include a target form `layout` when the desired create/edit form is known.
- Use `passvalues` for safe defaults and `queryParams` for contextual parameters when the scenario requires them.
- Validate target list/library, layout, and passvalue field references before handoff.

### Open Dashboard

Export pattern:

```json
{
  "action-type": "6",
  "data": {
    "page": {
      "AppID": "[AppID]",
      "ListSetID": "[ListSetID]",
      "PageID": "[PageID]"
    }
  },
  "op": "target"
}
```

Generation guidance:

- Use Open dashboard for navigation, drill-down pages, reports, team pages, analytics, and operational workspaces.
- Prefer `Open dashboard` over Link for included Yeeflow dashboard pages.
- Validate that `PageID` resolves to a Type `103` dashboard layout.

Export finding: one sample Open dashboard action omits `PageID`. Compatibility inspection warns; generated-final validation treats this as an error.

### Open Approval Form

Export pattern:

```json
{
  "action-type": "8",
  "data": {
    "form": {
      "AppID": "[AppID]",
      "ListSetID": "[ListSetID]",
      "ProcKey": "[ProcKey]"
    }
  },
  "setVars": "[variable initialization rules]",
  "op": "modal",
  "modalsize": 2
}
```

Generation guidance:

- Use Open approval form to start workflow requests such as AP approval, invoice/payment request, purchase request, leave request, or budget approval.
- Target the approval form structurally with `ProcKey`.
- Use `setVars` only when defaults are required and every variable id resolves to the target approval form definition.
- Validate that the approval form exists, is intended to open, and is published when the generated package is final.

## Open Behavior

The export-proven open behavior field is `attrs.op`.

| UI behavior | Export value | Applies to |
| --- | --- | --- |
| Default | omitted / empty | Link, Add list item, Open dashboard |
| Pop-up window | `modal` | Add list item, Open dashboard, Open approval form |
| Slide in | `slide` | Open dashboard, Open approval form |
| Full page | `target` | Add list item, Open dashboard, Open approval form |
| New window | `new` | Add list item, Open dashboard, Open approval form |

`attrs.modalsize` and `attrs.cusize` customize the opened surface. The export proves `modalsize` values `0`, `1`, `2`, `3`, and `9`; `9` is used with custom width shapes.

Validation rules:

- Generated actions must use a known `op` value.
- Generated custom size must use an object shape when present.
- Generated actions that open resources must resolve target identity inside the package unless intentionally and explicitly cross-app.

## Business Selection Guidance

Use Link when the destination is a URL: external docs, help, website, or a deliberately raw deep link.

Use Add list item when the user intent is quick-create: Create request, Add task, Upload document, Add vendor, New invoice, or similar list/library item creation.

Use Open dashboard when the intent is navigation or drill-down: reporting, analytics, team workspaces, operational queues, or related dashboards.

Use Open approval form when the intent is to start a workflow/request: AP approval, payment request, purchase request, HR request, budget approval, or other approval process.

Use Action/form-action binding when the intent is in-form or in-page logic rather than navigation: set variables, confirm, query data, submit, or other front-end action sequences. Dashboard-specific Action binding remains unproven in this export.

## Validator And Generator Rules Added

- Container/Button action `action-type` must be known for generated apps.
- Link actions must include a literal URL or URL expression variable.
- Add list item actions must resolve `attrs.data.list.ListID`; optional `layout` must resolve; `passvalues[].Name` must resolve to target fields.
- Open dashboard actions must resolve `attrs.data.page.PageID` to a Type `103` dashboard.
- Open approval form actions must resolve `attrs.data.form.ProcKey` to an included approval form.
- Open mode values must be export-proven: empty/default, `modal`, `slide`, `target`, or `new`.
- Modal size values must be export-proven: `0`, `1`, `2`, `3`, or `9`.
- Generated-final unresolved action targets are hard errors; compatibility/source-export inspection warns first.
- Import-readiness now runs the Container/Button action inspector.

## Proof Boundary

Container/Button action settings in this dashboard export are export-proven for Link, Add list item, Open dashboard, Open approval form, open modes, modal sizes, current-app target references, passvalues, queryParams, and Open approval form `setVars`.

The screenshot is UI-reference-backed only.

The new inspector and validator checks are validator-backed.

Runtime navigation/open behavior, cross-application targeting, literal external Link behavior, Add list item form-save behavior, dashboard open behavior, approval form start behavior, and form-action binding behavior are not runtime-proven by this pass.

---

<a id="data-filter-controls-runtime-proof"></a>

<!-- source: docs/studies/collection-01.md#data-filter-controls-runtime-proof -->

# Dashboard Data Filter Controls Runtime Proof

## Scope

- Branch: `codex/data-filter-controls-runtime-proof`
- Generated package: `data-filter-controls-runtime-proof.v1.yap`
- Downloads copy: `<downloads>/data-filter-controls-runtime-proof.v1.yap`
- App: `Data Filter Runtime Proof`
- Dashboard: `Dashboard`
- Runtime tenant: `https://<yourdomain>.yeeflow.com/`

This is a focused runtime proof for a generated dashboard package. It does not attempt exhaustive behavior coverage for all 12 Data Filter controls or every operator/value shape.

## Package Shape

The generated package uses synthetic event-planning rows and a small dashboard with:

- Search filter bound to `filter_Search`
- Radio filter bound to `filter_Radio_Vendor`
- Range filter bound to `filter_Range_BudgetNumber`
- Sorting filter bound to `filter_Sorting`
- Apply button bound by `search-filter.attrs.apply_t = "2"` and `search-filter.attrs.apply_btn`
- One data table/list-like dashboard control
- Summary controls
- Pie, column, and line chart/report controls

Remove filters is not included in the final runtime package. A candidate `remove-filers` control using the export-proven no-target shape was tried during the session, but it did not surface as a visible runtime control. Keep Remove filters reset behavior export-proven only until a focused generated package proves a stable reset-target/runtime shape.

Hierarchy filter is not included in this runtime package. It remains export-proven from the CRM dashboard learning pass, not runtime-proven.

## Local Validation

Final package validation results:

| Check | Result |
| --- | --- |
| Generator syntax check | pass |
| Data Filter inspector | pass, 0 errors, 0 warnings |
| Dashboard page inspector | pass |
| Strict package validator | pass with warnings, 0 errors |
| Strict graph validator | pass with warnings, 0 errors |
| App creation rules inspector | pass |
| YAP schema-standard inspector | pass |
| Materialization inspector | pass with warnings, 0 errors |
| Aggregate import-readiness gate | pass with warnings, 0 errors |
| Wrapper round trip / placeholder scan | pass |

Warnings were non-blocking template-backbone warnings, including schema-supported-runtime-unproven field controls and `NO_FORMS` for the dashboard/list-only proof package. The Data Filter-specific inspector reported no unresolved filter variables, no unresolved downstream references, and no missing apply-button reference.

## Runtime Result

Runtime import/open was performed in Yeeflow using the generated package from Downloads.

Observed:

- Package upload parsed app metadata as `Data Filter Runtime Proof`.
- Import completed and the app appeared in the workspace.
- The app opened.
- The `Dashboard` page opened.
- Dashboard summary controls rendered.
- The data table/list-like control rendered rows.
- Chart/report controls rendered: pie, column, and line chart areas were visible.
- Representative Data Filter controls rendered: Search filter, Radio filter, Range filter, Sorting filter, and Apply button.
- No missing filter variable error was observed.
- No missing downstream data binding error was observed.
- No dashboard render crash was observed.

## Interactions Tested

- Click-on-apply interaction: entered `Annual` in the Search filter and clicked `Apply Search`. The dashboard stayed stable and the URL updated with `__filter_filter_Search=Annual`.
- Value-change interaction: selected a Radio filter option (`Party Props & More`). The dashboard stayed stable, the selected value rendered in the control, and chart/table surfaces remained visible.

These interactions prove only basic safe interaction stability for this generated representative dashboard package. They do not prove full filter semantics, every operator, every control type, pagination behavior, performance, or cross-control combination correctness.

## Proof Boundary

- Dashboard Data Filter schemas across Sales and CRM exports: export-proven.
- Help Center behavior: product-documented.
- Generated dashboard package import/open/render/basic interaction: runtime-proven for the representative controls in this package only.
- Search click-apply wiring: runtime-proven for the representative generated package.
- Radio value-change selection stability: runtime-proven for the representative generated package.
- Range and Sorting controls: render-proven in this package; exhaustive interaction semantics not proven.
- Remove filters: not runtime-proven in this package.
- Hierarchy filter: not runtime-proven in this package.
- Approval-form and data-list-form Data Filter usage: not runtime-proven.
- Data mutation, workflow behavior, notifications, permissions/security, and large-data performance: not tested.

---

<a id="data-filter-controls"></a>

<!-- source: docs/studies/collection-01.md#data-filter-controls -->

# Data Filter Controls Export Study

Proof boundary: dashboard Data Filter schema across `Sales_Management_AD.yap` and `CRM - Customer relationship management.yap` is export-proven only. Help Center behavior is product-documented. Approval-form and data-list-form applicability is product-documented only in this pass. Runtime behavior is not runtime-proven.

## Source

- Export path: `/Users/Renger/Downloads/Sales_Management_AD.yap`
- Follow-up export path: `/Users/Renger/Downloads/CRM - Customer relationship management.yap`
- Repository branch: `codex/data-filter-controls-learning`
- Follow-up branch: `codex/data-filter-controls-crm-learning`
- Target dashboard pages: `Dashboard`, `Data Report`
- CRM follow-up target dashboard page: `Dashboard`
- Original export handling: decoded read-only into ignored `.tmp/` only; raw `.yap` and decoded payloads are not committed.
- Privacy handling: normalized references use placeholders for app/list/page/control IDs, data sources, labels, and fields where needed.

## Help Center Context

Reviewed collection: <https://<yourdomain>.yeeflow.com/<redacted-path>>

The Help Center currently lists 14 Data Filter articles: overview, apply type, Search Filter, Select Filter, Checkbox Filter, Radio Filter, Range Filter, Check Range, Date Filter, Relative Period, Apply Button, Remove Filters, Hierarchy filter, and Sorting filter.

Product-documented concepts:

- Data Filter controls can be added to approval forms, list forms, and dashboards.
- Most value-producing filters bind to a filter variable. The filter variable stores the selected filter condition and can be used by data-bound controls.
- Data-bound consumers include data tables, collections/news/gallery-style controls, document library controls, lookup/lookup list controls in approval forms, and dashboard/data-list-form analytics controls such as Pie Chart, Column Chart, Line Chart, Gauge, Funnel Chart, Color Block Heatmap, Summary, and Pivot Table.
- Apply type has two documented modes: `Value change` and `Click on Apply Button`. In exports studied here, `attrs.apply_t = "2"` maps to `Click on apply button`; missing/unspecified apply type is treated as default/unspecified and should not be overclaimed as runtime behavior.
- Apply Button and Remove Filters are special filter-related controls rather than normal value-producing filters.
- Checkbox-style multi-value variables should be consumed with multi-value-compatible operators such as In/Not In; Date Filter variables internally carry the date-range semantics and are product-documented as used with an Equal condition on a date field.

## Export Inventory

| Page | Page ID | Total controls inspected | Data Filter controls | Types found | Filter variables | Apply types found | Apply buttons | Remove filters | Consumers using filter variables | Affected data sources | Unresolved references | Proof |
| --- | --- | ---: | ---: | --- | ---: | --- | ---: | ---: | ---: | --- | --- | --- |
| Dashboard | `<page-id>` | 51 | 8 | Checkbox filter, Date filter, Range filter, Check range, Relative period, Select filter, Apply button, Remove filters | 10 | Click on apply button; default/unspecified | 1 | 1 | 2 | `<list:5>` | 2 stale chart references to undeclared `filter_4` / `filter_5` | export-proven with warnings |
| Data Report | `<page-id>` | 56 | 3 | Checkbox filter, Date filter | 7 | default/unspecified | 0 | 0 | 7 | `<list:5>`, `<list:7>` | none found | export-proven |

Export-proven from this sample:

- Checkbox filter, exported as `check-filter`
- Select filter, exported as `select-filter`
- Range filter, exported as `range-filter`
- Check range, exported as `check-range`
- Date filter, exported as `date-filter`
- Relative period, exported as `relative-period`
- Apply button, exported as `apply-button`
- Remove filters, exported in this sample as `remove-filers`

Not found in the Sales sample, then covered by the CRM follow-up:

- Search filter: export-proven from CRM Dashboard.
- Radio filter: export-proven from CRM Dashboard.
- Hierarchy filter: export-proven from CRM Dashboard.
- Sorting filter: export-proven from CRM Dashboard.

## 12-Control Coverage

| Product control | Export status | Export evidence | Runtime boundary |
| --- | --- | --- | --- |
| Search filter | export-proven | CRM Dashboard | not runtime-proven |
| Select filter | export-proven | Sales Dashboard | not runtime-proven |
| Checkbox filter | export-proven | Sales Dashboard and Data Report | not runtime-proven |
| Radio filter | export-proven | CRM Dashboard | not runtime-proven |
| Range filter | export-proven | Sales Dashboard | not runtime-proven |
| Check range | export-proven | Sales Dashboard | not runtime-proven |
| Date filter | export-proven | Sales Dashboard and Data Report | not runtime-proven |
| Relative period | export-proven | Sales Dashboard and CRM Dashboard | not runtime-proven |
| Hierarchy filter | export-proven | CRM Dashboard | not runtime-proven |
| Sorting filter | export-proven | CRM Dashboard, exported as `sorting-filters` | not runtime-proven |
| Apply button | export-proven | Sales Dashboard and CRM Dashboard | not runtime-proven |
| Remove filters | export-proven | Sales Dashboard, exported as `remove-filers` | not runtime-proven |

## Export Schema Pattern

Dashboard pages are Type `103` layouts with embedded page JSON in `Data.Item.Layouts[].LayoutInResources[0].Resource`. The embedded page JSON declares filter variables at:

```text
$.filterVars[]
```

Observed variable entries are minimal:

```json
{ "id": "filter_<name>", "idx": "<optional-uuid-like-id>" }
```

Value-producing filter controls bind through:

```text
<control>.binding = "__filter_" + filterVars[].id
```

The associated filter variable is therefore resolved by stripping the `__filter_` prefix from the control binding and matching it to `page.filterVars[].id`.

Downstream consumers reference filter variables in expression-token arrays:

```json
{
  "exprType": "variable",
  "valueType": "string",
  "id": "__filter_<filterVarId>",
  "type": "expr",
  "name": "<filterVarId>"
}
```

Consumer condition locations found:

- Data table/list-like dashboard control: `$.children[*].attrs.data.filter[]`
- Dashboard report/chart extensions: `$.exts[].attr.settings.Conditions[]`

The export uses condition fields such as `left`, `op`, `pre`, `right`, and `showCus`. Nested condition groups should be checked recursively.

## Export-Proven Controls

### Checkbox Filter

- Product name: Checkbox filter
- Export type: `check-filter`
- Binding path: `<control>.binding`
- Associated variable path: `$.filterVars[]`
- Option source path: `attrs.data.list`
- Option settings: `attrs.display_f`, `attrs.value_f`, `attrs.ps`, optional `attrs.data.filter[]`
- Display settings found: `layout`, `search-enable`, `more-enable`, `dropdown-enable`, `more-text`, `less-text`, label/field styling
- Apply type found: one `Click on apply button` through `attrs.apply_t = "2"` plus default/unspecified examples
- Downstream usage: table/data-list filters and dashboard report/chart extension conditions reference the variable with `__filter_` expression tokens
- Validation rule: binding must resolve to `page.filterVars[]`; option source list should resolve; downstream operators should be compatible with multi-select conditions
- Generation rule: generate checkbox filters only when a multi-select filter is needed and use multi-value-compatible downstream operators

### Select Filter

- Product name: Select filter
- Export type: `select-filter`
- Binding path: `<control>.binding`
- Option source path: `attrs.data.list`
- Option fields: `attrs.display_f`, `attrs.value_f`
- Apply type found: default/unspecified
- Downstream usage: one data table/list-like dashboard control consumes the selected value through a filter variable
- Validation rule: binding, option source list, display field, value field, and downstream filter variable references must resolve
- Generation rule: use for single-value selection when a source list field provides stable display/value fields

### Range Filter

- Product name: Range filter
- Export type: `range-filter`
- Binding path: `<control>.binding`
- Settings found: `attrs.number_max`, slider/input/value styling
- Apply type found: default/unspecified
- Downstream usage: no downstream consumer in this export used the range variable
- Validation rule: binding must resolve; variable shape/operator compatibility remains warning-first until a consumer example is export-proven
- Generation rule: do not generate downstream range conditions from this sample alone unless another export/runtime proof maps the operator shape

### Check Range

- Product name: Check range
- Export type: `check-range`
- Binding path: `<control>.binding`
- Settings found: `attrs.options[]` with min/max range entries and optional `attrs.prefix`
- Apply type found: default/unspecified
- Downstream usage: no downstream consumer in this export used the check-range variable
- Validation rule: binding must resolve; option range entries should include min/max when generated
- Generation rule: keep downstream condition wiring unproven until a consumer condition is captured

### Date Filter

- Product name: Date filter
- Export type: `date-filter`
- Binding path: `<control>.binding`
- Settings found: `attrs.layout`, edit/label/common styling
- Apply type found: default/unspecified
- Downstream usage: `Data Report` report extensions consume the date filter variable in date-field conditions; `Dashboard` data table/list-like control also consumes a date-like variable
- Validation rule: binding and downstream filter variable references must resolve; date-filter downstream operators should be warning-first unless a target consumer/operator shape is export-proven
- Generation rule: use a date field condition and bind the `right` expression token to the date filter variable

### Relative Period

- Product name: Relative period
- Export type: `relative-period`
- Binding path: `<control>.binding`
- Settings found: `attrs.choice-options[]`, layout and choice styling
- Apply type found: default/unspecified
- Downstream usage: no downstream consumer in this export used the relative-period variable
- Validation rule: binding must resolve; supported choice-option tokens should remain export-backed
- Generation rule: do not invent relative-period condition schema beyond the exported control settings until a consumer example is captured

### Apply Button

- Product name: Apply button
- Export type: `apply-button`
- Binding path: none found; it is referenced by filter controls through `attrs.apply_btn`
- Export-proven association: one `check-filter` uses `attrs.apply_t = "2"` and `attrs.apply_btn = <apply-button-control-id>`
- Behavior boundary: product-documented as the trigger for click-apply filters; actual runtime refresh behavior is not proven in this pass
- Validation rule: every `attrs.apply_btn` must resolve to an existing `apply-button` control id
- Generation rule: when any filter uses click-apply mode, generate an Apply button and wire every click-apply filter to it

### Remove Filters

- Product name: Remove filters
- Export type found: `remove-filers`
- Binding path: none found
- Explicit reset targets: none visible in this sample
- Behavior boundary: product-documented reset behavior; export proves the special control exists but not whether this instance clears all filters or selected filters
- Validation rule: explicit reset targets must resolve when present; missing/unknown target structure should warn, not fail
- Generation rule: include only when a dashboard has multiple filters and a reset action is useful; keep exact target/reset schema warning-first until a target-bearing export is captured

## Downstream Consumers

`Dashboard` contains one data table/list-like consumer that resolves three filter variables:

- checkbox filter variable to a text field with operator `9`
- date filter variable to a date-like field with operator `0`
- select filter variable to a text field with operator `0`

`Dashboard` also contains one chart/report extension with two unresolved stale filter variable references (`filter_4`, `filter_5`). Those references do not resolve to `Dashboard` page `filterVars[]`, so they should be treated as an export warning and a generated-package validator error.

`Data Report` contains seven dashboard report/chart extension consumers. They use three resolved filter variables:

- `filter_customers`
- `filter_productline`
- `filter_orderdate`

Observed report/chart extension condition path:

```text
$.exts[].attr.settings.Conditions[]
```

Each dynamic filter condition uses `right` as an expression-token array whose `id` is `__filter_<filterVarId>` and whose `name` is the filter variable id.

## Generation Rules

- Define `page.filterVars[]` before any filter control or consumer references them.
- Bind value-producing Data Filter controls with `binding: "__filter_" + filterVarId`.
- Do not treat Apply button or Remove filters as value-producing filter controls.
- Use `attrs.apply_t = "2"` only with a valid `attrs.apply_btn` pointing to an `apply-button`.
- New plans must explicitly choose immediate or apply-button mode; retain unspecified mode only as historical export evidence. Declare reload dependents or the linked apply-button ID. Selection options come from runtime-distinct values of the bound field, without enumeration or default selection. Omitted remove-filter scope resets only apply-button variables; explicit scope resets exactly those variables. See `../standards/product-14.5/incremental-alignment.md` for the fixed product profile and preserved runtime conflicts.
- Wire downstream `attrs.data.filter[]` or `exts[].attr.settings.Conditions[]` to filter variables through expression-token arrays.
- Validate the condition left field against the consumer data source.
- Generate only export-proven filter control types and settings. Sales proves Select, Checkbox, Range, Check range, Date, Relative period, Apply button, and Remove filters. CRM proves Search, Radio, Hierarchy, and Sorting. Keep any settings not seen in these exports as product-documented or unproven until later evidence.
- Treat `remove-filers` as the observed export spelling for this sample, while product terminology remains Remove filters.

## Validation Rules

- `page.filterVars` must be an array when present.
- Every value-producing Data Filter control binding must resolve to `page.filterVars[].id`.
- Every downstream `__filter_` expression must resolve to `page.filterVars[].id`.
- Expression `id` must equal `__filter_` plus expression `name`.
- Click-apply filters must reference an existing `apply-button` control.
- Remove filters reset targets must resolve when explicit targets are present.
- Unknown filter control types should warn first.
- Unsupported filter variable shapes should warn first unless an export proves the shape invalid.
- Generated final packages should treat unresolved filter variable references as validation errors.

## Normalized References

Created under `docs/studies/normalized/data-filter-controls.bundle.json`:

- `data-filter-variable-definition.normalized.json`
- `data-filter-checkbox-control.normalized.json`
- `data-filter-select-control.normalized.json`
- `data-filter-range-control.normalized.json`
- `data-filter-check-range-control.normalized.json`
- `data-filter-date-control.normalized.json`
- `data-filter-relative-period-control.normalized.json`
- `data-filter-apply-button.normalized.json`
- `data-filter-remove-filters.normalized.json`
- `data-filter-consumer-condition.normalized.json`
- `data-filter-consumer-chart.normalized.json`
- `data-filter-consumer-table.normalized.json`
- `data-filter-click-apply-binding.normalized.json`
- `data-filter-value-change-binding.normalized.json`

The Sales export did not provide normalized refs for Search, Radio, Hierarchy, or Sorting filters; the CRM follow-up refs below cover those product controls.

## CRM Dashboard Follow-up: Search, Radio, Hierarchy, and Sorting Filters

Source export: `/Users/Renger/Downloads/CRM - Customer relationship management.yap`

Target page: `Dashboard`

CRM Dashboard inventory:

- Total controls inspected: 75
- Data Filter controls found: 15
- Filter variables found: 14
- Search filters: 3
- Radio filters: 5
- Hierarchy filters: 4
- Sorting filters: 1
- Apply buttons: 1
- Remove filters: 0
- Downstream consumers using filter variables: 8
- Unresolved filter references: none found in the CRM Dashboard target page

The CRM Dashboard also includes one Relative period filter; that control was already export-proven from the Sales export, so the CRM pass primarily extends the missing Search, Radio, Hierarchy, and Sorting controls.

### Search Filter

- Product name: Search filter
- Export type: `search-filter`
- Settings path: `<control>.attrs`
- Associated filter variable path: `$.filterVars[]`
- Binding path: `<control>.binding = "__filter_" + filterVarId`
- Settings found: `attrs.placeholder`, optional `attrs["minnumber-letters"]`, optional `attrs.apply_t`, optional `attrs.apply_btn`
- Apply type found: default/unspecified and `Click on apply button` through `attrs.apply_t = "2"`
- Option/source behavior: no option source list; the search text is stored in the filter variable
- Downstream consumers: dashboard data-list control `attrs.data.fulltext[]` entries reference Search variables through expression-token arrays
- Value shape: expression token in `fulltext[].value[]` with `id = "__filter_<filterVarId>"` and `name = "<filterVarId>"`
- Generation rule: use Search filter for text/fulltext search across one or more fields; configure `minnumber-letters` only when a minimum input length is desired; use click-apply mode for heavier searches
- Validation rule: Search filter binding must resolve to `page.filterVars[]`; every `fulltext[].value[]` filter variable must resolve and use the matching `__filter_` id
- Proof level: export-proven dashboard schema, not runtime-proven

### Radio Filter

- Product name: Radio filter
- Export type: `radio-filter`
- Settings path: `<control>.attrs`
- Associated filter variable path: `$.filterVars[]`
- Binding path: `<control>.binding = "__filter_" + filterVarId`
- Option/source behavior: `attrs.data.list` points to an option source list; `attrs.display_f` selects the displayed field; `attrs.value_f` selects the stored value field; `attrs.data.sort[]` can order options
- Settings found: `attrs.ps`, `attrs.layout`, `attrs.search-enable`, `attrs.more-enable`, `attrs.more-text`, `attrs.less-text`, `attrs.search-placeholder`, `attrs.displayStyle`, optional click-apply settings
- Apply type found: default/unspecified and `Click on apply button` through `attrs.apply_t = "2"`
- Downstream consumers: no CRM downstream condition consumed a Radio variable in the data-list table, but dashboard report/chart extension conditions consume the existing `filter_period` and one radio-like condition was observed in CRM report extension conditions. Treat data-list consumer wiring for Radio as the same filter-variable expression bridge but keep exact operator choice field-specific.
- Value shape: single selected option value from `attrs.value_f`
- Generation rule: use Radio filter for single-choice filtering where choices come from a list or reference table; use `displayStyle: "dropdown"` only when a dropdown presentation is desired; use search/more settings for longer option lists
- Validation rule: Radio binding, option source list, display field, value field, and any downstream variable expression must resolve
- Proof level: export-proven dashboard schema, not runtime-proven

### Hierarchy Filter

- Product name: Hierarchy filter
- Export type: `hierarchy-filter`
- Settings path: `<control>.attrs`
- Associated filter variable path: `$.filterVars[]`
- Binding path: `<control>.binding = "__filter_" + filterVarId`
- Hierarchy settings found: optional `attrs.parentId`, `attrs.multiple`, `attrs["hierarchical-select"]`, `attrs.type`, `attrs.categoryId`, `attrs.source`
- List-backed hierarchy settings found: `attrs.data.list`, `attrs.data.sort[]`, `attrs.display_f`, `attrs.value_f`, `attrs.parent_f`, `attrs.child_f`
- Apply type found: default/unspecified
- Downstream consumers: dashboard data-list control `attrs.data.filter[]` references Hierarchy variables in expression-token arrays. One list-backed hierarchy uses a child/self field relationship through `child_f` and `parent_f`.
- Value shape: selected hierarchy node value(s), with multiple selection visible through `attrs.multiple = true`
- Generation rule: use Hierarchy filter only when the source data has a hierarchy relationship. For list-backed hierarchies, configure list, display/value fields, parent field, and child field together. Do not invent category/source modes beyond export-proven settings.
- Validation rule: Hierarchy binding must resolve; list-backed source list and fields should resolve; downstream consumer expressions must resolve; unsupported hierarchy source modes should warn first
- Proof level: export-proven dashboard schema, not runtime-proven

### Sorting Filter

- Product name: Sorting filter
- Export type: `sorting-filters`
- Settings path: `<control>.attrs`
- Associated filter variable path: `$.filterVars[]`
- Binding path: `<control>.binding = "__filter_" + filterVarId`
- Source behavior: `attrs.data.list` points to the data source being sorted
- Sorting options path: `attrs.sort_list[]`
- Sorting option shape: each option includes `mapkey`, `title`, `orderby`, and `order`
- Apply type found: default/unspecified
- Downstream consumers: dashboard data-list control `attrs.data.sortingfilter[]` references the Sorting variable through an expression-token array
- Value shape: selected sort option mapped to a field/order pair
- Generation rule: use Sorting filter when users need to choose between preconfigured sort orders. Each generated option should point to a real field and use `asc` or `desc`.
- Validation rule: Sorting filter binding must resolve; source list must resolve; every `sort_list[].orderby` should resolve to a source field when possible; every downstream `sortingfilter[]` expression must resolve to `page.filterVars[]`
- Proof level: export-proven dashboard schema, not runtime-proven

### CRM Apply Button

The CRM Dashboard includes one `apply-button`. Two filters use click-apply wiring:

- one Search filter with `attrs.apply_t = "2"` and `attrs.apply_btn`
- one Radio filter with `attrs.apply_t = "2"` and `attrs.apply_btn`

Generation and validation rule: one Apply button can be referenced by multiple click-apply filters. Every `attrs.apply_btn` must resolve to an existing `apply-button` control id.

### CRM Downstream Consumers

The CRM Dashboard proves additional consumer paths:

- Search filters feed dashboard data-list `attrs.data.fulltext[]`.
- Hierarchy filters feed dashboard data-list `attrs.data.filter[]`.
- Sorting filter feeds dashboard data-list `attrs.data.sortingfilter[]`.
- Report/chart extension conditions continue to use `exts[].attr.settings.Conditions[]`.

The shared expression token rule remains:

```json
{
  "exprType": "variable",
  "id": "__filter_<filterVarId>",
  "name": "<filterVarId>"
}
```

### CRM Normalized References

Added under `docs/studies/normalized/data-filter-controls.bundle.json`:

- `data-filter-search-control-crm.normalized.json`
- `data-filter-search-variable-crm.normalized.json`
- `data-filter-radio-control-crm.normalized.json`
- `data-filter-radio-variable-crm.normalized.json`
- `data-filter-hierarchy-control-crm.normalized.json`
- `data-filter-hierarchy-variable-crm.normalized.json`
- `data-filter-sorting-control-crm.normalized.json`
- `data-filter-sorting-variable-crm.normalized.json`
- `data-filter-crm-consumer-condition.normalized.json`
- `data-filter-crm-apply-binding.normalized.json`

These references use redacted list/control/page placeholders and do not include raw CRM data rows, customer names, emails, tenant IDs, or decoded payloads.

---

<a id="data-list-custom-form-fields"></a>

<!-- source: docs/studies/collection-01.md#data-list-custom-form-fields -->

# Data List Custom List Form Fields

## Scope

Source export: `<downloads>/Data Lists (3).yap`

Target data lists:

- `Data list with fields part A`
- `Data list with fields part B`
- `Data list with fields part C`

Proof boundary:

- Data List custom list form schema and field-control usage in this export: export-proven.
- Data List validation updates in this branch: validator-backed.
- Document Library custom-form applicability: product/user-understanding-backed only; this export contains no Type `16` document library custom form proof.
- Runtime form rendering, field editing, save/submit behavior, form action execution, and Document Library behavior: not runtime-proven.

This pass studies how already-defined Data List fields are placed and configured on custom list forms. It does not import the package, create records, or execute form actions.

## Inventory

The export contains 11 list-like app resources. The three target data lists were found and inspected.

| Target list | Fields in `Defs` | Custom fields | System fields | Custom list forms | Controls inspected | List-bound controls | Grid controls | Temp variables | Form actions | Field types represented on forms |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `Data list with fields part A` | 75 | 74 | 1 | 2 | 157 | 150 | 2 | 2 | 3 | `input`, `textarea`, `richtext`, `input_number`, `percent`, `currency`, `switch`, `radio`, `checkbox`, `datepicker`, `time`, `identity-picker`, `organization-picker`, `location-picker`, `file-upload`, `icon-upload`, `lookup`, `calculated-column` |
| `Data list with fields part B` | 18 | 17 | 1 | 1 | 20 | 17 | 1 | 0 | 0 | `input`, `metadata`, `mutiple-metadata`, `cost-center-picker`, `rate`, `hyperlink`, `signer`, `tag`, `autonumber`, `list` |
| `Data list with fields part C` | 75 | 74 | 1 | 1 | 78 | 75 | 1 | 0 | 0 | `input`, `textarea`, `richtext`, `input_number`, `percent`, `currency`, `switch`, `radio`, `checkbox`, `datepicker`, `time`, `identity-picker`, `organization-picker`, `location-picker`, `file-upload`, `icon-upload`, `lookup`, `calculated-column` |

Summary:

- Custom list forms found: 4.
- Controls inspected: 255.
- List-bound controls inspected: 242.
- Grid controls inspected: 4.
- Nested sub-list field controls found: 13, all under the Part B `list` field control.
- Temp variables found: 2.
- Form actions found: 3.
- Export-proven field types represented on custom forms: 27.
- `select` and `flowstatus` were not present on these custom list forms and remain unproven for this export.

## Storage Shape

Custom list forms are stored on the list resource itself:

- `Data.Item.Layouts[]` or `Data.Childs[].Layouts[]`
- custom form layout: `Type = 1`
- normal data view layout: `Type = 0`
- embedded form JSON: `Layouts[].LayoutInResources[0].Resource`
- `LayoutInResources[0].ID` and `RefId` match `Layouts[].LayoutID` in this export.
- `Layouts[].LayoutView` is `null` for embedded custom form layouts.
- `Layouts[].Ext2` carries a source marker shape like `{"src":true}`.
- `ListModel.LayoutView` assigns custom forms to display settings such as `add`, `edit`, and `view`.

Assignment patterns found:

- Part A uses one custom form for `edit` and another for `add`; `view` remains `default`.
- Part B assigns the same custom form to `add`, `edit`, and `view`.
- Part C assigns the same custom form to `add` and `edit`; `view` remains `default`.

The embedded form resource has these export-proven top-level keys:

- `children`
- `attrs`
- `title`
- `filterVars`
- `ver`
- `tempVars`
- `exts`
- optional `actions`
- optional `formAction`
- optional `filter`

Generation rule: generated custom list forms must keep `children`, `filterVars`, and `tempVars` as arrays. Actions should be arrays when present. Do not confuse Data List custom list forms with approval submission forms or workflow task forms.

## Field Controls

List fields are represented on custom list forms as controls under `Resource.children`, usually inside nested containers and one `flex_grid`.

Common export-proven control shape:

- `id`: control identifier.
- `type`: control type; usually matches the Data List field `Type`.
- `binding`: list field `FieldName`.
- `fieldID`: list field `FieldID`.
- `label`: display label.
- `attrs`: field settings copied or transformed from `Defs[].Rules`.
- `isFilter`, `isSort`, `isUnique`, `isSystem`, `status`: field metadata mirrored from the list field.
- optional `value`: default value or control value shape when configured.

Validation rules:

- A list-bound control `binding` must resolve to a field in the same list, except nested sub-list controls that use `attrs.list_field = true`.
- A list-bound control `fieldID` must resolve to the same field as `binding`.
- Control ids should be unique within a custom form.
- Unknown visual controls should warn until export-proven.

## Field Families On Forms

Text and input:

- `input`, `textarea`, `richtext`, and `hyperlink` controls keep the list field `binding` and carry field-specific `attrs` such as `placeholder`, `input-maxlength`, `required`, `encrypt`, `allowScan`, `richtext-type`, and hyperlink settings where present.

Numeric and financial:

- `input_number`, `currency`, `percent`, and `rate` controls preserve numeric settings from field rules, including `displayThousandths`, `rounded-to`, `number_min`, `number_max`, `number_step`, `currencyCode`, `displayFormat`, and `rate-type`.
- `calculated-column` controls are present as list-bound controls with `attrs.calculated` and `attrs.calculated_result`; runtime formula calculation is not proven here.

Selection and choice:

- `switch`, `radio`, `checkbox`, and `tag` controls carry selection settings such as `choices`, `color_choices`, `displayStyle`, `show_color`, `required`, `placeholder`, and tag source/category settings.
- `select` is not present on these custom forms.

Date and time:

- `datepicker` and `time` controls use datetime-backed fields and carry settings such as `date_picker`, `showtime`, `dateformat`, `minuteStep`, `date_type`, `disable_now_button`, `placeholder`, and `required`.

Identity and organization:

- `identity-picker`, `organization-picker`, and `cost-center-picker` controls carry picker settings such as `identity-maxselection`, `multiple`, `metadata-treeselect`, `parentId`, and `required`.
- Tenant-specific default values or picker sources must not be generated without a safe source or explicit user mapping.

Uploads and media:

- `file-upload` and `icon-upload` controls carry settings such as `ver`, `file_multiple`, `file_types`, `file_typeslimit`, `maxsize`, `controlmultiple`, `picture_size_limit`, and `required`.
- Upload behavior is not runtime-proven by this export.

Advanced:

- `lookup` controls carry target metadata such as `appid`, `listid`, `listsetid`, `listfield`, `listfilter`, `search-fields`, `search-scope`, sort settings, `max-selection`, `multiple`, and display settings.
- `metadata` and `mutiple-metadata` controls carry `source`, `categoryId`, optional `parentId`, `metadata-treeselect`, `max-selection`, and default value shapes.
- `autonumber` and `signer` appear as list-bound controls.
- `flowstatus` is not present on these custom forms.

## Grid Layout

Each custom list form uses a simple nested layout:

- outer `container`
- inner `container`
- one `flex_grid`
- list-bound field controls as grid children

The `flex_grid` control has:

- `attrs.ver`
- `attrs.columns` with breakpoint keys such as `1`, `2`, and `3`
- `attrs.rows`
- `attrs.cgap`
- `attrs.cgapU`
- `children` containing the controls in render/order sequence

The export does not show explicit per-child row/column coordinates. Generation should therefore keep a stable non-overlapping child order and use `flex_grid` settings conservatively. Field order should follow list field order unless the user asks for grouped sections.

## Sub-list Controls

The sub-list field in Part B is represented by a parent control:

- parent control `type = "list"`
- parent control `binding` points to the Data List field `FieldName`
- `attrs.list-variables` defines nested sub-list variables
- `attrs.list-fields` defines nested field entries and their nested controls

Nested sub-list variables found:

- `text`
- `number`
- `boolean`
- `date`
- `file`
- `metadata`
- `user`
- `costcenter`
- `groupselect`
- `location`
- `lookup`
- `img`
- `mutiple-metadata`

Nested controls use:

- `control.type` mapped from the nested variable type, such as `input`, `input_number`, `switch`, `datepicker`, `file-upload`, `metadata`, `identity-picker`, `cost-center-picker`, `organization-picker`, `location-picker`, `lookup`, `icon-upload`, and `mutiple-metadata`.
- `control.binding` values such as `field_1`, scoped to the sub-list control rather than the parent list `Defs`.
- `control.attrs.list_field = true`
- `control.attrs.list_field_binding` pointing back to the parent sub-list field binding.
- `control.attrs.list_control_id` pointing back to the parent list control id.

Validation rules:

- Parent sub-list controls should include both `attrs.list-variables` and `attrs.list-fields`.
- Nested `list-fields` entries should resolve to `list-variables` by name.
- Nested controls should include `type`, `binding`, and `attrs.list_field = true`.
- Nested controls should point back to the parent sub-list field via `attrs.list_field_binding`.

## Temp Variables

Temp variables are custom-list-form scoped, not workflow variables.

Export-proven path:

- `LayoutInResources[0].Resource.tempVars[]`

Observed shape:

- `idx`
- `id`

The Part A edit form includes two temp variables and uses them in form actions. Action expressions reference temp variables with expression objects using `exprType = "variable"` and ids/names such as `__temp_var_*` / `var_*`. Validators should allow the export-proven aliasing pattern but must still require every referenced temp variable to resolve.

Generation rules:

- Temp variable ids must be unique within a form.
- Temp variables should be generated only when an action or control needs them.
- Temp variables should not be confused with Data List fields or workflow variables.

## Form Actions

Only Part A's edit custom list form contains form actions.

Export-proven paths:

- `LayoutInResources[0].Resource.actions[]`
- `LayoutInResources[0].Resource.actions[].steps[]`
- `LayoutInResources[0].Resource.formAction`
- action button binding: `control.attrs.control_action`

Observed action shape:

- action `id`
- action `name`
- `steps` array
- step `type = "setvar"`
- step `attrs.setvar_multi` and `attrs.setvar_array` for multi-set actions
- step `attrs.setvar_var` and `attrs.setvar_val` for single-set actions
- optional `condition` expression array

Observed bindings:

- `formAction.onLoad` points to a form action id.
- an `action_button` control uses `attrs.control_action` to point to an action id.

No submit/save action step was found in the target custom list forms. Submit/save action behavior remains unproven for this export.

Validation rules:

- `form.actions` must be an array when present.
- action ids must be present and unique.
- action steps must be arrays.
- action button `attrs.control_action` references must resolve to `form.actions[].id`.
- `formAction` hook references must resolve to `form.actions[].id`.
- Set-variable targets that use `exprType = "list_field"` must resolve to the list `Defs`.
- Temp variable references must resolve to `form.tempVars`.
- Unknown action step types should warn, not fail, until export-proven.

## Custom List Form Display Settings

Custom form display settings are stored on the list resource, separate from the form layout itself.

Export-proven path:

- `Data.Childs[].ListModel.LayoutView`

Observed schema:

- `add`: New Item form assignment.
- `edit`: Edit Item form assignment.
- `view`: View Item form assignment.
- `opentype`: optional object keyed by `add`, `edit`, and `view`.
- `modalsize`: optional object keyed by `add`, `edit`, and `view`.

Form assignment values are either a custom form `Layouts[].LayoutID` or the literal `default`. The `default` value means the built-in default layout is used instead of a custom list form. In this export, custom form references resolve to `Layouts[]` entries with `Type = 1`.

Open mode mapping:

- `opentype.<usage> = "modal"` maps to Pop-up window.
- `opentype.<usage> = "slide"` maps to Slide in.
- missing `opentype.<usage>` uses the export/UI default: New Item and Edit Item open as Pop-up window; View Item opens as Slide in.
- Full page appears in the UI screenshots but was not found in the target-list export, so its exact export token remains unproven in this pass.

Size mapping:

- `modalsize.<usage> = 0` maps to Medium.
- `modalsize.<usage> = 1` maps to Small.
- `modalsize.<usage> = 2` maps to Large.
- `modalsize.<usage> = 3` maps to Full screen.
- missing `modalsize.<usage>` means Default size in the export, seen on Part B New Item.

The export proves size settings for Pop-up window and Slide in. Full page should not rely on pop-up/slide size behavior unless a future export proves that shape.

Target-list combinations found:

| List | Usage | Form assignment | Open mode | Size |
| --- | --- | --- | --- | --- |
| `Data list with fields part A` | New Item | `Custom list form 2` | Pop-up window | Small |
| `Data list with fields part A` | Edit Item | `Custom list form` | Pop-up window | Large |
| `Data list with fields part A` | View Item | Default layout | Slide in | Medium |
| `Data list with fields part B` | New Item | `Custom list form` | Slide in | Default |
| `Data list with fields part B` | Edit Item | `Custom list form` | Slide in | Large |
| `Data list with fields part B` | View Item | `Custom list form` | Pop-up window | Full screen |
| `Data list with fields part C` | New Item | `Custom list form` | Pop-up window | Full screen |
| `Data list with fields part C` | Edit Item | `Custom list form` | Pop-up window | Large |
| `Data list with fields part C` | View Item | Default layout | Slide in | Medium |

Validation rules:

- New/Edit/View form references must resolve to either `default` or an existing custom form layout.
- Open mode should be one of the export-proven values, or warn if unknown.
- Size should be one of the export-proven codes, or warn if unknown.
- Pop-up window and Slide in should preserve valid size settings when specified.
- Full page should not require a size setting; a size paired with Full page should warn until product/export evidence proves it.

Generation rules:

- Generated Data Lists may assign separate custom/default layouts for New Item, Edit Item, and View Item.
- Choose open mode and size based on form complexity: simple forms can use Small/Medium, longer field-heavy forms should use Large/Full screen.
- If the user does not specify display behavior, use conservative defaults: New Item in Pop-up window with Medium or Large, Edit Item in Pop-up window or Slide in with Medium/Large, and View Item in Slide in with Medium.
- Validate display settings independently from form layout validity.
- Document Library display-setting applicability remains product/user-understanding-backed only until a Type `16` export proves the exact shape.

Proof boundary: Data List custom list form display settings are export-proven from `Data Lists (3).yap`; the screenshots were used only as visual UI-label reference. Runtime opening/rendering behavior is not proven in this pass.

## Generation Rules

For generated Data List custom list forms:

- Store custom forms as `Layouts[]` entries with `Type = 1`.
- Use `LayoutInResources[0].Resource` as the embedded JSON string.
- Keep `LayoutInResources[0].ID` and `RefId` aligned to `LayoutID`.
- Assign forms through `ListModel.LayoutView.add`, `edit`, and/or `view`.
- Generate `children`, `filterVars`, and `tempVars` as arrays.
- Use `container` -> `container` -> `flex_grid` unless a stronger export-proven pattern is selected.
- Place list fields as controls with matching `type`, `binding`, `fieldID`, `label`, field metadata, and field-specific `attrs`.
- Use default/system fields only when appropriate; keep them read-only or system-aware when generated.
- Generate unique control ids.
- Validate all list-bound controls against `Defs`.
- Generate sub-list nested controls only inside a parent `list` control and use `attrs.list_field` metadata.
- Generate temp variables only when needed, and validate references.
- Generate form actions only when requested or structurally required, and validate button/hook/action references.
- Do not reuse approval form submission/task form assumptions without export proof.

Mandatory schema rules from earlier milestones still apply:

- `Defs` and `Layouts` must be arrays, never `null`.
- `ListModel.Flags = 1`.
- `ListModel.Status = 1` when present.
- `FieldIndex` / `FieldName` suffix must sync.
- `DisplayName`, `FieldName`, and `InternalName` must be unique in the same list.
- `InternalName` must match `[a-zA-Z0-9_]`.

## Normalized References

Redacted normalized references were created under:

`docs/studies/normalized/data-list-custom-forms.bundle.json`

Created refs:

- `custom-list-form-resource.normalized.json`
- `custom-list-form-grid-layout.normalized.json`
- `custom-list-form-list-bound-control.normalized.json`
- `custom-list-form-default-field-control.normalized.json`
- `custom-list-form-sublist-control.normalized.json`
- `custom-list-form-lookup-control.normalized.json`
- `custom-list-form-user-picker-control.normalized.json`
- `custom-list-form-attachment-control.normalized.json`
- `custom-list-form-image-control.normalized.json`
- `custom-list-form-calculated-control.normalized.json`
- `custom-list-form-temp-variable.normalized.json`
- `custom-list-form-action.normalized.json`
- `custom-list-form-button-action-binding.normalized.json`
- `custom-list-form-display-new-item-popup-small.normalized.json`
- `custom-list-form-display-edit-item-popup-large.normalized.json`
- `custom-list-form-display-view-item-slide-medium.normalized.json`
- `custom-list-form-display-default-layout.normalized.json`
- `custom-list-form-display-view-item-popup-full-screen.normalized.json`

No `custom-list-form-submit-action-step.normalized.json` was created because no submit/save step was found in the target custom list forms.
No `custom-list-form-display-full-page.normalized.json` was created because Full page was not found in the target-list export.

---

<a id="data-list-document-library-field-types"></a>

<!-- source: docs/studies/collection-01.md#data-list-document-library-field-types -->

# Data List and Document Library Field Types Learning

Source export: `<downloads>/Data Lists (2).yap`

Proof boundary: Data List field schemas in this document are export-proven from the two target Type `1` data lists. Document Library applicability is product/user-understanding-backed only in this pass because this export contains no Type `16` document library. Local validation and inspector checks are validator-backed. Import, data entry, Excel export, workflow behavior, and field-creation behavior are not runtime-proven in this pass.

## Inventory Summary

- Target lists: `Data list with fields part A`, `Data list with fields part B`.
- Fields inspected: 92 total, 90 custom, 2 system.
- Export-proven field types found: `autonumber`, `calculated-column`, `checkbox`, `cost-center-picker`, `currency`, `datepicker`, `file-upload`, `hyperlink`, `icon-upload`, `identity-picker`, `input`, `input_number`, `list`, `location-picker`, `lookup`, `metadata`, `mutiple-metadata`, `organization-picker`, `percent`, `radio`, `rate`, `richtext`, `signer`, `switch`, `tag`, `textarea`, `time`.
- Expected but not found in the two target lists: `select`, `flowstatus`. Treat these as product-rule-backed/planning only until a focused export proves their exact settings.
- Export-proven primitive FieldType values: `Bigint`, `Bit`, `Datetime`, `Decimal`, `Text`.
- App Creation Rules check: all target fields satisfy FieldIndex/FieldName suffix synchronization, unique DisplayName/FieldName/InternalName, valid InternalName pattern, and identifier length constraints.

## Field Type Matrix

| Field type | Count | Primitive FieldType | Settings keys found | Required examples | Generation use case | Proof |
|---|---:|---|---|---:|---|---|
| `input` | 3 | `Text` | `allowScan`, `encrypt`, `input-maxlength`, `placeholder`, `required` | 2 | short labels, codes, references, names, searchable text | export-proven |
| `textarea` | 2 | `Text` | `placeholder`, `required` | 1 | notes, comments, plain long descriptions | export-proven |
| `richtext` | 4 | `Text` | `required`, `richtext-choose-toolbar`, `richtext-type` | 1 | formatted instructions, agendas, rich descriptions | export-proven |
| `hyperlink` | 2 | `Text` | `hyperlink_buttonname`, `hyperlink_open`, `required` | 1 | external references and clickable links | export-proven |
| `input_number` | 7 | `Decimal` | `displayThousandths`, `number_max`, `number_min`, `number_step`, `placeholder`, `required`, `rounded-to` | 1 | quantities, scores, measurements | export-proven |
| `currency` | 7 | `Decimal` | `currencyCode`, `displayFormat`, `displayThousandths`, `number_max`, `number_min`, `placeholder`, `required`, `rounded-to` | 1 | money amounts with currency display | export-proven |
| `percent` | 2 | `Decimal` | `number_max`, `number_min`, `placeholder`, `required`, `rounded-to` | 1 | rates, ratios, completion percentages | export-proven |
| `calculated-column` | 6 | `Bit`, `Datetime`, `Decimal`, `Text` | `calculated`, `calculated_result` | 0 | derived values from other fields | export-proven |
| `rate` | 4 | `Decimal` | `icon`, `rate-allowHalf`, `rate-count`, `rate-type`, `required` | 1 | user ratings or quality scores | export-proven |
| `switch` | 2 | `Bit` | `categoryId`, `displayStyle`, `source` | 0 | yes/no flags | export-proven |
| `checkbox` | 2 | `Text` | `choices`, `color_choices`, `displayStyle`, `placeholder`, `required` | 0 | multi-select fixed choices | export-proven |
| `radio` | 4 | `Text` | `choices`, `color_choices`, `displayStyle`, `placeholder`, `required`, `show_color` | 2 | single-select fixed choices | export-proven |
| `select` | 0 | not found | unproven | 0 | single-select dropdown when export-proven | unproven in this export |
| `tag` | 1 | `Text` | `category`, `customTags`, `multiple`, `required`, `source` | 1 | managed tag classification | export-proven |
| `datepicker` | 8 | `Datetime` | `date_picker`, `date_type`, `dateformat`, `disable_now_button`, `minuteStep`, `placeholder`, `required`, `showtime` | 1 | dates or date-time values | export-proven |
| `time` | 3 | `Datetime` | `dateformat`, `minuteStep`, `placeholder`, `required` | 1 | time-of-day values | export-proven |
| `identity-picker` | 4 | `Text` | `identity-maxselection`, `multiple`, `required` | 3 | users/requesters/owners | export-proven |
| `organization-picker` | 5 | `Text` | `identity-maxselection`, `metadata-treeselect`, `multiple`, `parentId`, `required` | 1 | departments or org units | export-proven |
| `cost-center-picker` | 1 | `Text` | `identity-maxselection`, `multiple`, `required` | 1 | cost center ownership/accounting | export-proven |
| `signer` | 1 | `Text` | `placeholder`, `required` | 1 | signature capture | export-proven |
| `file-upload` | 3 | `Text` | `file_multiple`, `file_types`, `file_typeslimit`, `maxsize`, `required`, `ver` | 2 | attachments and documents | export-proven |
| `icon-upload` | 3 | `Text` | `controlmultiple`, `picture_size_limit`, `required` | 1 | images or icons | export-proven |
| `lookup` | 6 | `Text` | `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `multiple`, `placeholder`, `required`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | 2 | reference to another list record | export-proven |
| `metadata` | 3 | `Bigint` | `categoryId`, `metadata-treeselect`, `parentId`, `placeholder`, `required`, `source` | 2 | single metadata taxonomy value | export-proven |
| `mutiple-metadata` | 2 | `Text` | `categoryId`, `max-selection`, `metadata-treeselect`, `placeholder`, `required`, `source` | 2 | multiple metadata taxonomy values | export-proven |
| `location-picker` | 3 | `Text` | `identity-maxselection`, `parentId`, `placeholder`, `required` | 2 | location taxonomy values | export-proven |
| `flowstatus` | 0 | not found | unproven | 0 | workflow/list status where system-proven | unproven in this export |
| `autonumber` | 1 | `Text` | `minDigits`, `prefix`, `startNum`, `suffix` | 0 | generated record numbers | export-proven |
| `list` | 1 | `Text` | `list-variables`, `required` | 1 | nested line items/sub-list details | export-proven |

## Field Families

### Text & Input

- `input`: export-proven with `Text` primitive storage and settings `allowScan`, `encrypt`, `input-maxlength`, `placeholder`, `required`. Use for short labels, codes, references, names, searchable text.
- `textarea`: export-proven with `Text` primitive storage and settings `placeholder`, `required`. Use for notes, comments, plain long descriptions.
- `richtext`: export-proven with `Text` primitive storage and settings `required`, `richtext-choose-toolbar`, `richtext-type`. Use for formatted instructions, agendas, rich descriptions.
- `hyperlink`: export-proven with `Text` primitive storage and settings `hyperlink_buttonname`, `hyperlink_open`, `required`. Use for external references and clickable links.

### Numeric & Financial

- `input_number`: export-proven with `Decimal` primitive storage and settings `displayThousandths`, `number_max`, `number_min`, `number_step`, `placeholder`, `required`, `rounded-to`. Use for quantities, scores, measurements.
- `currency`: export-proven with `Decimal` primitive storage and settings `currencyCode`, `displayFormat`, `displayThousandths`, `number_max`, `number_min`, `placeholder`, `required`, `rounded-to`. Use for money amounts with currency display.
- `percent`: export-proven with `Decimal` primitive storage and settings `number_max`, `number_min`, `placeholder`, `required`, `rounded-to`. Use for rates, ratios, completion percentages.
- `calculated-column`: export-proven with `Bit`, `Datetime`, `Decimal`, `Text` primitive storage and settings `calculated`, `calculated_result`. Use for derived values from other fields.
- `rate`: export-proven with `Decimal` primitive storage and settings `icon`, `rate-allowHalf`, `rate-count`, `rate-type`, `required`. Use for user ratings or quality scores.

### Selection & Choice

- `switch`: export-proven with `Bit` primitive storage and settings `categoryId`, `displayStyle`, `source`. Use for yes/no flags.
- `checkbox`: export-proven with `Text` primitive storage and settings `choices`, `color_choices`, `displayStyle`, `placeholder`, `required`. Use for multi-select fixed choices.
- `radio`: export-proven with `Text` primitive storage and settings `choices`, `color_choices`, `displayStyle`, `placeholder`, `required`, `show_color`. Use for single-select fixed choices.
- `select`: not found; keep product/user-understanding-backed only. Use for single-select dropdown when export-proven.
- `tag`: export-proven with `Text` primitive storage and settings `category`, `customTags`, `multiple`, `required`, `source`. Use for managed tag classification.

### Date & Time

- `datepicker`: export-proven with `Datetime` primitive storage and settings `date_picker`, `date_type`, `dateformat`, `disable_now_button`, `minuteStep`, `placeholder`, `required`, `showtime`. Use for dates or date-time values.
- `time`: export-proven with `Datetime` primitive storage and settings `dateformat`, `minuteStep`, `placeholder`, `required`. Use for time-of-day values.

### Identity & Organization

- `identity-picker`: export-proven with `Text` primitive storage and settings `identity-maxselection`, `multiple`, `required`. Use for users/requesters/owners.
- `organization-picker`: export-proven with `Text` primitive storage and settings `identity-maxselection`, `metadata-treeselect`, `multiple`, `parentId`, `required`. Use for departments or org units.
- `cost-center-picker`: export-proven with `Text` primitive storage and settings `identity-maxselection`, `multiple`, `required`. Use for cost center ownership/accounting.
- `signer`: export-proven with `Text` primitive storage and settings `placeholder`, `required`. Use for signature capture.

### Uploads & Media

- `file-upload`: export-proven with `Text` primitive storage and settings `file_multiple`, `file_types`, `file_typeslimit`, `maxsize`, `required`, `ver`. Use for attachments and documents.
- `icon-upload`: export-proven with `Text` primitive storage and settings `controlmultiple`, `picture_size_limit`, `required`. Use for images or icons.

### Advanced & System

- `lookup`: export-proven with `Text` primitive storage and settings `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `multiple`, `placeholder`, `required`, `search-fields`, `search-scope`, `sort-first`, `sort-second`. Use for reference to another list record.
- `metadata`: export-proven with `Bigint` primitive storage and settings `categoryId`, `metadata-treeselect`, `parentId`, `placeholder`, `required`, `source`. Use for single metadata taxonomy value.
- `mutiple-metadata`: export-proven with `Text` primitive storage and settings `categoryId`, `max-selection`, `metadata-treeselect`, `placeholder`, `required`, `source`. Use for multiple metadata taxonomy values.
- `location-picker`: export-proven with `Text` primitive storage and settings `identity-maxselection`, `parentId`, `placeholder`, `required`. Use for location taxonomy values.
- `flowstatus`: not found; keep product/user-understanding-backed only. Use for workflow/list status where system-proven.
- `autonumber`: export-proven with `Text` primitive storage and settings `minDigits`, `prefix`, `startNum`, `suffix`. Use for generated record numbers.
- `list`: export-proven with `Text` primitive storage and settings `list-variables`, `required`. Use for nested line items/sub-list details.

## Field Inventory

| Source list | Field placeholder | Type | FieldType | FieldIndex | FieldName | InternalName | Required | Unique | Settings summary | Proof |
|---|---|---|---|---:|---|---|---|---|---|---|
| Data list with fields part A | __DISPLAY_NAME_1__ | `input` | `Text` | 1 | `Title` | __INTERNAL_NAME_1__ | no | no | - | export-proven |
| Data list with fields part A | __DISPLAY_NAME_2__ | `input` | `Text` | 1 | `Text1` | __INTERNAL_NAME_2__ | yes | no | `input-maxlength`, `placeholder`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_3__ | `input` | `Text` | 2 | `Text2` | __INTERNAL_NAME_3__ | yes | yes | `allowScan`, `placeholder`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_4__ | `input` | `Text` | 3 | `Text3` | __INTERNAL_NAME_4__ | no | no | `encrypt`, `placeholder` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_5__ | `textarea` | `Text` | 4 | `Text4` | __INTERNAL_NAME_5__ | no | no | `placeholder` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_6__ | `textarea` | `Text` | 5 | `Text5` | __INTERNAL_NAME_6__ | yes | no | `placeholder`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_7__ | `richtext` | `Text` | 6 | `Text6` | __INTERNAL_NAME_7__ | no | no | - | export-proven |
| Data list with fields part A | __DISPLAY_NAME_8__ | `richtext` | `Text` | 7 | `Text7` | __INTERNAL_NAME_8__ | no | no | `richtext-type` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_9__ | `richtext` | `Text` | 8 | `Text8` | __INTERNAL_NAME_9__ | no | no | `richtext-type` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_10__ | `richtext` | `Text` | 9 | `Text9` | __INTERNAL_NAME_10__ | yes | no | `required`, `richtext-choose-toolbar`, `richtext-type` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_11__ | `input_number` | `Decimal` | 1 | `Decimal1` | __INTERNAL_NAME_11__ | no | no | `placeholder` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_12__ | `input_number` | `Decimal` | 2 | `Decimal2` | __INTERNAL_NAME_12__ | no | yes | - | export-proven |
| Data list with fields part A | __DISPLAY_NAME_13__ | `input_number` | `Decimal` | 3 | `Decimal3` | __INTERNAL_NAME_13__ | no | no | `displayThousandths` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_14__ | `input_number` | `Decimal` | 4 | `Decimal4` | __INTERNAL_NAME_14__ | no | no | `displayThousandths` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_15__ | `input_number` | `Decimal` | 5 | `Decimal5` | __INTERNAL_NAME_15__ | no | no | `displayThousandths` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_16__ | `input_number` | `Decimal` | 6 | `Decimal6` | __INTERNAL_NAME_16__ | yes | no | `required`, `rounded-to` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_17__ | `input_number` | `Decimal` | 7 | `Decimal7` | __INTERNAL_NAME_17__ | no | no | `number_max`, `number_min`, `number_step` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_18__ | `percent` | `Decimal` | 8 | `Decimal8` | __INTERNAL_NAME_18__ | yes | no | `number_max`, `number_min`, `placeholder`, `required`, `rounded-to` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_19__ | `percent` | `Decimal` | 9 | `Decimal9` | __INTERNAL_NAME_19__ | no | no | - | export-proven |
| Data list with fields part A | __DISPLAY_NAME_20__ | `currency` | `Decimal` | 10 | `Decimal10` | __INTERNAL_NAME_20__ | no | no | `currencyCode`, `displayFormat`, `placeholder` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_21__ | `currency` | `Decimal` | 11 | `Decimal11` | __INTERNAL_NAME_21__ | no | no | `currencyCode`, `displayFormat` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_22__ | `currency` | `Decimal` | 12 | `Decimal12` | __INTERNAL_NAME_22__ | no | no | `currencyCode`, `displayFormat` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_23__ | `currency` | `Decimal` | 13 | `Decimal13` | __INTERNAL_NAME_23__ | no | no | `currencyCode`, `displayFormat`, `displayThousandths` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_24__ | `currency` | `Decimal` | 14 | `Decimal14` | __INTERNAL_NAME_24__ | no | no | `currencyCode`, `displayFormat`, `displayThousandths` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_25__ | `currency` | `Decimal` | 15 | `Decimal15` | __INTERNAL_NAME_25__ | no | no | `currencyCode`, `displayFormat`, `displayThousandths`, `rounded-to` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_26__ | `currency` | `Decimal` | 16 | `Decimal16` | __INTERNAL_NAME_26__ | yes | no | `currencyCode`, `displayFormat`, `number_max`, `number_min`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_27__ | `switch` | `Bit` | 1 | `Bit1` | __INTERNAL_NAME_27__ | no | no | `categoryId`, `source` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_28__ | `switch` | `Bit` | 2 | `Bit2` | __INTERNAL_NAME_28__ | no | no | `displayStyle` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_29__ | `radio` | `Text` | 10 | `Text10` | __INTERNAL_NAME_29__ | no | no | `choices`, `color_choices` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_30__ | `radio` | `Text` | 11 | `Text11` | __INTERNAL_NAME_30__ | yes | no | `choices`, `color_choices`, `required`, `show_color` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_31__ | `radio` | `Text` | 12 | `Text12` | __INTERNAL_NAME_31__ | no | no | `choices`, `color_choices` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_32__ | `radio` | `Text` | 13 | `Text13` | __INTERNAL_NAME_32__ | yes | no | `choices`, `color_choices`, `displayStyle`, `placeholder`, `required`, `show_color` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_33__ | `checkbox` | `Text` | 14 | `Text14` | __INTERNAL_NAME_33__ | no | no | `choices`, `color_choices`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_34__ | `checkbox` | `Text` | 15 | `Text15` | __INTERNAL_NAME_34__ | no | no | `choices`, `color_choices`, `displayStyle`, `placeholder` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_35__ | `datepicker` | `Datetime` | 1 | `Datetime1` | __INTERNAL_NAME_35__ | no | no | `placeholder` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_36__ | `datepicker` | `Datetime` | 2 | `Datetime2` | __INTERNAL_NAME_36__ | no | no | `date_picker` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_37__ | `datepicker` | `Datetime` | 3 | `Datetime3` | __INTERNAL_NAME_37__ | no | no | `date_picker` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_38__ | `datepicker` | `Datetime` | 4 | `Datetime4` | __INTERNAL_NAME_38__ | no | no | `showtime` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_39__ | `datepicker` | `Datetime` | 5 | `Datetime5` | __INTERNAL_NAME_39__ | no | no | `dateformat`, `minuteStep`, `showtime` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_40__ | `datepicker` | `Datetime` | 6 | `Datetime6` | __INTERNAL_NAME_40__ | no | no | `dateformat`, `minuteStep`, `showtime` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_41__ | `datepicker` | `Datetime` | 7 | `Datetime7` | __INTERNAL_NAME_41__ | yes | no | `date_type`, `disable_now_button`, `required`, `showtime` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_42__ | `datepicker` | `Datetime` | 8 | `Datetime8` | __INTERNAL_NAME_42__ | no | no | `date_type`, `showtime` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_43__ | `time` | `Datetime` | 9 | `Datetime9` | __INTERNAL_NAME_43__ | yes | no | `placeholder`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_44__ | `time` | `Datetime` | 10 | `Datetime10` | __INTERNAL_NAME_44__ | no | no | `dateformat`, `minuteStep` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_45__ | `time` | `Datetime` | 11 | `Datetime11` | __INTERNAL_NAME_45__ | no | no | `dateformat` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_46__ | `identity-picker` | `Text` | 16 | `Text16` | __INTERNAL_NAME_46__ | no | no | `identity-maxselection` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_47__ | `identity-picker` | `Text` | 17 | `Text17` | __INTERNAL_NAME_47__ | yes | no | `identity-maxselection`, `multiple`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_48__ | `identity-picker` | `Text` | 18 | `Text18` | __INTERNAL_NAME_48__ | yes | no | `identity-maxselection`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_49__ | `identity-picker` | `Text` | 19 | `Text19` | __INTERNAL_NAME_49__ | yes | no | `identity-maxselection`, `multiple`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_50__ | `organization-picker` | `Text` | 20 | `Text20` | __INTERNAL_NAME_50__ | no | no | `identity-maxselection` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_51__ | `organization-picker` | `Text` | 21 | `Text21` | __INTERNAL_NAME_51__ | yes | no | `identity-maxselection`, `metadata-treeselect`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_52__ | `organization-picker` | `Text` | 22 | `Text22` | __INTERNAL_NAME_52__ | no | no | `identity-maxselection`, `metadata-treeselect` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_53__ | `organization-picker` | `Text` | 23 | `Text23` | __INTERNAL_NAME_53__ | no | no | `identity-maxselection`, `metadata-treeselect` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_54__ | `organization-picker` | `Text` | 24 | `Text24` | __INTERNAL_NAME_54__ | no | no | `identity-maxselection`, `metadata-treeselect`, `multiple`, `parentId` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_55__ | `location-picker` | `Text` | 25 | `Text25` | __INTERNAL_NAME_55__ | yes | no | `identity-maxselection`, `parentId`, `placeholder`, `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_56__ | `location-picker` | `Text` | 26 | `Text26` | __INTERNAL_NAME_56__ | no | no | - | export-proven |
| Data list with fields part A | __DISPLAY_NAME_57__ | `location-picker` | `Text` | 27 | `Text27` | __INTERNAL_NAME_57__ | yes | no | `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_58__ | `file-upload` | `Text` | 28 | `Text28` | __INTERNAL_NAME_58__ | no | no | `ver` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_59__ | `file-upload` | `Text` | 29 | `Text29` | __INTERNAL_NAME_59__ | yes | no | `file_multiple`, `maxsize`, `required`, `ver` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_60__ | `file-upload` | `Text` | 30 | `Text30` | __INTERNAL_NAME_60__ | yes | no | `file_multiple`, `file_types`, `file_typeslimit`, `required`, `ver` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_61__ | `icon-upload` | `Text` | 31 | `Text31` | __INTERNAL_NAME_61__ | no | no | - | export-proven |
| Data list with fields part A | __DISPLAY_NAME_62__ | `icon-upload` | `Text` | 32 | `Text32` | __INTERNAL_NAME_62__ | yes | no | `required` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_63__ | `icon-upload` | `Text` | 33 | `Text33` | __INTERNAL_NAME_63__ | no | no | `controlmultiple`, `picture_size_limit` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_64__ | `lookup` | `Text` | 34 | `Text34` | __INTERNAL_NAME_64__ | yes | no | `addition`, `appid`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `placeholder`, `required`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_65__ | `lookup` | `Text` | 35 | `Text35` | __INTERNAL_NAME_65__ | no | no | `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_66__ | `lookup` | `Text` | 36 | `Text36` | __INTERNAL_NAME_66__ | yes | no | `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `multiple`, `required`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_67__ | `lookup` | `Text` | 37 | `Text37` | __INTERNAL_NAME_67__ | no | no | `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `placeholder`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_68__ | `lookup` | `Text` | 38 | `Text38` | __INTERNAL_NAME_68__ | no | no | `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `multiple`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_69__ | `lookup` | `Text` | 39 | `Text39` | __INTERNAL_NAME_69__ | no | no | `addition`, `appid`, `displayStyle`, `list_tooltip_field`, `listfield`, `listfilter`, `listid`, `listsetid`, `max-selection`, `placeholder`, `search-fields`, `search-scope`, `sort-first`, `sort-second` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_70__ | `calculated-column` | `Decimal` | 17 | `Decimal17` | __INTERNAL_NAME_70__ | no | no | `calculated`, `calculated_result` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_71__ | `calculated-column` | `Text` | 40 | `Text40` | __INTERNAL_NAME_71__ | no | no | `calculated`, `calculated_result` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_72__ | `calculated-column` | `Text` | 41 | `Text41` | __INTERNAL_NAME_72__ | no | no | `calculated`, `calculated_result` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_73__ | `calculated-column` | `Decimal` | 18 | `Decimal18` | __INTERNAL_NAME_73__ | no | no | `calculated`, `calculated_result` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_74__ | `calculated-column` | `Datetime` | 12 | `Datetime12` | __INTERNAL_NAME_74__ | no | no | `calculated`, `calculated_result` | export-proven |
| Data list with fields part A | __DISPLAY_NAME_75__ | `calculated-column` | `Bit` | 3 | `Bit3` | __INTERNAL_NAME_75__ | no | no | `calculated`, `calculated_result` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_1__ | `input` | `Text` | 1 | `Title` | __INTERNAL_NAME_1__ | no | no | - | export-proven |
| Data list with fields part B | __DISPLAY_NAME_2__ | `metadata` | `Bigint` | 1 | `Bigint1` | __INTERNAL_NAME_2__ | no | no | `categoryId`, `source` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_3__ | `metadata` | `Bigint` | 2 | `Bigint2` | __INTERNAL_NAME_3__ | yes | no | `categoryId`, `metadata-treeselect`, `parentId`, `placeholder`, `required`, `source` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_4__ | `metadata` | `Bigint` | 3 | `Bigint3` | __INTERNAL_NAME_4__ | yes | no | `categoryId`, `metadata-treeselect`, `required`, `source` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_5__ | `mutiple-metadata` | `Text` | 1 | `Text1` | __INTERNAL_NAME_5__ | yes | no | `categoryId`, `max-selection`, `metadata-treeselect`, `placeholder`, `required`, `source` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_6__ | `mutiple-metadata` | `Text` | 2 | `Text2` | __INTERNAL_NAME_6__ | yes | no | `categoryId`, `metadata-treeselect`, `placeholder`, `required`, `source` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_7__ | `cost-center-picker` | `Text` | 3 | `Text3` | __INTERNAL_NAME_7__ | yes | no | `identity-maxselection`, `multiple`, `required` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_8__ | `rate` | `Decimal` | 1 | `Decimal1` | __INTERNAL_NAME_8__ | no | no | - | export-proven |
| Data list with fields part B | __DISPLAY_NAME_9__ | `rate` | `Decimal` | 2 | `Decimal2` | __INTERNAL_NAME_9__ | no | no | `rate-type` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_10__ | `rate` | `Decimal` | 3 | `Decimal3` | __INTERNAL_NAME_10__ | no | no | `rate-type` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_11__ | `rate` | `Decimal` | 4 | `Decimal4` | __INTERNAL_NAME_11__ | yes | no | `icon`, `rate-allowHalf`, `rate-count`, `rate-type`, `required` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_12__ | `hyperlink` | `Text` | 4 | `Text4` | __INTERNAL_NAME_12__ | no | no | `hyperlink_open` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_13__ | `hyperlink` | `Text` | 5 | `Text5` | __INTERNAL_NAME_13__ | yes | no | `hyperlink_buttonname`, `hyperlink_open`, `required` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_14__ | `signer` | `Text` | 6 | `Text6` | __INTERNAL_NAME_14__ | yes | no | `placeholder`, `required` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_15__ | `tag` | `Text` | 7 | `Text7` | __INTERNAL_NAME_15__ | yes | no | `category`, `customTags`, `multiple`, `required`, `source` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_16__ | `autonumber` | `Text` | 8 | `Text8` | __INTERNAL_NAME_16__ | no | no | `minDigits`, `prefix`, `startNum`, `suffix` | export-proven |
| Data list with fields part B | __DISPLAY_NAME_17__ | `list` | `Text` | 9 | `Text9` | __INTERNAL_NAME_17__ | yes | no | `list-variables`, `required` | export-proven |

## Export Paths

- Application shell: `Data.Item.ListModel` with child resources under `Data.Childs[]`.
- Target data-list resource: `Data.Childs[]` where `ListModel.Title` matches the target list and `ListModel.Type = 1`.
- Field definitions: `Data.Childs[].Defs[]`.
- Core field metadata: `FieldID`, `ListID`, `FieldName`, `FieldType`, `FieldIndex`, `DisplayName`, `InternalName`, `Type`, `Status`, `Category`, `DefaultValue`, `IsSystem`, `IsUnique`.
- Field settings: stringified JSON in `Defs[].Rules`.
- Sample data: `ListDatas`; not used as runtime proof and not normalized into docs.

## App Creation Rules Interaction

Generation must preserve v0.5.12 gates: FieldName numeric suffix at the end must equal FieldIndex for generated non-system fields, DisplayName/FieldName/InternalName are unique within the same list, InternalName matches `[A-Za-z0-9_]`, and identifier lengths stay <=255. This export also proves single metadata fields use `Type = metadata`, which should be accepted alongside the product-team type list.

## Field-Specific Generation Rules

- Text fields use `TextN` FieldName slots; settings may include `placeholder`, `required`, `input-maxlength`, `allowScan`, `encrypt`, `richtext-type`, and toolbar settings.
- Numeric fields use `DecimalN`; settings include `displayThousandths`, `rounded-to`, `number_min`, `number_max`, `number_step`, `currencyCode`, and `displayFormat`.
- Switch fields use `BitN`; tick-box display uses `displayStyle`, while one export-proven switch also carries metadata-like `source` and `categoryId`.
- Choice fields store options in `Rules.choices`; color data is in `Rules.color_choices`; radio/select-like dropdown behavior uses `displayStyle`; `show_color` controls color display where present.
- Date/time fields use `DatetimeN`; datepicker settings include `showtime`, `dateformat`, `minuteStep`, `date_picker`, `date_type`, and `disable_now_button`. Time fields use `Type = time` with `dateformat` and `minuteStep`.
- Identity, organization, location, and cost center picker fields store as Text, with selection limits in `identity-maxselection`, multi-select in `multiple`, and hierarchy/root constraints through `parentId` or `metadata-treeselect`. Tenant-specific IDs must be redacted and not guessed.
- File and image fields store as Text with upload settings such as `ver`, `maxsize`, `file_multiple`, `file_typeslimit`, `file_types`, `picture_size_limit`, and `controlmultiple`. Runtime storage is not proven.
- Lookup fields require `appid`, `listsetid`, `listid`, and `listfield`; optional settings include tooltip, additions, sorting, filtering, search scope/fields, max selection, display style, placeholder, required, and multiple. Target list and display field should resolve before import.
- Calculated columns require `calculated_result` and `calculated`; result primitives in this export are Decimal, Text, Datetime, and Bit. Referenced fields must exist and use stable internal names/FieldNames.
- Metadata fields use `metadata` + Bigint for single value and `mutiple-metadata` + Text for multiple values; both require `source` and `categoryId`, with optional tree select, parent, placeholder, max selection, required, and default value.
- Tag fields use `Type = tag`, Text storage, `source`, `category`, `customTags`, `multiple`, and `required`.
- Auto number fields use Text storage and `minDigits`, `startNum`, `prefix`, and `suffix`. Do not assume equivalence to approval form `NoRule`; no relation is export-proven here.
- Sub-list fields use `Type = list`, Text storage, and `Rules.list-variables[]`. Nested variable types found: `text`, `number`, `boolean`, `date`, `file`, `metadata`, `user`, `costcenter`, `groupselect`, `location`, `lookup`, `img`, and `mutiple-metadata`. Nested fields do not use top-level FieldIndex/FieldName slots in this export.

## Document Library Applicability

Data List and Document Library custom fields are likely shared or highly similar, but this export has no Type `16` resource. Apply these settings to Document Library only as product/user-understanding-backed guidance until a focused document-library export proves exact `Defs[]` and `Rules` shapes for custom library fields. Keep document-library default fields and upload-file rules from existing Type `16` studies.

## Normalized References

Redacted normalized field refs were created under `docs/studies/normalized/data-list-fields.bundle.json` for every field type found. Files are synthetic-normalized from export shapes; IDs, user/org/category/list values, labels, placeholders, and sample data are redacted.

## Validator Guidance

Hard errors: duplicate DisplayName/FieldName/InternalName, invalid InternalName, FieldIndex/FieldName suffix mismatch, malformed Rules JSON, missing options for generated choice fields, missing required lookup target metadata, and unresolved generated lookup targets where package-local resolution is required.

Warnings: unknown field types, sparse optional settings, metadata/tag source gaps, calculated-column dependency risks, sub-list nested-field gaps, upload/image runtime storage assumptions, tenant-specific picker IDs, Document Library applicability without Type `16` proof, and any field-specific settings not observed in this export.

---

<a id="data-list-document-library-permissions-notifications"></a>

<!-- source: docs/studies/collection-01.md#data-list-document-library-permissions-notifications -->

# Data List And Document Library Permissions And Notifications

Proof boundary: export-proven for Data List permission flags and Data List `RemindRules` notification schema found in `<downloads>/Data Lists (1).yap`; product-documented only for Document Library applicability; Form Report negative rule; not runtime-proven for permission enforcement or notification delivery.

## Overview

This study reopens the Data List permissions and notifications pass after UI review confirmed that the source app contains configured permission examples. The export was decoded read-only into ignored `tmp/` workspace using the repository decoder, preserving large numeric IDs as strings. The original `.yap` was not modified or committed.

The decoded application contains seven `Data.Childs[]` Type `1` Data List resources and no Type `16` Document Library resources. The five target lists were found:

| List | Export path | Export-located permission fields | Detailed audience matrix |
| --- | --- | --- | --- |
| Event Planning | `Data.Childs[0]` | `ListModel.Perm = 4`, `IsBreakInherit = false`, `IsItemPerm = true`; every view has `IsItemPerm = false` | UI-confirmed, not found in package payload |
| Event planning 2 | `Data.Childs[2]` | `ListModel.Perm = 4`, `IsBreakInherit = false`, `IsItemPerm = true`; every view has `IsItemPerm = false` | UI-confirmed, not found in package payload |
| Event planning 3 | `Data.Childs[3]` | `ListModel.Perm = 4`, `IsBreakInherit = false`, `IsItemPerm = true`; every view has `IsItemPerm = false` | UI-confirmed, not found in package payload |
| Event planning 4 | `Data.Childs[4]` | `ListModel.Perm = 4`, `IsBreakInherit = false`, `IsItemPerm = true`; every view has `IsItemPerm = false` | UI-confirmed, not found in package payload |
| Event planning 5 | `Data.Childs[5]` | `ListModel.Perm = 4`, `IsBreakInherit = false`, `IsItemPerm = true`; every view has `IsItemPerm = false` | UI-confirmed, not found in package payload |

## Scope And Resource Applicability

Data List and Document Library manage-permission and custom-notification features are product-documented for Data Lists and Document Libraries. The two Help Center references reviewed were:

- Data List / Document Library permissions: `https://<yourdomain>.yeeflow.com/<redacted-path>
- Data List / Document Library notifications: `https://<yourdomain>.yeeflow.com/<redacted-path>

This export proves the Data List package shape for the located fields. It does not include a Type `16` Document Library, so Document Library schema remains product-documented only in this pass. Form Report is not a Data List or Document Library for these settings: prior Form Report learning covers report access, export permission, and detail-view access only. Do not generate Data List / Document Library manage-permission or custom-notification settings on Form Reports.

## Data List Permissions Findings

The deep permission search covered:

- wrapper keys: `Title`, `Description`, `IconUrl`, `IsListSet`, `Resource`
- decoded resource keys: `MainListType`, `AppID`, `ReplaceIds`, `ReportIds`, `FormKeys`, `Data`, `SimplePortal`
- `Data.Item`, `Data.Childs[]`, `ListModel`, `Defs`, `Layouts`, `PublicForms`, `RemindRules`, `FlowMappings`, `ListDatas`
- parsed nested JSON strings in `LayoutView`, `Ext1`, `Rules`, `Receiver`, and layout resources
- broad key/string patterns including `Perm`, `Permission`, `IsItemPerm`, `IsBreakInherit`, `Inherit`, `Administrators`, `Admin`, `Basic`, `Advanced`, `Edit`, `Delete`, `New`, `Import`, `Export`, `View`, `Audience`, `Users`, `Groups`, `Departments`, `UserGroups`, `DataSecurity`, `Security`, `Policy`, `Role`, `ACL`, `Access`, `Auth`, `Guest List`, and `Created By`

Export-located permission fields:

- `Data.Childs[].ListModel.Perm` is numeric; all five target lists use `4`.
- `Data.Childs[].ListModel.IsBreakInherit` is boolean; all five target lists use `false`.
- `Data.Childs[].ListModel.IsItemPerm` is boolean; all five target lists use `true`.
- `Data.Childs[].ListModel.AdvanceList` is an empty array in all five target lists.
- `Data.Childs[].ListModel.Ext1`, `Ext2`, and `Ext3` are `null` in all five target lists.
- `Data.Childs[].Layouts[].IsItemPerm` is `false` on every data-view layout in the five target lists.
- `Data.Childs[].Layouts[].Perm` is absent.

Not export-located after deep search:

- administrators audience array
- Edit Item / View Item all-users versus specified-users selection
- selected user, department/team, user-group, or field/list-field audience rows for basic permissions
- advanced permission matrix rows
- advanced operation booleans for edit, delete, new, import, and export

Interpretation:

- UI evidence confirms the configured permission examples exist in the live app. This study does not treat screenshots as export schema.
- The `.yap` package appears to carry the coarse permission flags and view-level `IsItemPerm`, but not the detailed manage-permission audience payload.
- Validators should parse and warn on the located flags. They should not invent or require detailed audience blocks from this export.
- A focused export or API-backed read-only permission export is still needed to promote administrator/basic/advanced audience schema to export-proven.

## Data List Notifications Findings

`Event planning 5` contains nine notification rules at `Data.Childs[5].RemindRules[]`. The app also contains two more `RemindRules` in `Project tasks`, but the targeted notification study uses `Event planning 5`.

Export-located rule fields:

- `TenantID`, `ID`, `CategoryID`, `AppID`, `ListID`
- `Title`
- `SendType`
- `Type`
- `From`
- `Subject`
- `Content`
- `Rules` as stringified JSON
- `Receiver` as stringified JSON
- `Status`
- created/modified audit fields

Notification type codes found in `Event planning 5`:

| Type code | Meaning inferred from rule shape and UI label | Count | Export notes |
| --- | --- | ---: | --- |
| `1` | item added | 1 | `Rules.Rules.Period = "Daily"`, empty conditions |
| `4` | item changed | 1 | `Rules.Rules.Type = 2`, `Rules.Rules.Fields[]`, condition data with `left`, `op`, `right` |
| `3` | date-field reminder | 2 | `Duedate`, `Day`, `IsCheck`, `Time`, optional `Type = "before"` |
| `2` | regular reminder | 5 | daily, weekly, monthly, and yearly schedules via `Period`, date ranges, day/month fields, and `Time` |

Recipient shapes found:

- explicit user recipient: `Receiver.Identities[].Type = 1`
- explicit department/team recipient: `Receiver.Identities[].Type = 2`
- explicit user-group recipient: `Receiver.Identities[].Type = 3`
- list-field recipient: `Receiver.ListDefs[]`, for example the internal list field name for the guest-list identity picker

Message/template fields:

- `Subject` and `Content` are HTML strings.
- Field tokens appear as HTML `input` placeholders with JSON-like `data` expressions.
- Content can include list item field tokens and list URL tokens such as `viewURL` and `editURL`.
- `Status = 1` appears on all nine rules and is treated as enabled in this export.

Condition findings:

- Empty condition is represented as `Rules.Conditions = { "Type": 1, "Data": "" }`.
- A fixed condition sample appears on the item-changed rule with `Conditions.Type = 2` and `Conditions.Data` as stringified JSON array.
- The condition item reuses the data-view-like flat condition shape: `key`, `pre`, `left`, `op`, `right`, `group`.

## Document Library Applicability

No Type `16` Document Library resources are present in `Data Lists (1).yap`. Product documentation describes Data List and Document Library permissions/notifications together, but this pass did not export-prove a Document Library package schema for manage-permission or notification settings.

Generator and validator guidance should therefore say:

- Data List notification `RemindRules` are export-proven from this package.
- Document Library permission/notification concepts are product-documented only in this pass.
- Do not clone Data List notification or permission package fields into Document Library generation until a Type `16` export proves the exact shape, unless explicitly marked product-documented and runtime-sensitive.

## Form Report Negative Rule

Form Reports do not support the same Data List / Document Library manage-permission and custom-notification feature set. Keep Form Report permission handling limited to the prior Form Report study:

- report access behavior
- export permission toggle
- detail-page access flag
- view-level settings for Type `32` list-like resources

Do not add Data List / Document Library `RemindRules` custom notifications or manage-permission matrix guidance to Form Report generation.

## Export Paths And Schema Notes

Permission paths found:

- `Data.Childs[n].ListModel.Perm`
- `Data.Childs[n].ListModel.IsBreakInherit`
- `Data.Childs[n].ListModel.IsItemPerm`
- `Data.Childs[n].ListModel.AdvanceList`
- `Data.Childs[n].Layouts[m].IsItemPerm`

Notification paths found:

- `Data.Childs[5].RemindRules[]`
- `Data.Childs[5].RemindRules[].Rules` as stringified JSON
- `Data.Childs[5].RemindRules[].Receiver` as stringified JSON
- `Rules.Rules.Period`, `StartTime`, `EndTime`, `WeekDay`, `MonthDay`, `YearMonth`, `Duedate`, `Type`, `Day`, `IsCheck`, `Time`, `Fields[]`
- `Rules.Conditions.Type`
- `Rules.Conditions.Data` as empty string or stringified condition-array JSON
- `Receiver.Identities[]`
- `Receiver.ListDefs[]`

## Validator Rules

Warning-first checks added or expected:

- parse list permission flags where present: `Perm`, `IsBreakInherit`, `IsItemPerm`
- warn if `AdvanceList` is present but not an array
- warn if `ListModel.Ext1/Ext2/Ext3` contain permission-like stringified JSON that cannot be parsed
- parse `RemindRules[]`
- parse each notification `Rules` JSON string
- parse each notification `Receiver` JSON string
- recognize notification type codes `1`, `2`, `3`, and `4`
- recognize recipient identity type codes `1`, `2`, and `3`
- recognize list-field recipients through `Receiver.ListDefs[]`
- warn on unknown notification types, unknown recipient types, invalid condition JSON, or unknown schedule shapes

Do not hard-fail compatibility packages for missing detailed audience matrices, because this export does not include one.

## Remaining Unproven Gaps

- Administrator audience schema is UI-confirmed but not export-located.
- Basic permission edit/view all-users versus specified-users schema is UI-confirmed but not export-located.
- Advanced permission matrix row schema and edit/delete/new/import/export operation booleans are UI-confirmed but not export-located.
- Field/list-field permission audience rows are UI-confirmed but not export-located.
- Document Library permission and notification package schema is product-documented only in this pass.
- Permission enforcement and notification delivery are not runtime-proven.

---

<a id="data-list-field-creation-runtime-proof"></a>

<!-- source: docs/studies/collection-01.md#data-list-field-creation-runtime-proof -->

# Data List Field Creation Runtime Proof

## Scope

This focused runtime baseline validates a generated Data List package after the Data List / Document Library field-type learning milestone. It proves import/open/field-creation behavior for a representative Data List subset only. It does not exhaustively test all 27 export-proven field types, does not test Document Library custom fields, and does not test Form Report.

## Package

- Generator: `tools/generators/generate-data-list-field-creation-runtime-proof.mjs`
- Local package: `data-list-field-creation-runtime-proof.v1.yap`
- Import copy: `<downloads>/data-list-field-creation-runtime-proof.v1.yap`
- App: `Data List Field Creation Runtime Proof`
- Runtime site: `https://<yourdomain>.yeeflow.com/`

Generated resources:

- `Field Creation Runtime Test`
- `Lookup Source Runtime Test`

Representative field subset:

| Field type | Included field | Runtime proof level |
| --- | --- | --- |
| `input` | Runtime Record Name | import/open/visible |
| `textarea` | Runtime Notes | import/open, not grid-visible because textarea grid column remains unproven |
| `input_number` | Runtime Quantity | import/open/visible |
| `currency` | Runtime Budget | import/open/visible |
| `percent` | Runtime Completion Percent | import/open/visible |
| `switch` | Runtime Approved Flag | import/open/visible |
| `checkbox` | Runtime Choice | import/open/visible |
| `datepicker` | Runtime Due Date | import/open/visible |
| `time` | Runtime Due Time | import/open/visible |
| `identity-picker` | Runtime Owner | import/open/visible |
| `organization-picker` | Runtime Department | import/open/visible |
| `file-upload` | Runtime Attachment | import/open/visible |
| `icon-upload` | Runtime Image | import/open/visible |
| `lookup` | Runtime Lookup | import/open/schema-validated; lookup value resolution not tested |
| `calculated-column` | Runtime Total Estimate | import/open/schema-validated; formula runtime correctness not tested |
| `list` | Runtime Sub List | import/open/schema-validated; nested data-entry behavior not tested |

Metadata fields were intentionally omitted from this runtime package because stable tenant/category taxonomy values were not safe to generate without a focused reference.

## Local Validation

All local validation was run against `data-list-field-creation-runtime-proof.v1.yap`.

| Check | Result |
| --- | --- |
| Generator syntax check | pass |
| `scripts/inspect-data-list-fields.mjs` | pass, 0 errors, 0 warnings |
| `scripts/inspect-app-creation-rules.mjs` | pass, 0 errors, 0 warnings |
| `validate-yap-package.js --mode generator --stage final` | pass with warnings, 0 errors |
| `validate-yap-graph.js --mode generator --stage final` | pass with warnings, 0 errors |

The package preserves the v0.5.12 app-creation gates: `FieldIndex` / `FieldName` suffix synchronization, unique `DisplayName`, `FieldName`, and `InternalName`, valid `InternalName`, and valid local lookup references.

Warnings are retained for runtime-sensitive field families such as percent, checkbox, time, identity/organization pickers, upload fields, calculated columns, and sub-lists.

## Runtime Result

Runtime import/open/field-creation proof was completed in Yeeflow.

| Step | Result | Proof label |
| --- | --- | --- |
| Import package | Succeeded | runtime-import-proven |
| Open app | Succeeded as `Data List Field Creation Runtime Proof` | runtime-open-proven |
| Open `Field Creation Runtime Test` | Succeeded | data-list-open-proven |
| Confirm representative columns | Visible columns included record name, number, currency, percent, switch, checkbox, date, time, user, department, attachment, and image fields | field-visibility-proven for visible subset |
| Open `+ New column` | Succeeded | field-creation-panel-proven |
| Save additional single-line field | Saved `Runtime Extra Field` | data-list-field-creation-proven |
| Duplicate-value error | Did not appear | runtime-regression-not-observed |

Yeeflow displayed `Added Successfully` after saving `Runtime Extra Field`.

## Proof Boundary

Runtime-proven:

- Generated package imports.
- App opens.
- Data List resource opens.
- Representative Data List columns render visibly for the focused subset listed above.
- A new single-line field can be added after import.
- The previous duplicate-value error associated with `FieldIndex` / `FieldName` mismatch did not appear.

Validator-backed:

- Lookup field references a generated local source list and source field.
- Calculated column references generated earlier numeric fields.
- Sub-list nested variables are structurally valid and unique.

Not runtime-proven:

- Exhaustive runtime behavior for all 27 export-proven field types.
- Record data-entry behavior for the generated fields.
- Lookup value resolution.
- Calculated formula result correctness.
- Attachment upload behavior.
- Image upload behavior.
- User/department picker selection behavior.
- Sub-list row data entry.
- Metadata or multiple metadata generation.
- Document Library custom-field behavior.
- Workflow behavior.
- Form Report behavior.

## Next Step

Review and merge this branch, then decide whether to run a second focused runtime proof for the higher-risk advanced fields that were intentionally not exercised here, especially metadata, multiple metadata, lookup runtime value resolution, calculated result correctness, and sub-list row entry.

---

<a id="data-list-layoutview-add-form-runtime-fix"></a>

<!-- source: docs/studies/collection-01.md#data-list-layoutview-add-form-runtime-fix -->

# Data List LayoutView Add Form Runtime Fix

## Issue

The generated package `container-button-action-runtime-proof.v1.yap` imported and opened, but the `Action Runtime Requests` Data List default `+ New item` button opened an Add modal that kept loading and never rendered the form.

Product feedback identified the cause as an incorrect `ListInfo.LayoutView` / `ListModel.LayoutView` shape for the generated Data List.

## Evidence

Broken generated shape, redacted:

```json
{
  "opentype": { "add": "modal" },
  "modalsize": { "add": 1 },
  "sort": [
    { "SortName": "Created", "SortByDesc": true }
  ]
}
```

The broken list had only one Type `0` view layout and no Type `1` New/Edit/View form layouts. `LayoutView.add`, `LayoutView.edit`, and `LayoutView.view` were missing, so the default New Item modal had no concrete add-form target.

Product-provided working comparison used an object with concrete layout references, redacted:

```json
{
  "add": "__ADD_FORM_LAYOUT_ID__",
  "edit": "default",
  "opentype": { "view": "new" },
  "sort": [
    [
      "__FIELD_ID__",
      "__FIELD_ID__"
    ]
  ],
  "view": "__VIEW_FORM_LAYOUT_ID__"
}
```

The exact IDs are package-local generated IDs and must resolve inside the package.

## Root Cause

The runtime-proof generator assumed the source package already contained usable custom form layouts. The source template only had a default Type `0` list view. The generator used optional chaining for `add/edit/view`, so `JSON.stringify` omitted those keys when no form layouts existed.

The generated Data List also put `[{ SortName, SortByDesc }]` into `ListModel.LayoutView.sort`. That object shape is widely used in Type `0` view layout JSON, but product comparison indicates Data List display settings expect either an export-shaped field-ID array or no sort setting.

## Fix

`tools/generators/generate-container-button-action-runtime-proof.mjs` now creates concrete Type `1` Data List form layouts:

- `New Item`
- `Edit Item`
- `View Item`

It assigns `ListModel.LayoutView.add/edit/view` to those generated layout IDs, keeps `opentype.add = "modal"` and `modalsize.add = 1`, and omits unsafe display-settings `sort`.

## Validator Hardening

Generated-final validation now fails when:

- `ListModel.LayoutView.add` is missing or resolves to `default`
- `LayoutView.add/edit/view` references do not resolve to local Type `1` layouts
- Data List display-settings `sort` is not an export-supported array shape
- Data List display-settings `sort` uses object entries such as `SortName` / `SortByDesc`

The old broken package now fails with:

- `LAYOUTVIEW_ADD_LAYOUT_MISSING`
- `LAYOUTVIEW_SORT_OBJECT_UNSUPPORTED`

## Regenerated Package

Manual-test package:

`<downloads>/container-button-action-runtime-proof.layoutview-fixed.v1.yap`

Safe local inspection confirms:

- `LayoutView.add` resolves
- `LayoutView.edit` resolves
- `LayoutView.view` resolves
- Type `1` New/Edit/View layouts have embedded resources
- display-settings `sort` is omitted

## Local Proof

Local validation passed with zero errors:

- `validate-yap-package.js`: 0 errors
- `validate-yap-graph.js`: 0 errors
- `scripts/inspect-yap-materialization.mjs`: 0 errors, 0 warnings
- `scripts/inspect-data-list-custom-forms.mjs`: 0 findings
- `scripts/inspect-data-list-fields.mjs`: 0 findings
- `scripts/inspect-container-button-actions.mjs`: 0 findings
- `scripts/inspect-yap-import-readiness.mjs`: 0 errors
- wrapper decode/re-encode round trip: pass

Existing warnings are proof-boundary warnings for unproven field/control behavior and the intentionally minimal approval workflow, not LayoutView failures.

## User-Confirmed Runtime Proof

Runtime test date: 2026-05-27

Generated package tested:

`<downloads>/container-button-action-runtime-proof.layoutview-fixed.v1.yap`

User-confirmed runtime result:

- the fixed package imported and was tested in Yeeflow
- the Data List `Action Runtime Requests` opened
- the default `+ New item` button was clicked
- the Add modal rendered successfully
- the previous infinite loading issue is fixed for this generated package

Runtime-proven for this focused generated package:

- `ListModel.LayoutView.add` resolving to a concrete Type `1` form layout fixes the default Data List New Item modal loading failure
- concrete generated Type `1` `New Item`, `Edit Item`, and `View Item` layouts are accepted for this list
- omitting object-shaped display-settings `sort` avoids the broken `ListModel.LayoutView` shape observed in the failing package

Not proven by this test:

- Add form save or record data mutation
- exhaustive behavior for every field/control in the form
- other open modes or modal sizes
- public forms
- Document Library layouts
- Form Report layouts
- arbitrary generated application patterns outside this focused Container/Button action runtime package

## Proof Boundary

Runtime Add modal rendering is user-confirmed for this generated fixed Container/Button action runtime package. Add form save/data mutation and broader Data List, Public Form, Document Library, Form Report, and unrelated generated-app layout behavior remain unproven unless separately tested.

---

<a id="data-list-public-form-runtime-proof"></a>

<!-- source: docs/studies/collection-01.md#data-list-public-form-runtime-proof -->

# Data List Public Form Runtime Proof

## Scope

Branch:

`codex/data-list-public-form-runtime-proof`

Generated package:

`data-list-public-form-runtime-proof.v1.yap`

Downloads copy:

`/Users/Renger/Downloads/data-list-public-form-runtime-proof.v1.yap`

Package strategy:

Focused generated package based on the learned Data List Public Form schema from `Data Lists (4).yap`, not the full source export.

App:

`Data List Public Form Runtime Proof`

Target data list:

`Public Form Runtime Test`

Public form:

`Runtime Public Form`

## Representative Public Form Contents

The generated package includes one Data List with one Public Form stored under `Data.Childs[].PublicForms[]`. The Public Form `Resource` is a JSON string with `pagetype: 3`, `ver: 2`, `attrs`, `children`, and `tempVars`.

Representative list-bound field controls:

- `Title` primary-field exception / `input`
- single line / `input`
- multiple line / `textarea`
- number / `input_number`
- percent / `percent`
- switch / `switch`
- radio / `radio`
- date / `datepicker`
- time / `time`
- hyperlink / `hyperlink`

Controls and layout:

- container
- `flex_grid`
- centered submit container
- `submit-button`

The package intentionally avoids default/system fields other than the `Title` primary-field exception, login-dependent fields, lookup/reference fields, upload/image/signature behavior, and sub-list row entry.

## Local Validation

Validation was run on the generated package before manual import testing.

Results:

- JS syntax check: pass
- Public form inspector: pass, 0 errors, 0 warnings
- Data-list field inspector: pass, 0 errors, 0 warnings
- App-creation rules inspector: pass, 0 errors, 0 warnings
- YAP schema-standard inspector: pass, 0 errors, 0 warnings
- Data-view inspector: pass
- Package validator: pass with warnings only, 0 errors
- Graph validator: pass with warnings only, 0 errors

Historical proof note: this focused runtime package omitted standard custom New/Edit/View forms because it isolated Public Form import/open behavior. That omission is no longer acceptable for newly generated full applications or standalone Data Lists. Current generation must preserve the Public Form proof while also producing Data List Form Layouts v1.1 New/Edit/View forms and resolved `List.LayoutView.add/edit/view` routing.

## User-Proven Runtime Result

The user manually imported the generated package into Yeeflow and confirmed:

- package import succeeded
- app opened
- target data list opened
- the Public Form exists inside the data list
- Public Form designer opened
- Public Form controls rendered
- no unsupported field/control error was reported
- no missing field binding error was reported

The first imported package revealed designer usability issues:

- the generated grid control used a thin grid attrs shape, which caused the grid Appearance settings in the designer not to expand correctly
- the grid displayed a caption, but the desired Public Form layout should turn the caption off
- the submit button was full-width/inside the grid rather than inline and centered

The generator was updated and the package was regenerated. The fixed package:

- uses the export-proven Public Form `flex_grid` shape with `ver: 1`, structured `columns`, `rows`, `cgap`, and `cgapU`
- sets grid caption display off with `displayLabel: [null, false]`
- places the submit button in a separate container below the grid
- centers the submit container with row direction, centered alignment, and centered justification
- sets submit button width to the export-proven inline width shape `common.positioning.widthtype: [null, "2"]`

The user manually imported the regenerated package and confirmed that the test passed.

## Share URL Redaction

No public share URL is recorded in this document. Public share URLs and share codes must remain redacted in docs, logs, normalized refs, and final reports.

Use this placeholder if a share URL must be documented later:

`https://share.yeeflow.com/f/<REDACTED_PUBLIC_FORM_CODE>`

## Proof Boundary

Runtime-proven for this generated representative Data List Public Form package:

- package import
- app open
- target data list open
- Public Form presence inside the data list
- Public Form designer open
- representative allowed list-bound control rendering
- grid layout rendering after the export-shaped grid fix
- submit button rendering as inline and centered after the container fix
- no observed unsupported field/control error
- no observed missing field binding error

Not proven:

- anonymous submit
- real public data collection
- public share/open outside authenticated designer unless separately confirmed
- file/image upload
- signature capture
- lookup/runtime reference resolution
- sub-list data entry
- email/notification behavior
- Document Library public form behavior
- Form Report behavior

Proof labels:

- Data List Public Form schema: export-proven from `Data Lists (4).yap`
- Generated package import/open/designer/control-render path: user-proven for this focused package
- Validators: validator-backed
- Public URL anonymous access and submit behavior: not runtime-proven

---

<a id="data-list-public-forms"></a>

<!-- source: docs/studies/collection-01.md#data-list-public-forms -->

# Data List Public Forms

## Scope

Source export:

`/Users/Renger/Downloads/Data Lists (4).yap`

Target data lists:

- `Data list with fields part A`
- `Data list with fields part B`

This is an export-learning pass for Data List Public Forms. Screenshots provided by the user were used as UI reference only and are not committed. No runtime import, public URL open, anonymous submission, or data creation test was run.

Proof boundary:

- Data List Public Forms in this export: export-proven.
- Screenshot control palette availability: UI-reference-backed.
- Validator behavior after this pass: validator-backed.
- Anonymous public URL behavior and submit execution: not runtime-proven.
- Document Library applicability: unproven; no Type `16` document library public-form export was present.
- Form Report: not involved.

## Public Forms Versus Custom List Forms

Data List Public Forms are separate from Custom List Forms. Custom List Forms configure authenticated New/Edit/View item experiences inside the data list UI. Public Forms are anonymous/no-login collection forms intended to be shared publicly, similar in purpose to Google Forms or Microsoft Forms.

Because Public Forms can be submitted without login, they use a restricted field/control palette. Generators must not reuse Custom List Form or Approval Form assumptions blindly.

## Public Form Page Layout Golden Reference

`Customer Satisfaction.ydl` adds the export-proven `Public form page layout standard` public form. This is the default golden reference page layout template for generated Public Forms.

Template resource:

`docs/reference/public-form-page-layout-standard.template.json`

Generation rule:

Every generated Public Form must be plan-first and must declare a page layout template selection. Use `public-form-page-layout-standard` unless the user explicitly provides and approves another Public Form page layout template. Do not hand-build a simplified public form body.

Locked template contract:

- `Resource.pagetype = 3`
- `Resource.ver = 2`
- root Content area uses full width (`attrs.container.cw = "2"`)
- root Content padding remains zero on all sides
- root page background remains the template background
- `public_form_title_wrapper`, `public_form_content_section`, and `pubic_form_bottom_section` keep synchronized custom widths, defaulting to `1280px`
- `pubic_form_submit_button` remains required and stays inside `pubic_form_bottom_section > section_content_center_area`

Editable public-form regions:

- `public_form_title_section`
- `public_form_title_header`
- `public_form_title_text`
- `public_form_description`
- `public_form_title_cta_area`
- `public_form_title_cta_button_primary`
- `public_form_title_cta_button_secondary`
- `public_form_content_section`
- `1_columns_section`
- `2_columns_section`
- `3_columns_section`
- `2_columns_60/40_section`
- `content_card_wrapper`
- `content_card_60_wrapper`
- `content_card_40_wrapper`
- `section_title_header`
- `section_title_text`
- `section_title_description`
- `Operations`
- `section_content_area`
- `pubic_form_bottom_section`
- `section_content_center_area`
- `pubic_form_submit_button`

All business field controls must be placed inside `content_card_wrapper`, `content_card_60_wrapper`, or `content_card_40_wrapper`, specifically inside `section_content_area`. The top-level `public_form_content_section` may only contain approved section layout containers: `1_columns_section`, `2_columns_section`, `3_columns_section`, or `2_columns_60/40_section`.

Generated output must treat those regions as optional choices, not retain the complete example set. Remove `public_form_title_cta_area` when no real CTA is configured; remove copied column sections with no mapped business content; replace retained section placeholder copy with business-specific text; and remove `Operations` when it contains no configured actions. If neither a business-specific `section_title_header` nor configured `Operations` remains, remove the entire `section_title_area`. Standalone `.ydl` and full-application generation use the same cleanup contract.

Public Form field groups may reuse the Data List Form field-layout templates when the fields are public-form-compatible:

- `public_form_fields_1col_v1_1`, rooted at `form_grid_fields_1col_wrapper`
- `data_list_form_fields_grid_v1_1`, rooted at `form_grid_fields_wrapper`
- `data_list_form_control_sublist_v1_1`, rooted at the approved Sub List `list` control

`public_form_fields_1col_v1_1` is the preferred field layout for survey/questionnaire Public Forms, forms with a small number of fields, and forms with long question-style labels. It is a Public Form-specific golden reference learned from `Customer Satisfaction (1).ydl` and `form_grid_fields_1col_wrapper.json`. The root Grid has one column at every responsive breakpoint and its Grid attributes are locked.

When field-layout templates are used in a Public Form, they must be hosted inside an approved Public Form content card's `section_content_area`. Generation may remap labels, bindings, field IDs, option rules, and public-form-compatible field controls, but must keep the template grid/sub-list structure, spacing, zero-margin discipline, responsive columns, and designer labels. Do not use Data List Form grids to sneak login-dependent or public-form-unsupported fields into anonymous Public Forms.

For `public_form_fields_1col_v1_1`, every field cell must be a `form_grid_field_container` containing `form_grid_field_title` and `form_grid_field_control`. The field control's native Display title must be off (`displayLabel: [null, false]`) because the separate title heading carries the prompt/description, and the field control margin must be set to `--sp--s0` on all sides to remove the default Public Form field margin. Dynamic display, custom validation, and control actions may be used; if multiple fields share the same display rule, place them in a shared container or nested grid and put the dynamic display rule on that group.

Shared validators now enforce the page layout contract for standalone `.ydl` and full `.yap/.yapk` output through `scripts/lib/public-form-template-utils.cjs`.

## Export Storage

In the studied export, each target Data List stores public forms on the list resource:

`Data.Childs[].PublicForms[]`

Each public form entry contains:

- `ListID`
- `ID`
- `Type`
- `Name`
- `Desc`
- `Ext`
- `ExpiredTip`
- `RefId`
- `Resource`

`Resource` is a JSON string, not a gzip-prefixed sub-resource. The parsed public form resource contains:

- `pagetype: 3`
- `ver: 2`
- `attrs`
- `children`
- `tempVars`

The main page content is nested under `children`. The field/control layout uses nested `container` controls with a `flex_grid` that contains list-bound field controls and the submit control.

Public share metadata may live in public-form entry fields such as `Ext`, public settings, or related share fields in future exports. Any public URL or share code must be redacted in committed docs, normalized refs, and logs, for example:

`https://share.yeeflow.com/f/<REDACTED_PUBLIC_FORM_CODE>`

## Inventory

| List | Public forms | Public form names | Fields in `Defs` | Visual controls | List-bound controls | Submit controls | Proof |
|---|---:|---|---:|---:|---:|---:|---|
| `Data list with fields part A` | 1 | `Public form` | 75 | 57 | 51 | 1 | export-proven |
| `Data list with fields part B` | 1 | `Public form test` | 18 | 15 | 10 | 1 | export-proven |

Totals:

- Public forms found: 2
- Visual controls inspected: 72
- List-bound controls inspected: 61
- Submit controls found: 2
- Inspector errors: 0
- Inspector warnings: 0

## Export-Proven Public Field Types

The following top-level list field types are represented by public form field controls in the two target lists:

- `input`
- `textarea`
- `richtext`
- `input_number`
- `percent`
- `currency`
- `switch`
- `radio`
- `checkbox`
- `datepicker`
- `time`
- `file-upload`
- `icon-upload`
- `rate`
- `hyperlink`
- `signer`
- `list`

Field controls use the same broad list-bound pattern seen in custom list forms:

- `type` stores the visual/control type, usually matching the list field `Type`.
- `binding` stores the list field `FieldName`.
- `fieldID` points to the list field `FieldID`.
- `label` mirrors the display label.
- `attrs` stores field-control settings such as `required`, `placeholder`, numeric formatting, rich text toolbar options, rating settings, hyperlink settings, and sub-list configuration.

The native primary `Title` field appears in both public forms as an export-proven list-bound control. This should be treated as a special primary-field exception. It does not prove that default/system fields are generally allowed.

## Disallowed Or Unavailable Fields

Public Forms are anonymous submission forms, so they cannot depend on signed-in user, organization, tenant metadata, app data browsing, or related-record context. The product UI screenshots show that these field families are unavailable for Public Forms, and generated Public Forms must reject them during App Plan review, materialization, and final validation.

User-confirmed unsupported Data List field families:

- User, represented in exports as `identity-picker` / `user`
- Department, represented in exports as `organization-picker`, `groupselect`, or `department`
- Metadata, represented in exports as `metadata`
- Tag, represented in exports as `tag`
- Multi Meta, represented in exports as `mutiple-metadata` or `multiple-metadata`
- Location, represented in exports as `location-picker` / `location`
- Cost center, represented in exports as `cost-center-picker` / `costcenter`
- Lookup, represented in exports as `lookup`

Additional UI-reference-backed unavailable or not-public-safe custom field types:

- `calculated-column`
- `autonumber`

UI-reference-backed default/system fields that should not be generated into Public Forms:

- `Id`
- `Created By`
- `Created Time`
- `Modified By`
- `Modified Time`

Generation rule:

Do not generate login-dependent fields, default/system fields, lookup/calculated fields, tenant-specific picker fields, metadata/tag/location/cost-center fields, or unknown field types into Data List Public Forms unless a future export or product rule proves support. If the source Data List contains such fields, omit them from the Public Form and explain the omission in the App Plan / artifact plan. If a generated public form includes a known-disallowed field type, validators must report a hard error. Unknown or ambiguous field types should warn first.

## Field Family Notes

Text and input:

- `input`, `textarea`, `richtext`, and `hyperlink` are export-proven.
- Public form controls preserve placeholder, required, max-length, encrypted field, rich text toolbar/type, and hyperlink open/button-name settings where present.

Numeric and financial:

- `input_number`, `currency`, `percent`, and `rate` are export-proven.
- Numeric controls preserve formatting settings such as thousands display, rounding, min/max/step, currency code/display format, and rating type/count/icon/half-star settings where present.
- `calculated-column` is not export-proven as a Public Form field control in this export and is UI-reference-backed as unavailable.

Selection and choice:

- `switch`, `radio`, and `checkbox` are export-proven.
- `select` is not present in the target public forms.
- `tag` is present as a list field in the export but is not used as a public form field control.

Date and time:

- `datepicker` and `time` are export-proven.

Identity and organization:

- `identity-picker` and `organization-picker` are UI-reference-backed as unavailable. Do not generate them into anonymous Public Forms.

Uploads, media, signature:

- `file-upload`, `icon-upload`, and `signer` are export-proven structurally.
- Upload/image/signature runtime behavior is not proven by this export-learning pass.

Advanced/reference:

- `list` is export-proven as a Public Form field control and includes nested sub-list field control settings.
- `lookup`, `metadata`, `mutiple-metadata`, `cost-center-picker`, `location-picker`, and `autonumber` are not export-proven as top-level Public Form field controls in this pass.

## Visual Controls

Export-proven structural controls found in the public form resources include:

- `container`
- `flex_grid`
- `action_button`
- `submit-button`

The sub-list control includes nested control types inside `attrs.list-fields`, including `text`, `number`, `boolean`, `date`, `file`, `metadata`, `user`, `costcenter`, `groupselect`, `location`, `lookup`, `img`, and `total`. These are nested sub-list control structures, not proof that the corresponding top-level public field types are available.

UI-reference-backed General controls visible in screenshots:

- Section
- Grid
- Container
- Text
- Paragraph
- Picture
- Divider
- Spacer
- Button
- Tab
- Table
- Toggle
- Icon
- Timer

UI-reference-backed Advanced controls visible in screenshots:

- Drop bar
- Alert
- Progress bar
- Progress circle
- Steps bar
- QR Code
- Barcode
- Custom code
- Embed
- Submit

Generation rule:

Public Form generators must only use export-proven or UI-reference-backed Public Form controls. They must not add other general, advanced, dashboard, custom-list-form-only, approval-form-only, or workflow controls to a Public Form.

Public Forms must not include data-browsing or filter controls. This specifically forbids Collection/Data table/Data list controls, Pivot Table, Chart/Data Analytics, Summary/KPI, Data filter controls, Search filter, Select filter, Date/Number/User filters, and Dynamic record display controls such as Dynamic field, Dynamic user, Dynamic image, or Dynamic file. Public Forms are for anonymous submission, not authenticated record browsing or dashboard-style filtering.

## Submit Behavior

Both target public forms include one `submit-button` control. This proves the structural submit control exists in Public Form resources.

This pass does not prove:

- anonymous submit runtime behavior
- public URL access behavior
- save/data creation behavior
- upload execution
- validation message behavior
- redirect/success page behavior

Validators should require a submit control when a generated Public Form is intended to collect submissions. Unknown action/submit extensions should warn first unless a future export or runtime test proves they break import/open.

## Validation Rules

Hard errors for generated Public Forms:

- `PublicForms[]` entry references a missing list.
- Public form `Resource` is missing or not valid JSON.
- `Resource.children` is missing or not an array.
- List-bound control `binding`/`fieldID` cannot resolve to a field in the same list.
- Known-disallowed default/system fields are used, except the export-proven primary `Title` special case.
- Known-disallowed public form field types are used.
- Known-disallowed Public Form control types are used, including Data filter/Search filter/Collection/Data table controls.
- Duplicate control IDs occur within a public form.

Warnings:

- `Resource.pagetype` differs from the export-proven value `3`.
- `tempVars` is present but not an array.
- Unknown public form visual control type appears.
- A collection public form has no submit control.
- Public URL/share metadata is present and must be redacted before docs/logs/refs are committed.
- Unknown public-form settings are present but not yet understood.

## Generation Rules

When generating Data List Public Forms:

- Store public forms under `Data.Childs[].PublicForms[]`.
- Use `Resource` as JSON string with `pagetype: 3`, `ver: 2`, `attrs`, `children`, and `tempVars`.
- Keep Public Forms separate from Custom List Forms and Approval Forms. Public Forms are additive and never replace the host Data List's standard New/Edit/View custom forms.
- Complete normal Type `1` Data List generation first: fields, default and business views, Data List Form Layouts v1.1 New/Edit/View forms, `List.LayoutView.add/edit/view`, workflows, navigation, permissions, package validation, and full generated-final preflight. Then attach planned Public Forms under `PublicForms[]`.
- Do not create a reduced Public Form-only Data List package or a dedicated hand-built generator path. Full-app and standalone Data List generation must use the shared Data List builder and validators whether `PublicForms[]` is empty or populated.
- Use safe layout controls such as `container` and `flex_grid`.
- Include only public-safe, export-proven field types unless product evidence expands the allowlist.
- Do not include default/system fields except the export-proven primary `Title` field when needed.
- Do not include login-dependent picker fields unless future product/runtime proof explicitly allows them.
- Include a `submit-button` for anonymous collection forms.
- Validate every list-bound field reference before packaging.
- Redact public URLs/share codes in docs, logs, normalized refs, and generated reports.
- Preserve existing app-creation gates: valid `Defs`/`Layouts` arrays, `ListModel.Flags = 1`, valid `FieldIndex`/`FieldName`, unique identifiers, and valid internal names.
- Treat `YAPK_PUBLIC_FORM_CANNOT_REPLACE_CUSTOM_FORMS` and `DATA_LIST_PUBLIC_FORM_CUSTOM_FORMS_REQUIRED` as generated-final blockers.

## Normalized References

Redacted refs are stored in:

`docs/studies/normalized/data-list-public-forms.bundle.json`

They are synthetic or redacted shape references only. They do not contain raw public form resources, real share URLs, tenant IDs, user IDs, or private payloads.
# Public Form Form Actions

Public Form actions are intentionally narrower than Custom Data List Form actions. Use `docs/standards/collection-02.md#public-form-form-action-golden-reference-standard` and the normalized Set variables/Redirect templates. Anonymous actions may use only current-list Public Form-compatible fields, same-form temp variables, and fixed values. They must not query, open, or mutate other application resources.

---

<a id="data-list-workflow-ai-agent-runtime-test-plan"></a>

<!-- source: docs/studies/collection-01.md#data-list-workflow-ai-agent-runtime-test-plan -->

# Data-List Workflow AI Agent Runtime Test Plan

Source study baseline: `<downloads>/Spark & AI (1).yap`

## Classification Goal

Possible result levels:

- export-proven
- generated import/open runtime-proven
- execution-proven
- partial
- blocked

## Safety Gate

Do not run live workflow execution when any of these remain true:

- the package points to real business records
- the Agent can update real rows
- the image input comes from private or customer data
- the runtime would call live AI with non-disposable content
- the app-resource tool could update a non-test list

For this branch, those conditions remained true for the source export, so execution is blocked.

## Safe Runtime Scope

Safe-first checks for a future isolated baseline:

1. import generated sandbox package
2. open app shell
3. open Stock Box-like list
4. open workflow designer for the list workflow
5. open AI node configuration
6. confirm `label_image` and `stock_box_item_id` bindings render
7. open Agent configuration
8. confirm app-resource tool is visible and scoped to local sandbox lists
9. stop before running the flow

## Optional Execution Scope

Only if a future generated sandbox baseline is proven harmless:

- use a disposable sample image with no customer data
- use a disposable sandbox row
- confirm the target list is newly generated and not connected to real records
- run the workflow once
- record whether the Agent updates the same row and how fields change

## Current Branch Decision

- Source-export runtime execution: blocked as unsafe
- Safe generated baseline: not produced in this branch
- Runtime status: export-proven only

## Evidence To Capture In A Future Safe Pass

- package validation pass logs
- import/open screenshots or UI notes
- AI node configuration visibility
- Agent tool configuration visibility
- whether execution was skipped, partially run, or fully run
- exact sandbox-only side effects, if any

---

<a id="data-view-resource-settings"></a>

<!-- source: docs/studies/collection-01.md#data-view-resource-settings -->

# Yeeflow Data View Resource Settings Study

Proof boundary: export-proven, product-documented, and validator-backed only. This pass studied `/Users/Renger/Downloads/Data Lists (1).yap` read-only, generated redacted normalized references under `docs/studies/normalized/data-views.bundle.json`, and did not import, open, execute, mutate, or runtime-test the package.

## Summary

The export contains 7 list-like child resources, all data lists (`Data.Childs[].ListModel.Type = 1`). It contains no document libraries (`Type = 16`) and no Form Report child resources (`Type = 32`), so document-library and Form Report applicability is inferred from existing repository studies and Help Center/product behavior rather than this export.

Data views live in `Data.Childs[].Layouts[]`. View metadata such as name, type, default flag, permission flag, and URL is stored on each layout record. View configuration is split between:

- `Layouts[].Ext1`: JSON string with `Url`, `displayStyle`, `ShowTable`, and view-type-specific field selectors such as `TitleField`, `CoverField`, `CategoryField`, and `IncludeUncategorized`.
- `Layouts[].LayoutView`: JSON string with list-style `layout`, `query`, `sort`, `filter`, and `rowColor`, or calendar-specific keys such as `Scope`, `Columns`, `DefaultColor`, `ColorClass`, and `ColorClassSetting`.

Help Center pages document data views as configurable per-resource presentations with list, gallery, calendar, and kanban styles, plus fields, filters, sorts, URLs, permissions, and default-view behavior:

- [Understand Data views](https://<yourdomain>.yeeflow.com/<redacted-path>
- [Getting started with Yeeflow data views](https://<yourdomain>.yeeflow.com/<redacted-path>
- [Understand Data List](https://<yourdomain>.yeeflow.com/<redacted-path>
- [Creating and utilizing data views in form reports](https://<yourdomain>.yeeflow.com/<redacted-path>

## Export Inventory

| Resource | Resource type | Views | Default view | View types found | URL/key findings | Permission findings | Sort/filter findings | Proof |
| --- | --- | ---: | --- | --- | --- | --- | --- | --- |
| `<data-list-1>` | data list | 4 | `All Items` | list, gallery, kanban | default URL `default`; custom URLs present and redacted | `IsItemPerm = false`; no custom audience schema found | list/gallery filters; created/creator sorts | export-proven |
| `<data-list-2>` | data list | 4 | `All Items` | list, gallery, kanban | default URL `default`; custom URLs present and redacted | `IsItemPerm = false`; no custom audience schema found | one list filter; created/creator sorts | export-proven |
| `<data-list-3>` | data list | 4 | `All Items` | list, gallery, kanban | default URL `default`; custom URLs present and redacted | `IsItemPerm = false`; no custom audience schema found | list/gallery filters; created/creator sorts | export-proven |
| `<data-list-4>` | data list | 4 | `All Items` | list, gallery, kanban | default URL `default`; custom URLs present and redacted | `IsItemPerm = false`; no custom audience schema found | list/gallery filters; created/creator sorts | export-proven |
| `<data-list-5>` | data list | 4 | `All Items` | list, gallery, kanban | default URL `default`; custom URLs present and redacted | `IsItemPerm = false`; no custom audience schema found | list/gallery filters; created/creator sorts | export-proven |
| `<data-list-6>` | data list | 4 | `All Items` | list, gallery, kanban | default URL `default`; custom URLs present and redacted | `IsItemPerm = false`; no custom audience schema found | list/gallery filters; created/creator sorts | export-proven |
| `<data-list-7>` | data list | 4 | `All tasks` | list, calendar | default flag present but name is not `All Items` | `IsItemPerm = false`; no custom audience schema found | active/closed task filters; calendar user-filter fields | export-proven |

## Data View Inventory

| Resource type | Resource | View | Default/custom | View type | URL/key | Visible fields | Primary sort | Secondary sort | Data filter | User filter | Permission | Special settings | Proof |
| --- | --- | --- | --- | --- | --- | ---: | --- | --- | --- | --- | --- | --- | --- |
| data list | `<data-list-1>` | `All Items` | default | list | `default` | 12 | none | none | none | none | inherited/no view item flag | none | export-proven |
| data list | `<data-list-1>` | `<custom-list-view>` | custom | list | redacted | 14 | `Created` desc | none | 1 condition | none | inherited/no view item flag | `displayStyle`, `ShowTable` | export-proven |
| data list | `<data-list-1>` | `<gallery-view>` | custom | gallery | redacted | 2 | `Created` desc | none | 1 condition | none | inherited/no view item flag | `TitleField` | export-proven |
| data list | `<data-list-1>` | `<kanban-view>` | custom | kanban | redacted | 4 | `CreatedBy` desc | none | none | none | inherited/no view item flag | `TitleField`, `CategoryField`, `IncludeUncategorized` | export-proven |
| data list | `<data-list-2>` | `All Items` | default | list | `default` | 16 | none | none | none | none | inherited/no view item flag | none | export-proven |
| data list | `<data-list-2>` | `<custom-list-view>` | custom | list | redacted | 18 | `CreatedBy` desc | none | 1 condition | none | inherited/no view item flag | `displayStyle`, `ShowTable` | export-proven |
| data list | `<data-list-2>` | `<gallery-view>` | custom | gallery | redacted | 5 | `Created` desc | none | none | none | inherited/no view item flag | `TitleField`, `CoverField` | export-proven |
| data list | `<data-list-2>` | `<kanban-view>` | custom | kanban | redacted | 5 | `CreatedBy` desc | none | none | none | inherited/no view item flag | `TitleField`, `CoverField`, `CategoryField` | export-proven |
| data list | `<data-list-3..6>` | same pattern as `<data-list-1>` | mixed | list/gallery/kanban | redacted | 2-14 | created/creator sorts | none | list/gallery filters | none | inherited/no view item flag | title/category selectors | export-proven |
| data list | `<data-list-7>` | `All tasks` | default | list | `all` | 12 | none | none | none | none | inherited/no view item flag | `displayStyle`, `ShowTable` | export-proven |
| data list | `<data-list-7>` | `<active-task-view>` | custom | list | redacted | 10 | `Created` asc | none | 2 conditions | none | inherited/no view item flag | `displayStyle`, `ShowTable` | export-proven |
| data list | `<data-list-7>` | `<closed-task-view>` | custom | list | redacted | 10 | `Created` asc | none | 2 conditions | none | inherited/no view item flag | `displayStyle`, `ShowTable` | export-proven |
| data list | `<data-list-7>` | `<calendar-view>` | custom | calendar | redacted | 0 list columns | none | none | none | six query fields marked filterable | inherited/no view item flag | `Scope`, `Columns`, colors, external setting | export-proven |

## Default View Behavior

Default views are represented by `Layouts[].IsDefault = true`. Six of the seven exported data lists use the title `All Items` and `Ext1.Url = "default"`. The seventh uses `IsDefault = true` with title `All tasks` and `Ext1.Url = "all"`, so the export disproves a strict rule that every default view must literally be named `All items`.

Generation rule: every list-like resource should have exactly one default view unless a focused export proves a different resource-specific pattern. Prefer `All Items` / `Url: "default"` for generic generated data lists, but validators should key default detection on `IsDefault`, not only the title.

## View Types

| Product type | Export Type code | Export settings | Required/dependent fields | Proof |
| --- | ---: | --- | --- | --- |
| List view | `0` | `LayoutView.layout`, `query`, optional `sort`, `filter`, `rowColor`; `Ext1.Url`, optional `displayStyle`, `ShowTable` | layout/query/filter/sort fields reference resource `Defs[]` or system fields | export-proven |
| Gallery view | `999` | list-like `LayoutView`; `Ext1.TitleField`; optional `Ext1.CoverField` | title field required in found samples; cover image optional/product-documented | export-proven |
| Kanban view | `104` | list-like `LayoutView`; `Ext1.TitleField`, `CategoryField`, optional `CoverField`, `IncludeUncategorized` | category field should resolve to a valid field | export-proven |
| Calendar view | `100` | calendar-specific `LayoutView.Scope`, `Columns`, colors, `Filter`, `ExternalSetting`, `query` | start/end/title fields in `Columns` should resolve to valid fields | export-proven |
| News, Gantt, Timeline | not found | Help Center documents these types, but this export does not include their schema | unknown | product-documented only |

## Permissions

This export only proves the inherited/no-custom view permission shape:

- Each studied view has `Layouts[].IsItemPerm = false`.
- No users, groups, departments, audiences, or custom permission collections were found in the view objects.
- Help Center documents choosing a view permission type when creating a view and notes that setting a view as default forces permission to all users.

Generation rule: emit deliberate permission settings where the schema is known; otherwise preserve inherited/all-user default-view behavior and warn on opaque custom audience shapes. Redact users, departments, groups, tenant IDs, emails, and list/view names in study artifacts.

## Sorting

Sort settings live in `LayoutView.sort[]`. Each sort object uses:

- `SortName`: field name or system field.
- `SortByDesc`: boolean; `true` means descending, `false` means ascending.

Only one sort entry was found per sorted view in this export, so primary sort is export-proven and secondary sort remains generation-supported by schema expectation but not sample-proven here. Sort fields found include system fields (`Created`, `CreatedBy`). No custom-field sort sample was found.

## Data Filters

Fixed data filters live in `LayoutView.filter[]`. Observed condition shape:

```json
{
  "key": "<condition-key>",
  "pre": "and",
  "left": "<field-name>",
  "op": "<operator-code>",
  "right": "<fixed-value-or-null>",
  "showCus": true
}
```

The first sample proved flat `and` conditions and fixed literal values. The focused `Event Planning (1).ydl` follow-up extends the export-proven baseline:

- `op: "7"` with `right: null` is used for `is not empty`.
- `pre: "or"` is valid for any-of business filters, as in `Budget and Vendors`.
- Date lower-bound requirements such as `Date >= Today` should use the expression token `[{ "type": "func", "func": "now", "params": [] }]`; do not generate unsupported `Today` tokens/functions.
- Empty or absent `filter` means no fixed filter, so all records are eligible subject to permissions and resource behavior.

Event Planning proof:

| View | Fixed filter proof |
| --- | --- |
| `All Events` | `filter: []` |
| `Schedule Overview` | `Datetime1 op "7" right null` AND `Datetime1 op "3" right now()` |
| `RSVP Tracker` | `Text3 op "7" right null` AND `Text4 op "7" right null` |
| `Budget and Vendors` | `Text7 op "7" right null` OR `Decimal1 op "7" right null` |

## User Filters

End-user filterable fields are represented in `LayoutView.query[]` with `IsFilter = true`. The calendar view sample marks six query fields as filterable. Most list/gallery/kanban samples include query fields but no explicit `IsFilter = true`.

User filters are separate from fixed `LayoutView.filter[]`: user filters declare interactive fields, while fixed filters constrain records for the view.

## Columns And Visible Fields

List-like views use `LayoutView.layout[]` for visible/hidden field columns:

- `FieldID` and `FieldName` identify the field.
- `Order` stores column order.
- `Show` controls visibility.
- `Mobile` stores mobile-display behavior.
- `DisplayName` can differ per view.

This export includes field reference warnings in `<data-list-7>` where view layouts reference fields not present in that resource's `Defs[]`. Treat unresolved view field references as validator warnings for compatibility exports and stronger generator-final findings when they are introduced by generated packages.

## Data List vs Document Library vs Form Report Applicability

| Resource type | Shared schema | Resource-specific differences | Proof |
| --- | --- | --- | --- |
| Data list | `Data.Childs[].Layouts[]`; Type `0`, `100`, `104`, `999`; `Ext1.Url`; `LayoutView` view settings | Normal editable list resource with custom forms, list workflows, sample rows, and generated `Title` metadata requirements | export-proven in this pass |
| Document library | Existing docs prove Type `16` resources use `Defs[]`, Type `0` views, and Type `1` forms; configured views can reuse list-like `LayoutView` keys | Newly created document libraries may have empty-string default view `LayoutView`; document fields/folder/upload behavior are library-specific | prior export/runtime docs, not this export |
| Form Report | Existing docs prove Type `32` resources expose Type `0` views and `Attr_IsViewDetail` for detail access | Report fields derive from approval form variables and optional one sub-list; no workflows/edit forms/sample mutation surface | prior export docs plus Help Center Form Report view doc, not this export |

## Help Center Comparison

| Concept | Product behavior | Export-proven field/shape | Match status | Proof |
| --- | --- | --- | --- | --- |
| Multiple views per resource | Data lists can contain multiple views and users can switch/reorder them | 7 data lists with 28 total views | match | product-documented + export-proven |
| View name and URL | New view creation asks for name and URL | `Layouts[].Title`, `Ext1.Url` | match | product-documented + export-proven |
| View permission | View creation asks for permission type; default views use all users | `IsItemPerm = false`; no custom audience found | partial | product-documented + export-proven gap |
| Default view | Product supports setting a default view | `IsDefault = true`; six names `All Items`, one renamed `All tasks` | match with naming caveat | product-documented + export-proven |
| List/Gallery/Calendar/Kanban | Help Center documents all four | `Type 0`, `999`, `100`, `104` | match | product-documented + export-proven |
| News/Gantt/Timeline | Help Center documents additional types | not present | not found | product-documented only |
| Filters and sorting | Product documents per-view filtering/sorting | `LayoutView.filter[]`, `LayoutView.sort[]` | match | product-documented + export-proven |

## Validator And Generator Guidance

- Detect list-like views from `Layouts[]`, not from a separate `Views` collection.
- Default view detection should use `IsDefault = true`; warn when there is no default or multiple defaults.
- View URLs should be unique within a resource; `default` and `all` are export-proven default URL values.
- Known view type codes: list `0`, calendar `100`, kanban `104`, gallery `999`.
- Field references in visible columns, sort, filters, user filters, and type-specific settings should resolve to `Defs[]` or known system fields.
- Treat unknown view types and opaque permission audiences as warnings until a focused export/runtime pass proves failure behavior.
- Do not claim runtime view switching, permissions, drag/drop kanban updates, calendar rendering, or detail-page behavior without a focused runtime baseline.

---

<a id="doc-library-control-form-hosts-runtime"></a>

<!-- source: docs/studies/collection-01.md#doc-library-control-form-hosts-runtime -->

# Doc Library Control Form Hosts Runtime Study

Branch: `codex/doc-library-control-form-hosts`

Generated app: `Enterprise Document Center - Form Hosted Doc Library`

Generated package: `enterprise-document-center-form-hosted-doc-library.yap` (ignored/uncommitted)

## Scope

This pass extends the dashboard Doc library control milestone into form-host contexts:

- approval forms
- data-list custom forms
- document-library custom forms

The generated package keeps the Enterprise Document Center structure with three Type `16` document libraries, generated root folders, simple custom fields, custom views, and dashboard controls for comparison. It does not include uploaded files or document binaries.

## Local Validation

The corrected package passed local validation with warnings only:

- `node --check tools/generators/generate-doc-library-control-form-hosts.mjs`
- `node --check validate-yap-package.js`
- `node --check validate-yap-graph.js`
- `node --check validate-ydl-list.js`
- `node --check validate-ywf-def.js`
- `node --check yeeflow-control-field-schema-utils.js`
- `node --check skills/installed/yeeflow-application-generator/scripts/yeeflow-control-field-schema-utils.js`
- `node tools/generators/generate-doc-library-control-form-hosts.mjs <downloads>/Document\ Library\ Sample.yap`
- `node validate-yap-package.js enterprise-document-center-form-hosted-doc-library.yap --mode generator --stage final`
- `node validate-yap-graph.js enterprise-document-center-form-hosted-doc-library.yap --mode generator --stage final`
- `node validate-ywf-def.js enterprise-document-center-form-hosted-doc-library.approval-form-def.json --mode final`
- `node scripts/inspect-yap-materialization.mjs enterprise-document-center-form-hosted-doc-library.resource.json`

Final local status:

- package validation: `pass_with_warnings`, `0` errors
- graph validation: `pass_with_warnings`, `0` errors, `0` unresolved edges
- approval-form validation: `pass_with_warnings`, `0` errors
- materialization inspection: `pass`

## Runtime Evidence

Runtime testing was performed in `https://<yourdomain>.yeeflow.com/` on 2026-05-18.

### Document Library Custom Form Host

Status: runtime-proven for open/render and disabled search/add settings.

Evidence:

- The imported app opened and was not an empty shell.
- `Company Policies` opened as a Type `16` document library with generated folders visible.
- `Add > New File` opened the generated custom form `Policy File With Template Reference`.
- The form rendered the native `Upload File` field.
- The embedded Doc library control rendered the `Templates and Forms` root library.
- The control displayed generated folders: `HR Forms`, `Finance Forms`, `Project Templates`, and `Legal Templates`.
- The control was generated with `caption.search = false` and `caption.add = false`; the runtime form showed no search box and no add button for that embedded control.

### Approval Form Host

Status: partial.

Evidence:

- `Document Review Request` imported and appeared in app navigation.
- Opening it before publishing showed `This form has not been published yet`.
- Opening the form builder showed the generated Doc library controls rendered in the approval form designer preview:
  - Company Policies root-bound control with generated folders.
  - HR Policies folder-bound control with disabled search/add and folder context label.
  - Project Documents / Contracts folder-bound control.
- Publishing the first runtime import failed because the approval task node was not configured correctly.
- The blocker is in workflow generation for the `Document Review` assignment task: the task assignee and task form setting are not right.
- This is not treated as a Doc library control schema failure because the controls rendered in Form Builder preview before the workflow publish blocker.

Remaining proof required:

- Import the patched package fresh.
- Fix the workflow/task-node assignee and task form setting.
- Publish the approval form successfully.
- Open the published request page outside the designer and confirm the Doc library controls render there.

### Data List Custom Form Host

Status: validation-only / blocked in this pass.

Evidence:

- The generated `Document Review Tasks` data list is present in the package and graph validation.
- It has custom New/Edit/View forms containing Doc library controls.
- In the imported runtime app used for this pass, the data list was not visible in the top navigation, so the custom forms were not opened from the live UI.

Remaining proof required:

- Determine whether Yeeflow hides mixed data-list navigation in this app shape or whether additional app-resource metadata is needed.
- Import a patched package if needed and open the data-list custom forms directly.

## Generator Fixes From Runtime

- Shortened the generated app description to stay within Yeeflow's import field limit.
- Corrected approval page metadata to keep approval `pageurls[].pagetype = 1` while the embedded approval page form keeps `formdef.pagetype = 2`.
- Added a first-pass current-user `Requester` identity variable and approval task expression assignment, but the approval workflow still needs a focused task-node configuration fix before live request-page proof.
- Updated schema validation so `{LayoutID}` enum placeholders also accept concrete large numeric layout IDs in `attrs.caption.layout`.

## Deferred

- Dynamic expression folder binding remains unproven.
- Direct static sub-folder binding remains unproven.
- Data-list custom-form Doc library controls remain validation-only until opened in runtime.
- Published approval-form Doc library controls remain partial until the workflow/task-node assignee and task form setting are fixed, the patched package is freshly imported, and the request page opens outside Builder preview.
- Actual upload persistence was not tested.
