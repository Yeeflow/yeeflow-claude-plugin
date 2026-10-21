# skills/yeeflow-data-list-generator/references collection 1

This file combines 6 documents from `skills/yeeflow-data-list-generator/references/` for the Claude directory build. A reference to `skills/yeeflow-data-list-generator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [examples-summary.md](#examples-summary)
- [knowledge-base-list-pattern.md](#knowledge-base-list-pattern)
- [metadata-guide.md](#metadata-guide)
- [operating-playbook.md](#operating-playbook)
- [validation-guide.md](#validation-guide)
- [ydl-structure-study.md](#ydl-structure-study)

---

<a id="examples-summary"></a>

<!-- source: skills/yeeflow-data-list-generator/references/collection-01.md#examples-summary -->

# Examples Summary

This skill intentionally summarizes examples instead of bundling bulky `.ydl`, `.yap`, or customer export files.

## Asset Inventory v5

Purpose: prove single-list generation.

Proved:

- generated standalone `.ydl` wrapper imports
- fields, views, sample rows, custom form, and form assignment work
- custom form controls render in designer
- `LayoutInResources[0].ID` and `RefId` must equal `LayoutID`

Use as the baseline for custom-form registration and usable form layout.

Source project artifacts:

- `asset-inventory-def.import-debug-v5.json`
- `asset-inventory.import-debug-v5.ydl` as test reference only
- `docs/collection-02.md#generated-data-list-baseline-asset-inventory-v5`

## Departments -> Employees

Purpose: prove staged related-list lookup generation.

Proved:

- standalone related `.ydl` files should be imported in stages
- reference list is imported/exported first
- dependent lookup is patched to real exported-back metadata
- single lookup sample values are plain target `ListDataID` strings
- external lookup record IDs must not be in dependent `Resource.ReplaceIds`

Source project artifacts:

- `departments-def.draft.json`
- `employees-def.final-after-departments-v4-sample-lookups.json`
- `departments-employees-generation-plan.md`
- `docs/collection-02.md#generated-related-data-lists-staged-lookup-pattern`

## NHIC Study

Purpose: learn real `.ydl` and `.yap` structures.

Studied lists:

- Portfolio Management
- Partner Management
- Communication Records

Observed:

- field/control type diversity
- lookup relationships
- views and Type 104 board-like view
- custom forms
- list workflows
- sample data shapes
- full `.yap` metadata resolves dependencies not present in the three standalone `.ydl` files

Reference summaries:

- `nhic-ydl-metadata.md`
- `nhic-yap-metadata.md`

## Visitor Access Management v11 Storage List Patterns

Source project references:

- `visitor-access-management-app-def.v11-five-fields-multitype.json`
- `docs/collection-02.md#data-list-generator-operating-playbook`
- `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`

Proves generated app-level child-list support for:

- `Text13` / `Text14` as text input storage
- `Decimal1` with `FieldType = "Decimal"` and `Type = "input_number"`
- `Text15` with text/radio-compatible choice storage
- `Bit1` with `FieldType = "Bit"` and `Type = "switch"`

Sample value shapes:

- Decimal: numeric values
- choice: selected option text
- Bit/switch: `"1"` or `"0"`

Use these when a generated approval form persists values into a generated request/storage list.

---

<a id="knowledge-base-list-pattern"></a>

<!-- source: skills/yeeflow-data-list-generator/references/collection-01.md#knowledge-base-list-pattern -->

# Knowledge Base List Pattern

Source studied: `/Users/Renger/Downloads/Knowledge Base_1.yap`

Use this reference for Knowledge Base-style generated child lists inside app-level `.yap` packages.

## Source Lists

- `Categories`: category name, icon, description, parent category lookup
- `Sections`: section name, description, category lookup
- `Articles`: article title, summary, content, feature image, category lookup, section lookup

## Safe Runtime-Proven Generated Lists

Start with only:

`Categories`

- `Title` as Category Name
- `Text1` as Description

`Articles`

- `Title` as Article Title
- `Text1` as Summary
- `Text2` as Category Label

## Hard Rules

- Keep every generated `Title` field native: `Status: 0`, `IsSystem: true`, `IsIndex: true`, `FieldIndex: 0`.
- Do not copy the source `Title.Status: 1` values into generated packages.
- Use plain text `Category Label` for the first runtime baseline instead of article-to-category lookup metadata.
- Do not include `Categories.Parent category` in the first package because it is a self lookup and creates a dependency cycle.
- Do not include `Sections`, article-to-category lookup, or article-to-section lookup until each lookup pattern has imported and opened cleanly in an isolated package.
- When local app-level lookup samples are reintroduced, the target category sample record IDs may be local generated IDs and must be included in `ReplaceIds`.
- Do not place Knowledge Base `Articles.Category` lookup in `Text3`; v5-v7 import tests showed Articles stayed on a loading spinner even with blank lookup values and the lookup hidden from the list view.
- The source template places `Articles.Category` in `Text4`, but source-like `Text4` lookup isolations still failed list runtime: v8 imported and rendered Home while Categories and Articles stayed on loading spinners; v9 removed the generated `Text3` placeholder and Categories opened, but Articles still stayed on a loading spinner. Keep the v4 plain text `Category Label` baseline until a reduced/source-aligned lookup isolation opens lists cleanly.
- v10 adds `Articles.Text4` as a plain input field and imports; Home renders; Categories opens; Articles stays on a loading spinner with Chrome console `Uncaught RangeError: Wrong length!`. Do not treat the plain `Text4` slot as runtime-proven.
- v11 is the first runtime-proven Knowledge Base lookup isolation. It follows the user's exported v10 update: add `Categories.Decimal1` as `Order`, keep `Articles.Text4` as a plain input slot, add `Articles.Text3` as `Category Lookup`, and set `Rules.sort-first` to `{ "SortName": "Decimal1", "SortByDesc": false }`. Keep article lookup sample values blank. Articles loaded after one refresh, and the new-item lookup dropdown resolved Categories in Order sequence.
- Phase 1 adds a plain generated `Sections` list and combines it with the v11 lookup shape. Runtime import/open passed: Categories, Sections, and Articles rendered local sample rows after refresh stabilization where needed. Keep `Sections.Category` and `Articles.Section` as plain label text until source-style lookup chains are isolated.

