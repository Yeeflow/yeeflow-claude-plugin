# docs/studies collection 2

This file combines 17 documents from `docs/studies/` for the Claude directory build. A reference to `docs/studies/<name>.md` is the section that starts at the anchor `#<name>` below.

- [doc-library-control-runtime-test-plan.md](#doc-library-control-runtime-test-plan)
- [document-library-runtime-test-plan.md](#document-library-runtime-test-plan)
- [form-report-resource.md](#form-report-resource)
- [kanban-collection-dynamic-controls.md](#kanban-collection-dynamic-controls)
- [new-capability-runtime-gate-process.md](#new-capability-runtime-gate-process)
- [pivot-table-control-runtime-proof.md](#pivot-table-control-runtime-proof)
- [pivot-table-control.md](#pivot-table-control)
- [scheduled-workflow-generation-rules.md](#scheduled-workflow-generation-rules)
- [scheduled-workflow-resource-structure.md](#scheduled-workflow-resource-structure)
- [sub-list-dynamic-content.md](#sub-list-dynamic-content)
- [three-column-dashboard-layout-runtime-css-study.md](#three-column-dashboard-layout-runtime-css-study)
- [timeline-controls-dynamic-controls.md](#timeline-controls-dynamic-controls)
- [workflow-ai-assistant-image-input.md](#workflow-ai-assistant-image-input)
- [workflow-approval-vs-data-list-actions.md](#workflow-approval-vs-data-list-actions)
- [workflow-assignment-task-assignee-settings.md](#workflow-assignment-task-assignee-settings)
- [workflow-assignment-task-complete-task-and-due-date.md](#workflow-assignment-task-complete-task-and-due-date)
- [workflow-assignment-task-generation-rules.md](#workflow-assignment-task-generation-rules)

---

<a id="doc-library-control-runtime-test-plan"></a>

<!-- source: docs/studies/collection-02.md#doc-library-control-runtime-test-plan -->

# Doc Library Control Runtime Test Plan

Generated package:

`enterprise-document-center-doc-library-control-v3.yap`

Generated app:

`Enterprise Document Center - Doc Library Control`

## Local Gates

Before Yeeflow import:

- JS syntax checks for changed `.js` and `.mjs` files.
- JSON parse checks for changed `.json` files.
- `validate-yap-package.js` on the generated package.
- `validate-yap-graph.js` on the generated package.
- standalone `validate-ydl-list.js` checks for each generated Type `16` document library.
- build wrapper round trip through `build-yap-wrapper.js`.
- `git diff --check`.
- safety scan for raw exports, decoded payloads, generated `.yap` tracking, document binaries, `.env`, credentials, and token-like values.

## Runtime Test Steps

Import into `https://<yourdomain>.yeeflow.com/` only after local validation passes.

App-level checks:

- Package imports successfully.
- App opens correctly and is not an empty shell.
- All three Type `16` document libraries appear.
- Generated root folders remain visible.
- Dashboard `Document Center` appears.
- Dashboard opens without designer placeholder or blank content.

Dashboard Doc library control checks:

- `Company Policies Root` renders the Company Policies library root.
- `HR Policies Folder` renders the Company Policies / HR Policies folder context.
- `Project Contracts Folder` renders Project Documents / Contracts folder context.
- `Templates Root` renders the Templates and Forms library root.
- Caption/title area appears where configured.
- Search box appears where configured.
- Add button appears where configured and uses the target library `New file` form.
- Refresh the page and confirm controls still render.
- Navigate folders inside a control if the control UI exposes folder navigation.

Do not test real upload unless using a harmless dummy file and explicitly record the result. Do not claim upload persistence unless it is actually tested.

## Expected Proof Level

This v3 package is intended to prove dashboard Doc library controls for:

- root library display
- static root-folder display
- caption/search/add true settings
- field-column display through `listarr`

It is not intended to prove:

- dynamic expression folder paths
- add/search/caption disabled states
- approval form context
- data-list form context
- document-library form context
- actual upload behavior

## Runtime Result

Runtime-tested in `https://<yourdomain>.yeeflow.com/` on 2026-05-18.

Status: runtime-proven for dashboard root-bound and static root-folder-bound Doc library controls.

Passed checks:

- Package imported successfully.
- App opened correctly and was not an empty shell.
- All three document libraries appeared in navigation.
- Generated root folders appeared in all three document libraries:
  - Company Policies: `HR Policies`, `IT Policies`, `Finance Policies`, `Compliance Policies`
  - Project Documents: `Requirements`, `Contracts`, `Meeting Notes`, `Deliverables`
  - Templates and Forms: `HR Forms`, `Finance Forms`, `Project Templates`, `Legal Templates`
- Dashboard `Document Center` appeared and opened.
- All four generated dashboard controls rendered:
  - `Company Policies Root`
  - `HR Policies Folder`
  - `Project Contracts Folder`
  - `Templates Root`
- Root-bound controls showed the expected library-root folders.
- Static folder-bound controls showed the expected folder context.
- Caption/title, search box, and add button were visible where configured.
- Add button opened the target document-library action menu; choosing `New File` opened the target `New file` modal. No file was uploaded.
- Dashboard controls still rendered after page refresh and after navigation through document-library pages.

Deferred checks:

- Actual upload behavior and upload persistence.
- Dynamic expression-based folder binding in a generated package.
- Disabled caption/search/add settings.
- Approval form, data-list form, and document-library form host contexts.

---

<a id="document-library-runtime-test-plan"></a>

<!-- source: docs/studies/collection-02.md#document-library-runtime-test-plan -->

# Yeeflow Document Library Runtime Test Plan

Target environment: `https://<yourdomain>.yeeflow.com/`

Run this only after local validation passes.

## Import And Open

1. Import the generated `.yap`.
2. Confirm the imported app name matches the generated baseline.
3. Open the app.
4. Confirm the app does not open as an empty shell or `Start to build with Components`.
5. Confirm the document library appears in app navigation.
6. Open the document library.

## Library Structure

Confirm:

- library opens without query errors.
- default view opens.
- default document fields are present:
  - Upload File / `Text4`
  - Name / `Title`
  - Type / `Text1`
  - Size/FileSize / `Bigint2`
- support fields are present or safely hidden:
  - ParentID / `Bigint1`
  - Extension / `Text2`
  - UniqueName / `Text3`

## Upload Behavior

Use only a safe disposable test file with no private content.

Test:

- upload control opens.
- upload succeeds.
- Name is populated with the file name or expected display value.
- Type and/or Extension populate as expected.
- Size/FileSize populates as expected.
- record persists after refresh.
- list query still works after upload.

## Forms

If custom forms are included:

- New form opens.
- Edit form opens.
- View form opens.
- Upload File control renders on all included forms where expected.
- form assignment follows `ListModel.LayoutView.add/edit/view`.

## Folder Behavior

Test only if generated and safe.

For export-backed generated root folders:

- confirm every generated folder row is visible in the default library view.
- refresh the page and confirm generated folders persist.
- switch generated views and document whether folders remain visible or are hidden by view filters.
- open representative generated folders and confirm the folder page/list loads.

For manual folder creation:

- create a folder.
- open the folder.
- refresh and confirm the manually created folder persists.
- confirm `ParentID` behavior through export-back or safe metadata inspection when available.

Upload a file into the folder only if a harmless disposable test file is explicitly safe to use.

## Result Classification

Use these labels:

- runtime-proven
- partially proven
- validation-only
- blocked by package/materialization
- blocked by Yeeflow runtime context
- not tested

Validation-only success must not be reported as runtime proof.

## First Runtime Attempt

Baseline package: `document-library-runtime-baseline.v1.yap`

Result: blocked by Yeeflow runtime context / import create failure.

Observed evidence:

- local validation passed before import.
- Yeeflow accepted the `.yap` upload and showed the package name and description.
- clicking OK on the import metadata step returned `Tip: Created failed`.
- the app was not created, so app open, library open, upload, folder, and persistence checks were not reached.

Do not patch this baseline until the failing import response or an exported-back Type `16` minimal package identifies the missing or invalid property. Current status is validation-only, not runtime-proven.

## Sample Isolation Attempt

Source studied: `<downloads>/Document Library Sample.yap`

What the sample proved:

- document-library-only app exports can have `Item.Layouts = []`.
- root `LayoutView` can be only `{"sortVer":1}` with no navigation `sort[]`.
- top-level `Resource.SimplePortal` is `null` in both known-good exports.
- the minimal document library has seven native fields, one empty Type `0` view, and one `New file` upload form with no `ListModel.LayoutView` assignment.

Baseline v2 changes:

- generated from `Document Library Sample.yap` rather than `Projects Center.yap`.
- removed generated root Type `103` Home page and root navigation.
- removed partial New-only custom-form assignment.
- kept one Type `16` library with no uploaded rows.

Runtime result: still blocked by Yeeflow runtime context / import create failure. Yeeflow accepted the upload and metadata dialog, then returned `Tip: Created failed`.

Baseline v3 changes:

- changed top-level `Resource.SimplePortal` from `[]` to `null`.
- switched to a fresh Yeeflow-like `2059...` ID family.
- local package, graph, materialization, wrapper round-trip, and standalone document-library validation pass with warnings only.

Runtime status for v3: validation-only. A clean v3 import attempt still needs to be repeated with reliable browser/network evidence before claiming runtime proof.

## Runtime-Proven Minimal Baseline

Package: `document-library-sample-new-library-only.v1.yap`

Source shape: `New Document Library` from `<downloads>/Document Library Sample.yap`

Result: runtime-proven for import/open by user manual test.

Generated baseline alignment: `document-library-runtime-baseline.v4.yap` uses this runtime-passed `New Document Library` base definition with fresh IDs. The generated `.yap` runtime artifact remains ignored/uncommitted; the committed source is the generator and study guidance.

What is proven:

- A document-library-only app can contain a single Type `16` child resource cloned from `New Document Library`.
- The base document-library definition is the `New Document Library` shape, not the earlier generated `Baseline Documents` experiment.
- The seven native fields are sufficient for import/open when paired with the sample's minimal library view/form structure.
- The default Type `0` view should preserve the sample's empty string `LayoutView = ""`; changing it to `null` is not the canonical base shape.
- The `New file` Type `1` upload form can remain unassigned in `ListModel.LayoutView` for a newly-created-library baseline.
- Root app `Item.Layouts = []`, root `LayoutView = {"sortVer":1}`, and top-level `Resource.SimplePortal = null` are valid for this minimal document-library-only package.

What remains unproven:

- upload persistence behavior.
- folder create/open behavior.
- custom New/Edit/View form assignment for generated libraries.
- configured views with additional custom fields in a generated package.

## V2 Enterprise Document Center Runtime Pass

Package: `enterprise-document-center-v2.yap`

Generated app: `Enterprise Document Center`

Result: partial runtime proof.

What is proven:

- A generated app can import/open with three Type `16` document libraries cloned from the canonical `New Document Library` base.
- Navigation can expose all three generated libraries.
- Each generated library opens as a document library, not as an empty app shell.
- Simple custom field definitions can be included in Type `16` libraries.
- Multiple configured Type `0` views can be included in generated Type `16` libraries.
- The document-library Add menu exposes New Folder, New File, and Batch Upload in the generated libraries.

What is partially proven:

- Manual folder creation: `Templates and Forms` created `HR Forms` and displayed the row; `Company Policies` accepted `HR Policies` with an `Added Successfully` toast but did not visibly refresh the row during the spot-check.

What remains unproven:

- generated folder-row import shape.
- folder persistence after refresh across all libraries.
- actual upload persistence with a harmless test file.
- richer assigned New/Edit/View form mapping beyond the unassigned `New file` base form.

## V2 Generated Folder Runtime Pass

Package: `enterprise-document-center-v2-folders.yap`

Generated app: `Enterprise Document Center`; imported as `Enterprise Document Center Folders Runtime` to avoid duplicate app-name conflict.

Result: partial runtime proof, with generated root folders runtime-proven.

What is proven:

- The generated app imports/opens with three Type `16` document libraries and generated root folder rows.
- Generated folders appear in all three document libraries:
  - Company Policies: `HR Policies`, `IT Policies`, `Finance Policies`, `Compliance Policies`.
  - Project Documents: `Requirements`, `Contracts`, `Meeting Notes`, `Deliverables`.
  - Templates and Forms: `HR Forms`, `Finance Forms`, `Project Templates`, `Legal Templates`.
- Generated folders persist after refresh/navigation checks.
- Manual `New Folder` creation works across all three libraries:
  - `Runtime Check Policies`
  - `Runtime Check Project Docs`
  - `Runtime Check Templates`
- Company Policies may require a page refresh after the success toast before the newly created manual folder appears; after refresh, the folder persisted.
- The Add menu still exposes `New Folder`, `New File`, and `Batch Upload`.

What remains unproven:

- nested generated folder rows.
- exhaustive open/navigation behavior for every generated folder.
- actual upload persistence with a harmless test file.
- richer assigned New/Edit/View form mapping beyond the unassigned `New file` base form.

---

<a id="form-report-resource"></a>

<!-- source: docs/studies/collection-02.md#form-report-resource -->

# Yeeflow Form Report Resource Study

Proof boundary: export-proven and validator-backed only. This pass studied `/Users/Renger/Downloads/AI Training-2 (1).yap` read-only, generated redacted normalized references under `docs/studies/normalized/form-report.bundle.json`, and did not import, open, execute, mutate, or runtime-test the package.

## Summary

The export proves Form Report is an application-level resource represented in two linked places:

- `Data.FormNewReports[]` stores the report configuration.
- `Data.Childs[]` stores a matching child resource where `ListModel.Type = 32` and `ListModel.ListID = FormNewReports[].ID`.

The export contains one approval form and four Form Reports. All four Form Reports reference the same approval form through `Data.FormNewReports[].DefKey`, which resolves to `Data.Forms[0].Key`. This proves one approval form can have multiple Form Reports in the export schema.

The export does not prove item mutation behavior, Excel export execution, row click runtime behavior, report row multiplication at runtime, or Form Report usage as a data source by lookup/data-table/dashboard/chart controls. Those remain product-understanding-backed or unproven until a focused runtime baseline is run.

## Inventory

| Report | Report resource | Source approval form | Selected variables | Selected sub-list | Sub-list fields | Filter | Views | Detail access | Export setting | Proof |
| --- | --- | --- | ---: | --- | ---: | --- | ---: | --- | --- | --- |
| `<form-report-1>` | `<report-id-1>` | `<approval-form-key-1>` | 38 | none | 0 | none | 3 | enabled in default/custom views | enabled | export-proven |
| `<form-report-2>` | `<report-id-2>` | `<approval-form-key-1>` | 38 | none | 0 | two conditions | 1 | disabled | enabled | export-proven |
| `<form-report-3>` | `<report-id-3>` | `<approval-form-key-1>` | 38 | `vlist_field_37` | 3 | none | 1 | enabled | disabled | export-proven |
| `<form-report-4>` | `<report-id-4>` | `<approval-form-key-1>` | 6 | `vlist_field_38` | 6 | none | 1 | enabled in view, disabled at resource attr | enabled | export-proven |

Names, labels, tenant IDs, user IDs, org data, report titles, list titles, form titles, and category values are redacted in committed study artifacts.

## Export Paths

| Concept | Export path | Notes | Proof |
| --- | --- | --- | --- |
| App resource/nav entry | `Data.Item.ListModel.LayoutView.sort[]` | Type `32` entries point to report child resources. One of four reports is present as a child resource but not visible in the observed root nav sort array. | export-proven |
| Form Report config | `Data.FormNewReports[]` | Keys observed: `ID`, `DefKey`, `Name`, `Description`, `Attr`, `Settings`. | export-proven |
| Child resource | `Data.Childs[]` | Report resources have `ListModel.Type = 32`, `Perm = 4`, and fields/views like list-like resources. | export-proven |
| Source approval form | `Data.FormNewReports[].DefKey` -> `Data.Forms[].Key` | All reports reference the same approval form key. | export-proven |
| Field config | `Data.FormNewReports[].Settings.Fields[]` | `Settings` is JSON string with `Fields`, `Filters`, and `SubListID`. | export-proven |
| Field display names | `Settings.Fields[].Name`, `Label`, `PropName` | Values are redacted; paths are stable. | export-proven |
| Field type | `Settings.Fields[].L_Type` with `Type` | `L_Type` is the report display/control field type; `Type` preserves source-ish variable category. | export-proven |
| Variable reference | `Settings.Fields[].ID`, `Key` | Main variables use keys like `v_<variableId>`. List variables use `vlist_<variableId>`. | export-proven |
| Sub-list reference | `Settings.SubListID` | Empty string means no selected sub-list; one string selects one list variable. | export-proven |
| Sub-list fields | `Settings.Fields[].Key = <SubListID>_<subFieldId>` | The field `ID` remains the parent list variable id; `Key` identifies the sub-list field. | export-proven |
| Filter condition | `Settings.Filters` | Null/absent means no filter in this export; filtered sample uses an array of condition objects. | export-proven |
| Export/detail flags | `Data.FormNewReports[].Attr` and child `ListModel.LayoutView` | `isViewDetail`, `isExport`, and `Attr_IsViewDetail` were observed. | export-proven |
| Views | `Data.Childs[].Layouts[]` | Type `0` list views are used. Some LayoutView payloads include `layout`, `sort`, `query`, `rowColor`, and `filter`. | export-proven |
| Permissions | `Data.Childs[].ListModel.IsBreakInherit`, `Perm` | This export only proves inherited permission shape (`IsBreakInherit = false`). Custom permission group shape was not found. | export-proven gap |

## Incremental MCP Type 32 Physical Field Contract

A live FormNewReport incident established an additional persistence requirement for incremental MCP creation. `Data.FormNewReports[].Settings.Fields[]` describes the mapping from Approval Form variables to report columns, but it does not replace the matching Type `32` child resource's physical `Fields[]` collection.

Before save, the Type `32` child must contain one MCP-issued physical field for every `Settings.Fields[]` mapping. The observed accepted physical storage names use native slots such as `Text1` through `TextN`, `Decimal1`, and `Datetime1`, with a positive index. `Text0` and an Approval mapping key such as `v_requestTitle` used as a physical `FieldName` were rejected by the platform.

The Type `0` default view must bind its columns and query entries to the physical field's issued `FieldID` and native `FieldName`, rather than the Approval variable key. Exact persisted readback must then prove the `DefKey`, matching Type `32` child, physical fields, and default-view bindings before the report receives Type `32` navigation.

This is persisted-readback evidence for the resource structure only. It does not prove runtime report row population, filtering, submitted-form detail opening, or export behavior.

## Field Mapping Findings

Export-proven mappings found:

| Approval variable type | Report field types found | Additional settings found | Proof |
| --- | --- | --- | --- |
| `text` | `input`, `textarea`, `richtext` | none for text/rich text | export-proven |
| `number` | `input_number`, `percent`, `currency` | `rounded-to`, `displayThousandths`, `currencyCode`, `displayFormat` | export-proven |
| `boolean` | `switch` | `displayStyle` | export-proven |
| `date` | `datepicker`, `time` | `showtime`, `dateformat` | export-proven |
| `file` | `file-upload` | none | export-proven |
| `img` | `icon-upload` | none | export-proven |
| `user` | `identity-picker` | `multiple` | export-proven |
| `groupselect` | `organization-picker` | `multiple`, `metadata-treeselect` | export-proven |
| `location` | `location-picker` | none | export-proven |
| `costcenter` | `cost-center-picker` | `multiple` | export-proven |
| `metadata` | `metadata` | `source`, `categoryId` | export-proven |
| `mutiple-metadata` | `mutiple-metadata` | `source`, `categoryId` | export-proven |
| `lookup` | `lookup` | `appid`, `listid`, `listfield`, optional `multiple` | export-proven |
| `list` | `textarea` | `listref` | export-proven |
| selected sub-list field `text` | `input` | none | export-proven |
| selected sub-list field `number` | `input_number` | `rounded-to`, `displayThousandths` | export-proven |
| selected sub-list field `date` | `datepicker` | `showtime`, `dateformat` | export-proven |
| selected sub-list field `user` | `identity-picker` | `multiple` | export-proven |
| selected sub-list field `lookup` | `lookup` | `appid`, `listid`, `listfield` | export-proven |
| selected sub-list field `file` | `file-upload` | none | export-proven |

User-provided/product-understanding mappings not found in this export:

- Text variable to Percent, Switch, Signature, Number, Currency, Date picker, Time picker, User, Department, Location, Cost center, Attachment, and Image.
- Attachment variable to any field type beyond `file-upload`.
- Location variable multiple behavior.
- Lookup display using a non-`Title` display field.

These are allowed as product expectations only after Help Center or runtime proof. Validators should warn on unknown mappings, not hard-fail compatibility exports.

## Additional Field Settings

Observed settings:

- Number: `rounded-to`, `displayThousandths`.
- Percent: `rounded-to`.
- Currency: `currencyCode`, `displayFormat`, `displayThousandths`, `rounded-to`.
- Switch: `displayStyle`.
- Date picker: `showtime`, `dateformat`.
- Time picker: `dateformat`.
- User: `multiple`.
- Department: `multiple`, `metadata-treeselect`.
- Cost center: `multiple`.
- Lookup: `appid`, `listid`, `listfield`, optional `multiple`.

The export uses numeric/string date format codes rather than literal `HH`, `HH:MM`, or `HH:MM:SS` labels. The UI-label mapping for those codes is not runtime-proven in this pass.

## Sub-list Behavior

Export-proven:

- `Settings.SubListID = ""` means no selected sub-list.
- `Settings.SubListID = "vlist_<variableId>"` selects one approval form list variable.
- When a sub-list is selected, report fields can include both normal approval variables and selected sub-list fields.
- Selected sub-list fields use `Key = <SubListID>_<subFieldId>`.
- Field keys must be unique within the report.
- Only one sub-list string is represented in each studied report.

Product-understanding-backed, not runtime-proven:

- If no sub-list is selected, one submitted approval request becomes one report row.
- If one sub-list is selected, report row count is driven by the number of sub-list rows.
- A request with three selected sub-list rows should appear as three report records.

Validator guidance: warn if more than one sub-list is generated or if a selected sub-list has no selected sub-list field mappings.

## Permissions

Export-proven:

- Report child resources use `ListModel.IsBreakInherit = false`, so this sample inherits application permissions.
- `ListModel.Perm = 4` is present on report child resources.
- `Attr.isExport` exists on report configs and appears mirrored into child `ListModel.LayoutView`.

Not found in this export:

- Custom administrators by users/departments/user groups.
- Custom View Report permission.
- Custom Export permission audience.
- All-users vs specific-users/departments/user-groups representation.

Validator guidance: if permission inheritance is disabled, warn when explicit administrator/view/export permission metadata is missing or opaque. Do not hard-fail custom permission gaps until an import-breaking shape is proven.

## Views

Export-proven:

- Form Reports support multiple `Data.Childs[].Layouts[]` list views.
- Views use `Type = 0`.
- Default views may have minimal `LayoutView` with only `Attr_IsViewDetail`.
- Custom views can include `layout`, `sort`, `query`, `rowColor`, `filter`, and `Attr_IsViewDetail`.
- `Attr_IsViewDetail` is the export flag for whether users can access the submitted form detail page from the view.

Cross-resource data-view update from `Data Lists (1).yap`:

- Data views are stored in the same `Layouts[]` family used by data lists and document libraries, with `Layouts[].Title`, `Type`, `Ext1.Url`, `IsDefault`, and `IsItemPerm` as top-level view metadata.
- Data-list export proves view type codes `0` list, `999` gallery, `104` kanban, and `100` calendar. The Form Report sample only proved `Type = 0`, so gallery/calendar/kanban Form Report view generation remains product-documented until a Form Report export proves those specific Type `32` settings.
- Default view detection should use `IsDefault = true`, not only the name `All Items`; the data-list export includes one default view renamed `All tasks`.
- Form Report views should continue recognizing `Attr_IsViewDetail` as a Form Report-specific detail-page access flag.

Runtime-sensitive:

- Whether row click actually opens or blocks the submitted detail page was not tested.

## Resource Relationships And Data Sources

Export-proven:

- Form Report relates to its source approval form through `DefKey`.
- Form Report appears as an app child resource with Type `32`.
- Type `32` entries can appear in root application navigation.

Not proven by this export:

- Lookup fields using Form Report as a data source.
- Data table or Collection controls using Form Report as a source.
- Summary/Pie/Column/Line/Pivot controls using Form Report as a source.
- Direct add/update/delete of report items.

The product understanding says Form Report can be used as a read/reporting data source. Keep that as product-understanding-backed until an export or runtime baseline demonstrates the referencing control schema.

## Validator Updates

`validate-yap-package.js` now adds warning-first Form Report checks:

- `Data.FormNewReports[].Settings` parses as JSON.
- `DefKey` resolves to an included approval form.
- A matching Type `32` child resource exists.
- Report fields have unique keys.
- Fields resolve to approval variables or selected sub-list fields.
- Known variable-to-report-field mappings are compatible.
- Field settings for number, percent, currency, switch, date/time, pickers, and lookup are recognized.
- One-sub-list-only guidance is enforced as a warning.
- Report resources should not define workflows or edit/create forms.
- View detail-page access flags are recognized.

Hard errors are reserved for clear generated-final structural failures. Compatibility exports should receive warnings/dependencies rather than runtime claims.

## Skill Recommendation

Recommendation: Option A, create a dedicated `yeeflow-form-report-generator` skill.

Reason: this export proves enough distinct reusable generation rules to justify a focused skill:

- a distinct app-level resource schema (`FormNewReports[]` plus Type `32` child resources),
- a mandatory approval-form dependency through `DefKey`,
- variable-to-report-field mapping rules,
- optional one-sub-list inclusion rules that change row granularity,
- list-view/detail-access rules,
- export/detail flags and permission inheritance behavior,
- no workflow and no direct item mutation surface.

The new skill must remain export-proven and validator-backed only until a focused runtime baseline imports, opens, configures, submits disposable approval requests, verifies row/detail/export behavior, and exports back for comparison.

## Recommended Next Step

After review/merge, run a focused Form Report runtime baseline branch. The baseline should generate the smallest safe package with one approval form, two Form Reports, one no-filter report, one selected-sub-list report, local validation, import/open/designer proof, disposable submissions, row-count/detail-access/export checks, and export-back comparison before plugin rebuild or broad generation use.

---

<a id="kanban-collection-dynamic-controls"></a>

<!-- source: docs/studies/collection-02.md#kanban-collection-dynamic-controls -->

# Kanban, Collection, And Dynamic Controls

## Feature Overview

This study documents the Kanban control, Collection control, and Dynamic field/user/image/file controls from the `Company Overview.yap` export. It focuses on how dashboard item templates bind to a data source current item, and how Dynamic controls bind to the current list item on a Data List custom form.

Export studied: `/Users/Renger/Downloads/Company Overview.yap`

Targets studied:

- Dashboard `Company overview`: Kanban control with Dynamic controls inside Kanban item cards.
- Dashboard `Collection of activity`: Collection control with Dynamic controls inside Collection items.
- Data List `Company Overview`: source fields for dashboard controls and custom forms.
- Data List custom form `View page`: Dynamic controls bound to the current list item.

Vertical Timeline and Horizontal Timeline controls are intentionally out of scope for this branch.

## Data Source

The export contains one Data List named `Company Overview`. The dashboard Kanban and Collection controls both use this list as their data source.

Observed fields include:

- `Title` / `Name`: text.
- `Datetime1` / `Start date`, `Datetime2` / `Due date`: date/time.
- `Text1` / `Status`, `Text2` / `Project owner`, `Text4` / `RAG status`, `Text5` / `Complexity`, `Text6` / `Impact Level`, `Text7` / `Risk Factor`: choice-style text fields.
- `Decimal1` / `Progress rate`: percent/decimal.
- `Decimal2` / `Project cost`: currency/decimal.
- `Text8` / `Reference`: file attachment field with multiple files enabled.
- `Text10` / `Assignee`: identity/user field with multiple users enabled.
- `Text11` / `Cover image`: image field.

The export does not include raw sample rows in the decoded `ListDatas` array. This study records field/data shape only and does not include private records.

## Kanban Control

Dashboard: `Company overview`

Observed Kanban configuration:

- Control type: `kanban`.
- Data source: `attrs.data.list`, resolving to the `Company Overview` Data List.
- Category/group-by field: `attrs.data.cateField = "Text2"`, resolving to `Project owner`.
- Item template: stored as child controls under the Kanban control, including `kanban-body` and `kanban-footer`.
- Current item dynamic bindings: Dynamic controls inside the Kanban item template use `attrs.source = "3"` plus `attrs["obj-f"] = <field name>`.
- Context expressions: the export contains `exprType = "variable_ctx"` with `ctx = "__ctx_kanban"` and `id = "_cate"` for the current Kanban category. It also contains a collection-style item expression context for progress display.

Observed Dynamic controls inside Kanban item cards:

- Dynamic image bound to `Text11` / `Cover image`.
- Dynamic user bound to `Text10` / `Assignee`.
- Dynamic field bound to general fields including `Title`, `Text3`, `Datetime1`, `Datetime2`, `Text4`, and `Decimal2`.

Kanban runtime behavior, card rendering, drag/drop, and click behavior are not proven by this export.

## Collection Control

Dashboard: `Collection of activity`

Observed Collection configuration:

- Control type: `collection`.
- Data source: `attrs.data.list`, resolving to the `Company Overview` Data List.
- Layout settings: `attrs.layout` contains grid/card layout keys such as column gap, row gap, content padding, and alignment.
- Item template: stored as child controls under the Collection control.
- Current item dynamic bindings: Dynamic controls inside the Collection item template use `attrs.source = "3"` plus `attrs["obj-f"] = <field name>`.

Observed Dynamic controls inside Collection items:

- Dynamic image bound to `Text11` / `Cover image`.
- Dynamic field bound to `Title`, `Text3`, and `Text4`.
- Dynamic user bound to `Text10` / `Assignee`.
- Dynamic file bound to `Text8` / `Reference`; two variants were observed, both limited to two files, one with file type icon display enabled.

Collection runtime rendering, card click behavior, filtering execution, image preview, and file preview/download behavior are not proven by this export.

## Data List View Page

Data List: `Company Overview`

Custom forms found:

- `View page`
- `Edit item`

The `View page` contains three Dynamic field controls bound to the current list item:

- `attrs.source = "4"` and `attrs["obj-f"] = "Title"` for `Name`.
- `attrs.source = "4"` and `attrs["obj-f"] = "Datetime1"` for `Start date`.
- `attrs.source = "4"` and `attrs["obj-f"] = "Datetime2"` for `Due date`, with an observed prefix of `-`.

This export proves Dynamic field usage on a Data List custom form with current-list-item context. It does not show Dynamic user, Dynamic image, or Dynamic file controls on the Data List View page; those controls are export-proven in Kanban/Collection item templates only in this study.

## Dynamic Control Schema

Shared observed binding pattern:

- `attrs.source = "3"` means current Collection/Kanban item context inside dashboard item templates.
- `attrs.source = "4"` means current Data List item context inside Data List custom forms.
- `attrs["obj-f"]` stores the source field name.

Dynamic field:

- Observed on Kanban, Collection, and Data List View page.
- Used for text, choice, date/time, percent/currency, and other general display values.
- Formatting/style settings appear under `attrs.common`, `attrs.item_style`, `attrs.t-af`, `attrs.t-be`, and optional `attrs.prefix`.

Dynamic user:

- Observed inside Kanban and Collection item templates.
- Bound to `Text10` / `Assignee`, an `identity-picker` field with multiple users enabled.
- Observed settings include `attrs["i-len"]`, `attrs.display_name`, `attrs.addition_fields`, `attrs.picture_style`, `attrs.text_style`, and `attrs.item_style`.

Dynamic image:

- Observed inside Kanban and Collection item templates.
- Bound to `Text11` / `Cover image`, an `icon-upload` image field.
- Observed settings include `attrs.preview_image`, `attrs.setting.img_height`, `attrs.setting.fit`, and image border/style settings.

Dynamic file:

- Observed inside Collection item templates.
- Bound to `Text8` / `Reference`, a `file-upload-merge` field with multiple files enabled.
- Observed settings include `attrs["i-len"]`, `attrs.content`, `attrs.opbtn`, and `attrs.type_icon_show`.

Single vs multiple:

- Multiple user handling is export-proven for the `Assignee` field through the list field rules and Dynamic user display limit.
- Multiple file handling is export-proven for the `Reference` field through the list field rules and Dynamic file display limit.
- Multiple image handling is not observed in this export.

## Comparison

Kanban:

- Data source lives at `attrs.data.list`.
- Grouping lives at `attrs.data.cateField`.
- Dynamic controls use current item source `3`.
- Category context appears as `__ctx_kanban` / `_cate`.

Collection:

- Data source lives at `attrs.data.list`.
- Layout lives at `attrs.layout`.
- Dynamic controls use current item source `3`.
- The studied Collection did not show a separate category context.

Data List custom form:

- Host list is implied by the custom form's parent Data List.
- Dynamic controls use current item source `4`.
- Field binding still uses `attrs["obj-f"]`.

## Generation Guidance

- Resolve every Kanban/Collection `attrs.data.list.ListID` to an included list-like source before handoff.
- For Kanban, resolve `attrs.data.cateField` to a source field.
- Use `attrs.source = "3"` for Dynamic controls inside Kanban/Collection item templates.
- Use `attrs.source = "4"` for Dynamic controls on Data List custom forms that display current record values.
- Bind Dynamic user controls to user/person fields, Dynamic image controls to image fields, and Dynamic file controls to attachment/file fields.
- Use Dynamic field for general field values; prefer specialized controls for user/image/file fields when generating new layouts.
- Do not claim Vertical Timeline or Horizontal Timeline support from this study.

## Validation Guidance

Validators should warn or hard-error depending on generation mode:

- Kanban data source must resolve.
- Kanban category/group-by field must resolve.
- Collection data source must resolve.
- Dynamic controls inside Kanban/Collection must bind to fields available on the source list current item.
- Dynamic controls on Data List custom forms must bind to current list item fields.
- Specialized Dynamic controls should bind to compatible source field types.
- Historical exports should remain warning-first where schema variants are incomplete.

## Proof Boundary

- Kanban control usage on `Company overview` is export-proven.
- Collection control usage on `Collection of activity` is export-proven.
- Dynamic controls inside Kanban/Collection are export-proven where observed.
- Dynamic controls on Data List `View page` are export-proven where observed.
- Runtime Kanban/Collection rendering, drag/drop, click behavior, dynamic file/image preview behavior, and Data List form runtime behavior are not proven unless later tested.
- Vertical Timeline and Horizontal Timeline are not studied in this branch.

---

<a id="new-capability-runtime-gate-process"></a>

<!-- source: docs/studies/collection-02.md#new-capability-runtime-gate-process -->

# Runtime Gate For New Capability Learning

This process applies when Codex learns a new Yeeflow function, schema, runtime behavior, resource type, workflow action, app setting, AI feature, report/service capability, or reusable generator rule.

The goal is to prevent export-only learning from being merged as runtime proof. New capability learning should pass through an explicit runtime decision before merge or final report.

## Two-Step Proof Model

1. Export-backed learning branch
2. Focused runtime baseline branch or isolated runtime continuation
3. Merge only after the proof boundary is clear

The steps may happen in one long task only when the user explicitly asks for end-to-end learning and runtime testing, and Codex can keep generated runtime artifacts isolated and safe. Otherwise, split the work.

## A. Learning Branch

Purpose:

- decode and study exports read-only
- document export-backed schema and relationships
- create normalized redacted references
- update validators carefully, using hard errors only for proven invalid generated shapes
- update relevant skills with export-proven knowledge only
- identify runtime-sensitive behavior and open questions
- avoid claiming import/runtime behavior

Do not commit raw `.yap`, `.yapk`, `.ydl`, `.ywf`, `.yaia`, `.yaic`, downloaded exports, decoded raw payloads, generated runtime packages, credentials, tokens, private users, tenant IDs, private images, or document binaries.

An export-backed learning branch may be merged before runtime only when the final report and docs clearly label the milestone as one of:

- export-proven only
- validator-backed only
- planning-guidance only
- import-proven only
- partial runtime proof

It should not be merged automatically after export study unless the user explicitly approves that proof boundary.

## B. Runtime Decision Before Merge

Before merging new capability learning, decide and document one of these paths:

- run a focused runtime test in a separate branch
- continue runtime proof on the same branch only if artifacts remain isolated, ignored, and safe
- defer runtime proof and merge as export-proven/validator-backed/planning-guidance/import-proven only, with explicit approval

Runtime proof is required before broad generation guidance may claim Yeeflow runtime behavior, especially when the capability affects imported app rendering, workflow execution, email delivery, AI execution, document upload/persistence, external API calls, user/group membership, permissions, custom code execution, row mutation, or destructive operations.

Runtime proof may be deferred when the change is only documentation, export schema mapping, conservative validator warnings, planning guidance, or non-executing import/configuration proof.

## C. Focused Runtime Baseline

When runtime proof is needed:

- create the smallest focused app that tests only the learned capability
- avoid unrelated app complexity, broad dashboards, large workflows, or mixed feature packs
- use fresh generated IDs
- validate locally before building
- build only after validation passes
- import into `https://<yourdomain>.yeeflow.com/` only after local validation passes and the user has requested runtime testing
- test safe runtime behavior only
- avoid real email, live AI calls, external APIs, destructive updates, private users, private images, private documents, and credentials unless the safe test scope is explicit

Document the exact proof boundary:

- passed
- partial
- blocked
- not tested

## D. Merge Rule

Do not merge new capability learning to `main` as runtime-proven unless focused runtime testing passed and the tested behavior is documented.

If merged before runtime, the merge report must state the exact proof boundary and preserved gaps. Good labels include:

- export-proven only
- validator-backed only
- planning-guidance only
- import-proven only
- configuration-visible only
- render-only proven
- partial runtime proof
- runtime-proven for the tested host/scope only

Skill updates should also preserve the label. Do not promote a partial result into broad app-generation rules.

## Final Report Requirements

Every new capability learning final report should include:

- branch and commit
- exports studied or runtime package tested
- whether runtime was run
- runtime decision before merge
- proof-boundary label
- validators updated
- skills updated
- generated baseline package path, if any, and whether it is ignored/uncommitted
- safety scan result
- what is proven
- what remains unproven
- whether a focused runtime branch or follow-up is needed

## Focused Runtime Baseline Prompt For Newly Learned Capability

Use this prompt when export-backed learning needs a focused runtime follow-up:

```text
Continue from branch:
<learning branch>

Goal:
Create a focused runtime baseline for the newly learned Yeeflow capability:
<capability name>

Learned patterns to test:
- <export-proven structure or validator rule>
- <resource/action/setting/control shape>
- <known dependency or risk>

Focused app requirements:
- Build the smallest app that exercises only this capability.
- Use fresh generated IDs and fresh FlowKey/form keys when forms/workflows are included.
- Include only required lists/forms/dashboards/workflows/resources.
- Do not include real users, real emails, private tenant data, credentials, external connections, private images, or document binaries.

Validation checks:
- Run relevant JS/MJS syntax checks.
- Run JSON parse checks.
- Run package/list/form/workflow/graph/materialization validators.
- Run wrapper round trip if a `.yap` is generated.
- Run safety scans for raw exports, decoded payloads, generated packages, credentials, tokens, emails, tenant IDs, users, image binaries, and private data.
- Do not build before validation passes.
- Do not runtime-test before local validation passes.

Runtime steps:
- Import only into https://<yourdomain>.yeeflow.com/.
- Confirm app imports and opens.
- Test only the learned capability and required host/resource surfaces.
- Do not execute live AI, send email, call external APIs, mutate real records, or upload private files unless explicitly scoped and safe.

Proof-boundary documentation:
- Classify each tested behavior as passed, partial, blocked, or not tested.
- Separate import proof, configuration visibility, render proof, and actual runtime execution.
- Preserve unproven contexts and unsafe paths as follow-ups.

Commit and push:
- Commit docs, validators, skills, and safe tests only.
- Do not commit generated `.yap` packages, raw exports, decoded payloads, private data, image binaries, `.env`, credentials, or tokens.
- Push the runtime branch.

Final report:
- branch
- generated baseline path, if any
- validation results
- runtime result
- exact proof boundary
- docs/skills updated
- safety scan result
- commit hash
- push status
- whether the original learning branch can now merge as runtime-proven, partial, or still export-proven only
```

---

<a id="pivot-table-control-runtime-proof"></a>

<!-- source: docs/studies/collection-02.md#pivot-table-control-runtime-proof -->

# Pivot Table Control Runtime Proof

## Scope

This is a focused runtime proof pass for Dashboard Pivot Table Data Analytics controls.

- Branch: `codex/pivot-table-control-runtime-proof`
- Generated package: `pivot-table-control-runtime-proof.v1.yap`
- Downloads copy: `<downloads>/pivot-table-control-runtime-proof.v1.yap`
- Follow-up package: `pivot-table-control-runtime-proof-with-sample-data.v2.yap`
- Follow-up Downloads copy: `<downloads>/pivot-table-control-runtime-proof-with-sample-data.v2.yap`
- Runtime site: `https://<yourdomain>.yeeflow.com/`
- App name: `Pivot Table Runtime Proof`
- Dashboard page: `Dashboard`
- Source learning baseline: `docs/studies/collection-02.md#pivot-table-control`

## Proof Boundary

- Dashboard Pivot Table schema remains export-proven from `CRM - Customer relationship management (1).yap`.
- The generated package is locally validator-backed for Pivot Table data source, row, column, value, aggregation, and date-grouping references.
- Runtime import/open/empty-render behavior is runtime-proven for the generated representative Dashboard package.
- The v2 follow-up package is user-confirmed to work well after manual import, and adding new items in its data list is user-confirmed successful.
- Populated Pivot Table rendering is user-confirmed at package level, but this pass does not separately prove exhaustive aggregation correctness or every individual value.
- Data List form Pivot Table behavior, alternate data source classes, Data Filter variable interaction, permissions, security, large-data performance, and workflow behavior are not runtime-proven.
- Data mutation proof is limited to adding new items in the v2 package's `Deals Analytics Runtime Test` data list; do not generalize it to other lists, workflows, permissions, or bulk operations.

## Generated Package

The generator script `tools/generators/generate-pivot-table-control-runtime-proof.mjs` creates a small synthetic Yeeflow app with one data list and three Dashboard Pivot Table controls:

| Pivot Table | Rows | Columns | Value | Purpose |
| --- | --- | --- | --- | --- |
| Count by Deal Type and Owner | Deal Type | Deal Owner | `COUNT(ListDataID)` | count-based representative Pivot Table |
| Sum Amount by Lead Source and Year | Lead Source | Close Date grouped by `YEAR` | `SUM(Amount)` | numeric/date-grouped representative Pivot Table |
| Average Amount by Stage and Deal Type | Stage | Deal Type | `AVG(Amount)` | additional numeric aggregation representative |

The package uses synthetic fields and local-safe sample row definitions only. The source export and decoded payloads are not committed.

The v2 follow-up package uses the same three Pivot Table controls and includes 20 safe synthetic sample rows in `Deals Analytics Runtime Test`. It also fixes the generated field definition alignment by cloning data-list field definitions by `FieldName`, so exported `FieldID` / `FieldName` / row-cell references stay aligned after import.

## Local Validation

Local validation passed the strict generated-app readiness gate with zero errors.

- `node --check tools/generators/generate-pivot-table-control-runtime-proof.mjs`: pass
- `node --check scripts/inspect-pivot-table-controls.mjs`: pass
- Pivot Table inspector: pass, 3 Pivot Tables, 0 findings
- Dashboard/page inspector: pass
- YAP schema-standard inspector: pass, 0 errors
- App-creation rules inspector: pass, 0 errors
- Strict package validator: `pass_with_warnings`, 0 errors
- Strict graph validator: pass, 0 errors
- Materialization inspector: `pass_with_warnings`, 0 errors; one expected dashboard-only `NO_FORMS` warning
- Aggregate import-readiness inspector: `pass_with_warnings`, 0 errors

Non-blocking warnings were limited to small proof-app presentation warnings such as empty app theme/arbitrary design colors and the expected dashboard-only no-forms warning.

## Runtime Result

The generated package imported successfully into Yeeflow.

The imported app opened successfully, and the `Dashboard` page opened without a dashboard crash. The three Pivot Table controls rendered in the Dashboard shell with visible control titles and row header bindings:

- `Deal Type`
- `Lead Source`
- `Stage`

No visible missing data source error, missing row/column/value binding error, or dashboard render crash appeared during the Dashboard open.

## Runtime Blocker

The package-defined `ListDatas` sample rows were not present in the imported data list at runtime. The imported `Deals Analytics Runtime Test` list opened and showed the expected field headers, but no rows.

A manual attempt to add one safe synthetic row through the Yeeflow UI reached the add form and populated fields, but Yeeflow returned `Add failed`. Because this proof pass is not a data-mutation investigation, the failure is recorded as a blocker for populated Pivot Table rendering rather than converted into a broader data-list mutation claim.

As a result, this pass does not prove:

- populated count-based Pivot Table value cells
- populated `SUM(Amount)` Pivot Table value cells
- populated date-grouped Pivot Table value cells
- grand total row or column display with values
- aggregation correctness

## V2 Follow-Up Result

After the v1 blocker, the generator produced `pivot-table-control-runtime-proof-with-sample-data.v2.yap` with 20 safe synthetic data-list rows and field definitions cloned by field name instead of by array position.

Local validation for v2 passed with zero errors:

- `node --check tools/generators/generate-pivot-table-control-runtime-proof.mjs`: pass
- Pivot Table inspector: pass, 3 Pivot Tables, 0 findings
- Aggregate import-readiness inspector: `pass_with_warnings`, 0 errors

The v2 package was copied to `<downloads>/pivot-table-control-runtime-proof-with-sample-data.v2.yap` for manual import testing.

The user confirmed that the regenerated package works well and that new items can be added successfully in the data list. This resolves the v1 data-list sample/add blocker for the focused v2 package.

The confirmation is intentionally narrow:

- generated v2 package import/open/use is user-confirmed
- data-list sample/add behavior is user-confirmed for this v2 app
- Dashboard Pivot Table package behavior is user-confirmed at practical smoke-test level
- exhaustive aggregation correctness, every aggregation function, every date grouping mode, alternate data source classes, Data Filter variable interaction, Data List form hosting, permissions, security, large-data performance, and workflow behavior remain unproven

## v1 to v2 Data List Seed/Add Readiness Root Cause

The v1 failure is strongly indicated by the v1/v2 package and generator diff rather than by a Yeeflow server trace. The reusable issue is generic generated Data List seed/add readiness, not Pivot Table schema.

What failed in v1:

- the package imported and the Dashboard opened
- Pivot Table controls rendered structurally
- imported `ListDatas` rows did not appear in the runtime data list
- manual add of a safe synthetic row returned `Add failed`

What changed in v2:

- the data-list field definitions are cloned by `FieldName` instead of by array position
- the same list carries 20 safe synthetic rows
- the user confirmed that rows appeared, Pivot Tables worked with sample data, and adding new list items succeeded

Strongest identified cause:

- v1 reused template `Defs[]` by array position after replacing the field names
- this crossed storage metadata:
  - `Text1` and `Text2` kept `FieldType: Datetime`
  - `Datetime1` kept `FieldType: Text`
  - `Decimal1` kept `FieldType: Text`
- seeded rows used keys such as `Text1`, `Datetime1`, and `Decimal1`, but the field storage metadata no longer matched those keys
- v2 selected the template field definition by `FieldName`, restoring `Text* -> Text`, `Datetime* -> Datetime`, and `Decimal* -> Decimal`

Exact generator rule learned:

- when generating or repurposing data-list fields, never clone field definition templates by array position
- select the source/template field definition by the target `FieldName` storage family, then rewrite display metadata, internal name, ID, and list ownership
- seeded `ListDatas` rows must use keys that resolve to fields whose `FieldName`, `FieldType`, and `Type` agree with the runtime value format
- analytics/demo apps that depend on populated dashboard controls should validate seed rows and add-ready list metadata before handoff

Validator/import-readiness rule added:

- generated-final validation now hard-errors on `FIELD_NAME_FIELDTYPE_MISMATCH`
- the app-creation rules inspector also hard-errors the same mismatch
- the aggregate import-readiness gate inherits this check through package validation and app-creation rules

Proof boundary:

- v2 rows visible, Pivot Tables working with sample data, and add-new-item success are user-confirmed
- the root cause is strongly indicated by the v1/v2 diff and by v2 recovery after field definition alignment, but no server-side import/add trace was captured
- this is not exhaustive proof for all data-list field types, custom forms, permissions, lookup values, bulk imports, or all analytics aggregations

## Runtime Observations

Observed at runtime:

- package import succeeded
- app appeared in the workspace
- app opened
- Dashboard opened
- Dashboard Pivot Table controls did not crash
- row header bindings rendered for the three representative Pivot Tables
- no visible missing data source or missing binding error appeared

Not observed in this pass:

- exhaustive aggregation correctness
- all supported aggregation functions
- all supported date grouping modes
- Data Filter variable interaction
- alternate data source classes
- Data List form Pivot Table hosting

## Next Step

If broader proof is needed, run a dedicated aggregation-correctness pass with known expected values, plus separate focused passes for Data List form hosting, Data Filter variable interaction, and alternate data sources such as Document Library, Form Report, and Data Report.

---

<a id="pivot-table-control"></a>

<!-- source: docs/studies/collection-02.md#pivot-table-control -->

# Pivot Table Data Analytics Control

## Source

- Source export: `<downloads>/CRM - Customer relationship management (1).yap`
- Target page: root dashboard page `Dashboard`
- Inspector: `scripts/inspect-pivot-table-controls.mjs`
- Decode policy: export decoded read-only; raw `.yap` and raw decoded payloads are not committed.

## Proof Boundary

- Pivot Table Dashboard usage in this CRM export: export-proven.
- Data List form availability: product/user-understanding-backed in this pass.
- Approval Form and Data List Public Form non-availability: product/user-understanding-backed in this pass.
- Data List source usage, field references, aggregations, date grouping, static filters, and style sections described below: export-proven from the CRM Dashboard.
- Document Library, Form Report, and Data Report as supported Pivot Table source classes: product/user-understanding-backed unless a later export proves exact source bindings.
- Pivot Table filter-variable support: product/user-understanding-backed; this export contains Data Filter variables on the page but no Pivot Table condition that consumes them.
- Runtime import/open/render/interaction behavior: not runtime-proven in this pass.

## What Pivot Table Is

Pivot Table is a Data Analytics control for multi-dimensional data summaries. Unlike simpler chart controls that usually bind one group axis and one value measure, Pivot Table defines rows, columns, and values together, can create nested row/column headers, and can show totals across grouped data.

Product context says Pivot Table is available on Dashboard pages and Data List forms. It is not available on Approval Forms or Data List Public Forms. It belongs to the same Data Analytics family as Pie chart, Column chart, Line chart, and Summary.

## Export Schema Summary

The visible dashboard control is stored in the embedded Type `103` page JSON:

- `children[]...type = "pivot-table"`
- `children[]...id = <control id>`
- `children[]...attrs.rows`, `attrs.columns`, and `attrs.values` hold layout sizing/visibility metadata keyed like `pt_c_0`.
- `children[]...attrs.header`, `attrs.body`, `attrs.subtotal`, and `attrs.grandtotal` hold table style sections.
- Some controls include `attrs.showsorter = false` and `attrs.values.showtotal = false`.

The data analytics binding is stored separately in the page `exts[]` array:

- `category = "___Pivot___"`
- `key = "PivotTable"`
- `i = <matching pivot-table control id>`
- `attr.ListID = <source resource id>`
- `attr.ListSetID = <app/listset id>`
- `attr.settings.rows[]`
- `attr.settings.columns[]`
- `attr.settings.values[]`
- optional `attr.settings.Conditions[]`

Generation rule: a generated Pivot Table must create both the visible `pivot-table` control and a matching `page.exts[]` item whose `i` equals the control id.

## Inventory

The CRM Dashboard contains 14 Pivot Table controls.

| Control | Source | Rows | Columns | Values | Aggregation | Date grouping | Filters | Style sections | Proof |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `<pivot:1>` | Data list `<source:12>` | Deal type, owner | Close date | Amount | COUNT | DAY | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:2>` | Data list `<source:12>` | Deal type, owner | Close date | Amount | COUNT | MONTH | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:3>` | Data list `<source:12>` | Deal type, owner | Close date | Amount | COUNT | QUARTER | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:4>` | Data list `<source:12>` | Deal type, owner | Close date | Amount | COUNT | YEAR | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:5>` | Data list `<source:12>` | Deal type, owner | Source, close year | Amount | COUNT | YEAR | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:6>` | Data list `<source:12>` | Deal type, owner | Source, close year | Amount | COUNT_DISTINCT | YEAR | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:7>` | Data list `<source:12>` | Deal type, owner | Source, close year | Amount | SUM | YEAR | static conditions | header/body/subtotal/grandtotal | export-proven |
| `<pivot:8>` | Data list `<source:12>` | Deal type, owner | Source, close year | Amount | AVG | YEAR | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:9>` | Data list `<source:12>` | Deal type, owner | Source, close year | Amount | MAX | YEAR | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:10>` | Data list `<source:12>` | Deal type, owner | Source, close year | Amount | MIN | YEAR | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:11>` | Data list `<source:3>` | Country/region | Industry | record id | COUNT | none | none | header/body/subtotal/grandtotal | export-proven |
| `<pivot:12>` | Data list `<source:3>` | Country/region | Industry | record id | COUNT | none | none | header/body/subtotal/grandtotal, sorter/total disabled | export-proven |
| `<pivot:13>` | Data list `<source:3>` | Country/region | none | record id | COUNT | none | none | header/body/subtotal/grandtotal, sorter/total disabled | export-proven |
| `<pivot:14>` | Data list `<source:3>` | none | Industry | record id | COUNT | none | none | header/body/subtotal/grandtotal, sorter/total disabled | export-proven |

## Data Sources

The sample export proves Data List sources. Each Pivot Table `attr.ListID` resolves to an included child resource whose fields are used by `settings.rows[]`, `settings.columns[]`, `settings.values[]`, and optional `settings.Conditions[]`.

Validation rules:

- `attr.ListID` must resolve to an included data source.
- Supported source classes are Data List, Document Library, Form Report, and Data Report.
- Unknown source types should warn first unless an export or runtime proof shows the shape is invalid.
- Row, column, value, and condition fields must resolve to fields on the selected source or known system fields such as `ListDataID`, `Created`, and `Modified`.

## Rows, Columns, And Values

Rows and columns use field descriptor objects:

- `fieldName`
- `label`
- `fieldType`
- `type`
- `func`
- `id`
- optional `attr`

Rows in the sample use normal grouping fields with empty `func`. Columns use both normal grouping fields and date/time grouping functions. Values use a field plus aggregation in `func`, with `id` commonly shaped as `<fieldName>_<AGGREGATION>`.

Generation rules:

- Choose row fields for categorical grouping.
- Choose column fields for time, category, or second-dimension grouping.
- Choose value fields for record counts or numeric measures.
- Use `ListDataID` or another stable field with `COUNT` when counting records.
- Use numeric/currency fields for `SUM`, `AVG`, `MIN`, and `MAX`.

## Aggregations

Export-proven value aggregations:

- `COUNT`
- `COUNT_DISTINCT`
- `SUM`
- `AVG`
- `MAX`
- `MIN`

Validation rules:

- Value entries should declare an aggregation.
- Unknown value aggregation should warn first.
- `SUM`, `AVG`, `MIN`, and `MAX` should target numeric/currency fields where detectable.
- `COUNT` and `COUNT_DISTINCT` can count records or count values.

## Date/Time Grouping

Export-proven date grouping functions:

- `DAY`
- `MONTH`
- `QUARTER`
- `YEAR`

The sample uses a date field as a column dimension and serializes grouping in the axis field descriptor `func`. The field descriptor id follows `<fieldName>_<GROUPING>`.

Validation rules:

- Date grouping should only be applied to date/time fields where detectable.
- Unknown row/column grouping should warn first.
- Generated apps should prefer month/quarter/year for dashboard readability unless daily detail is explicitly useful.

## Filters And Data Filter Variables

One Pivot Table uses static `attr.settings.Conditions[]` entries with:

- `key`
- `pre`
- `left`
- `op`
- `right`
- `showCus`

The Dashboard also has `filterVars[]` and multiple Data Filter controls. Other analytics controls consume those variables, but the inspected Pivot Table conditions do not. Product context says Pivot Tables can use Data Filter variables; generation should use the same expression-token shape as other Data Analytics controls when a Pivot Table is meant to react to dashboard filters.

Validation rules:

- Condition `left` fields must resolve to the selected source fields.
- Filter variable expression tokens such as `__filter_<filterVar>` must resolve to `page.filterVars[]`.
- Filter variable operator compatibility should warn first until more exports prove operator/value semantics.

## Styling

Export-proven style sections:

- `attrs.header`
- `attrs.body`
- `attrs.subtotal`
- `attrs.grandtotal`
- optional `attrs.showsorter`
- optional `attrs.values.showtotal`

The sample styles headers with a dark neutral background and white text, body cells with padding and borders, and grand totals with stronger font weight plus a subtle tinted background.

Generation guidance:

- Style Pivot Tables for readability by default.
- Use clear table headers.
- Use subtle header and grand-total backgrounds.
- Align numeric values consistently.
- Avoid overly narrow columns.
- Avoid excessive nested groups unless they answer a real business question.
- Use nearby title or description context when the Pivot Table's row/column meaning is not obvious.

## Generator Rules

- Use Pivot Table for multi-dimensional summaries where users need row/column analysis.
- Generate Pivot Tables only on Dashboard pages or Data List forms.
- Do not generate Pivot Tables on Approval Forms or Data List Public Forms.
- Create the visible `pivot-table` control and matching `page.exts[]` data analytics entry.
- Resolve the source resource before handoff.
- Resolve every row, column, value, and filter condition field before handoff.
- Use count aggregation for record counts.
- Use sum/average/min/max for numeric or currency metrics.
- Use date grouping only on date/time fields.
- Wire Data Filter variables into `Conditions[]` only when the variables exist on the page.
- Include readable header/body/grand-total styling by default.

## Validator Rules Added

- Dashboard Pivot Table controls require matching `page.exts[]` entries.
- `page.exts[]` Pivot Table source ListID must resolve.
- Row, column, value, and condition fields must resolve.
- Value aggregation must be known or warning-first.
- Numeric aggregations must target numeric/currency fields where detectable.
- Date grouping must target date/time fields where detectable.
- Filter variable references must resolve to `page.filterVars[]`.
- Pivot Table is blocked on Approval Forms and Data List Public Forms for generated final packages.
- Import-readiness inspection includes the Pivot Table inspector.

---

<a id="scheduled-workflow-generation-rules"></a>

<!-- source: docs/studies/collection-02.md#scheduled-workflow-generation-rules -->

# Scheduled Workflow Generation Rules

Source export: `<downloads>/AI Agent and Copilot Local Resource Baseline8.yap`

Classification: generation guidance from export-proven structure plus generated import/open/designer runtime proof for the safe local baseline. A later user-confirmed function test passed, but the exact execution scope is not yet documented, so schedule trigger execution, email delivery, manual run behavior, and AI execution should not be claimed separately from that result.

## Minimal Shape

To generate a Scheduled Workflow resource, include an app-level `Data.Forms[]` entry with:

- `Name`
- `Key`
- `AppID`
- `ListID = 0`
- `ProcModelID`
- `DefResource` JSON string
- `WorkflowType = 3`
- `Settings` JSON string
- `Deployed` value aligned to intended runtime behavior

The `DefResource` should include:

- `childshapes[]`
- `variables.basic`, `variables.listref`, `variables.filter`
- `pageurls[]`
- `defkey`
- `workflowType = 3`
- `AppListSetID`
- graph position/zoom metadata

## Safe Baseline Decision

The safe baseline branch generated `scheduled-workflow-safe-runtime-baseline.v1.yap` and proved import/open/designer rendering in Yeeflow. Use this as the current minimal generated Scheduled Workflow baseline:

- app: `Scheduled Workflow Safe Runtime Baseline`
- local list: `Runtime Ideas`
- AI Agent: `Email generation`
- Scheduled Workflow: `Safe scheduled idea summary`
- workflow graph: `Start -> QueryData -> AI -> MailTask -> End`
- schedule: far-future weekly Monday/Wednesday, `11:59PM`, with `Deployed = false`
- recipient: `<REDACTED_SAFE_TEST_EMAIL>`
- no external connections or credentials

The generated package imported, opened, displayed the local list, listed the Scheduled Workflow resource, opened the workflow detail page, rendered recurrence settings and variables, opened the workflow designer, and opened the `QueryData`, `AI`, and `MailTask` configuration panels. User-confirmed function test passed; exact execution scope not yet documented.

Do not claim that a non-deployed or far-future schedule can never execute under all tenant/runtime conditions. Treat this as an import/open-safe runtime baseline, not an execution baseline.

## Candidate Execution Baseline

Only after explicit safe test scope is available, generate a separate execution baseline:

- one harmless local data list
- one local AI Agent with no external tools
- one Scheduled Workflow with `QueryData`, `AI`, and `MailTask`
- safe test recipient controlled by the test tenant
- schedule disabled until manually triggered, or configured for a clearly safe manual execution path
- explicit confirmation before running the workflow
- fresh ID family and fresh workflow node IDs

## Validator Gates

- package validation
- graph validation
- workflow action configuration validation
- scheduled recurrence settings validation
- `QueryData` list reference validation
- `AI` Agent reference validation
- `MailTask` recipient risk scan
- expression/button HTML reference scan
- ReplaceIds scope check
- raw export and secret scan

For import/open-safe generated baselines, an `AI` workflow node that resolves to a bundled local Agent and contains no credentials should be reported as a runtime-sensitive dependency, not as a package-blocking validation error. Missing Agent references, unresolved `QueryData` list targets, unsafe recipients, embedded secrets, or credential-bearing external actions remain blockers.

## Start and Assignment Task Action Addendum

`Workflow Actions Runtime Baseline (1).yap` adds a quick export-proven Scheduled Workflow comparison for Start and Assignment Task actions:

- one Scheduled Workflow resource with `WorkflowType = 3`, `ListID = 0`, parseable schedule `Settings`, and parseable `DefResource`
- one `StartNoneEvent` with zero incoming flows, one outgoing flow, `isenabledemail`, `to`, `subject`, `html`, and `taskurl`
- no `terminate`, `terminate-conditions`, or `revoke-conditions` fields on the scheduled Start action
- one `MultiAssignmentTask` with `properties.usertaskassignment[]`
- the scheduled Assignment Task uses one applicant-line-manager expression assignee
- the scheduled Assignment Task has absent `tasktype`, `approveway = "allapprove"`, `approvepercentage = 100`, absent `issequential`, `duedatedefinition = 120`, `isenabledemail = false`, and no `notifyrules`

Generation rules from this addendum:

- Preserve Scheduled Workflow Start email fields when present, but do not add approval-form terminate/recall fields unless a scheduled export proves them.
- Preserve Scheduled Workflow Assignment Tasks as the same `MultiAssignmentTask` family, but only treat the applicant-line-manager expression assignee as export-proven for Scheduled Workflow from this source.
- Do not add data-list field or Created By expression sources to Scheduled Workflow Assignment Tasks unless a scheduled export proves a list/query context that provides those values.
- Do not claim Scheduled Workflow task routing, task creation, email delivery, due-date behavior, or appointed-order behavior without focused runtime proof.

Reference:

- `docs/studies/collection-03.md#workflow-scheduled-vs-approval-data-list-actions`

---

<a id="scheduled-workflow-resource-structure"></a>

<!-- source: docs/studies/collection-02.md#scheduled-workflow-resource-structure -->

# Scheduled Workflow Resource Structure

Source export: `<downloads>/AI Agent and Copilot Local Resource Baseline8.yap`

Classification: export-proven structure only. No generated scheduled-workflow package, schedule execution, email delivery, or AI execution is runtime-proven in this branch.

## Location

Scheduled Workflow resources are stored in `Data.Forms[]`, not in `Data.Childs[]`.

Observed resource markers:

| Field | Value / pattern |
| --- | --- |
| `WorkflowType` | `3` |
| `ListID` | `0` |
| `ProcModelID` | large numeric workflow/process ID string |
| `Key` | workflow key, such as `AS-WIU` or `AS-DIU` |
| `DefResource` | JSON string containing workflow graph, variables, pageurls, and metadata |
| `Settings` | JSON string containing recurrence schedule |
| `Deployed` | `true` in both studied workflows |

The decoded `DefResource` has the same broad process graph shape as other Yeeflow workflows:

- `childshapes[]` contains `StartNoneEvent`, action nodes, `SequenceFlow`, and `EndNoneEvent`.
- `variables.basic[]`, `variables.listref[]`, and `variables.filter[]` store workflow variable definitions.
- `pageurls[]` exists even for scheduled workflows and contains a lightweight workflow page with `workflowControlPanel` and `workflowHistory`.
- `defkey` matches `Data.Forms[].Key`.
- `workflowType = 3`.
- `AppListSetID` points to the containing application/listset.

## Workflows Found

| Workflow | Key | Schedule | Actions |
| --- | --- | --- | --- |
| Weekly information update | `AS-WIU` | weekly, Monday and Wednesday | `MailTask` |
| Daily information update | `AS-DIU` | every 2 working days | `QueryData`, `AI`, `MailTask` |

## Resource Dependencies

The daily workflow references:

- data list `Innovation Ideas` through `QueryData.properties.listid`
- AI Agent `Email generation` through `AI.properties.data.AgentID`
- workflow variables `QueryItems`, `QueryAmount`, `EmailSubject`, and `EmailBody`

## ReplaceIds

The wrapper contained 23 `ReplaceIds`. For generated packages, scheduled workflow process IDs, workflow graph node IDs, page IDs, local list IDs, local field IDs, AI resource IDs, and local app/listset IDs should be considered for fresh generated IDs. Do not remap tenant/user metadata or private recipient values.

## Validation Recommendations

- Treat `WorkflowType = 3` as the Scheduled Workflow discriminator.
- Validate `Settings` parses as JSON and contains `TimeZone`, `Times`, `StartDate`, `Frequency`, and `Interval`.
- Validate `DefResource` parses as JSON and `defkey` matches `Key`.
- Validate `QueryData` targets resolve to included data lists.
- Validate `AI` agent-mode actions resolve to included AI Agent resources.
- Warn on fixed email recipients and block live execution unless recipients are explicitly safe.

## Start and Assignment Task Comparison Export

`Workflow Actions Runtime Baseline (1).yap` adds one Scheduled Workflow with Start and Assignment Task actions:

| Item | Export-proven shape |
| --- | --- |
| Workflow | `WorkflowType = 3`, `ListID = 0` |
| Schedule | `TimeZone`, `Times[]`, `StartDate`, `EndDate`, `Frequency`, `Interval` |
| Start | `StartNoneEvent`, no incoming flow, one outgoing flow |
| Start email | `isenabledemail`, `to`, `subject`, `html` |
| Start terminate/recall | `terminate`, `terminate-conditions`, and `revoke-conditions` absent |
| Assignment Task | `MultiAssignmentTask` with `properties.usertaskassignment[]` |
| Assignee | one applicant-line-manager expression-button assignee |
| Assignment settings | absent `tasktype`, `approveway = "allapprove"`, `approvepercentage = 100`, absent `issequential`, `duedatedefinition = 120` |

This proves Scheduled Workflow can store the same Start and Assignment Task action families, but not execution behavior. Do not trigger schedules, send email, or claim task routing from this export.

---

<a id="sub-list-dynamic-content"></a>

<!-- source: docs/studies/collection-02.md#sub-list-dynamic-content -->

# Sub List Dynamic Content Layout And List Actions

## Source

Export studied: `/Users/Renger/Downloads/Sub list Dynamic.yap`

The package contains one Approval Form named `Dynamic sub list form`. It contains two visible Sub List controls. Both Sub Lists use the advanced Dynamic content layout. No Data List custom form with a Sub List is present in this export.

## Feature Overview

Sub List controls in Approval Forms are exported as normal form controls with `type = "list"`. The associated variable is a `variables.basic[]` entry with `type = "list"`, and its `value` points to a `variables.listref[]` row schema. The control stores displayed row fields in `attrs["list-fields"]`, row variable schema in `attrs["list-variables"]`, summary configuration in `attrs["list-fields-summary"]`, fallback text in `attrs.fallback`, list actions in `attrs.actions`, and Dynamic content layout selection in `attrs["list-display-preference"] = "dynamic"`.

Dynamic content layout stores the item template as normal child controls under a `list-body` child. Field controls inside the dynamic item template use the row field as `binding`, set `attrs.list_field = true`, set `attrs.list_field_binding` to the parent Sub List binding, and keep `attrs.list_control_id` pointing at the parent control. The footer is a separate `list-footer` child and can contain action buttons plus `list-summary` controls.

## Controls Found

The first Sub List is titled `Sub list`, binds to the list variable `field_9`, and displays four row fields: text, number, date, and text. It uses Dynamic content layout, a fallback text of `Click the following button to add a new item.`, a visible total summary for the numeric row field, footer buttons for Add another item and Import items, and item-level actions for Duplicate, Insert before, Insert after, and Delete.

The second Sub List is titled in navigation as `Dynamic List Body`, binds to the list variable `DynamicScrollList`, and displays seven row fields: five text fields, one date field, and one number field. It uses the same Dynamic content model, includes a visible total summary for its numeric field, includes the same list actions, and includes the custom CSS pattern below.

```css
selector .dynamic-list .list-footer
{
    position: absolute;
    left: 0;
    right: 0;
    bottom: -60px;
}
```

This CSS is export-proven as part of the second Sub List's layout. Treat it as a layout-preservation pattern for fixed-width or horizontal-scroll dynamic list designs, not as a required rule for every Dynamic content Sub List.

## Dynamic Content Schema

Normalized shape:

```json
{
  "type": "list",
  "binding": "<list-variable-id>",
  "attrs": {
    "list-display-preference": "dynamic",
    "list-fields": ["<row-field-controls>"],
    "list-variables": ["<row-field-definitions>"],
    "list-fields-summary": ["<summary-settings>"],
    "fallback": { "et": "Click the following button to add a new item." },
    "actions": ["<list-action-definitions>"]
  },
  "children": [
    { "type": "list-body", "children": ["<dynamic-item-template-controls>"] },
    { "type": "list-footer", "children": ["<footer-buttons-and-summaries>"] }
  ]
}
```

The dynamic item template can contain normal layout and visual controls. This export proves `container`, `dropbar`, `action_button`, `line`, `icon`, `input`, `input_number`, `datepicker`, and `flex_grid` inside the item template. The second Sub List uses a `flex_grid` inside the item template to align row controls into a table-like row.

## Table-Header Grid Pattern

The second Sub List includes an independent `flex_grid` directly above the Sub List. It is labeled as a Sub List header and contains field-header text such as `Field 1`, `Field 2`, and related column headers. This grid is not stored as a formal child of the Sub List; it is an adjacent/sibling layout control. The matching row layout lives inside the Sub List `list-body` dynamic item template.

Generation guidance: build table-style Dynamic Sub Lists as a sibling header grid plus a Dynamic content Sub List whose item template uses a matching grid/column layout. Do not assume the header grid is semantically attached to the Sub List beyond layout proximity.

## List Actions

Sub List list actions are stored under the parent Sub List control at `attrs.actions[]`. They use `type = "list"` and step definitions under `steps[]`. Action buttons inside the dynamic item template and footer bind to those Sub List action IDs through `attrs.control_action`. These are list-scoped actions, not page-level `formdef.actions[]`.

Export-proven action names and step types:

| Action label | Step type | Scope |
| --- | --- | --- |
| Add sub item | `list_new` | Current list |
| Import items | `list_import` | Current list |
| Duplicate item | `list_dup` | Current object |
| Delete item | `list_del` | Current object |
| Insert before current item | `list_new` with `attrs.position = "0"` | Current list, positioned relative to current object |
| Insert after current item | `list_new` with `attrs.position = "1"` | Current list, positioned relative to current object |

The product UI also shows Current object actions such as Move item and Update fields. Those step names are product-observed from screenshots, but the studied export only contains `list_new`, `list_import`, `list_dup`, and `list_del`. Keep `list_move` and `list_update` warning-first until an export proves their exact serialized attrs.

## Expression Notes

This export proves list action scoping and row-field bindings inside the dynamic item template. It does not contain a focused expression example that reads current object properties inside a list action expression. Treat current-object expression access as product-understanding-backed until a dedicated export or runtime test proves exact expression tokens.

## Validator Guidance

Validators should check Sub Lists without confusing their list-scoped actions with normal form actions:

- `type = "list"` controls must bind to a list variable whose value resolves to a `variables.listref[]` schema.
- Dynamic content layout should include a `list-body` with child controls.
- Dynamic item field controls should resolve to row fields and keep `attrs.list_field_binding` equal to the parent Sub List binding.
- Summary fields should resolve to row fields. Numeric summary compatibility should remain warning-first where exact runtime support is not proven.
- `attrs.actions[]` should be an array; actions should include step objects; export-proven step types are `list_new`, `list_import`, `list_dup`, and `list_del`.
- Action buttons inside `list-body` or `list-footer` should resolve to `attrs.actions[].id` on the same Sub List rather than `formdef.actions[]`.
- The sibling header Grid plus Dynamic Sub List pattern should be allowed.
- The `.dynamic-list .list-footer` CSS pattern should be preserved when required by layout, but it is not globally required.

## Proof Boundary

Approval Form Sub List dynamic content layout in this export is export-proven.

Sub List list actions in this export are export-proven for Add sub item, Duplicate item, Delete item, Import items, Insert before current item, and Insert after current item, with step types `list_new`, `list_dup`, `list_del`, and `list_import`.

Data List custom form Sub List support is product/user-understanding-backed only in this pass because this export contains Approval Form evidence, not a Data List custom form.

Runtime behavior, add/duplicate/delete/import/move/update execution, scrollbar behavior, and current-object expression evaluation are not runtime-proven by this study.

---

<a id="three-column-dashboard-layout-runtime-css-study"></a>

<!-- source: docs/studies/collection-02.md#three-column-dashboard-layout-runtime-css-study -->

# Three-Column Dashboard Layout Runtime CSS Study

## Scope

This corrected study re-inspected the golden reference from the runtime/layout perspective:

- `/Users/Renger/Downloads/Dashboard Layout Golden Reference.yap`
- `/Users/Renger/Downloads/Dashboard Layout Golden Reference.yapk`
- `/Users/Renger/Downloads/Dashboard-Home.html`

The raw uploaded `.yap`, `.yapk`, and `.html` files were not committed. Findings below are sanitized structural findings only. Raw `Resource`, `Sign`, decoded payloads, tenant/workspace/upload/private identifiers, private URLs, screenshots, and raw API responses were not persisted in the repository.

The optional live runtime URL was not used as source evidence in this branch because the exported HTML already contains the runtime DOM/CSS evidence needed for the layout study, and live browser access would require an authenticated tenant context. No live API calls were run.

## Why The First Study Was Insufficient

The first study correctly identified the conceptual three-panel layout, but it treated `three_column_workspace_shell` mostly as semantic structure:

- left context panel
- main content panel
- right detail panel
- header/body/bottom regions
- no placeholder-only final panels

That was not enough. The first Service Desk Inbox runtime generation test placed meaningful content into three groups, but the generated dashboard rendered vertically/sequentially rather than as a real three-column workspace. API install success only proved package acceptance; it did not prove browser layout.

The missing learning was the Yeeflow container mechanics that make the layout render side by side.

## Package Structure Findings

Both reference packages contain the same visible layout tree for the Dashboard page and the requester-style approval page.

The `.yap` package is a YAP wrapper with gzip/base64 `Resource`, serialized `Data`, one Type `103` dashboard layout, one deployed approval form, no child lists, and `PortalInfo`/`SimplePortal` empty/null behavior.

The `.yapk` package is an `AppExportPackageInfo` wrapper with Brotli/base64 `Resource`, current `AppPackageInfo` shape, one Type `103` Dashboard page, one deployed form, `FormReports: []`, `FormNewReports: []`, `PortalInfo: null`, and no `Childs`.

The YAPK nested dashboard/form resources use current encoded resource behavior where some nested values may be `base64("::brotli::" + Brotli(JSON))`, not only plain JSON strings.

## Runtime HTML Findings

The exported `Dashboard-Home.html` contains:

- one occurrence each of `App name`, `Main content page`, `Detail information panel`, and each bottom-panel label
- three occurrences of designer placeholder text `Drag to add controls here`
- runtime class families such as `ak-form`, `comp-wrapper`, `akc_container`, `form-container`, `akc_heading`, `akc_icon`, and generated `ak-dynamic-*` CSS classes
- CSS rules generated from Yeeflow container attrs rather than extensive inline styles

Key runtime CSS evidence from the exported HTML:

- page/root area: `height:100%`, `flex:1 0 auto`, zero padding in the page container
- shell: `position:absolute`, `left:0`, `right:0`, `top:0`, `bottom:0`, padding from `--sp--s075`
- shell layout: `display:flex`, `flex-direction:row`, gap from `--sp--s100`
- left panel: `display:flex`, `flex-direction:column`, `overflow:hidden`, `width:360px`, `max-width:100%`, `height:100%`
- main panel: `display:flex`, `flex-direction:column`, `overflow:hidden`, `height:100%` with fill-width behavior from package `widthtype = 0`
- right panel: `display:flex`, `flex-direction:column`, `overflow:hidden`, `width:480px`, `max-width:100%`, `height:100%`
- panel headers: `position:sticky`, `top:0`, `z-index:2`, row flex, center alignment, `justify-content:space-between`, min height around `52px`
- panel bodies: column flex, `overflow:scroll`, `height:100%`, padded content
- bottom regions: row flex, center alignment, top border separator, padded content

## Golden Reference Layout Anatomy

### Page And Root

Dashboard host:

- Type `103`
- `LayoutView = null`
- `Ext2 = "{\"src\":true}"`
- one `LayoutInResources[]` entry with the embedded page resource
- embedded resource has `children`, `attrs`, `title`, `ver`, `filterVars`, `tempVars`, and `exts`

Approval requester host:

- one `pageurls[]` requester page
- outer `type = 1`
- outer `pagetype = 1`
- embedded `formdef.pagetype = 1`

Workflow task rules remain separate and unchanged:

- task page outer `pageurls[].type = 2`
- task page outer `pageurls[].pagetype = 1`
- embedded task `pageurls[].formdef.pagetype = 2`

### Main Workspace Shell

The shell is the first visible container under the page/form resource:

- `attrs.common.pos = [null, "absolute"]`
- horizontal and vertical placement are centered in the reference
- `attrs.common.padding` uses `--sp--s075`
- `attrs.common.background` uses a neutral-light hover token
- `attrs.style.direction = [null, "row"]`
- `attrs.style.gap = [null, "--sp--s100"]`
- exactly three direct child panel containers

This is the critical property that the first generated smoke package missed. Without row/flex mechanics or equivalent side-by-side positioning, Yeeflow can render the groups as sequential vertical content.

### Left Context Panel

Observed package properties:

- fixed width behavior: `widthtype = "3"`
- width around `360`
- full-height behavior: `height = "2"`, `cushei = 100`, `cusheiu = "%"`
- `justify_content = "flex-start"`
- `overflow = "hidden"`
- bounded panel styling: white translucent background, border/radius/shadow
- three children: header, scroll body, bottom region

Runtime CSS confirms `width:360px`, `height:100%`, `overflow:hidden`, and column flex.

### Main Content Panel

Observed package properties:

- fill/remaining width behavior: `widthtype = "0"`
- full-height behavior matching side panels
- `overflow = "hidden"`
- bounded panel styling
- three children: header/action area, scroll body, bottom region

Runtime CSS confirms column flex, `height:100%`, and hidden overflow. The package `widthtype = 0` is the important fill-width signal.

### Right Detail Panel

Observed package properties:

- fixed width behavior: `widthtype = "3"`
- width around `480`
- full-height behavior matching the left panel
- `overflow = "hidden"`
- left separator border plus bounded panel styling
- three children: header/action area, scroll body, bottom region

Runtime CSS confirms `width:480px`, `height:100%`, `overflow:hidden`, and column flex.

### Header And Action Areas

Panel headers use:

- sticky positioning
- `top = 0` behavior through sticky target
- `zidx = 2`
- row direction
- center alignment
- `justify_content = "space-between"`
- min height around `52`
- padding using `--sp--s100` / `--sp--s200`
- bottom border separator

Main and right headers include action/icon containers with small fixed widths/heights and center alignment. Active generated actions must have valid bindings; placeholder action containers are not enough in generated-final apps.

### Body And Bottom Regions

Body containers use:

- `height = "2"`
- `cushei = 100`
- `cusheiu = "%"`
- `overflow = "scroll"`
- padding around `--sp--s200`

Bottom containers use:

- row direction
- center alignment
- padding around `--sp--s100` / `--sp--s200`
- top border separator
- neutral background

The reference body text is designer placeholder text. Generated-final pages must replace it with useful business controls.

## Dashboard Vs Approval Form Comparison

| Area | Dashboard page | Approval requester page | Reusable rule |
|---|---|---|---|
| Host | Type `103` page resource | `DefResource.pageurls[].formdef` | Host wrappers differ, layout tree is shared |
| Shell | absolute, row-direction container | same | Do not generate three sequential sections |
| Left panel | fixed width, full height, hidden overflow | same | Side panels need explicit fixed width |
| Main panel | fill-width, full height, hidden overflow | same | Main must occupy remaining middle space |
| Right panel | fixed width, full height, hidden overflow | same | Right must be sibling, not below main |
| Headers | sticky row headers | same | Header/action areas stay visible in scrolling panels |
| Bodies | scrollable body containers | same | Content scrolls inside panels |
| Bottom regions | separator footer containers | same | Include only meaningful generated content |
| Runtime placeholders | present in reference | same | Forbidden in generated-final packages |

## Correct Generation Rules

- Use `three_column_workspace_shell` only when the generator can reproduce the shell mechanics.
- For dashboard pages, use Style 2 root layout: create `three_column_workspace_shell` as the root page body layout container.
- Do not nest `three_column_workspace_shell` inside default `Main > Content` wrappers.
- Set dashboard page content width to Full Width and page padding to `0`.
- Create one shell container with explicit side-by-side layout, preferably the export-proven absolute row shell.
- Add exactly three direct panel containers under the shell for desktop layout.
- Use fixed-width left/right panels and fill-width main panel.
- Use full-height panel behavior and hidden outer overflow.
- Use scrollable body regions inside every panel.
- Use sticky header/action regions inside panels.
- Replace every designer placeholder with meaningful business content.
- Use safe static/sample detail content when selected-record binding is not proven, but keep the layout mechanics correct.
- Do not claim conformance if the output only contains three semantic sections stacked vertically.
- Treat install/API success as package acceptance only; browser/runtime verification must confirm side-by-side panels.

## Runtime Test Feedback: Shell Must Be Root Layout Container

The first corrected Service Desk Inbox runtime smoke package installed successfully and the generated Navigator tree showed a recognizable `three_column_workspace_shell` with left, main, and right panel children. However, the dashboard still rendered incorrectly: panels were clipped/distorted, text wrapped aggressively in narrow header areas, and the page did not behave like the golden full-page reference.

The root cause was placement. The generated dashboard embedded `three_column_workspace_shell` under the ordinary `Main > Content` dashboard containers. Those `Main` and `Content` containers have default height and positioning in this generated shape, so the absolute/full-height shell could not occupy the full available dashboard page. This means the earlier rule was incomplete: `Main > Content` is correct for standard dashboard content pages, but it is not the correct host for the three-column workspace shell.

The corrected distinction is:

- Style 1, standard dashboard content layout: dashboard wrapper > `Main` > `Content` > sections/cards/tables.
- Style 2, three-column workspace shell layout: dashboard wrapper > `three_column_workspace_shell` > left/main/right panels.

For Style 2, the shell must be the root page body layout container. The page must use Full Width content and `0` page padding, and the shell itself must own full-width/full-height positioning, row layout, panel widths, hidden outer overflow, scrollable bodies, and sticky/stable headers.

The same runtime test also showed that Icon controls can distort panel headers when they keep large/default sizing or block/full-width behavior. Generated Icon controls inside this layout should use inline width behavior, and icon size must be intentionally selected for the surrounding header or action area. Narrow panels should not allow icons to force overflow, wrapping, or oversized header rows.

## Validator Changes

The generated app quality inspector now hardens `three_column_workspace_shell` checks when a checklist/template declares the pattern.

New or now-emitted error codes include:

- `THREE_COLUMN_SHELL_MISSING`
- `THREE_COLUMN_SHELL_NOT_ROOT`
- `THREE_COLUMN_SHELL_NESTED_IN_MAIN_CONTENT`
- `THREE_COLUMN_PARENT_HEIGHT_DEFAULT_RISK`
- `THREE_COLUMN_PAGE_WIDTH_NOT_FULL`
- `THREE_COLUMN_PAGE_PADDING_NOT_ZERO`
- `THREE_COLUMN_ICON_WIDTH_NOT_INLINE`
- `THREE_COLUMN_ICON_SIZE_LAYOUT_RISK`
- `THREE_COLUMN_PANEL_MISSING`
- `THREE_COLUMN_PANEL_WIDTH_MISSING`
- `THREE_COLUMN_PANEL_POSITIONING_MISSING`
- `THREE_COLUMN_PANELS_STACKED_VERTICALLY`
- `THREE_COLUMN_OVERFLOW_MISSING`
- `THREE_COLUMN_PLACEHOLDER_PANEL`
- `THREE_COLUMN_RUNTIME_LAYOUT_RISK`

The validator remains optional. It applies when a page declares the three-column template or layout rule; it is not a global dashboard requirement.

## Template Behavior

The `three_column_workspace_shell` template now includes `three_column_reference_mechanics`, root page body placement, Full Width page content, zero page padding, and required layout properties for the shell, panels, headers, bodies, icons, and bottom regions. Template conformance can no longer pass on labels alone or on a shell nested inside `Main > Content`.

## Next Runtime Test Plan

Rerun the Service Desk Inbox smoke generation, but require the generated YAPK to pass `three_column_workspace_shell` property validation before signing or install.

Suggested prompt:

```text
Generate a YAPK-only Three Column Service Desk Inbox smoke app using three_column_workspace_shell.
Before signing or install, validate that the dashboard shell passes the three_column_workspace_shell property validator:
root page body shell placement, no Main > Content wrapper, Full Width page content, 0 page padding, left/right fixed-width panels, fill-width main panel, row shell, full-height panels, hidden outer overflow, scrollable bodies, sticky headers, inline/bounded icons, meaningful content, and no placeholder text.
Stop and repair locally if the validator reports THREE_COLUMN_* errors.
```

---

<a id="timeline-controls-dynamic-controls"></a>

<!-- source: docs/studies/collection-02.md#timeline-controls-dynamic-controls -->

# Timeline Controls And Dynamic Controls Study

## Feature Overview

This study covers the Yeeflow Vertical Timeline and Horizontal Timeline controls from `Company Overview (1).yap`, focused on dashboard usage with Dynamic controls inside timeline item templates.

Reference product docs:

- [Vertical timeline](https://<yourdomain>.yeeflow.com/<redacted-path>
- [Horizontal timeline](https://<yourdomain>.yeeflow.com/<redacted-path>

The product docs describe both controls as Advanced controls that can be used on dashboards, approval forms, and data-list list forms. They are data-source-backed, support filtering and ordering, let designers place controls into repeated timeline item blocks, and offer timeline/card/point/line styling. The export studied here proves the dashboard control schema only.

## Export Source

- Export: `/Users/Renger/Downloads/Company Overview (1).yap`
- App title: `Company Overview`
- Target dashboard: `Timeline with controls`
- Comparison dashboards from the prior study: `Company overview`, `Collection of activity`
- Target data source: Data List `Company Overview`

The export contains one Data List named `Company Overview`, three dashboard pages, and two Data List custom forms (`View page`, `Edit item`). The updated dashboard adds one Vertical Timeline, one Horizontal Timeline, and one Collection control on the same page.

## Data Source

Both timeline controls and the same-page Collection bind to the Data List `Company Overview` through `attrs.data.list`.

Relevant fields observed:

| Field | Display Name | Control Type | Field Type | Usage |
| --- | --- | --- | --- | --- |
| `Title` | Name | `input` | Text | Vertical title expression and Dynamic field |
| `Datetime1` | Start date | `datepicker` | Datetime | Sort/date field and Horizontal title expression |
| `Datetime2` | Due date | `datepicker` | Datetime | Available timeline/date field, not observed as timeline binding |
| `Text3` | Stakeholders | `checkbox` | Text | Dynamic field |
| `Text4` | RAG status | `radio` | Text | Dynamic field |
| `Text8` | Reference | `file-upload-merge` | Text | Dynamic file, multiple files |
| `Text10` | Assignee | `identity-picker` | Text | Dynamic user, multiple users |
| `Text11` | Cover image | `icon-upload` | Text | Dynamic image |

The wrapper has no committed sample record payload in this study. Field shapes are safe schema evidence only.

## Vertical Timeline

Observed control type: `timeline-v`

Observed configuration:

- Data source: `attrs.data.list` resolves to Data List `Company Overview`
- Timeline title/content label: `attrs.data.title.variable[]` uses `exprType = "variable_ctx"`, `ctx = "__ctx_coll"`, `id = "Title"`
- Sorting/date order: `attrs.data.sort[]` includes `SortName = "Datetime1"` with descending order
- Point/icon setting: `attrs.data.icon`
- Item click target: `attrs.data.link` references a custom list form layout
- Open sizing: `attrs.data.modalsize = 0`
- Animation: `attrs.data.animate = true`
- Horizontal/vertical alignment: `attrs.data.align`, `attrs.data.va`
- Style: `attrs.title.ty`
- Item template: child `container` holding Dynamic image, Dynamic field, Dynamic user, and Dynamic file controls

The export uses `__ctx_coll` as the repeated item context even inside the timeline. Do not invent a separate context unless a future export proves it.

## Horizontal Timeline

Observed control type: `timeline-h`

Observed configuration:

- Data source: `attrs.data.list` resolves to Data List `Company Overview`
- Timeline title/date label: `attrs.data.title.variable[]` uses `exprType = "variable_ctx"`, `ctx = "__ctx_coll"`, `id = "Datetime1"`
- Sorting/date order: `attrs.data.sort[]` includes `SortName = "Datetime1"` with descending order
- Point/icon settings: `attrs.data.icon`, `attrs.data.ptype`, `attrs.point.size`
- Columns: `attrs.data.col = [null, 4]`
- Item click target/opening: `attrs.data.link`, `attrs.data.op = "modal"`, `attrs.data.modalsize = 0`
- Sliding cards: `attrs.data.cardarr = true`, `attrs.data.slides = "4"`, `attrs.arrow`, `attrs.card.arrow`
- Alignment: `attrs.data.align`, `attrs.data.va`, `attrs.card["align-i"]`
- Item template: child `container` holding the same Dynamic controls used by the Vertical Timeline

Horizontal Timeline uses the same source list and Dynamic control item-template schema as Vertical Timeline, but adds horizontal/card-specific options such as columns, card arrows, slides-to-scroll, and arrow styling.

## Dynamic Controls In Timeline Templates

Timeline item templates use the same current-item binding pattern previously observed in Collection and Kanban:

```json
{
  "type": "dynamic-field",
  "attrs": {
    "source": "3",
    "obj-f": "Title"
  }
}
```

Observed Dynamic controls inside both Vertical and Horizontal Timeline templates:

- Dynamic image: `type = "dynamic-image"`, `attrs.source = "3"`, `attrs["obj-f"] = "Text11"` (`Cover image`, `icon-upload`)
- Dynamic field: `type = "dynamic-field"`, bound to `Title`, `Text3`, and `Text4`
- Dynamic user: `type = "dynamic-user"`, bound to `Text10` (`Assignee`, `identity-picker`, multiple users)
- Dynamic file: `type = "dynamic-file"`, bound to `Text8` (`Reference`, `file-upload-merge`, multiple files)

Observed user settings include avatar/name style, `i-len`, and `addition_fields = ["Email"]`. Observed image settings include preview/fit/image-height styling. Observed file settings include item limit, content sizing, icon size, padding, border/background, and optional `type_icon_show`.

## Same-Page Collection Comparison

The `Timeline with controls` dashboard also includes a Collection control named `Active survey program collection`, bound to the same `Company Overview` list. It uses the same item-template Dynamic control family and `attrs.source = "3"` / `attrs["obj-f"]` field binding pattern.

Comparison:

| Host | Main Structure | Current Item Binding | Main Use |
| --- | --- | --- | --- |
| Vertical Timeline | `timeline-v` with timeline title/date/order settings and item template children | `attrs.source = "3"` and `__ctx_coll` expressions | Chronological feeds, histories, lifecycle logs, milestones |
| Horizontal Timeline | `timeline-h` with title/date/order plus columns/arrows/slides settings | `attrs.source = "3"` and `__ctx_coll` expressions | Roadmaps, schedules, phases, campaign plans |
| Collection | `collection` with data source and layout settings | `attrs.source = "3"` | Flexible card/list display when time axis is secondary |
| Kanban | `kanban` with data source and `attrs.data.cateField` grouping | `attrs.source = "3"` | Status/stage/category work management |

## Business Use Guidance

Product-doc-backed:

- Use Vertical Timeline for key information in chronological order, such as milestones, project phases, event sequences, and product-development history.
- Use Horizontal Timeline for time-series progression, such as project progress, event planning, product stages, roadmaps, and schedules.
- Both controls support dashboards, approval forms, and data-list list forms according to product docs. This export proves dashboard usage only.

Export-proven:

- Dashboard Vertical Timeline and Horizontal Timeline controls can bind to a Data List source.
- Timeline item templates can use Dynamic field, Dynamic user, Dynamic image, and Dynamic file controls.
- Timeline Dynamic controls use the same `attrs.source = "3"` current-item context pattern as Collection/Kanban in this export.

Inferred generation guidance:

- Prefer Vertical Timeline when users scan events from top to bottom, such as audit trails, lifecycle history, approval history, or activity feeds.
- Prefer Horizontal Timeline when the page needs a left-to-right schedule or phase progression, such as release plans, project roadmaps, marketing campaigns, or lifecycle stages.
- Prefer Collection when chronological order is not the dominant structure.
- Prefer Kanban when users manage work by status/stage/category.

## Validation Guidance

Generated packages should validate:

- `timeline-v` and `timeline-h` controls resolve `attrs.data.list.ListID` to an included list-like source.
- Timeline date/order/title fields resolve to source list fields.
- Dynamic controls inside timeline item templates use `attrs.source = "3"` and bind through `attrs["obj-f"]`.
- Dynamic user binds to identity/person fields.
- Dynamic image binds to image/icon-upload fields.
- Dynamic file binds to attachment/file fields.
- Dynamic field binds to general fields.
- Historical exports should receive warnings for uncertain variants; generated-final packages can hard-error clear missing data sources, unresolved fields, and type mismatches.

## Proof Boundary

- Vertical Timeline usage on `Timeline with controls` is export-proven.
- Horizontal Timeline usage on `Timeline with controls` is export-proven.
- Dynamic controls inside timeline templates are export-proven where observed.
- Reference docs provide product guidance for when/how to use vertical and horizontal timelines.
- Runtime timeline rendering, scrolling, click behavior, dynamic file/image preview behavior, and drag/drop/reordering if any are not proven unless later tested.
- Kanban/Collection learning remains export-proven from the previous branch.
- Data List form runtime behavior is not proven by this branch.

## Follow-Up

Recommended next step: generate a focused runtime proof package with one Data List, one dashboard Vertical Timeline, one dashboard Horizontal Timeline, representative Dynamic controls, and safe sample rows, then manually verify rendering, click behavior, image/file display, and timeline navigation.

---

<a id="workflow-ai-assistant-image-input"></a>

<!-- source: docs/studies/collection-02.md#workflow-ai-assistant-image-input -->

# Workflow AI Assistant Image Input

Source export: `<downloads>/Spark & AI (1).yap`

Classification: export-proven image-input mapping structure.

## Proven Pattern

The `Extract&Update In-Stock` workflow proves that the AI Assistant workflow node can pass an image/file-like list field into an Agent input variable typed as `img`.

Observed node pattern:

- workflow node: `stencil.id = "AI"`
- node mode: `properties.type = "agent"`
- Agent input variable entry:
  - `id = "label_image"`
  - `type = "img"`
  - `description` present
  - `value.type = 1`
  - `value.value.exprType = "list_field"`

## Source Field Binding

The mapped source field is the `Stock Box` field:

- display name: `Image File`
- internal name: `ImageFile`
- field slot: `Text9`
- control type: `icon-upload`
- field type: `Text`

Observed image mapping expression:

```json
{
  "type": 1,
  "value": {
    "exprType": "list_field",
    "valueType": "icon-upload",
    "prop": "Text9",
    "id": "ImageFile",
    "type": "expr"
  }
}
```

## What This Proves

- Image-capable Agent inputs are not restricted to Copilot chat contexts.
- Data-list workflows can bind a current-row upload control directly into an Agent `img` input.
- The workflow mapping uses the list-field expression system, not a file URL literal.
- The value-type discriminator on the expression preserves the UI/storage shape of the source control.

## What Remains Unproven

- The exact persisted binary/storage payload format behind `icon-upload`.
- Whether `file-upload` and `icon-upload` are interchangeable for all Agent image-input flows.
- Whether multiple-image inputs use the same mapping shape.
- Whether runtime image execution succeeds in every tenant without additional model or feature flags.

## Validation Guidance

- Require `properties.inputVariables` to be an array.
- When an input variable has `type = "img"`, require a structured `value` object.
- Accept export-proven list-field image mappings with `valueType = "icon-upload"` or `valueType = "file-upload"`.
- Treat live execution as runtime-sensitive even when the structure validates.

---

<a id="workflow-approval-vs-data-list-actions"></a>

<!-- source: docs/studies/collection-02.md#workflow-approval-vs-data-list-actions -->

# Workflow Actions: Approval Form vs Data List

## Purpose

This study compares export-proven Start action and Assignment Task action shapes between approval form workflows, data-list workflows, and the later quick Scheduled Workflow export study.

Source exports:

- Approval workflow baseline: `<downloads>/Test ABC.yap`
- Approval workflow extended settings: `<downloads>/Test ABC (1).yap`, `<downloads>/Test ABC (2).yap`, `<downloads>/Test ABC (3).yap`
- Data-list workflow: `<downloads>/Purchase Requests.ydl`
- Scheduled workflow comparison: `<downloads>/Workflow Actions Runtime Baseline (1).yap`

The data-list export was decoded read-only. Raw `.ydl`, decoded payloads, private IDs, user names, email addresses, tenant IDs, and raw response data are not committed.

References:

- Yeeflow Help Center: Assignment Task Action, https://<yourdomain>.yeeflow.com/<redacted-path>
- Yeeflow Help Center: Add assignee with Assignee Editor, https://<yourdomain>.yeeflow.com/<redacted-path>
- Yeeflow Help Center: Start Action, https://<yourdomain>.yeeflow.com/<redacted-path>

## Data-List Workflow Inventory

| Item | Export-proven finding | Proof level |
|---|---|---|
| Export | `Purchase Requests.ydl` | export-proven |
| Data list | One data list with 14 fields; list and field IDs redacted | export-proven |
| Workflow | One workflow with `WorkflowType: 1` and one `FlowMappings[]` entry | export-proven |
| Trigger | `FlowMappings[].Setting.NewTrigger: true` with `FieldName: null` | export-proven |
| Start action | One `StartNoneEvent`, zero incoming flows, one outgoing flow | export-proven + validator-backed |
| Assignment Task | One `MultiAssignmentTask` with four assignee entries | export-proven + validator-backed |
| Task form | One task form with list-bound controls, normal controls, action panel, and history | export-proven |

## Start Action Comparison

| Capability | Approval form workflow | Data-list workflow | Export-proven field/shape | Match/difference | Proof level |
|---|---|---|---|---|---|
| Start node | `StartNoneEvent` | `StartNoneEvent` | Same stencil ID | matched | export-proven |
| Incoming/outgoing | No incoming; outgoing sequence flows | No incoming; one outgoing sequence flow | `incoming: []`, `outgoing[]` | matched | export-proven + validator-backed |
| Allow terminate | Present in `Test ABC (3).yap` as `terminate` and `terminate-conditions` | Not present in `Purchase Requests.ydl` Start properties | fields absent in data-list Start | difference | export-proven |
| Allow recall | `revoke-conditions` present in `Test ABC (3).yap` | Not present in data-list Start properties | field absent in data-list Start | difference | export-proven |
| Email notification | `isenabledemail`, `to`, `subject`, `html` | Same email field family present | `isenabledemail: true`, expression-button recipient/subject/body shapes | matched | product-documented + export-proven |
| Conditions | Terminate/recall condition rows in approval form workflow | No Start condition fields found in data-list export | not found in data-list export | difference | export-proven |
| Request context | Applicant/request context in expression labels | Applicant/submitter labels still appear in notification expressions | expression labels only, values redacted | partially matched | export-proven |
| List item context | Not applicable in approval form exports | Data-list workflow has list item trigger and list field context in assignment expressions | `FlowMappings[].Setting.NewTrigger` and list-item expression source | export-only difference | export-proven |

## Assignment Task Comparison

| Capability | Approval form workflow | Data-list workflow | Export-proven field/shape | Match/difference | Proof level |
|---|---|---|---|---|---|
| Action stencil | `MultiAssignmentTask` | `MultiAssignmentTask` | Same stencil ID | matched | export-proven |
| Assignee editor array | `properties.usertaskassignment[]` | `properties.usertaskassignment[]` | Same property family | matched | export-proven + validator-backed |
| Static users | Direct user entries found | One direct user entry found | `type: user`, `method: direct`, redacted value | matched | export-proven + API-category-assisted from prior study |
| Job positions | Direct/by department/by location found in approval exports | No job-position assignee found in this data-list export | not found in data-list export | approval-only in current source | export-proven |
| Departments/locations | Present through position routing in approval exports | Not found as assignment sources in this data-list export | not found | approval-only in current source | export-proven |
| Expression editor | Applicant, department, position, user group expressions found | Applicant line manager, user group, and list-item expressions found | expression-button `value` strings | matched with extra data-list context | export-proven |
| List field values | Not found in approval form exports | Found as list-item expression context | `type: listitem` inside assignee expression | data-list only | export-proven |
| Created By field assignee | Not found in approval form exports | Found as Created By line-manager expression | list-item `CreatedBy` with `LineManager` property | data-list only | export-proven |
| Task type | Approval/default by absent `tasktype`; Complete via `tasktype="complete"` in `Test ABC (2).yap` | `tasktype` absent, matching approval/default shape | absent `tasktype` | matched default | export-proven |
| Appointed order | `issequential=true` or absent/default | Not found in this data-list assignment task | absent `issequential` | not found in data-list source | export-proven |
| Due date | `duedatedefinition`, `duedatetype`, `duedateexpress`, reminders | `duedatedefinition: 120`, `duedatetype: hour`, one `notifyrules[]` entry | same field family | matched | export-proven + validator-backed |
| Email notification | `isenabledemail`, `to`, `subject`, `html`, `notifyrules` | Same field family present | current task assignee email expression; subject/body redacted | matched | product-documented + export-proven |
| Task form | Approval form task forms use workflow controls and normal controls | Data-list task form mixes normal controls with list-bound controls | `pageurls[].formdef` with `isListControl` controls | data-list extension | export-proven |

## Task Form Comparison

| Capability | Approval form workflow | Data-list workflow | Export-proven field/shape | Match/difference | Proof level |
|---|---|---|---|---|---|
| Normal controls | Present in approval task forms | Present in data-list task form | normal controls bound to workflow variables | matched | export-proven |
| List field controls | Not applicable in approval form exports | Present | `isListControl: true` | data-list only | export-proven |
| Custom list fields | Not applicable in approval form exports | Present | `identifier` such as a custom `Text*` field, binding prefix `____customListFields_` | data-list only | export-proven |
| Default/native fields | Not applicable in approval form exports | Present for Created By and Title-style list fields | `identifier: CreatedBy`, native field binding shape | data-list only | export-proven |
| Read-only custom fields | Not applicable in approval form exports | Present | custom list field controls with `readonly: true` | data-list only | export-proven |
| Default/native read-only behavior | Not applicable in approval form exports | Created By appears read-only in studied export | `attrs.readonly: true` | data-list only; do not overgeneralize | export-proven, runtime-unproven |
| Workflow controls | Action panel/history found in task forms | Action panel/history found in task form | `workflowControlPanel`, `workflowHistory` | matched | export-proven |

## Help Center Comparison

| Help Center concept | Export-proven field/shape | Match status | Notes | Proof level |
|---|---|---|---|---|
| Start action initiates workflow and can send email | Data-list Start has no incoming flow and has `isenabledemail/to/subject/html` | matched | Product article describes Start email generally; data-list export confirms email fields in a data-list workflow. | product-documented + export-proven |
| Start allow terminate / allow recall | Approval export has terminate/recall condition fields; data-list export does not | partially matched | Help article describes these for Start action generally; data-list export did not include them. | product-documented + export-proven |
| Assignment Task can assign to users, groups, positions, expressions | Data-list export has direct user, applicant line manager, user group, and list-item expressions | matched | Position shapes were proven in approval exports, not this `.ydl`. | product-documented + export-proven |
| Workflow variable / data-list value as assignee | Data-list export uses list-item Created By context in assignee expression | matched | Help Center discusses variable/data-list-driven users; export proves one serialized list-item shape. | product-documented + export-proven |
| Task forms are associated with assignment tasks | Data-list assignment task references a task form in `pageurls[]` | matched | Data-list task form additionally includes list-bound controls. | product-documented + export-proven |

## Generation Rules

- Keep approval form workflow and data-list workflow Start action rules separate.
- For data-list workflow Start actions, do not invent `terminate`, `terminate-conditions`, or `revoke-conditions` unless a data-list export proves them.
- Preserve Start email notification fields together when present: `isenabledemail`, `to`, `subject`, and `html`.
- Preserve data-list workflow `FlowMappings[]` trigger settings and workflow `ListID`/`WorkflowType` linkage.
- Preserve Assignment Task assignee list values as expression-button strings; do not convert list-item expressions into static IDs.
- For Created By assignee routing, preserve the list-item `CreatedBy` source and selected property, such as `LineManager`, as export-proven.
- Do not invent user, group, position, department, location, list, or field IDs. Use safe lookup or explicit tenant-local mapping only when authorized.
- Preserve data-list task form list controls with `isListControl: true`, `identifier`, `InternalName`, `fieldID`, and `____customListFields_` binding.
- Set custom list fields read-only when the task should display source data without updating it.
- Treat default/native list field editability as export-proven only for the fields observed here; runtime/designer proof is still needed before broad claims.
- Lay out generated workflow nodes without overlap and keep sequence-flow source/target plus incoming/outgoing references consistent.

## Validator Recommendations

- Warn, not hard-error in compatibility mode, when a data-list workflow task form list-field control references an unknown field.
- Warn when a list-field binding does not use the studied `____customListFields_` prefix.
- Warn when a native/default list field is explicitly editable; studied native/default examples were read-only.
- Warn when a custom list field is editable unless generation explicitly wants the task to update that source field.
- Warn that list-item assignee expressions are export-proven but runtime routing still needs a focused baseline.

## Runtime-Test Additions

Future focused runtime baselines should add data-list workflow designer/open proof for:

- data-list Start action email notification configuration
- Assignment Task assignee from Created By/list item context
- data-list task form list-bound controls
- read-only custom list fields
- default/native list fields in task forms

Runtime submit/routing proof should use disposable list records and safe assignee references only. Do not test email delivery unless safe recipients and delivery scope are explicitly approved.

## Combined Baseline Coverage

`tools/generators/generate-workflow-actions-combined-runtime-baseline.mjs` builds a single ignored package with one approval form and one data list so the two workflow families can be checked in one Yeeflow app:

- approval-form workflow: Start terminate/recall/email fields plus representative Assignment Task assignee, task type, appointed order, due-date, reminder, and notification settings
- data-list workflow: Start email settings without terminate/recall fields, Created By/list-field assignee expression, and a task form with list-bound controls

The combined package is for import/open/designer/publish proof first. It should not be used to claim routing, email delivery, due-date reminder delivery, or data-list task form save behavior until those paths are explicitly submitted and observed with safe test data.

The combined package was imported and tested in Chrome. The approval workflow designer opened with a non-overlapping graph, the approval Start panel rendered terminate/recall/condition/email settings, representative Assignment Task panels rendered assignee/task-type/appointed-order controls, and the approval workflow published successfully. After read-only API-assisted safety checks confirmed the first approval Assignment Task was a single direct-user target, one fake approval request was submitted and a new Pending task appeared for `Workflow Action Approval Test` with task label `Static User Assignment`. This submit proof is limited to the first approval direct-user task.

The data-list workflow designer opened with a Start -> Assignment Task -> End graph, rendered data-list Start email settings without terminate/recall controls, rendered the data-list Assignment Task mixed assignee/list-field expression configuration, and published successfully. Data-list item submission was skipped because the first data-list Assignment Task has mixed direct, expression, user-group, and list-item/Created By sources. Data-list routing, group/position/list-field expansion, task-form save/edit behavior, due-date reminders, and email delivery were not tested.

## Known Gaps

- Data-list Start terminate/recall fields were not found in this export.
- Data-list Start email delivery was not tested.
- Created By/list-field assignee routing was not runtime-tested.
- Approval direct static-user routing is runtime-proven only for the first submitted task in the combined baseline.
- Data-list task form save/edit behavior for list-bound controls was not runtime-tested.
- Default/native list field read-only behavior is based only on the studied export shape.

## Claim Task Addendum

`Workflow Actions Runtime Baseline (3)_Claim task.yap` adds Claim Task comparison coverage.

| Capability | Approval form workflow | Data-list workflow | Proof level |
|---|---|---|---|
| Internal action type | `CandidateTask` | `CandidateTask` | export-proven + config-reference-backed |
| Receiver/candidate field | `properties.usertaskassignment[]` | `properties.usertaskassignment[]` | export-proven |
| Task form association | `properties.taskurl` resolves to task pages such as `WARTB Task3`/`WARTB Task4` | `properties.taskurl` resolves to data-list task page `Approval task` | export-proven |
| Task type | `approve` and `complete` | `approve` and `complete` | export-proven |
| Receiver sources | user-group expression | direct user, applicant line manager, Created By line manager | export-proven |
| Data-list field context | not found | `listitem` / `CreatedBy` expression found | export-proven |
| Email config | fields present but disabled in studied Claim Tasks | `isenabledemail=true` with `to`, `subject`, and `html` | export-proven |
| Claim ownership behavior | product-documented only | product-documented only | not runtime-proven |

Use Claim Task for candidate/receiver pool ownership. Use Assignment Task for direct assignee ownership. Do not claim group/list-field claim routing or claim locking until a focused runtime baseline proves it.

## Scheduled Workflow Addendum

`Workflow Actions Runtime Baseline (1).yap` adds one Scheduled Workflow comparison point. See `docs/studies/collection-03.md#workflow-scheduled-vs-approval-data-list-actions` for the focused study.

| Capability | Approval form workflow | Data-list workflow | Scheduled workflow | Proof level |
|---|---|---|---|---|
| Workflow discriminator | `WorkflowType = 2` | `WorkflowType = 1` | `WorkflowType = 3`, `ListID = 0` | export-proven |
| Start node | `StartNoneEvent` | `StartNoneEvent` | `StartNoneEvent` | export-proven |
| Start terminate/recall | present in approval export | absent in data-list export | absent in scheduled export | export-proven |
| Start email config | present | present | present | product-documented + export-proven |
| Assignment Task family | `MultiAssignmentTask` | `MultiAssignmentTask` | `MultiAssignmentTask` | export-proven + validator-backed |
| Scheduled assignee source | approval/applicant context | list-item context available in data-list export | applicant line-manager expression only in scheduled export | export-proven |
| Data-list field expression source | not found | found | not found in scheduled export | export-proven |

Generation rule: do not copy approval-form terminate/recall fields or data-list field assignee sources into Scheduled Workflow generation unless a Scheduled Workflow export proves those fields. Scheduled Workflow Assignment Task routing remains untested.

## Set Variable Addendum

`Workflow Actions Runtime Baseline (4)_Set variable.yap` adds Set variable comparison coverage.

| Capability | Approval form workflow | Data-list workflow | Proof level |
|---|---|---|---|
| Internal action type | `SetVariableTask` | `SetVariableTask` | export-proven + config-reference-backed |
| Current workflow variable target | `formtype="current"` found | `formtype="current"` found | export-proven |
| Another approval workflow target | `formtype="custom"` with target app/listset/proc key and form id found | same target metadata shape found | export-proven |
| Multiple variables in one action | 3 `variablesetting[]` rows found | 4 `variablesetting[]` rows found on another-workflow node | export-proven |
| Data-list field values | not found as right-side values | `exprType="list_field"` right-side values found | export-proven |
| Data-list field targets | not applicable | not found; Set variable targets workflow variables only | export-proven absence |
| Data-list field mutation | use Set data list / `ContentList` | use Set data list / `ContentList` | product-documented |

Do not claim runtime variable mutation, another-workflow updates, or list-field value resolution until a focused runtime baseline proves those paths.

## Set Data List Addendum

`Workflow Actions Runtime Baseline (5)_Set data list.yap` adds Set data list comparison coverage.

| Capability | Approval form workflow | Data-list workflow | Proof level |
|---|---|---|---|
| Internal action type | `ContentList` | `ContentList` | export-proven + config-reference-backed |
| Selected data source | `listtype="select"` found | `listtype="select"` found | export-proven |
| Current list mode | not found | `listtype="current"` found | export-proven |
| Add operation | `type="add"` found | `type="add"` found | export-proven |
| Edit/update operation | `type="edit"` found | `type="edit"` found | export-proven |
| Delete/remove operation | `type="remove"` found | not found in this export | export-proven |
| Field mappings | `listdatas[]` with `Columns`, `Per`, `Data` | same shape | export-proven |
| Filters | `wheres[]` found on edit/remove | `wheres[]` found on edit | export-proven |
| Numeric operation codes | `Per` codes `0`, `1`, `2`, `3`, `4` found | `Per` codes `0`, `1` found | export-proven |
| List-field values | approval variables/application context; approval sub-list variable references | `exprType="list_field"` values, including sub-list valueType `list` | export-proven |
| Document-library target | not found | not found | unproven in this export |

Set data list is the workflow action for data-source mutation. Use it instead of Set variable when the requirement is to add, update, or delete data-list records or fields. Runtime mutation, numeric operation execution, and sub-list row iteration remain not runtime-proven.

## Signal Event Addendum

`Workflow Actions Runtime Baseline (6)_Signal event.yap` adds Signal event comparison coverage.

| Capability | Approval form workflow | Data-list workflow | Scheduled workflow | Proof level |
|---|---|---|---|---|
| Internal action type | `SignalEvent` | not found | not found | export-proven absence outside approval workflow in this export |
| Purpose | recall/terminate event source branch | unproven | unproven | product-documented + export-proven |
| Incoming flow | none | not applicable | not applicable | export-proven |
| Outgoing flow | one flow to downstream cleanup branch | not applicable | not applicable | export-proven |
| Event definitions | `RevokeEventDefinition`, `CancelEventDefinition` | not found | not found | export-proven + config-reference-backed |
| Relationship to Start | pairs with approval Start terminate/recall settings | data-list Start has no terminate/recall fields in studied exports | scheduled Start has no terminate/recall fields in studied exports | export-proven comparison |
| Downstream cleanup | `ContentList` edit with nonempty `wheres[]` | not found | not found | export-proven schema; runtime-unproven |

Use Signal event only as approval-workflow-proven schema for now. It is a separate event source with no incoming line, unlike normal workflow actions. It can connect to Set data list / `ContentList` for compensation or cleanup patterns, but recall/terminate firing and downstream data mutation are not runtime-proven.

---

<a id="workflow-assignment-task-assignee-settings"></a>

<!-- source: docs/studies/collection-02.md#workflow-assignment-task-assignee-settings -->

# Workflow Assignment Task Assignee Settings

## Purpose

This focused export-learning study documents how Yeeflow approval workflow assignment tasks store assignee settings in `Test ABC.yap`, `Test ABC (1).yap`, and later focused exports for task type/due-date settings.

Proof boundary:

- `product-documented`: described in Yeeflow Help Center behavior references.
- `export-proven`: found in `Test ABC.yap` or `Test ABC (1).yap`.
- `API-category-assisted`: static references were classified with safe read-only Yeeflow API Operator lookup.
- `validator-backed`: checked by local validation after this study.
- `runtime-proven`: not claimed in this study.
- `unproven`: not found or not tested.

This branch does not generate a runtime baseline, does not submit approval requests, and does not run live workflow tasks.

## Sources

Source export:

```text
/Users/Renger/Downloads/Test ABC.yap
/Users/Renger/Downloads/Test ABC (1).yap
/Users/Renger/Downloads/Test ABC (2).yap
/Users/rengerhu/Downloads/Workflow actions layout.json
```

Reference:

- Yeeflow Help Center: Assignment task action
  https://<yourdomain>.yeeflow.com/<redacted-path>
- Related Yeeflow Help Center behavior reference: Add assignee with Assignee Editor
  https://<yourdomain>.yeeflow.com/<redacted-path>

Source priority:

1. `Workflow actions layout.json` is the focused golden-reference source for the common business assignee patterns: applicant line manager, applicant department manager, direct Job Position, workflow user variable, workflow user variable line manager, and multiple assignee entries.
2. `Test ABC (2).yap` is the latest Assignment Task source of truth for task type, Complete task, due-date, and reminder settings.
3. `Test ABC (1).yap` is the source of truth for multiple-assignee, Appointed Order, approveway, and email notification settings.
4. `Test ABC.yap` is the previous baseline export for comparison.
5. Help Center articles are product-behavior and terminology references.
6. Yeeflow API Operator lookup is only for safe read-only org/reference category support.
7. Existing validators and skills are implementation references.

## Redaction Policy

The raw export and decoded payloads are not committed. Decoding was performed under ignored `tmp/`.

Normalized references redact private org data with placeholders:

- `<REDACTED_USER_ID>`
- `<REDACTED_USER_EMAIL>`
- `<REDACTED_USER_GROUP_ID>`
- `<REDACTED_DEPARTMENT_ID>`
- `<REDACTED_LOCATION_ID>`
- `<REDACTED_POSITION_ID>`
- `<REDACTED_TENANT_ID>`
- `<REDACTED_WORKFLOW_NODE_ID>`
- `<REDACTED_TASK_FORM_PAGE_ID>`
- `<REDACTED_ASSIGNEE_LABEL>`

Raw user names, emails, phones, tenant IDs, department IDs, location IDs, position IDs, workflow node IDs, and task form page IDs must not be copied into reusable docs or normalized references.

## Located Workflow

`Test ABC.yap` decoded successfully with large numeric IDs preserved as strings.

Export-proven structure:

- Application shell: `Test ABC`
- Forms found: `1`
- Approval workflow forms found: `1`
- Approval form name: `ABC`
- Workflow type: `2`
- Workflow nodes found: `29`
- Assignment task nodes found: `9`
- Assignment task node type: `MultiAssignmentTask`
- Sequence flow nodes found: `18`
- Start/end nodes found: `2`

Each assignment task stores assignees under:

```text
properties.usertaskassignment
```

The value is an array. `Test ABC.yap` used one assignee entry per task. `Test ABC (1).yap` proves multiple assignee entries in the same array, including mixed static/direct users, position sources, expression sources, and a user-group expression source.

Common task fields seen on each `MultiAssignmentTask`:

- `properties.duedatedefinition`
- `properties.approveway`
- `properties.approvepercentage`
- `properties.isenabledemail`
- `properties.isallowreassign`
- `properties.isallowsign`
- `properties.allowskip`
- `properties.name`
- `properties.usertaskassignment`
- `properties.taskurl`

Common approval settings in this export:

- `approveway`: `allapprove`
- `approvepercentage`: `100`
- `isenabledemail`: `false`
- `isallowreassign`: `false`
- `isallowsign`: `false`
- `allowskip`: `true`
- `taskurl`: redacted task form page reference

## Assignment Task Inventory

| Task | Node index | Assignee classification | Export-proven entry shape | Expression source | Static references | Approval mode | Runtime dependencies | Confidence |
|---:|---:|---|---|---|---|---|---|---|
| 1 | 9 | applicant department manager | `type=user`, `method=expression`, `value`, `title` | `Applicant:Department:Manager`; outer expression resolves applicant department then `Manager` | none | `allapprove`, `100` | applicant user context, department hierarchy/manager data, user directory data | export-proven |
| 2 | 11 | direct job position | `type=position`, `method=position`, `position`, `title` | none | `<REDACTED_POSITION_ID>` | `allapprove`, `100` | position assignments | export-proven |
| 3 | 13 | direct job position | `type=position`, `method=position`, `position`, `title` | none | `<REDACTED_POSITION_ID>` | `allapprove`, `100` | position assignments | export-proven |
| 4 | 15 | position by selected department | `type=position`, `method=positionorg`, `position`, `value`, `title` | none | `<REDACTED_POSITION_ID>`, `<REDACTED_DEPARTMENT_ID>` | `allapprove`, `100` | position assignments, department hierarchy | export-proven |
| 5 | 18 | position by applicant department | `type=position`, `method=positionorgexpr`, `position`, `value`, `title` | `Applicant:Department`; expression references applicant user context and `OrganizationID` | `<REDACTED_POSITION_ID>` | `allapprove`, `100` | applicant user context, position assignments, department hierarchy | export-proven |
| 6 | 20 | position by selected location | `type=position`, `method=positionloc`, `position`, `value`, `title` | none | `<REDACTED_POSITION_ID>`, `<REDACTED_LOCATION_ID>` | `allapprove`, `100` | position assignments, location assignments | export-proven |
| 7 | 22 | position by applicant location | `type=position`, `method=positionlocexpr`, `position`, `value`, `title` | `Applicant:Location`; expression references applicant user context and `LocationID` | `<REDACTED_POSITION_ID>` | `allapprove`, `100` | applicant user context, position assignments, location assignments | export-proven |
| 8 | 24 | applicant line manager | `type=user`, `method=expression`, `value`, `title` | `Applicant:Line Manager`; expression references applicant user context and `LineManager` | none | `allapprove`, `100` | applicant user context, user directory data | export-proven |
| 9 | 27 | specific user | `type=user`, `method=direct`, `value`, `title` | none | `<REDACTED_USER_ID>` | `allapprove`, `100` | valid target-tenant user | export-proven |

## Normalized References

Created under:

```text
docs/studies/normalized/workflow-assignment-task-assignees.bundle.json
```

Files:

- `assignment-assignee-inventory.normalized.json`
- `assignment-assignee-specific-user.normalized.json`
- `assignment-assignee-applicant-line-manager.normalized.json`
- `assignment-assignee-department-manager.normalized.json`
- `assignment-assignee-job-position.normalized.json`
- `assignment-assignee-position-by-department.normalized.json`
- `assignment-assignee-position-by-applicant-department.normalized.json`
- `assignment-assignee-position-by-location.normalized.json`
- `assignment-assignee-position-by-applicant-location.normalized.json`

Not created because not found in this export:

- specific multiple users
- generic workflow-variable assignee
- user group assignee
- position by department plus location in one assignee entry
- mixed static plus expression assignee list
- selected department manager as a static department manager entry distinct from position-by-department

## Export-Proven Assignee Patterns

## Workflow Actions Layout Assignee Golden References

`Workflow actions layout.json` adds a focused business-process assignee reference set. The raw export contains tenant-local Job Position IDs; committed references must redact those IDs and preserve only the reusable shape.

Reusable reference file:

```text
docs/reference/workflow-assignment-task-assignee-golden-references.json
```

Golden-reference node mapping:

| Business intent | Source node | Required `usertaskassignment[]` shape | Generation rule |
|---|---|---|---|
| Applicant line manager approval | `Line manager approval` | one entry with `type="user"`, `method="expression"`, expression-button data `type=user`, nested `param.id=${type:application, prop:ApplicantUserID}`, `prop=LineManager` | Use for “Line manager approval” and “Applicant line manager” approval tasks. Do not model applicant as a workflow variable. |
| Applicant department manager/head approval | `Department head approval` | one entry with `type="user"`, `method="expression"`, expression-button data `type=org`, nested `param.id=${type:user, param.id=${type:application, prop:ApplicantUserID}, prop:OrganizationID}`, `prop=Manager` | Use for “Department manager”, “Department head”, and “Department approval” tasks. |
| Job Position approval/complete task | `Finance manager approval`, `Casher confirm`, `General manager approval` | one entry with `type="position"`, `method="position"`, `position=<confirmed numeric Job Position ID>`, `title="Job position: <name>"` | Query existing Job Positions through the Yeeflow API when credentials allow. If a matching position is not found, create/update only after explicit system-admin confirmation. If neither is possible, leave the position unresolved and let preflight block signing. Never reuse sample export IDs. |
| Workflow user variable approval | `Owner approval` | one entry with `type="user"`, `method="expression"`, expression-button data `type=variable`, `param.id=Owner` | Use when the App Plan states the task owner comes from a user-valued workflow variable. |
| Workflow user variable line manager | `Owner's manager confirm` | one entry with `type="user"`, `method="expression"`, expression-button data `type=user`, nested `param.id=${type:variable, param.id=Owner}`, `prop=LineManager` | Use for tasks assigned to the manager of a workflow user variable. |
| Multiple assignees | `Owner's manager approval` | two entries in `usertaskassignment[]`: the workflow user variable and that variable's line manager | Preserve array order and both entries when the business plan explicitly requires multiple assignees. |

Generation boundary:

- one Assignment Task may contain one or multiple assignee entries
- multiple assignees are valid but should be generated only when the App Plan explicitly calls for them
- Job Position assignees require proof metadata (`requiredJobPositionName`, `source`, `proofStatus`) and a real numeric position ID
- unresolved Job Position requirements are generation blockers, not a reason to silently fallback to applicant line manager
- direct user assignment remains tenant-sensitive and should require explicit user mapping

## API-Assisted Org/Reference Interpretation

Proof level: `api-assisted-category-check`

The `yeeflow-api-operator` read-only connectivity helper was originally run with local credentials. Current helper behavior is OAuth-first and does not require `.env.local` for normal read-only checks. Raw API responses were not committed.

API availability:

| Endpoint category | Endpoint | HTTP/API status | Count reported |
|---|---|---:|---:|
| users | `POST /users/search` | `200` / `0` | `3` total, `3` returned in the category check |
| departments | `GET /departments?parentId=0` | `200` / `0` | `6` returned |
| locations | `GET /locations` | `200` / `0` | `2` returned |
| positions | `GET /positions` | `200` / `0` | `6` returned |

Base URL behavior:

- configured env base: `404` for directory probe
- configured env base plus `/v1`: `404` for directory probe
- documented developer API base: `200` and used for read-only lookup

Redaction handling:

- raw user, department, location, and position IDs stayed in memory or ignored `tmp/`
- committed normalized references use only placeholders such as `<USER_REF_CONFIRMED_BY_API>`, `<DEPARTMENT_REF_CONFIRMED_BY_API>`, `<LOCATION_REF_CONFIRMED_BY_API>`, and `<POSITION_REF_CONFIRMED_BY_API>`
- no user names, emails, phone numbers, tenant IDs, raw API responses, or private org values are committed

API lookup confirms org object categories, not runtime workflow routing. A reference matching a readable API category means the exported static value corresponds to a known object category in the queried tenant; it does not prove that a submitted workflow will route to the intended assignee at runtime.

## Job Position Generation Policy

Generated approval workflows must not invent job-position IDs or names. For each Assignment Task, the app plan and generation report must list:

- task name
- assignment type: line manager, department manager, location manager, job position, explicit user, requester/submitter, or another supported expression
- required job position name when applicable
- source: discovered existing job position, user-selected existing job position, admin-created after confirmation, unresolved, or not applicable
- proof status
- fallback or blocker

If the required job position exists, generation may use it only with discovered or explicitly confirmed proof metadata. If the required job position is missing and current user system-admin status is proven, prepare a guarded creation/update plan and require explicit confirmation before any write. If admin status is missing or cannot be proven, stop and instruct the user to ask a system admin to create/update the job position or provide an existing position/fallback. Job-position creation, update, and user assignment/removal are writes and must never run automatically during generation.

Generated application reports must list the workflow assignment tasks and assignee sources, state whether each job position was discovered, user-selected, created after confirmation, or unresolved, and state the blocker when unresolved. Reports must not print raw user records, raw job-position records, full IDs, full emails, raw API responses, decoded package payloads, raw `Resource`, or raw `Sign`. Workflow assignment correctness is not proven until browser/runtime verification submits a safe request and observes task routing.

## Updated Export Extension: Test ABC (1).yap

`Test ABC (1).yap` decoded successfully as a read-only export and contains the same approval form/workflow shell:

- source path: `/Users/Renger/Downloads/Test ABC (1).yap`
- approval workflow forms found: `1`
- assignment task nodes found: `9`
- assignment task node type: `MultiAssignmentTask`
- assignee storage: `properties.usertaskassignment`

New export-proven fields and settings:

- `properties.usertaskassignment` can contain multiple assignee entries.
- `properties.issequential=true` is the observed Sequential Appointed Order marker.
- Absence of `issequential` on multiple-assignee tasks is interpreted as `parallel-or-default` from export structure plus Help Center terminology, not runtime proof.
- `properties.approveway` varies across `allapprove`, `anyprocess`, `anyapprove`, `anyreject`, and `custompercentage`.
- `properties.approvepercentage=60` is observed with `approveway=custompercentage`.
- `properties.isenabledemail=true` adds notification fields: `to`, `subject`, `html`, `notifyrules`, and `duedatetype`.
- `properties.automaticapproveddefinition=true` and `properties.isallowrecalled=true` are observed on one task as additional assignment-task settings.

### Export Comparison

| Task | Previous pattern from `Test ABC.yap` | Updated pattern from `Test ABC (1).yap` | What changed | Newly observed fields | Appointed Order | Completion / approval setting | Email | Proof level |
|---|---|---|---|---|---|---|---|---|
| 1 | applicant department manager | applicant line manager + user group expression | multiple expression assignee sources; user group source added | `issequential` | Sequential (`issequential=true`) | `allapprove`, `100` | disabled | export-proven + product-documented |
| 2 | direct job position | applicant department manager + position all-users expression | mixed expression sources; position expression can resolve users | none beyond prior common settings | Parallel/default | `anyprocess`, `100` | disabled | export-proven + API-category-assisted |
| 3 | direct job position | direct position + two direct users | multiple static users plus position source | multiple `direct` entries in array | Parallel/default | `anyapprove`, `100` | disabled | export-proven + API-category-assisted |
| 4 | position by selected department | direct position + direct user | mixed static position/user source | mixed entries in array | Parallel/default | `anyreject`, `100` | disabled | export-proven + API-category-assisted |
| 5 | position by applicant department | position by selected department + direct user | selected department position plus direct user | `approveway=custompercentage`, `approvepercentage=60` | Parallel/default | `custompercentage`, `60` | disabled | export-proven + API-category-assisted |
| 6 | position by selected location | position by applicant department + direct user | context-derived department position plus direct user | `automaticapproveddefinition`, `isallowrecalled`, `allowskip=false` | Parallel/default | `allapprove`, `100` | disabled | export-proven |
| 7 | position by applicant location | position by selected location | same source category as previous selected-location task; reassign/sign enabled | `isallowreassign=true`, `isallowsign=true` | Parallel/default | `allapprove`, `100` | disabled | export-proven + API-category-assisted |
| 8 | applicant line manager | position by applicant location | email notification enabled on assignment task | `isenabledemail`, `to`, `subject`, `html`, `notifyrules`, `duedatetype` | Parallel/default | `allapprove`, `100` | enabled | export-proven |
| 9 | direct user | direct user + applicant line manager | mixed static and expression user sources | multiple entries in array | Parallel/default | `allapprove`, `100` | disabled | export-proven + API-category-assisted |

### Multiple-Assignee Structures

`Test ABC (1).yap` proves that `properties.usertaskassignment` is the only observed list container for multiple Assignment Task assignee sources. The list stores assignee entries as objects. Export-proven entry field names remain:

- direct user: `type`, `method`, `value`, `title`
- expression user/user group/position-all-users: `type`, `method`, `value`, `title`
- direct position: `type`, `method`, `position`, `title`
- position by selected department/location: `type`, `method`, `position`, `value`, `title`
- position by applicant department/location: `type`, `method`, `position`, `value`, `title`

Mixed source types can coexist in one task:

- expression + user group expression
- applicant department manager expression + position all-users expression
- direct position + multiple direct users
- position by selected department + direct user
- position by applicant department + direct user
- direct user + applicant line manager expression

User group assignment is represented as an expression-button value with data shape `type=usergroup`, `prop=Users_ID`. Yeeflow API Operator v1 does not provide a user-group endpoint, so this is export-proven and product-documented but not API-confirmed.

Job positions can represent multi-user assignee sources in two export-proven ways:

- `type=position`, `method=position` or variants, where the referenced job position may resolve to users in the organization model.
- expression-button value with data shape `type=position`, `prop=Users_ID`, interpreted as all users for a position.

### Appointed Order and Completion Settings

Help Center describes Appointed Order as Sequential or Parallel. `Test ABC (1).yap` proves the following serialized shape:

| Concept | Export-proven field/shape | Notes | Proof level |
|---|---|---|---|
| Sequential Appointed Order | `properties.issequential=true` | Found on Task 1 with two assignee entries. List order is the only visible ordering signal. | product-documented + export-proven |
| Parallel Appointed Order | `properties.issequential` absent | Found on the other multiple-assignee tasks. Interpreted as parallel/default from product docs and contrast with sequential marker. | product-documented + export-proven partial |
| All approve | `approveway=allapprove`, `approvepercentage=100` | Found on several tasks. | export-proven |
| Anyone/process completion | `approveway=anyprocess`, `approvepercentage=100` | Found on Task 2. Exact runtime semantics need runtime proof. | export-proven |
| Anyone approve | `approveway=anyapprove`, `approvepercentage=100` | Found on Task 3. | export-proven |
| Anyone reject | `approveway=anyreject`, `approvepercentage=100` | Found on Task 4. | export-proven |
| Custom percentage | `approveway=custompercentage`, `approvepercentage=60` | Found on Task 5. | export-proven |
| Auto approve | `automaticapproveddefinition=true` | Found on Task 6. Product docs describe auto-approve behavior; runtime not tested here. | product-documented + export-proven |
| Allow recall | `isallowrecalled=true` | Found on Task 6. | product-documented + export-proven |

### Email Notification Settings

Task 8 in `Test ABC (1).yap` proves Assignment Task email notification configuration without sending email:

- enable flag: `properties.isenabledemail=true`
- recipient expression: `properties.to` expression-button label `Current Task Context:Assignee:Email`
- subject expression: `properties.subject` expression-button label `Workflow Name`
- body field: `properties.html`
- notification rules list: `properties.notifyrules=[]`
- due date unit: `properties.duedatetype=hour`

No delivery was tested. No real email addresses are committed. The notification shape is export-proven only; delivery and quick-completion behavior remain runtime-unproven.

### Updated Help Center Comparison

| Help Center concept | Export-proven field/shape from `Test ABC (1).yap` | Match status | Notes | Proof level |
|---|---|---|---|---|
| Assignment Task can assign to one or multiple owners | `usertaskassignment` array with one, two, or three entries | matched | Multiple entries are now export-proven. | product-documented + export-proven |
| Mix users, user groups, and job positions | Mixed arrays containing direct users, user group expression, and position sources | matched | User group is expression-shaped, not direct static object. | product-documented + export-proven |
| User group assignee | expression-button `type=usergroup`, `prop=Users_ID` | matched | API category not confirmed because API Operator v1 has no group endpoint. | product-documented + export-proven |
| Job position assignees can resolve to users | position entries and position `Users_ID` expression | matched | API confirms position category for static position refs, not runtime expansion. | product-documented + export-proven + API-category-assisted |
| Sequential appointed order | `issequential=true` | matched | Runtime ordering not tested. | product-documented + export-proven |
| Parallel appointed order | absent `issequential` on other multi-assignee tasks | partially matched | Export does not show an explicit `false`; treat as default/parallel until runtime baseline confirms. | product-documented + export-proven partial |
| Completion condition all/any/custom percentage | `approveway` plus `approvepercentage` | matched | Values observed: `allapprove`, `anyprocess`, `anyapprove`, `anyreject`, `custompercentage`. | product-documented + export-proven |
| Email notification to task assignees | `isenabledemail=true`, `to` current task assignee email expression | matched | No email sent; no delivery proof. | product-documented + export-proven |
| Custom email subject/body | `subject`, `html` fields | matched | Body content redacted as shape only. | product-documented + export-proven |

### Updated API-Assisted Interpretation

The read-only Yeeflow API Operator lookup was run again with local credentials in the historical proof. Current helper behavior is OAuth-first; only auth presence, statuses, and counts should be reported. No credential values or raw API responses were saved or committed.

| Category endpoint | HTTP/API status | Count observed | Use in this study |
|---|---:|---:|---|
| users | `200` / `0` | `3` | Confirmed static direct-user references as user category. |
| departments | `200` / `0` | `6` | Confirmed selected department reference in position-by-department task. |
| locations | `200` / `0` | `2` | Confirmed selected location reference in position-by-location task. |
| positions | `200` / `0` | `6` | Confirmed direct/static position references and position all-users expression reference. |

API-assisted confirmations:

- static direct users: confirmed where static values were present
- static direct positions: confirmed
- selected department for position-by-department: confirmed
- selected location for position-by-location: confirmed
- position all-users expression: position category confirmed
- user group expression: now confirmed as a readable group/member category through documented read-only `GET /groups` and `GET /groups/{id}/users`
- applicant/context-derived expressions: not static API lookups; runtime-context dependent

See also `docs/studies/yeeflow-api-operator-assignment-routing-coverage.md` for the API coverage audit. The audit adds read-only support for user detail, location detail, groups, group members, and position assignments. It does not add or test write APIs, and it does not prove runtime routing.

| Task | Export assignee classification | API-assisted interpretation | Confirmation | Notes |
|---:|---|---|---|---|
| 1 | applicant department manager | expression-only user/org manager lookup | not applicable to static ID | Expression references applicant department manager; no static department/user ID is embedded for API matching. |
| 2 | direct job position | job position reference | confirmed | Static position reference matched the positions category. |
| 3 | direct job position | job position reference | confirmed | Static position reference matched the positions category. |
| 4 | position by selected department | job position plus department references | confirmed | Static position and department references matched their API categories. |
| 5 | position by applicant department | job position plus applicant department expression | confirmed for static position | Position matched the positions category; applicant department is resolved by runtime expression. |
| 6 | position by selected location | job position plus location references | confirmed | Static position and location references matched their API categories. |
| 7 | position by applicant location | job position plus applicant location expression | confirmed for static position | Position matched the positions category; applicant location is resolved by runtime expression. |
| 8 | applicant line manager | expression-only user manager lookup | not applicable to static ID | Expression references applicant line manager; no static user ID is embedded for API matching. |
| 9 | specific user | user reference | confirmed | Static user reference matched the users category. |

Unresolved references:

- No static export reference remained unresolved by category in the read-only API comparison.
- Expression-derived applicant manager, department, and location references remain runtime-dependent because their final values are resolved from applicant context during workflow execution.

### Specific User

Proof level: `export-proven`

Shape:

```json
{
  "type": "user",
  "method": "direct",
  "value": "<USER_REF_CONFIRMED_BY_API>",
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: tenant-sensitive. Do not generate with hardcoded private user IDs unless the target tenant mapping is explicitly authorized and validated. Prefer applicant/current-user expression assignment or a safe user-selection field for generic packages.

### Applicant Line Manager

Proof level: `export-proven`

Shape:

```json
{
  "type": "user",
  "method": "expression",
  "value": {
    "kind": "expressionButton",
    "label": "Applicant:Line Manager",
    "dataShape": {
      "type": "user",
      "param": {
        "id": {
          "parseStatus": "opaque",
          "expressionSourceHints": {
            "props": ["ApplicantUserID"],
            "usesApplicantUserId": true
          }
        }
      },
      "prop": "LineManager"
    }
  },
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: depends on runtime applicant context and user profile manager data. Export-proven only; routing behavior requires focused runtime proof.

### Applicant Department Manager

Proof level: `export-proven`

Shape:

```json
{
  "type": "user",
  "method": "expression",
  "value": {
    "kind": "expressionButton",
    "label": "Applicant:Department:Manager",
    "dataShape": {
      "type": "org",
      "param": {
        "id": {
          "parseStatus": "opaque",
          "expressionSourceHints": {
            "props": ["ApplicantUserID", "OrganizationID"],
            "usesApplicantUserId": true
          }
        }
      },
      "prop": "Manager"
    }
  },
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: depends on applicant department and department manager configuration. The Help Center describes parent-department fallback behavior when a lower department has no manager, but that fallback is product-documented, not export-proven by this package.

### Direct Job Position

Proof level: `export-proven`

Shape:

```json
{
  "type": "position",
  "method": "position",
  "position": "<POSITION_REF_CONFIRMED_BY_API>",
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: requires a valid target-tenant position. Use only when authorized org/reference data is available.

### Job Position By Selected Department

Proof level: `export-proven`

Shape:

```json
{
  "type": "position",
  "method": "positionorg",
  "position": "<POSITION_REF_CONFIRMED_BY_API>",
  "value": "<DEPARTMENT_REF_CONFIRMED_BY_API>",
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: requires a valid target-tenant position and department pairing.

### Job Position By Applicant Department

Proof level: `export-proven`

Shape:

```json
{
  "type": "position",
  "method": "positionorgexpr",
  "position": "<POSITION_REF_CONFIRMED_BY_API>",
  "value": {
    "kind": "expressionButton",
    "label": "Applicant:Department",
    "dataShape": {
      "type": "user",
      "prop": "OrganizationID"
    }
  },
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: depends on applicant context, department hierarchy, and position assignment data.

### Job Position By Selected Location

Proof level: `export-proven`

Shape:

```json
{
  "type": "position",
  "method": "positionloc",
  "position": "<POSITION_REF_CONFIRMED_BY_API>",
  "value": "<LOCATION_REF_CONFIRMED_BY_API>",
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: requires valid target-tenant location and position data.

### Job Position By Applicant Location

Proof level: `export-proven`

Shape:

```json
{
  "type": "position",
  "method": "positionlocexpr",
  "position": "<POSITION_REF_CONFIRMED_BY_API>",
  "value": {
    "kind": "expressionButton",
    "label": "Applicant:Location",
    "dataShape": {
      "type": "user",
      "prop": "LocationID"
    }
  },
  "title": "<REDACTED_ASSIGNEE_LABEL>"
}
```

Generation note: depends on applicant context, location setup, and position assignment data.

## Help Center Comparison

| Help Center concept | Export-proven field/shape from `Test ABC.yap` | Match status | Notes | Proof level |
|---|---|---|---|---|
| Assignment Task action creates approval/complete work for assigned users | `MultiAssignmentTask` nodes with `properties.usertaskassignment`, approval settings, and task form `taskurl` | matched | Export has approval-style tasks; task type field is not present in these nodes. | product-documented + export-proven |
| Task assignee can be single or multiple users | Every export task has `usertaskassignment` array with one entry | partially matched | Array shape supports multiple entries, but multiple users were not present. | product-documented + export-proven partial |
| Specific direct user | `type=user`, `method=direct`, `value`, `title` | matched | Values are redacted. | product-documented + export-proven |
| Expression-based assignee | `type=user`, `method=expression`, rich HTML expression button in `value` | matched | Export includes applicant line manager and applicant department manager expressions. | product-documented + export-proven |
| Applicant/requester line manager | expression label `Applicant:Line Manager`, `prop=LineManager` | matched | Depends on applicant user context and manager profile data. | product-documented + export-proven |
| Applicant/requester department manager | expression label `Applicant:Department:Manager`, outer `type=org`, `prop=Manager` | matched | Parent-department fallback is product-documented but not visible in export schema. | product-documented + export-proven |
| Selected department manager | no distinct static department-manager entry found | not found in export | Static department reference appears only as position-by-department, not direct manager. | product-documented + unproven |
| Direct job position | `type=position`, `method=position`, `position`, `title` | matched | Two tasks use this shape. | product-documented + export-proven |
| Job position by selected department | `type=position`, `method=positionorg`, `position`, `value`, `title` | matched | `value` is the selected department reference. | product-documented + export-proven |
| Job position by applicant/current department | `type=position`, `method=positionorgexpr`, expression label `Applicant:Department` | matched | Uses applicant context and `OrganizationID`. | product-documented + export-proven |
| Job position by selected location | `type=position`, `method=positionloc`, `position`, `value`, `title` | matched | `value` is the selected location reference. | product-documented + export-proven |
| Job position by applicant/current location | `type=position`, `method=positionlocexpr`, expression label `Applicant:Location` | matched | Uses applicant context and `LocationID`. | product-documented + export-proven |
| Job position by department plus location | no combined department/location shape found | not found in export | Do not generate until export/runtime evidence exists. | product-documented terminology only if applicable + unproven |
| User group assignee | no user group assignment entry found | not found in export | Mentioned in Assignee Editor behavior reference, but not export-proven here. | product-documented + unproven |
| Workflow variable assignee | no workflow-variable assignee entry found | not found in export | Mentioned in Assignee Editor behavior reference, but not export-proven here. | product-documented + unproven |
| Appointed order sequential/parallel | no `issequential` field found in this export's task properties | not found in export | Existing action reference lists `properties.issequential`, but this sample did not include it. | product-documented + implementation-reference |
| Completion condition all/any/custom percentage | `approveway=allapprove`, `approvepercentage=100` | partially matched | Other modes are in existing action reference, not in this export. | product-documented + export-proven partial |
| Task form association | `properties.taskurl` points to a redacted task form page id | matched | Task form settings are secondary in this study. | product-documented + export-proven |
| Reassign/add-assignee buttons | `isallowreassign=false`; no add-assignee field observed | partially matched | Sign/additional assignee behavior not proven by this export. | product-documented + export-proven partial |
| Email/notification settings | `isenabledemail=false`; notification fields absent | partially matched | Notification settings are secondary in this study. | product-documented + export-proven partial |

## Complete Task And Due Date Extension

`Test ABC (2).yap` extends the Assignment Task learning from assignee-only settings into task type and due-date configuration.

| Finding | Export-proven field/shape | Count / values | Proof level |
|---|---|---|---|
| Assignment Task count | `MultiAssignmentTask` | 12 | export-proven |
| Approval/default task type | absent `properties.tasktype` | 5 nodes | export-proven |
| Complete task type | `properties.tasktype="complete"` | 7 nodes | export-proven |
| Due date amount | `properties.duedatedefinition` | `120`, `3` | export-proven |
| Due date units | `properties.duedatetype` | `hour`, `day`, `express` | export-proven |
| Working-day calculation | `properties.isfromworkcalendar=true` | observed on one day-based node | export-proven |
| Due-date expression | `properties.duedateexpress` | expression-button date variable | export-proven |
| Reminder rules | `properties.notifyrules[]` | 20 rules across email-enabled tasks | export-proven |
| Reminder timing | `notifyrules[].actiondate.relative` | `"0"`, `"-1"`, `"1"` | export-proven |
| Reminder offset units | `notifyrules[].actiondate.type` | `day`, `hour` | export-proven |

`minute` due-date units are product-documented by the Help Center but were not found in `Test ABC (2).yap` or `Test ABC (3).yap`.

Reminder recipient interpretation:

- Task-level `properties.to` uses the current task assignee email expression; this matches the task-owner/default-recipient behavior described by the Help Center.
- `notifyrules[]` stores action timing, subject, and content. No separate rule-level recipient field was found.
- Applicant, task-owner line manager, task-owner department manager, and specified-recipient reminder shapes were not found in the export.

Detailed task-type and due-date findings are documented in:

```text
docs/studies/collection-02.md#workflow-assignment-task-complete-task-and-due-date
```

Normalized references are under:

```text
docs/studies/normalized/workflow-assignment-task.bundle.json
```

## Validator Recommendations

Warning-first validator support is appropriate:

- identify `MultiAssignmentTask` nodes
- warn when `properties.usertaskassignment` is missing, not an array, or empty
- warn when an assignee entry is not an object
- warn when `type` or `method` is missing
- warn when method is outside the export-proven set: `direct`, `expression`, `position`, `positionorg`, `positionorgexpr`, `positionloc`, `positionlocexpr`
- warn when position-based assignment lacks `position`
- warn when static direct/department/location assignment lacks `value`
- warn when expression assignment lacks an expression-button-shaped `value`
- warn that direct user assignment is tenant-sensitive
- warn for unknown `approveway` values; export-proven values are `allapprove`, `anyprocess`, `anyapprove`, `anyreject`, and `custompercentage`
- warn when `approveway=custompercentage` lacks a numeric `approvepercentage`
- warn when `issequential` is present but not boolean
- warn when `isenabledemail=true` lacks `to`, `subject`, or `html` notification shape
- warn for unknown `tasktype` values; absence of `tasktype` is the studied approval/default shape and `complete` is the studied Complete task shape
- warn for unknown `duedatetype` values; `hour`, `day`, and `express` are export-proven while `minute` is product-documented
- warn when `duedatetype=express` lacks `duedateexpress`
- warn when `notifyrules[]` has malformed action-date timing or missing subject/content
- warn that user-group assignment is export-proven and API-category-assisted, but group expansion/routing remains runtime-unproven

Do not add hard errors in compatibility mode from this study. Existing packages may omit optional fields or use unstudied tenant-specific shapes.

## Generation Rules

- Treat these shapes as export-proven schema references, not runtime-proven routing behavior.
- Do not invent assignee methods beyond the export-proven set.
- Specific direct-user assignment requires explicit authorized target-tenant user mapping or safe read-only directory lookup.
- Position, department, and location assignment requires valid target-tenant org/reference data.
- If org/reference lookup is authorized, use `yeeflow-api-operator` read-only lookup and never commit raw API responses or private org data.
- For generic generated packages, prefer applicant/current-user expression routing or user-selection field patterns over hardcoded direct users.
- Multiple assignee arrays, user-group expression, position all-users expression, Sequential marker, Parallel/default absence of `issequential`, `approveway` variants, custom percentage, and email notification fields are export-proven from `Test ABC (1).yap`.
- Preserve assignee array order when `issequential=true`; do not claim runtime order until a focused runtime baseline proves it.
- Treat absent `issequential` as parallel/default only with the current proof boundary; runtime proof is still required.
- Email notification config can be generated from export-proven fields only when explicitly requested and safe, but notification delivery must not be tested or claimed without scoped runtime approval.
- Preserve Complete task marker `tasktype="complete"` only when generating Complete task nodes; preserve absent `tasktype` for approval/default nodes unless a future export proves an explicit approval marker.
- Preserve due-date fields together: `duedatedefinition`, `duedatetype`, optional `duedateexpress`, optional `isfromworkcalendar`, and `notifyrules[]`.
- Do not invent Automatic Treatment due-date action rules; only reminder `actiontype="1"` is export-proven here.
- Data-list workflow Assignment Tasks can use list-item field context in assignee expressions. `Purchase Requests.ydl` export-proves a Created By list-field expression that resolves the Created By user's line manager.
- Preserve data-list assignee expressions as expression-button strings; do not replace them with static users or invented IDs.
- When generating data-list workflow task forms, preserve list-bound controls with `isListControl`, `identifier`, `InternalName`, `fieldID`, and `____customListFields_` binding. Use read-only settings for custom list fields when the task should not update source list data.
- Do not claim selected department manager, workflow variable, position by department plus location, or quick-completion behavior as generation-safe from these exports alone.

## Runtime-Test Plan

A focused runtime baseline is recommended before merge if these rules will be promoted beyond export-proven/validator-backed guidance.

Suggested minimal runtime baseline:

1. Build a small approval app with one request form and one task form.
2. Include one assignment task per assignee class to test, starting with:
   - applicant line manager
   - applicant department manager
   - direct job position
   - job position by applicant department
   - job position by applicant location
   - multiple static users
   - mixed direct user plus position
   - user group expression if a safe disposable group is available
   - Sequential appointed order
   - Parallel/default appointed order
   - custom percentage completion
   - Complete task designer/open proof
   - due-date and reminder configuration designer/open proof
   - email notification configuration designer/open proof, with delivery disabled or explicitly scoped
3. Use safe target-tenant org data selected through authorized read-only lookup or user-provided non-private setup.
4. Locally validate package, graph, workflow action configuration, and secret scans before import.
5. Import/open only after local validation passes.
6. Submit only disposable requests with explicitly safe assignees.
7. Record whether tasks route to expected safe users/positions without exposing identities.

Do not execute workflow, send notifications, or expose user identities unless the runtime test scope is explicitly safe.

## Known Gaps

- Runtime routing is not proven.
- Multiple direct users are export-proven only as part of a mixed position/user task, not as a task containing only direct users.
- Mixed assignee sources in one task are export-proven but not runtime-proven.
- User group assignee is export-proven, product-documented, and API-category-assisted, but group expansion/routing is not runtime-proven.
- Workflow variable assignee is not found in this export.
- Position by department plus location is not found in this export.
- Selected department manager as a direct static manager shape is not found in this export.
- Parent-department manager fallback is product-documented but not export-proven.
- Email notification fields and due-date reminder settings are export-proven, but delivery, quick completion, due-date scheduling, and task-form open behavior are not runtime-proven here.
- Minute due-date units and Automatic Treatment reminder actions are product-documented but not export-proven in these files.
- Data-list workflow Created By/list-field assignee routing is export-proven but not runtime-proven.
- Data-list workflow task form list-field edit/save behavior is not runtime-proven.

## Data-List Workflow Extension

`Purchase Requests.ydl` adds export-proven data-list workflow behavior to the Assignment Task study:

| Area | Export-proven finding | Proof level |
|---|---|---|
| Workflow type | One `WorkflowType: 1` data-list workflow with a `FlowMappings[]` new-item trigger | export-proven |
| Assignment task | One `MultiAssignmentTask` with `usertaskassignment[]` | export-proven + validator-backed |
| Direct user | Static direct user shape reused from approval workflows | export-proven; tenant-sensitive |
| Applicant manager | Applicant line manager expression shape reused | export-proven |
| User group | User group `All users` expression shape reused | export-proven; runtime-unproven |
| List-item assignee | Created By list-field expression resolving `LineManager` | export-proven |
| Due date | `duedatedefinition: 120`, `duedatetype: hour` | export-proven |
| Notification | Assignment Task email fields and one due-date reminder rule present | export-proven; delivery untested |
| Task form | Task form includes normal controls, list-bound controls, workflow action panel, and history | export-proven |

The data-list workflow export proves that Assignment Task expressions can reference list-item context. The concrete committed normalized example uses `<CREATED_BY_FIELD_ASSIGNEE_REF>` and `<LIST_FIELD_ASSIGNEE_REF>` placeholders and does not expose raw list, field, user, or tenant identifiers.

See also:

- `docs/studies/collection-02.md#workflow-approval-vs-data-list-actions`
- `docs/studies/normalized/workflow-assignment-task-assignees.bundle.json#assignment-assignee-data-list-field-value.normalized.json`
- `docs/studies/normalized/workflow-assignment-task-assignees.bundle.json#assignment-assignee-created-by-list-field.normalized.json`

---

<a id="workflow-assignment-task-complete-task-and-due-date"></a>

<!-- source: docs/studies/collection-02.md#workflow-assignment-task-complete-task-and-due-date -->

# Workflow Assignment Task Complete Task And Due Date Settings

## Purpose

This study extends Assignment Task export learning with `Test ABC (2).yap`, focused on task type, Complete task configuration, due dates, due-date reminder actions, and reminder email settings.

Proof boundary:

- `export-proven`: found in `Test ABC (2).yap`.
- `product-documented`: described in Yeeflow Help Center.
- `validator-backed`: checked by local validators after this study.
- `runtime-proven`: not claimed here.

No approval requests were submitted and no email delivery was tested.

## Sources

Source export:

```text
<downloads>/Test ABC (2).yap
```

Comparison exports:

```text
<downloads>/Test ABC.yap
<downloads>/Test ABC (1).yap
```

References:

- Yeeflow Help Center: Assignment Task Action
  https://<yourdomain>.yeeflow.com/<redacted-path>
- Yeeflow Help Center: Add assignee with Assignee Editor
  https://<yourdomain>.yeeflow.com/<redacted-path>

## Export Inventory

`Test ABC (2).yap` decoded successfully with large numeric IDs preserved as strings.

| Item | Count / value | Proof |
|---|---:|---|
| Approval forms | 1 | export-proven |
| Workflow type | 2 | export-proven |
| Workflow shapes | 38 | export-proven |
| Assignment Task nodes | 12 | export-proven |
| Start nodes | 1 | export-proven |
| SequenceFlow nodes | 24 | export-proven |

## Task Type Findings

Assignment Task task type is stored on `MultiAssignmentTask.properties.tasktype` when present.

| Task type | Export shape | Count | Interpretation | Proof |
|---|---|---:|---|---|
| Approval/default task | `tasktype` absent | 5 | Existing exports use absent `tasktype` for approval/default behavior. | export-proven |
| Complete task | `tasktype: "complete"` | 7 | Complete-task behavior is explicitly serialized with `complete`. | export-proven |

Product documentation describes two Assignment Task types: Approval task and Complete task. The export proves `complete` as the explicit Complete task marker, while Approval task remains represented by absent `tasktype` in these exports.

## Complete Task Configuration

Fields found on Complete task nodes:

| Field | Observed values | Notes | Proof |
|---|---|---|---|
| `tasktype` | `complete` | Explicit Complete task marker. | export-proven |
| `approveway` | `allapprove`, `anyapprove`, `custompercentage` | Completion condition is shared with approval-style nodes. | export-proven |
| `approvepercentage` | `100`, `60` | `60` appears with `custompercentage`. | export-proven |
| `issequential` | `true` on two Complete tasks | Sequential Appointed Order marker remains the same field. | export-proven |
| `automaticapproveddefinition` | `true` on two Complete tasks | Product-doc equivalent is Auto approve/auto complete style behavior; runtime effect untested. | export-proven |
| `isallowrecalled` | `true` on two Complete tasks | Allows recall after task action, subject to runtime rules. | export-proven |
| `isallowreassign` | `true` or `false` | Reassign button setting. | export-proven |
| `isallowsign` | `true` or `false` | Add-assignee/sign-like button setting. | export-proven |
| `allowskip` | mostly `true`, one `false` | Skip/auto handling remains runtime-sensitive. | export-proven |

## Due Date Settings

Due date fields are stored directly under `MultiAssignmentTask.properties`.

| Field | Shape | Notes | Proof |
|---|---|---|---|
| `duedatedefinition` | number | Numeric due amount. Default-like `120` is common; `3` appears for day-based due dates. | export-proven |
| `duedatetype` | `hour`, `day`, `express` | `hour` and `day` are direct units; `express` uses a date variable expression. | export-proven |
| `duedateexpress` | expression-button HTML string | Present with expression-based due date; observed label references a workflow variable. | export-proven |
| `isfromworkcalendar` | boolean | Observed `true` with a day-based due date. | export-proven |

Product documentation also lists minutes as a due-date unit. `minute` was not found in `Test ABC (2).yap` or `Test ABC (3).yap`, so it remains `product-documented` but not export-proven in these files.

## Reminder Actions

Due-date reminder actions are stored in `properties.notifyrules[]`.

Common rule fields:

| Field | Shape | Notes | Proof |
|---|---|---|---|
| `id` | UUID-like string | Redact as `<REDACTED_NOTIFY_RULE_ID>`. | export-proven |
| `actiontype` | `"1"` | Interpreted as Reminder from product docs and email-shaped rule content. | export-proven + product-documented |
| `actiondate.relative` | `"0"`, `"-1"`, `"1"` | `"0"` = at/on due date; `"-1"` = before due date; `"1"` = after due date. | export-proven + product-documented |
| `actiondate.value` | number | Present for before/after offsets. | export-proven |
| `actiondate.type` | `day`, `hour` | Offset unit for before/after rules. | export-proven |
| `subject` | expression-button rich text | Redacted; references form/task due-date context. | export-proven |
| `content` | expression-button rich text / HTML | Redacted; references form/task due-date context. | export-proven |

Observed reminder timing patterns:

| Timing | Export shape | Example offset | Proof |
|---|---|---:|---|
| On due date | `actiondate.relative: "0"` | none | export-proven |
| Before due date | `actiondate.relative: "-1"` | 2 days, 10 hours | export-proven |
| After due date | `actiondate.relative: "1"` | 4 days, 30 hours | export-proven |

Automatic Treatment is product-documented in the Help Center, but only reminder-shaped `actiontype: "1"` rules were found in this export.

## Reminder Recipients

Assignment task email recipient configuration was observed on the task-level `to` field, not inside each `notifyrules[]` item.

| Recipient concept | Export-proven shape | Status | Notes |
|---|---|---|---|
| Task owner / assignee | `properties.to` expression button labeled `Current Task Context:Assignee:Email` | matched | Used by enabled assignment task notification and due-date reminders. |
| Applicant | not found as reminder recipient | product-documented / not found in export | Applicant appears in reminder body tokens, not as the reminder recipient. |
| Task owner line manager | not found | unproven | No committed shape. |
| Task owner department manager | not found | unproven | No committed shape. |
| Others / specified recipients | not found | unproven | No literal or fixed email recipient was found. |

## Help Center Comparison

| Help Center concept | Export-proven field/shape | Match status | Notes | Proof level |
|---|---|---|---|---|
| Assignment Task can be Approval or Complete task | `tasktype: "complete"` or absent `tasktype` | matched | Complete task marker is explicit; Approval/default is absent in the studied export. | product-documented + export-proven |
| Complete task uses Complete-style completion | `tasktype: "complete"`, `approveway`, `approvepercentage` | partially matched | Export proves storage, not button/runtime behavior. | export-proven |
| Due date custom time supports days/hours/minutes | `duedatetype: "day"`, `"hour"` | partially matched | Minutes are product-documented but not found. | product-documented + export-proven |
| Due date from date variable | `duedatetype: "express"`, `duedateexpress` | matched | Expression button references a workflow date variable. | product-documented + export-proven |
| Working day option for day unit | `isfromworkcalendar: true` | matched | Runtime working-calendar behavior not tested. | export-proven |
| Reminder before/at/after due date | `notifyrules[].actiondate.relative` | matched | Relative values map to before/on/after. | product-documented + export-proven |
| Reminder email to task owner | task-level `to` expression for current task assignee email | matched | Rule-level recipient not found. | product-documented + export-proven |
| Automatic Treatment action | not found | not found in export | Do not generate until an export proves shape. | product-documented / unproven |

## Generation Rules

- Preserve absent `tasktype` for approval/default tasks unless a future export proves an explicit approval marker.
- Use `tasktype: "complete"` only for Complete task generation.
- Preserve `approveway` and `approvepercentage` on both Approval/default and Complete task nodes.
- Preserve `automaticapproveddefinition` and `isallowrecalled` when copied from an export, but do not claim runtime behavior until tested.
- Preserve due-date fields together: `duedatedefinition`, `duedatetype`, optional `duedateexpress`, and optional `isfromworkcalendar`.
- Generate `minute` due-date units only as product-documented until an export proves the exact serialized value.
- Preserve `notifyrules[]` as an array of reminder action objects; do not invent Automatic Treatment actions.
- Treat all email notification behavior as configuration-only until a safe delivery test is explicitly scoped.

## Validator Recommendations

Validators should warn, not hard-error in compatibility mode, for:

- unknown `tasktype` values
- `custompercentage` without numeric `approvepercentage`
- unknown `duedatetype`
- `duedatetype: "express"` without `duedateexpress`
- non-boolean `isfromworkcalendar`
- malformed `notifyrules[]`
- unknown reminder timing values
- missing reminder subject/content

## Known Gaps

- Minute due-date serialization was not found.
- Reminder recipients other than current task assignee were not found.
- Automatic Treatment rule shape was not found.
- Complete task runtime button behavior was not tested.
- Due-date reminder scheduling and email delivery were not tested.

---

<a id="workflow-assignment-task-generation-rules"></a>

<!-- source: docs/studies/collection-02.md#workflow-assignment-task-generation-rules -->

# Workflow Assignment Task Generation Rules

## Scope

These rules cover Yeeflow approval workflow `MultiAssignmentTask` assignee, task type, due-date, reminder, and Start-action-adjacent generation guidance learned from `Workflow actions layout.json`, `Test ABC.yap`, `Test ABC (1).yap`, `Test ABC (2).yap`, and `Test ABC (3).yap`.

Proof boundary:

- Shapes in this document are `export-proven` unless explicitly marked otherwise.
- Validator checks are warning-first and `validator-backed` only after local validation.
- Assignment task runtime routing is not `runtime-proven` until a focused runtime baseline passes.
- Help Center behavior can guide terminology and expected behavior, but generated assignee configurations should use export-proven shapes unless a shape is explicitly marked product-documented but not export-proven.

## Export-Proven Storage

Assignment task assignees are stored on `MultiAssignmentTask` nodes under:

```text
properties.usertaskassignment
```

The value is an array of assignee entries.

Every entry found in `Test ABC.yap` includes:

- `type`
- `method`
- `title`

Some methods also include:

- `value`
- `position`

Common task approval settings found alongside assignees:

- `properties.tasktype`
- `properties.approveway`
- `properties.approvepercentage`
- `properties.duedatedefinition`
- `properties.duedatetype`
- `properties.duedateexpress`
- `properties.isfromworkcalendar`
- `properties.notifyrules`
- `properties.taskurl`
- `properties.isenabledemail`
- `properties.isallowreassign`
- `properties.isallowsign`
- `properties.allowskip`

## Export-Proven Methods

| Method | Type | Meaning | Required fields found | Runtime dependencies | Generation safety |
|---|---|---|---|---|---|
| `direct` | `user` | specific direct user | `value`, `title` | valid target-tenant user | tenant-sensitive; avoid broad generation |
| `expression` | `user` | expression editor user assignee | `value`, `title` | applicant/user context or expression result | export-proven shape, runtime routing unproven |
| `position` | `position` | direct job position | `position`, `title` | valid position assignment | requires target org/reference data |
| `positionorg` | `position` | job position by selected department | `position`, `value`, `title` | valid department and position assignment | requires target org/reference data |
| `positionorgexpr` | `position` | job position by applicant/current department | `position`, `value`, `title` | applicant context, department hierarchy, position assignment | requires runtime proof |
| `positionloc` | `position` | job position by selected location | `position`, `value`, `title` | valid location and position assignment | requires target org/reference data |
| `positionlocexpr` | `position` | job position by applicant/current location | `position`, `value`, `title` | applicant context, location data, position assignment | requires runtime proof |
| `expression` | `user` with expression data `type=usergroup`, `prop=Users_ID` | all users in a user group | `value`, `title` | valid user group membership | export-proven in `Test ABC (1).yap`; API Operator can confirm group/member category through documented read-only endpoints |
| `expression` | `user` with expression data `type=position`, `prop=Users_ID` | all users in a job position | `value`, `title` | valid position assignment and position membership | requires target org/reference data and runtime proof |

## Safe Generation Rules

Use these rules for generated packages:

- Every generated approval workflow Assignment Task must have an explicit assignee plan before package generation. The plan must list task name, assignment type, required job position name when applicable, source, proof status, fallback, and blocker.
- Use `docs/reference/workflow-assignment-task-assignee-golden-references.json` as the focused golden reference for common business assignee patterns.
- Apply `docs/standards/collection-02.md#workflow-assignee-expression-serialization-standard` to every expression-based assignee. The outer Expression Button uses Yeeflow `${ key:value... }` variable JSON rather than `${{...}}`, and manager/organization `param.id` references use nested `${...}` expressions rather than plain JSON strings. Use the shared serializer; do not interpolate generic `JSON.stringify()` output directly.
- “Line manager approval” must use the applicant application context shape: expression data `type=user`, `param.id={type:application, prop:ApplicantUserID}`, `prop=LineManager`.
- “Department manager”, “Department head”, and “Department approval” must use the applicant department manager shape: expression data `type=org`, `param.id={type:user, param.id={type:application, prop:ApplicantUserID}, prop:OrganizationID}`, `prop=Manager`.
- Workflow user-variable assignees must use expression data `type=variable`, `param.id=<workflow user variable id>`.
- Workflow user-variable manager assignees must use expression data `type=user`, `param.id={type:variable, param.id=<workflow user variable id>}`, `prop=LineManager`.
- Multiple assignees must be represented as multiple objects in `properties.usertaskassignment[]`; do not concatenate them into one expression string.
- Do not hardcode tenant-specific direct users by default.
- Use `type=user`, `method=direct` only when the user explicitly supplies or authorizes a valid target-tenant user mapping.
- Redact private IDs in docs and never commit user/org lookup output.
- Prefer applicant/current-user expression routing or an explicit user-selection field when a package must be portable across tenants.
- Use job-position, department, and location assignment only when target-tenant org/reference data is available and authorized.
- Job-position assignments must use a discovered existing job position, a user-selected existing job position, or a job position created/updated only after explicit confirmation by a confirmed system admin. The generator must not silently choose a job position, must not invent job-position IDs or names, and must not reuse IDs from study exports.
- Expired OAuth is not proof that Job Position lookup is unavailable. When OAuth reports expired, the plugin must attempt the supported OAuth refresh flow, re-check authenticated status, and only then decide whether API lookup is unavailable. Final generated Job Position assignee proof must record OAuth status and refresh status for API-assisted discovery or creation.
- API-assisted Job Position assignment must attempt read-only `GET /positions` lookup before using a Job Position ID. Exact case-insensitive name matches may be reused. Ambiguous partial matches require user selection. A missing exact match may proceed to creation only after confirmed admin permission and explicit user approval.
- Job Position creation must be a single accepted write, not a retry loop. If `POST /positions` returns success and a numeric ID, record that ID immediately as provisional creation proof, poll read-back without issuing another create, and stop/report ambiguity if read-back does not converge. Do not create a duplicate just because immediate `GET /positions` is stale.
- Before assigning users or writing workflow routing, run a duplicate-name scan and select one explicit numeric Job Position ID. If duplicates exist, report the side effect and do not silently hide tenant data hygiene issues.
- To make runtime-test-ready Job Position routes, assign the current user only after explicit user/admin confirmation, using `POST /positions/{id}/users` with a `UserIDs` array. This is a write operation and must never run automatically during ordinary generation.
- Placeholder or empty Job Position assignment is allowed only after the plugin has proven OAuth recovery failed or API lookup is genuinely unavailable, or after lookup found no suitable position and either the current user is not admin-capable or the user declined creation. That fallback must remain staged/blocking and cannot be signing/publish ready.
- If a required job position is missing and system-admin permission is not proven, stop generation and ask the user to have a system admin create/update the job position or provide an existing job position/fallback to use.
- If a required job position cannot be resolved during materialization, generated-final must carry unresolved proof metadata and preflight must block signing rather than silently replacing the task with applicant line manager.
- Job-position creation, update, assignment, and removal are write operations and must not be run automatically during workflow generation.
- Use `yeeflow-api-operator` for read-only org/reference lookup when real users/departments/locations/positions are needed and local credentials are available.
- API lookup may confirm that static exported values correspond to user, department, location, or position categories, but it does not prove runtime workflow routing.
- Use placeholders such as `<USER_REF_CONFIRMED_BY_API>`, `<DEPARTMENT_REF_CONFIRMED_BY_API>`, `<LOCATION_REF_CONFIRMED_BY_API>`, and `<POSITION_REF_CONFIRMED_BY_API>` in committed normalized examples.
- Do not paste API keys into chat and do not save raw API responses.
- Keep generated packages free of private user, department, location, position, tenant, and email data unless explicitly required and safe.
- Preserve multiple assignee entries under `properties.usertaskassignment[]`; mixed source arrays are export-proven in `Test ABC (1).yap`.
- Preserve `properties.issequential=true` for Sequential Appointed Order. Treat absent `issequential` as parallel/default only within the current export-proven boundary.
- Preserve `approveway` and `approvepercentage` together. Export-proven values include `allapprove`, `anyprocess`, `anyapprove`, `anyreject`, and `custompercentage`.
- Generate email notification fields only from export-proven shapes and only when explicitly requested. Do not claim notification delivery without runtime testing.
- Use `yeeflow-api-operator` assignment-routing coverage checks for authorized read-only confirmation of users, groups, group members, locations, positions, and position assignments. Do not invent org object IDs or embed private org data.
- User group assignment can now be API-category-assisted through documented `GET /groups` and `GET /groups/{id}/users`, but group expansion and routing still require runtime proof.
- Preserve absent `tasktype` for approval/default tasks and `tasktype="complete"` for Complete tasks. Do not invent another approval marker until a focused export proves it.
- Preserve due-date settings together. Export-proven due-date types are `hour`, `day`, and `express`; `minute` is product-documented but not export-proven in the studied files.
- Preserve `duedateexpress` when `duedatetype="express"` and keep it as an expression-button shape.
- Preserve `isfromworkcalendar` for day-based working-calendar due dates when found.
- Preserve due-date reminder rules under `notifyrules[]`; export-proven reminder timing values are `relative="0"` for on due date, `relative="-1"` for before due date, and `relative="1"` for after due date.
- Treat `notifyrules[].actiontype="1"` as the export-proven reminder action shape. Do not generate Automatic Treatment due-date actions until an export proves the serialized schema.
- Reminder recipient settings are export-proven only for task-level current task assignee email via `properties.to`; applicant, manager, department-manager, and arbitrary recipient reminder shapes were not found.
- Start action settings from `Test ABC (3).yap` use `StartNoneEvent.properties.terminate`, `terminate-conditions`, `revoke-conditions`, `isenabledemail`, `to`, `subject`, and `html`. Preserve condition rows as operand-wrapper arrays and do not claim terminate/recall/email runtime behavior without focused proof.
- Data-list workflow Start action from `Purchase Requests.ydl` uses `StartNoneEvent.properties.isenabledemail`, `to`, `subject`, `html`, and `taskurl`, but does not include `terminate`, `terminate-conditions`, or `revoke-conditions`. Do not add approval-form terminate/recall fields to data-list workflow Start actions until a data-list export proves them.
- Data-list workflow Assignment Task can reference list item context in assignee expressions. The studied export proves a Created By list-field expression that resolves `LineManager`; preserve this as an expression-button value and never replace it with invented static users.
- Data-list workflow task forms can mix normal task-form controls and list-bound controls. Preserve `isListControl`, list field `identifier`, `InternalName`, `fieldID`, and `____customListFields_` binding.
- Custom list fields can be marked `readonly=true` when the task should not update source list data. Default/native fields such as Created By appear read-only in the studied export; keep broader native-field behavior runtime-pending.
- Lay out generated workflow nodes with non-overlapping coordinates and keep `SequenceFlow` source/target plus node incoming/outgoing references consistent.
- For combined workflow-action baselines, include approval-form and data-list workflows in one package only when each workflow keeps its own Start-action rules, process/list IDs, `FlowMappings[]`, task forms, and sequence-flow references. Remap `Data.Forms[].ProcModelID` and data-list workflow `ListID`/`ProcModelID` completely.
- Scheduled Workflow Assignment Task from `Workflow Actions Runtime Baseline (1).yap` uses the same `MultiAssignmentTask.properties.usertaskassignment[]` family with `WorkflowType = 3`, `ListID = 0`, absent `tasktype`, `approveway="allapprove"`, `approvepercentage=100`, absent `issequential`, `duedatedefinition=120`, and one applicant-line-manager expression assignee. Preserve this as export-proven for Scheduled Workflow only; do not infer Scheduled Workflow support for direct users, positions, groups, Complete task, reminder rules, enabled task email, or data-list field assignee expressions until a scheduled export proves them.
- Scheduled Workflow Start from the same export has email fields but no approval-form terminate/recall fields. Keep Scheduled Workflow Start generation aligned with the data-list-style absence of terminate/recall unless another scheduled export proves those fields.
- Approval workflow task forms from `Workflow Actions Runtime Baseline (2)_Task forms.yap` are stored in `Data.Forms[].DefResource.pageurls[]` beside the submission form. Preserve `pageurls[].type=1` for submission forms and `pageurls[].type=2` for task forms when using this export family.
- Every generated `MultiAssignmentTask` should set `properties.taskurl` to an existing task form page ID. Reusing one task form for multiple Assignment Tasks is export-proven when the task-owner responsibilities match.
- Copied task forms for approve/reject or complete-only review can set copied value-entry controls to `readonly=true`; `WARTB Task` export-proves all copied value controls readonly.
- Task-specific task forms can keep copied request fields readonly and add editable task-owner fields. `WARTB Task2`, `WARTB Task3`, and `WARTB Task4` prove extra editable inputs, number, textarea, and user-picker-style controls on task forms.
- The Action Panel control is `type="workflowControlPanel"` with button behavior derived from the associated Assignment Task type and options. The export does not store explicit Approve/Reject/Complete/Reassign/Add assignee child buttons under the panel.
- Custom task buttons use `type="action_button"` and bind through `attrs.control_action` to a `formdef.actions[].id`. Keep that binding consistent with the button label and Submit form operation.
- Submit form operation shapes found in task forms are: no `submitType` for approval/default submit on Approval tasks, `submitType="2"` for reject, `submitType="4"` for reassign with `forword` and `remark`, `submitType="5"` for add assignee with `forword`, `remark`, and `assignee`, and no `submitType` for complete on Complete tasks. Preserve the export spelling `forword`.
- `Workflow Actions Runtime Baseline (2)_Task forms.yap` contains a custom-button binding mismatch: the visible `Add others to this task` button points to the reject action ID while a separate `Add assignee button clicked` action contains `submitType="5"`. The follow-up `Workflow Action Approval Test.ywf` corrects this binding so the Add others button resolves to the add-assignee action. Use the corrected `.ywf` shape as the positive export-proven reference, and keep validators warning-first when label, bound action, and Submit form operation disagree.
- `tools/generators/generate-workflow-task-form-runtime-baseline.mjs` applies the corrected `.ywf` definition to the studied task-form package family. The resulting package imported, opened, rendered the form designer and workflow designer, showed all four task forms in the selector, showed `WARTB Task3` custom buttons, and published successfully. Treat this as import/open/designer/publish proof only; custom button execution, reassign/add-assignee runtime behavior, Complete task execution, task-owner field persistence, Claim Task task-form behavior, and email delivery remain unproven.

## Runtime-Proof Requirements

Runtime routing claims require a focused baseline that proves:

- imported workflow designer accepts the generated assignment task assignee configuration
- request submission creates a pending task
- the pending task routes to the intended safe assignee class
- approval/complete outcomes route through expected sequence flows
- no private identities are exposed in reusable docs

Do not claim runtime behavior from export study alone.

## Focused Baseline Result

`tools/generators/generate-assignment-task-assignee-runtime-baseline.mjs` creates `assignment-task-assignee-runtime-baseline.v1.yap` from the export-proven shapes in `Test ABC (1).yap`. The package is intentionally ignored because it can contain tenant-local copied assignment references.

Local validation of the generated package is validator-backed for 11 Assignment Task nodes covering static user, multiple users, direct position, position by department, position by location, user group, Sequential appointed order, Parallel/default appointed order, `approveway` variants, custom percentage, and email notification configuration.

The first runtime pass imported and opened the generated app and showed Assignment Task designer panels, including Sequential selected for `issequential=true` and an Email Notification Config task panel. Publishing that first package was blocked by a missing-input-line designer error on `Sequential Multiple Assignees`, and no request was submitted. A rebuilt package with native-looking shape IDs and incoming/outgoing flow references passed local validation but hit duplicate import/open interference before clean publish proof.

The V2 runtime package fixed the duplicate-import/process-ID problem by using fresh app/process IDs, fresh form key `ATAR2`, semantic `rt_*` workflow shape IDs, complete `Data.Forms[0].ProcModelID` remapping, and a non-overlapping left-to-right workflow layout. V2 imported, opened, rendered the workflow designer, published successfully, and opened the published form.

Treat V2 as import/open/designer/publish proof only. Do not promote Assignment Task routing, group expansion, position expansion, appointed-order behavior, custom-percentage completion, or email delivery to runtime-proven until a safe request is submitted and observed.

`tools/generators/generate-workflow-actions-combined-runtime-baseline.mjs` extends the publish-proven V2 approach with one data-list workflow and additional Complete task, due-date, reminder, and Start settings. Use it for designer/open/publish proof first. Its `minute` due-date task is exploratory/product-documented because the studied exports proved `hour`, `day`, and `express` but did not prove the minute serialization.

The combined workflow-actions package imported, opened, rendered approval and data-list workflow designers, and published both workflows successfully. This upgrades the covered shapes to import/open/designer/publish-proven for the generated host package:

- approval Start terminate/recall/condition/email configuration
- representative Assignment Task assignee editor, appointed-order, task-type, and completion-condition panels
- generated Complete task, due-date, reminder, `approveway`, and notification configuration family at package/publish level
- data-list Start email configuration without terminate/recall controls
- data-list Assignment Task mixed direct/expression/list-item assignee configuration including the Created By/list-field expression family
- data-list form/list-field rendering in the imported app

Do not treat this as routing proof. No approval request or data-list item was submitted, no task was completed, no group/position/list-field expansion was observed, no due-date scheduler behavior was observed, and no notification email was sent.

## Product-Documented But Not Export-Proven Here

These were not found in the current exports or remain insufficiently proven:

- direct users only in one multi-user task, without another source type
- workflow variable assignee
- selected department manager as a distinct direct manager shape
- position by department plus location in one entry
- explicit `issequential=false` parallel marker
- quick-completion notification behavior
- notification delivery behavior
- standalone department detail endpoint and standalone position detail endpoint in the public API
- minute due-date unit serialization
- due-date Automatic Treatment action schema
- reminder recipients other than task owner/current assignee
- enabled Start terminate condition behavior
- Start recall runtime behavior
- Start email delivery
- data-list workflow Created By/list-field assignee routing
- data-list workflow task form save/edit behavior for list-bound controls
- scheduled workflow Assignment Task routing and task creation
- scheduled workflow Start email delivery
- scheduled workflow terminate/recall support
- Claim Task task-form association
- custom task-form button runtime execution for approve/reject/reassign/add-assignee/complete
- Action Panel explicit button child schema, because buttons appear derived rather than serialized as child controls in the studied export
- Add-assignee custom button runtime execution, even though the corrected `.ywf` now proves the intended button-to-action binding shape
- task-owner field save/persistence from custom task forms, even though the generated task-form baseline rendered and published

Do not generate these as schema-safe until an export proves their package shape. Do not claim routing-safe until runtime proof exists.

## Validator Rules

Validators should remain warning-first in compatibility mode and fail generated-final packages before signing, install, upgrade-check, package handoff, or runtime automation when assignment routing is unsafe:

- warn when `MultiAssignmentTask.properties.usertaskassignment` is missing, not an array, or empty
- warn when an assignee entry is not an object
- warn when `type` or `method` is missing
- warn when method is outside the export-proven method list
- warn when position-based assignments lack `position`
- warn when static direct/department/location assignments lack `value`
- warn when expression-based assignments lack an expression-button-shaped `value`
- warn that direct user assignment is tenant-sensitive
- warn when `tasktype` uses an unknown explicit value
- warn when due-date type, expression, working-calendar, or reminder rule shapes are malformed
- warn when Start action terminate/recall condition or email-notification fields are malformed
- warn when Scheduled Workflow Start action includes approval-only terminate/recall fields unless a scheduled export proves them
- warn when Scheduled Workflow Assignment Task uses list-field/Created By expression sources unless a scheduled export proves a list context for that workflow
- warn when `MultiAssignmentTask.properties.taskurl` is missing or does not resolve to a task form page
- warn when a custom task-form `action_button.attrs.control_action` is missing or does not resolve to `formdef.actions[]`
- warn when a custom button label implies approve/reject/reassign/add-assignee/complete but the resolved Submit form operation differs
- warn when reassign/add-assignee Submit form steps lack user-valued expression sources
- warn when Complete task pages rely only on approval/reject custom operations, or approval/default task pages rely only on complete custom operations
- hard-fail generated-final Assignment Tasks with empty assignees, placeholder assignee values, invented local/mock/generated job-position references, missing job-position proof status, missing required job positions without confirmed admin creation/update, unsupported manager expressions, direct user assignment without explicit app-plan/user request, or job-position write requirements that were not confirmed by a system admin
- hard-fail generated-final packages when assignment uses a job position that was not discovered, user-selected, or admin-created-after-confirmation

These hard gates prove only local package safety. Workflow assignment correctness is not proven until browser/runtime verification submits a safe request and observes task routing.

## References

Normalized export references:

```text
docs/studies/normalized/workflow-assignment-task-assignees.bundle.json
```

Study doc:

```text
docs/studies/collection-02.md#workflow-assignment-task-assignee-settings
```

## Claim Task Addendum

`Workflow Actions Runtime Baseline (3)_Claim task.yap` export-proves that front-end Claim Task maps to internal `CandidateTask` in approval-form and data-list workflows. See `docs/studies/collection-03.md#workflow-claim-task-action` and normalized refs under `docs/studies/normalized/workflow-claim-task.bundle.json`.

Generation rules:

- Use `CandidateTask` for Claim Task and `MultiAssignmentTask` for Assignment Task.
- Treat `CandidateTask.properties.usertaskassignment[]` as receiver/candidate pool configuration, not direct final assignee ownership.
- Preserve `CandidateTask.properties.taskurl` and validate it resolves to a task-form page.
- Preserve `tasktype="approve"` and `tasktype="complete"` when present.
- Do not generate the config-reference typo `properties.tasktype ` with a trailing space; the export uses `properties.tasktype`.
- Preserve user group, applicant/context, and data-list Created By/list-item receiver expressions as expression-button strings.
- Do not claim claim-pool behavior, claim locking, Pending Tasks ownership after claim, quick completion, or email delivery until focused runtime proof exists.

Claim Task should be recommended when a team/pool can voluntarily claim ownership. Assignment Task should be recommended when known users must take direct action.

## Set Variable Addendum

`Workflow Actions Runtime Baseline (4)_Set variable.yap` export-proves that front-end Set variable maps to internal `SetVariableTask` in approval-form and data-list workflows. See `docs/studies/collection-03.md#workflow-set-variable-action` and normalized refs under `docs/studies/normalized/workflow-set-variable.bundle.json`.

Generation rules:

- Use `SetVariableTask` for Set variable and `ContentList` for Set data list.
- Treat `properties.variablesetting[]` as workflow-variable assignments. Each row's `id` is the left-side target workflow variable and `value` is the right-side expression-token array.
- `formtype="current"` targets the current workflow's variables.
- `formtype="custom"` targets another approval form workflow instance and should preserve `properties.data.AppID`, `properties.data.ListSetID`, `properties.data.ProcKey`, and `properties.formids`.
- One Set variable node may set one or multiple workflow variables.
- In data-list workflows, list fields may appear as right-side `exprType="list_field"` values, but Set variable should still target workflow variables.
- Do not use Set variable to write data-list fields; use Set data list / `ContentList` for list record or field mutation.
- Do not claim runtime variable mutation, another-workflow updates, or form-id targeting behavior until focused runtime proof exists.

## Set Data List Addendum

`Workflow Actions Runtime Baseline (5)_Set data list.yap` export-proves that front-end Set data list maps to internal `ContentList` in approval-form and data-list workflows. See `docs/studies/collection-03.md#workflow-set-data-list-action` and normalized refs under `docs/studies/normalized/workflow-set-data-list.bundle.json`.

Generation rules:

- Use `ContentList` for Set data list and `SetVariableTask` for Set variable.
- Use Set data list for data-source item/field mutation; do not use Set variable to write data-list fields.
- Preserve `properties.listtype`: `select` for selected data sources, `current` for data-list current-list context.
- Preserve selected target metadata: `properties.appid`, `properties.listsetid`, and `properties.listid`.
- Preserve `properties.type` as `add`, `edit`, or `remove`.
- For add/edit, generate `properties.listdatas[]` entries with `Columns`, `Per`, and expression-token-array `Data`.
- For edit/remove, require explicit safe `properties.wheres[]` filters unless the user deliberately accepts a broad operation. Missing or empty filters should warn strongly.
- Treat `remove` as destructive and require explicit user intent plus disposable or tightly filtered target data before runtime execution.
- Preserve numeric operation codes `Per="0".."4"` from the export. Use codes `1..4` only for number fields when target field metadata is known.
- Preserve approval-form and data-list sub-list/detail-row value expressions when the source and target fields are export-backed.
- Do not claim add/update/delete, current-list mutation, document-library mutation, numeric operation execution, or sub-list row iteration without focused runtime proof.

## Signal Event Addendum

`Workflow Actions Runtime Baseline (6)_Signal event.yap` export-proves that front-end Signal event maps to internal `SignalEvent` in approval-form workflows. See `docs/studies/collection-03.md#workflow-signal-event-action` and normalized refs under `docs/studies/normalized/workflow-signal-event.bundle.json`.

Generation rules:

- Use `SignalEvent` only for approval-form Signal event branches unless another workflow host is export-proven.
- Treat Signal event as a special event source with no incoming flow and one or more outgoing flows.
- Preserve `properties.eventdefinitions[]` with `CancelEventDefinition` and/or `RevokeEventDefinition`.
- Use Signal event for recall/terminate compensation logic, often followed by Set data list / `ContentList` cleanup.
- Keep downstream edit/remove filters explicit and safe.
- Do not claim recall/terminate triggering or cleanup mutation without focused runtime proof.
