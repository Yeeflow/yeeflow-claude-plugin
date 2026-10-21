# skills/yeeflow-dashboard-generator/references collection 1

This file combines 13 documents from `skills/yeeflow-dashboard-generator/references/` for the Claude directory build. A reference to `skills/yeeflow-dashboard-generator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [chart-widgets-pattern.md](#chart-widgets-pattern)
- [collection-control-pattern.md](#collection-control-pattern)
- [data-bound-elements-pattern.md](#data-bound-elements-pattern)
- [filter-controls-pattern.md](#filter-controls-pattern)
- [knowledge-base-pattern.md](#knowledge-base-pattern)
- [minimal-dashboard-pattern.md](#minimal-dashboard-pattern)
- [runtime-testing.md](#runtime-testing)
- [service-desk-pro-stage-c-pattern.md](#service-desk-pro-stage-c-pattern)
- [service-desk-pro-stage-d-pattern.md](#service-desk-pro-stage-d-pattern)
- [service-desk-pro-stage-e-f-pattern.md](#service-desk-pro-stage-e-f-pattern)
- [service-desk-pro-stage-i-l-pattern.md](#service-desk-pro-stage-i-l-pattern)
- [simple-elements-pattern.md](#simple-elements-pattern)
- [validator-rules.md](#validator-rules)

---

<a id="chart-widgets-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#chart-widgets-pattern -->

# Chart Widgets Pattern

Use this after the empty dashboard shell, static page-builder elements, and local data-bound summary/table elements are already proven.

Source of truth: `Generated Dashboard Data Bound v3.yap`.

Proven generated package: `generated-dashboard-chart-widgets-v4.yap`.

Runtime result: imported into `https://<yourdomain>.yeeflow.com`, opened as `Dashboard | Generated Dashboard Chart Widgets v4`, rendered summaries, a data table, one pie chart, one column chart, one line chart, and opened the local `Event Planning` list without a visible query failure.

## App Shape

- one root app/listset
- one Type `103` dashboard layout
- one local child data list
- two summary controls
- one dashboard data table
- three chart widgets
- no approval forms
- no dashboard filters
- no AI modules, connections, document libraries, or external dependencies

## Chart Control Pattern

Chart controls live in the embedded dashboard page JSON under `LayoutInResources[0].Resource`.

Page controls:

- `type: "pie-chart"` for pie charts
- `type: "bar-chart"` for column charts
- `type: "line-chart"` for line charts

The chart control id is a UUID-like page control id. Preserve it as the page control id and do not remap it into the generated numeric resource ID family.

Chart visual options found in the page control:

- `attrs.title`: title text, display, position, alignment, typography, padding, and color
- `attrs.common`: padding, border, radius, and hover shadow
- `attrs.label`: present on the studied line chart for label font settings

## Chart Runtime Binding

Each chart control has a matching `page.exts[]` entry:

- `category: "___Pivot___"`
- `key`: `pie-chart`, `bar-chart`, or `line-chart`
- `i`: the matching chart control id
- `attr.AppID`: app id
- `attr.ListID`: local source list id
- `attr.ListSetID`: root app/listset id
- `attr.chartType`: Yeeflow chart type string
- `attr.settings.rows[]`: grouping/category field definitions
- `attr.settings.values[]`: aggregate value definitions
- `attr.settings.columns[]`: optional when a future export proves it

`Resource.ReportIds` includes all data-bound dashboard control ids, including summaries and chart controls.

## Chart Types Proven

- Pie chart: `key: "pie-chart"`, `attr.chartType: "0"`
- Column chart: `key: "bar-chart"`, `attr.chartType: "2"`, page control label `Column chart`
- Line chart: `key: "line-chart"`, `attr.chartType: "1"`

The proven pie and column charts group by `Text7` / `Vendors` and aggregate `Decimal1` / `Budget` with `SUM`.

The proven line chart groups by `Datetime1` / `Date` with `func: "MONTH"` and aggregates `Decimal1` / `Budget` with `SUM`.

## ReplaceIds

Include every local package id:

- root app/listset id
- dashboard layout id
- child list layout id
- child list id
- child list field ids
- local sample row ids

Do not include UUID-like page-builder, summary, table, or chart control ids in `ReplaceIds` unless a future export proves Yeeflow treats them as local remappable resources.

Do not remap tenant/user metadata.

## Generation Rules

- Use a fresh local ID family for every import test.
- Preserve large numeric ids as strings.
- Build generated `Resource.Data` from decoded app-def JSON instead of plain-parsing compressed `Resource.Data`.
- Preserve chart control ids, `Resource.ReportIds`, page `exts[]`, and local list data-source bindings.
- Remap `exts[].attr.ListID` and `ListSetID` to the generated local list and root ids.
- Preserve `settings.rows[]`, `settings.columns[]`, and `settings.values[]` field references by `fieldName`.
- Validate chart field references against the referenced list fields before building the wrapper.
- Keep generated native Title field metadata: `Status: 0`, `IsSystem: true`, `IsIndex: true`.

## Validator Rules

Validators should catch:

- each chart `exts[].i` resolves to a page control id
- each chart `exts[].i` appears in `Resource.ReportIds` when report ids are present
- chart `exts[].attr.ListID` resolves to a packaged list
- chart `settings.rows[]`, `columns[]`, and `values[]` field references resolve to that list's fields or known system fields
- chart `key` and page control `type` are consistent for the proven types

## Stop Conditions

Stop before generation if:

- chart `exts[].i` cannot be matched to a chart control id
- chart source list is missing from `Data.Childs[]`
- chart row/value field names do not exist on the source list
- chart control ids are missing from `Resource.ReportIds`
- large numeric ids have become JavaScript numbers
- package validation, graph validation, or wrapper round-trip checks fail

---

<a id="collection-control-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#collection-control-pattern -->

# Dashboard Collection Control Pattern

Use this reference after the data-bound and Service Desk Pro Stage M/N patterns when generating or validating dashboard Collection controls.

Source studied: `/Users/Renger/Downloads/Service Desk Pro Dashboard Stage M.yap`

Dashboard studied: `Tickets with Collection`

## Proven From Export Study

- A dashboard Collection control uses `type: "collection"`.
- The Collection data source is stored at `attrs.data.list`.
- The repeated item template is the first child of the Collection.
- Dynamic fields inside Collection items use `attrs.source: "3"` and `attrs["obj-f"]`.
- Collection item expressions use `exprType: "variable_ctx"` with `ctx: "__ctx_coll"`.
- Conditional per-item style rules can be stored in `attrs.control_display`.
- Designer navigator names are stored in `nv_label`.

## Proven From Runtime Generation

- `generated-dashboard-collection-card-v1.yap` imported and rendered card/grid Collection rows from a local Support Tickets list.
- `generated-dashboard-collection-table-v3.yap` imported and rendered table-style Collection rows from the same list pattern.
- `generated-dashboard-collection-table-v2.yap` is a failed learning artifact: it imported but rendered vertically because it used the wrong `flex_grid` schema.
- `generated-dashboard-collection-search-v5.yap` imported and rendered table-style Collection rows with hidden table-grid captions and working full-text search binding.
- `generated-dashboard-collection-search-button-v6.yap` imported and proved a local `New Ticket` action button bound to the local Support Tickets list.
- `generated-dashboard-collection-grid-v7.yap` imported and proved the card/grid Collection renders as a three-column grid and can include the local `New Ticket` action button.
- `generated-dashboard-collection-grid-display-v8.yap` imported and proved an inline/grow header action layout plus a per-item dynamic show rule inside the Collection item template.

## Collection Data Source

Minimum observed data shape:

```json
{
  "attrs": {
    "data": {
      "list": {
        "AppID": 41,
        "ListID": "<local-list-id>",
        "Type": 1,
        "Title": "Support Tickets",
        "ListSetID": "<root-listset-id>"
      },
      "link": "default"
    }
  }
}
```

For generated packages, `ListID` must resolve to a local child list included in the package.

## Dynamic Field Pattern

```json
{
  "type": "dynamic-field",
  "attrs": {
    "source": "3",
    "obj-f": "Title"
  }
}
```

Rules:

- only use `source: "3"` inside a Collection item template
- `obj-f` must resolve to the Collection source list field
- use `item_style` for display styling
- `prefix` is supported by the studied export

## Text Expression Pattern

Collection item expression:

```json
{
  "exprType": "variable_ctx",
  "valueType": "datepicker",
  "id": "Datetime1",
  "ctx": "__ctx_coll",
  "type": "expr",
  "name": "Collection item:Created Time"
}
```

## Search Filter Binding Pattern

Observed shape:

```json
{
  "type": "search-filter",
  "binding": "__filter_filter_Tickets",
  "attrs": {
    "placeholder": "Search contacts"
  }
}
```

Page-level filter variable:

```json
{
  "filterVars": [
    { "idx": "<guid>", "id": "filter_Tickets" }
  ]
}
```

Collection full-text binding:

```json
{
  "attrs": {
    "data": {
      "fulltext": [
        {
          "fields": ["Title", "Text1", "Text4"],
          "value": [
            {
              "exprType": "variable",
              "valueType": "string",
              "id": "__filter_filter_Tickets",
              "type": "expr",
              "name": "filter_Tickets"
            }
          ]
        }
      ]
    }
  }
}
```

Generate this only as an isolated package until runtime search narrowing is confirmed.

Observed function wrapper:

```json
{
  "type": "func",
  "func": "dateFormat",
  "params": [
    [{ "exprType": "variable_ctx", "id": "Datetime1", "ctx": "__ctx_coll" }],
    [{ "type": "str", "value": "MMM DD, YYYY HH:MM" }]
  ]
}
```

## Conditional Style Pattern

Per-item conditional style rules are stored in `attrs.control_display`.

```json
{
  "formulas": [
    { "exprType": "variable_ctx", "id": "Text2", "ctx": "__ctx_coll", "type": "expr" },
    { "type": "op", "op": "==" },
    { "type": "str", "value": "High" }
  ],
  "actions": {
    "type": 1,
    "attrs": {
      "style_regulation_action": "style_class",
      "action_style": "{\"normal\":{\"bgcolor\":\"rgba(245, 166, 35, 0.15)\",\"color\":\"#f5a623\"}}"
    }
  }
}
```

Do not generate visibility behavior from this pattern. Conditional style and dynamic display actions use the same `attrs.control_display` array but different `style_regulation_action` values.

## Dynamic Show Rule Pattern

The updated v7 export proves a non-style dynamic show action on a dynamic field inside a Collection item template.

```json
{
  "controlId": "<target-dynamic-field-id>",
  "formulas": [
    { "exprType": "variable_ctx", "valueType": "radio", "id": "Text2", "ctx": "__ctx_coll", "type": "expr" },
    { "type": "op", "op": "!=" },
    { "type": "str", "value": "Medium" },
    { "type": "op", "op": "and" },
    {
      "type": "func",
      "func": "isNullOrEmpty",
      "params": [[
        { "exprType": "variable_ctx", "valueType": "datepicker", "id": "Datetime1", "ctx": "__ctx_coll", "type": "expr" }
      ]]
    },
    { "type": "op", "op": "==" },
    { "type": "bool", "value": false }
  ],
  "actions": {
    "type": 1,
    "attrs": {
      "style_regulation_action": "style_regulation_action_show",
      "style_regulation_action_color": null,
      "action_style": null,
      "icon_type": null
    }
  }
}
```

Runtime proof:

- v8 showed Assigned Team for non-Medium records with Created Time present.
- v8 hid Assigned Team for Medium records.
- The rule is evaluated per Collection item through `ctx: "__ctx_coll"`.

Generator rules:

- use this only inside a Collection item template
- ensure every `variable_ctx` field resolves to the Collection source list
- keep `actions.type: 1`
- keep `action_style: null`
- do not infer dynamic hide action shape from this show action

## Local New Ticket Action Button Pattern

Use this only for local lists included in the same generated `.yap`.

```json
{
  "type": "action_button",
  "label": "New Ticket",
  "attrs": {
    "icon-type": "3",
    "icon": "fa-solid fa-plus",
    "align": [null, "right", null, "justify"],
    "action-type": "5",
    "common": {
      "container": {
        "size": [null, "grow"]
      }
    },
    "data": {
      "list": {
        "AppID": 41,
        "ListSetID": "<root-listset-id>",
        "ListID": "<local-list-id>"
      }
    }
  },
  "parentCol": 1
}
```

Runtime proof:

- v6 table-style Collection opened the local Support Tickets add panel.
- v7 card/grid Collection opened the local Support Tickets add panel.
- v8 proved the inline/right/grow header action layout and opened the local Support Tickets add panel.
- Testing must cancel the panel without saving a record.

Validator expectation:

- `attrs.data.list.ListID` must resolve to a local child list.

## Layout Patterns

Card/grid Collection:

- no explicit `layout.col` was present in the studied export
- `layout.cg` and `layout.rg` were `24`
- item template used a vertical container and dynamic fields for title, priority, status, and assigned team
- v7 runtime proved this renders as a three-column card grid for six generated Support Ticket records

Inline header/action layout:

- for header containers with a title block and a button, set the title block width type to inline where the export uses `style.widthtype: [null, "2"]`
- set action button `attrs.common.container.size: [null, "grow"]` when the button must not wrap awkwardly
- set action button `attrs.align: [null, "right", null, "justify"]`
- keep the button bound only to a local included list

Table-style Collection:

- Collection layout used `col: [null, 1]`, `cg: [null, 0]`, and `rg: [null, 0]`
- header was a separate `flex_grid` outside the Collection
- item template contained a nested `flex_grid` row
- row cells used dynamic fields and one formatted date heading expression

Dashboard `flex_grid` table schema:

- use `attrs.columns`, not `attrs.layout.cols`
- include `attrs.rows`
- include `attrs.cgap` and `attrs.cgapU` when matching the studied table pattern
- include `attrs.content.pd` for cell padding
- table v2 proved that `attrs.layout.cols` is ignored by runtime and causes vertical stacking
- table header and repeated row grids must set `displayLabel: [null, false]`; otherwise runtime shows designer captions such as `Table header` and `Table row`

## Navigator Naming

Use `nv_label` for generated designer readability.

Recommended labels:

- Collection section
- Collection control
- Collection item wrapper
- Collection priority badge
- Collection table header
- Collection table row
- Collection date text

## Validator Expectations

The dashboard validators should catch:

- unresolved Collection `attrs.data.list.ListID`
- unresolved full-text fields
- unresolved full-text filter variables
- `source: "3"` dynamic fields outside Collection item templates
- unresolved `obj-f` fields
- unresolved `variable_ctx` collection item fields
- invalid conditional `action_style` JSON
- duplicate dashboard page control IDs
- invalid `flex_grid` column schema using `attrs.layout.cols`
- visible Display caption on table-style Collection header or repeated row `flex_grid` controls
- unresolved filter control binding values such as `__filter_filter_Tickets`
- local action buttons with `action-type: "5"` must resolve `attrs.data.list.ListID` to a local list
- dynamic show rules with `style_regulation_action_show` must have `actions.type: 1`, `action_style: null`, a valid target `controlId`, and formula fields that resolve against the Collection source list