## Runtime Lessons

- A package can import while list runtime still fails; list navigation must be opened as part of the proof.
- v2/v3 showed that Home/Categories can work while Articles remains on a spinner.
- v4 proved Articles after removing first-stage lookup metadata and setting native `Title.FieldIndex: 0`.
- v5/v6/v7 showed that an unresolved lookup list runtime issue can persist even without local lookup sample values and even when the lookup column is not visible in the list layout.
- v8 showed that moving the lookup to the source-like `Text4` slot is not sufficient by itself; v9 narrowed the failure to Articles lookup metadata by restoring Categories runtime. Do not advance to local lookup sample values until lookup metadata alone opens cleanly.
- v10 proved non-contiguous generated `Text4` alone was not enough. v11 proved the combined `Text3` lookup plus `Text4` plain slot and category `Decimal1` sort pattern. Because Articles needed one refresh on first open, keep this as a lookup-isolation pattern rather than the default first Knowledge Base package.
- Phase 1 proves the three-list app shape for learning use, but keeps the same refresh caveat. Do not promote lookup sample values, section lookup fields, or nested dashboard Collections until separately proven.

## Validation Notes

Embedded child lists in an app-level `.yap` are validated through `validate-yap-package.js` and `validate-yap-graph.js`. Standalone `.ydl` validation requires a standalone `.ydl` resource shape and is not authoritative for embedded child list objects.

---

<a id="metadata-guide"></a>

<!-- source: skills/yeeflow-data-list-generator/references/collection-01.md#metadata-guide -->

# Metadata Guide

Use metadata to avoid guessing Yeeflow IDs and dependencies.

## Tools

- `inspect-ydl-package.js`: detailed package inventory for a `.ydl`.
- `extract-ydl-metadata.js`: clean list metadata from one or more `.ydl` exports.
- `inspect-yap-package.js` and `extract-yap-metadata.js` in the project can provide app-level metadata when available.

## .ydl Metadata

Good for:

- local list structure
- fields and field types
- views
- custom forms
- list workflows inside the list
- sample data value shapes
- lookup relationships among supplied `.ydl` files

Limitations:

- standalone `.ydl` files may not include all lookup targets
- ListSetID may be missing or only inferable
- external workflow targets may not resolve

## Full .yap Metadata

Use full `.yap` app metadata when final confidence matters.

It resolves:

- app/listset context
- all app lists
- lookup target lists
- display fields
- workflow target lists
- related app resources

In the NHIC study, `.ydl` metadata alone left several lookup targets unresolved. Full `.yap` metadata resolved the additional lists such as NHIC Grants and Contact Database.

## Dependency Maps

Use dependency maps for generated lists when dependencies are external or staged.

Include:

- source list/field
- dependency type
- target app/listset/list IDs
- target display field
- target sample/reference record IDs when sample lookup values are used
- status, such as `resolved`

Dependency maps are required for safe non-empty external lookup sample data.

## Safety Rules

- Preserve large numeric IDs as strings.
- Redact secrets/tokens/client credentials.
- Do not treat sample IDs as production metadata.
- Do not guess production IDs.
- For staged related standalone lists, import/export the reference list first and use exported-back metadata.

---

<a id="operating-playbook"></a>

<!-- source: skills/yeeflow-data-list-generator/references/collection-01.md#operating-playbook -->

# Data List Generator Operating Playbook

This is the first operating playbook for Codex-generated Yeeflow data lists. It covers `.ydl` data-list definitions and related app metadata, not approval workflows.

Reference materials:

- `inspect-ydl-package.js`
- `extract-ydl-metadata.js`
- `validate-ydl-list.js`
- `validate-ydl-against-yap.js`
- `build-ydl-wrapper.js`
- `docs/collection-02.md#validate-ydl-list`
- `docs/collection-02.md#validate-ydl-against-yap`
- `docs/collection-01.md#build-ydl-wrapper`
- `docs/collection-02.md#generated-data-list-baseline-asset-inventory-v5`
- `docs/collection-02.md#generated-related-data-lists-staged-lookup-pattern`
- `nhic-ydl-metadata.md`
- `nhic-yap-metadata.md`
- `docs/collection-01.md#approval-form-generator-operating-playbook`

## 1. Purpose

Codex can help generate Yeeflow data list definitions, but only through a staged, validated, metadata-aware process.

This playbook is for generated Yeeflow data lists:

- list schema
- fields and field types
- lookup relationships
- views
- custom data-list forms
- list workflows
- optional sample data

It is not the approval-form workflow generator. Approval forms use `.ywf` and decoded workflow `Def` structures; data lists use `.ydl` packages built around `Resource.Data.Item`.

Codex must not generate final `.ydl` or `.yap` packages directly from a business prompt. The safe path is decomposition, normalized design, dependency resolution, decoded draft generation, validation, then sandbox import/export testing.

## 2. Proven Findings

### .ydl Wrapper Format

Standalone Yeeflow data-list exports are top-level JSON wrappers with:

- `Title`
- `Description`
- `IconUrl`
- `IsListSet`
- `Resource`

The `Resource` field starts with:

```text
[______gizp______]
```

After that prefix, the payload is base64-encoded gzip data. Decoding produces Resource JSON. `Resource.Data` is itself a JSON string containing the real list package.

### Decoded Data Structure

The decoded `Resource.Data` object contains:

- `Item`: the main list resource
- `Childs`: child resources if included
- `Forms`: list workflows
- `FormReports`
- `DataReports`
- `FormNewReports`
- `AppGroups`
- `AppTags`
- `AppMetadatas`
- `AppThemes`
- `AppComponents`
- `PortalInfo`
- `OtherModules`

For the list itself, `Data.Item` contains:

- `ListModel`: list identity and app/list metadata
- `Defs`: field definitions
- `Layouts`: views and custom forms
- `PublicForms`
- `RemindRules`
- `FlowMappings`
- `ListDatas`: sample records

### Field Definitions

Fields are stored in `Item.Defs[]`. Common fields include:

- `FieldID`
- `ListID`
- `FieldName`: storage field such as `Title`, `Text1`, `Datetime1`, `Bit1`
- `InternalName`
- `DisplayName`
- `FieldType`
- `Type`: control type
- `DefaultValue`
- `Rules`: JSON string containing control and validation attrs
- `IsSystem`
- `IsSort`
- `IsIndex`
- `IsFilter`

Observed field/control patterns include:

| Normalized type | FieldType / Type examples |
| --- | --- |
| text | `Text` / `input` |
| longText | `Text` / `textarea` |
| choice | `Text` / `radio` |
| multiChoice | `Text` / `checkbox` |
| lookup | `Text` / `lookup` |
| date | `Datetime` / `datepicker` without time |
| datetime | `Datetime` / `datepicker` with time |
| boolean | `Bit` / `switch` |
| hyperlink | `Text` / `hyperlink` |
| list | `Text` / `list` |
| flowstatus | `Text` / `flowstatus` |

### Views And Custom Forms

`Item.Layouts[]` stores both views and custom forms.

Observed patterns:

- View layouts have `Type` empty or `0`.
- A board/pipeline-style view used `Type: 104`.
- Custom forms use `Type: 1` and may include a form `Resource` inside `LayoutInResources[]`.
- Some real exports contain `Type: 1` layout placeholders without parsed form resources; these should be tolerated in compatibility mode but not generated blindly.

View `LayoutView` is a JSON string with:

- `layout`: displayed/hidden columns
- `filter` or `query`
- `sort`
- `rowColor`

Custom form resources use a form JSON shape with:

- `children`
- `attrs`
- `title`
- `filterVars`
- `ver`
- `tempVars`

Controls bind directly to list storage fields such as `Title`, `Text2`, or `Datetime1`.

### Successful Generated Baseline

`asset-inventory.import-debug-v5.ydl` is the first successful generated data-list baseline in this track.

It proves:

- generated `.ydl` import can work
- generated fields, views, and sample records can be created
- generated custom forms can be assigned to New/Edit/View
- generated custom forms can open in the designer
- generated custom form controls can render
- export-back comparison can identify import remapping and rejected resource patterns

The durable reference is `docs/collection-02.md#generated-data-list-baseline-asset-inventory-v5`.

### Successful Related-List Lookup Baseline

The Departments -> Employees staged test is the first successful generated related-list lookup baseline.

It proves:

- generated related standalone `.ydl` packages should be imported in dependency order
- Departments was generated and imported first
- Departments was exported back to collect real remapped `ListSetID`, `ListID`, display field, and sample record IDs
- Employees.Department was patched to the real Departments lookup metadata
- Employees sample rows can use real Department `ListDataID` values
- external lookup sample IDs must be excluded from the Employees wrapper `Resource.ReplaceIds`
- if external lookup record IDs are included in `ReplaceIds`, Yeeflow remaps them during standalone import and the lookup displays as `(Deleted)`
- after the `ReplaceIds` fix, the Employees import test passed with sample rows displaying lookup values

The durable reference is `docs/collection-02.md#generated-related-data-lists-staged-lookup-pattern`.

### Sample Data

Sample records are stored in `Item.ListDatas`, keyed by `ListDataID`.

Observed value shapes:

- single lookup: target record ID string
- multi lookup: JSON-stringified array of target record IDs
- checkbox/multi-choice: JSON-stringified array
- date: `YYYY-MM-DD`
- datetime: `YYYY-MM-DD HH:mm:ss`
- switch/boolean: `"1"` or `"0"`
- hyperlink: URL string
- empty values: usually `""`

Sample data should be treated as example data only. It is not automatically safe for generated production packages.

### List Workflows

List workflows are stored in `Data.Forms[]`. These are workflow definitions associated with a data list, not approval workflows.

Observed workflow types and nodes:

- `workflowType: 1`
- `StartNoneEvent`
- `ContentList`
- `QueryData`
- `AI`
- `EndNoneEvent`
- `SequenceFlow`

Observed patterns include:

- current-list update via `ContentList`
- `QueryData` against another list
- AI enrichment followed by list update
- sync/update helper workflows

### Lookup Relationships

Lookup field Rules contain:

- `appid`
- `listsetid`
- `listid`
- `listfield`
- `multiple`
- `search_fields`
- optional filters/query metadata

Full `.yap` metadata is needed to resolve lookup dependencies reliably.

In the NHIC study, `.ydl`-only metadata resolved only relationships among the three supplied `.ydl` files. The full NHIC `.yap` resolved the additional lookup targets `NHIC Grants` and `Contact Database`.

### .ydl vs .ywf vs .yap

| Topic | .ydl | .ywf | .yap |
| --- | --- | --- | --- |
| Purpose | Data list export | Approval/workflow form export | Full application export |
| Wrapper payload | `Resource` gzip/base64 | `Def` base64 | `Resource` gzip/base64 |
| Main decoded structure | `Resource.Data.Item` | decoded workflow `Def` | app-level `Resource.Data` |
| Fields | `Item.Defs[]` | `variables.basic` / `variables.listref` | list resources plus forms/reports/modules |
| Forms | `Item.Layouts[]` custom forms | `pageurls[].formdef` | embedded list/form/app resources |
| Workflows | `Data.Forms[]` list workflows | `childshapes` approval workflow | app workflows and forms |
| Sample data | `Item.ListDatas` | not typical | usually not in child lists from studied app |
| Best use | list schema and sample-data study | approval process generation | full dependency resolution |

