# docs collection 4

This file combines 21 documents from `docs/` for the Claude directory build. A reference to `docs/<name>.md` is the section that starts at the anchor `#<name>` below.

- [yeeflow-dashboard-design-standards.md](#yeeflow-dashboard-design-standards)
- [yeeflow-dashboard-ui-ux-patterns.md](#yeeflow-dashboard-ui-ux-patterns)
- [yeeflow-data-list-form-design-standards.md](#yeeflow-data-list-form-design-standards)
- [yeeflow-data-list-ui-ux-patterns.md](#yeeflow-data-list-ui-ux-patterns)
- [yeeflow-expression-editor-reference.md](#yeeflow-expression-editor-reference)
- [yeeflow-expression-editor-ui-contexts.md](#yeeflow-expression-editor-ui-contexts)
- [yeeflow-expression-function-reference.md](#yeeflow-expression-function-reference)
- [yeeflow-expression-generation-rules.md](#yeeflow-expression-generation-rules)
- [yeeflow-expression-reference-reconciliation.md](#yeeflow-expression-reference-reconciliation)
- [yeeflow-expression-use-cases.md](#yeeflow-expression-use-cases)
- [yeeflow-field-configuration-reference.md](#yeeflow-field-configuration-reference)
- [yeeflow-form-action-generation-rules.md](#yeeflow-form-action-generation-rules)
- [yeeflow-form-action-query-data-step-rules.md](#yeeflow-form-action-query-data-step-rules)
- [yeeflow-form-action-submit-step-rules.md](#yeeflow-form-action-submit-step-rules)
- [yeeflow-form-design-quality-rules.md](#yeeflow-form-design-quality-rules)
- [yeeflow-root-style-token-reference.md](#yeeflow-root-style-token-reference)
- [yeeflow-runtime-test-checklist-template.md](#yeeflow-runtime-test-checklist-template)
- [yeeflow-temp-variable-generation-rules.md](#yeeflow-temp-variable-generation-rules)
- [yeeflow-text-control-generation-standards.md](#yeeflow-text-control-generation-standards)
- [yeeflow-workflow-generation-rules.md](#yeeflow-workflow-generation-rules)
- [yeeflow-yapk-version-package-study.md](#yeeflow-yapk-version-package-study)

---

<a id="yeeflow-dashboard-design-standards"></a>

<!-- source: docs/collection-04.md#yeeflow-dashboard-design-standards -->

# Yeeflow Dashboard Design Standards

Generated dashboards should be useful operational pages, not landing-page artwork.

## Required Shell

Use:

- Type `103` app page
- `attrs.hideHeaderAll = true`
- dashboard/app page root content width and padding:
  - `attrs.container.cw = "2"`
  - `attrs.container.padding = [null, { top: "--sp--s0", right: "--sp--s0", bottom: "--sp--s0", left: "--sp--s0" }]`
- page-level background on embedded page `attrs.background` when a full-page background is needed
- `Main` -> `Content`
- meaningful `nv_label` values

Supplier Onboarding & Risk Review runtime/designer evidence proves this exact root token-array shape. It is mandatory for every generated or upgraded dashboard/app page and must not be overridden by app requirements, layout style, design image, prompt wording, or later UI tuning. Scalar padding, numeric padding, object padding such as `{ "1": { top: 0, right: 0, bottom: 0, left: 0 } }`, `attrs.common.padding`, and `attrs.style.padding` do not satisfy the dashboard/app page root content-area gate. Inner layout containers may keep intentional spacing.

`Main` is structural. Do not set full-page background color on `Main.attrs.common.background`; use the page-level background instead. Section, card, header, KPI, Collection, and content-area backgrounds are allowed on their own containers.

## Default Structure

```text
Main
└─ Content
   ├─ Page header
   ├─ Summary section
   ├─ Body section
   └─ Collection section or Empty state
```

`Page header` should explain the current page in concise business language. `Summary section` should show KPI/status cards. `Body section` should hold tables, Collections, charts, or operational shortcuts.

## Visual Style

Use:

- neutral page background
- white or token background cards
- subtle neutral borders
- limited soft shadow
- clear spacing rhythm
- primary blue only for active actions, selected states, and main calls to action

Avoid broad gradients, decorative graphics, and oversized hero composition unless the user explicitly asks for a marketing page.

## KPI And Status Cards

Use semantic colors:

- primary for total/current focus
- success for completed/approved
- warning for pending/attention
- danger for overdue/rejected/failed
- neutral for draft/inactive/reference

Cards should have concise labels, a clear value, and optional helper metadata. Avoid filler metrics.

## Collection Presentation

Use Collection controls for repeated list-style data when source lists are local and proven. For table-like dashboard record lists, prefer the grid-table Collection pattern over dashboard `data-list` unless the user explicitly requests Data table.

Recommended names:

- `Collection section`
- `Collection`
- `Collection item`
- `Table header`
- `Table row`
- `Status badge`

For table-style Collections, keep visible captions off on layout-only grids and rely on `nv_label` for designer readability.

Grid-table Collections must be paired with a header `flex_grid` inside one wrapper container. The wrapper must set both `attrs.container.gap = 0` and `attrs.style.gap = [null, 0]`. Planned row-click slide detail requires `attrs.data.link`, `attrs.data.opentype = "slide"`, `attrs.data.modalsize = 2`, and a concrete Type `1` custom detail layout for the source list. Helper-only metadata must not leak into encoded package objects.

## Empty States

Use business-friendly copy:

- state what is empty
- say what the user can do next
- avoid technical wording

Example: `No requests yet. Submitted requests will appear here after the workflow starts.`

## Validator Guidance

Fail when dashboards are missing hidden header or the exact root content-area shape `attrs.container.cw = "2"` plus token-array zero padding with `--sp--s0` on all sides. Warn for missing page-level background when `Main` has a background, missing `Main`, missing `Content`, missing meaningful `nv_label`, or many arbitrary hard-coded colors. Warn when `Main` carries a full-page-like background because generated dashboards should put full-page background on embedded page attrs.

Dashboard Text controls should follow the shared Yeeflow Text control standard in `docs/collection-04.md#yeeflow-text-control-generation-standards`: native `heading` controls, inline width by default, typography presets as `[null, token]`, and `heads.color` as a plain string. Dashboard page titles should use a visible title token such as `attrs.heads.ty = [null, "h5-medium"]` unless another validated style is intentionally planned. Do not reuse the old CAPEX generated text style shape with pair-shaped color values.

## Form Actions Boundary

Approval-form exports prove native form actions for page load, button click, temp variables, Set variable, Confirm dialog, Query data, Submit form, and Save changes. Dashboard pages may share some front-end action concepts in later phases, but Submit form steps are not supported on dashboards.

For generated dashboards:

- do not generate Submit form or Save changes steps
- do not rely on approval-form wrapper shapes for dashboard action generation without a dashboard export
- continue to use page-level dashboard background, not Main background
- keep action-like buttons inline width with meaningful `nv_label`

## Runtime-Proven Generation Notes

The first Design System Request Tracker runtime package proved that a generated Type `103` dashboard with embedded page JSON should store `LayoutInResources[0].ID` and `LayoutInResources[0].RefId` as the dashboard `LayoutID` when the page resource is the runtime dashboard body. A separate resource ID imported successfully but rendered the dashboard as an empty designer placeholder. Use the dashboard `LayoutID` for the embedded resource unless a later export proves a safer split-resource pattern.

Keep `LayoutView = null`, `Ext2 = "{\"src\":true}"`, and `LayoutInResources[0].Resource` as the serialized page JSON. Before signing, installing, importing, upgrading, or handing off generated packages, parse each `Pages[].LayoutInResources[].Resource` JSON string and normalize the root object to `attrs.container.cw = "2"` plus `attrs.container.padding = [null, { top: "--sp--s0", right: "--sp--s0", bottom: "--sp--s0", left: "--sp--s0" }]`. The runtime-proven shell also uses embedded `attrs.hideHeaderAll = true` and `Main` -> `Content`.

---

<a id="yeeflow-dashboard-ui-ux-patterns"></a>

<!-- source: docs/collection-04.md#yeeflow-dashboard-ui-ux-patterns -->

# Yeeflow Dashboard UI/UX Patterns

This document defines the dashboard UI/UX standard extracted from `UI and UX design (1).yap`.

Use `docs/collection-03.md#yeeflow-application-design-system`, `docs/collection-04.md#yeeflow-dashboard-design-standards`, and `docs/collection-04.md#yeeflow-root-style-token-reference` for the current reusable dashboard standard. Generated dashboards should prefer native token references and avoid arbitrary custom color palettes.

## Studied Dashboard Resource

The mini app has one root Type `103` dashboard layout:

- Layout title: `Dashboard`
- Layout type: `103`
- `LayoutView`: `null`
- `Ext2`: `{"src":true}`
- `LayoutInResources`: one embedded page JSON resource

The embedded page JSON contains:

- `title`: `Dashboard`
- `ver`: `2`
- `filterVars`: `[]`
- `tempVars`: `[]`
- `exts`: `[]`
- one `Main` container
- one nested `Content` container

## Header Visibility

Hide header bar is stored on the embedded dashboard page:

```json
{
  "attrs": {
    "hideHeaderAll": true
  }
}
```

Generator rule:

- Generated standard dashboards should set `attrs.hideHeaderAll = true`.
- Keep app/page title available in metadata, but hide the default Yeeflow page header so the generated layout controls the first viewport.

## Grid-Table Collection Record Lists

Task Tracker hardening adds a blocking generated-final gate for dashboard record-list sections that claim the grid-table Collection pattern. Use grid-table style `collection` controls, not dashboard `data-list` controls, unless the user explicitly requests Data table. Pair each Collection with a header `flex_grid`, place both controls in one wrapper container, and serialize both `attrs.container.gap = 0` and `attrs.style.gap = [null, 0]` so runtime spacing and designer Element Gap agree. When row-click detail is planned, set `attrs.data.link`, `attrs.data.opentype = "slide"`, and `attrs.data.modalsize = 2`, and ensure the link resolves to a Type `1` custom detail layout for the same source list. Run `scripts/validate-dashboard-grid-table-collections.mjs` before signing, install, upgrade-check, or handoff.

## Content Area And Padding

The studied dashboard uses zero padding:

```json
{
  "attrs": {
    "container": {
      "padding": [
        null,
        {
          "top": "--sp--s0",
          "right": "--sp--s0",
          "bottom": "--sp--s0",
          "left": "--sp--s0"
        }
      ]
    }
  }
}
```

Supplier Onboarding & Risk Review runtime/designer evidence proves that generated dashboard/app page roots must set `attrs.container.cw = "2"` and use the exact token-array zero-padding shape `attrs.container.padding = [null, { top: "--sp--s0", right: "--sp--s0", bottom: "--sp--s0", left: "--sp--s0" }]`. Earlier dashboard exports that omitted `cw` are superseded for generated and upgraded dashboard/app page roots. Keep intentional spacing on inner layout containers, not on the page root.

## Container Tree

Studied tree:

```text
Dashboard page
└─ container nv_label="Main"
   └─ container nv_label="Content"
```

`Main` style:

```json
{
  "gap": [null, "--sp--s0"],
  "direction": [null, "column"],
  "justify_content": [null, "flex-start"],
  "align_items": [null, "center"]
}
```

`Content` style:

```json
{
  "gap": [null, "--sp--s0"],
  "direction": [null, "column"],
  "justify_content": [null, "flex-start"],
  "align_items": [null, null]
}
```

## Naming Rules

Use `nv_label` for designer-facing names:

- `Main`
- `Content`

Do not use visible text controls just to label structural containers.

## Generator Rules

Dashboard generators should:

- Use Type `103` page layouts for app pages.
- Keep `LayoutView = null`.
- Store embedded page JSON in `LayoutInResources[0].Resource`.
- Use `Ext2 = "{\"src\":true}"`.
- Set `attrs.hideHeaderAll = true`.
- Set page padding to `--sp--s0` on all sides.
- Create a top-level `Main` container and nested `Content` container.
- Put KPI cards, filters, Collection controls, tables, charts, help blocks, and empty states inside `Content`.
- Use `--c--neutral-light` or `--c--background` for page/background surfaces.
- Use `--c--background` for cards and content panels.
- Use `--c--primary` for primary dashboard actions and active pagination/action states.
- Use `--c--success`, `--c--warning`, `--c--danger`, and neutral tokens for status cards and badges.
- Use `--sp--s200`, `--sp--s300`, and `--sp--s400` for dashboard content rhythm.
- Use `8px` or `12px` radius for dashboard cards, with neutral borders before shadows.

## Validator Recommendations

Safe warnings:

- Missing `attrs.hideHeaderAll = true`.
- Missing zero padding.
- Missing `Main` container.
- Missing `Content` container.
- `Content` not nested inside `Main`.
- Many arbitrary hex colors in generated dashboard styles where semantic root tokens are available.

Risky checks for later:

- Exact layout width on dashboards, because the studied export did not store `cw`.
- Exact visual dimensions, because runtime rendering must prove how Yeeflow interprets generated sizing.
- Requiring token references when Yeeflow exports resolved hex values.

## Future Dashboard Test

The first generated UI/UX standard dashboard should add content inside the proven shell:

- small page title/header block inside `Content`
- one empty-state block
- one static KPI row
- one table or Collection bound to a local included list

Do not add charts or filters in the first UI/UX standard test unless the goal is specifically to prove dashboard data binding.


Dashboard/app page root content-area padding is a hard gate: every generated or upgraded Type 103 dashboard/app page must serialize `Pages[].LayoutInResources[].Resource` with root `attrs.container.cw = "2"` and `attrs.container.padding = [null, { top: "--sp--s0", right: "--sp--s0", bottom: "--sp--s0", left: "--sp--s0" }]`. Scalar padding, object/numeric padding, numeric array padding, `attrs.common.padding`, or `attrs.style.padding` alone are invalid for the root content area. Normalize existing dashboard/app page roots to this exact token-array shape before signing, installing, importing, or upgrading. Inner layout containers may keep intentional spacing.

Data-list custom form root content-area padding uses the same hard gate: every generated or upgraded New, Edit, View, Detail, or custom form under `Data.Childs[].Layouts[].LayoutInResources[].Resource` or `Childs[].Layouts[].LayoutInResources[].Resource` must parse to a root with `attrs.container.cw = "2"` and the same `--sp--s0` token-array padding. Scalar zero, numeric object zero, `attrs.common.padding`, or `attrs.style.padding` alone remain compatibility fallbacks only and do not satisfy generated-final validation. Inner form sections, cards, grids, controls, and content wrappers may keep intentional spacing.

---

<a id="yeeflow-data-list-form-design-standards"></a>

<!-- source: docs/collection-04.md#yeeflow-data-list-form-design-standards -->

# Yeeflow Data List Form Design Standards

Generated data lists should provide predictable custom forms by default.

## Required Custom Forms

Every generated data list should include:

- `Edit Item`
- `View Item`

New and Edit should use `Edit Item`. View should use `View Item`.

## Required Shell

Both forms use:

- `attrs.container.cw = "2"`
- zero padding
- page-level background on the custom form `attrs.background` when a full-page background is needed
- `Main` -> `Content`

`Main` is structural. Do not set full-page background color on `Main.attrs.common.background`; use the custom form page-level background instead. Field group, card, page header, and readonly-section backgrounds are allowed on their own containers.

## Edit Form Pattern

`Edit Item` should be input optimized:

- group related fields into `Field group`
- keep required fields near the top
- use concise field labels
- use helper text only when it clarifies data entry
- use native controls first

## View Form Pattern

`View Item` should be display optimized:

- use readonly/display controls where available
- group important identifiers and status near the top
- use `Readonly section` for non-editable detail
- avoid presenting the view form as a disabled copy of the edit form when a cleaner display layout is possible

## Field Grouping

Recommended groups:

- `Basic information`
- `Details`
- `Status`
- `System information`
- `Readonly section`

Keep group names business-friendly and avoid field-slot names.

## Style

Use root-token-aligned styles:

- white or background-token sections
- neutral borders
- `6px` control radius
- `8px` or `12px` section radius
- `--fs--base` body text
- `--sp--s150` / `--sp--s200` field gaps

## Native Title Rule

Generated lists must preserve native `Title` metadata. Business labels may be displayed on `Title`, but `Title` is not a normal custom field.

## Validator Guidance

Warn for missing Edit/View forms, incorrect display settings, missing `Main` / `Content`, non-full-width form pages, non-zero padding, `Main` carrying full-page-like background, missing page-level background when `Main` has a background, and excessive arbitrary colors.

Data-list custom form headings, helper text, card labels, and empty-state text should follow the shared Text control standard in `docs/collection-04.md#yeeflow-text-control-generation-standards`. Default to native `heading` Text controls with inline width, `[null, token]` typography presets, plain string colors, and meaningful `nv_label`.

## Form Actions

Approval-form exports prove that Yeeflow form actions can use page load, button click, temp variables, Set variable, Confirm dialog, Query data, Submit form, and Save changes patterns. Data-list custom forms may use the same conceptual front-end form action model, but generator output should wait for a data-list custom form export or focused runtime test for the exact wrapper.

When a data-list custom form uses form actions in the future:

- use page-level background, not Main background
- keep action buttons inline width
- use meaningful `nv_label`
- use temp variables only for front-end/runtime state
- do not rely on temp variables for backend persistence unless values are copied into real fields
- use Query data with explicit selected fields and mappings
- use Submit form / Save changes only in form contexts, never dashboard contexts

---

<a id="yeeflow-data-list-ui-ux-patterns"></a>

<!-- source: docs/collection-04.md#yeeflow-data-list-ui-ux-patterns -->

# Yeeflow Data List UI/UX Patterns

This document defines the data-list UI/UX standard extracted from `UI and UX design (1).yap`.

Use `docs/collection-03.md#yeeflow-application-design-system`, `docs/collection-04.md#yeeflow-data-list-form-design-standards`, and `docs/collection-04.md#yeeflow-root-style-token-reference` for the current reusable data-list form standard. Generated data-list views and custom forms should prefer native tokens for colors, spacing, and typography when style attributes are generated.

## Studied Data List Resource

The mini app contains one data list:

- List title: `Data List`
- List type: `1`
- Fields: native `Title`
- Custom forms:
  - `Edit Item`
  - `View Item`

The data list is intentionally minimal. It proves custom list form registration, display-setting mapping, page width, padding, and container naming.

Compatibility note: the studied export stores `Title.Status = 1` and `Title.FieldIndex = 1`. That is accepted as source-export evidence only. Generated packages must continue to follow the stricter generated-list rule: native `Title` metadata with `Status = 0`, `IsSystem = true`, `IsIndex = true`, and `FieldIndex = 0` where the generator controls field metadata.

## Custom Form Requirement

Every generated data list should have at least two custom list forms:

1. `Edit Item`
2. `View Item`

Display settings should map:

```json
{
  "add": "<Edit Item LayoutID>",
  "edit": "<Edit Item LayoutID>",
  "view": "<View Item LayoutID>",
  "opentype": {
    "add": "modal"
  },
  "modalsize": {}
}
```

The studied export maps New and Edit to the same `Edit Item` form and View to `View Item`.

## Custom Form Storage

Both custom forms are stored as Type `1` layouts on the data list:

- `Layout.Type = 1`
- `Layout.Title = "Edit Item"` or `"View Item"`
- `Layout.LayoutView = null`
- `Layout.Ext2 = "{\"src\":true}"`
- `Layout.IsItemPerm = false`
- `Layout.LayoutInResources[0].Resource` contains the form JSON

The studied export used `LayoutInResources[0].ID` and `RefId` values that match after numeric precision preservation. Generators should continue using the proven generated-list rule: `LayoutInResources[0].ID` and `RefId` match the custom form `LayoutID`.

## Form Page Settings

Both `Edit Item` and `View Item` form resources use:

```json
{
  "attrs": {
    "container": {
      "cw": "2",
      "padding": [
        null,
        {
          "top": "--sp--s0",
          "right": "--sp--s0",
          "bottom": "--sp--s0",
          "left": "--sp--s0"
        }
      ]
    }
  }
}
```

`cw = "2"` is the studied full-width content setting for custom list forms.

## Container Tree

Both custom list forms use:

```text
Custom list form
└─ container nv_label="Main"
   └─ container nv_label="Content"
```

`Main` style:

```json
{
  "gap": [null, "--sp--s0"],
  "direction": [null, "column"],
  "justify_content": [null, "flex-start"],
  "align_items": [null, "center"]
}
```

`Content` style:

```json
{
  "gap": [null, "--sp--s0"],
  "direction": [null, "column"],
  "justify_content": [null, "flex-start"],
  "align_items": [null, null]
}
```

## Edit Versus View Form Pattern

The mini app's `Edit Item` and `View Item` forms have the same shell structure. Their difference is role and display-setting assignment:

- New item: `Edit Item`
- Edit item: `Edit Item`
- View item: `View Item`

Future generated forms should use the same shell but can vary controls:

- `Edit Item`: editable controls for business fields.
- `View Item`: readonly/display controls for business fields.

## Generator Rules

Data-list generators should:

- Always preserve native `Title` metadata.
- Create both `Edit Item` and `View Item` custom forms for generated lists unless explicitly scoped out.
- Assign New/Edit/View display settings as proven above.
- Use full-width `cw = "2"` and zero padding.
- Use `Main` and `Content` containers by `nv_label`.
- Place business controls inside `Content`.
- Use meaningful `nv_label` names for section containers and important controls.
- Use `--c--background` for form/card surfaces.
- Use `--c--neutral-light-active` for neutral borders.
- Use `--c--neutral-light-hover` for subtle table/filter/addon backgrounds.
- Use `--fs--base` for normal field labels and values.
- Use `--fs--s` or `--fs--xs` for help text and metadata.
- Use `6px` radius for controls and `8px` or `12px` radius for form sections.

## Validator Recommendations

Safe warnings:

- Missing `Edit Item` custom form.
- Missing `View Item` custom form.
- New or Edit display setting not mapped to `Edit Item`.
- View display setting not mapped to `View Item`.
- Missing full-width `cw = "2"`.
- Missing zero padding.
- Missing `Main` or `Content` container.
- `Content` not nested inside `Main`.
- Excessive hard-coded colors in generated custom form styles when root tokens are available.

Keep existing hard errors for:

- Invalid custom form resource JSON.
- Missing required custom form resource keys.
- Unresolved field bindings.
- Broken custom form registration.

## Future Data-List Test

The first generated UI/UX standard data-list test should include:

- native `Title`
- one or two simple business fields
- `Edit Item` custom form with editable fields inside `Content`
- `View Item` custom form with readonly/display fields inside `Content`
- New/Edit/View display settings mapped to the correct form layouts

---

<a id="yeeflow-expression-editor-reference"></a>

<!-- source: docs/collection-04.md#yeeflow-expression-editor-reference -->

# Yeeflow Expression Editor Reference

This reference is normalized from the first expression training file, enriched by `expression training data generator_2.txt`, and cross-checked against supplied expression-editor screenshots. Uploaded training files and screenshots are treated as read-only evidence, not bundled source data.

## Core Token Model

Yeeflow expressions are JSON arrays of tokens. When a control or workflow setting expects expression editor output, generate expression arrays, not arbitrary JavaScript or raw text formulas.

Common token shapes:

```json
[
  { "exprType": "variable", "valueType": "number", "id": "Amount", "type": "expr", "name": "Workflow Variables:Amount" },
  { "type": "op", "op": "*" },
  { "type": "num", "value": "0.8" }
]
```

Literal tokens:

- string: `{ "type": "str", "value": "abc" }`
- number: `{ "type": "num", "value": "123" }`
- boolean: `{ "type": "bool", "value": true }`
- empty string: `{ "type": "str", "value": "" }`

Variable token:

```json
{
  "exprType": "variable",
  "valueType": "text",
  "id": "field_1",
  "type": "expr",
  "name": "Workflow Variables:field1"
}
```

All five variable properties are required. `valueType` may only be `number`, `text`, `date`, or `boolean`.

Function token:

```json
{ "type": "func", "func": "dateFormat", "params": [[{ "type": "func", "func": "now", "params": [] }], [{ "type": "str", "value": "YYYYMMDD" }]] }
```

Function params are arrays. In most cases, each param is itself an expression-token array. A few export examples use primitive strings or booleans inside params; validators should warn before rejecting these unless a focused export proves the context.

The enriched function knowledge base is stored in `yeeflow-expression-function-knowledge-base.normalized.json`. It adds bilingual descriptions, parameter names, return types, business scenarios, and keywords. It does not override the first-pass allowed function/operator set unless the function exists in `yeeflow-expression-functions.normalized.json`.

## Operators

Allowed operators from the training reference:

- arithmetic: `+`, `-`, `*`, `/`
- grouping: `(`, `)`
- logical: `and`, `or`
- comparison: `>`, `>=`, `<`, `<=`, `==`, `!=`
- string concatenation: `&`

The screenshots also show `%` in the expression-editor toolbar. Do not generate `%` until it is present in the normalized operator reference or an export-backed expression proves the token shape.

Do not invent operator names.

## Usage Contexts

The same token model can appear in:

- calculated controls
- dynamic display rules
- custom validation rules
- lookup/data filters
- workflow sequence/transition conditions
- workflow action conditions
- default values
- generated request numbers
- subtotal and total calculations
- date calculations
- string formatting

Different contexts may wrap expression tokens inside larger Yeeflow condition objects. Preserve the local wrapper shape from the studied export and validate the nested expression token array separately.

For screenshot-derived entry points and caveats, see `docs/collection-04.md#yeeflow-expression-editor-ui-contexts`.

## UI Observations From Screenshots

- Calculation controls expose an `Expression` field and `Edit` button.
- Dynamic display rules open from control settings.
- Custom validation opens from the field Validation section.
- Lookup/data filters open from the Lookup control's data source/filter settings.
- Workflow transition conditions open from the selected sequence/transition arrow.
- The Expression Editor has separate context-variable and function selectors.
- Variable groups observed include Context, Workflow Variables, Static Variables, Temp variables, and Filter variables.
- Function tab categories observed include All, String, Logical, Date, Mathematical, and Other.
- `addWorkDays` and `addWorkHours` are visible in the Function tab but remain metadata-pending and not generation-safe.

## Serialization

For model-training samples, the assistant content must be a minified JSON string containing the expression array. For generated `.yap`/`.ywf` resources, keep the native shape expected by the target property: a JSON array when the export uses arrays, or a minified string only when the export-backed property stores it as a string.

---

<a id="yeeflow-expression-editor-ui-contexts"></a>

<!-- source: docs/collection-04.md#yeeflow-expression-editor-ui-contexts -->

# Yeeflow Expression Editor UI Contexts

This reference is based on the supplied screenshots from the Yeeflow designer. It documents where expressions are configured and what generators should assume before export-backed wrapper shapes are available.

## Calculation Control Expression

Entry point:

- select a Calculation control
- open the Content tab
- use the `Expression` field and `Edit` button

Expected expression:

- a value-producing expression token array
- usually numeric for subtotal/total formulas, but can return text/date/boolean depending on the bound variable

Typical variables:

- workflow variables such as Quantity, Unit Price, Amount, Date, Status
- list/sublist variables when the calculation summarizes line items

Generator guidance:

- use calculated controls for fields named like Subtotal, Total, Balance, Duration, Difference, or Amount when a formula is implied
- represent `Quantity * Unit Price` as variable tokens with the `*` operator
- use `arraySum` for list/sublist totals only when the list variable and column names are resolved

Validator guidance:

- require a non-empty expression on generated Calculation controls unless explicitly deferred
- validate nested function params and variable tokens
- warn on primitive function params unless export-backed for the function

## Expression Editor Toolbar

Observed toolbar entries:

- number literal: `123`
- text literal
- expression/context variable selector
- function selector
- concatenation: `&`
- arithmetic: `+`, `-`, `*`, `/`, `%`
- grouping: `(`, `)`
- logical: `and`, `or`
- comparison: `==`, `!=`, `>`, `>=`, `<`, `<=`
- boolean literals: `true`, `false`
- empty literal: `[empty]`
- copy/paste controls

Generator guidance:

- generate the normalized token arrays directly; do not generate raw JavaScript formulas
- use only operators present in the normalized operator reference
- treat `%` as UI-observed but do not generate it until it is present in the normalized operators or export-backed

## Function Tab

Entry point:

- open Expression Editor
- choose the Function tab/icon

Observed categories:

- All
- String
- Logical
- Date
- Mathematical
- Other

Observed function list included:

- `abs`
- `addWorkDays`
- `addWorkHours`
- `arrayAverage`
- `arrayConcat`

Generator guidance:

- use `yeeflow-expression-functions.normalized.json` and `yeeflow-expression-function-knowledge-base.normalized.json` for generation-safe functions
- `addWorkDays` and `addWorkHours` are observed in the UI but metadata-pending; do not generate them yet
- choose functions by business intent using category, keywords, and businessScenarios metadata

## Expression / Context Variable Selector

Entry point:

- open Expression Editor
- click the expression/context selector

Observed variable groups:

- Context
- Workflow Variables
- Static Variables
- Temp variables
- Filter variables

Expected token:

```json
{
  "exprType": "variable",
  "valueType": "text",
  "id": "field_1",
  "type": "expr",
  "name": "Workflow Variables:field1"
}
```

Generator guidance:

- use exact variable token shape with all five required properties
- use `valueType` only as `number`, `text`, `date`, or `boolean`
- set `name` to the correct group-qualified label when known
- do not reference unresolved filter/temp variables outside the context where they exist

Export-backed current-user context tokens use a different application-token shape:

```json
{
  "id": "CurrentUser",
  "exprType": "application",
  "valueType": "string",
  "type": "expr",
  "name": "Context:Current User"
}
```

Use this context token with `getUserAttr` only after the target page is an approval-form expression context. Attribute selector parameters are direct descriptor objects such as `{ "key": "Email", "label": "Email" }`; do not wrap the descriptor in an expression-token array.

## Dynamic Display Rules

Entry point:

- select a control
- open Content or Appearance settings
- click `Dynamic display rules`

Expected expression:

- boolean/comparison expression nested inside the control display rule wrapper

Common use cases:

- show justification when amount is greater than a threshold
- show details when a switch is true
- show vendor fields when procurement type equals single source

Generator guidance:

- preserve the export-backed `attrs.control_display` wrapper shape for the target control
- validate the nested expression token array independently
- use simple comparison expressions before complex function expressions

## Custom Validation Rules

Entry point:

- select a field control
- open the Validation section
- click `Custom validation`

Expected expression:

- boolean validation condition, normally true when the field value is valid

Common use cases:

- required-if validation using `isNullOrEmpty`
- required date must be greater than or equal to today
- amount must be positive or below an allowed threshold

Generator guidance:

- use native required flags for simple required fields
- use custom validation only when a business rule cannot be expressed by built-in validation
- keep validation messages and wrappers export-backed before generation

## Lookup / Data Filter Conditions

Entry point:

- select a Lookup control or data-bound control
- open the data source/filter settings
- click `Data filters` / `Condition`

Expected expression:

- condition-builder object with nested expression tokens for the chosen side of a condition

Common use cases:

- filter lookup records where Active equals true
- filter records by category, current user, or context variable
- constrain lookup source data before user selection

Generator guidance:

- preserve lookup filter wrapper shape from the target export
- validate source list fields before referencing them
- use readable lookup autofill variables for user-facing persistence rather than raw row IDs

## Workflow Transition Condition Editor

Entry point:

- open Workflow designer
- select a sequence/transition arrow
- use the right-side `Condition` button

Expected expression:

- boolean condition controlling whether the transition is taken

Common use cases:

- route to Finance when total amount is at least a threshold
- route to Security when security review is required
- route approve/reject paths based on task outcome

Generator guidance:

- generate workflow conditions only when the transition-condition wrapper is export-backed
- keep approve/reject outcome transitions in the proven simple condition pattern
- use `iif` for conditional values, but use direct boolean comparisons for routing decisions
- for numeric workflow routes from list summaries, use the export-backed `conditioninfo` wrapper with `group: "number"`, `n.>` / `n.<=` operators, and a summary-bound number variable such as `TotalAmount`

## User/Profile Expression Display

Observed in `Expression Runtime Test v1 Patch.yap`:

- text-like expression display can be stored under native heading/Text controls at `attrs.headc.title.variable`
- generated runtime proof used calculated controls bound to text variables so values could render on submission/task pages and also participate in ContentList persistence
- profile image display can use `getUserAttr(Context:Current User, Photo, ...)`, but generated image/profile controls remain runtime-sensitive unless a focused media expression test is requested

Generator guidance:

- use `getUserAttr`, `getOrgAttr`, and `getLocAttr` with export-backed descriptor parameters
- use `getOrgAttr`, not `getDeptAttr`, for department/organization attributes
- add safe fallback arrays for optional profile values
- for persistence, prefer calculated/text summary variables and map those to ContentList; do not directly persist object-shaped user, department, or location values into text fields
- document tenant-data dependency when Location, Boarding Date, Phone, or manager-related values are blank

## Sub List Current Object Expressions

Observed in updated `Approval Form Controls Test v6.yap`.

Entry point:

- select a field inside a Sub list/List control
- set `Control type` to `Calculation`
- open the calculation expression editor for that sub list field

Expected expression:

- row-level calculated value evaluated against the current sub list row

Observed variable scope:

- `Current object:<Row Field Name>` entries are available inside the expression editor
- export token shape uses `exprType: "variable_ctx"`
- `ctx` is the parent list variable id, for example `LineItems`
- `id` is the row field id, for example `LineQuantity`

Common use cases:

- `Sub Total = Current object:Quantity * Current object:Unit Price`
- row amount, score, duration, or difference calculations

Generator guidance:

- add the calculated row field to `variables.listref[].fields[]`
- render the row field as a `calculated` control inside the submit-page list control
- store the expression at `control.attrs.calculated`
- validate every `variable_ctx` token against the listref row fields

## Sub List Summary Editor

Observed in updated `Approval Form Controls Test v6.yap`.

Entry point:

- select a Sub list/List control
- open Summary settings from the control panel

Expected configuration:

- list control summary metadata, not a normal expression token array

Observed structure:

- summaries live in `attrs["list-fields-summary"]`
- summary field references use row field ids
- `type: "total"` renders as Sum
- `type: "avg"` renders as Average
- optional binding uses `{ "prefix": "__variables_", "value": "<WorkflowVariableId>" }`

Common use cases:

- quantity sum
- unit price average
- subtotal sum bound to a top-level `TotalAmount` number variable

Generator guidance:

- use summary-bound variables for totals that drive workflow routes
- display the bound variable through a readonly number control when users need to see it outside the list
- keep summary binding target types compatible with the row field and summary type

## Form Actions

Observed in manually updated `Expression Sublist Summary Workflow Test v1.yap` and `Form Actions Phase 1 Test v1 Runtime.yap`.

Entry points:

- Button control: `attrs.control_action` binds a click action to an action id in `page.formdef.actions[]`.
- Page form definition: `page.formdef.formAction.onLoad` binds a page-load action.
- Form action steps: Set variable steps use expression-token arrays for values and optional conditions.
- Confirm dialog steps use expression-token arrays for the dialog message and a variable token for the result target.
- Query data steps can write count and collection outputs into temp variables.
- Submit form steps progress or save the active form and are not expression wrappers unless an execution condition is configured.

Typical variables:

- workflow variables from `variables.basic[]`
- temp variables declared in `variables.tempVars[]` and referenced as `__temp_<id>`
- context/profile functions such as `currentUser`, `getUserAttr`, and `now` when export-backed
- query result collections stored in temp variables such as `__temp_var_CollectionofQueryItems`

Generator guidance:

- use form actions for client-side form behavior and initialization
- use workflow graph actions for backend/process behavior
- validate temp variable references separately from workflow variables
- keep generated action buttons inline and named with meaningful `nv_label`
- for query-result aggregates, use `arraySum` against an explicit selected field from the preceding query step
- preserve export-backed `JSONStringfy` spelling for query collection display/debugging
- keep `vLookup` deferred until an export provides its function token shape

---

<a id="yeeflow-expression-function-reference"></a>

<!-- source: docs/collection-04.md#yeeflow-expression-function-reference -->

# Yeeflow Expression Function Reference

Sources:

- first-pass expression foundation: `expression training data generator.txt`
- second-pass enriched function knowledge base: `expression training data generator_2.txt`
- expression-editor screenshots for UI context and observed function-tab behavior

Use the normalized machine-readable reference in `yeeflow-expression-functions.normalized.json` for validators and generators.
Use `yeeflow-expression-function-knowledge-base.normalized.json` for richer bilingual descriptions, parameter names, business scenarios, and keywords.

## Selection Guidance

Choose functions by business intent before choosing by name:

| Intent | Preferred functions | Notes |
| --- | --- | --- |
| Amount formatting | `formatNumber`, `round`, `fixed` | Use `formatNumber(value, digits, thousandths)` when the result is user-facing text. |
| Deadline or due-date calculation | `dateAdd`, `dateDiff`, `now` | Use `dateAdd` for offset dates and `dateDiff` for overdue/duration checks. |
| Conditional value/text | `iif`, `isTrue`, `isFalse` | Use direct boolean comparisons for routing and display rules where possible. |
| Required-if validation | `isNullOrEmpty`, comparison operators | Native required flags stay preferred for unconditional required fields. |
| Request number generation | `concat` or `&`, `dateFormat`, `now`, `UniqueID` | Preserve exact `UniqueID` capitalization. |
| List/sublist totals | `arraySum`, `arrayCount`, `arrayAverage`, `arrayMin`, `arrayMax` | Only generate after the list variable and column names are resolved. |
| Safe object attribute access | `getAttr` | Use when nested object path access is proven for the target context. |
| Current user/profile attributes | `getUserAttr`, `getOrgAttr`, `getLocAttr` | Export-backed from `Expression Runtime Test v1 Patch`; use exact exported `getOrgAttr` for department/organization attributes. |
| Text cleanup | `trim`, `replace`, `lower`, `upper` | Useful for normalization and generated summaries. |
| Duplicate removal | `removeDuplicates` | Use for arrays after the array variable is resolved. |

## Enrichment Notes

- The second-pass knowledge base enriched 54 of 55 baseline functions.
- `strIndex` is baseline-only in the second pass. Keep the exact camel-case name and do not rename it to `strindex`.
- The Function tab screenshot shows `addWorkDays` and `addWorkHours`, but the second file does not provide parameter metadata. Treat them as observed but not generation-safe yet.
- Function examples in the knowledge base are human-readable expression strings. Generated packages must convert them to Yeeflow JSON token arrays.
- `Expression Runtime Test v1 Patch.yap` adds export-backed user/profile attribute functions. The user notes mentioned `getDeptAttr`, but the decoded export uses `getOrgAttr`; preserve `getOrgAttr` until `getDeptAttr` is proven by an export.

## Math

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `abs` | 1 | number | Absolute value. |
| `ceil` | 1 | number | Smallest integer not less than value. |
| `fixed` | 2 | number | Truncate to digits. |
| `floor` | 1 | number | Largest integer not greater than value. |
| `log` | 1-2 | number | Natural log or log with base. |
| `max` | 1-4 | number | Maximum input. |
| `min` | 1-4 | number | Minimum input. |
| `pow` | 2 | number | Power. |
| `rand` | 0 | number | Random 0 <= v < 1. |
| `round` | 1-2 | number | Round, optional digits. |

## String And Conversion

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `concat` | 1-4 | text | Concatenate strings. |
| `formatNumber` | 3 | text | Numeric display string. |
| `left` | 2 | text | Leftmost characters. |
| `len` | 1 | number | Array count or text length. |
| `lower` | 1 | text | Lowercase. |
| `number` | 1 | number | Convert to number, invalid/null to 0. |
| `repeat` | 2 | text | Repeat text. |
| `replace` | 4 | text | Replace all/first/last using type 1/0/2. |
| `right` | 2 | text | Rightmost characters. |
| `split` | 2 | array | Split text. |
| `strIndex` | 2-3 | number | Find text position. |
| `subString` | 3 | text | Substring. |
| `text` | 1 | text | Convert to text. |
| `trim` | 1 | text | Trim whitespace. |
| `upper` | 1 | text | Uppercase. |

## Date And Time

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `date` | 3-6 | date | Build date string from year/month/day and optional time. |
| `dateAdd` | 3 | date | Add year/month/day/hour/minute/second interval. |
| `dateDiff` | 4 | number | Difference between two dates. The third date-unit parameter must be a raw lowercase unit string such as `"year"` or `"day"`, not an expression string-token array. |
| `dateFormat` | 2 | text | Format date with tokens such as YYYY, MM, DD, HH, mm, ss. |
| `datePart` | 2 | number | Year, Quarter, Month, dayofweek, day, hour, minute, second. |
| `datePicker` | 2 | date | Date picker expression helper. |
| `day` | 1 | number | Day of month. |
| `hour` | 1 | number | Hour. |
| `minute` | 1 | number | Minute. |
| `month` | 1 | number | Month. |
| `now` | 0 | date | Current date/time. |
| `second` | 1 | number | Second. |
| `weekNum` | 1-2 | number | Week number, optional ISO flag. |
| `year` | 1 | number | Year. |

## Array

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `arrayAverage` | 1-4 | number | Optional column/filter column/filter value. |
| `arrayConcat` | 2 | array | Concatenate two arrays. |
| `arrayCount` | 1-4 | number | Count items/non-empty column values. |
| `arrayIndex` | 2 | number | Index of value, -1 if missing. |
| `arrayMax` | 1-4 | number | Max with optional column/filter. |
| `arrayMin` | 1-4 | number | Min with optional column/filter. |
| `arraySum` | 1-4 | number | Sum with optional column/filter. |
| `join` | 1-2 | text | Join array values. |
| `removeDuplicates` | 1 | array | Unique values. |

## Logical, System, Utility

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `iif` | 3 | unknown | Condition, then, else. |
| `isFalse` | 1 | boolean | False-like check. |
| `isNullOrEmpty` | 1 | boolean | Null or empty string. |
| `isTrue` | 1 | boolean | True-like check. |
| `currentUser` | 0 | text | Current user ID. |
| `UniqueID` | 0 | text | Unique ID. |
| `getAttr` | 2-3 | unknown | Object path lookup with optional default. |
| `JSONStringfy` | 1 | text | Export-backed Phase 2 spelling for serializing a value or query-result collection to JSON text. |

## Query Result Functions

These functions are used with Yeeflow Form Actions Phase 2 Query data steps.

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `arraySum` | 1-4 | number | Runtime-known array aggregate. Phase 2 export uses it against a temp query collection and selected column label `"Amount"`. |
| `JSONStringfy` | 1 | text | Export-backed collection display/debug function. Preserve this exact spelling until another export proves `JSONStringify` as a valid alias. |

Do not use `arraySub`; the aggregate function learned for query-result totals is `arraySum`.

`vLookup` remains deferred. The Phase 2 export contains `vLookup` only in button/action labels, not as an expression token.

## User, Department, And Location Attributes

These functions are export-backed from `Expression Runtime Test v1 Patch.yap` and runtime-tested in `Expression User Profile Test v1`.

| Function | Params | Returns | Notes |
| --- | --- | --- | --- |
| `getUserAttr` | 3 | unknown/text/date/object | `getUserAttr(userOrContext, attrDescriptor, defaultValue)`. The current-user context token uses `exprType: "application"`, `id: "CurrentUser"`, `valueType: "string"`, `type: "expr"`, and `name: "Context:Current User"`. |
| `getOrgAttr` | 3 | unknown/text/object | `getOrgAttr(departmentValue, attrDescriptor, defaultValue)`. Use this exact function for department/organization values; do not generate `getDeptAttr` yet. |
| `getLocAttr` | 3 | unknown/text/object | `getLocAttr(locationValue, attrDescriptor, defaultValue)`. Use only for location values returned from user attributes or other export-backed location expressions. |

Attribute parameters are direct descriptor objects, not plain strings and not one-item expression arrays:

```json
{ "key": "Email", "label": "Email" }
```

Fallback/default parameters are expression arrays, usually:

```json
[{ "type": "str", "value": "N/A" }]
```

Observed `getUserAttr` attributes include `Name_CN`, `SPAccount`, `Email`, `DepartmentID`, `LineManager`, `EmployeeNo`, `IsAdmin`, `Photo`, `JobTitle`, `JobGrade`, `OfficeAddress`, `Phone`, `Mobile`, `Telephone`, `Remark`, `LocationID`, `LastLoginTime`, `ServiceStartDate`, `LatestHireDate`, `Created`, and `CreatedBy`.

Observed `getOrgAttr` attributes include `Name`, `ParentID`, and `Manager`.

Observed `getLocAttr` attributes include `Name` and `Manager`.

Common recipes:

```json
[
  {
    "type": "func",
    "func": "getUserAttr",
    "params": [
      [{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }],
      { "key": "Email", "label": "Email" },
      [{ "type": "str", "value": "N/A" }]
    ]
  }
]
```

```json
[
  {
    "type": "func",
    "func": "getOrgAttr",
    "params": [
      [{ "type": "func", "func": "getUserAttr", "params": [[{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }], { "key": "DepartmentID", "label": "Department" }, [{ "type": "str", "value": "N/A" }]] }],
      { "key": "Name", "label": "Name" },
      [{ "type": "str", "value": "N/A" }]
    ]
  }
]
```

Boarding anniversary recipe:

```json
[
  {
    "type": "func",
    "func": "dateFormat",
    "params": [
      [{ "type": "func", "func": "dateAdd", "params": [[{ "type": "func", "func": "getUserAttr", "params": [[{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }], { "key": "LatestHireDate", "label": "Boarding Date" }, [{ "type": "str", "value": "N/A" }]] }], [{ "type": "str", "value": "year" }], [{ "type": "num", "value": "1" }]] }],
      [{ "type": "str", "value": "MMM DD, YYYY" }]
    ]
  }
]
```

Runtime correction from `Employee Family Implant v1 Core Patch 20260516`: a manually repaired Set variables step worked with `params[1]` as `{ "key": "Name_CN", "label": "Name" }`. Generated siblings that used `[{ "key": "...", "label": "..." }]` remained suspect and should be patched.

---

<a id="yeeflow-expression-generation-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-expression-generation-rules -->

# Yeeflow Expression Generation Rules

Use `yeeflow-expression-functions.normalized.json`, `yeeflow-expression-function-knowledge-base.normalized.json`, `yeeflow-expression-operators.normalized.json`, and `yeeflow-expression-utils.js` before generating expressions.

## Default Rules

- Generate expression arrays, not raw JavaScript.
- Use exact token keys from the reference: `exprType`, `valueType`, `id`, `type`, `name`, `op`, `func`, `params`, and `value`.
- Use variable tokens with `exprType: "variable"` and `type: "expr"`.
- Use workflow variable tokens with `exprType: "variable"` and value types `number`, `text`, `date`, or `boolean`.
- User/profile context exports may use application variable tokens such as `exprType: "application"`, `id: "CurrentUser"`, `valueType: "string"`, `type: "expr"`, and `name: "Context:Current User"`. Use only export-backed context tokens.
- Do not invent function names or operators.
- Use `iif` for conditional values.
- Use `isNullOrEmpty` for empty checks.
- Use `dateDiff`, `dateAdd`, and `dateFormat` for date logic.
- Use `arraySum`, `arrayCount`, `arrayAverage`, `arrayMin`, and `arrayMax` for list/sublist summaries.
- Use `concat` or `&` for string assembly.
- Use `formatNumber`, `fixed`, or `round` for numeric display.
- Use explicit readable lookup summary variables for user-facing output. Do not persist raw lookup row IDs into text fields unless the row ID is intentional.
- Use enriched `businessScenarios` and `keywords` only to select from known functions. They do not authorize inventing new functions or unsupported token shapes.
- Treat screenshot-observed functions such as `addWorkDays` and `addWorkHours` as metadata-pending until export-backed parameters are available.
- Runtime note from `Expression Runtime Test v1`: do not assign raw serialized expression token arrays directly to `SetVariableTask` text values for request numbers. Yeeflow displayed the raw JSON literally. Use an export-backed value shape such as the FlowNo expression-button pattern until a real SetVariable expression-token assignment is studied.
- Runtime note from `Expression Runtime Test v1`: workflow transition conditions need their exact outer wrapper from a working export. A locally valid numeric token array is not enough to claim workflow branch routing is runtime-proven.
- Runtime note from `Expression User Profile Test v1`: `getUserAttr`, `getOrgAttr`, `getLocAttr`, `dateFormat`, and nested `dateAdd` can render in generated approval forms and task pages. Tenant-missing profile values should use safe fallbacks. The decoded export used `getOrgAttr` for department/organization attributes; do not generate `getDeptAttr` until it is export-backed.
- Export-back note from updated `Approval Form Controls Test v6`: sub list row calculations use `exprType: "variable_ctx"` current-object tokens with `ctx` equal to the parent list variable id. Sub list summaries live in `attrs["list-fields-summary"]`; a numeric summary can bind to a top-level number variable through `{ "prefix": "__variables_", "value": "TotalAmount" }`, and that variable can drive workflow numeric `conditioninfo`. FlowKey/form keys must not collide with reserved JSON property names used by these binding objects; `EFI` corrupted `prefix` to `pr<runtimeFlowKey>x` during import replacement.
- Export-back note from `Implant Application Request (3).ywf`: `dateDiff` uses a raw lowercase date-unit string as its third parameter, for example `"year"`, not an expression string-token array. The broken generated shape `[{ "type": "str", "value": "Year" }]` rendered as `formcraft.formula.datetype.[object Object]`; the manually working export used `"year"` and displayed as `Year`.
- Export-back note from `Implant Application Request (5).ywf`: Query data filter right operands that reference workflow variables or calculations must be expression-token arrays with `showCus: false`. Direct-value mode with a frontend `<input ...>` expression-button HTML string is treated as a literal string and will not match query rows.
- Export-back note from `Implant Application Request (4).ywf`: workflow transition conditions can use expression-token arrays on the left operand, right operand, or both through operand wrapper `type: 2`. Direct selector operands use `type: 1`; static/selected/date values use `type: 0`. Do not assume workflow transitions only support old simple field-equals-value HTML-button strings.

## Function Selection By Business Intent

| Business intent | Generate with | Notes |
| --- | --- | --- |
| Display a currency or amount string | `formatNumber(amount, 2, 1)` | Returns text. Use only for display/persistence summaries, not numeric calculations. |
| Add a deadline offset | `dateAdd(startDate, "day", offset)` | Units from the enriched reference include `year`, `month`, `day`, `hour`, `minute`, `second`. |
| Detect overdue work | `dateDiff(dueDate, now(), "day", false) < 0` | In token arrays, encode the date unit as raw `"day"`, not `[{ "type": "str", "value": "day" }]`. Confirm the sign expectation in the target runtime before using for blocking validation. |
| Conditional text/value | `iif(condition, thenValue, elseValue)` | Use direct comparison instead of `iif` when a context expects boolean routing. |
| Required-field validation | `isNullOrEmpty(value) == false` | Prefer native required when the field is always required. |
| Request number | `concat(prefix, dateFormat(now(), "YYYYMMDD"), UniqueID())` or `&` tokens | Preserve exact `UniqueID` capitalization. |
| List total | `arraySum(lineItems, "LineTotal")` | Resolve list variable and column names first. |
| Sub list row subtotal | `Current object:Quantity * Current object:Unit Price` | Use `variable_ctx` tokens, not top-level workflow variables. |
| Workflow branch from sub list total | `TotalAmount > 5000` | Use the workflow `conditioninfo` wrapper with `n.>` / `n.<=` operators and the summary-bound number variable. |
| List count | `arrayCount(lineItems)` | Use optional column/filter params only when column names are proven. |
| Safe object lookup | `getAttr(object, "path.to.value", defaultValue)` | Useful for nested objects, but only when object-shaped values are proven. |
| Current user email/name/profile | `getUserAttr(Context:Current User, attr, fallback)` | Attribute params are direct descriptor objects such as `{ "key": "Email", "label": "Email" }`, not one-item expression arrays. |
| Department/parent department | `getOrgAttr(getUserAttr(Context:Current User, Department, fallback), attr, fallback)` | Exact exported function is `getOrgAttr`, not `getDeptAttr`. |
| Location name/manager | `getLocAttr(getUserAttr(Context:Current User, Location, fallback), attr, fallback)` | Environment-dependent when tenant location data is empty. |
| Boarding anniversary | `dateFormat(dateAdd(getUserAttr(Context:Current User, Boarding Date, fallback), "year", 1), "MMM DD, YYYY")` | Runtime-tested shape; value quality depends on the current user's Boarding Date data. |
| Text cleanup | `trim`, `replace`, `lower`, `upper` | Use for normalized summaries and comparisons. |
| Duplicate removal | `removeDuplicates(arrayValue)` | Use after the array variable is resolved. |

## Common Patterns

Subtotal:

```json
[
  { "exprType": "variable", "valueType": "number", "id": "Quantity", "type": "expr", "name": "Workflow Variables:Quantity" },
  { "type": "op", "op": "*" },
  { "exprType": "variable", "valueType": "number", "id": "UnitPrice", "type": "expr", "name": "Workflow Variables:Unit Price" }
]
```

Show field when switch is true:

```json
[
  { "exprType": "variable", "valueType": "boolean", "id": "RequiresJustification", "type": "expr", "name": "Workflow Variables:Requires Justification" },
  { "type": "op", "op": "==" },
  { "type": "bool", "value": true }
]
```

Route workflow when amount is high:

```json
[
  { "exprType": "variable", "valueType": "number", "id": "TotalAmount", "type": "expr", "name": "Workflow Variables:Total Amount" },
  { "type": "op", "op": ">=" },
  { "type": "num", "value": "5000" }
]
```

Generated request number:

```json
[
  { "type": "str", "value": "REQ-" },
  { "type": "op", "op": "&" },
  { "type": "func", "func": "dateFormat", "params": [[{ "type": "func", "func": "now", "params": [] }], [{ "type": "str", "value": "YYYYMMDD" }]] },
  { "type": "op", "op": "&" },
  { "type": "str", "value": "-" },
  { "type": "op", "op": "&" },
  { "type": "func", "func": "UniqueID", "params": [] }
]
```

For app-level request numbers in workflow actions, this token recipe is only the expression logic. Do not serialize this array into a `SetVariableTask` value unless that exact assignment shape has been proven by export/runtime. In the first runtime test, the safe generated package used Yeeflow's FlowNo expression-button value shape for `Request No`.

Overdue check:

```json
[
  { "type": "func", "func": "dateDiff", "params": [[{ "exprType": "variable", "valueType": "date", "id": "DueDate", "type": "expr", "name": "Workflow Variables:Due Date" }], [{ "type": "func", "func": "now", "params": [] }], "day", [{ "type": "bool", "value": false }]] },
  { "type": "op", "op": "<" },
  { "type": "num", "value": "0" }
]
```

Amount formatting:

```json
[
  { "type": "func", "func": "formatNumber", "params": [[{ "exprType": "variable", "valueType": "number", "id": "Amount", "type": "expr", "name": "Workflow Variables:Amount" }], [{ "type": "num", "value": "2" }], [{ "type": "num", "value": "1" }]] }
]
```

Required-field validation with explicit false check:

```json
[
  { "type": "func", "func": "isNullOrEmpty", "params": [[{ "exprType": "variable", "valueType": "text", "id": "Reason", "type": "expr", "name": "Workflow Variables:Reason" }]] },
  { "type": "op", "op": "==" },
  { "type": "bool", "value": false }
]
```

List total:

```json
[
  { "type": "func", "func": "arraySum", "params": [[{ "exprType": "variable", "valueType": "number", "id": "LineItems", "type": "expr", "name": "Workflow Variables:Line Items" }], [{ "type": "str", "value": "LineTotal" }]] }
]
```

Sub list current object row subtotal:

```json
[
  { "exprType": "variable_ctx", "valueType": "number", "id": "LineQuantity", "ctx": "LineItems", "type": "expr", "name": "Current object:Quantity" },
  { "type": "op", "op": "*" },
  { "exprType": "variable_ctx", "valueType": "number", "id": "LineUnitPrice", "ctx": "LineItems", "type": "expr", "name": "Current object:Unit Price" }
]
```

Workflow numeric condition wrapper for a summary-bound total:

```json
{
  "pre": "and",
  "left": {
    "type": 1,
    "value": {
      "exprType": "variable",
      "valueType": "number",
      "id": "TotalAmount",
      "type": "expr"
    }
  },
  "op": "n.>",
  "right": {
    "type": 2,
    "value": [{ "type": "num", "value": "5000" }]
  },
  "group": "number"
}
```

Text cleanup:

```json
[
  { "type": "func", "func": "upper", "params": [[{ "type": "func", "func": "trim", "params": [[{ "exprType": "variable", "valueType": "text", "id": "ProductCode", "type": "expr", "name": "Workflow Variables:Product Code" }]] }]] }
]
```

Current user email:

```json
[
  {
    "type": "func",
    "func": "getUserAttr",
    "params": [
      [{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }],
      { "key": "Email", "label": "Email" },
      [{ "type": "str", "value": "N/A" }]
    ]
  }
]
```

Current user department name:

```json
[
  {
    "type": "func",
    "func": "getOrgAttr",
    "params": [
      [{ "type": "func", "func": "getUserAttr", "params": [[{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }], { "key": "DepartmentID", "label": "Department" }, [{ "type": "str", "value": "N/A" }]] }],
      { "key": "Name", "label": "Name" },
      [{ "type": "str", "value": "N/A" }]
    ]
  }
]
```

Profile function parameter rule from `Employee Family Implant v1 Core Patch 20260516`: `getUserAttr`, `getOrgAttr`, and `getLocAttr` use `params[1]` as a direct descriptor object. The broken generated shape wrapped the descriptor as `[{ "key": "...", "label": "..." }]` and failed in a Set variables form action.

## Form Action Expressions

Form Actions Phase 1 proves that form action steps use the same expression-token array model for Set variable values, step conditions, and confirm dialog messages.

Export-backed contexts:

- `setvar.attrs.setvar_val`: expression-token array for the value assigned to one target variable
- `setvar.attrs.setvar_array[].value`: expression-token array for a multi-set value
- `step.condition`: expression-token array controlling whether a step runs
- `confirm.attrs.confirm_qs`: expression-token array for the confirmation message
- `heading.attrs.headc.title.variable`: expression-token array for displaying temp-variable results

Temp variables are referenced as normal variable tokens with the generated temp prefix:

```json
{
  "exprType": "variable",
  "valueType": "string",
  "id": "__temp_var_DialogResult",
  "type": "expr",
  "name": "var_DialogResult"
}
```

Generation rules:

- Declare temp variables in `variables.tempVars[]` before referencing `__temp_` tokens.
- Keep workflow variables unprefixed.
- Validate form action expressions with the same expression utility used for calculated controls and workflow conditions.
- Do not confuse form action `setvar` steps with workflow graph `SetVariableTask` nodes; their JSON shapes are different.

## Query Result Expressions

Form Actions Phase 2 proves query-result expressions in form action flows.

Export-backed aggregate recipe:

```json
[
  {
    "type": "func",
    "func": "arraySum",
    "params": [
      [
        {
          "exprType": "variable",
          "valueType": "string",
          "id": "__temp_var_CollectionofQueryItems",
          "type": "expr",
          "name": "var_CollectionofQueryItems"
        }
      ],
      [{ "type": "str", "value": "Amount" }],
      [],
      []
    ]
  }
]
```

Use this when a `querydata` step stores selected fields into a temp collection and the aggregate result is copied into a workflow number variable.

For debugging/display of a temp query collection, the Phase 2 export uses the exact function token `JSONStringfy`:

```json
[
  {
    "type": "func",
    "func": "JSONStringfy",
    "params": [
      [
        {
          "exprType": "variable",
          "valueType": "string",
          "id": "__temp_var_CollectionofQueryItems",
          "type": "expr",
          "name": "var_CollectionofQueryItems"
        }
      ]
    ]
  }
]
```

Do not rename this function to `JSONStringify` unless a future export proves that exact spelling as a function token. Do not use `arraySub`; use `arraySum`.

`vLookup` remains deferred because the Phase 2 export contains it only in labels, not in expression tokens.

## Validation Standard

Run expression smoke tests after changing helpers or references:

```bash
node scripts/smoke-expression-validation.mjs
```

Generated packages should run existing package/form/list/workflow validators. Expression-related findings are warning-first unless JSON is structurally invalid, a generated final package references an unknown function/operator, or a variable token is missing required fields.

---

<a id="yeeflow-expression-reference-reconciliation"></a>

<!-- source: docs/collection-04.md#yeeflow-expression-reference-reconciliation -->

# Yeeflow Expression Reference Reconciliation

This pass enriches the first expression foundation baseline with `expression training data generator_2.txt` and the supplied expression-editor screenshots. It does not replace the first-pass token model or validator source of truth.

## What Already Existed

The first pass established:

- expression token arrays for literals, variables, operators, and functions
- normalized function and operator references
- expression utility validation
- smoke tests for arithmetic, string, date, logical, array, system, and object functions
- generation rules for calculated controls, dynamic display rules, lookup/data filters, workflow transition conditions, request numbers, and list totals
- the dedicated `yeeflow-expression-generator` skill

The first-pass function validator source remains `yeeflow-expression-functions.normalized.json`.

## What The Second File Adds

The second file includes a structured function knowledge base with 54 entries. Each entry can provide:

- bilingual Chinese/English description
- category
- call-format sketch
- parameter names, types, and descriptions
- return type
- example expression string
- business scenarios
- keywords

This metadata has been merged into `yeeflow-expression-functions.normalized.json` for functions already present in the baseline, and preserved separately in `yeeflow-expression-function-knowledge-base.normalized.json` for richer generation guidance.

## Reconciliation Summary

| Item | Result |
| --- | --- |
| Baseline functions | 55 |
| Second-file knowledge-base functions | 54 |
| Functions enriched | 54 |
| Present in baseline but missing from second file | `strIndex` |
| Present in second file but missing from baseline | none |
| Screenshot-observed but metadata pending | `addWorkDays`, `addWorkHours` |

## Exact Function Names

Preserve exact runtime function names from the normalized reference. Do not silently rename functions based on lowercase headings, translations, or UI labels.

Important cases:

- `strIndex` remains camel-case. The second-file knowledge base omits it, so keep the first-pass name and parameter range.
- `UniqueID` remains capitalized exactly as shown.
- `subString` remains camel-case exactly as shown.
- `datePicker` is retained, but its knowledge-base call-format includes a primitive boolean placeholder, so primitive params should remain warning-level unless export-backed in the target context.

## Screenshot-Observed Functions

The Function tab screenshot shows `addWorkDays` and `addWorkHours`. These are not present in either normalized baseline or the second-file knowledge base.

Current status:

- observed in the expression editor UI
- not generation-safe yet
- do not add to generated expressions until an export-backed token example or function metadata confirms parameter shape and return type

## Known Typo And Malformed Example Risks

- Some first-pass prose contained a `strindex` spelling; use `strIndex`.
- Some training examples use expression strings for human explanation. Generators must convert these into Yeeflow JSON token arrays before writing packages.
- Some function `call_format` examples are sketches, not always exact package-ready tokens.
- Primitive function params are allowed only when proven for the specific function/context. Otherwise use nested expression arrays.

## Categories Added

The second-file knowledge base contributes category labels:

- `array`
- `conversion`
- `date`
- `logic`
- `math`
- `object`
- `string`
- `system`
- `validation`

These labels are useful for function selection. They do not override first-pass validator categories unless the exact runtime function and parameter shape are already known.

## Deferred Uncertainties

- `addWorkDays` and `addWorkHours` need export-backed examples.
- UI condition-builder wrappers still need per-context export evidence before generator code writes full wrapper objects.
- Lookup/data filter expressions should use the studied filter-condition wrapper for the target control, not only the nested expression token array.

---

<a id="yeeflow-expression-use-cases"></a>

<!-- source: docs/collection-04.md#yeeflow-expression-use-cases -->

# Yeeflow Expression Use Cases

## Calculated Controls

Use calculated controls for formulas such as `Subtotal = Quantity * Unit Price`. Avoid editable inputs for calculated-looking values.

Recommended expression:

```json
[
  { "exprType": "variable", "valueType": "number", "id": "Quantity", "type": "expr", "name": "Workflow Variables:Quantity" },
  { "type": "op", "op": "*" },
  { "exprType": "variable", "valueType": "number", "id": "UnitPrice", "type": "expr", "name": "Workflow Variables:Unit Price" }
]
```

Use `formatNumber` only after calculation when the target is display text. Keep numeric controls numeric while workflow rules or ContentList mappings still need numeric values.

## Dynamic Display Conditions

Use boolean/comparison expressions in `attrs.control_display` wrapper structures learned from exports. The expression itself should use standard variable/operator tokens.

Example: show reason when amount exceeds threshold:

```json
[
  { "exprType": "variable", "valueType": "number", "id": "Amount", "type": "expr", "name": "Workflow Variables:Amount" },
  { "type": "op", "op": ">" },
  { "type": "num", "value": "10000" }
]
```

Screenshot evidence confirms Dynamic display rules are configured from the selected control settings panel. Generators should preserve the export-backed wrapper shape around the nested expression.

## Custom Validation

Use expression tokens when a validation rule needs comparison or empty checks. Keep simple required validation in native required flags when possible.

Example: date must not be earlier than today:

```json
[
  { "type": "func", "func": "dateDiff", "params": [[{ "exprType": "variable", "valueType": "date", "id": "RequiredDate", "type": "expr", "name": "Workflow Variables:Required Date" }], [{ "type": "func", "func": "now", "params": [] }], "day", [{ "type": "bool", "value": false }]] },
  { "type": "op", "op": ">=" },
  { "type": "num", "value": "0" }
]
```

Screenshot evidence confirms Custom validation is a control-level Validation section entry point. Use this for conditional or cross-field validation only; keep simple required validation in native properties.

## Lookup And Data Filters

For lookup filters, preserve the wrapper shape from the target lookup/filter property and validate the expression token array inside it.

Example: only active records:

```json
[
  { "exprType": "variable", "valueType": "boolean", "id": "Active", "type": "expr", "name": "Workflow Variables:Active" },
  { "type": "op", "op": "==" },
  { "type": "bool", "value": true }
]
```

Lookup/data filter screenshots show condition-builder rows with field, operator, value, expression-toggle, delete, and drag controls. The nested expression token model is reusable, but the outer condition row object should be copied from an export-backed lookup/filter pattern.

## Workflow Transition Conditions

Use expression tokens for generated conditional routing only with the export-backed workflow transition operand wrapper. `Implant Application Request (4).ywf` proves each transition condition row can put either side in expression-editor mode with `type: 2`.

Example: route to Finance when total amount is at least 5000:

```json
[
  { "exprType": "variable", "valueType": "number", "id": "TotalAmount", "type": "expr", "name": "Workflow Variables:Total Amount" },
  { "type": "op", "op": ">=" },
  { "type": "num", "value": "5000" }
]
```

Workflow screenshots show transition conditions are configured on selected sequence/transition arrows. Generated workflow conditions must remain Yeeflow expression tokens and must not use raw JavaScript.

Pattern choices:

- Direct variable/field left + static option right: use `left.type = 1`, `right.type = 0`.
- Direct variable/field left + expression right: use `left.type = 1`, `right.type = 2`.
- Expression left + static/date/option right: use `left.type = 2`, `right.type = 0`.
- Expression left + expression right: use `left.type = 2`, `right.type = 2`.

Keep the `dateDiff(..., "year", [])` raw date-unit rule inside transition expression operands; do not wrap the date unit as a string token array.

## String Formatting

Use `concat` or `&` for generated request numbers and display text. Use `formatNumber` for currency-like display strings and `dateFormat` for date labels.

Common recipes:

- request number: prefix + `dateFormat(now(), "YYYYMMDD")` + `UniqueID()`
- normalized text: `upper(trim(value))`
- cleanup: `replace(value, " ", "-", 1)` when all spaces should be replaced

Runtime caution from `Expression Runtime Test v1`: a request-number expression recipe is not automatically a valid `SetVariableTask` value. Serializing the token array into a text variable caused Yeeflow to display the raw JSON. Until a working export proves the SetVariable expression assignment wrapper, use the proven FlowNo expression-button pattern for workflow-generated request numbers.

## Runtime-Proven v1 Contexts

`Expression Runtime Test v1 Patch` proved these contexts together in Yeeflow:

- calculated control: `Quantity * Unit Price`
- dynamic display: show High Value Reason when Total Amount is greater than 10000
- lookup data filter: Active products only
- lookup addition/autofill into readable summary variables
- FlowNo request number generation
- simple Finance Review task
- ContentList persistence of readable request values

Workflow transition branching by numeric expression remains deferred because the first isolation package did not route as expected. Treat workflow branch conditions as export-wrapper-sensitive.

## User/Profile Expressions

`Expression Runtime Test v1 Patch.yap` and the generated `Expression User Profile Test v1` package prove a focused user/profile expression family in approval forms:

- `getUserAttr(Context:Current User, Name, N/A)` and related current-user attributes render on submission and task pages.
- `getOrgAttr(getUserAttr(Context:Current User, Department, N/A), Name, N/A)` renders the current user's department name.
- Nested `getOrgAttr(..., Parent, ...)` can render the parent department name when tenant organization data exists.
- `getUserAttr(getUserAttr(Context:Current User, Line Manager, N/A), Name, N/A)` can render line manager display text.
- `getLocAttr(getUserAttr(Context:Current User, Location, N/A), Name, N/A)` is valid but environment-dependent when the tenant user has no location value.
- `dateFormat(getUserAttr(Context:Current User, Created Time, N/A), "MMM DD, YYYY")` rendered a formatted current-user Created Time.
- `dateFormat(dateAdd(getUserAttr(Context:Current User, Boarding Date, N/A), "year", 1), "MMM DD, YYYY")` rendered with the generated nested shape, but in the tested tenant the Boarding Date value itself was blank, so treat populated boarding-date arithmetic as data-dependent until tested with a profile that has that attribute.

Generation notes:

- Preserve exact function name `getOrgAttr`; do not generate `getDeptAttr` from the user-facing label.
- Attribute parameters are direct descriptor objects with `key` and `label`, not plain string literals and not one-item expression arrays. The export-backed Set variables fix in `Employee Family Implant v1 Core Patch 20260516` failed with wrapped profile descriptors and worked with a direct descriptor object.
- Context current-user tokens are application tokens, not workflow variable tokens.
- Use fallback arrays such as `[{ "type": "str", "value": "N/A" }]` for optional tenant profile fields.
- Persist readable profile summary values through variables and ContentList only after render proof; do not persist object-shaped profile values directly to text fields.

## List/Sublist Summaries

Use `arraySum`, `arrayCount`, `arrayAverage`, `arrayMin`, and `arrayMax` for list/sublist summary formulas when the list variable and column names are resolved.

For nested objects, use `getAttr(object, "path.to.value", defaultValue)` only after object-shaped values are proven in the target export. For duplicate cleanup, use `removeDuplicates(arrayValue)` only after the array variable is resolved.

## Sub List Current Object And Bound Summaries

The updated `Approval Form Controls Test v6` export proves the native list summary pattern for approval forms.

Use it when an approval form has line items and each row needs a calculated value:

- Row fields: Product, Quantity, Unit Price, Sub Total, Line Note
- Row expression: `Current object:Quantity * Current object:Unit Price`
- Quantity summary: `field = LineQuantity`, `type = total`
- Unit price summary: `field = LineUnitPrice`, `type = avg`
- Total amount summary: `field = LineSubTotal`, `type = total`, bound to `TotalAmount`

Generation notes:

- Current row values use `exprType: "variable_ctx"` and `ctx` equal to the parent list variable id.
- Summary values live in the parent list control `attrs["list-fields-summary"]`.
- Bind summary values to top-level workflow variables when they need to drive display, validation, ContentList persistence, or workflow routing.
- Prefer list summary binding for totals instead of manually recalculating line items with a top-level formula.
- Persist a readable line item summary plus numeric total fields until direct row-to-child-list persistence is separately proven.

Workflow branch example:

- `TotalAmount > 5000` routes to department manager approval.
- `TotalAmount <= 5000` routes to line manager approval.

Use the export-backed workflow `conditioninfo` wrapper with numeric operators such as `n.>` and `n.<=`.

## Query Data Form Action Results

The manually updated `Form Actions Phase 1 Test v1 Runtime.yap` export proves query-result expressions in form actions.

Use cases:

- Load multiple data-list records into a form list variable.
- Load one source record into workflow variables.
- Store a query result count in a temp variable for display.
- Store selected query fields in a temp collection variable for aggregation.
- Use `arraySum` to total a numeric selected field in the temp collection.
- Use `JSONStringfy` to display/debug the temp collection.

Safe aggregate recipe:

```json
[
  {
    "type": "func",
    "func": "arraySum",
    "params": [
      [{ "exprType": "variable", "valueType": "string", "id": "__temp_var_CollectionofQueryItems", "type": "expr", "name": "var_CollectionofQueryItems" }],
      [{ "type": "str", "value": "Amount" }],
      [],
      []
    ]
  }
]
```

Generation notes:

- Select the source fields explicitly in the preceding `querydata` step.
- Use the selected field display/key, such as `"Amount"`, as the `arraySum` column parameter.
- Store the aggregate in a workflow number variable when it must be submitted, persisted, or used by workflow.
- Do not use `arraySub`.
- Do not generate `vLookup` yet; the function label was observed, but the function token was not present in the export.

---

<a id="yeeflow-field-configuration-reference"></a>

<!-- source: docs/collection-04.md#yeeflow-field-configuration-reference -->

# Yeeflow Field Configuration Reference

Sources studied read-only:

- `<downloads>/field-configurations.json`

The source contains 35 configuration entries covering 27 field/control types.

## Support Groups

| Group | Field/control types |
| --- | --- |
| provenSafe | currency, datepicker, input, input_number, lookup, radio, switch, textarea |
| schemaSupportedRuntimeUnproven | autonumber, checkbox, hyperlink, percent, rate, richtext, tag, time |
| environmentDependent | cost-center-picker, file-upload, icon-upload, identity-picker, location-picker, organization-picker, signer |
| riskyDeferred | calculated-column, list, metadata, mutiple-metadata |
| schemaSupportedUnclassified | - |

## Common Properties

Common data-list field properties include:

- `Type`: Yeeflow field control type.
- `DisplayName`: user-facing field name.
- `DefaultValue`: field default.
- `Rules.required`: required behavior.
- `Rules.placeholder`: input placeholder.
- `IsUnique`: uniqueness constraint.
- `Rules.choices`: choice options.
- `Rules.number_min`, `Rules.number_max`, `Rules.number_step`, `Rules.rounded-to`: numeric settings.
- `Rules.currencyCode`, `Rules.displayFormat`, `Rules.displayThousandths`: currency/number display.
- `Rules.dateformat`, `Rules.date_picker`, `Rules.showtime`, `Rules.minuteStep`: date/time settings.
- `Rules.listid`, `Rules.listsetid`, `Rules.listfield`: lookup metadata when present in generated fields.

## Field Inventory

| Field/control type | Support level | Key property paths |
| --- | --- | --- |
| autonumber | schema-supported-runtime-unproven | DisplayName, Rules.minDigits, Rules.prefix.value, Rules.startNum, Rules.suffix.value, Type |
| calculated-column | risky-deferred | Rules.calculated, Rules.calculated_result.attrs, Rules.calculated_result.type |
| checkbox | schema-supported-runtime-unproven | DefaultValue, DisplayName, Rules.choices, Rules.displayStyle, Rules.placeholder, Rules.required, Type |
| cost-center-picker | environment-dependent | DefaultValue, DisplayName, IsUnique, Rules.identity-maxselection, Rules.multiple, Rules.placeholder, Rules.required, Type |
| currency | proven-safe | DefaultValue, DisplayName, IsUnique, Rules.currencyCode, Rules.displayFormat, Rules.displayThousandths, Rules.number_max, Rules.number_min, Rules.placeholder, Rules.required, Rules.rounded-to, Type |
| datepicker | proven-safe | DefaultValue, DisplayName, IsUnique, Rules.date_picker, Rules.dateformat, Rules.disable_now_button, Rules.minuteStep, Rules.placeholder, Rules.required, Rules.showtime, Type |
| file-upload | environment-dependent | DefaultValue, DisplayName, Rules.file_maxcount, Rules.file_multiple, Rules.file_types.cusValue, Rules.file_types.value, Rules.file_typeslimit, Rules.maxsize, Rules.required, Type |
| hyperlink | schema-supported-runtime-unproven | DefaultValue, DisplayName, Rules.hyperlink_buttonname, Rules.hyperlink_open, Rules.required, Type |
| icon-upload | environment-dependent | DefaultValue, DisplayName, Rules.controlmultiple, Rules.maxselection, Rules.picture_size_limit, Rules.required, Type |
| identity-picker | environment-dependent | DefaultValue, DisplayName, IsUnique, Rules.identity-maxselection, Rules.multiple, Rules.placeholder, Rules.required, Type |
| input | proven-safe | DefaultValue, DisplayName, IsUnique, Rules.allowScan, Rules.encrypt, Rules.input-maxlength, Rules.placeholder, Rules.required, Type |
| input_number | proven-safe | DefaultValue, DisplayName, IsUnique, Rules.displayThousandths, Rules.number_max, Rules.number_min, Rules.number_step, Rules.placeholder, Rules.required, Rules.rounded-to, Type |
| list | risky-deferred | DisplayName, Rules.list-variables, Rules.required, Type |
| location-picker | environment-dependent | DefaultValue, DisplayName, IsUnique, Rules.placeholder, Rules.required, Type |
| lookup | proven-safe | DisplayName, IsUnique, Rules.addition, Rules.appid, Rules.displayStyle, Rules.list_tooltip_field, Rules.listfield, Rules.listfilter, Rules.listid, Rules.listsetid, Rules.max-selection, Rules.multiple, Rules.placeholder, Rules.required, Rules.search-fields, Rules.search-scope, Rules.sort-first.SortByDesc, Rules.sort-first.SortName |
| metadata | risky-deferred | DefaultValue, DisplayName, IsUnique, Rules.categoryId, Rules.metadata-treeselect, Rules.parentId, Rules.placeholder, Rules.required, Rules.source, Type |
| mutiple-metadata | risky-deferred | DefaultValue, DisplayName, Rules.categoryId, Rules.max-selection, Rules.metadata-treeselect, Rules.parentId, Rules.placeholder, Rules.required, Rules.source, Type |
| organization-picker | environment-dependent | DefaultValue, DisplayName, IsUnique, Rules.hierarchical-select, Rules.identity-maxselection, Rules.metadata-treeselect, Rules.multiple, Rules.parentId, Rules.placeholder, Rules.required, Type |
| percent | schema-supported-runtime-unproven | DefaultValue, DisplayName, IsUnique, Rules.number_max, Rules.number_min, Rules.placeholder, Rules.required, Rules.rounded-to, Type |
| radio | proven-safe | DefaultValue, DisplayName, IsUnique, Rules.choices, Rules.color_choices, Rules.displayStyle, Rules.placeholder, Rules.required, Rules.show_color, Type |
| rate | schema-supported-runtime-unproven | DefaultValue, DisplayName, Rules.icon, Rules.rate-allowHalf, Rules.rate-count, Rules.rate-type, Rules.required, Type |
| richtext | schema-supported-runtime-unproven | DisplayName, Rules.required, Rules.richtext-type, Type |
| signer | environment-dependent | DisplayName, Rules.required, Type |
| switch | proven-safe | DefaultValue, DisplayName, IsUnique, Rules.displayStyle, Type |
| tag | schema-supported-runtime-unproven | DisplayName, Rules.category, Rules.customTags, Rules.multiple, Rules.process-field-tag-maxcount, Rules.required, Rules.source, Type |
| textarea | proven-safe | DefaultValue, DisplayName, Rules.input-maxlength, Rules.placeholder, Rules.required, Type |
| time | schema-supported-runtime-unproven | DefaultValue, DisplayName, IsUnique, Rules.dateformat, Rules.minuteStep, Rules.placeholder, Rules.required, Type |

## Generator Guidance

- Preserve native `Title` metadata for generated lists.
- Use Text/input, Text/textarea, Decimal/input_number, Text/radio, Bit/switch, Datetime/datepicker, and local lookup as the safest generated field families.
- Keep file, icon, identity, organization, location, cost-center, signer, metadata, and calculated-column shapes warning-first until focused runtime proof exists.
- Keep lookup sample values blank unless local target sample IDs are proven and included in `ReplaceIds`.

---

<a id="yeeflow-form-action-generation-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-form-action-generation-rules -->

# Yeeflow Form Action Generation Rules

These rules cover Yeeflow Form Actions learned from manually updated runtime exports.

Phase 1 source: `Expression Sublist Summary Workflow Test v1.yap`.

Phase 2 source: `Form Actions Phase 1 Test v1 Runtime.yap`.

Focused v1.1 source: `Approval form workflow sample-v1.1.yapk`, Approval Submission form action `Query data sample`.

## Scope

Phase 1 covers:

- action buttons
- button click triggers
- page-load triggers
- temp variables
- `setvar` action steps
- `confirm` action steps

The V1.7 Open Resource training supersedes the earlier `listitem` deferral: generated Approval, Data List/Document Library, and Dashboard Form Actions may use the shared `listitem`, `openform`, and `opendashboard` builder only when the matching planning and package hard gates pass.

Phase 2 covers:

- `querydata` action steps
- query multiple items
- query single item
- query result count temp variables
- query result collection temp variables
- query result mapping to form `list` variables
- `arraySum` over query result collections
- `submit` action steps
- Save changes submit mode

Custom Code action step status:

- Runtime script contract is documented in `docs/standards/collection-01.md#custom-code-form-action-step-runtime-standard`.
- A Form Action Custom Code step uses `execute(context, fieldsValues)` as its business entrypoint.
- It must not be generated from the Custom Code control `render(...)` contract.
- The package JSON storage shape for this step type is not yet export-proven in this standard. Until a decoded export proves the step shape, generated package materialization for Custom Code action steps must stay gated and must not be claimed as final-package ready.

## Form Action Location

Form actions live on a page form definition:

```json
{
  "formdef": {
    "actions": [
      { "id": "uuid", "name": "Action name", "steps": [] }
    ],
    "formAction": {
      "onLoad": "uuid"
    }
  }
}
```

Use UUIDs for action ids and business-readable action names.

## Button Click Trigger

Action buttons trigger form actions through `attrs.control_action`:

```json
{
  "type": "action_button",
  "label": "Set default request title",
  "attrs": {
    "button-style": "3",
    "common": {
      "positioning": { "widthtype": [null, "2"] }
    },
    "control_action": "uuid"
  }
}
```

Rules:

- The target action must exist in the same page `formdef.actions[]`.
- Action buttons should use inline width by default.
- Generated action buttons must have meaningful `nv_label` values.
- Avoid attaching action wiring until the action validates.

## Page Load Trigger

Page load uses:

```json
{
  "formAction": {
    "onLoad": "uuid"
  }
}
```

Use page-load actions for safe initialization:

- default title
- temp status text
- default notes
- requester/current-user display values

Do not use page-load for data writes or external calls in Phase 1.

## Set Variable Step

Single variable:

```json
{
  "type": "setvar",
  "name": "Set request title",
  "attrs": {
    "setvar_var": { "exprType": "variable", "valueType": "text", "id": "RequestTitle", "type": "expr", "name": "Workflow Variables:Request Title" },
    "setvar_val": [{ "type": "str", "value": "Default title" }]
  }
}
```

Multiple variables:

```json
{
  "type": "setvar",
  "name": "Set default values",
  "attrs": {
    "setvar_multi": true,
    "setvar_array": [
      {
        "var": { "exprType": "variable", "valueType": "text", "id": "Notes", "type": "expr", "name": "Workflow Variables:Notes" },
        "value": [{ "type": "str", "value": "Default notes" }]
      }
    ]
  }
}
```

Rules:

- Values are expression-token arrays.
- Targets may be workflow variables or temp variables.
- Temp variable target ids use `__temp_` prefix in expression tokens.
- A step may include `condition` expression tokens.
- Use multi-value Set variables only when assignments are independent and order does not matter.
- Keep dependent calculations in separate ordered Set variable steps. For example, set a numeric boarding-year value before setting an eligibility status that compares that value.

## Field Change Triggers For Driver Fields

When a key driver field changes, bind a form action to that field instead of relying only on page load.

Examples:

- editable Requester / Applicant picker for proxy submission
- application type selector that controls family/quota sections
- product sublist or quantity changes that affect total amount

Requester/applicant rule:

- The requester/applicant field may default to Current User and remain editable when proxy submission is allowed.
- If the requester/applicant value changes, rerun applicant profile snapshot and dependent quota/policy calculations.
- If a configuration list such as Attachment Requirement Rules is included in v1, form actions should query it or otherwise use it for visible guidance/validation. Do not include a maintained configuration list that the form never reads.
- If a field or derived variable drives workflow routing, generate it as required, readonly auto-derived, or protected by a workflow fallback route. Do not leave route-driving flags blank and nullable without coverage.
- Do not use Context:Current User for applicant profile reads after the requester/applicant value is set.
- Snapshot/profile fields populated from the requester/applicant should be readonly unless HR is intentionally allowed to correct them.

## Show Confirm Dialog Step

```json
{
  "type": "confirm",
  "attrs": {
    "confirm_qs": [{ "type": "str", "value": "Confirm this action?" }],
    "confirm_rs": { "exprType": "variable", "valueType": "string", "id": "__temp_var_DialogResult", "type": "expr", "name": "var_DialogResult" }
  }
}
```

Rules:

- Store confirm result in a temp variable.
- Show the result with a Text/heading control using `attrs.headc.title.variable`.
- Runtime result semantics need the focused Phase 1 generated test before broad promises.

## Step Execution Condition Flow

Phase 3 source: `Implant Application Request (2).ywf`.

When a form action step has an execution condition, the designer checkbox `Continue next step when condition is not met` is stored on the step as:

```json
{
  "condition": [],
  "continue": true
}
```

Default behavior:

- If the condition is met, the step executes.
- If the condition is not met and `continue` is missing or false, the step is skipped and the remaining action flow stops.
- This means a conditional warning step before submit can accidentally block valid submissions when the warning condition is false.

Continue behavior:

- If the condition is not met and `continue: true`, the step is skipped and the action continues to the next step.

Submit guard pattern:

1. Run the reusable check action.
2. Add the conditional warning/confirm step for the invalid case.
3. Set `continue: true` on that warning/confirm step.
4. Place the native `submit` step after it.
5. Put the valid-path condition on the submit step when the invalid path must remain blocked.

For Employee Family Implant v1, `Warn when family quota is exceeded` must use `continue: true`; otherwise family requests within quota skip the warning but stop before reaching `Submit when quota is valid or not a family request`.

## Query Data Step

Use the detailed rules in `docs/collection-04.md#yeeflow-form-action-query-data-step-rules` and the normalized modes/templates in `docs/standards/collection-02.md#form-action-query-data-golden-reference-standard`.

Export-backed `querydata` step shape:

```json
{
  "type": "querydata",
  "attrs": {
    "querydata_list": { "AppID": 41, "ListSetID": "2055176306075381761", "ListID": "2055176313546620929", "ListType": 1 },
    "querydata_type": "multiple",
    "querydata_fieldmap": { "Title": "field_Title" },
    "querydata_listname": "FormActions",
    "querydata_vartype": "list",
    "querydata_listname_parent": "__variables_",
    "querydata_fields": null,
    "querydata_totalcount": "var_TotalQueryItems",
    "querydata_totalparent": "__temp_",
    "querydata_pagesize": 200
  }
}
```

Rules:

- Use `querydata_type = "multiple"` when mapping rows into a form list/sub list or temp collection.
- The v1.1 export omits `querydata_type` for the default single-record mode. Explicit `querydata_type = "single"` from older proven exports remains accepted.
- Use explicit `querydata_fieldmap` and verify every mapped target variable or list row field exists.
- Store display-only counts in temp variables via `querydata_totalcount` and `querydata_totalparent = "__temp_"`.
- Store transient query collections in temp variables only when they are used for display or client-side expressions.
- Copy business-relevant values into workflow variables before submit when persistence or workflow routing is required.
- Single-record temp mappings use `__temp_<id>` targets while declarations use unprefixed ids under `variables.tempVars[]`.
- A multiple-record count-only query omits record result mappings/targets completely and retains only the count target.
- Never place Form Action Query Data on a Public Form.
- On Custom Data List Forms, bind page-load actions through `formAction.onLoad` and field-change actions through the source control's `control_event_rule`.
- Encode current-record field targets as `____customListFields_<FieldName>` and current-record Sub list output with `querydata_listname_parent = "__list_"`.
- Do not use Approval `__variables_` result targets on Custom Data List Forms.
- Prefer Collection/Data Table for read-only reverse-related display. Query into a current-record Sub list is for editable working-copy/line-item scenarios and normally belongs on New/Edit forms.

## Query Result Expressions

Phase 2 export-backed:

- `arraySum(__temp_var_CollectionofQueryItems, "Amount", [], [])`
- `JSONStringfy(__temp_var_CollectionofQueryItems)`

The export-backed collection display function is spelled `JSONStringfy`. Do not silently rename it to `JSONStringify` until another export proves that spelling as a runtime function token.

The export labels a button/action as `Calculate with vLookup function`, but no `vLookup` function token appears in the export. Treat `vLookup` as UI-observed only.

The aggregate function is `arraySum`, not `arraySub`.

## Submit Form Step

Use the detailed rules in `docs/collection-04.md#yeeflow-form-action-submit-step-rules`.

Default submit:

```json
{ "type": "submit" }
```

Save changes:

```json
{
  "type": "submit",
  "name": "Submit with draft",
  "attrs": {
    "submitType": "3",
    "closeForm": true,
    "ignoreValid": true
  }
}
```

Rules:

- Use default submit for normal form submit / task approve / task complete.
- Use Save changes for draft-like behavior, not workflow progression.
- Do not generate Submit form steps on dashboards.
- Keep Reject, Task forward, and Add task assignee submit modes deferred until export-backed.

## Button Style Defaults

Observed `action_button.attrs["button-style"]` values:

- `"2"`: primary
- `"3"`: soft secondary
- `"4"`: outline primary
- `"5"`: neutral outline
- `"6"`: dashed/utility, including add/import/next patterns

Use icons with:

- `attrs["icon-type"] = "3"`
- `attrs.icon = "fa-regular fa-plus"` for add
- `attrs.icon = "fa-regular fa-arrow-right"` for next
- `attrs["icon-posi"] = "1"` for before text
- `attrs["icon-posi"] = "2"` for after text

## Validation Policy

Use warnings first:

- unknown action reference
- missing page-load action target
- action with empty steps
- unknown step type
- missing Set variable target
- missing Confirm dialog message
- missing temp variable declaration
- action button without meaningful `nv_label`
- Query data step missing source list
- Query data step missing or invalid query type
- Query data field mapping references missing variables or list row fields
- Query result target variable missing
- Submit form step used in dashboard context
- Save changes step generated without a draft/save purpose
- accidental `arraySub` expression usage; use `arraySum`
- recursive `otheraction` step that calls the same form action containing it

Treat recursive `otheraction` self-calls as errors in generated packages. Other validation items stay warning-first unless a runtime-proven rule makes them unsafe.

## Runtime Baseline Status

`Form Actions Phase 1 Test v1 Runtime` proved the Phase 1 form-action surface after the workflow assignee was manually corrected in the designer:

- button style gallery renders
- button click `control_action` triggers fire
- page-load `formAction.onLoad` runs
- temp variables display and update
- `setvar` updates temp variables and workflow text variables
- `confirm` opens a dialog and stores the result in a temp variable
- submit, reviewer task open, and approval complete

The same runtime found an assignment-generation rule: do not hardcode tenant-specific direct-user assignees such as `method: "users"` with a local user ID and `title: "User:Renger"`. Use a requester/current-user expression assignment unless an export-backed valid user mapping is intentionally supplied for the target tenant.

ContentList persistence remained pending in the first runtime app; the regenerated package switches to the proven workflow-variable button mapping pattern for the next persistence test.

`Form Actions Phase 2 Query Submit Test v1` proved the generated Phase 2 package runtime path. The final corrected retest used `attrs.querydata_filters` plural for Query data filters:

- Query data multiple button action populated a form sub list and count value.
- Query data single button action mapped selected fields into workflow/display variables.
- Query data `Active == true` filters excluded inactive source rows.
- Loaded Count was `2` from two active rows out of three source rows.
- `arraySum` over the filtered temp query collection returned `2000`.
- `JSONStringfy` displayed the filtered temp query collection.
- Save changes returned success without advancing workflow.
- Custom Submit form action submitted the workflow.
- Reviewer task opened, approval completed, and ContentList created a readable target-list record.

Filter correction:

- Query data `Active == true` filter was ignored by runtime in the generated package because the generator wrote `attrs.querydata_filter`, which did not populate the Query data step's actual `Data filter -> Condition` setting.
- User follow-up confirmed that manually setting the Query data step `Data filter -> Condition` to `Active Equals ON` filters the result correctly.
- The patched export and corrected generated retest prove the working JSON path is `attrs.querydata_filters` plural. For an Active Bit field, the exported working condition is `{ "left": "Bit1", "op": "0", "right": "true", "pre": "and", "showCus": true }`.
- `Implant Application Request (5).ywf` proved that Query data filter variable operands must be in expression-editor mode: `right` is an expression-token array and `showCus` is `false`. A frontend `<input type="button" ... Workflow Variables:...>` string in `right` with direct-value mode is compared literally and will not match records.
- Do not generate `attrs.querydata_filter` singular as the final filter implementation.
- `vLookup` remains deferred because only labels, not function tokens, have been observed.

## Employee Family Implant Requester and Submit-Time Quota Pattern

`Employee & Family Implant Application Management_Test.yap` confirmed these form-action rules for generated business apps:

- If an applicant user control is required and already has Default value = Current User (`attrs.default = "currentUser"` and `value = "CurrentUser"`), do not add a redundant Set variable action to write that applicant variable from Current User.
- Applicant snapshot actions should read profile fields from the fixed applicant/requester variable, not from `Context:Current User`, after the applicant field has been initialized.
- Page-load initialization may end with an `otheraction` call to a reusable check action, such as Family Quota Check.
- Submit-time business validation should be wired through `formdef.formAction.onSubmit` when the app must check a policy before submission.
- A submit validation action should call the reusable check action first, then use conditional warning/confirm behavior and a conditional native `submit` step.
- Do not rely only on a manual Check button for core policy checks such as quota validation.
- If quota is occupied on submission, quota checks must include in-progress/occupied usage records as well as approved/confirmed usage records. Use separate safe Query data steps per status if OR/in-list filters are not proven, then aggregate each collection with `arraySum`.

Form Action Query Data must use the shared builder and pagination contract on every supported host. Approval Form, Custom Data List/Document Library Form, and Dashboard default Page Size to 100, cap it at 1000, and default Page Number to 1; non-default Page Number serializes as `querydata_pageindex`. Dashboard outputs are temp-only; chained Dashboard queries preserve step order and guard temp-dependent filters. Temp JSON is not a Collection/Data Table source. Run the host-specific Query Data generated-final gate before signing.
# Form Action Set Data List

Form Action `Set Data List` uses the shared contract in `docs/standards/collection-02.md#form-action-set-data-list-golden-reference-standard` and `scripts/lib/form-action-set-data-list-utils.cjs`. It is available on Approval Submission/Task forms, Data List and Document Library custom forms, and Dashboards; it is forbidden on Public Forms. Print Page Set Data List remains focused-export-proof-required.

Use selected-resource `add`, `edit`, or `remove` for a Data List or Document Library. Selected-resource edit/remove requires a bounded filter. Data List/Document Library View Item forms may use current-record `edit`. On New/Edit forms, prefer Set Variable plus Submit Form for pending field values; use direct Set Data List only when an explicitly planned immediate side effect is required. Dashboard execute results must target temp variables.

Form Action Set Data List writes one mutation per step. It must not expand a Sub List into multiple records; that capability belongs to Workflow Set Data List. Generated steps must have concise business names, explicit operation and target mode, complete field mappings for add/edit, optional execution conditions plus `continue` for mutually exclusive chains, and declared result targets when status/item/count outputs are consumed. Run `validate-form-action-set-data-list-plan.mjs` before generation and `validate-form-action-set-data-list.mjs --strict-generated` before signing.

Approval Task Form Set Data List uses the same serialization as Submission Form. Data List and Document Library New/Edit/View forms share the custom-form contract. For selected Document Library Add, map `_Path` when a folder path is required and always map one single-file value to native Upload File `Text4`; Form Actions do not Loop or expand multi-file/array/List/Sub List sources. Print Page remains deferred to focused export training.

# Form Action Open Resource

Use `docs/standards/collection-02.md#form-action-open-resource-golden-reference-standard` and `scripts/lib/form-action-open-resource-utils.cjs` for all generated Open Item Form, Open Approval Form, and Open Dashboard steps.

- `listitem`: Add omits `op_type`; Edit/View emit it. Selected Edit/View requires `listdataid`. Current item is valid only on Data List/Document Library custom forms. Optional `layout` must belong to the selected target.
- `openform`: New submission may carry `setVars` and `queryParams`; `setVars.defKey` equals the target `ProcKey`. Submitted form requires `formid` and carries neither input block.
- `opendashboard`: target `PageID` resolves to Type 103 and may receive query parameters.
- Open modes are `slide`, `modal`, `target`, and `new`. Slide/Pop-up sizes are `0`, `1`, `2`, `3`, and custom `9`; custom requires `cusize.w`.
- Approval Submission/Task, Data List/Document Library custom forms, and Dashboard share the builder and trigger binder. Dashboard expressions are temp-only. Public Form is forbidden.

Run `validate-form-action-open-resource-plan.mjs` before generation and `validate-form-action-open-resource.mjs --strict-generated` before signing. Export and validator proof do not by themselves claim live navigation or modal rendering.

# Form Action Print Page and Barcode Scan

Generated Print Page and Barcode Scan steps use `scripts/lib/form-action-print-barcode-utils.cjs` and the standard App Plan table. The export-proven Print Page target in this training is a Dashboard only. Preserve `type=print`, selected page metadata, expression-token title, A4 size code `6`, portrait/landscape, decimal scale, and Minimum margins code `3`. A multi-record print page selects `dashboard-print-multi-record-table-v1`, repeats records with a Collection, lays out each record with `table-v2`, preserves merge metadata, and may link QR to the current Collection item with `qr-code-link.type=2`.

Barcode Scan uses `type=barcode`; proven modes are `multiple`, `select`, and `auto`; proven types are Auto (omitted `attrs.type`) and `ean-13`. Declare result/error variables before binding the action and make the result observable in form logic or a Collection filter. A Data List scan-enabled identifier remains a Text/input field whose stringified `Rules` contains `allowScan:true`. Run both print/barcode planning and generated-package validators. Camera, browser print preview, physical scan, and printed output remain runtime proof boundaries.

---

<a id="yeeflow-form-action-query-data-step-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-form-action-query-data-step-rules -->

# Yeeflow Form Action Query Data Step Rules

Use these rules when generating Yeeflow front-end form actions that read records from a list/report/library source into form variables.

## Scope

Export-backed from `Form Actions Phase 1 Test v1 Runtime.yap` and the focused v1.1/v1.2 Query Data exports:

- query data from a packaged data list
- query multiple items
- query single item
- sort results
- map multiple results to a form `list` variable
- map single result fields to workflow variables
- store total query count in a temp variable
- store selected multiple-query fields in a temp collection variable
- use `arraySum` against the temp query collection
- single-record mapping to `__temp_<id>` page temp variables
- multiple-record Sub list mapping with count output stored directly in a workflow number variable
- multiple-record count-only output
- Custom Data List View Item page-load query into temp variables
- Custom Data List New Item field-change query into current fields plus temp variables
- multiple query into the current Data List record's Sub list through `__list_`, with count saved to a temp variable

Generated-runtime-proven by `Form Actions Phase 2 Query Submit Test v1`:

- query multiple from a packaged data list
- query multiple result collection mapped into a form list/sub list variable
- query result count copied into a display/workflow variable
- Query data `Data filter -> Condition` using `attrs.querydata_filters`
- query single from a packaged data list
- query single selected-field mapping into workflow variables
- temp query collection aggregation with `arraySum`
- temp query collection display with `JSONStringfy`

Still deferred for exact source metadata or host wrapper generation:

- document library sources
- form report sources
- data report sources
- paging beyond `querydata_pagesize`
- empty-result handling
- `vLookup` function token shape

Product guidance allows Query Data Form Action steps on Approval Submission/task/print pages, custom Data List forms, and Dashboard pages. Public Forms do not support Query Data. The focused exports prove Approval Submission and Custom Data List Form wrappers; Dashboard remains focused-proof-required.

The first generated runtime test attempted an `Active == true` filter by writing `attrs.querydata_filter`. Runtime still returned the inactive `SRC-003` record because that singular helper attribute did not populate the Query data step's actual `Data filter -> Condition` setting. User follow-up and the patched export confirmed the fix: the runtime filter is stored in `attrs.querydata_filters` (plural).

## Step Type

Use:

```json
{ "type": "querydata" }
```

The step lives in:

```text
page.formdef.actions[].steps[]
```

## Source List

Source list shape:

```json
{
  "querydata_list": {
    "AppID": 41,
    "ListSetID": "2055176306075381761",
    "ListID": "2055176313546620929",
    "ListType": 1
  }
}
```

Rules:

- Use confirmed app/list metadata only.
- Prefer internal packaged source lists for generated test apps.
- Do not guess external list/report/library ids.

## Multiple Query To List Variable

Use when the UI needs to display query rows.

Required pattern:

```json
{
  "querydata_type": "multiple",
  "querydata_fieldmap": {
    "Title": "field_Title",
    "Text1": "field_RequestTitle"
  },
  "querydata_listname": "FormActions",
  "querydata_vartype": "list",
  "querydata_listname_parent": "__variables_",
  "querydata_fields": null
}
```

Rules:

- `querydata_listname` is the form-level list variable id without prefix.
- `querydata_listname_parent = "__variables_"` means the target is a workflow/form variable.
- `querydata_fieldmap` maps source `FieldName` values to target list row field ids.
- Every target field id must exist in `variables.listref[]` and the rendered list control.

## Single Query To Variables

Use when the form needs a few values from one source item.

Pattern:

```json
{
  "querydata_fieldmap": {
    "Title": "QueryTitle",
    "Text1": "QueryRequestTitle"
  },
  "querydata_listname": "",
  "querydata_vartype": "",
  "querydata_listname_parent": ""
}
```

Rules:

- The v1.1 export omits `querydata_type` for the default single-record mode. Older proven exports may include `querydata_type: "single"`; validators accept both.
- Map source fields directly to workflow variables when values must be submitted, persisted, or used by workflow.
- Use temp variables for transient display-only values.
- Add filters in future generated apps when business logic requires a specific source row.

## Single Query To Temp Variables

The v1.1 export maps each source field directly to a prefixed temp target:

```json
{
  "querydata_fieldmap": {
    "Title": "__temp_var_Name",
    "Datetime1": "__temp_var_Date"
  },
  "querydata_fields": null
}
```

Declare `var_Name` and `var_Date` under `variables.tempVars[]`. The `__temp_` prefix appears in the field map/expression reference, not in the declaration id. Temp values last only while the page is open.

## Multiple Query To Temp Collection

Use when the query result collection is needed for a client-side aggregate or display.

Pattern:

```json
{
  "querydata_type": "multiple",
  "querydata_fieldmap": null,
  "querydata_listname": "var_CollectionofQueryItems",
  "querydata_vartype": "text",
  "querydata_listname_parent": "__temp_",
  "querydata_fields": [
    { "FieldName": "Title", "Type": "input", "DisplayName": "Title" },
    { "FieldName": "Decimal1", "Type": "input_number", "DisplayName": "Amount" }
  ]
}
```

Rules:

- Use explicit `querydata_fields[]`.
- Reference the temp collection in expressions as `__temp_<querydata_listname>`.
- Copy any final persisted or workflow-relevant value into a workflow variable.

## Total Count

Observed count target:

```json
{
  "querydata_totalcount": "var_TotalQueryItems",
  "querydata_totalparent": "__temp_"
}
```

Rules:

- Use temp variables for display-only query counts.
- Use workflow variables when the count must be submitted, persisted, or used by workflow routing.
- Workflow count targets must be number variables.

### Count-Only Multiple Query

When only the number of matching records is needed, use `querydata_type: "multiple"`, set `querydata_totalcount`/`querydata_totalparent`, and omit all record-result settings (`querydata_fieldmap`, `querydata_fields`, `querydata_listname`, `querydata_vartype`, and `querydata_listname_parent`).

The source v1.1 export retained hidden Sub list attrs on its count-only UI step. Treat that as designer residue. Generated count-only steps must use the clean normalized template in `docs/reference/form-action-query-data-multiple-count-only.template.json`.

## Sorts And Page Size

Observed sort:

```json
{
  "querydata_sorts": [
    { "SortName": "Created", "SortByDesc": true }
  ],
  "querydata_pagesize": 200
}
```

Rules:

- Use explicit sort for deterministic single-item query behavior.
- Keep page sizes modest for generated runtime tests.

## Data Filter Condition

Correct UI location:

```text
Form actions -> Query data step -> Data filter -> Condition
```

Runtime-proven behavior:

- `Active Equals ON` filters the Query data step result correctly when configured through the `Data filter -> Condition` editor.
- The patched export stores the condition in `attrs.querydata_filters`.
- `Implant Application Request (5).ywf` proved a second Query data filter mode issue: workflow-variable operands must be stored as expression-editor token arrays with `showCus: false`. If the right operand is serialized as a frontend `<input type="button" ...>` HTML string while the row remains in direct-value mode, Yeeflow treats the HTML as a literal string and the condition does not match records.

Export-backed shape:

```json
{
  "querydata_filters": [
    {
      "key": "d7bf4cd0-0b69-47f6-9fbd-fc6cee84c78e",
      "pre": "and",
      "left": "Bit1",
      "op": "0",
      "right": "true",
      "showCus": true
    }
  ]
}
```

Workflow-variable operand shape:

```json
{
  "querydata_filters": [
    {
      "key": "8e0ee823-d395-45db-838e-5d9c9e2f4015",
      "pre": "and",
      "left": "Text2",
      "op": "0",
      "right": [
        {
          "exprType": "variable",
          "valueType": "text",
          "id": "ApplicantEmployeeID",
          "type": "expr",
          "name": "Workflow Variables:Applicant Employee ID"
        }
      ],
      "showCus": false
    }
  ]
}
```

Field meanings:

- `left`: source list `FieldName`.
- `op`: comparison operator; `0` is the exported Equals operator.
- `right`: comparison value. For direct literal values, use a primitive value such as `"true"` or `"Active"` with `showCus: true`. For workflow variables or calculations, use an expression-token array with `showCus: false`.
- `pre`: condition joiner. Use `and` for the first simple filter.
- `key`: generated UUID-like condition key.
- `showCus`: direct-value mode when `true`; expression-editor mode when `false`.

Known generator mistake:

- `attrs.querydata_filter` singular was generated in the first Phase 2 package but did not populate the designer's Data filter condition and was ignored by runtime.
- Generated HTML button strings such as `<input type="button" data="...Workflow Variables:Applicant Employee ID...">` were placed in `right` with `showCus: true`. The designer displayed a variable-looking button, but runtime compared the list field to that literal HTML string. This caused Employee Family Implant second-request quota checks to miss existing approved usage rows until the filter rows were switched to expression-editor mode.

Generation rule:

- Use `attrs.querydata_filters` plural for Query data Data filter conditions.
- Do not use `attrs.querydata_filter` singular as the final filter implementation.
- For an Active/Bit field, generate `right: "true"` for ON.
- For variable/calculated operands, generate `right` as a Yeeflow expression-token array and set `showCus: false`.
- Do not emit frontend expression-button HTML inside `attrs.querydata_filters[].right`.
- For quota/usage queries, compare stable fields with compatible operand modes. Example: `Applicant Employee ID == Workflow Variables:Applicant Employee ID` and `Quota Cycle Key == Workflow Variables:Quota Cycle Key` both use expression-token `right` values with `showCus: false`.

## Validation Recommendations

Warn when:

- `querydata_list` is missing.
- `querydata_filters[].right` is a string containing `<input`, `&quot;`, or `Workflow Variables:` because this is almost certainly a direct-value-mode HTML operand instead of an expression-editor operand.
- `querydata_filters[].right` is an expression-token array but `showCus` is not `false`.
- `querydata_filters[].right` is a primitive literal but `showCus` is not `true`, unless a future export proves another mode.
- `querydata_type` is not `single` or `multiple`.
- `querydata_fieldmap` maps to missing variables/list row fields.
- `querydata_listname_parent` points to an unknown namespace.
- multiple query collection target is missing.
- `querydata_fields` is empty when an aggregate expression depends on selected fields.
- result count target variable is missing.

Use errors only for structurally invalid JSON.

## Runtime Baseline Status

`Form Actions Phase 2 Query Submit Test v1` plus the patched export and corrected generator retest proved the generated query data surface and filter property:

- Source list and target list opened without `datas/query` 400.
- Source list contained three sample rows: two active rows and one inactive row.
- `Load Multiple Test Requests` with `attrs.querydata_filters` populated a form sub list with only the two active rows and set Loaded Count to `2`.
- `Load Single Test Request` mapped an active returned item into display/workflow variables.
- `arraySum(__temp_var_CollectionofQueryItems, "Amount", [], [])` returned `2000` from the active rows only.
- `JSONStringfy(__temp_var_CollectionofQueryItems)` displayed the filtered collection JSON.
- Submit, reviewer task open, approval completion, and ContentList persistence passed with filtered values.

Filter follow-up:

- The generated `attrs.querydata_filter` singular path was ignored by runtime.
- The patched export and corrected generated retest confirm the correct path is `attrs.querydata_filters` plural.
- `vLookup` remains UI-observed only and is not generation-safe.

## Shared Pagination And Dashboard v1.3

Page Size and Page Number use one contract across Approval Form, Custom Data List/Document Library Form, and Dashboard Query Data. Omitted Page Size means `100`; explicit `querydata_pagesize` must be `1..1000`. Omitted Page Number means `1`; v1.4 proves non-default Page Number uses `querydata_pageindex`.

The `My latest event` v1.3 Dashboard proves page-load Query Data with temp-only outputs: count current-user records, load the newest matching record into temp variables, then use a temp related-record ID in a later single query. Keep chained step order and guard the dependent step with `isNullOrEmpty(temp) == false`. Dashboard field/count/result targets must all resolve to declared temp variables. `op: "11"` with `right: null` is the export-proven current-user membership filter and is not an incomplete condition.

The v1.4 Dashboard proves multiple rows can be selected into a temp JSON payload through `querydata_fields[]`, `querydata_vartype: "text"`, and `querydata_listname_parent: "__temp_"`. Use exact function `JSONStringfy` for text. Collection/Data Table cannot consume this temp payload directly; use normal data-source binding or an explicitly justified Custom Code renderer. v1.4 also proves native Type 16 Document Library custom forms share the Data List Form Action wrapper, and Approval Task Form uses the Submission-style `formAction.onLoad + actions[]` wrapper.

---

<a id="yeeflow-form-action-submit-step-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-form-action-submit-step-rules -->

# Yeeflow Form Action Submit Step Rules

Use these rules when generating Yeeflow form action steps that submit, save, validate, reject, forward, or otherwise operate on the active form/task.

## Scope

Export-backed from `Form Actions Phase 1 Test v1 Runtime.yap`:

- minimal Submit form step
- Save changes submit-step mode
- designer-visible submit type values

Configured and studied in this export:

- Form Submit / Task Approve / Task Complete
- Save changes

Generated-runtime-proven by `Form Actions Phase 2 Query Submit Test v1`:

- default `submit` step triggered from an `action_button` submits the approval workflow
- reviewer task opens for a requester/current-user expression assignment
- approval completes
- workflow ContentList persistence creates a target-list record
- Save changes mode returns success and does not advance the workflow

Designer-visible but not configured/proven in this export:

- Execute form validation
- Reject
- Task forward
- Add task assignee

## Step Type

Submit form actions use:

```json
{ "type": "submit" }
```

The step lives in:

```text
page.formdef.actions[].steps[]
```

## Default Submit

Minimal observed shape:

```json
{
  "type": "submit"
}
```

Use this for normal form submit / task approve / task complete when no special options are needed.

Rules:

- Keep the native Action Panel available unless the user explicitly asks for custom submit buttons.
- Use a custom action button only when the app needs a specific form-action-driven submit experience.
- Use `formdef.formAction.onSubmit` for submit-time business validation that must run before workflow submission, such as quota checks.
- When submit-time validation calls another action first, use an `otheraction` step that targets the reusable check action, then conditionally run the native `submit` step.
- Conditional warning/confirm/check steps before the native `submit` step normally require step-level `continue: true`, which is the export-backed shape for designer checkbox `Continue next step when condition is not met`. Without it, the valid path can skip the warning but stop before submit.
- If the submit check depends on editable driver fields such as Requester / Applicant, product rows, quota cycle, or application type, make sure those same checks also run on page load and relevant field changes when users can alter the values before submit.
- For guard/block designs, put the invalid-case condition on the warning step and the valid-case condition on the following submit step. If override is allowed, store the confirm result in a temp variable and reference that result in the submit condition.
- Never generate an `otheraction` step that calls its own parent action; that is a recursive action loop, not a submit implementation.
- Do not confuse form action `submit` with workflow graph actions.

## Save Changes

Observed Save changes shape:

```json
{
  "type": "submit",
  "name": "Submit with draft",
  "attrs": {
    "submitType": "3",
    "closeForm": true,
    "ignoreValid": true
  }
}
```

Rules:

- Treat Save changes as a submit mode.
- Use it for draft-like or intermediate save behavior.
- Do not use Save changes for workflow progression.
- Generate `ignoreValid: true` only when draft behavior is intentional.
- Document whether the form closes after save.

## Context Support

Based on designer behavior and current docs:

- approval forms: supported
- data-list forms: supported
- public forms: supported
- dashboards: Submit form step is not supported

Dashboard form actions may still use non-submit step types in future phases if dashboard exports prove the wrapper shape.

## Task Context

Task-specific submit options remain deferred until export-backed:

- Reject
- Task forward
- Add task assignee

Do not generate these from labels alone. They likely require task-context settings, comments, target users, or routing properties.

## Validation Recommendations

Warn when:

- submit step has malformed `attrs`.
- `submitType` is not one of the observed/designer-visible values.
- Save changes uses `ignoreValid: true` without a draft/save purpose.
- Submit form step appears in a dashboard context.
- action button triggering submit has missing or generic `nv_label`.

Use errors only for structurally invalid JSON.

## Runtime Baseline Status

`Form Actions Phase 2 Query Submit Test v1` proved both submit modes in a generated package:

- `Submit form for testing` used a default `type: "submit"` form action step and created workflow instance `20552123947196252162026051500001`.
- The reviewer task opened for `Renger from Yeeflow`, was approved, and the workflow completed.
- The ContentList step created a target-list row with readable values including Request Title, Loaded Count, Selected Query Title, Selected Query Status, Approval Status, and Created From Workflow.
- `Save as draft` used `attrs.submitType = "3"` and returned a success toast without progressing the workflow.

Keep `Save changes` for draft/intermediate save behavior only. Use default submit when the button is intended to move the workflow forward.

---

<a id="yeeflow-form-design-quality-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-form-design-quality-rules -->

# Yeeflow Form Design Quality Rules

Use these rules for generated Yeeflow approval forms, custom list forms, and dashboard pages when a richer native UI is needed.

These rules are based on the manually improved `IT Hardware CAPEX Request Runtime V2.yap` export and should be applied as warning-first generator standards until more app types prove the same patterns.

## Page Background

- Put full-page approval form backgrounds on `page.formdef.attrs.background`.
- Put full-page data-list custom form backgrounds on the custom form `attrs.background`.
- Put full-page dashboard backgrounds on embedded dashboard page `attrs.background`.
- Keep `Main` as a structural wrapper.
- Do not set full-page background on `Main.attrs.common.background`.
- `Content` may carry padding and a neutral surface, but should not be the only source of the full-page background.

## Standard Shell

Approval form pages should use:

- `attrs.container.cw = "2"`
- zero page padding with `--sp--s0`
- `Main`
- `Content`
- `Form body`
- `Form bottom`

Submission forms that include a request summary should also use:

- `Form header`
- `Request summary panel`
- `Request metric row`

## Form Header

Use a dedicated `Form header` container for the top visual area.

Recommended properties:

- `attrs.style.overflow = [null, "hidden"]`
- `attrs.common.background.normal`
- `attrs.common.border.normal.type = "1"`
- `attrs.common.border.normal.width = [null, { top: 1, right: 1, bottom: 1, left: 1 }]`
- `attrs.common.border.normal.radius = [null, { top: 12, right: 12, bottom: 12, left: 12 }]`

## Request Summary Panel

Use native background first. When a gradient is required, use:

```css
selector
{
    background-image: linear-gradient(to right, <start> 0%, <end> 100%) !important;
}
```

Store it at:

- `attrs.common.css`

Set inherited text color on the container when useful:

- `attrs.common.ty.normal.color`

## Metric Row

Use:

- row container: `Request metric row`
- child containers: `Status metric`, `Cost metric`, `Owner metric`, `Next step metric`
- row direction: `attrs.style.direction = [null, "row"]`
- row gap: `--sp--s200`
- metric card padding: `--sp--s300`
- metric card background: `var(--c--neutral-light)` or `var(--c--background)`
- metric border: `var(--c--neutral-light-active)`
- metric radius: `8`

Metric labels and values should be `heading` controls with inline width.

## Text Controls

Generated `heading` and `text-editor` controls should default to:

- meaningful `nv_label`
- `attrs.common.positioning.widthtype = [null, "2"]`
- zero padding for helper/description text
- tokenized typography when the visual hierarchy matters

Use the focused `Text Style Sample.ywf` export as the source of truth for generated Text controls. The native Text control is `type: "heading"` with `label: "Text"`.

```json
{
  "headc": { "title": { "value": "Workflow route", "variable": null } },
  "heads": {
    "ty": [null, "h5-medium"],
    "color": "var(--c--text)"
  },
  "common": { "positioning": { "widthtype": [null, "2"] } }
}
```

Observed safe heading tokens:
- `attrs.heads.ty = [null, "h5-medium"]` for section headings, card titles, and metric values
- `attrs.heads.ty = [null, "xs-medium"]` for labels and eyebrow text
- custom typography objects under `attrs.heads.ty` only when explicitly export-backed

Text Style Sample correction:
- `attrs.heads.ty` should be a config pair such as `[null, "h5-medium"]` for named presets, or an explicit typography object.
- `attrs.heads.color` should be a plain string such as `"var(--c--text)"`, not `[null, "var(--c--text)"]`.
- The prior CAPEX attempts rendered visually but left Typography/Text shadow editors unresponsive because they mixed unsupported generated style shapes.

Use helper text pattern:

```json
{
  "ty": { "size": [null, 14] },
  "color": "var(--c--neutral-dark-hover)"
}
```

## Icon Badges

Section icons should be wrapped in square containers.

Wrapper rules:

- width type fixed: `attrs.style.widthtype = [null, "3"]`
- width and height use the same pixel value
- center children horizontally and vertically
- use token background such as `var(--c--primary-light)`
- use rounded corners

Icon rules:

- set icon width inline: `attrs.common.positioning.widthtype = [null, "2"]`
- set icon size to about half the wrapper width
- use token foreground color such as `var(--c--primary)`

## Field Grids

Default field sections should use `flex_grid`.

Recommended grid:

- desktop/tablet: two `1fr` columns
- mobile: one `1fr` column
- row size: auto
- column gap: `10px` or a token-equivalent if supported
- display caption: off, represented as `displayLabel = [null, false]`

Generated layout-only `Grid`/`flex_grid` controls must keep captions off by default. Only generate a visible grid caption when the user explicitly asks for a titled grid or a real export requires that title. Route summaries, KPI/status chips, and horizontal information blocks should use container/card blocks with spacing/gap rather than captioned grids.

Long controls should be full row:

- textarea
- richtext
- list/sublist
- large upload areas
- long helper text

Until column-span behavior is export-proven, place long controls outside the two-column grid below the grid.

Submission approval forms should focus on submitter inputs and business context. Do not show internal approval routing, budget owner, finance approver, decision notes, or reviewer-only routing details on the submit page unless the user requests them. Task pages may show reviewer decision sections and routing context.

## Runtime-Sensitive Controls

The following controls are schema-supported but runtime-sensitive:

- `location-picker`
- `organization-picker` / department picker
- `cost-center-picker`
- `metadata`
- `mutiple-metadata`
- `icon-upload`
- file/image controls when missing export-backed attrs

V2 showed repaired pickers with minimal attrs:

- `location-picker`: empty `attrs`
- `cost-center-picker`: empty `attrs`
- `icon-upload`: `attrs.controlmultiple = true`
- `file-upload`: `attrs.ver = 1`

Generation rule:

- Use fallback controls unless the user explicitly wants native runtime-sensitive controls.
- If native controls are used, follow the studied export shape and warn that environment metadata may still be needed.

## Calculated Fields

Detect calculated intent from names and descriptions:

- Subtotal
- Total
- Amount
- Balance
- Difference
- Duration
- Cost derived from quantity and price

Do not generate editable controls for these fields unless the requirement clearly says users enter the value.

Export-proven pattern:

- control type: `calculated`
- formula array at `attrs.calculated`
- variables referenced as `{ exprType: "variable", valueType: "number", id, type: "expr", name }`
- operator object such as `{ type: "op", op: "*" }`

CAPEX baseline:

- `Subtotal = Quantity * Unit Price`
- total estimated cost should aggregate subtotals when line items are introduced

## Validator Policy

Use warnings first for design-quality findings:

- missing page-level background
- background on `Main`
- missing `Form header`
- `Form header` without overflow/background/radius
- text/icon controls without inline width
- field sections without a `flex_grid`
- grids not using two desktop columns

## Action Buttons

The Form Actions Phase 1 export proves native `action_button` controls as a reusable form UI pattern.

Default generated action buttons should use:

- `type: "action_button"`
- `attrs.common.positioning.widthtype = [null, "2"]`
- meaningful visible label
- meaningful `nv_label`
- `attrs["button-style"]` chosen by purpose

Observed style codes:

- `"2"`: primary action
- `"3"`: soft secondary action
- `"4"`: outline primary/save action
- `"5"`: neutral outline/verify action
- `"6"`: dashed utility action such as import, add, or next

For icon buttons, use export-backed Font Awesome values such as `fa-regular fa-plus` and `fa-regular fa-arrow-right`, with `attrs["icon-type"] = "3"` and `attrs["icon-posi"]` set to `1` for before-text or `2` for after-text.
- runtime-sensitive controls using unproven attrs
- calculated-looking fields generated as editable inputs

Escalate to errors only after focused runtime testing proves a shape breaks import or runtime behavior.

---

<a id="yeeflow-root-style-token-reference"></a>

<!-- source: docs/collection-04.md#yeeflow-root-style-token-reference -->

# Yeeflow Root Style Token Reference

This document summarizes reusable design-token guidance from `<downloads>/root_styles.txt`.

The source file is a reference only. Do not inject the full CSS into generated apps, do not modify the original file, and preserve token names exactly when documenting or using them.

## Source Summary

Source stylesheet:

- product header: `YeeOffice`
- version: `v3.24.2`
- date in source comment: `Thu May 07 2026 18:40:50 GMT+0800`
- root token block: `:root`
- token groups found:
  - `66` color tokens
  - `10` font-size tokens
  - `10` line-height tokens
  - `6` font-weight tokens
  - `15` spacing tokens
  - `1` icon-width token

## Color Tokens

Primary:

| Token | Value |
| --- | --- |
| `--c--primary` | `#0065FF` |
| `--c--primary-hover` | `#005BE6` |
| `--c--primary-active` | `#0051CC` |
| `--c--primary-light` | `#E6F0FF` |
| `--c--primary-light-hover` | `#D9E8FF` |
| `--c--primary-light-active` | `#B0CFFF` |
| `--c--primary-dark` | `#004CBF` |
| `--c--primary-dark-hover` | `#003D99` |
| `--c--primary-dark-active` | `#002D73` |
| `--c--primary-darker` | `#002359` |

Secondary:

| Token | Value |
| --- | --- |
| `--c--secondary` | `#00D1FF` |
| `--c--secondary-hover` | `#00BCE6` |
| `--c--secondary-active` | `#00A7CC` |
| `--c--secondary-light` | `#E6FAFF` |
| `--c--secondary-light-hover` | `#D9F8FF` |
| `--c--secondary-light-active` | `#B0F1FF` |
| `--c--secondary-dark` | `#009DBF` |
| `--c--secondary-dark-hover` | `#007D99` |
| `--c--secondary-dark-active` | `#005E73` |
| `--c--secondary-darker` | `#004959` |

Base:

| Token | Value | Use |
| --- | --- | --- |
| `--c--background` | `#FFFFFF` | default page/card background |
| `--c--text` | `#071638` | primary text |
| `--c--text-normal` | `#071638` | normal text |
| `--c--accent` | `#03B349` | accent/success-like status |
| `--c--extra-color-1` | `#F9C434` | warning-like status |
| `--c--extra-color-2` | `#F61515` | danger-like status |

Success:

| Token | Value |
| --- | --- |
| `--c--success` | `#15DF42` |
| `--c--success-hover` | `#13C93B` |
| `--c--success-active` | `#11B235` |
| `--c--success-light` | `#E8FCEC` |
| `--c--success-light-hover` | `#DCFAE3` |
| `--c--success-light-active` | `#B6F5C4` |
| `--c--success-dark` | `#10A732` |
| `--c--success-dark-hover` | `#0D8628` |
| `--c--success-dark-active` | `#09641E` |
| `--c--success-darker` | `#074E17` |

Warning:

| Token | Value |
| --- | --- |
| `--c--warning` | `#F9C434` |
| `--c--warning-hover` | `#E0B02F` |
| `--c--warning-active` | `#C79D2A` |
| `--c--warning-light` | `#FEF9EB` |
| `--c--warning-light-hover` | `#FEF6E1` |
| `--c--warning-light-active` | `#FDEDC0` |
| `--c--warning-dark` | `#BB9327` |
| `--c--warning-dark-hover` | `#95761F` |
| `--c--warning-dark-active` | `#705817` |
| `--c--warning-darker` | `#574512` |

Danger:

| Token | Value |
| --- | --- |
| `--c--danger` | `#F61515` |
| `--c--danger-hover` | `#DD1313` |
| `--c--danger-active` | `#C51111` |
| `--c--danger-light` | `#FEE8E8` |
| `--c--danger-light-hover` | `#FEDCDC` |
| `--c--danger-light-active` | `#FCB6B6` |
| `--c--danger-dark` | `#B91010` |
| `--c--danger-dark-hover` | `#940D0D` |
| `--c--danger-dark-active` | `#6F0909` |
| `--c--danger-darker` | `#560707` |

Neutral:

Generated app themes should use Neutral with `lightmodel: "Luminance"`. Do not use `Lightness` for Neutral in new generated packages.

| Token | Value |
| --- | --- |
| `--c--neutral` | `#B3B7C0` |
| `--c--neutral-hover` | `#A1A5AD` |
| `--c--neutral-active` | `#8F929A` |
| `--c--neutral-light` | `#F7F8F9` |
| `--c--neutral-light-hover` | `#F4F4F6` |
| `--c--neutral-light-active` | `#E7E9EB` |
| `--c--neutral-dark` | `#868990` |
| `--c--neutral-dark-hover` | `#6B6E73` |
| `--c--neutral-dark-active` | `#515256` |
| `--c--neutral-darker` | `#3F4043` |

## Typography Tokens

Font size:

| Token | Value | Suggested use |
| --- | --- | --- |
| `--fs--xs` | `10px` | helper text, metadata |
| `--fs--s` | `12px` | compact labels, table metadata |
| `--fs--base` | `14px` | body text and form labels |
| `--fs--l` | `16px` | section headings or prominent body |
| `--fs--h6` | `18px` | card titles |
| `--fs--h5` | `20px` | page subsection titles |
| `--fs--h4` | `22px` | dashboard panel titles |
| `--fs--h3` | `25px` | page titles |
| `--fs--h2` | `28px` | app overview headings |
| `--fs--h1` | `32px` | rare top-level dashboard titles |

Line height:

- `--lh--xs`
- `--lh--s`
- `--lh--base`
- `--lh--l`
- `--lh--h6`
- `--lh--h5`
- `--lh--h4`
- `--lh--h3`
- `--lh--h2`
- `--lh--h1`

All line-height tokens are `160%` in the source.

Font weight:

| Token | Value |
| --- | --- |
| `--fw--light` | `300` |
| `--fw--regular` | `400` |
| `--fw--medium` | `500` |
| `--fw--semi-bold` | `600` |
| `--fw--bold` | `700` |
| `--fw--italic` | `400` |

## Spacing Tokens

| Token | Value |
| --- | --- |
| `--sp--s0` | `0` |
| `--sp--s012` | `1` |
| `--sp--s025` | `2` |
| `--sp--s050` | `4` |
| `--sp--s075` | `6` |
| `--sp--s100` | `8` |
| `--sp--s150` | `12` |
| `--sp--s200` | `16` |
| `--sp--s250` | `20` |
| `--sp--s300` | `24` |
| `--sp--s400` | `32` |
| `--sp--s500` | `40` |
| `--sp--s600` | `48` |
| `--sp--s800` | `64` |
| `--sp--s1000` | `80` |

Recommended generated-app spacing:

- page padding in the official UI/UX shell: `--sp--s0`
- card internal padding: `--sp--s200` or `--sp--s300`
- field group gap: `--sp--s150` or `--sp--s200`
- dense inline gap: `--sp--s100`
- large dashboard section gap: `--sp--s300` or `--sp--s400`

## Border, Radius, And Shadow Patterns

Observed reusable patterns:

- neutral borders commonly use `var(--c--neutral-light-active)`
- light panel backgrounds commonly use `var(--c--neutral-light)` or `var(--c--neutral-light-hover)`
- selected controls often use `var(--c--primary)`
- common radius values are `6px`, `8px`, and `12px`
- circular icon buttons use `50%`
- common soft shadow: `0px 0px 10px 0px #C1C1C140`
- stronger popover shadow: `0 2px 20px 0 rgba(0, 0, 0, 0.1)`

Generated-app recommendation:

- use `6px` for small inputs/buttons and segmented controls
- use `8px` for standard cards/sections
- use `12px` for larger cards, upload areas, and settings-style containers
- use `var(--c--neutral-light-active)` for standard borders
- use shadows sparingly; prefer neutral borders for operational apps

## Form Control Patterns

Observed form-control defaults:

- `.ant-btn`, `.ant-select-selection`, `.ant-input`, `.ant-time-picker-input`: `border-radius: 6px`, `border-color: var(--c--neutral-light-active)`
- `.ant-input-number`: `border-radius: 6px`, `overflow: hidden`
- input group addons use `background-color: var(--c--neutral-light-hover)` and `border-color: var(--c--neutral-light-active)`
- helper descriptions use small text and `var(--c--text)`

Generator guidance:

- avoid arbitrary input border colors
- use neutral borders and background tokens
- use `--fs--base` for normal field text
- use `--fs--s` or `--fs--xs` for help text

## Button, Switch, And Radio Patterns

Buttons:

- primary action color should use `var(--c--primary)`
- hover/active states should use `var(--c--primary-hover)` and `var(--c--primary-active)` where configurable
- secondary/neutral buttons should use neutral borders and background tokens

Switch:

- off state: background `var(--c--background)`, border `var(--c--text)`, knob `var(--c--text)`
- checked state: background `var(--c--text)`, knob/text `var(--c--background)`

Radio / segmented controls:

- outer group background: `var(--c--neutral-light-hover)`
- group border: `var(--c--neutral-light-active)`
- group radius: `6px`
- selected item background: `#ffffff`
- selected item weight: `600`
- selected border: `var(--c--primary)`

Generator guidance:

- prefer native switch/radio controls and token-backed state styles
- avoid custom color palettes for one-off options
- use semantic options and let Yeeflow native controls carry interaction states

## Task Panel Styles

Task/action panel styles use semantic button classes:

- `.btn-save`
- `.btn-approve`
- `.btn-reject`
- `.btn-action-right`

Relevant semantic colors:

- approve/primary actions: `var(--c--primary)` or success tokens when explicitly approval-success
- reject/danger actions: `var(--c--danger)` and `var(--c--danger-hover)`
- secondary actions: neutral tokens

Generated approval forms should continue placing `workflowControlPanel` and `workflowHistory` in `Form bottom`; do not custom-style them unless a real export proves the style payload.

## Dynamic Style Class Patterns

The stylesheet includes dynamic classes for red/yellow/blue/black variants:

- `background-color-red`
- `color-red`
- `border-red`
- `background-color-yellow`
- `color-yellow`
- `border-yellow`
- `background-color-blue`
- `color-blue`
- `border-blue`
- `background-color-black`
- `color-black`
- `border-black`

These classes target labels, radio groups, disabled radio spans, switches, and label dynamic style wrappers.

Generator guidance:

- preserve existing dynamic style class behavior when copying real exports
- prefer semantic tokens for new generated status styles
- do not replace platform dynamic classes unless the generated package owns the complete style pattern

## Table, Grid, And Collection Patterns

Observed patterns:

- `.grid-fullwidth` uses `grid-column: 1 / end`
- Collection grids use `.coll-data-grid` with three equal columns and `30px` row/column gaps
- workflow log collection grids collapse to one column
- table active pagination uses `var(--c--primary)`
- table header/placeholder surfaces often use neutral-light tokens

Generator guidance:

- use native Collection/table controls where possible
- prefer token-backed table borders/backgrounds
- use `grid-fullwidth` behavior for full-width long fields when the form schema supports it
- avoid dense multi-column card grids unless runtime proof exists for the target dashboard pattern

## Semantic Token Mapping For Generated Apps

| Semantic role | Preferred token(s) |
| --- | --- |
| page background | `--c--neutral-light` or `--c--background` |
| card/container background | `--c--background` |
| primary action | `--c--primary` |
| primary hover | `--c--primary-hover` |
| success status | `--c--success`, `--c--success-light`, `--c--success-dark` |
| warning status | `--c--warning`, `--c--warning-light`, `--c--warning-dark` |
| danger status | `--c--danger`, `--c--danger-light`, `--c--danger-dark` |
| neutral border | `--c--neutral-light-active` |
| neutral muted background | `--c--neutral-light-hover` |
| body text | `--c--text` or `--c--text-normal` |
| muted text | `--c--neutral-dark` |

## Validator Recommendations

Add warnings, not hard failures, for generated app definitions when:

- a generated UI surface contains many arbitrary hard-coded hex colors
- a color exactly matches a known token value but is stored as a literal where token usage is supported
- status colors use non-semantic one-off values instead of primary/success/warning/danger/neutral roles
- generated controls override native switch/radio/button colors without a real export pattern

Do not require token usage when Yeeflow exports resolved hex values instead of token references. Compatibility mode should preserve real export evidence.

---

<a id="yeeflow-runtime-test-checklist-template"></a>

<!-- source: docs/collection-04.md#yeeflow-runtime-test-checklist-template -->

# Yeeflow Runtime Test Checklist Template

Use this template after local `.yap` validation passes and before claiming runtime proof for a generated Yeeflow application. Treat each item as evidence to collect, not a statement of confidence. Mark every row as `pass`, `fail`, `blocked`, or `not applicable`, and keep exact app/package names, runtime URLs, request numbers, list row IDs, screenshots, and console/network observations when useful.

## Test Run Header

| Field | Value |
| --- | --- |
| Application name |  |
| Package path |  |
| Package type | `.yap` new/cloned app / `.yapk` existing-app upgrade package |
| Runtime tenant |  |
| Import date |  |
| Imported app name |  |
| Runtime tester |  |
| Local validation summary |  |
| Known deferred items |  |

## Runtime Checklist

| # | Area | Result | Evidence / notes |
| --- | --- | --- | --- |
| 1 | App import/open |  | Import the package, open the imported app, confirm the left navigation and default/home route load without a blank shell or fatal browser console error. |
| 2 | Dashboards render |  | Open every generated dashboard page. Confirm KPI cards, charts, tables, filters, and action buttons render with expected labels, spacing, and data source bindings. Page render alone is not proof when the plan promised KPIs, queues, reports, analytics, trends, or charts. |
| 3 | Data lists open without `datas/query` 400 |  | Open every generated data list page and custom list view. Watch the network panel for `datas/query` 400 responses, endless loading states, and first-load refresh caveats. |
| 4 | Approval forms open |  | Open each approval form from the app and from any create/new-request entry point. Confirm the form is published if required, and task pages open after submission. |
| 5 | Requester/applicant logic |  | Verify Current User defaults, proxy applicant behavior, applicant change actions, profile snapshots, readonly applicant fields, and workflow/user expressions use the intended requester/applicant variable. |
| 6 | Product/sublist/summary calculations |  | Add representative sublist rows, commit/blur edited cells, confirm row subtotals, visible summary values, bound total variables, and any policy-critical preflight `arraySum` recalculations. |
| 7 | Form actions |  | Test page-load, on-change, button-click, set-variable, confirm, query-data, submit, and save-changes actions that are included. Confirm valid paths continue to submit and invalid paths block or warn as designed. |
| 8 | Query data filters and expression operands |  | Confirm Query data steps use `attrs.querydata_filters` and return the intended records. For expression operands, verify token-array operands render/evaluate correctly and do not become literal HTML or raw JSON. |
| 9 | Workflow branch coverage |  | Exercise normal, exception, empty, and unexpected routing-variable values. Confirm every gateway branch reaches a valid task/end state and fallback review paths are reachable. |
| 10 | Submit/approve/reject paths |  | Submit at least one valid request, complete each required approval task, and test rejection/cancel paths. Confirm task assignees are valid runtime users/groups and no workflow node errors occur. |
| 11 | Return/resubmission if included |  | If return/rework is in scope, return a request, edit required fields, resubmit, and confirm status, audit trail, and downstream routing are correct. |
| 12 | `ContentList` persistence |  | Confirm create/edit/remove `ContentList` workflow actions write the expected rows and fields after submit, approval, rejection, and final completion. Include readable lookup/profile summaries, not only internal IDs, when reports need readable values. |
| 13 | Usage/audit list lifecycle |  | For quota, benefit, booking, or audit lists, confirm row creation timing, in-progress inclusion, final approval confirmation, rejection release/update, and correlation keys back to the source request. |
| 14 | Dashboard KPI validation |  | Compare dashboard KPI values against source list rows or submitted test requests. Confirm KPI cards are real Summary/data-bound controls, not static Text values such as `0`, `0.00`, `N/A`, or placeholder text. Confirm filter controls change the expected widgets and totals after records are created or updated. |
| 15 | No temp variables used for backend persistence |  | Verify temp variables are only used for frontend form state. Backend persistence, workflow routing, audit rows, and dashboard/report fields must use workflow/form/list variables or persisted fields. |
| 16 | No FlowKey collision |  | Confirm generated FlowKey/form keys do not appear as lowercase substrings of reserved JSON property names such as `prefix`, `suffix`, `field`, `profile`, `workflow`, `variable`, `filter`, `collection`, `condition`, `expression`, `attributes`, `actions`, or `binding`. |
| 17 | No corrupted `prefix` / `pr<id>x` keys |  | Inspect the imported/exported form or package for corrupted binding keys such as `pr205...x`. Pay special attention to sublist summary bindings and variable binding objects. |
| 18 | Form design quality gates |  | Confirm page-level background, `Main` / `Content` / `Form body` / `Form bottom`, header/request summary, two-column field grids, full-row long controls, Action Panel, Flow History, meaningful `nv_label`, and no obvious overlap or caption clutter. |
| 19 | Attachment verification/guidance |  | Verify required upload controls render, accept expected file types when tested, and show clear guidance. If binary persistence to data-list file/image fields is not runtime-proven, document the fallback/manual review rule. |
| 20 | Existing-app package limitation |  | If the target is an existing app, confirm `.yapk` handling is read-only/server-generated. Do not claim Codex-edited `.yapk` upgrade packages are valid unless Yeeflow Version management generated/signed the package or a future proof establishes the encoding/signing model. |
| 21 | Dashboard queues/reports/charts |  | Confirm operational queues and report sections are real data-list/Collection/table controls bound to expected source lists. For chart sections, first create or confirm representative source records: the data source exists, grouping fields have non-empty values, the measure/count field is available, and at least one matching record exists. Classify results separately as chart renders with data, chart model loads but has no data, or chart model fails to load. Do not treat an empty source list as a broken chart model, and do not accept free-text "advanced reporting" as runtime proof. |

## Workflow Branch Matrix

Duplicate this table for each workflow gateway or approval branch.

| Scenario | Input values | Expected branch/task | Actual branch/task | Result | Evidence |
| --- | --- | --- | --- | --- | --- |
| Normal |  |  |  |  |  |
| Exception |  |  |  |  |  |
| Empty/missing value |  |  |  |  |  |
| Unexpected value |  |  |  |  |  |

## Persistence Matrix

Use this table for request records, usage records, audit records, dashboard source lists, and any integration-staging lists.

| Event | Expected persisted target | Required fields | Actual row / value | Result | Notes |
| --- | --- | --- | --- | --- | --- |
| Submit |  |  |  |  |  |
| Approve |  |  |  |  |  |
| Reject/cancel |  |  |  |  |  |
| Return/resubmit |  |  |  |  |  |
| Final completion |  |  |  |  |  |

## Evidence Rules

- Do not report runtime proof from local validators alone.
- Record the exact package tested, because later generated packages may differ by IDs, FlowKey, or workflow/action serialization.
- Capture at least one clean app open, one dashboard open, one data-list open, one form open, and one workflow path before marking a generated app runtime-smoke-passed.
- For production-like claims, include submit, approval, rejection, persistence, dashboard KPI refresh, and branch coverage evidence.
- If runtime access is lost or a browser session fails before completing the matrix, report the package as locally validated or partially runtime-tested only.

---

<a id="yeeflow-temp-variable-generation-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-temp-variable-generation-rules -->

# Yeeflow Temp Variable Generation Rules

Temp variables are page/form runtime state for Yeeflow form actions.

## Export-Backed Shape

Temp variables are listed under the approval form definition:

```json
{
  "variables": {
    "tempVars": [
      { "idx": "uuid", "id": "var_Status" }
    ]
  }
}
```

Expression tokens reference a temp variable by prefixing its id with `__temp_`:

```json
{
  "exprType": "variable",
  "valueType": "string",
  "id": "__temp_var_Status",
  "type": "expr",
  "name": "var_Status"
}
```

## Use Cases

Use temp variables for:

- button interaction state
- temporary status text
- confirm dialog result display
- temporary counters or toggles
- UI-only defaults before values are copied to workflow variables
- query result counts
- query result collections used for client-side display or aggregate expressions

Do not use temp variables as the only source for persisted business data. Copy the value to a workflow variable or persist with an explicit supported action when persistence is required.

## Generation Rules

- Use fresh UUIDs for `idx`.
- Use readable ids such as `var_TempStatus`, `var_DialogResult`, or `var_ClickCount`.
- Use `valueType: "string"` for text-like temp variables in action expressions, matching the Phase 1 export.
- Use `__temp_${id}` only in expression tokens; keep `variables.tempVars[].id` unprefixed.
- Display temp values with the learned Text control standard: `type: "heading"`, inline width, token typography, and plain string color.
- Query data steps may write counts with `querydata_totalcount = "<temp id>"` plus `querydata_totalparent = "__temp_"`.
- Query data steps may write transient collections with `querydata_listname = "<temp id>"` plus `querydata_listname_parent = "__temp_"`.
- Reference query-result temp collections in expression tokens as `__temp_<temp id>`.
- Copy query-derived values into workflow variables before submit when the value must be persisted or used by workflow.

## Validation Rules

Warn when:

- a form action references an unknown `__temp_` variable
- a temp variable is declared but never used
- a temp variable is used in ContentList persistence without an explicit workflow-variable copy
- a generated temp value control uses the old/broken Text control shape
- a query result temp collection is used for persistence without a workflow-variable copy

---

<a id="yeeflow-text-control-generation-standards"></a>

<!-- source: docs/collection-04.md#yeeflow-text-control-generation-standards -->

# Yeeflow Text Control Generation Standards

Use these standards for generated approval forms, data-list custom forms, dashboards, and app pages.

## Default Control

The native Text control is:

```json
{
  "type": "heading",
  "label": "Text"
}
```

Do not use ad hoc text-editor controls for normal labels/headings unless a real export proves the specific pattern.

## Default Static Text

```json
{
  "type": "heading",
  "label": "Text",
  "attrs": {
    "headc": {
      "title": {
        "value": "Section heading",
        "variable": null
      }
    },
    "common": {
      "positioning": {
        "widthtype": [null, "2"]
      }
    },
    "heads": {
      "ty": [null, "h5-medium"],
      "color": "var(--c--text)"
    }
  },
  "nv_label": "Section heading"
}
```

## Width

Default:

- `attrs.common.positioning.widthtype = [null, "2"]`

Use full width only for intentional long paragraph text, empty states, or page-level content blocks where inline text would harm layout.

## Presets

Recommended generated presets:

| Pattern | Typography | Color |
| --- | --- | --- |
| Page title | `[null, "h4-medium"]` when proven or `[null, "h5-medium"]` fallback | `var(--c--text)` |
| Section heading | `[null, "h5-medium"]` | `var(--c--text)` |
| Card title | `[null, "h5-medium"]` | `var(--c--text)` |
| Metric value | `[null, "h5-medium"]` | `var(--c--text)` |
| Metric label | `[null, "xs-medium"]` | `var(--c--neutral-dark-hover)` |
| Eyebrow text | `[null, "xs-medium"]` | inherited or token string |
| Helper text | `[null, "xs-medium"]` or body fallback | `var(--c--neutral-dark-hover)` |

When generating white text on a colored/gradient header, prefer container-level text color inheritance where the container pattern is proven. If setting text color directly, still use a plain string color value.

## Custom Typography

Custom typography uses an object under `attrs.heads.ty`:

```json
{
  "fam": "Default",
  "size": [null, 16],
  "wei": "500",
  "tf": "0",
  "sty": "0",
  "dec": "0",
  "lh": [null, 24],
  "ls": [null, 0]
}
```

Use this only when an export-backed design requires explicit font properties.

## Dynamic Text

Dynamic text remains a Text control with `attrs.headc.title.variable[]`.

Static content and dynamic content should not use different style shapes. Keep the same `common.positioning`, `heads.ty`, and `heads.color` rules.

## Migration Rule

When replacing old generated Text controls:

1. Keep the existing `id` only when patching an already-imported/exported design; use fresh UUIDs for newly generated packages.
2. Preserve `headc.title.value` or `headc.title.variable`.
3. Convert `heads.ty` to `[null, token]` for named presets.
4. Convert `heads.color` to a plain string.
5. Keep or add inline width unless the text is intentionally full row.
6. Keep meaningful `nv_label`.

## Do Not Generate

Avoid these old shapes for new packages:

```json
"heads": {
  "ty": "h5-bold",
  "color": [null, "var(--c--text)"]
}
```

```json
"heads": {
  "ty": [null, "h5-bold"],
  "color": [null, "var(--c--text)"]
}
```

Both can render visually in some cases, but the focused Text Style Sample export shows the safer editor-compatible shape uses a plain string color.

---

<a id="yeeflow-workflow-generation-rules"></a>

<!-- source: docs/collection-04.md#yeeflow-workflow-generation-rules -->

# Yeeflow Workflow Generation Rules

Use these rules when generating Yeeflow approval workflow nodes, sequence flows, branch conditions, and process routing.

## Transition Condition Operand Modes

`Implant Application Request (4).ywf` proved the latest workflow arrow condition UI stores left and right operands as independent mode-aware values.

Observed `conditioninfo[]` operand wrappers:

- `type: 0`: direct/static value mode. Use for static text, selected option values, and date-picker values.
- `type: 1`: direct field/workflow-variable selector. Observed on the left operand for `Workflow Variables:Application Type`.
- `type: 2`: expression editor mode. Use for variables, functions, calculations, and dynamic comparisons on either side.

Legacy generated conditions may store operands as frontend `<input type="button" ...>` expression-button HTML strings. Existing exports can contain this, but new generated conditions should prefer the wrapper object shape when possible.

## Pattern Selection

Use direct variable left + static/option/date right when routing depends on a single variable value.

Example: `ApplicationType == Family`

```json
{
  "pre": "and",
  "left": {
    "type": 1,
    "value": {
      "exprType": "variable",
      "valueType": "text",
      "id": "ApplicationType",
      "type": "expr"
    }
  },
  "op": "s.=",
  "right": { "type": 0, "value": "Family" },
  "group": "string"
}
```

Use direct variable left + expression right when a selected variable is compared to a dynamic value, such as an approval threshold or calculated cycle.

Use expression left + static/option/date right when the left side must be calculated before comparison, such as `dateDiff(BoardingDate, now(), "year", []) > 0`.

Use expression left + expression right when both sides are dynamic, such as `TotalApplicationAmount > RemainingQuota` or `year(RequestDate) >= year(BoardingDate)`.

## Direct Workflow Variable Conditions

For direct-value conditions where the left side is a Workflow variable selector and the right side is a fixed value, use the current Condition editor wrapper shape:

- `left.type = 1`
- `left.value.exprType = "variable"`
- `left.value.type = "expr"`
- `left.value.id` resolves to `DefResource.variables`
- `right.type = 0`
- `group` matches the Condition editor group for the selected variable type

Condition editor group mapping:

| Workflow variable type | Condition editor group | Operator prefix |
|---|---|---|
| `file`, `img`, `image`, `signature`, `signer` | `general` | null checks only |
| `text`, `list`, `richtext`, `metadata`, `mutiple-metadata`, `dict`, `lookup`, `user`, `groupselect`, `department`, `organization`, `location`, `costcenter` | `string` | `s.` |
| `number`, `decimal`, `currency`, `percent`, `rate` | `number` | `n.` |
| `boolean`, `bit`, `switch` | `boolean` | `b.` |
| `date`, `datetime`, `datepicker`, `time` | `datetime` | `dt.`, plus `isNull` / `isNotNull` |

Object-like workflow variables such as User, Department, Location, Cost Center, Lookup, Metadata, and Dictionary values compare as String because the Condition editor stores their selected IDs as string values. File, Image, and Signature variables should use General `isNull` / `isNotNull` checks in this variable-condition pattern.

Date Time workflow variables such as `EffectiveDate` support `dt.=`, `dt.!=`, `dt.>=`, `dt.<=`, `dt.>`, `dt.<`, `isNull`, and `isNotNull`. Direct date/time right operands use `right.type = 0` with a date/time string, preferably the export-observed `YYYY-MM-DD HH:mm:ss` shape when time is present; null checks use `right: null`.

Use `right.type = 0` for fixed known values. Use `right.type = 2` only when the comparison value needs the Expression editor: concatenated strings, numeric calculations, Boolean functions such as `iif`, current-user/applicant-derived values such as `getUserAttr(Instance:Applicant, DepartmentID, [])`, cross-variable comparisons, or date functions such as `dateAdd(now(), "month", 1)`. A `right.type = 2` value must be a non-empty expression-token array, not a raw string or primitive.

When a connector needs grouped logic, use the current Condition editor's two-layer condition shape. Top-level `conditioninfo[]` rows may include a group wrapper row with `left: null`, `op: "isNull"`, `right: null`, and a non-empty child `conditions[]` array. The wrapper row's `pre` joins the group to previous top-level rows; every child condition row carries its own `pre` (`and` or `or`). Use child `and` rows for ranges and all-required clauses; use child `or` rows for alternatives. Do not generate nested child groups beyond this one child layer.

Assignment task output branches are not normal workflow-variable comparisons. For `Approved`, `Rejected`, and `Completed` outgoing lines, generate an export-style task Outcome expression-button on the left side with `data.type = "task"`, `param.defid = <source task id>`, and `prop = "Outcome"`, `op = "s.="`, and an export-style right expression-button with `Task outcome:Approved`, `Task outcome:Rejected`, or `Task outcome:Completed`. Do not generate simplified `left.value.id = "Outcome"` variable tokens for task outcome branches.

See `docs/studies/collection-03.md#workflow-condition-editor-direct-value` for the export-backed reference and validation rules.

## Explicit Branch Condition Coverage

Yeeflow workflow does not provide an implicit `else` or `default` SequenceFlow. Every outgoing branch that should be executable must have explicit `conditioninfo[]` coverage.

When a source node branches on a workflow variable:

- cover every known option with equality branches, or
- add a real complement branch using `AND` not-equals conditions for every equality value already routed.

Example:

```text
TravelType == type1 -> Branch A
TravelType == type2 -> Branch B
TravelType != type1 AND TravelType != type2 -> Branch C
```

Do not generate a blank/unconditioned SequenceFlow as a default fallback. For multi-condition branches, the complement still must be explicit and may be combined with other business constraints using the two-layer Condition editor shape.

## Branch Coverage

For every multi-branch node:

- cover all meaningful task outcomes and routing values
- make routing variables required, auto-derived, or covered by explicit complement conditions
- include a conditioned fallback for empty/null/unexpected values when the variable can be blank
- avoid dead-end workflow branches
- prefer expression operands for computed routing instead of creating temporary intermediate variables unless the value must be persisted or debugged

Examples:

- HR Review standard route: Approved + `HasCustomPackageProduct == No`
- HR Review Finance/fallback route: Approved + `HasCustomPackageProduct != No`, covering Yes, empty, and unexpected values. This is a real condition, not an unconditioned default connector.
- Family quota occupation route: `ApplicationType == Family`
- Date eligibility route: `dateDiff(ApplicantBoardingDate, now(), "year", []) > 0`
- Amount threshold route: `TotalApplicationAmount >= ApprovalThreshold`

## Validation

Validators should warn when:

- generated transitions use legacy HTML-button operands where wrapper operands are available
- operand wrapper objects have unknown `type`
- `type: 2` expression operands do not contain expression-token arrays
- `type: 1` direct selector operands do not contain a selected variable/field token object
- generated multi-branch nodes cover Yes/No but not empty/unexpected values where the routing variable can be blank
- a condition row duplicates or contradicts another row on the same transition

---

<a id="yeeflow-yapk-version-package-study"></a>

<!-- source: docs/collection-04.md#yeeflow-yapk-version-package-study -->

# Yeeflow `.yapk` Version Package Study

## Source

- Input package: `/Users/Renger/Downloads/Employee Family Implant Query Filter Expression Fix 20260517-Employee Family Implant v1.0.yapk`
- Related new-application package used for comparison: `/Users/Renger/Downloads/Employee Family Implant Query Filter Expression Fix 20260517.yap`
- Study date: 2026-05-17

## Summary

Yeeflow uses different package shapes for new application import and existing application version upgrades.

`.yap` packages are new-application import packages. They use a normal JSON wrapper with a `Resource` string prefixed by `[______gizp______]`. After removing the prefix, base64-decoding, and gunzipping, the payload becomes decoded Yeeflow application `Resource` JSON with `Data`, `ReplaceIds`, and related package metadata.

`.yapk` packages are application version-management packages downloaded from Application Settings -> Version management. They also use a JSON wrapper, but the wrapper contains upgrade-specific metadata and the observed `Resource` is an opaque high-entropy base64 payload. It does not use the `.yap` gzip prefix and could not be decoded with gzip, zlib inflate, raw inflate, unzip, or brotli.

## Observed `.yapk` Top-Level Structure

The studied `.yapk` is not a zip archive. It is UTF-8 JSON with BOM and these top-level keys:

| Key | Meaning |
| --- | --- |
| `PackageId` | Version package identifier. |
| `TenantID` | Source tenant identifier. |
| `AppID` | Yeeflow application type/app id. Observed value: `41`. |
| `ListID` | Existing application/list-set identity. |
| `Title` | Application/package title. |
| `Description` | Version/package description. |
| `IconUrl` | JSON string storing icon style metadata. |
| `Resource` | Opaque base64 payload for the version package. |
| `Notes` | Release notes from version management. |
| `Author` | Version package author. |
| `Date` | Version package timestamp. |
| `Version` | Version name shown in version management. |
| `Sign` | 32-byte base64 signature-like value. |

The observed `Resource` payload begins with high-entropy bytes and has no visible JSON/gzip marker. Treat it as encoded, encrypted, signed, or otherwise Yeeflow-managed until proven otherwise.

## `.yap` vs `.yapk`

| Area | `.yap` New Application | `.yapk` Existing App Upgrade |
| --- | --- | --- |
| User action | Import/create new application | Upgrade current existing application |
| Wrapper type | JSON | JSON with BOM observed |
| Resource prefix | `[______gizp______]` | no prefix observed |
| Resource decoding | base64 + gzip -> JSON | opaque in observed package |
| Identity behavior | Generated packages use fresh local IDs and `ReplaceIds` for import remapping | Upgrade package preserves existing app/list identity fields |
| Fresh IDs | Required for generated new apps to avoid collision | Do not regenerate existing object IDs unless Yeeflow version package semantics are proven |
| App metadata | `Title`, `Description`, `IconUrl`, `IsListSet`, `Resource` | `PackageId`, `TenantID`, `AppID`, `ListID`, `Version`, `Notes`, `Author`, `Date`, `Sign`, plus app metadata |
| Safe Codex operation today | Decode, modify, rebuild, validate `.yap` using existing scripts | Inspect wrapper metadata; preserve opaque `Resource`; do not mutate app internals without export-backed encoding/signing proof |

## Generation Strategy

### New Application

When the user wants a new Yeeflow application:

- output `.yap`
- use a fresh ID family
- use a safe FlowKey/form key
- keep `Data.Forms[].ListID = 0` for app-level approval forms
- run normal app/form/list/workflow validators
- import as a new app

### Existing Application Upgrade

When the user wants to modify or upgrade an existing Yeeflow application:

- ask whether this should be a new cloned app or an upgrade of the current app
- if it is an upgrade, request a baseline `.yapk` generated by Yeeflow Version management
- preserve `PackageId`, `TenantID`, `AppID`, `ListID`, and other app identity fields unless Yeeflow-generated evidence says otherwise
- preserve stable object IDs inside the package; do not apply fresh-ID replacement as if this were a new `.yap`
- test by Application Settings -> Version management -> Upgrade application, not by Import new application

Current limitation: the observed `.yapk` app resource is opaque. Codex can inspect and validate wrapper structure, but cannot safely patch wrapper metadata or app internals inside `.yapk` until the encoding/signing model is learned. A runtime test rejected a metadata-only wrapper edit even though `Resource` and `Sign` were preserved.

## Proof Package Created

Output:

`/Users/Renger/Downloads/Employee Family Implant v1.1 upgrade test.yapk`

This is a metadata-only proof package. It preserves:

- `PackageId`
- `TenantID`
- `AppID`
- `ListID`
- `Resource`
- `Sign`

It changes only wrapper metadata:

- `Version`
- `Notes`
- `Description`
- `Date`

This proved local structure handling only. Runtime upgrade rejected the package, so metadata-only wrapper edits are not considered safe.

## Validation

Added validator:

```bash
node validate-yapk-package.js <package.yapk> [--baseline <baseline.yapk>]
```

The validator checks:

- required `.yapk` wrapper keys
- base64 `Resource`
- expected 32-byte `Sign` shape
- resource does not use the `.yap` gzip prefix
- baseline identity/resource/sign preservation when a baseline is supplied
- high-entropy opaque-resource warning

Validation results:

- Original `.yapk`: pass with `YAPK_RESOURCE_OPAQUE` warning
- Metadata-only proof `.yapk`: pass with `YAPK_RESOURCE_OPAQUE` warning

## Runtime Upgrade Test: Metadata-Only Proof

Runtime test date: 2026-05-17

Tested package:

`/Users/Renger/Downloads/Employee Family Implant v1.1 upgrade test.yapk`

Target application:

`Employee Family Implant Query Filter Expression Fix 20260517`

Target URL:

`[redacted Yeeflow Version management URL]`

Observed flow:

1. Opened Application Settings -> Version management for the existing app.
2. Clicked Upgrade application.
3. Uploaded the metadata-only `.yapk`.
4. Yeeflow accepted the file enough to parse and display wrapper metadata:
   - Package ID: `[redacted package id]`
   - Version name: `Employee Family Implant v1.1 upgrade test`
   - Release notes: metadata-only Codex proof note
5. On the next upgrade step, Yeeflow rejected the package with a visible toast indicating the uploaded file/package was incorrect and to try again.
6. The upgrade dialog was cancelled. The version table still showed only the original `Employee Family Implant v1.0` row with status `Succeed`.

Result:

- Metadata-only wrapper modification was rejected at runtime.
- Local structure validation is not sufficient for `.yapk` upgrade acceptance.
- `Resource` and `Sign` preservation alone is not enough if wrapper metadata is edited outside Yeeflow.
- App-content mutation remains fully unproven and blocked.

Updated rule:

For `.yapk`, do not edit wrapper metadata, `Resource`, or `Sign` unless the package is produced by Yeeflow Version management or a future Yeeflow-supported signing/encoding workflow is proven. Offline `.yapk` generation should preserve the exact wrapper or remain study-only.

## Multi-Version Comparison

Additional Yeeflow-generated packages from the same application lineage were compared:

- `/Users/Renger/Downloads/Employee Family Implant Query Filter Expression Fix 20260517-Employee Family Implant v1.2.yapk`
- `/Users/Renger/Downloads/Employee Family Implant Query Filter Expression Fix 20260517-Employee Family Implant v1.3.yapk`

Comparison output:

`yeeflow-yapk-version-package-comparison.json`

### Package Table

| Version | PackageId | Notes | Date UTC | Resource bytes | Resource entropy | Sign |
| --- | --- | --- | --- | ---: | ---: | --- |
| Employee Family Implant v1.0 | `[redacted package id]` | The first version of this application | 2026-05-17T08:03:37Z | 27379 | 7.9935 | `[redacted sign]` |
| Employee Family Implant v1.2 | `[redacted package id]` | Added a single line control on the approval form. | 2026-05-17T08:42:47Z | 27753 | 7.9920 | `[redacted sign]` |
| Employee Family Implant v1.3 | `[redacted package id]` | Dashboard and data list changes | 2026-05-17T08:44:48Z | 27909 | 7.9911 | `[redacted sign]` |

### Stable Fields

Stable across v1.0, v1.2, and v1.3:

- `TenantID`
- `AppID`
- `ListID`
- `Title`
- `Description`
- `IconUrl`
- `Author`

Changing across versions:

- `PackageId`
- `Version`
- `Notes`
- `Date`
- `Sign`
- `Resource`

### Resource Findings

All three `Resource` values:

- are valid base64 character strings
- decode to high-entropy bytes
- do not start with gzip, zlib, or zip magic bytes
- fail gzip, zlib inflate, raw inflate, unzip, and brotli decoding
- do not contain readable searched app strings:
  - `Employee Family Implant`
  - `Implant Application Request`
  - `Family Quota Usage`
  - `Product Selection`
  - `RequesterApplicant`

Pairwise byte comparison shows almost no stable byte positions:

| Pair | Common prefix bytes | Common suffix bytes | Same-position byte ratio |
| --- | ---: | ---: | ---: |
| v1.0 vs v1.2 | 1 | 2 | 0.0040 |
| v1.0 vs v1.3 | 0 | 0 | 0.0043 |
| v1.2 vs v1.3 | 0 | 0 | 0.0046 |

This strongly suggests that `.yapk` `Resource` is encrypted, signed, compressed with an unknown server-side process, or otherwise non-deterministically packaged by Yeeflow.

### Feasibility Classification

Classification: **C. Not locally generatable with current knowledge**.

Reasons:

- `Resource` remains opaque and high entropy across all studied packages.
- Normal decompression/inspection does not reveal app content.
- `Sign` changes every Yeeflow-generated version.
- `PackageId` changes every Yeeflow-generated version.
- A metadata-only wrapper edit was rejected at runtime.
- There is no known way to reproduce Yeeflow's package integrity/signing process locally.

Recommended safe workflow:

- For new app creation, Codex generates `.yap`.
- For existing app changes, Codex should not output an edited `.yapk` as a valid upgrade package.
- Codex can analyze `.yapk` metadata, produce a change plan, update source generators/specs, or create a `.yap` clone for testing if requested.
- Official `.yapk` upgrade packages should be generated by Yeeflow Version management until the `Resource`/`Sign` mechanism is proven.

## Signing API and Resource Generation Follow-up

Follow-up date: 2026-05-27

Source evidence:

- Product-provided utility API documentation for app-package signing, verification, and ID generation.
- User-tested signing/runtime notes from the companion workspace at `/Users/Renger/Documents/Yeeflow Custom Code`.
- Read-only local comparison of two Yeeflow-generated Version management packages:
  - source version package: `Application Settings Runtime Baseline - onheader-Version test.yapk`
  - Yeeflow-generated follow-up version package: `Application Settings Runtime Baseline - onheader-Mena Test.yapk`

Redaction note: this follow-up records only safe statistics and synthetic normalized references. It does not preserve raw `.yapk` files, raw `Resource` values, `Sign` values, tenant IDs, app/list/package IDs, private URLs, API keys, or raw API responses.

## From-Scratch Generation Hardening Follow-up

Follow-up date: 2026-05-28

The Business Travel generated-app/YAPK-from-scratch attempt added a stricter boundary: signing a generated `.yapk` is not enough. The inner generated `AppPackageInfo` must pass app-creation, package, graph, workflow publish-readiness, and placeholder checks before Brotli/base64/sign.

Reusable hardening rules are captured in `docs/studies/yapk-from-scratch-generation-hardening.md` with safe normalized references under `docs/studies/normalized/yapk-from-scratch-generation-hardening/`.

Content validators should block generated-final packages before signing when root or child list flags are missing/non-1, sequence-flow conditions reference stale variables, Set Variable targets are undeclared, assignment expressions reference undeclared variables, direct position assignments contain placeholders or non-numeric IDs, or any required placeholders remain unresolved.

### API Findings

The product-provided utility endpoints establish three relevant operations:

- `POST /utils/apppackage/setsign`: accepts the `.yapk` wrapper fields without `Sign` and returns a package sign.
- `POST /utils/apppackage/verifysign`: accepts the wrapper fields plus `Sign` and verifies the signature.
- `GET /utils/generate/ids`: generates unique IDs for package/app/list style identifiers.

User-tested result:

- `setsign` can produce a signature for an existing valid `.yapk` wrapper when the `Resource` is already valid for the signing path.
- `verifysign` confirms the generated signature.
- Yeeflow accepted a wrapper-only signed `.yapk` as a version package.
- The accepted wrapper-only signed package did not change app content because `Resource` was unchanged.

This upgrades the earlier broad "signing unknown" conclusion: wrapper signing is evidence-backed for already-valid `.yapk` resources. It does not solve app-content generation.

### Resource Encoding Experiments

The companion study tried replacing the `.yapk` `Resource` with normal `.yap`-style resource encodings and submitting the wrapper to `setsign`.

Rejected encodings:

- `base64(gzip(Resource JSON))`
- `[______gizp______] + base64(gzip(Resource JSON))`
- plain Resource JSON encoded as base64

Observed rejection class:

- `setsign` rejected invalid resource shapes with a resource-decoder error rather than signing blindly.

Implication:

`setsign` is not merely a detached wrapper signing helper. It validates or decodes the `Resource` using Yeeflow's `.yapk` resource format. Normal `.yap` gzip Resource encoding is not valid for `.yapk`.

### Wrapper-only vs Yeeflow-generated Resource Comparison

Safe local comparison of the readable Yeeflow-generated source and follow-up version packages showed:

| Metric | Source version package | Yeeflow-generated follow-up |
| --- | ---: | ---: |
| File shape | JSON with BOM | JSON with BOM |
| Wrapper keys | same observed key set | same observed key set |
| Decoded `Resource` bytes | 5719 | 8342 |
| `Resource` entropy | 7.9684 | 7.9763 |
| gzip/zlib/raw inflate/unzip/brotli decode | failed | failed |

Pairwise decoded-resource comparison:

| Pair | Common prefix bytes | Common suffix bytes | Same-position byte ratio |
| --- | ---: | ---: | ---: |
| wrapper source vs Yeeflow-generated follow-up | 0 | 0 | 0.0061 |

Stable wrapper fields across the two packages:

- `TenantID`
- `AppID`
- `ListID`
- `Title`
- `Description`
- `IconUrl`
- `Author`

Changed wrapper fields:

- `PackageId`
- `Version`
- `Notes`
- `Date`
- `Sign`
- `Resource`

This matches the earlier multi-version comparison: Yeeflow-generated app-content changes produce a different high-entropy `Resource`, while tenant/app/list identity stays stable.

### Byte-flip Rejection

The companion study reported that flipping even one byte inside a valid Yeeflow-generated decoded `Resource` caused `setsign` to reject the request with a resource-decoder error.

Implication:

- The inner payload has integrity, structure, encryption, compression, checksum, or schema requirements that must be satisfied before signing.
- Local byte-level mutation is not viable.
- A valid `Sign` cannot rescue an invalid or manually corrupted `Resource`.

### Signing Follow-up Conclusion

What is now supported:

- Generate or verify `Sign` for a wrapper whose `Resource` is already valid for the `.yapk` signing path.
- Validate `.yapk` wrapper structure and compare resources using redacted safe statistics.
- Use wrapper-only signed packages for metadata/version-package experiments when the target proof boundary is explicitly wrapper-only.

What is still unsupported:

- Generating a new `.yapk Resource` from modified app JSON.
- Reusing `.yap` gzip Resource encoding as `.yapk Resource`.
- Editing lists, fields, dashboards, forms, workflows, or app internals inside `.yapk` offline.
- Treating wrapper-sign success as proof that app-content changes are packaged.
- Claiming an externally edited app-content `.yapk` is a valid upgrade package.

Product question preserved from this follow-up:

> We can sign `.yapk` wrappers with `/utils/apppackage/setsign`, but how do we generate or rebuild the Resource after changing lists, fields, dashboards, or forms? Please provide the API or encoding method that turns modified app package JSON into a valid `.yapk Resource`.

## Product Schema Follow-up: Brotli Resource and AppPackageInfo

Follow-up date: 2026-05-27

Source schema:

- Product-provided `yapk-schema.json`
- JSON Schema draft: `https://json-schema.org/draft/2020-12/schema`
- Schema title: `AppExportPackageInfo`

The schema defines top-level `.yapk` as `AppExportPackageInfo` with these required wrapper fields:

- `PackageId`
- `TenantID`
- `AppID`
- `ListID`
- `Title`
- `Description`
- `IconUrl`
- `Resource`
- `Notes`
- `Author`
- `Date`
- `Version`
- `Sign`

Important schema rule:

`Resource` is described as a Brotli compressed string. Decompressed JSON should match `#/$defs/AppPackageInfo`.

This is a product-schema-backed path, but it must be verified against real packages before generation claims are upgraded.

### AppPackageInfo Shape

The decoded `AppPackageInfo` object requires:

- `ListSet`
- `Pages`
- `Forms`
- `FormReports`
- `FormNewReports`
- `DataReports`
- `Groups`
- `Tags`
- `Metadatas`
- `Agents`
- `Connections`
- `Knowledges`
- `Themes`
- `Components`
- `PortalInfo`
- `Childs`

Important child schemas:

- `ListPackageInfo`: requires `List`, `Fields`, `Layouts`, `RemindRules`, `PublicForms`, and `FlowMappings`.
- `ListInfo`: includes `ListID`, `Title`, `Type`, `Flags`, permissions, layouts, and item data. `Type` enum is `1`, `16`, `32`, `64`, `128`, `1024`. `Flags` includes `Show = 1`.
- `ListFieldInfo`: includes `FieldID`, `ListID`, `FieldName`, `FieldType`, `FieldIndex`, `DisplayName`, `InternalName`, and `Type`.
- `ListLayoutInfo`: includes `ListID`, `LayoutID`, `Type`, `Title`, `LayoutView`, default/item-permission flags, permissions, and `LayoutInResources`.
- `ProcessFormInfo`: includes process/form metadata, `DefResource`, `WorkflowType`, `Settings`, `Deployed`, `NoRule`, and permissions.
- `FormNoRuleInfo`: requires `Prefix`, `StartIndex`, `CustomLength`, and `AutoIncrement`.
- Permission objects cover portal, workflow, list, advanced list, and base permission entries with product enum values.
- App-level modules include Agents, Connections, Knowledges, Themes, and Components.

Schema rules to preserve:

- `FieldName` must end with digits.
- `FieldName` trailing digits must equal `FieldIndex`.
- `InternalName` must match `^[a-zA-Z0-9_]+$`.
- `NoRule.Prefix` must contain `{index}`.
- `List.Type` uses product enum values.
- `List.Flags` includes `Show = 1`.
- `Date` should be UTC `yyyy-MM-ddTHH:mm:ssZ`.
- `TenantID` and `ListID` are numeric strings.

### Brotli Verification Against Local Artifacts

Read-only artifacts checked:

| Artifact label | Exists | Readable here | Resource string length | Base64 decoded bytes | Brotli result |
| --- | --- | --- | ---: | ---: | --- |
| Application Settings Runtime Baseline - onheader-Version test | yes | yes | 7628 | 5719 | failed |
| Application Settings Runtime Baseline - signed wrapper test | yes | no, local reads hang | not inspected | not inspected | not inspected |
| Application Settings Runtime Baseline - requested changes test | yes | no, local reads hang | not inspected | not inspected | not inspected |
| Application Settings Runtime Baseline - onheader-Mena Test | yes | yes | 11124 | 8342 | failed |

Decode variants tested for the two readable artifacts:

- base64-decode `Resource`, then Brotli-decompress
- raw `Resource` UTF-8 bytes, then Brotli-decompress
- base64url-normalized decode, then Brotli-decompress

All tested variants failed to produce decompressed JSON for the readable historical artifacts. The failures were safe decode failures; no raw `Resource`, `Sign`, IDs, private URLs, or decoded payloads were recorded.

Current interpretation:

- The product schema says `.yapk Resource` should be Brotli-compressed `AppPackageInfo`.
- The currently readable historical artifacts do not verify that decode path.
- Possible explanations include schema/version drift, an additional encoding/encryption layer, older package format, or a missing product-specific pre/post-processing step.
- Do not discard the product schema; use it as the target standard for new tooling and future product clarification.

### Updated `.yap` vs `.yapk` Comparison

| Area | `.yap` New Application | `.yapk` Existing App Upgrade |
| --- | --- | --- |
| User action | Import/create new application | Upgrade current existing application |
| Wrapper type | JSON | Product schema `AppExportPackageInfo` JSON |
| Resource encoding | `[______gizp______]` + base64 gzip | Product schema says Brotli-compressed `AppPackageInfo`; current readable artifacts did not Brotli-decode |
| Decoded shape | Resource wrapper with `Data`, `ReplaceIds`, package metadata | Target decoded shape is `AppPackageInfo` with `ListSet`, `Pages`, `Forms`, reports, modules, portal info, and `Childs` |
| Safe Codex operation today | Generate, validate, build wrapper, import as new app | Inspect wrapper, validate schema rules, attempt Resource decode, compare safely, and preserve generation/runtime proof boundaries |
| Content generation status | Proven for new/cloned app workflows | Not proven until edit -> Brotli encode -> sign -> verify -> runtime upgrade succeeds |

### Signing Boundary

The signing follow-up remains relevant:

- Product `setsign` / `verifysign` utilities are evidence-backed for signing/verifying wrappers when `Resource` is already valid for the signing path.
- Wrapper-only signed packages may be accepted but do not change app content when `Resource` is unchanged.
- Normal `.yap` Resource encodings were rejected by the signing path.
- Wrapper signing is not Resource generation proof.

Product question:

> Confirm whether `.yapk Resource` is exactly Brotli-compressed UTF-8 JSON of AppPackageInfo, and whether `/utils/apppackage/setsign` is the supported way to sign locally generated Resource.

### Updated Classification

Current classification:

**C/B-partial. Product schema defines `.yapk Resource` as Brotli `AppPackageInfo`, but the Brotli decode path is not verified on the currently readable historical artifacts. `.yapk` content generation/runtime-upgrade remains unproven.**

Do not upgrade to `B. Locally decodable and schema-validatable` until at least one real `.yapk` artifact decodes from `Resource` to `AppPackageInfo` with the product schema path.

Do not upgrade beyond `B` until a focused edit -> Brotli encode -> sign -> verify -> runtime upgrade succeeds.

## Open Learning Items

Before Codex can generate real app-content `.yapk` upgrades, Yeeflow needs a further export-backed study for:

- why currently readable historical artifacts do not Brotli-decode despite the product schema Resource description
- whether `Resource` has an additional encoding, encryption, checksum, or package-version layer before/after Brotli
- what `Sign` covers and whether it can be recomputed outside Yeeflow
- whether wrapper metadata is included in server-side package integrity checks
- whether app-content modifications must be made by generating a new Yeeflow version from the UI/API instead of editing `.yapk` offline
- whether downloaded `.yapk` packages with included application data have additional payload markers
- whether `/utils/apppackage/setsign` is the supported way to sign a locally Brotli-encoded `AppPackageInfo` Resource

Until that learning is complete, do not claim a modified app-content `.yapk` is safe.