## Stop Conditions

Stop before generation if:

- the Collection source list is not included in the package
- a Collection item field reference cannot be resolved
- a dynamic rule uses an unstudied action shape, especially hide behavior
- non-default card-grid column behavior requires guessing
- a table-style Collection requires a `flex_grid` schema not matching `attrs.columns` / `attrs.rows`
- a table-style Collection header or repeated row grid would render its Display caption
- the package has not been validated and runtime import-tested

## Proven Search Table Baseline

`generated-dashboard-collection-search-v5.yap` is the first proven table-style Collection with search binding and hidden grid captions.

Runtime proof:

- imported successfully into `https://<yourdomain>.yeeflow.com`
- opened as `Tickets Collection Search | Generated Dashboard Collection Search v5`
- rendered horizontal table header and rows
- did not render `Table header` or `Table row` captions
- typing `VPN` in the search filter narrowed the Collection to the matching Support Ticket row

## Proven Action and Grid Baselines

`generated-dashboard-collection-search-button-v6.yap` is the first proven table-style Collection with a local New Ticket action button.

`generated-dashboard-collection-grid-v7.yap` is the first proven card/grid Collection that combines the card layout with the local New Ticket action button.

`generated-dashboard-collection-grid-display-v8.yap` is the first proven card/grid Collection with an inline/grow New Ticket header action and a per-item dynamic show rule.

Runtime proof:

- v6 and v7 imported successfully into `https://<yourdomain>.yeeflow.com`
- both opened the local Support Tickets add panel through the New Ticket button
- v7 rendered two rows of three Collection cards
- v8 rendered two rows of three Collection cards, opened the add panel, and hid Assigned Team on Medium-priority cards while showing it on non-Medium cards
- no records were saved during testing

## Hide/Show Dynamic Display Status

Dynamic show display rules are proven. Dynamic hide display rules are not proven yet.

The Stage M export proved conditional style actions in `attrs.control_display` with `style_regulation_action: "style_class"`. The updated v7 export proved show actions in `attrs.control_display` with `style_regulation_action: "style_regulation_action_show"`. Do not generate hide rules until a focused export proves the native hide action shape.

---

<a id="data-bound-elements-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#data-bound-elements-pattern -->

# Data-Bound Dashboard Elements Pattern

Use this after the empty dashboard shell and static page-builder elements are already proven.

Source of truth: `Generated Dashboard Simple Elements v2.yap`.

Proven generated package: `generated-dashboard-data-bound-v3.yap`.

Runtime result: imported into `https://<yourdomain>.yeeflow.com`, opened as `Dashboard | Generated Dashboard Data Bound v3`, rendered two summary cards, rendered a data table, and opened the local `Event Planning` list without a visible query failure.

## App Shape

- one root app/listset
- one Type `103` dashboard layout
- one local child data list
- no approval forms
- no reports outside dashboard `ReportIds`
- no AI modules, connections, or document libraries
- app metadata arrays remain present, including empty `AppThemes` and `AppComponents`

## Dashboard Layout

Keep the proven static page shape:

- `Type: 103`
- `LayoutView: null`
- `Ext2: "{\"src\":true}"`
- `LayoutInResources[0].ID = LayoutID`
- `LayoutInResources[0].RefId = LayoutID`
- `LayoutInResources[0].Resource` is JSON string page content

Root navigation includes:

- duplicate dashboard entries targeting the dashboard `LayoutID`
- one `Type: 1` child list entry for the local data list

## Summary Control Pattern

Summary controls are split across page controls and page `exts`.

Page control:

- `type: "summary"`
- `id`: UUID-like control id
- visual settings live in `attrs.prefix`, `attrs.suffix`, `attrs.layout`, and `attrs.common`

Matching `page.exts[]` entry:

- `category: "___Pivot___"`
- `key: "summary"`
- `i`: the summary control id
- `attr.AppID`: app id
- `attr.ListID`: local source list id
- `attr.ListSetID`: root app/listset id
- `attr.settings.values[]`: aggregate fields such as `SUM` on `Decimal1` or `COUNT` on `ListDataID`
- `attr.settings.Conditions[]`: optional field-name filter conditions

`Resource.ReportIds` includes every summary `exts[].i`.

## Data Table Pattern

Dashboard data table control:

- `type: "data-list"`
- `label: "Data table"`
- `attrs.data.list.AppID`: app id
- `attrs.data.list.ListID`: local child list id
- `attrs.data.list.ListSetID`: root app/listset id
- `attrs.data.list.Type: 1`
- `attrs.data.list.Title`: local list title

The source list must be included under `Data.Childs[]`.

## ReplaceIds

Include every local package id:

- root app/listset id
- dashboard layout id
- child list layout id
- child list id
- child list field ids
- local sample row ids

Important: sample row ids can appear as `ListDatas` object keys. Generators must remap object keys as well as values.

Do not remap tenant/user metadata.

## Generation Rules

- Use a fresh local ID family for every import test.
- Preserve large numeric ids as strings.
- Build generated `Resource.Data` from decoded app-def JSON instead of plain-parsing the compressed `Resource.Data` string.
- Preserve summary `exts[]` and data-table `attrs.data.list` bindings through exact ID remapping.
- Keep generated native Title metadata: `Status: 0`, `IsSystem: true`, `IsIndex: true`.
- Normalize generated child list grid `LayoutView` with `layout`, `sort`, `query`, `rowColor`, and `filter`.
- Keep the data-bound baseline limited to summary/table binding until charts and filters are separately studied.

## Validator Rules

Validators should catch:

- dashboard control `attrs.data.list.ListID` resolves to a packaged list
- dashboard `exts[].attr.ListID` resolves to a packaged list
- dashboard `exts[].i` resolves to a page control id
- dashboard `exts[].i` appears in `Resource.ReportIds` when report ids are present
- every local list/layout/field/sample id appears in `ReplaceIds`
- generated Title field metadata is native/system/indexed

## Stop Conditions

Stop before generation if:

- summary `exts[].i` cannot be matched to a summary control id
- data table source list is missing from `Data.Childs[]`
- local sample row ids are not in `ReplaceIds`
- large numeric ids have become JavaScript numbers
- dashboard or child list query validators fail

---

<a id="filter-controls-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#filter-controls-pattern -->

# Filter Controls Pattern

Use this after the local data-bound chart widget pattern is already proven.

Source of truth: `Generated Dashboard Chart Widgets v4.yap`.

Generated validation package: `generated-dashboard-filter-controls-v5.yap`.

Runtime result boundary: `generated-dashboard-filter-controls-v5.yap` imported, opened, rendered search/radio/range filter controls, rendered summary/table/chart content, and opened the local source list with sample rows. Interactive filtering behavior, click-apply refresh, and reset behavior still require focused runtime proof.

Additional export-learning sources: `Sales_Management_AD.yap` and `CRM - Customer relationship management.yap`, studied in `docs/studies/collection-01.md#data-filter-controls`.

Export-proven dashboard Data Filter control types from `Sales_Management_AD.yap`:

- `check-filter` for Checkbox filter
- `select-filter` for Select filter
- `range-filter` for Range filter
- `check-range` for Check range
- `date-filter` for Date filter
- `relative-period` for Relative period
- `apply-button` for Apply button
- `remove-filers` for Remove filters in this sample export

`Sales_Management_AD.yap` also proves filter variables consumed by both dashboard `attrs.data.filter[]` controls and `page.exts[].attr.settings.Conditions[]` report/chart conditions. It includes unresolved stale chart references on the `Dashboard` page, so generated packages must validate every downstream filter variable reference before handoff.

Export-proven dashboard Data Filter control types from `CRM - Customer relationship management.yap`:

- `search-filter` for Search filter
- `radio-filter` for Radio filter
- `hierarchy-filter` for Hierarchy filter
- `sorting-filters` for Sorting filter in this sample export
- `apply-button` for Apply button

The CRM export proves additional consumer paths: Search filters feed dashboard `attrs.data.fulltext[]`, Hierarchy filters feed `attrs.data.filter[]`, Sorting filters feed `attrs.data.sortingfilter[]`, and report/chart extension conditions continue to use `page.exts[].attr.settings.Conditions[]`.

## App Shape

- one root app/listset
- one Type `103` dashboard layout
- one local child data list
- two summary controls
- one dashboard data table
- three chart widgets
- one filter container
- one search filter
- one radio filter
- one range filter
- no approval forms
- no AI modules, connections, document libraries, or external dependencies

## Filter Variables

The dashboard page declares filter variables in `filterVars`:

- `filter_Search`
- `filter_Radio_Vendor`
- `filter_Range_BudgetNumber`

Filter controls bind to these variables with `binding: "__filter_" + filterVarId`.

`Sales_Management_AD.yap` confirms the same bridge for additional value-producing Data Filter controls. Apply Button and Remove Filters are special controls: they do not bind like normal value-producing filters. Click-apply filters use `attrs.apply_t = "2"` and reference an Apply button through `attrs.apply_btn`.

## Filter Controls

Search filter:

- `type: "search-filter"`
- `label: "Search filter"`
- `binding: "__filter_filter_Search"`
- CRM export settings include `attrs.placeholder`, optional `attrs["minnumber-letters"]`, optional `attrs.apply_t`, and optional `attrs.apply_btn`; consumers can reference Search variables in `attrs.data.fulltext[]`

Radio filter:

- `type: "radio-filter"`
- `label: "Radio filter"`
- `binding: "__filter_filter_Radio_Vendor"`
- `attrs.data.list` points to the local source list
- `attrs.display_f: "Text7"`
- `attrs.value_f: "Text7"`
- `attrs.displayStyle: "dropdown"`
- `attrs.data.sort[0].SortName: "Text7"`
- CRM export settings also include `attrs.ps`, `attrs.layout`, `attrs.search-enable`, `attrs.more-enable`, `attrs.more-text`, `attrs.less-text`, and `attrs.search-placeholder`; one Radio filter uses click-apply wiring

Hierarchy filter:

- `type: "hierarchy-filter"`
- `binding: "__filter_<filterVarId>"`
- list-backed hierarchy settings include `attrs.data.list`, `attrs.display_f`, `attrs.value_f`, `attrs.parent_f`, and `attrs.child_f`
- optional settings include `attrs.parentId`, `attrs.multiple`, `attrs["hierarchical-select"]`, `attrs.type`, `attrs.categoryId`, and `attrs.source`
- use only when source data has a hierarchy relationship

Sorting filter:

- CRM export type is `sorting-filters`
- `binding: "__filter_<filterVarId>"`
- `attrs.data.list` points to the sorted source list
- `attrs.sort_list[]` entries include `mapkey`, `title`, `orderby`, and `order`
- downstream data-list consumers reference the filter variable in `attrs.data.sortingfilter[]`

Range filter:

- `type: "range-filter"`
- `label: "Range filter"`
- `binding: "__filter_filter_Range_BudgetNumber"`
- numeric display and behavior settings live in `attrs`
- studied values include `number_max: 100000`, `number_step: 1000`, `prefix.value: "USD "`, `displayThousandths: "1"`, and `rounded-to: "0"`

Additional `Sales_Management_AD.yap` export-proven controls:

- Checkbox filter: `type: "check-filter"`, `binding: "__filter_<filterVarId>"`, options from `attrs.data.list`, `attrs.display_f`, `attrs.value_f`, optional `attrs.data.filter[]`, and display settings such as `search-enable`, `more-enable`, and `dropdown-enable`.
- Select filter: `type: "select-filter"`, `binding: "__filter_<filterVarId>"`, options from `attrs.data.list`, `attrs.display_f`, and `attrs.value_f`.
- Date filter: `type: "date-filter"`, `binding: "__filter_<filterVarId>"`; downstream date-field conditions use the filter variable expression in condition `right`.
- Check range: `type: "check-range"`, `binding: "__filter_<filterVarId>"`, with `attrs.options[]` range entries. Downstream consumer shape remains unproven in this export.
- Relative period: `type: "relative-period"`, `binding: "__filter_<filterVarId>"`, with `attrs["choice-options"][]`. Downstream consumer shape remains unproven in this export.
- Apply button: `type: "apply-button"`; referenced by click-apply filters through `attrs.apply_btn`.
- Remove filters: exported as `type: "remove-filers"` in this sample. No explicit reset target shape was visible, so clear-all vs selected-reset behavior remains runtime-sensitive.

## Chart Conditions

Filter controls affect charts through `page.exts[].attr.settings.Conditions`.

Variable expression shape:

```json
{
  "exprType": "variable",
  "valueType": "string",
  "id": "__filter_filter_Search",
  "type": "expr",
  "name": "filter_Search"
}
```

Rules:

- expression `name` must match a page `filterVars[].id`
- expression `id` must be `__filter_` plus the expression `name`
- condition `left` must be a field on the referenced source list
- nested condition groups use `conditions[]` and must be validated recursively

Observed chart condition counts:

- pie chart: 3 conditions, bound to search, radio, and range filters
- column chart: 4 top-level conditions, including radio/range filters plus nested static RSVP conditions
- line chart: 2 conditions, bound to radio and range filters

## ReplaceIds

`Resource.ReplaceIds` remains local numeric resource ids only.

Do not include UUID-like filter control ids or chart/summary/page-builder control ids in `ReplaceIds` unless a future export proves Yeeflow treats them as remappable resources.

## Validator Rules

Validators should catch:

- page `filterVars` is an array
- filter control `binding` resolves to a page `filterVars[].id`
- radio filter `attrs.data.list.ListID` resolves to a packaged list
- search filter downstream `attrs.data.fulltext[]` variable references resolve
- hierarchy filter source list/display/value/parent/child fields resolve when list-backed
- sorting filter `attrs.sort_list[].orderby` fields and downstream `attrs.data.sortingfilter[]` variable references resolve
- condition `left` fields resolve to the referenced source list fields
- condition variable expression `name` resolves to page `filterVars`
- condition variable expression `id` equals `__filter_` plus `name`
- nested condition groups are checked recursively
- Data Filter controls that bind with `__filter_` must resolve to `page.filterVars[].id`
- click-apply filters with `attrs.apply_t = "2"` must reference an existing `apply-button` through `attrs.apply_btn`
- Remove filters explicit reset targets must resolve when present; missing/unknown target structure should warn first
- unknown Data Filter control types should warn first

## Stop Conditions

Stop before generation if:

- a filter control binding cannot be matched to `filterVars`
- a chart condition references a missing filter variable
- a condition left field does not exist on the source list
- a radio filter source list is missing from `Data.Childs[]`
- a hierarchy source/list field reference is missing
- a sorting option points to an unknown field
- package validation, graph validation, or wrapper round-trip checks fail
- runtime testing is required but has not been explicitly authorized

## Runtime Checklist

For each newly generated dashboard `.yap` in this learning loop, import-test after local validation when the user has authorized runtime testing.

Confirm:

- import metadata parses expected name and description
- app appears in Shared Workspace
- dashboard opens
- filter controls render
- summary/table/chart content renders
- source list opens without a visible `datas/query` failure
- deeper interactive filter behavior is tested before claiming dynamic filtering is fully proven

---

<a id="knowledge-base-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#knowledge-base-pattern -->

# Knowledge Base Dashboard Pattern