## 3. Standard Pipeline

```text
Business requirement
  -> requirement decomposition
  -> normalized data list spec
  -> field model
  -> lookup/dependency model
  -> view model
  -> custom form model
  -> workflow model if needed
  -> sample data plan if requested
  -> decoded .ydl draft
  -> structural validation
  -> app-context validation
  -> wrapper build only after validation
  -> sandbox import/export round-trip
```

Codex should stop at the earliest failing stage. A generated data list is not wrapper-ready until both structural and app-context validations pass with no unresolved placeholders or required dependency gaps.

## 4. Metadata Rules

Use metadata in this order:

1. Existing full `.yap` app metadata when target app dependencies matter.
2. Related `.ydl` metadata for local list structure and sample value-shape evidence.
3. User-provided dependency mappings for resources not present in exports.

Rules:

- `.ydl` metadata is useful for local list structure, fields, views, custom forms, and sample records.
- Full `.yap` metadata is required for final confidence when lookups, list workflows, app resources, dashboards, reports, or external target lists exist.
- Never guess `AppID`, `ListSetID`, `ListID`, lookup target list IDs, target display fields, or workflow target lists.
- Preserve large IDs as strings.
- Redact and do not reuse token/client/secret values.
- Treat sample record IDs as sandbox evidence, not reusable production metadata.
- For staged standalone related lists, export the reference list back after import and use that real remapped metadata for the dependent list.

The NHIC comparison proves the rule:

| List | Validation with `.ydl` metadata | Validation with full `.yap` metadata |
| --- | --- | --- |
| Portfolio Management | `pass_with_warnings`, unresolved lookup dependencies | `pass` |
| Partner Management | `pass_with_warnings`, unresolved lookup dependency | `pass` |
| Communication Records | `pass_with_warnings`, unresolved lookup/workflow dependencies | `pass_with_warnings`, only AI/search-field warnings remain |

### ID Policy For Generated .ydl Files

Current Yeeflow ID policy for this generation track:

- `AppID` default: `41` for the studied Yeeflow exports.
- `ListSetID` means the application/listset unique ID.
- `ListID` means the data-list unique ID.
- Generated IDs must be large numeric string IDs so JavaScript and JSON tooling preserve precision.

Generation modes:

| Mode | AppID | ListSetID | ListID | Rule |
| --- | --- | --- | --- | --- |
| Standalone testing | Use `41` unless the target app says otherwise. | Codex may generate a new large numeric string ID. | Codex may generate a new large numeric string ID. | Use only for sandbox/package testing, not production. |
| Existing-app sandbox | Use confirmed target app AppID, normally `41`. | Use confirmed real target ListSetID. | Generate a new package-local ListID only if the import strategy confirms this is safe; otherwise require metadata. | Validate against full `.yap` metadata when lookups/workflows exist. |
| Existing-list update | Use confirmed target app AppID. | Use confirmed target ListSetID. | Use confirmed target ListID. | Never guess. |
| Production | Use metadata-confirmed IDs only. | Use metadata-confirmed ListSetID. | Use confirmed ListID or confirmed import-generated-ID behavior. | No placeholders, no guessed IDs, sandbox round-trip required first. |

Generated standalone test IDs make a draft structurally testable, but they do not prove that the list is production-ready or safe to import into an existing application.

## 5. Data List Design Rules

### Field Type Selection

Choose the narrowest native field/control type that satisfies the requirement:

- short text: `Text` + `input`
- long notes/descriptions: `Text` + `textarea`
- fixed small option set: `Text` + `radio` or dropdown-style Rules
- multiple fixed options: `Text` + `checkbox`
- true relationship to another list: `Text` + `lookup`
- number: `Decimal` + `input_number`
- date-only: `Datetime` + `datepicker` configured as date
- date/time: `Datetime` + `datepicker` configured with time
- yes/no: `Bit` + `switch`
- URL: `Text` + `hyperlink`
- nested rows: `Text` + `list`
- workflow status button/field: `Text` + `flowstatus`

Use choices when values are stable and self-contained. Use lookup when values are shared, governed, reused across lists, reportable as master data, or must preserve relationship identity.

### Required Fields

Required behavior belongs in `Rules.required` or equivalent native validation metadata. Do not implement required fields with custom code or workflow logic.

### Default Values

Use native defaults when available. Default values should be explicit in the field Rules or form control attrs. Do not rely on sample data as defaults.

### Views

Generated lists should include at least one all-records view. Add filtered views only when the business workflow clearly needs them.

### Custom Forms

Use custom forms when the default list form is not sufficient for user experience, such as:

- grouped sections
- responsive grids
- explanatory headings
- embedded related data views
- simplified intake/edit forms

Do not create elaborate forms before the field model is stable.

Generated custom forms must follow the v5 baseline registration pattern:

- `Layout.Type = 1`
- `Layout.LayoutView = null`
- `Layout.Ext2 = "{\"src\":true}"`
- `Layout.IsItemPerm = false`
- `Layout.LayoutInResources[0].ID = Layout.LayoutID`
- `Layout.LayoutInResources[0].RefId = Layout.LayoutID`
- `Layout.LayoutInResources[0].Resource` is a JSON string containing the form JSON
- `Item.ListModel.LayoutView.add/edit/view` points to the custom form `LayoutID` when assigning the form to New/Edit/View

If `LayoutInResources[0].ID` or `RefId` differs from `LayoutID`, Yeeflow may import the form layout and assignment but drop the embedded form resource, resulting in an empty custom form designer after import.

### List Workflows

Use list workflows only for list-triggered automation, not normal data modeling.

Good candidates:

- sync a denormalized helper field
- enrich a record after create/update
- query a related list and update the current list
- trigger a controlled list-side update

Poor candidates:

- core field validation that native controls can handle
- approval routing
- security or permission logic
- critical financial calculations
- bulk persistence that belongs in an approval workflow or server-side process

### Sample Data

Sample data is optional. Include it only when the user asks for demo/test seed data or when a sandbox import needs examples.

The generated Visitor Access Management v11 app-level package confirms additional data-list field/sample patterns inside generated `.yap` child lists:

| Field type | FieldType / Type | Sample shape |
| --- | --- | --- |
| Text input | `Text` / `input` | string |
| Number | `Decimal` / `input_number` | numeric values such as `1`, `2`, `4` |
| Single select dropdown storage | `Text` / `radio` | selected option text |
| Switch/boolean | `Bit` / `switch` | string `"1"` or `"0"` |

When a data list is generated as the storage target for an approval form, field type compatibility must be checked against the approval form `ContentList` mappings:

- approval text variables -> `Text` fields
- approval number variables -> `Decimal/input_number` fields
- approval radio/dropdown text variables -> text/radio-compatible fields
- approval boolean switch variables -> `Bit/switch` fields

## 6. Lookup Rules

Every lookup requires:

- source list
- source field
- target `AppID`
- target `ListSetID`
- target `ListID`
- target display field such as `Title`
- `multiple` true/false
- search fields if configured
- filters if configured

Generation rules:

- Single lookup sample values should be target `ListDataID` strings.
- Example single lookup sample value: `"Text4": "2053735610870284294"`.
- Multi lookup sample values should be JSON-stringified arrays of target IDs.
- Search fields must exist in the target list metadata or be documented as aliases needing confirmation.
- Filters must reference target fields that exist.
- If target records are referenced in sample data, those record IDs must be available in supplied sample data or declared as sandbox-only examples.
- Full `.yap` app-context validation is required before final package generation if any lookup exists.

For standalone `.ydl` packages with lookups to an already-imported external list:

- require a dependency map
- require the real exported-back target `ListSetID`, `ListID`, and display field
- require real target reference/sample record `ListDataID` values if dependent sample data should include lookup values
- keep local generated IDs in `Resource.ReplaceIds`
- exclude external resolved dependency IDs from `Resource.ReplaceIds`, including target record `ListDataID` values
- validate the wrapper to confirm external lookup sample values are not in `ReplaceIds`

If external target record IDs are included in the dependent package `Resource.ReplaceIds`, Yeeflow remaps them during standalone import. The sample rows then point to non-existent target records and display `(Deleted)`.

Stop if:

- the target list is unknown
- the target display field is unknown
- `multiple` behavior is unclear
- lookup filters reference unknown fields
- sample lookup IDs cannot be resolved and sample data is intended for import

## 7. View Generation Rules

Recommended baseline views:

- `All <List Name>`: all records, sensible columns, default view
- `Active <List Name>`: when an Active/Status field exists
- `Open <List Name>`: when a status workflow includes open/closed states
- category-specific views: only when the requirement names important segments

Column rules:

- Put title/name first.
- Put status, owner, date, and key lookup fields near the front.
- Hide low-value system fields unless the target users need them.
- Keep view filters simple and native.

Known view types:

- empty or `0`: normal list view
- `104`: observed board/pipeline-style view; treat as learned but not fully proven for generation

Generator stop conditions:

- requested view type is unknown
- filter operator is unknown
- filter/sort fields do not exist
- board/pipeline grouping field is unclear

## 8. Custom Form Rules

Use custom forms when they improve data entry or review.

Safe default custom form structure:

- top-level container
- header text/title
- short description/help text
- section containers
- grid/flex layout
- bound field controls
- optional embedded data-list control

Critical registration rules from the Asset Inventory v5 baseline:

- custom form layouts use `Type: 1`
- embedded custom form layouts use `LayoutView: null`
- `LayoutInResources` must contain a resource entry
- `LayoutInResources[0].ID` must equal the custom form `LayoutID`
- `LayoutInResources[0].RefId` must equal the custom form `LayoutID`
- `LayoutInResources[0].Resource` must be a JSON string
- `Ext2` must be the JSON string `{"src":true}`
- `IsItemPerm` should be `false`
- `Item.ListModel.LayoutView` stores New/Edit/View assignment
- form Resource JSON must include `children`, `attrs`, `title`, `filterVars`, `ver`, and `tempVars`

Binding rules:

- every input control must bind to an existing list field
- layout/text controls may be unbound
- lookup controls must match lookup field metadata and target list metadata
- nested list controls must match the list field Rules
- controls should use UUID-style IDs
- bound controls should use list storage `FieldName` values such as `Title`, `Text1`, `Datetime1`, or `Bit1`
- bound controls should include the matching `fieldID`

Differences from approval forms:

- data-list form controls bind directly to storage fields such as `Title`, `Text2`, `Datetime1`
- approval forms bind to workflow variables and pageurls
- data-list custom forms do not use approval `workflowControlPanel` or approval `workflowHistory` patterns unless a list-specific control is confirmed

Custom code should be avoided unless the requirement is client-side behavior that cannot be handled natively. Any custom code control is a manual review dependency.

## 9. List Workflow Rules

List workflows are appropriate when automation is triggered by changes to list records.

Observed useful patterns:

- current-list update with `ContentList`
- related-list query with `QueryData`
- AI enrichment followed by current-list update
- sync helper IDs or denormalized reference fields

Rules:

- `ContentList` current-list updates must reference local fields.
- `ContentList` target-list updates must resolve against full app metadata.
- `QueryData` target lists must resolve against full app metadata.
- AI nodes must be declared as dependencies and reviewed before generation.
- HTTP/API nodes must be treated as external dependencies and reviewed for credentials/secrets.
- Workflow expression field references must resolve where parseable.

Stop if:

- target list is missing
- mapped target field is missing
- query target list is missing
- AI agent/runtime dependency is unresolved
- external HTTP/API credentials or endpoints are involved
- field expressions cannot be understood well enough for generator mode

## 10. Sample Data Rules

Sample data should be optional and sandbox-scoped.

Use sample data when:

- the user asks for demo rows
- sandbox import testing needs realistic data
- value-shape examples help validate lookup or multi-choice configuration

Rules:

- Include `ListDataID` only when generating a full sample import format and the ID strategy is confirmed.
- For lookup values, use real target sample IDs only in sandbox contexts.
- For related-list sample data, import the reference list first, export it back, then use real target `ListDataID` strings in dependent sample rows.
- Exclude external target record IDs from the dependent package `Resource.ReplaceIds`.
- For multi-choice and multi-lookup values, use JSON-stringified arrays.
- Use date strings like `YYYY-MM-DD`.
- Use datetime strings like `YYYY-MM-DD HH:mm:ss` if Yeeflow expects that format.
- Do not include user/file references unless real sandbox-safe IDs/files are provided.
- Do not include secrets, tokens, client IDs, credentials, or live private data.

Stop if:

- sample lookup record IDs are unresolved
- user/file values are required but not mapped
- sample data includes sensitive or customer-confidential values

## 11. Validation Gates

### `inspect-ydl-package.js`

Purpose: decode `.ydl` and produce a detailed inventory.

Use when:

- studying a real export
- debugging package anatomy
- extracting exact view/form/workflow/sample patterns

Outputs:

- inspection JSON
- inspection Markdown

### `extract-ydl-metadata.js`

Purpose: produce clean machine-readable metadata from one or more `.ydl` files.

Use when:

- building a local relationship model
- validating generated drafts against known list structures
- creating a data-list generator reference set

### `validate-ydl-list.js`

Purpose: validate a `.ydl` wrapper or decoded data-list draft structurally.

Modes:

- `compatibility`: tolerant for real exports
- `generator`: strict for generated drafts

Use before any wrapper build or sandbox import.

For related standalone list packages, pass a dependency map. The validator allows non-empty external lookup sample values only when the dependency map resolves the target records, and it fails wrappers when those external target IDs appear in `Resource.ReplaceIds`.

### `validate-ydl-against-yap.js`

Purpose: validate a `.ydl` package or decoded draft against app metadata.

Use with full `.yap` metadata whenever lookups or workflows exist.

### `build-ydl-wrapper.js`

Purpose: create a `.ydl` wrapper from a final validated decoded list package.

Must not build if:

- placeholders remain
- structural validation fails
- app-context validation fails
- required dependencies are unresolved

The builder is now available and has been used successfully for the Asset Inventory v5 baseline.

When a dependency map is supplied, the builder excludes resolved external dependency IDs from `Resource.ReplaceIds`. This is required for standalone dependent lists whose sample rows reference already-imported lookup target records.

### Future `compare-ydl-yap-list.js`

Purpose: compare a standalone `.ydl` with the same list inside a `.yap` app export or export-back package.

Use after sandbox import/export round-trip to learn remapping behavior and drift.

## 12. Stop Conditions

Codex must stop before final `.ydl` or `.yap` generation when:

- lookup target list is missing
- lookup target display field is unknown
- lookup search/filter fields are unresolved
- field type or control type is uncertain
- field internal/storage name strategy is unclear
- custom form binding is unresolved
- nested list metadata is incomplete
- workflow references missing fields, lists, actions, agents, or APIs
- AI or external dependencies are unresolved
- sample data references unresolved records, users, files, or lookup rows
- external lookup sample values appear in `Resource.ReplaceIds`
- placeholder values remain
- validators fail in generator mode
- sensitive credential-like resources are involved

## 13. Readiness Levels

| Level | Meaning | Allowed output |
| --- | --- | --- |
| Research-ready | Real exports decoded and understood. | Reports and pattern notes only. |
| Draft-ready | Business requirement decomposed and normalized spec proposed. | Normalized spec and decoded draft with placeholders. |
| Validation-ready | Decoded draft exists and can run structural validation. | Validation reports; no wrapper unless dependencies pass. |
| Wrapper-ready | Structural and app-context validation pass with no unresolved placeholders. | `.ydl` wrapper can be built when builder exists. |
| Sandbox-import-ready | Wrapper built and round-trip validation passes. | Manual sandbox import by user/operator. |
| Production-candidate | Sandbox import, runtime test, and export-back comparison are clean. | Production planning package; still requires approval. |

## 14. Example: Asset Inventory

The first generated Asset Inventory baseline reached sandbox-import-ready after v5.

Key milestone:

- v2 imported but custom form registration was incomplete.
- v3 registered and assigned the form but did not reliably load content.
- v4 preserved assignment, but export-back showed `LayoutInResources: []` because `ID`/`RefId` did not match `LayoutID`.
- v5 set `LayoutInResources[0].ID` and `RefId` equal to the custom form `LayoutID`, preserving the embedded custom form content.

Use `docs/collection-02.md#generated-data-list-baseline-asset-inventory-v5` as the source of truth for generated custom-form registration.

### Decomposition

Business purpose:

Track company assets such as laptops, monitors, phones, accessories, and assigned owners.

Primary users:

- IT administrators
- operations staff
- department managers

Expected outcome:

A searchable inventory list with owner assignment, category/status views, and optional lifecycle workflow later.

### Field Model