Use this reference after the minimal dashboard, data-list, lookup, and Collection-control patterns are already understood.

Source studied: `/Users/Renger/Downloads/Knowledge Base_1.yap`

Runtime-proven generated baseline: `knowledge-base-generated-v4.yap`

Phase 1 completion baseline: `knowledge-base-phase1-full.generated.yap`

## Source Shape

The Knowledge Base template is an app-level `.yap` with:

- one root app/listset
- local `Categories`, `Sections`, and `Articles` data lists
- Type `103` dashboard pages: `Home Page`, `Search`, and `Admin`
- no approval forms, workflows, reports, AI modules, connections, or document libraries

## Safe Generated v4 Pattern

The first runtime-proven generated package includes only:

- root app shell
- one `Home Page` Type `103` dashboard
- local `Categories` list
- local `Articles` list
- plain text article category labels
- local category and article sample rows
- article Collection
- category Collection
- search filter bound to page `filterVars`
- dynamic fields inside Collection item templates
- meaningful `nv_label` names

Defer lookup fields, `Sections`, nested Collections, rich text, images, detail-page links, query-param navigation, and Admin action cards until the simpler package has imported and opened.

## Dashboard Rules

- Dashboard page lives in root `Layouts[]` with `Type: 103`.
- Embedded page JSON lives in `LayoutInResources[0].Resource`.
- `LayoutInResources[0].ID` and `RefId` match the dashboard `LayoutID` for the Knowledge Base source pattern.
- Root navigation must include duplicate Type `103` entries for the dashboard page when following the proven generated dashboard shell pattern.
- Search filter binding uses `__filter_<filterVarId>`, for example `__filter_filter_Keywords`.
- Collection fulltext values reference the same bound filter variable.

## Data Rules

- Generated lists must preserve native Title metadata:
  - `FieldName: "Title"`
  - `Status: 0`
  - `IsSystem: true`
  - `IsIndex: true`
- `FieldIndex: 0`
- The first safe package should use a plain text category label on `Articles`; article-to-category lookup must be introduced only in a later isolation package.
- When lookup is introduced, it must target the local generated Categories list and display `Title`.
- Local lookup sample values may reference local category sample row IDs, and those IDs must be in `ReplaceIds`.
- Do not copy the source export's self lookup on `Categories.Parent category` into the first generated package; it creates a dependency cycle and should be isolated separately.
- Knowledge Base lookup isolation v5-v7 showed that `Articles.Text3` lookup is not runtime-safe: Articles stayed on a loading spinner with local lookup sample values, with blank lookup values, and with the lookup hidden from the list layout.
- The source export uses `Articles.Text4` for Category lookup. A source-like `Text4` lookup package (`knowledge-base-lookup-v8-text4-lookup.yap`) validates, round-trips, imports, and renders Home, but Categories and Articles stay on loading spinners. Do not promote lookup generation from this package.
- A narrower source-like package (`knowledge-base-lookup-v9-text4-only.yap`) removes the generated `Text3` placeholder and adds only blank `Articles.Text4` lookup metadata. It imports, renders Home, and opens Categories, but Articles still stays on a loading spinner. This narrows the blocker to Articles lookup metadata.
- A plain field-slot package (`knowledge-base-text4-input-v10.yap`) adds `Articles.Text4` as an input field instead of lookup metadata. It imports, Home renders, and Categories opens, but Articles stays on a loading spinner with Chrome console `Uncaught RangeError: Wrong length!`. Do not treat plain non-contiguous `Text4` as proven.
- The user's exported v10 update added `Categories.Decimal1` Order and `Articles.Text3` Category Lookup sorted by `Decimal1`, while leaving `Articles.Text4` as a plain input slot. The generated reproduction (`knowledge-base-category-lookup-v11.yap`) imports and opens. Home and Categories render immediately; Articles loaded after one refresh; the new-item lookup dropdown resolved Categories in Order sequence. This is a proven later-stage lookup isolation, not the safest first baseline.

## Runtime Lessons

- `knowledge-base-generated-v1.yap` uploaded, imported, and opened, but rendered Yeeflow's empty component shell instead of generated app content.
- v2/v3 imported and rendered Home/Categories, but `Articles` stayed on the loading spinner.
- v4 passed runtime: Home rendered article/category Collections, `Categories` opened with sample rows, and `Articles` opened with sample rows.
- Root causes fixed by v4: child list metadata followed the app-level baseline, root `ListSetID` was not added directly to the root `ListModel`, list `LayoutView` stayed `null`, and native `Title.FieldIndex` was set to `0`.
- v5/v6/v7 lookup isolations imported and rendered Home/Categories, but Articles stayed on a loading spinner. v8 imported and rendered Home, but Categories and Articles stayed on loading spinners. v9 restored Categories runtime while Articles still spun. v10 shows that plain non-contiguous `Text4` also leaves Articles spinning. v11 proves the manually discovered `Categories.Decimal1` sort + `Articles.Text3` lookup + `Articles.Text4` plain slot pattern, with a first-open refresh caveat.
- Phase 1 combines v4, v11, a plain `Sections` list, `Home Page`, and `Article Library`. Runtime import/open passed: Home rendered article/category/section Collections, Article Library rendered article rows, Categories/Sections/Articles rendered sample rows, and the Articles add form displayed Category Lookup. Sections and Articles may need refresh stabilization on first list open; Chrome console can log Yeeflow `RangeError: Wrong length!` during the transition. Treat Phase 1 as a learning/completion baseline with a caveat, not final production readiness.

## Stop Conditions

Stop before generation when:

- an article Collection source list is missing
- a lookup source/target cannot resolve locally
- lookup fields are being added before the plain text category-label baseline has passed
- a nested Collection filter depends on an unproven current-item comparison shape
- query-param page navigation has not been isolated
- generated Title metadata would violate the native Title rule

## Runtime Checklist

After import:

- app tile appears in Shared Workspace
- app opens
- Home dashboard renders
- search filter appears
- article Collection renders local rows
- category Collection renders local rows
- `Categories` list opens without datas/query `400`
- `Articles` list opens without datas/query `400`
- `Sections` list opens and renders local rows when included
- Article Library opens and renders article Collection rows when included

---

<a id="minimal-dashboard-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#minimal-dashboard-pattern -->

# Minimal Dashboard Pattern

Source of truth: `Test Dashboard Only.yap`.

Proven generated package: `generated-dashboard-minimal-v1.yap`.

Runtime result: imported into `https://<yourdomain>.yeeflow.com`, appeared in Shared Workspace after refresh, opened as `Dashboard | Generated Dashboard Minimal v1`, and rendered the empty dashboard page shell.

## App Shape

- root app/listset only
- `Data.Childs: []`
- `Data.Forms: []`
- `Data.FormReports: []`
- `Data.DataReports: []`
- `Data.FormNewReports: []`
- `Data.AppGroups: []`
- `Data.AppTags: []`
- `Data.AppMetadatas: []`
- `Data.AppThemes: []`
- `Data.AppComponents: []`
- `Data.OtherModules: []`

## Root ListModel

Required fields observed in the working export:

- `AppID: 41`
- `Type: 1024`
- `Perm: 0`
- `CustomType: ""`
- `WorkspaceID` present
- `IconUrl` non-empty
- `CreatedBy` and `ModifiedBy` populated
- `LayoutView` parseable JSON

`LayoutView.sort[]` contains two dashboard navigation entries:

- `AppID: 41`
- `ListID = dashboard LayoutID`
- `ListSetID = root ListID`
- `Type: 103`
- `Title: "Dashboard"`

The first nav entry includes `IsHidden: false`; the second does not.

## Dashboard Layout