| Field | Type | Notes |
| --- | --- | --- |
| Asset Name | text/input | Required title field. |
| Asset Tag | text/input | Unique if Yeeflow uniqueness behavior is confirmed. |
| Category | choice/radio or dropdown | Laptop, Monitor, Phone, Accessory, Other. |
| Serial Number | text/input | Optional. |
| Manufacturer | text/input or lookup | Lookup only if a manufacturer master list exists. |
| Model | text/input | Optional. |
| Purchase Date | date/datepicker | Optional. |
| Warranty Expiry | date/datepicker | Optional. |
| Assigned To | lookup or user | Use user field if Yeeflow user field pattern is confirmed; otherwise lookup to Employee master list if available. |
| Department | lookup or text | Lookup if Department list exists. |
| Status | choice/radio | Available, Assigned, In Repair, Retired. |
| Location | text/input or lookup | Lookup only if Location list exists. |
| Notes | longText/textarea | Optional. |
| Active | boolean/switch | Default true. |

### Lookup Needs

Potential lookups:

- Employee / Contact list for Assigned To
- Department list
- Location list
- Manufacturer list

If these target lists do not exist in full `.yap` metadata, generate the list in draft mode with placeholders or use text fields instead.

### Views

Recommended views:

- All Assets: default all-records view
- Available Assets: filter Status = Available
- Assigned Assets: filter Status = Assigned
- In Repair: filter Status = In Repair
- Retired Assets: filter Status = Retired or Active = false
- By Category: only if board/grouping view Type is confirmed

### Custom Forms

Recommended custom form:

- header: Asset Inventory
- section: Asset Details
- section: Assignment
- section: Purchase and Warranty
- section: Status and Notes

Use bound field controls only. Use a grid for compact operational entry.

### Workflow Decision

No workflow is needed for the first version.

Possible future workflow:

- When Status changes to Retired, update Active = false.
- When Warranty Expiry is approaching, trigger reminder.
- When Assigned To changes, write assignment history.

These should remain out of v1 unless explicitly requested.

### Sample Data Decision

Optional sandbox sample data:

- 5 to 10 example assets
- no real employee names unless provided
- no file attachments
- lookup values only if target sample records are available

### Readiness Assessment

Without target app metadata:

- readiness: Draft-ready
- produce normalized spec and decoded draft with placeholders
- do not build `.ydl`

With full `.yap` metadata and resolved lookup targets:

- readiness: Validation-ready
- run `validate-ydl-list.js --mode generator`
- run `validate-ydl-against-yap.js --mode generator`
- build wrapper only after both pass and no unresolved dependencies remain

## 15. Next Roadmap

### Harden `build-ydl-wrapper.js`

Purpose:

Continue hardening the existing `.ydl` wrapper builder from the Asset Inventory baseline.

Inputs:

- decoded `Resource.Data` JSON
- wrapper metadata such as title, description, icon, app/listset assumptions

Outputs:

- `.ydl` wrapper
- round-trip validation report

Why it matters:

The builder completes the package-generation path, but should continue to learn from import/export round trips.

### `compare-ydl-yap-list.js`

Purpose:

Compare a standalone `.ydl` list with the corresponding list inside a full `.yap` export.

Inputs:

- `.ydl` export
- `.yap` export or extracted `.yap` metadata
- target ListID or list name

Outputs:

- schema differences
- view/form/workflow differences
- dependency differences
- import/export remapping findings

Why it matters:

It helps prove whether generated standalone lists behave the same after app-level import/export.

### `generate-data-list-dependency-map.js`

Purpose:

Generate a dependency mapping file from a normalized data-list spec and app metadata.

Inputs:

- normalized data-list spec
- `.yap` metadata

Outputs:

- lookup dependencies
- app/list/listset dependencies
- field mapping dependencies
- sample data dependencies
- workflow dependencies

Why it matters:

It makes placeholder resolution explicit and prevents hidden guesses.

### `yeeflow-data-list-generator` Skill

Purpose:

Package this playbook, tools, templates, and validated examples into a reusable Codex/ChatGPT skill.

Include:

- requirement decomposition template
- normalized data-list spec
- native-first design rules
- lookup and workflow dependency rules
- validators
- examples from NHIC and future generated drafts

When to create:

After one or two generated data-list drafts have passed structural and app-context validation, and after wrapper build behavior is proven in sandbox.

## App-Level Field Type Evidence

The manual Visitor Access Management v10 `.yap` export first confirmed additional data-list field patterns inside an app-level package. The generated Visitor Access Management v11 package then proved these patterns in a generated import/runtime test. See `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`.

Confirmed data-list patterns:

| Field intent | FieldName slot | FieldType | Type | Rules/value notes |
| --- | --- | --- | --- | --- |
| Number | `Decimal1`, `Decimal2`, ... | `Decimal` | `input_number` | format/min settings in `Rules`; empty sample may be `""` |
| Boolean switch | `Bit1`, `Bit2`, ... | `Bit` | `switch` | sample false uses `"0"`; true uses `"1"` in prior list exports |

v11 proven storage slots:

| Business field | Slot | Pattern |
| --- | --- | --- |
| Visitor Email | `Text13` | text/input |
| Visitor Phone | `Text14` | text/input |
| Number of Visitors | `Decimal1` | Decimal/input_number |
| Access Type | `Text15` | Text/radio dropdown-compatible storage |
| Requires Escort | `Bit1` | Bit/switch |

Generation rules:

- Use `Decimal*` fields for numeric data that should be persisted as numbers.
- Use `Bit*` fields for boolean/switch data.
- Keep sample values type-shaped:
  - number: numeric value or empty string when intentionally blank
  - switch: `"1"` for true, `"0"` for false
- If an approval form writes to these fields, validate `ContentList` mappings for type compatibility:
  - number variable to `Decimal/input_number`
  - boolean variable to `Bit/switch`
- Add these fields to custom list forms and views deliberately; the manual export added the fields to views but did not add them to the custom list form.

---

<a id="validation-guide"></a>

<!-- source: skills/yeeflow-data-list-generator/references/collection-01.md#validation-guide -->

# Validation Guide

Use local tools only. Do not import or operate Yeeflow UI.

## Structural Validation

Validate real exports in compatibility mode:

```bash
node <plugin-root>/skills/yeeflow-application-generator/scripts/validate-ydl-list.js "./Real Export.ydl" --mode compatibility
```

Validate generated drafts:

```bash
node <plugin-root>/skills/yeeflow-application-generator/scripts/validate-ydl-list.js ./generated.decoded.json --mode generator --stage draft
```

Validate final decoded candidates:

```bash
node <plugin-root>/skills/yeeflow-application-generator/scripts/validate-ydl-list.js ./generated.final.json --mode generator --stage final
```

With dependencies:

```bash
node <plugin-root>/skills/yeeflow-application-generator/scripts/validate-ydl-list.js ./generated.final.json --mode generator --stage final --dependency-map ./dependencies.json
```

Final mode must have no placeholders and no unresolved dependencies.

## App Context Validation

Use when lookups, workflows, or app resources exist:

```bash
node scripts/validate-ydl-against-yap.js ./list.ydl ./app-metadata.json --mode generator
```

Full `.yap` metadata gives better confidence than isolated `.ydl` metadata.

## Wrapper Build

Build only after final validation passes:

```bash
node <plugin-root>/scripts/build-ydl-wrapper.js \
  ./generated.final.json \
  ./generated.ydl \
  --title "List Name" \
  --description "Sandbox generated data list" \
  --dependency-map ./dependencies.json
```

The builder round-trips:

- wrapper JSON
- Resource prefix
- base64
- gzip
- Resource JSON
- `Resource.Data`
- source equality
- no placeholders
- final validation

## Important Checks

`validate-ydl-list.js` checks:

- `Item`, `ListModel`, `Defs`, `Layouts`, `ListDatas`
- field IDs/names/types/Rules
- views and displayed columns
- custom forms and bound controls
- list workflows and target lists
- sample data value shapes
- lookup relationships
- placeholders
- unsafe external lookup sample values in `ReplaceIds`

`build-ydl-wrapper.js` refuses to write when validation fails. With a dependency map, it excludes resolved external dependency IDs from `Resource.ReplaceIds`.

For generated lists that will become approval-form storage targets, wrapper validation is not the final handoff. The user must import the `.ydl`, export the list back, and use exported-back list/field metadata to patch the approval form `ContentList` target.

---

<a id="ydl-structure-study"></a>

<!-- source: skills/yeeflow-data-list-generator/references/collection-01.md#ydl-structure-study -->

# .ydl Structure Study

## Wrapper Format

Standalone Yeeflow data-list exports are JSON wrappers:

```json
{
  "Title": "List Name",
  "Description": "...",
  "IconUrl": null,
  "IsListSet": false,
  "Resource": "[______gizp______]<base64-gzip-resource-json>"
}
```

Decode `Resource` by stripping `[______gizp______]`, base64-decoding, then gunzipping. The result is Resource JSON. `Resource.Data` is a JSON string containing the decoded data-list package.

## Resource JSON

Important Resource fields:

- `MainListType`
- `AppID`
- `ReplaceIds`
- `ReportIds`
- `FormKeys`
- `Data`
- `SimplePortal`

`ReplaceIds` controls import-time remapping. Local generated IDs belong there; external resolved dependency IDs do not.

## Decoded Resource.Data

The decoded data package contains:

- `Item`: main list resource
- `Childs`: child resources if included
- `Forms`: list workflows
- `FormReports`
- `DataReports`
- `FormNewReports`
- `AppGroups`
- `AppTags`
- `AppMetadatas`
- `AppThemes`
- `AppComponents`
- `PortalInfo`
- `OtherModules`

## Item Structure

`Item` contains:

- `ListModel`: identity, app/list metadata, display settings
- `Defs`: field definitions
- `Layouts`: views and custom forms
- `PublicForms`
- `RemindRules`
- `FlowMappings`
- `ListDatas`: sample records keyed by `ListDataID`

## Field Definitions

Fields live in `Item.Defs[]`.

Common keys:

- `FieldID`
- `ListID`
- `FieldName`
- `InternalName`
- `DisplayName`
- `FieldType`
- `Type`
- `DefaultValue`
- `Rules`: JSON string
- `IsSystem`, `IsSort`, `IsIndex`, `IsFilter`

Observed type mappings:

| Normalized | FieldType / Type |
| --- | --- |
| text | `Text` / `input` |
| longText | `Text` / `textarea` |
| choice | `Text` / `radio` or dropdown-style Rules |
| multiChoice | `Text` / `checkbox` |
| lookup | `Text` / `lookup` |
| date | `Datetime` / `datepicker` |
| datetime | `Datetime` / `datepicker` with time |
| boolean | `Bit` / `switch` |
| hyperlink | `Text` / `hyperlink` |
| list | `Text` / `list` |
| flowstatus | `Text` / `flowstatus` |

## Layouts

`Item.Layouts[]` stores both views and custom forms.

- Views usually use `Type: 0` or empty.
- Custom forms use `Type: 1`.
- Board/pipeline-style view `Type: 104` was observed but is not fully proven for generation.

View `LayoutView` is a JSON string containing displayed columns, filters/query, sort, and rowColor.

Custom form `LayoutView` should be `null`; the form body is stored in `LayoutInResources[0].Resource`.

## Sample Data

Sample records are stored in `Item.ListDatas`.

Observed value shapes:

- single lookup: plain target `ListDataID` string
- multi lookup: JSON-stringified array of target IDs
- checkbox/multi-choice: JSON-stringified array
- date: `YYYY-MM-DD`
- datetime: `YYYY-MM-DD HH:mm:ss`
- empty values: usually `""`

Sample data is sandbox/test evidence, not production metadata.

## List Workflows

List workflows are stored in `Data.Forms[]`.

Observed workflow nodes include `StartNoneEvent`, `SequenceFlow`, `ContentList`, `QueryData`, `AI`, and `EndNoneEvent`.

Validate list workflow dependencies against full app metadata before wrapper build.