The working minimal dashboard is a root `Data.Item.Layouts[]` entry:

- `LayoutID`: local dashboard page ID
- `ListID`: root app/ListSet ID
- `Type: 103`
- `Title: "Dashboard"`
- `LayoutView: null`
- `Ext2: "{\"src\":true}"`
- `IsDefault: false`
- `IsItemPerm: false`
- `LayoutInResources: []`

Do not invent `LayoutInResources[0].Resource` for the first minimal package.

## ReplaceIds

For the minimal dashboard-only pattern, `ReplaceIds` contains exactly:

1. dashboard `LayoutID`
2. root app/ListSet `ListID`

Do not include tenant IDs, user IDs, workspace IDs, or non-local external references.

## Generation Rules

- Clone the minimal export structure.
- Remap only root app/ListSet ID and dashboard LayoutID into a fresh local ID family.
- Preserve source tenant/user metadata for the same Yeeflow sandbox.
- Keep `AppThemes` empty for the minimal baseline; this is warning-only.
- Build the wrapper and round-trip validate.
- Import-test before adding child lists, widgets, embedded page JSON, or `exts`.

---

<a id="runtime-testing"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#runtime-testing -->

# Runtime Testing

Only operate Yeeflow UI when the user explicitly asks for runtime testing.

Runtime environment:

`https://<yourdomain>.yeeflow.com`

## Import Flow

1. Open Yeeflow.
2. Go to Shared Workspace.
3. Click New application.
4. Choose Import application.
5. Upload the generated `.yap`.
6. Click Next step.
7. Confirm the metadata dialog.
8. Refresh the workspace if the card does not appear immediately.
9. Open the imported app.

## Evidence To Capture

Success:

- import metadata dialog shows expected title/description/icon
- app appears in Shared Workspace
- app opens
- dashboard navigation renders
- dashboard page shell or content renders

Failure:

- failed API endpoint
- request payload shape
- response body or toast
- console errors
- whether the metadata dialog parsed the package
- whether failure happened before or after final OK

If import fails, do not patch the complex package directly. Create the next smaller isolation package with fresh IDs.

---

<a id="service-desk-pro-stage-c-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-c-pattern -->

# Service Desk Pro Stage C Pattern

Use this pattern when resuming Service Desk Pro-style dashboard generation after the generic dashboard baselines have passed.

Source export studied: `/Users/Renger/Downloads/Service Desk Pro (1).yap`

Proven generated package: `service-desk-pro-dashboard-stage-b-or-c.generated.yap`

Runtime result: imported into `https://<yourdomain>.yeeflow.com`, appeared in Shared Workspace, opened as `Executive Dashboard | Service Desk Pro Dashboard Stage C`, and rendered the static Executive Dashboard content.

## Scope

Stage C is intentionally static.

Included:

- one root app/listset
- one Type `103` `Executive Dashboard` page
- embedded page JSON resource
- static headings
- static filter note
- static KPI-card placeholders
- static chart and operational placeholders

Excluded:

- child data lists
- dashboard `exts`
- `filterVars`
- `tempVars`
- summary/chart/table data bindings
- `opendashboard` actions
- Settings page
- approval forms, workflows, AI modules, reports, connections, and document libraries

## Resource Pattern

- `Layout.Type = 103`
- `Layout.LayoutView = null`
- `Layout.Ext2 = "{\"src\":true}"`
- `LayoutInResources[0].ID = LayoutID`
- `LayoutInResources[0].RefId = LayoutID`
- `LayoutInResources[0].Resource` contains page JSON
- root navigation points to the dashboard `LayoutID`
- `Resource.ReplaceIds` contains only the root app/ListSet ID and dashboard `LayoutID`
- `Resource.ReportIds = []`
- `Resource.FormKeys = []`

## Page JSON Pattern

Top-level keys:

- `children`
- `attrs`
- `title`
- `ver`
- `filterVars`
- `tempVars`
- `exts`
- `actions`

Stage C values:

- `title: "Executive Dashboard"`
- `ver: 2`
- `filterVars: []`
- `tempVars: []`
- `exts: []`
- `actions: []`

Proven static component types:

- `container`
- `heading`
- `text-editor`
- `flex_grid`
- `icon`
- `line`

## Service Desk Pro Risk Notes

The real `Executive Dashboard` includes 26 dashboard `exts`, two filter variables, 14 temp variables, summary controls, chart controls, and drill-down actions. Do not introduce those in one jump.

The real `Settings` page includes external ListSetID / ProcKey action references. Do not generate those actions until dependency handling is explicit.

## Next Stage

Stage D should add one local `Support Tickets` data list with the minimal fields needed for the first summary metric. Keep the dashboard static until the list imports and opens without a visible query failure.

---

<a id="service-desk-pro-stage-d-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-d-pattern -->

# Service Desk Pro Stage D Pattern

Use this reference after `skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-c-pattern` when resuming the Service Desk Pro dashboard rebuild.

## Proven Package

- workspace package: `service-desk-pro-dashboard-stage-d.generated.yap`
- Downloads copy: `/Users/Renger/Downloads/service-desk-pro-dashboard-stage-d.generated.yap`
- source export studied: `/Users/Renger/Downloads/Service Desk Pro (1).yap`
- runtime result: imported into `https://<yourdomain>.yeeflow.com`, appeared in Shared Workspace, opened, rendered the static Executive Dashboard, and opened the local `Support Tickets` list with six rows and no visible `datas/query` failure

## Scope

Stage D adds one local `Support Tickets` child data list to the proven Stage C static Executive Dashboard shell.

It intentionally excludes:

- dashboard `exts`
- bound summary values
- chart widgets
- dashboard filters
- reports
- approval forms
- workflows
- Settings page
- Drill-down Tickets List
- Help Guide
- external dependencies

## Minimal Support Tickets Field Pattern

Use only the minimum local fields needed before adding the first bound KPI:

- native `Title` displayed as `Ticket Title`
- `Text1` displayed as `Ticket ID`
- `Text2` displayed as `Priority`
- `Text3` displayed as `Status`
- `Text4` displayed as `Assigned Team`
- `Datetime1` displayed as `Created Time`
- `Decimal1` displayed as `First Response Hours`
- `Decimal2` displayed as `Resolution Hours`
- `Bit1` displayed as `First Response SLA Compliance`
- `Bit2` displayed as `Resolution SLA Compliance`

The native Title field must preserve:

- `FieldName: "Title"`
- `Status: 0`
- `IsSystem: true`
- `IsIndex: true`

## ReplaceIds

Include local IDs for:

- root app/listset
- Type `103` Executive Dashboard layout
- Support Tickets list
- Support Tickets list view layout
- Support Tickets fields
- local sample rows

Do not introduce external dependency IDs in Stage D.

## Runtime Checklist

After import:

- app appears as `Service Desk Pro Dashboard Stage D`
- app opens to `Executive Dashboard`
- navigation includes `Support Tickets`
- Executive Dashboard renders static placeholder content
- `Support Tickets` opens as a data list
- sample rows render
- no visible `api/crafts/datas/{AppID}/{ListID}/query` failure occurs

## Generator Rule

Before binding any Service Desk Pro dashboard KPI, prove the source list imports and queries by itself. Stage E may add exactly one bound summary over this local list, using a fresh ID family.

---

<a id="service-desk-pro-stage-e-f-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-e-f-pattern -->

# Service Desk Pro Stage E and F Pattern

Use this after `skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-d-pattern`.

## Stage E Proven Pattern

Package: `service-desk-pro-dashboard-stage-e.generated.yap`

Runtime result: imported and opened in Yeeflow. `Total Submitted` rendered `6`, matching the six local `Support Tickets` rows.

Rules:

- add exactly one `summary` page control first
- add one matching `page.exts[]` item with `category: "___Pivot___"` and `key: "summary"`
- bind `attr.ListID` to the local `Support Tickets` list and `attr.ListSetID` to the root app/listset
- use `COUNT(ListDataID)` for ticket counts
- include the summary control id in `Resource.ReportIds`
- do not add Service Desk filters, charts, reports, Settings, or drill-down actions in Stage E

## Stage F1 Proven Pattern

Package: `service-desk-pro-dashboard-stage-f1.generated.yap`

Runtime result: imported and opened in Yeeflow. KPI values rendered as:

- `Total Submitted`: `6`
- `Resolved Tickets`: `2`
- `Open Tickets`: `4`
- `Critical Open`: `0`

Rules:

- bind each KPI card as a separate `summary` control
- include every summary control id in `Resource.ReportIds`
- use simple local field conditions:
  - resolved count: `Text3` in `Resolved`, `Closed`
  - open count: `Text3` in `New`, `Assigned`, `In Progress`, `On Hold`
  - critical open count: open status condition plus `Text2 = Critical`
- keep filter variables out until a dedicated filter stage

## Stage F2 Proven Pattern

Package: `service-desk-pro-dashboard-stage-f2.generated.yap`

Runtime result: imported and opened in Yeeflow. KPI cards still rendered `6`, `2`, `4`, and `0`; `Open Tickets by Priority` rendered as a column chart with Medium and High buckets.

Chart:

- `type: "bar-chart"`
- `key: "bar-chart"`
- `chartType: "2"`
- grouping row: `Text2` / Priority
- value: `COUNT(ListDataID)`
- condition: active status set over `Text3`

Rules:

- chart control id must be present in `page.exts[].i`
- chart control id must be included in `Resource.ReportIds`
- keep the first Service Desk chart grouped over a local text/radio field before introducing lookup groupings or SLA report resources
- if a later chart fails at runtime, isolate with KPI summaries plus only the chart control and inspect browser/network evidence before adding more charts

## Stage G Proven Pattern

Package: `service-desk-pro-dashboard-stage-g.generated.yap`

Runtime result: imported and opened in Yeeflow. Executive Dashboard rendered, and the static `Settings` page rendered Service Desk configuration cards.

Rules:

- add the Settings page as a separate Type `103` layout
- set `LayoutInResources[0].ID` and `RefId` to the Settings layout id
- include the Settings layout id in `ReplaceIds`
- add a root navigation entry for Settings
- keep Settings cards static until linked target lists/actions are included and validated
- do not copy original Settings links blindly; they can reference external ListSetID or ProcKey dependencies

## Stage H Proven Pattern

Package: `service-desk-pro-dashboard-stage-h.generated.yap`

Runtime result: imported and opened in Yeeflow. Executive Dashboard, Drill-down Tickets List, Help Guide, and Support Tickets all rendered.

Rules:

- add `Drill-down Tickets List` and `Help Guide` as separate Type `103` layouts after Executive Dashboard and Settings are already proven
- static drill-down rows are safe as a scaffold
- static help sections are safe as a scaffold
- keep dynamic drill-down `collection`, `tempVars`, filter variables, and `opendashboard` actions out until each is isolated and runtime-tested
- after adding secondary pages, re-open the local source list to confirm no `datas/query` regression

---

<a id="service-desk-pro-stage-i-l-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-i-l-pattern -->

# Service Desk Pro Stage I-N Pattern

Use this after `skills/yeeflow-dashboard-generator/references/collection-01.md#service-desk-pro-stage-e-f-pattern` when continuing the Service Desk Pro dashboard rebuild beyond static Settings, Help, and Drill-down scaffolds.

## Stage I: Local Support Teams Filter Source

Package: `service-desk-pro-dashboard-stage-i.generated.yap`

Runtime result: imported and opened in Yeeflow. Executive Dashboard rendered a Support Teams select filter and a submitted-period staged control. The local `Support Teams` list opened and rendered rows.

Rules:

- add `Support Teams` as a local child data list before binding the select filter
- preserve native `Title` metadata for the list
- use local rows for first filter-source proof
- keep submitted-period date filtering unbound until date semantics are isolated

## Stage J: Opendashboard Modal Action

Package: `service-desk-pro-dashboard-stage-j.generated.yap`

Runtime result: imported and opened in Yeeflow. Clicking the Executive Dashboard `Drill-down Tickets List` card opened the included Type `103` Drill-down page in a modal.

Rules:

- include the target Type `103` page before adding an action
- attach the action through the clickable control's `attrs.control_action`
- use `type: "opendashboard"` with `attrs.data.page.PageID` pointing to the local dashboard layout id
- graph validation should report a `dashboardActionPage` edge
- query params may appear in the URL, but do not claim temp-variable mapping until separately proven

## Stage K: Bound Drill-down Data-list

Package: `service-desk-pro-dashboard-stage-k.generated.yap`

Runtime result: imported and opened in Yeeflow. Direct navigation and modal launch both rendered the dashboard `data-list` table bound to local `Support Tickets`.

Rules:

- replace static drill-down rows with a dashboard `data-list` control only after the target list is already proven
- `attrs.data.list.AppID`, `ListID`, `ListSetID`, and `Type` must resolve to the local source list
- `attrs.listarr[].Field` must resolve to fields in the local list
- start with `attrs.data.filter: []`

## Stage L: Static Drill-down Filter

Package: `service-desk-pro-dashboard-stage-l.generated.yap`

Runtime result: imported and opened in Yeeflow. Drill-down table rendered only high-priority rows `T-1001` and `T-1006`.

Proven static filter:

```json
{
  "pre": "and",
  "left": "Text2",
  "op": "0",
  "right": "High",
  "showCus": true
}
```

Rules:

- prove static scalar `attrs.data.filter` before variable-driven filter expressions
- keep the filter field name aligned with the generated local list field, not the original export field if the local model differs
- for the current Support Tickets model, priority is `Text2`

## Stage M: Submitted-period Binding and Page Polish

Package: `service-desk-pro-dashboard-stage-m.generated.yap`

Runtime result: imported and opened in Yeeflow. Executive Dashboard rendered the four KPI cards and submitted-period control; clicking `Today` changed the four KPI values from `6`, `2`, `4`, `0` to `0`, `0`, `0`, `0`. Settings rendered 3-column card grids with wider spacing. Help Guide rendered improved static card-grid sections. Support Tickets and Support Teams lists opened without visible query failures.

Proven submitted-period condition:

```json
{
  "pre": "and",
  "left": "Datetime1",
  "op": "0",
  "right": [
    {
      "exprType": "variable",
      "valueType": "string",
      "id": "__filter_f_SubmittedPeriod",
      "type": "expr",
      "name": "f_SubmittedPeriod"
    }
  ],
  "showCus": false
}
```

Rules:

- apply the submitted-period condition to each relevant local Support Tickets `summary` and chart `exts`
- for the current generated Support Tickets model, the submitted date field is `Datetime1`
- the filter variable id is `__filter_f_SubmittedPeriod`; the variable name is `f_SubmittedPeriod`
- each target binding should have exactly one submitted-period condition
- Settings grids are safe with three columns and `24px` column and row gaps
- Help Guide page polish is safe as static cards, but links/actions remain excluded until target resources are proven
- update visible helper copy when a formerly staged filter becomes active

## Stage N: Active Submitted-period Helper Copy

Package: `service-desk-pro-dashboard-stage-n.generated.yap`

Runtime result: imported and opened in Yeeflow. Executive Dashboard rendered the updated helper copy that says Submitted period is bound to the generated Support Tickets Created Time field. Default KPI values rendered as `6`, `2`, `4`, `0`; clicking `Today` changed all four values to `0`. Settings, Help Guide, Drill-down Tickets List, Support Tickets, and Support Teams all opened without visible query failures.

Rules:

- use a fresh ID family even for copy-only page JSON cleanup
- keep the Stage M submitted-period conditions unchanged when only updating helper copy
- the active helper copy should mention both local filters: Support Teams and Submitted period
- a copy-only Type 103 dashboard change still requires wrapper validation and runtime import testing
- Stage N is the current safest Service Desk Pro generated dashboard baseline before isolating query-param to `tempVars`, original collection cards, Settings actions, or SLA report resources

## Remaining Stop Conditions

Stop before generation if:

- query-param to `tempVars` mapping is required but has not been isolated
- original collection card layout must be copied without a successful local baseline
- Settings tile targets external `ListSetID` or `ProcKey` dependencies
- SLA report resources are required but the report definitions and source lists are not included

---

<a id="simple-elements-pattern"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#simple-elements-pattern -->

# Simple Elements Pattern

Source of truth: `Test Dashboard Only (2).yap`.

Proven generated package: `generated-dashboard-simple-elements-v2.yap`.

Runtime result: imported into `https://<yourdomain>.yeeflow.com`, appeared in Shared Workspace after refresh, opened as `Dashboard | Generated Dashboard Simple Elements v2`, and rendered static dashboard page content.

## App Shape

- root app/listset only
- one Type `103` dashboard layout
- no child data lists
- no approval forms
- no reports
- no AI modules, connections, or document libraries
- app metadata arrays present, including empty `AppThemes` and `AppComponents`

## Dashboard Layout

The dashboard layout keeps the minimal shell fields:

- `Type: 103`
- `LayoutView: null`
- `Ext2: "{\"src\":true}"`
- root navigation points to the dashboard `LayoutID`

It adds one page resource:

- `LayoutInResources[0].ID = LayoutID`
- `LayoutInResources[0].RefId = LayoutID`
- `LayoutInResources[0].Resource` is JSON string page content

This inline page-resource ID pattern is valid for simple static dashboard content.

## Page JSON

Required top-level keys:

- `children`
- `attrs`
- `title`
- `ver`
- `filterVars`
- `tempVars`
- `exts`

For static simple elements:

- `filterVars: []`
- `tempVars: []`
- `exts: []`

## Proven Static Component Types

- `container`
- `heading`
- `text-editor`
- `action_button`
- `flex_grid`
- `icon`
- `line`
- `picture`

Also observed and proven:

- background image config under `attrs.common.background.normal.classic.image`
- hover/normal background overlays
- containers with `action-type: "2"` and `link.url: "#"`
- Font Awesome icon names in `attrs.icon.icon`
- rich text HTML in `text-editor.attrs.value`

## ReplaceIds

For this pattern, include exactly:

1. root app/ListSet ID
2. dashboard `LayoutID`

Because the page resource uses `ID = RefId = LayoutID`, do not add a separate page-resource ID.

## Generation Rules

- Clone the studied static page JSON when producing the first simple-elements test.
- Remap only root app/ListSet ID and dashboard LayoutID to a fresh ID family.
- Keep no data lists, forms, reports, widgets, or `exts` for this stage.
- Validate and import-test before introducing data-bound widgets or charts.

---

<a id="validator-rules"></a>

<!-- source: skills/yeeflow-dashboard-generator/references/collection-01.md#validator-rules -->

# Validator Rules

Use the project validators rather than copying them into this skill:

```bash
node validate-yap-package.js <resource-or-yap> --mode generator --stage final
node validate-yap-graph.js <resource-or-yap> --mode generator --stage final
node build-yap-wrapper.js <resource.json> <output.yap> --title "<title>" --validation-mode generator
```

## Minimal Dashboard Checks

For an empty dashboard-only app:

- root app/listset exists
- root app `LayoutView` parses
- root navigation has at least one `Type: 103` item
- each dashboard navigation item resolves to a root `Data.Item.Layouts[]` page
- dashboard layout has `Type: 103`
- dashboard `LayoutView` is `null`
- dashboard `LayoutInResources` is an array
- empty dashboard `LayoutInResources` is allowed only when `Ext2` parses as `{ "src": true }` and dashboard `LayoutID` is in `ReplaceIds`
- `AppTags`, `AppMetadatas`, `AppThemes`, and `AppComponents` are arrays even when empty
- `AppThemes: []` is a warning for minimal dashboard-only apps, not a hard failure

## Rich Dashboard Checks

When embedded page JSON or widgets are introduced:

- `LayoutInResources[].Resource` parses as JSON
- inline dashboard page resources may use `LayoutInResources[0].ID = RefId = LayoutID` when copied from the static simple-elements export pattern
- page has `children`, `attrs`, `title`, `ver`, `filterVars`, and `tempVars`
- populated `exts` is an array
- `save_var` references resolve to `tempVars`
- dashboard data-source `ListID` values resolve to package lists or reports
- `opendashboard` actions resolve to package Type `103` pages
- external form/listset actions are reported as dependencies in compatibility mode and errors in generator mode

## Current Root Cause Finding

The prior failing generator path added child lists, embedded page JSON, widgets, and dashboard `exts` before proving the empty dashboard shell. The working minimal export shows the safe first generated package should contain only the root app/listset and empty Type `103` dashboard layout.

The second proven path adds static page JSON only: containers, headings, rich text, buttons, icons, flex grids, divider, pictures, and empty `exts`.

The third proven path adds one local list, summary controls, and a dashboard data table.

The fourth proven path adds pie, column, and line chart widgets backed by the same local list.

The fifth proven path adds search, radio, and range filter controls plus chart conditions bound to page `filterVars`. This path is validation-proven and runtime-proven for import/open/render/source-list smoke testing.

## Data-Bound Dashboard Checks

For the proven local-list summary/table pattern:

- `data-list` controls resolve `attrs.data.list.ListID` to a packaged child list
- dashboard `exts[].attr.ListID` resolves to a packaged list
- dashboard `exts[].i` resolves to an actual page control id
- dashboard `exts[].i` is included in `Resource.ReportIds` when `ReportIds` is populated
- local sample row IDs, including `ListDatas` object keys, are included in `ReplaceIds`
- generated child list Title metadata remains `Status: 0`, `IsSystem: true`, `IsIndex: true`

## Chart Widget Checks

For the proven local-list pie/column/line chart pattern:

- chart controls use proven page control types: `pie-chart`, `bar-chart`, or `line-chart`
- chart `exts[]` entries use proven keys: `pie-chart`, `bar-chart`, or `line-chart`
- chart `exts[].i` resolves to a matching page control id
- chart `exts[].i` is included in `Resource.ReportIds` when `ReportIds` is populated
- chart `exts[].attr.ListID` resolves to a packaged list
- chart `exts[].attr.settings.rows[]` field names resolve to fields on the source list
- chart `exts[].attr.settings.columns[]` field names resolve to fields on the source list when present
- chart `exts[].attr.settings.values[]` field names resolve to fields on the source list or known system fields such as `ListDataID`
- chart type mappings remain explicit: pie `chartType: "0"`, line `chartType: "1"`, column/bar `chartType: "2"`

## Filter Control Checks

For the studied search/radio/range filter pattern:

- page `filterVars` is an array
- each filter control `binding` that starts with `__filter_` resolves to a page `filterVars[].id`
- radio filter `attrs.data.list.ListID` resolves to a packaged list
- condition `left` field names resolve to the source list fields
- condition variable expression `name` resolves to page `filterVars`
- condition variable expression `id` equals `__filter_` plus `name`
- nested condition groups are validated recursively
