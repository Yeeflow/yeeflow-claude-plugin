# docs/standards collection 1

This file combines 16 documents from `docs/standards/` for the Claude directory build. A reference to `docs/standards/<name>.md` is the section that starts at the anchor `#<name>` below.

- [account-health-smoke-issue-summary.md](#account-health-smoke-issue-summary)
- [app-plan-standard-template.md](#app-plan-standard-template)
- [approval-workflow-designer-editability-standard.md](#approval-workflow-designer-editability-standard)
- [approval-workflow-final-layout-closure-standard.md](#approval-workflow-final-layout-closure-standard)
- [approval-workflow-route-orthogonality-standard.md](#approval-workflow-route-orthogonality-standard)
- [choice-field-option-materialization-standard.md](#choice-field-option-materialization-standard)
- [collection-toolbar-style-standard.md](#collection-toolbar-style-standard)
- [custom-capability-app-plan-planning-standard.md](#custom-capability-app-plan-planning-standard)
- [custom-code-form-action-step-runtime-standard.md](#custom-code-form-action-step-runtime-standard)
- [custom-service-nodejs22-runtime-standard.md](#custom-service-nodejs22-runtime-standard)
- [dashboard-collection-choice-pagination-standard.md](#dashboard-collection-choice-pagination-standard)
- [dashboard-collection-schema-dependency-closure-standard.md](#dashboard-collection-schema-dependency-closure-standard)
- [dashboard-dataset-composition-standard.md](#dashboard-dataset-composition-standard)
- [dashboard-page-layouts-custom-code-standard.md](#dashboard-page-layouts-custom-code-standard)
- [dashboard-page-layouts-workbench-standard.md](#dashboard-page-layouts-workbench-standard)
- [dashboard-runtime-binding-standard.md](#dashboard-runtime-binding-standard)

---

<a id="account-health-smoke-issue-summary"></a>

<!-- source: docs/standards/collection-01.md#account-health-smoke-issue-summary -->

# v0.6.5 Account Health Smoke Issue Summary

Date: 2026-06-01

Workspace: `/Users/Renger/Documents/Codex Projects/Yeeflow Test Applications`

Final package:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-container-width-fixed.yapk`

Final report:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-container-width-fixed.report.json`

## Scope

This smoke test used the installed Yeeflow App Builder v0.6.5 plugin resources only. The target was a YAPK-only Account Health smoke app with:

- Data list: Accounts
- Data list: Health Signals
- Current dashboard: Account Health Dashboard
- No YAP
- No custom forms
- No print page
- No workflow
- No Kanban or Collection

The package was installed/upgraded through guarded API helper calls. API acceptance was treated only as package install/upgrade acceptance, not as runtime render proof.

## Issue 1: Health Signals Was Not Modeled As Dependent Data

### What Was Found

The first generated package had two lists, but `Health Signals` did not contain a real lookup relationship to `Accounts`.

The new Data List Generation Standard audit reported:

`LOOKUP_TARGET_LIST_MISSING`

### Why It Happened

The first smoke package focused on the v0.6.5 structural YAPK gates: schema shape, API-issued IDs, native Title, `List.Items`, dashboard shell, and basic table bindings. It treated the account relationship as ordinary text-like signal data rather than a runtime-facing master/detail relationship.

That meant local schema checks could pass while the app still failed the new data-list quality standard. The validator saw lists and fields, but the data model was not business-usable because Health Signals could not resolve back to Accounts.

### Fix

Generated the corrected package:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-data-list-standard-fixed.yapk`

Changes:

- Kept `Accounts` as the master/reference list.
- Generated `Health Signals` after `Accounts`.
- Added a real Yeeflow lookup field on `Health Signals` pointing to `Accounts`.
- Set lookup target metadata with app/list-set/list/display-field information.
- Used `Accounts.Title` as the lookup display field.
- Seeded `Accounts` sample rows before `Health Signals` sample rows.
- Set Health Signal sample lookup values to valid Account sample row IDs.
- Expanded business-specific choice fields and default view field sets.

Result:

- Data List Generation Standard: pass
- Existing v0.6.5 gates: pass

## Issue 2: Optional Date Fields Failed Storage-Family Validation

### What Was Found

An intermediate corrected package failed the installed data-list schema gate with:

`FIELD_STORAGE_FAMILY_MISMATCH`

### Why It Happened

The generated optional date fields used a storage family spelling that did not match the installed v0.6.5 validator's expected `FieldName`/`FieldType` alignment. This was caught before handoff, during self-check.

The date fields were not required for the scoped smoke test. Keeping them would have increased risk without helping prove the requested Account Health list/dashboard behavior.

### Fix

Removed the optional date fields from the corrected package before final handoff, then re-signed and revalidated.

Result:

- Data-list schema gate: pass
- First-generation preflight: pass
- No date-field runtime risk carried forward

## Issue 3: Data List Default Views Installed But Rendered With No Display Fields

### What Was Found

After install, the runtime data views `All Accounts` and `All Health Signals` still showed no display fields.

### Why It Happened

The generated list layouts had `Layouts[].LayoutView.layout` entries, but the view records were too minimal for runtime materialization. They were missing export-proven data-view metadata, especially:

- `Layouts[].Ext1 = {"Url":"default"}`
- export-shaped visible column objects with `FieldID`, `FieldName`, `DisplayName`, `Order`, `Mobile`, and `Show`
- full list-view `LayoutView` keys such as `layout`, `query`, `sort`, `rowColor`, and `filter`

The local validators checked that view fields existed, but they did not catch the runtime requirement that the data-view record itself needs the fuller export-shaped view metadata.

### Fix

Generated and upgraded:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-data-list-view-fixed.yapk`

Changes:

- Renamed default views to export-proven `All Items`.
- Set each default view `Type = 0`.
- Set `IsDefault = true`.
- Set `IsItemPerm = false`.
- Added `Ext1.Url = "default"`.
- Rebuilt visible columns as export-shaped objects with field IDs and display metadata.
- Preserved meaningful visible fields:
  - Accounts: `Title`, `Text2`, `Text4`, `Text3`, `Text1`, `Decimal5`
  - Health Signals: `Title`, `Text1`, `Text2`, `Text4`, `Text3`

Result:

- Upgrade check: passed
- Upgrade apply: applied
- User confirmed the data-list display-field issue was fixed

## Issue 4: Dashboard Page Rendered Blank

### What Was Found

After the data-list view fix, the `Account Health Dashboard` page opened but was blank.

### Why It Happened

The embedded dashboard `LayoutInResources[0].Resource` was shaped as a direct `Main` object. Local validation accepted `Main > Content`, but the runtime dashboard renderer expected the full export-proven page resource wrapper.

The missing top-level page keys were:

- `children`
- `attrs`
- `title`
- `ver`
- `filterVars`
- `tempVars`
- `exts`
- `actions`

Because the page resource did not match the runtime page-builder shape, Yeeflow opened the dashboard but did not materialize the controls.

### Fix

Generated and upgraded:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-dashboard-content-fixed.yapk`

Changes:

- Rewrapped the dashboard resource as an export-proven page object.
- Kept `Main > Content` inside top-level `children`.
- Added top-level `attrs`, `title`, `ver`, `filterVars`, `tempVars`, `exts`, and `actions`.
- Added visible dashboard controls:
  - 3 native heading/Text controls
  - 2 dashboard data-table controls
- Preserved table columns with both `Field` and `FieldName`.

Result:

- Upgrade check: passed
- Upgrade apply: applied
- User confirmed dashboard controls became visible

## Issue 5: Dashboard Containers Showed Custom Width 0px

### What Was Found

After dashboard controls appeared, the `Main` and `Content` containers showed custom width with `0px`.

### Why It Happened

The generated dashboard containers included:

`attrs.style.width = "100%"`

Yeeflow Designer did not interpret that as a valid width preset. It treated the field as a custom width configuration without a valid numeric value, causing the UI to show custom width `0px`.

This was an invented container style shape. The plugin guidance says dashboard page padding should be on page-level `attrs.container.padding`, and `Main` should remain structural.

### Fix

Generated and upgraded:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-container-width-fixed.yapk`

Changes:

- Removed invented `attrs.style.width` from `Main` and `Content`.
- Removed custom style/common width settings from those containers.
- Kept `Main` and `Content` as structural containers.
- Added `nv_label` values for cleaner designer navigation.
- Kept `displayLabel = false`.
- Moved page padding to page-level `attrs.container.padding`.

Result:

- Upgrade check: passed
- Upgrade apply: applied
- User confirmed the container width issue was fixed

## Validation Summary

The final package passed:

- Canonical YAPK schema validation
- Schema-v3 YAPK hardening validator
- Data-list schema validation
- First-generation preflight
- Signing and `verifysign`
- Data List Generation Standard checks
- Runtime manual checks for:
  - data-list display fields
  - dashboard controls rendering
  - container width settings

The validator still reports the proof-boundary code:

`YAPK_CONTENT_GENERATION_RUNTIME_PROOF_REQUIRED`

That is expected. It means local/API validation does not automatically prove every runtime UI behavior; manual runtime review remains required.

## Root Cause Analysis

The main pattern across the issues was that local schema/API acceptance was weaker than runtime materialization.

Three different layers were involved:

1. Structural schema validity:
   The YAPK could be schema-valid, signed, verified, installed, or upgraded.

2. Local generator validation:
   The package could pass local checks for fields, IDs, dashboard shell, `Main > Content`, and table bindings.

3. Runtime designer/materialization behavior:
   Yeeflow still required export-shaped view metadata, full dashboard page resource wrappers, and valid designer style settings.

The missing pieces were not raw schema fields alone. They were runtime-facing conventions learned from exports:

- Data views need `Ext1.Url` and export-shaped column objects.
- Dashboard resources need the full page wrapper, not only a `Main` node.
- Container widths should use known positioning/style patterns or omit width settings; invented style keys can map to broken Designer state.

## Lessons For Future Generation

- Treat API install/upgrade success as acceptance only, not runtime render proof.
- For data-list default views, require export-shaped `Layouts[].Ext1` and column objects, not just a non-empty `LayoutView.layout`.
- For dashboard pages, require full page resource keys: `children`, `attrs`, `title`, `ver`, `filterVars`, `tempVars`, `exts`, and `actions`.
- Do not invent container width/style settings.
- Keep `Main` structural and apply dashboard page padding at page-level `attrs.container.padding`.
- Promote runtime feedback into validators where possible, because these gaps passed earlier local checks.

## Final State

Final upgraded package:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-container-width-fixed.yapk`

Final redacted report:

`/Users/Renger/Downloads/v0.6.5-runtime-smoke-account-health-container-width-fixed.report.json`

No YAP was generated. No secrets, raw IDs, tenant IDs, workspace IDs, upload IDs, private URLs, raw API responses, raw `Resource`, raw `Sign`, or decoded payloads were printed. No commit, push, or tag was made.

---

<a id="app-plan-standard-template"></a>

<!-- source: docs/standards/collection-01.md#app-plan-standard-template -->

# <Application Name> - Yeeflow App Plan

Use this template as Stage 2 / Step 2 for every Yeeflow application build. This plan must be created from the approved Functional Specification, which is the business source of truth for process, rules, data, roles, dashboard content, reporting, and audit needs. Its purpose is to convert business requirements into Yeeflow-standard resources and define the correct generation order.

Canonical schema rule: this Markdown document is the single primary App Plan contract and must use the title shape `# <Application Name> - Yeeflow App Plan`. Validators, generation contracts, traceability checks, and lifecycle docs must treat `yeeflow-app-plan.md` in this resource-order format as canonical. Do not require a conflicting `Yeeflow Application Plan` schema as the primary App Plan artifact; any compatibility entrypoint must validate this same Markdown contract.

This document is the implementation contract for package generation after user approval. It must use only Yeeflow resource types, field types, variable types, controls, Dynamic controls, workflow nodes, form actions, Collection/Kanban actions, Sub List actions, property paths, bindings, and configuration shapes supported by the active `@yeeflow-app-builder` plugin knowledge base, skills, standards, validators, template library, control/property knowledge base, extension registry, or export-proven references.

Primary artifact rule: generate this plan as the primary human-readable Markdown file named `yeeflow-app-plan.md`. Machine-readable JSON may exist only as a companion/projection artifact for validation, traceability, or tests, such as `app-plan.trace.json`; JSON must not replace this Markdown document. The Markdown file must contain the Yeeflow resource plan and detailed Dashboard Pages Plan with legal Yeeflow control-type planning, not just a link to JSON.

## 1. Plan Status

- Application name:
- Functional specification path:
- Functional specification status:
- Planning plugin:
- Plugin version:
- Source artifacts:
- Current phase:
- Package target: `.yapk` by default / `.yap` only if explicitly requested
- Generation status:
- Approval status:
- Known blockers:

## 2. Requirement-to-Yeeflow Resource Mapping Summary

Map each major requirement from the Functional Specification to Yeeflow resources.

| Requirement ID | Requirement Area | Business Requirement | Yeeflow Resource Type | Planned Resource Name | Required | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| REQ-DATA-01 | Data management | <Requirement> | Data list or Document library | <Name> | Yes/No | Select one exact Yeeflow resource type |
| REQ-APPROVAL-01 | Approval/review | <Requirement> | Approval form | <Name> | Yes/No | <Notes> |
| REQ-REPORT-01 | Approval output | <Requirement> | Form report | <Name> | Yes/No | Must be based on a specific Approval form |
| REQ-VIEW-01 | List browsing | <Requirement> | Data List view | <Name> | Yes/No | Must belong to a specific Data list or Document library |
| REQ-UI-01 | Dashboard/workbench | <Requirement> | Dashboard page | <Name> | Yes/No | Page-level UI resource, not a Form report |
| REQ-WORKFLOW-01 | Automation | <Requirement> | Data-list workflow / Schedule workflow / Form action | <Name> | Yes/No | <Notes> |
| REQ-AI-01 | AI | <Requirement> | AI Agent / Copilot / AI Assistant node | <Name> | Yes/No | <Notes> |

Rules:

- Every core business requirement must map to a Yeeflow resource, a supported configuration, or an explicitly deferred item.
- Give every core Functional Specification requirement a stable `REQ-<AREA>-NN` or `FS-<AREA>-NN` marker and repeat that exact marker in this mapping table and any detailed App Plan row that implements it. Generic words such as `field`, `page`, `workflow`, or `dashboard` are not substitutes for item-level traceability.
- App Plan dashboard planning must trace back to the Functional Specification's business-level dashboard questions, source business objects/data lists, summary metrics, source fields, calculation logic, data regions, display fields, filters, sorting/grouping, user actions, mobile support, and alerts.
- Functional Specification to App Plan traceability is executable with `scripts/validate-functional-spec-to-app-plan-traceability.mjs --spec <functional-spec.md> --plan <app-plan.md>`.
- Form report is a standalone Yeeflow resource type created from a specific Approval form. Do not merge Form report planning with Dashboard page planning or Data List view planning.
- Do not include resources only to make the plan look complete. Every generated resource must serve a runtime purpose.
- Do not invent unsupported controls, Dynamic controls, field types, workflow node types, variable types, action shapes, property paths, bindings, or configuration shapes.
- Validator wording tolerance: the App Plan should follow this canonical template, but validators must also accept equivalent business wording for the same required intent, such as `Submission fields`, `Task page fields`, `Form reports are independent approval-based resources`, `Form reports are separate from Dashboard/page planning`, `Unsupported shapes should not be generated`, and `No Sub List actions are needed`. Equivalent wording does not relax resource-order, schema, or generation-readiness requirements.

## 3. Resource Generation Order

Define the exact order in which resources must be generated.

| Order | Resource Type | Resource Name | Depends On | Reason for Order | Blocking If Missing |
| --- | --- | --- | --- | --- | --- |
| 1 | Data list | <Parent/master list> | None | Lookup parent must exist first | Yes/No |
| 2 | Data list | <Child/transaction list> | <Parent list> | Needs lookup to parent | Yes/No |
| 3 | Approval form | <Form> | <Lists/fields> | Needs lookup/sublist/field bindings | Yes/No |

Standard order:

1. Data lists and Document libraries.
2. Approval forms and approval workflows.
3. Form reports based on their related Approval forms.
4. Schedule workflows.
5. AI Agents.
6. Copilots.
7. Custom Data List forms.
8. Data List workflows.
9. Notifications.
10. Data List views.
11. Dashboard pages.
12. Application navigation.
13. Target users, roles, groups, and permissions.

Rules:

- Parent/master data lists must be generated before child/transaction lists that depend on them.
- Lookup target lists and display fields must exist before dependent Lookup fields are generated.
- Approval forms that use list lookups, sublists, summaries, Query data, or Set data list actions must depend on those lists.
- Dashboards must depend on the lists, views, forms, actions, and variables they display or trigger.

## 4. Data Lists and Document Libraries Plan

Repeat this subsection for each Data list or Document library.

### 4.x <List or Library Name>

- Selected Yeeflow resource type: Data list or Document library
- Package/navigation representation: Type `1` Data list / documented Document library representation / `export-learning-required`
- Description:
- Business purpose:
- Master/transaction/reference:
- Creation order:
- Depends on:
- Used by:
- Navigation visibility:
- Sample/reference data required: Yes/No

#### Fields

| Field Order | Business Label | Display Name | Internal ID / Field Key | Exact Yeeflow Field Type | Exact Yeeflow Control Type | Support Source | Proof Label | Fallback / Deferred Reason | Required | Unique | Default Value | Placeholder | Validation Rules | Choice Values | Lookup Target | Lookup Display Field | Additional Lookup Fields | Allow Barcode / QR Scan | Description |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Contract title | Title | Title | Title | input control | plugin-known field/control type | validator-backed | N/A | Yes | No | | <Placeholder text> | | | | | | No | Native title field |

Field rules:

- Field types must follow active plugin Data List / Document Library field standards.
- Distinguish business-friendly labels from exact Yeeflow implementation types. `Business Label` may say `Owner/person`, but `Exact Yeeflow Field Type` must say the exact plugin-supported type such as `User`.
- Do not use slash-combined final implementation types such as `Title/Text`, `Currency/Number`, `User/person`, `Attachment/File upload`, `Document library / Data list`, or `Type 1/document library` unless the row is marked `export-learning-required`, `runtime-proof-required`, or `deferred` with a fallback/deferred reason.
- Document Library planning must select one clear Yeeflow resource type. If the package/navigation representation is not known, mark it `export-learning-required` or `runtime-proof-required` and do not treat the resource as generation-ready.
- Keep the native `Title` field unless the plugin standard explicitly allows otherwise.
- Choice fields must define runtime-visible choices using the supported schema.
- Lookup fields must identify target list, display field, and additional fields when needed.
- User/person fields must use supported User field shapes.
- Date fields must use supported Date/DateTime types.
- Currency/number fields must use supported numeric field types.
- Multiline notes, descriptions, and justifications must use supported multiline text fields.
- Barcode, serial number, asset tag, and external code values should use text unless a supported barcode-specific field is required.
- Set `Allow Barcode / QR Scan = Yes` only for Text fields rendered with the input control. Generated fields then serialize `Rules = {"allowScan":true}`; never infer scanning from a field name alone.
- Placeholder text must be defined for user-entered fields when the exact selected field/control type supports placeholders. If placeholder support is unknown, mark the row `runtime-proof-required`, `export-learning-required`, or `deferred` with reason, fallback, and proof/generation impact.

#### List Relationships

| Related List | Relationship | Field | Required | Notes |
| --- | --- | --- | --- | --- |
| <List> | parent/child/reference | <Lookup field> | Yes/No | <Notes> |

#### Sample or Reference Rows

| Row Purpose | Key Values | Depends On | Required for Validation | Notes |
| --- | --- | --- | --- | --- |
| <Purpose> | <Values> | <Lookup rows> | Yes/No | <Notes> |

## 5. Approval Forms Plan

Repeat this subsection for each Approval form.

### 5.x <Approval Form Name>

- Approval form name:
- Description:
- Business purpose:
- Submitter role:
- Approval status lifecycle:
- Depends on Data lists / Document libraries:
- Form reports required: Yes/No
- Assignment task required: Yes/No
- Generation blocker if missing: Yes/No

#### Submission Form Fields

| Field Order | Business Label | Field Name | Field ID / Variable ID | Exact Yeeflow Variable Type | Exact Yeeflow Control Type | Choice Values | Support Source | Proof Label | Fallback / Deferred Reason | Binding | Read Only | Required | Default Value | Placeholder | Dynamic Display | Custom Validation | Lookup Target | Lookup Display Field | Additional Lookup Fields | Sublist/Summary Notes | Description |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <Business label> | <Field> | <ID> | <Exact variable type> | <Exact control type> | <Explicit values for Choice; N/A otherwise> | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Binding> | Yes/No | Yes/No | <Default> | <Placeholder text> | <Rule> | <Rule> | <List> | <Field> | <Fields> | <Notes> | <Description> |

Submission form rules:

- Variable and control types must follow active plugin Approval Form standards.
- Text fields must not use select or radio controls. Choice fields must use an approved choice control and list explicit business values in `Choice Values`; for example, `Leave Type` is `Choice` + `select`, not `Text` + `select`.
- Choice/select values must appear in the `Choice Values` column. A value list written only in `Validation Rules` or `Custom Validation` does not satisfy the planning contract.
- Distinguish business labels from exact Yeeflow implementation types. Slash-combined or vague implementation wording is not generation-ready unless marked `runtime-proof-required`, `export-learning-required`, or `deferred`.
- Lookup fields must identify target list, display field, and additional fields.
- Sublist fields must define row fields, summary fields, and any summary-to-form-field bindings.
- Default values, dynamic display rules, read-only state, and validations must be explicit.
- Placeholder text must be planned for editable fields when the exact selected variable/control type supports placeholders. If support is unknown, mark the row `runtime-proof-required`, `export-learning-required`, or `deferred` with reason, fallback, and proof/generation impact.

#### Approval Workflow Nodes

| Step Order | Node Name | Node Type | Purpose | Assignee / Actor | Configuration | Branches | Branch Conditions | Data Read/Write | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Start | <Node type> | <Purpose> | <Actor> | <Config> | <Branches> | <Conditions> | <Data> | <Notes> |

When an Assignment Task uses a direct Job Position assignee, add or include Job Position proof columns before generation: `Required Job Position`, `Job Position ID`, `Job Position Source`, `Job Position Proof Status`, `Job Position OAuth Status`, `OAuth Refresh Status`, `Job Position Lookup Status`, `Job Position Lookup Attempted`, `Job Position Create Attempt Count`, `Job Position Create Response ID Recorded`, `Job Position Duplicate Scan`, `Job Position Creation Confirmed`, and `Job Position Admin Confirmed`. API-assisted Job Position routing is not generation-ready if OAuth refresh/lookup proof is missing.

When `Node Type = SetVariableTask`, add a `Set Variable Assignments` column using `VariableId :: variableType :: Business Name :: JSON expression token array`, separated by `;;` for multiple assignments. Exact assignments are mandatory; never use an empty/default placeholder.

#### Workflow Set Data List Action Plan

| Workflow Host | Workflow Name | Node Name | Target Mode | Target Resource | Target Resource Type | Operation | Mappings JSON | Filters JSON | Workflow Variable Declarations JSON | Batch Source Type | Batch Source | Batch Source Fields JSON | Parent Loop | Proof Boundary | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Approval Form / Data List / Scheduled | <Workflow> | <ContentList node> | current/select | <Host or selected resource> | Data List / Document Library | add/edit/remove | <JSON array of Columns, Per, Data> | <JSON array of where conditions> | <JSON array of id, type, valueType, name, expressionName, and optional key; [] when unused> | Workflow List Variable / Data List Sub List Field / blank | <List variable id or Sub list field id> | <JSON array of child field IDs> | <Loop name or blank> | export-proven/runtime-proof-required/export-learning-required | <Business intent> |

Rules:

- Approval Form and Scheduled workflows must use `Target Mode = select`.
- Data List workflows may use `current`; preserve the export-proven `current + add` shape for current-record mutation.
- `add` and `edit` require a non-empty `Mappings JSON` array. `edit` and `remove` require non-empty, field-specific `Filters JSON`.
- Approval Form and Scheduled workflow source expressions may use declared `variable`, `application`, or `loop_ctx` tokens. Data List workflows may additionally use current-record `list_field` tokens. `workflow-field` is not a valid workflow source token.
- Every `exprType: "variable"` token in mappings or filters must exactly match one entry in `Workflow Variable Declarations JSON` by `id`, optional child-field `key`, case-normalized `valueType`, and canonical `expressionName`. Each declaration must also include the workflow variable's actual `type` and business `name`.
- Numeric operation codes are `0` Value, `1` Increase, `2` Decrease, `3` Multiply, and `4` Divide. Codes `1` through `4` require a numeric target field.
- Document Library targets are export-proven for Type 16 node shape. Add operations must map `Title`, `Text4` (Upload file), and `_Path`; mark actual file mutation execution `runtime-proof-required` until disposable-library runtime proof exists.
- When `Parent Loop` is nonblank, the node is materialized inside the named `LoopBody`; the plan must also contain an exact Workflow Loop Planning row. It must never be flattened into a host graph or recreated as repeated static nodes.

Workflow rules:

- Node types must come from the active plugin workflow-node knowledge base.
- Branches must cover normal, exception, empty, and unexpected routing values where relevant.
- Assignment task nodes require Task form planning.
- Query data and Set data list actions must identify target application, list, filters, fields, and write behavior.

#### Assignment Task Forms

Required when the approval workflow contains Assignment task nodes.

| Task Form Name | Used By Workflow Node | Purpose | One Shared Form or Multiple Forms | Notes |
| --- | --- | --- | --- | --- |
| <Task form> | <Node> | <Purpose> | Shared/Specific | <Notes> |

##### Task Form Fields

| Field Order | Business Label | Field Name | Field ID / Variable ID | Exact Yeeflow Variable Type | Exact Yeeflow Control Type | Choice Values | Support Source | Proof Label | Fallback / Deferred Reason | Binding | Read Only | Required | Default Value | Placeholder | Dynamic Display | Custom Validation | Lookup Target | Lookup Display Field | Additional Lookup Fields | Sublist/Summary Notes | Description |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <Business label> | <Field> | <ID> | <Exact variable type> | <Exact control type> | <Explicit values for Choice; N/A otherwise> | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Binding> | Yes/No | Yes/No | <Default> | <Placeholder text> | <Rule> | <Rule> | <List> | <Field> | <Fields> | <Notes> | <Description> |

#### Approval Form Layout Template Selection

| Approval Form | Form Page | Page Role | Selected Approval Form Layout Template | Business Sections Needed | Related Data Needed | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- |
| <Approval form> | <Submission form> | Submission | approval_form_layout_submission_v1_1 | <Sections> | <Related data or None> | <Reason> | Generated-final validation |
| <Approval form> | <Task form> | Task | approval_form_layout_task_v1_1 | <Sections> | <Related data or None> | <Reason> | Generated-final validation |

Approval form layout rules:

- Every Approval form has exactly one submission form page. It must select `approval_form_layout_submission_v1_1`.
- Every generated task form page must select `approval_form_layout_task_v1_1`.
- The selected template must come from `docs/reference/approval-form-layout-templates.json`.
- The App Plan may describe business sections and related data needs, but must not include generated `ListID`, `FormID`, `ProcModelID`, `FlowKey`, `DefResourceID`, runtime IDs, JSON property paths, or copied control payloads.
- Approval form pages must not select Data Analytics templates, chart templates, pivot table templates, Summary/KPI analytics, or `kpi_metrics_wrapper`.
- Task form field controls should be readonly unless this table or the Task Form Fields table states a business reason for assignee-editable fields.

#### Approval Form Fields Layout Template Selection

Required for every generated Approval form submission or task page that displays Approval form fields.

| Approval Form | Form Page | Field Group | Selected Approval Form Fields Layout Template | Field Source | PC/Laptop Columns | Tablet Columns | Mobile Columns | Full-Row Field Controls | Dynamic Display Grouping | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Approval form> | <Submission or task form> | <Field group> | approval_form_fields_grid_2col_v1_1 or approval_form_fields_grid_3col_v1_1 | Submission fields or task fields | 2 or 3 | <= PC/laptop | 1 | Multiple line / Rich text / Sub list fields | <None or grouping rule> | Generated-final validation |

Approval form field-layout rules:

- Select `approval_form_fields_grid_2col_v1_1` or `approval_form_fields_grid_3col_v1_1` from `docs/reference/approval-form-field-layout-templates.json` for every generated field group on an Approval form submission or task page.
- Field-grid template selection is required in addition to the page-level Approval Form Layout Template Selection table.
- When the form page uses Approval Form Layouts v1.1, place the selected field-grid wrapper only inside `content_card_wrapper > section_content_area`.
- Every generated Approval form field control inside the selected field-grid wrapper must explicitly set margin to zero and must use a business-specific `nv_label` or `nav_label`.
- Submission and Task form fields must be projected from one normalised field model. Only role properties such as readonly, required, and visibility may differ; fields with the same binding must retain the same Yeeflow control type on every page.
- Only standard Yeeflow control types may be planned or materialized. `drop` is invalid. Choice controls must declare real options. Every editable Sub List row field must have a business placeholder: `Enter <field>`, `Select <field>`, or `Upload <field>` as appropriate.
- The generated-final package and the persisted MCP readback must pass `scripts/validate-approval-form-control-closure.mjs`; it recursively scans `pageurls[].formdef` and Sub List row controls.
- Multiple line, Rich text, and Sub list controls must span the full parent Grid width on every responsive breakpoint.
- Tablet columns must not exceed PC/laptop columns; mobile columns must be one.
- App Plan selection is a business/layout decision only. It must not include generated `ListID`, `FormID`, `ProcModelID`, `FlowKey`, field runtime IDs, JSON property paths, placeholder IDs, copied control JSON, or runtime payload fields.

#### Form Actions and Temp Variables

| Action Name | Step Name | Host Resource | Host Form | Host Surface / Page | Trigger | Exact Step Type | Query Mode | Source Resource Type | Source Resource | Filters | Sorts | Result Target Type | Result Target | Field Mapping | Count Target Type | Count Target | Page Size | Page Number | Persistence / Lifetime | Bound Control | Result Consumer / Use | Proof Boundary | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Action> | <Step> | <Approval form / Data List / Dashboard name> | <Submission / Task / Print / Custom form / Dashboard page> | Approval Submission / Task / Print / Data List New / Edit / View / Dashboard | Page Load / Field Change / Submission / Button | Query Data / Set variable / Confirm / Submit / other proven type | single_to_variables / single_to_temp_variables / single_to_list_fields / single_to_list_fields_and_temp_variables / multiple_to_sublist / multiple_to_list_sublist / multiple_to_temp_collection / multiple_count_only / N/A | Data List / Document Library / Form Report / Data Report / N/A | <Resource> | <Field/operator/value or None> | <Field asc/desc or None> | Workflow variables / Temp variables / Current Data List fields / Current fields + Temp variables / Approval Sub list / Current record Sub list / Temp collection / None | <Target id/name or None> | <Source field -> target id; ... or None> | Workflow number / Temp variable / None | <Target id/name or None> | 100 default / 1..1000 | 1 default / positive integer | Persisted instance / Current page session / N/A | <Control or trigger> | Text / calculation / Custom Code / persisted field / editable Sub list / None | export-proven / runtime-proven / focused-proof-required | <Notes and business rationale> |

Set Variable rows must additionally state in `Notes and business rationale`: exact target kind and ID, RHS fixed value or expression, optional condition, `continue` behavior, and the Dynamic Display/field/control consumer. Follow `docs/standards/collection-02.md#set-variable-golden-reference-standard`.

##### Form Action Set Variable Planning

Required whenever a Form Action contains `Set variable` or `Start another action`. One row represents one assignment. Repeat `Action Name` and `Step Order` to build a multi-variable step.

| Host Resource | Host Form / Page | Host Type | Action Name | Step Order | Step Name | Trigger | Bound Control / Field | Target Kind | Target ID | Target Value Type | RHS Expression Tokens | Condition Tokens | Continue | Start Another Action Target | Result Consumer / Use | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Resource> | <Submission form / Task form / Custom form / Dashboard> | Approval / Data List Form / Dashboard | <Business action name> | 1 | <Business step name> | Page Load / Field Change / Button Click / Container Click / None | <Control binding, nv_label, or None> | Workflow variable / Temp variable / Current list field / None | <Exact target ID or None> | <Exact Yeeflow value type or N/A> | <JSON expression token array or N/A> | <JSON expression token array or None> | true / false | <Action name or None> | <Dynamic Display, readonly derived field, persisted field, or other consumer> | export-proven / validator-backed / runtime-proof-required |

Rules:

- `RHS Expression Tokens` must be a JSON array. Do not replace `currentUser`, `getUserAttr`, `getOrgAttr`, `isInGroup`, or nested expressions with prose.
- `Temp variable` targets are declared on the same page and serialized with the `__temp_` runtime prefix. Approval workflow-variable targets keep their exact workflow variable ID.
- `Current list field` is allowed only on a Data List custom form. Dashboard and Approval Form Actions cannot target Data List fields.
- `Page Load` binds through `formAction.onLoad`; `Field Change` binds through the source control's `attrs.control_event_rule`; button/container triggers bind through `attrs.control_action`.
- `Start Another Action Target` must resolve to another action on the same page. Use it to chain derived-value actions such as Requester -> Department/Department name.
- Approval field rows that consume Set Variable output must set `Read Only = Yes` for derived display values. When generated Dynamic Display behavior is required, the field row's `Dynamic Display` cell must contain an exact JSON rule array; generation rewrites every rule `controlId` to the containing control ID and rejects undeclared variable references.
- Data List Workflow `SetVariableTask` cannot update the current record. Use a `ContentList` / Set Data List node with `listtype = "current"` for current-item field writes.
- A bulk Sub list write is an export-proven `ContentList` `add` action, not a Loop substitute: use `key: "_list.<childField>"` in each row-derived mapping. Approval Form and Scheduled workflows use `exprType: "variable"` from a declared Workflow List variable; Data List workflows use `exprType: "list_field"` from a current-record Sub list field. Declare the corresponding Batch Source columns above, use an explicit selected target, and include a parent-form/list association mapping such as Applicant, Employee, or instance ID whenever the target rows need to remain traceable to their source.

##### Form Action Set Data List Planning

Required whenever an Approval Submission/Task form, Data List or Document Library custom form, or Dashboard Form Action contains `Set Data List`. One row represents one step and `Step Order` is the order inside the named action. Approval Print Page Set Data List remains `focused-export-proof-required`.

| Host Resource | Host Form / Page | Host Type | Action Name | Step Order | Step Name | Trigger | Bound Control | Exact Step Type | Operation | Target Mode | Target Resource Type | Target Resource | Field Mapping JSON | Filter JSON | Execution Condition Tokens | Continue When Not Met | Status Target Kind | Status Target ID | Item Result Target Kind | Item Result Target ID | Item Result Attribute | Proof Boundary | Business Rationale |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Resource> | <Submission form / Task form / New/Edit/View form / Dashboard> | Approval Submission / Approval Task / Data List New / Data List Edit / Data List View / Document Library New / Document Library Edit / Document Library View / Dashboard | <Business action> | 1 | <Business step> | Page Load / Field Change / Button Click / Container Click | <Control identity or None> | Set Data List | add / edit / remove | current / select | Data List / Document Library / None | <Exact resource or None> | <JSON listdatas array or None for remove> | <JSON wheres array or None for add/current> | <JSON expression token array or None> | true / false | Temp variable / Workflow variable / None | <Exact ID or None> | Temp variable / Workflow variable / None | <Exact ID or None> | itemid / totalcount / None | export-proven / validator-backed / runtime-proof-required | <Persistence intent and risk rationale> |

Rules:

- `current` is valid only on Data List/Document Library custom forms and means an immediate update of the current record. It does not require a selected target resource or filters.
- On New/Edit forms, prefer `Set Variable` plus `Submit Form` for ordinary field changes. A direct current-record Set Data List write is allowed only with an explicit immediate-side-effect rationale.
- View Item forms cannot persist through Submit Form; use `current + edit` when a business command must update the viewed record.
- `select + add` requires field mappings. `select + edit/remove` requires explicit non-empty filters so the mutation scope is bounded.
- Target resources are Data Lists or Document Libraries. Form Report/Data Report are not Set Data List mutation targets.
- `Per` in `Field Mapping JSON` is `0` Value, `1` Increase, `2` Decrease, `3` Multiply, or `4` Divide. Arithmetic modes are valid only for compatible numeric target fields.
- `Status Target` and `Item Result Target` are optional. When configured, they must resolve to declared variables. Dashboard result targets must be page temp variables.
- Mutually exclusive conditional Add/Update steps normally set `Continue When Not Met = true`, allowing the next branch step to be evaluated.
- Form Action Set Data List cannot expand Sub List rows into multiple target records. Use Workflow Set Data List for bulk Sub List-to-Data List/Document Library writes.
- Document Library Add must map one single-file value to native Upload File `Text4`. `_Path` is the export-proven folder pseudo-field. Multi-file, array, List, and Sub List upload sources require Workflow Set Data List.
- Public Forms cannot contain Set Data List Form Action steps.
- Use `docs/standards/collection-02.md#form-action-set-data-list-golden-reference-standard`, run `validate-form-action-set-data-list-plan.mjs` before materialization, and run `validate-form-action-set-data-list.mjs --strict-generated` before signing.


##### Form Action Open Resource Planning

Required whenever a Form Action opens an Item Form, Approval Form, or Dashboard. One row represents one step; use exact step types `listitem`, `openform`, and `opendashboard`.

| Host Resource | Host Form / Page | Host Type | Action Name | Step Order | Step Name | Trigger | Bound Control | Exact Step Type | Operation Type | Target Mode | Target Resource Type | Target Resource | Item / Form ID Expression Tokens | Selected Custom Form | Default / Set Variables JSON | Query Parameters JSON | Open Mode | Modal Size | Custom Width | Return Item ID Temp Variable | Execution Condition Tokens | Continue When Not Met | Business Rationale | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Resource> | <Submission form / Task form / Custom form / Dashboard> | Approval Submission / Approval Task / Data List New/Edit/View / Document Library New/Edit/View / Dashboard | <Business action> | 1 | <Business step> | Button Click / Container Click / Field Change / Page Load | <Exact control identity> | listitem / openform / opendashboard | add / edit / view / new / submitted / open | current / select / N/A | Data List / Document Library / Approval Form / Dashboard | <Exact target> | <JSON token array or None> | <Target custom form or None> | <JSON array or None> | <JSON array or None> | slide / modal / target / new | 0 / 1 / 2 / 3 / 9 / None | <positive width for size 9 or None> | <temp variable or None> | <JSON token array or None> | Yes / No | <Reason> | export-proven / runtime-proof-required |

Rules:

- Follow `docs/standards/collection-02.md#form-action-open-resource-golden-reference-standard`; do not substitute direct control action settings for Form Action steps.
- Selected Item Edit/View requires exact Item ID expression tokens. Current item mode is available only on Data List or Document Library custom forms.
- `openform/new` may use target Approval variable defaults and query parameters. `openform/submitted` requires Form ID tokens and forbids both.
- Optional custom item forms must belong to the selected target Data List or Document Library and match the requested operation.
- Dashboard-host expressions may reference only declared temp variables.
- Slide/Pop-up use modal sizes 0/1/2/3/9. Size 9 requires Custom Width. Full page/New window carry no modal size.
- Public Forms cannot plan or materialize these steps.

##### Form Action Print Page and Barcode Scan Planning

Required whenever an Approval Submission/Task form, custom Data List/Document Library form, or Dashboard uses Print page or Barcode scan. One row represents one step. This focused baseline proves Dashboard print targets and Dashboard barcode scanning.

| Host Resource | Host Form / Page | Host Type | Action Name | Step Order | Step Name | Trigger | Bound Control | Exact Step Type | Target Page Type | Target Page | Print Title Expression Tokens | Paper Size | Print Layout | Scale Percent | Margins | Scanning Mode | Barcode Type | Result Temp Variable | Error Temp Variable | On Read Error | Barcode Filter Field | Business Rationale | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Inventory | Inventory | Dashboard | Print inventory | 1 | Print inventory records | Button Click | btn_print_inventory | print | Dashboard | Print inventory | `[{"type":"str","value":"Inventory List_"},{"type":"op","op":"&"},{"type":"func","func":"dateFormat","params":[[{"type":"func","func":"now","params":[]}],[{"type":"str","value":"DDMMYY"}]]}]` | A4 | landscape | 80 | Minimum | N/A | N/A | N/A | N/A | N/A | N/A | Print all inventory records | export-proven |
| Barcode Scan | Barcode Scan | Dashboard | Scan items | 1 | Scan inventory barcodes | Container Click | scan_trigger | barcode | N/A | N/A | N/A | N/A | N/A | N/A | N/A | multiple | auto | var_SelectedItems | var_ScanErrorMsg | Stop | Text10 | Filter Inventory Collection to scanned serial numbers | export-proven |

Rules:

- `print` currently supports an export-proven Dashboard target only. The target Dashboard must use `dashboard-print-multi-record-table-v1`, with a Collection, `table-v2` layout, at least one row/column merge example, and current-Collection-item QR binding.
- A4 serializes as `settings.Size = "6"`; Scale Percent serializes as a decimal (`80` -> `"0.8"`); Minimum margins serialize as `settings.Margins = "3"`.
- `barcode` modes are `multiple`, `select`, and `auto`. Export-proven barcode types in this round are Auto (omitted `attrs.type`) and `ean-13`.
- Result and error targets must be declared page temp variables. The result variable must be consumed by a Collection filter using operator `9` (contains/in scanned values); orphan scan output is a generated-final failure.
- This export proves Stop action on read error. Continue-to-next-step remains focused-export-proof-required.
- Public Form Barcode scan remains governed separately by the Public Form action allowlist and remains serialization-unproven until a Public Form export proves its exact shape.

##### Form Action Query Data Planning Rules

- One table row represents one Form Action step. Repeat `Action Name` for multi-step actions and preserve row order.
- Data List Form rows must identify both `Host Resource` and `Host Form`; these values are part of Plan-vs-Actual validation.
- Query Data steps must select an exact mode from `docs/standards/collection-02.md#form-action-query-data-golden-reference-standard`.
- Decide the native capability before selecting a Query Data mode. A read-only multi-row region should normally bind Collection/Data Table directly to its Data List source. Do not add a Query Data temp collection merely to feed a dataset control.
- `single_to_variables` maps source fields to declared Approval workflow variables.
- `single_to_temp_variables` maps source fields to declared page temp variables and is not persisted.
- `single_to_list_fields` is Data List New/Edit only and maps source fields to current-record fields using the canonical current-field target encoding.
- `single_to_list_fields_and_temp_variables` is Data List New/Edit only and may populate persisted current fields plus page-session display values in one query step.
- `multiple_to_sublist` requires a declared list variable/listref row schema and explicit source-to-row-field mappings.
- `multiple_to_list_sublist` writes queried rows into the current Data List record's Sub list and must use `__list_`; its count target uses a declared temp variable.
- Prefer Collection or Data Table for read-only reverse-related display. Use Query Data into a current-record Sub list only when rows become an editable working copy, such as quote lines loaded by product type before quantity or description changes.
- Prefer New/Edit Item forms for `multiple_to_list_sublist`. A View Item host is export-proven by the focused sample but requires an explicit save/edit workflow and business rationale.
- Data List Form field-change actions bind through the source control's `control_event_rule`; page-load actions bind through `formAction.onLoad`.
- `multiple_count_only` uses no record-result target and no field mapping; it only declares a workflow/temp count target.
- Public Forms must not plan or materialize Query Data Form Action steps.
- Pagination is shared across Approval Form, Custom Data List Form, and Dashboard Query Data. Omitted `Page Size` means `100`; an explicit Page Size must be `1..1000`. Omitted `Page Number` means `1`.
- Serialize a non-default Page Number with export-proven `querydata_pageindex`; omit it for default Page Number 1.
- Dashboard Query Data may write only declared temp variables/temp collections. Dashboard counts also use temp variables; workflow/current-record targets are forbidden.
- Dashboard temp collections are JSON payloads, not Collection/Data Table data sources. Plan `JSONStringfy` for text output or an explicitly justified Custom Code renderer for custom tabular presentation.
- Every temp collection/JSON row must name its `Result Consumer / Use`. Generic region names are insufficient; identify `JSONStringfy`, a calculation/aggregate, or a specifically justified Custom Code renderer.
- Generation-ready result targets must be exact. Terms such as `temp collection / import buffer`, `Query Data or lookup-bound display`, `preferably`, or `equivalent` are planning blockers unless the alternative is explicitly deferred with one selected implementation.
- Lookup-backed filters and chained ID mappings must state that the stored target record `ListDataID`/target identifier is used. A display title or ambiguous `lookup value` is not an ID contract.
- `validate-generation-readiness-review.mjs` automatically runs `validate-form-action-query-data-plan.mjs` when Form Action Query Data intent is detected. Resource-order validation alone is not planning readiness.
- Native Document Library custom forms use the same Query Data planning and target rules as Custom Data List forms. Form Report and Data Report cannot be independent Form Action hosts.
- Focused exports prove Approval Submission, Custom Data List Form, Document Library custom form, Approval Task Form, and Dashboard hosts. Query Data sources are generation-safe for Data List, Document Library, and Form Report. Form Report is a source but not a Form Action host; Data Report remains focused-proof-required.
- Form Action `querydata_sorts[]` supports at most two sort fields, in listed priority order.

#### Workflow Query Data Planning

Required for every `QueryData` node in an Approval Form workflow, Data List workflow, or Scheduled Workflow. This table is separate from Form Action Query Data planning.

| Workflow | Workflow Host Type | Node Name | Workflow Query Mode | Source Resource Type | Source Resource | Filters | Sorts | Result Variable | Result Variable Type | ListRef / Complex Type | Field Mapping | Count Variable | Page Size | Page Number | Downstream Consumer / Use | Branch Coverage | Proof Boundary | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Workflow> | Approval / Data List / Scheduled Workflow | <Business action name> | single_to_variables / multiple_to_list_variable / multiple_to_text_variable / multiple_count_only | Data List / Document Library / Form Report | <Resource> | <Field/operator/value; Lookup filters explicitly use ListDataID; or None> | <Field asc/desc; maximum 2; or None> | <Workflow variable or None> | Workflow variables / List workflow variable / Text workflow variable / None | <ListRef/Complex Type name or None> | <Source field -> workflow variable/ListRef field; or None> | <Number workflow variable or None> | 100 default / 1..1000 | 1 default / positive integer | Branch / Assignment assignee / Loop through list items / Invoke Custom Service / Send Email / calculation | <Complete complementary branches or N/A> | export-proven / runtime-proof-required | <Business rationale> |

Workflow Query Data planning rules:

- Use `docs/standards/collection-03.md#workflow-query-data-golden-reference-standard`; do not serialize a Form Action `querydata` step as a Workflow `QueryData` node.
- Workflow Query Data writes to declared normal workflow variables. Page temp variables are not available in Approval, Data List, or Scheduled workflow execution.
- `multiple_count_only` has no row result variable, Complex Type, or field mapping. It requires a number workflow variable and complete branch coverage such as `count > 0` plus `count <= 0`.
- `single_to_variables` requires explicit source-field to workflow-variable mappings. Query-derived User assignees must map to a declared User workflow variable before Assignment Task use.
- `multiple_to_list_variable` requires a List workflow variable, a linked ListRef/Complex Type definition, and source-field to ListRef-field mappings. A downstream Loop through list items must use this mode.
- `multiple_to_text_variable` is reserved for a proven JSON/text consumer. Do not use it as a substitute for a List variable when the workflow iterates rows.
- Lookup-backed filters must compare source `ListDataID` with the stored Lookup target record identity, never display text.
- Page Size is `1..1000`; omitted means `100`. Page Number is a positive integer; omitted means `1`.
- Export-proven source types are Data List, Document Library, and Form Report. Data Report remains `focused-learning-required` until its dedicated training round.
- A Query Data action supports at most two sort fields. Plan both explicitly when deterministic secondary ordering matters.
- Approval, Data List, and Scheduled workflows share the QueryData result contract. Host-specific current-record expressions still require host-specific export proof.
- `validate-generation-readiness-review.mjs` automatically runs `validate-workflow-query-data-plan.mjs` whenever Workflow Query Data intent is detected.
- Data List Workflow current-record filters must identify the exact source list field and target field. A related-record query commonly compares the target Lookup field to current-record `ListDataID`.

#### Workflow Loop Planning

Required for every Workflow `Loop` node.

| Workflow | Workflow Host Type | Loop Node Name | Loop Mode | Loop Source Parent | Loop Source | Loop Value Expression JSON | LoopBody Actions | Current Iteration / Current Item Use | Delay or Repeated Side Effects | Proof Boundary | Business Rationale |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Workflow> | Approval / Data List / Scheduled Workflow | <Business action name> | list / values / number | __variables_ / __list_ / blank for expression | <List variable or Sub List field> | <required for values/number; expression token array> | <Ordered actions inside LoopBody> | <LoopIndex / LoopItem fields / None> | <Email, mutation, Delay, or None> | export-proven / runtime-proof-required | <Why iteration is required> |

Workflow Loop planning rules:

- `list + __variables_` requires a declared List workflow variable linked to a ListRef/Complex Type.
- `list + __list_` is Data List Workflow-only and requires a real Sub List field on the host list.
- `values` requires a non-empty expression-token array that returns multiple values; V1.7 proves a multiple User field.
- `number` requires an expression-token array that resolves to a positive loop count; a fixed number uses a numeric token.
- For `values` and `number`, `Loop Value Expression JSON` is required and is copied exactly into `Loop.properties.loopValue.value`; do not serialize a business label or derive an expression from prose.
- Every mode requires a resolving `bodyRef` and explicit ordered `LoopBody Actions`.
- List current iteration is `LoopIndex`; current row fields use `LoopItem.<field>`. Do not invent field IDs.
- Repeated email, mutation, external calls, and Delay execution are runtime-sensitive even when their serialization is export-proven.
- `validate-generation-readiness-review.mjs` automatically runs the Workflow Loop planning gate whenever Loop intent is detected.

#### Sub List List Actions

Required whenever a Sub List control appears on a submission page or task page.

| Host Form | Sub List Field/Control | Action Name | Exact Yeeflow List Action Type | Support Source | Proof Label | Fallback / Deferred Reason | Current Row Context | Steps | Summary Fields Affected | Parent Field Binding | Runtime Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Form> | <Sub List> | No custom Sub List actions required / <Action> | add item / duplicate item / delete item / import items / move up/down / insert before/after / supported custom action | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Current row context> | <Steps> | <Summary fields> | <Binding> | Runtime action execution is separate proof |

Rules:

- State exactly which form hosts each form action.
- State where the action is triggered.
- Temp variables are frontend-only.
- Query data and Set data list actions must identify target application/list/fields.
- Data List Workflow Set Variable rows must target declared workflow variables only. A current-list field may appear only in the RHS expression. Plan current-record field mutation as Set Data List / `ContentList` with `Current list`, exact field mappings, and business values.
- If a form includes a Sub List, explicitly state whether custom List actions are required.
- If custom Sub List actions are required, list each action and its steps.
- Supported list action types must come from plugin-known/export-proven Sub List action shapes, such as add item, duplicate item, delete item, import items, move up/down, insert before/after, or supported custom action patterns.
- `Exact Yeeflow List Action Type` must use a plugin-known/export-proven action type or be marked `export-learning-required`, `runtime-proof-required`, or `deferred`.
- Each Sub List action must state current row context.
- If Summary fields are used, identify which Summary fields are affected.
- If Summary is bound to parent form fields, state the binding.
- Runtime execution proof for Sub List actions is separate from package validation.
- If no custom List actions are required, explicitly write: `No custom Sub List actions required`.

## 6. Form Reports Plan

Normally, each Approval form should have a corresponding Form report.

| Form Report Name | Related Approval Form | Purpose | Includes Sublist | Fields Included | Extra Field Configuration | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Report> | <Approval form> | <Purpose> | Yes/No | <Fields> | <Config> | <Notes> |

Rules:

- If a report needs Sublist data, plan a report structure that supports Sublist display.
- If an Approval form does not need a Form report, state the reason.

## 7. Schedule Workflows Plan

| Schedule Workflow Name | Description | Schedule Configuration | Trigger Frequency | Nodes | Branches | Data Read/Write | Proof Level | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Workflow> | <Description> | <Supported config> | Daily/Weekly/Monthly/etc. | <Nodes> | <Branches> | <Data> | proven/export-proven/runtime-proof-required/deferred | <Notes> |

#### Workflow Task Form Layout Template Selection

Required when a Schedule workflow includes a generated task form page.

| Workflow | Host Resource | Task Form | Workflow Surface | Selected Workflow Task Form Layout Template | Business Sections Needed | Editable Task Inputs | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Schedule workflow> | <Schedule> | <Task form> | Schedule workflow task | approval_form_layout_task_v1_1 | <Sections> | <None or fields> | <Reason> | Generated-final validation |

Rules:

- Schedule configuration must use properties supported by the plugin's Schedule workflow documentation.
- Each node must include type, order, configuration, branch conditions, and data read/write behavior.
- If a Schedule workflow includes a task form, it must select `approval_form_layout_task_v1_1`. The generated task form must follow the same Approval Form Layouts v1.1 task template contract as Approval form task pages, including readonly-by-default fields unless assignee input is required.

## 8. AI Agents Plan

| AI Agent Name | Description | Purpose | Inputs | Outputs | Tools | Used By Forms/Workflows | Human Review | Proof Level |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Agent> | <Description> | <Purpose> | <Inputs> | <Outputs> | <Tools> | <Nodes/actions> | Required/Not required | <Proof> |

Rules:

- After identifying AI Agents, recheck Approval forms, Schedule workflows, Data List workflows, and Form actions for AI Assistant usage.
- Do not embed connection secrets.
- Runtime execution proof is separate from package generation.

## 9. Copilots Plan

| Copilot Name | Description | Purpose | Inputs | Outputs | Tools | App Resource Access | Human Review | Proof Level |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Copilot> | <Description> | <Purpose> | <Inputs> | <Outputs> | <Tools> | <Access> | Required/Not required | <Proof> |

## 10. Custom Data List Forms Plan

Return to the Data lists and Document libraries from Section 4 and plan their custom forms.

Every Type `1` Data List from Section 4 must plan custom forms for New Item, Edit Item, and View Item. Do not plan Yeeflow's built-in default layout for generated Data Lists. Public Forms are additive anonymous-submission surfaces and never replace internal New/Edit/View custom forms. Generated Type `1` Data Lists do not receive a system/support-list exemption from this requirement. Document Library custom forms remain subject to an export-proven Type `16` contract and are not implied by this Data List rule.

### 10.x <Data List or Document Library Name>

| Form Name | Form Type | Purpose | Used By | Layout Pattern | Actions Required | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Form> | New/Edit/View/Detail/Custom/Print | <Purpose> | <Users/pages> | <Layout> | Yes/No | <Notes> |

#### Data List Form Layout Template Selection

Required for every Section 4 Data List or Document Library. Each business list must have New/Edit and View custom form coverage.

| Data List or Library | Custom Form | Form Usage | Selected Data List Form Layout Template | Business Sections Needed | Related Data / Analytics Needed | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- |
| <List> | <Form> | New/Edit/View | <approved template id> | <Sections> | <None or related datasets> | <Reason> | <Proof> |

Rules:

- Keep the heading exactly `#### Data List Form Layout Template Selection`; this table is mandatory before API ID allocation.
- Every Custom Data List Form row, including New, Edit, View, Detail, Print, and other custom forms, must have one explicit page-layout selection and a supported page role. Generators must clone that selected template and retain its provenance marker; they must not infer a template from the name, create a generic Container page, or silently fall back to a default.
- The Selected Data List Form Layout Template must be one of `data_list_form_layout_new_edit_v1_1`, `data_list_form_layout_view_item_v1_1`, or `data_list_form_layout_workbench` from `docs/reference/data-list-form-layout-templates.json`.
- New Item and Edit Item custom forms must select `data_list_form_layout_new_edit_v1_1`. If New and Edit use separate forms, both must still select this template.
- Standard View Item custom forms must select `data_list_form_layout_view_item_v1_1`.
- Full-page Workbench View Item custom forms must select `data_list_form_layout_workbench` and explicitly state `Open in: Full page` in the row's business sections, selection reason, or proof boundary.
- Default New/Edit/View layouts are forbidden for generated business Data Lists and Document Libraries.
- Public Forms, hidden/support purposes, or a narrow generation request do not exempt a Type `1` Data List from New/Edit/View custom forms.
- New/Edit forms focus on the current list item and must not plan related Collection/Data Analytics/KPI regions.
- View Item forms may plan current-record display plus related business data, approved Collection templates, approved Data Analytics templates, and KPI regions inside the v1.1 allowed slots. Field-grid and Collection templates may use the `section_content_area` of `content_card_wrapper`, `content_card_60_wrapper`, or `content_card_40_wrapper` depending on the required section width. Workbench View Item forms may use `primary_working_area`, optional `right_side_panel`, and `chart_cards_section`; empty Workbench right panels or chart sections must be pruned.
- App Plan selection is a business/layout decision only. It must not include generated `ListID`, `LayoutID`, action type codes, JSON property paths, placeholder IDs, copied control JSON, or runtime payload fields.

#### Public Forms Plan

Optional and additive. Add rows only for Data Lists that require anonymous submission. Every host Data List must still include the New/Edit/View custom forms planned above.

| Host Data List | Public Form Name | Form Title | Description / Purpose | Included Fields | Public Form Page Layout Template | Public Form Fields Layout Template | Business Sections Needed | CTA Actions | Section Title / Operations | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Data List> | <Public Form> | <Visible title> | <Anonymous submission purpose> | <Field labels or field names> | public-form-page-layout-standard | public_form_fields_1col_v1_1 | 1 column survey fields | None | Business-mapped title; no Operations | Generated-final validation; anonymous runtime submit proof separate |

Every planned Public Form must explicitly select `public-form-page-layout-standard` in its own row. The generator must clone that page layout and retain the Public Form page-layout provenance marker; it must not use a default when the cell is blank or omitted.

##### Public Form Form Action Planning

Required whenever a Data List Public Form contains a Form Action. Public Forms use an anonymous, current-list-only action surface and must not reuse the broader Custom Data List Form capability set.

| Host Data List | Public Form | Action Name | Step Order | Step Name | Trigger | Bound Control | Exact Step Type | Step Configuration JSON | Business Rationale | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Data List> | <Public Form> | <Action> | 1 | <Step> | Page Load / Button Click / Field Change / None | <Control or None> | Redirect page to / Show confirm dialog / Submit form / Start another action | <Exact JSON object> | <Reason> | export-proven / runtime-proof-required |

Rules:

- Follow `docs/standards/collection-02.md#public-form-form-action-golden-reference-standard`.
- Public Form Set variables must use the shared Form Action Set Variable Planning table with `Host Type = Public Form`.
- Only Set variables, Execute custom code, Show confirm dialog, Redirect page to, Submit form, Start another action, Barcode scan, and NFC reader are product-supported.
- Query data, Set data list, Open item form, Open approval form, Open dashboard, Invoke custom service, and all other application-resource steps are forbidden.
- Execute custom code, Barcode scan, and NFC reader remain generated-final blockers until exact decoded Public Form step exports are available.
- Redirect configuration uses `{ "fixedUrl": "https://...", "openType": false }` or `{ "expressionTokens": [...] }`. Tokens may reference only fixed values, current-list fields, or same-form temp variables.
- Confirm configuration uses `{ "messageTokens": [...], "resultTempVariable": "var_Result" }`; Start another action uses `{ "targetActionName": "Other action" }`; Submit uses `{}`.

Rules:

- Public Forms are stored under the host Type `1` Data List `PublicForms[]`; they are not custom Data List forms and must not replace `List.LayoutView.add/edit/view`.
- The host Data List must complete normal field, view, custom New/Edit/View form, workflow, navigation, permission, package, and preflight generation before the Public Form is added.
- Use `public-form-page-layout-standard` and an approved Public Form field-layout template. Survey/questionnaire forms should prefer `public_form_fields_1col_v1_1`.
- Included Fields must resolve to fields on the host Data List and must use Public Form-compatible anonymous controls. User, Department, Metadata, Tag, Multi Meta, Location, Cost center, and Lookup fields are forbidden.
- Public Form generation does not create a reduced Public Form-only Data List package. Full application and standalone Data List generation must remain on the shared builder and validator path.

#### Reverse-Related Collection Selection

Required when a parent/detail lookup relationship should be displayed from the parent record's View Item or Workbench View Item form. This table is the plan-to-package contract that tells generation to add a reverse-related Collection section rather than only defining the lookup field.

| Host Data List | View Item Form | Related Child List | Child Lookup Field | Section Title | Collection Template | Search | Add Record | Default Value |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Parent list> | <Parent View Item form> | <Child list> | <Child lookup FieldName> | <Visible section title> | collection_control_responsive | <Search fields or No> | <Add button label or No> | `<Child lookup FieldName> = current ListDataID` |

Rules:

- Generate reverse-related sections only for View Item or Workbench View Item forms, not New/Edit forms.
- Use this table when a child list has a lookup to the host list and users need to inspect or create those child records from the host record page.
- The Collection Template must be an approved responsive Collection template for list-like child records. Use `collection_control_responsive_multiple_select` only when the section needs a concrete bulk-selection action.
- The generated Collection must source the child list and filter the child lookup field to the current host record's `ListDataID`.
- If Search is enabled, the search fields must resolve on the child list and the `search-filter` variable must be consumed by the Collection `fulltext` binding.
- If Add Record is enabled, the Add action must target the child list and pass the current host `ListDataID` into the child lookup field through default `passvalues`.
- Run `scripts/validate-data-list-form-layout-template.mjs --package <generated-final.yapk> --plan <yeeflow-app-plan.md>` before signing. A row in this table must fail generated-final validation if the matching reverse-related section is not materialized in the generated package.

#### Form Fields

#### Form Fields Layout Template Selection

Required for every generated custom Data List New/Edit/View form that displays current-record fields.

| Data List or Library | Custom Form | Field Group | Selected Form Fields Layout Template | PC/Laptop Columns | Tablet Columns | Mobile Columns | Full-Row Field Controls | Dynamic Display Grouping | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <List> | <Form> | <Field group> | data_list_form_fields_grid_v1_1 | 2 or 3 | <= PC/laptop | 1 | Multiple line / Rich text / Sub list fields | <None or grouping rule> | Generated-final validation |

Rules:

- Keep the heading exactly `#### Form Fields Layout Template Selection`; this table is mandatory before API ID allocation.
- Select `data_list_form_fields_grid_v1_1` from `docs/reference/data-list-form-field-layout-templates.json` for every generated field group on a New/Edit/View custom Data List form.
- Current-record field controls must be placed inside the selected `form_grid_fields_wrapper`, not directly inside `section_content_area`; the field grid itself must be hosted in the `section_content_area` of `content_card_wrapper`, `content_card_60_wrapper`, or `content_card_40_wrapper`.
- Multiple line, Rich text, and Sub list controls must be listed as full-row field controls and generated with column span equal to the parent Grid's column count for each responsive breakpoint.
- PC/laptop columns should be 2 or 3, tablet columns must not exceed PC/laptop columns, and mobile columns should be 1.
- Sub list field controls must use the control-level `data_list_form_control_sublist_v1_1` template inside `form_grid_fields_wrapper`; the App Plan names the Sub list business field and nested fields, while generation maps concrete `attrs.list-variables` and `attrs.list-fields`.
- Generated field controls must receive business-specific `nv_label` / `nav_label` values derived from their field or group purpose.
- If a custom form intentionally has no current-record field controls, state that explicitly with the business reason and proof boundary.
- This table is a planning contract only. Do not include generated `ListID`, `LayoutID`, `FieldID`, JSON property paths, copied control JSON, or runtime payload fields.

| Form Name | Field Order | Business Label | Field Name | Field ID | Exact Yeeflow Field Type | Exact Yeeflow Control Type | Support Source | Proof Label | Fallback / Deferred Reason | Binding | Read Only | Required | Default Value | Placeholder | Dynamic Display | Custom Validation | Lookup Target | Lookup Display Field | Additional Lookup Fields | Sublist/Summary Notes | Description |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Form> | 1 | <Business label> | <Field> | <ID> | <Exact field type> | <Exact control type> | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Binding> | Yes/No | Yes/No | <Default> | <Placeholder text> | <Rule> | <Rule> | <List> | <Field> | <Fields> | <Notes> | <Description> |

#### Custom Form Actions and Temp Variables

| Action Name | Host Form | Trigger Location | Trigger Type | Temp Variables | Steps | Data Read | Data Write | Bound Controls | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Action> | <Form> | Page Load / Field Change / Submission / Button | <Type> | <Variables> | <Steps> | <Data> | <Data> | <Controls> | <Notes> |

#### Sub List List Actions

Required whenever a Sub List control appears on a New/Edit/View/Detail/Custom/Print form.

| Host Form | Sub List Field/Control | Action Name | Exact Yeeflow List Action Type | Support Source | Proof Label | Fallback / Deferred Reason | Current Row Context | Steps | Summary Fields Affected | Parent Field Binding | Runtime Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Form> | <Sub List> | No custom Sub List actions required / <Action> | add item / duplicate item / delete item / import items / move up/down / insert before/after / supported custom action | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Current row context> | <Steps> | <Summary fields> | <Binding> | Runtime action execution is separate proof |

Rules:

- Every list that users create or edit records in should have a runtime-safe Add/New form plan.
- Every generated New/Edit/View custom Data List form must select the correct Data List Form Layouts v1.1 template before generation.
- Every generated field group on a New/Edit/View custom Data List form must select `data_list_form_fields_grid_v1_1` before generation.
- Query data and Set data list actions must identify target application/list/fields.
- Data List custom form root padding must follow the active plugin standard.
- Distinguish business labels from exact Yeeflow implementation types. Slash-combined or vague implementation wording is not generation-ready unless marked `runtime-proof-required`, `export-learning-required`, or `deferred`.
- If a custom Data List form includes a Sub List, explicitly state whether custom List actions are required.
- If custom Sub List actions are required, list each action and its steps, current row context, affected Summary fields, and parent field bindings.
- Supported list action types must come from plugin-known/export-proven Sub List action shapes, such as add item, duplicate item, delete item, import items, move up/down, insert before/after, or supported custom action patterns.
- Runtime execution proof for Sub List actions is separate from package validation.
- If no custom List actions are required, explicitly write: `No custom Sub List actions required`.

## 11. Data List Workflows Plan

Return to the Data lists and Document libraries from Section 4 and plan list workflows.

| Workflow Name | Host List/Library | Trigger Condition | Nodes in Order | Branches | Branch Conditions | Data Read/Write | Notifications/AI/External Calls | Proof Level | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Workflow> | <List> | <Trigger> | <Nodes> | <Branches> | <Conditions> | <Data> | <Calls> | <Proof> | <Notes> |

#### Workflow Task Form Layout Template Selection

Required when a Data list workflow includes a generated task form page.

| Workflow | Host Resource | Task Form | Workflow Surface | Selected Workflow Task Form Layout Template | Business Sections Needed | Editable Task Inputs | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Data list workflow> | <List or Library> | <Task form> | Data list workflow task | approval_form_layout_task_v1_1 | <Sections> | <None or fields> | <Reason> | Generated-final validation |

Rules:

- Node types must come from active plugin workflow-node documentation or export-proven references.
- Every Data List workflow `ContentList` node must also appear in the Workflow Set Data List Action Plan with explicit target, mapping, and filter JSON.
- Runtime execution proof is separate from package validation.
- If a Data list workflow includes a task form, it must select `approval_form_layout_task_v1_1`. The generated task form must follow the same Approval Form Layouts v1.1 task template contract as Approval form task pages, including readonly-by-default fields unless assignee input is required.

## 12. Notifications Plan

Return to the Data lists and Document libraries from Section 4 and plan notifications.

| Notification Name | Host Resource | Trigger Condition | Recipients | Message Content | Data Fields Used | Proof Level | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| <Notification> | <List/Form/Workflow> | <Trigger> | <Users/roles/fields> | <Content> | <Fields> | config-proven/delivery-proof-required/deferred | <Notes> |

Rules:

- Notification configuration proof is separate from delivery proof.
- Do not use real users/emails unless explicitly approved.

## 13. Data List Views Plan

Return to the Data lists and Document libraries from Section 4 and plan views.

| View Order | List/Library | View Name | URL / Route Key | Default | Access Scope | Filter Conditions | Sort/Grouping | Display Fields | Query/Search Fields | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <List> | <View> | <url> | Yes/No | All users / role-specific | <Filters> | <Sort> | <Fields> | <Fields> | <Notes> |

Rules:

- Each generated list should have meaningful default display columns.
- Query/search fields must use fields that exist in the list.
- `Filter Conditions` are fixed `LayoutView.filter[]` constraints and must be planned separately from interactive `Query/Search Fields` in `LayoutView.query[]`.
- The fixed-filter column may be labeled `Filter Conditions` or `Filters`, but its contents must still be executable field-level conditions.
- If a view's business purpose narrows the dataset, write concrete field-level filters such as `Datetime1 is not empty`, `Text3 is not empty AND Text4 is not empty`, or `Decimal1 is not empty OR Text7 is not empty`; do not leave the fixed filter blank unless the view intentionally shows all records.
- Do not use vague fixed-filter phrases such as `All active meetings`, `lifecycle tracking`, or `active items` without naming the actual field and comparison. Convert them to concrete filters like `Meeting Status = Active` or `Meeting Date is not empty`.
- Any view that references business columns, query/search fields, or fixed filters requires a parseable field table for that Data List. Do not rely on prose such as `Fields: ...`; without a standard field table, generation must fail instead of downgrading the list to a native `Title`-only view.
- When business wording says `Today`, use the export-proven `now` expression in generated Data View filters; do not generate an unsupported `Today` function/token.
- View order must be explicit.

## 14. Dashboard Pages Plan

Repeat for each Dashboard page.

### 14.x <Dashboard Page Name>

- Page name:
- Business purpose:
- Source Functional Specification dashboard requirement reference:
- Source data lists/business objects:
- Navigation placement:
- Page Function Plan reference if applicable:
- Depends on:
- Temp variables required:
- Page/form actions required:
- Runtime proof required:
- Functional Specification dashboard source: business questions, source business objects/data lists, summary metrics, metric source fields, calculation logic, data regions, display fields, filters, sorting/grouping, user actions, mobile support, and alerts from Stage 1.

Dashboard page identity:

| Dashboard Page Name | Business Purpose | Functional Spec Dashboard Requirement Reference | Source Data Lists/Business Objects | Navigation Placement | Page Function Plan Reference |
| --- | --- | --- | --- | --- | --- |
| <Dashboard> | <Purpose> | <Functional Spec section/requirement> | <Lists/objects> | <Navigation group/item> | <Page Function Plan path or not applicable> |

#### Dashboard Page Layout Template Selection

Required for every generated Dashboard page.

| Dashboard Page | Selected Dashboard Page Layout Template | Business Layout Need | Primary Regions Needed | Right Side Panel Needed | Chart Cards Section Needed | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- |
| <Dashboard> | dashboard-page-layouts-v1.1 / dashboard-page-layouts-custom-code / dashboard-page-layouts-workbench / dashboard-page-layouts-two-panel-workspace / dashboard-page-layouts-three-panel-workspace / dashboard-print-multi-record-table-v1 | General overview / operational workbench / master-detail workspace / multi-record print page | <Sections/regions> | Yes/No | Yes/No | <Reason using template guidance> | Generated-final validation |

Rules:

- Select exactly one Dashboard page layout template per Dashboard page from `docs/reference/dashboard-page-layout-templates.json`.
- This selection is mandatory even for the standard `dashboard-page-layouts-v1.1` template. The generated Dashboard must be cloned from the selected template and retain its dashboard page-layout marker; no missing row may fall back to the default.
- Use `dashboard-page-layouts-custom-code` for an explicit primarily Custom Code Dashboard request, or consider it for an image-backed complex business page. Include **Custom Code Dashboard Composition**, **Custom Code Module Plan**, and **Custom Code Communication and Native Integration** using `docs/standards/collection-01.md#dashboard-page-layouts-custom-code-standard`. Plan optional header/regions, functional modules, temp-variable exchange, native filter inputs, and CSS-ID native Button/Form Action triggers. No default dataset or header is mandatory for this template.
- Use `dashboard-page-layouts-v1.1` for general overview dashboards, report-style dashboards, and section-first pages.
- Use `dashboard-page-layouts-workbench` for operational workbench pages that need a primary working area, optional right-side panel, top filters, KPI cards, grouped analytics, and queue/list regions.
- Use `dashboard-print-multi-record-table-v1` only for a Dashboard selected as a Print page target. It provides one-record-per-Collection-item Table layout, merged row/column support, and a current-Collection-item QR region.
- Use `dashboard-page-layouts-two-panel-workspace` when a Dashboard must manage one source dataset with a left record list and a right selected-record detail panel.
- Use `dashboard-page-layouts-three-panel-workspace` when the selected-record detail needs a main detail panel plus an additional right-side information/action panel.
- If `dashboard-page-layouts-workbench` is selected, state whether `right_side_panel` and `chart_cards_section` are needed. Empty Workbench right-side panels and empty chart sections must be removed during generation.
- If a master-detail workspace template is selected, state the source dataset, selected item title/description fields, left-panel filters/search, current-item operations, detail field groups, related-record sections, analytics sections, and whether the empty-selection state should keep or replace the template image/title/description.
- Master-detail workspace pages must preserve the `vCurrentItemID` temp variable. The left-panel Collection item click action must write the clicked item ID into `vCurrentItemID`, and the current-item detail Collection must limit records to `1` and filter record ID by `vCurrentItemID`.
- The App Plan may state selected template and business regions, but must not include copied control JSON, generated IDs, or low-level style/property payloads.

#### Dashboard Sections

| Section Order | Section Name | Business Purpose | Source Data List or Business Object | Required Fields or Metrics | Selected Yeeflow Control Type Category | Why This Control Type Is Appropriate | User Actions Needed | Proof Boundary or Deferred Note |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <Section> | <Purpose> | <List/object> | <Fields/metrics> | Summary / KPI card, Data Filter, Collection, Data table, Kanban, Vertical Timeline, Horizontal Timeline, Text / Heading, Button / action button, Container, Grid / flex grid, Chart / Data analytics if supported/proof-required | <Reason> | <Actions> | <Proof/deferred note> |

Rules:

- Each dashboard section must trace to a business-level Dashboard Page Requirement from the Functional Specification. Do not invent dashboard metrics, filters, regions, or actions that are not in the Functional Specification unless they are explicitly added as an App Plan assumption or clarification gate.
- Dashboard sections must map business requirements to legal Yeeflow control type categories. Allowed categories include Summary / KPI card, Data Filter, Collection, Data table, Kanban, Vertical Timeline, Horizontal Timeline, Text / Heading, Button / action button, Container, Grid / flex grid where needed for structured display, and Chart / Data analytics only if plugin-supported or marked `runtime-proof-required`, `export-learning-required`, or `deferred`.
- Unsupported or invented dashboard control type categories are not generation-ready unless marked `runtime-proof-required`, `export-learning-required`, or `deferred` with reason, fallback, and proof/generation impact.
- Summary/KPI cards must be data-bound using plugin-supported controls and bindings. Static text may be used only when explicitly marked fallback, `runtime-proof-required`, `export-learning-required`, or `deferred` with reason, fallback, and proof/generation impact.
- Collection, Kanban, Timeline, Data Filter, Data table, Summary/KPI, chart/data analytics, action button, Text/Heading, Container, and Grid/flex grid planning must include source data, fields/metrics, actions, and proof boundary at business/control-type level.
- Do not include concrete generated IDs, ListID/PageID/FormID/LayoutID/ProcKey values, actionTypeCode values, Yeeflow JSON property paths, exact Container nesting, exact style values, runtime binding payloads, implementation-level layout JSON, or fake placeholder IDs such as `LIST-*` or `LAYOUT-*`. Those belong to Blueprint/resource generation and validation.

#### Dashboard Filters

| Filter Name | Source Data List | Filter Field | Applies-to Dashboard Sections | Selected Yeeflow Filter/Control Type If Known | Default Business Scope | Proof Boundary or Deferred Note |
| --- | --- | --- | --- | --- | --- | --- |
| <Filter> | <List> | <Field> | <Sections> | Data Filter / Text search / choice/date filter / runtime-proof-required / export-learning-required / deferred | <Default scope> | <Proof/deferred note> |

#### Page Level Data Filter Group Template Selection

| Page/Form Surface | Page/Form Name | Filter Count | Selected Data Filter Group Template | Included Filters | Placement Region | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Dashboard / Custom Data List form / Approval submission / Approval task / Data list workflow task / Schedule workflow task | <Name> | <Count> | dashboard_standard_filter_group when count is two or more | <Filters> | <Approved section_content_area> | <Reason> | Generated-final validation plus runtime proof if linkage is claimed |

Rules:

- If a page or form contains two or more page level Data Filter controls, select `dashboard_standard_filter_group`.
- Do not use this table for local Collection toolbar search boxes.
- Do not include generated `ListID`, `ListSetID`, runtime field storage IDs, or copied control JSON.

#### Summary Metrics

| Metric Name | Source Data List | Source Field(s) | Calculation Logic | Selected Yeeflow Control Type Category | Display Format Intent | Proof Boundary or Deferred Note |
| --- | --- | --- | --- | --- | --- | --- |
| <Metric> | <List> | <Fields> | <Business calculation> | Summary / KPI card | count / percentage / currency / duration / number | <Proof/deferred note> |

#### Data Analytics Template Selection

Required for every Dashboard or Data List form section that uses a chart, pivot table, or Data Analytics control.

| Section | Surface | Data Source | Business Question | Selected Data Analytics Template | Grouping/Axis Fields | Value/Aggregate Fields | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Section> | Dashboard / Data List form | <Data List/Form Report/Data Report/Document Library metadata list> | <Question> | <approved Data Analytics template ID> | <Business fields only, no runtime IDs> | <Business fields/aggregate logic> | <Reason using template guidance> | <Local/runtime proof boundary> |

Rules:

- The Selected Data Analytics Template must be one of `data_analytics_pie_chart_with_title`, `data_analytics_column_chart_with_title`, `data_analytics_bar_chart_with_title`, `data_analytics_line_chart_with_title`, `data_analytics_area_chart_with_title`, or `data_analytics_pivot_table_standard` from `docs/reference/data-analytics-golden-references.json`.
- For each chart/pivot region, state the business question, source data, grouping/axis fields, value/aggregate fields, and selection reason. The rationale must use the selected template's `whenToUse`, `whenNotToUse`, `requiredBusinessSignals`, and `suitableSourceResourceTypes` guidance.
- Select exactly one approved Data Analytics template per analytics region. Do not list multiple possible templates for the same region and do not leave the template choice to generation.
- App Plan selection is a business decision only. It must not include generated `ListID`, `LayoutID`, `PageID`, action type codes, JSON property paths, placeholder IDs, or runtime payload fields in this table.
- Data Analytics templates are allowed only on Dashboard pages and Data List forms. They must not be planned for Approval forms.
- On Dashboard Page Layouts v1.1 pages, generated resources may place Data Analytics templates inside `content_card_wrapper`, `2_columns_section`, `3_columns_section`, or `2_columns_60/40_section`.
- On Workbench Dashboard pages and Workbench Data List View Item forms, chart-like analytics should use `chart_cards_section` under `primary_working_area` or `right_side_panel`; one `chart_cards_section` should contain no more than three chart-like Data Analytics templates.
- Pivot table analytics are table-like regions. Plan `data_analytics_pivot_table_standard` inside `content_card_wrapper > section_content_area`, preferably under `1_columns_section`; use `2_columns_section`, `2_columns_60/40_section`, or `3_columns_section` only for narrow/few-column pivots. Do not plan Pivot tables inside `chart_cards_section`.
- The App Plan may state the selected placement family, but must not prescribe low-level container properties.

#### Data Table Template Selection

Required for every Dashboard, Custom Data List form, Approval form page, workflow task form, or Approval print page section that uses a native Data table control.

| Host Surface | Page/Form | Region | Source Resource | Business Purpose | Selected Data Table Template | Column Width Mode | Caption/Search/Actions | Display Columns | Selection Reason | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Dashboard / Custom Data List form / Approval submission / Approval task / Approval print / Data List workflow task / Schedule workflow task> | <Name> | <Section> | <Data List / Document Library metadata list / Form Report / Data Report> | <Why a native Data table is needed> | <approved Data table template ID> | Default scroll / Auto no-scroll | <None or caption title/search/add/import/export> | <Business field labels only, no runtime IDs> | <Reason using template guidance> | <Local/runtime proof boundary> |

Rules:

- The Selected Data Table Template must be one of `data_table_control_standard_scroll`, `data_table_control_standard_no_scroll`, or `data_table_control_caption_scroll` from `docs/reference/data-table-golden-references.json`.
- Select exactly one approved Data table template per Data table region. Do not list multiple possible templates for the same region and do not leave the template choice to generation.
- Use `data_table_control_standard_scroll` when many columns need readable widths and horizontal scrolling is acceptable.
- Use `data_table_control_standard_no_scroll` when the table has a small number of columns and should fit inside the current control without horizontal scrolling.
- Use `data_table_control_caption_scroll` when the Data table needs caption title, built-in search, add item, and import/export more-menu behavior.
- The App Plan selection is a business decision only. It must not include generated `ListID`, `ListSetID`, field storage IDs, JSON property paths, copied control payloads, or placeholder IDs.
- Use Data table for lightweight tabular display. Use approved Collection/Kanban/Timeline templates instead when the requirement needs rich cards, multi-select state, complex row operations, kanban boards, or timeline presentation.

#### Dashboard Actions

| Action Name | Business Purpose | Source/Target Business Object | Expected User Outcome | Supported Yeeflow Action Category When Known | Proof Boundary or Deferred Note |
| --- | --- | --- | --- | --- | --- |
| <Action> | <Purpose> | <Object> | <Outcome> | open dashboard/form, open detail, add data list item, run form action, runtime-proof-required, export-learning-required, deferred | <Proof/deferred note> |

Rules:

- Dashboard action planning may name supported action categories, but must not include concrete `actionTypeCode`, ListID, PageID, FormID, LayoutID, ProcKey, runtime payload fields, or implementation JSON.

#### Record Display Control Selection

Required for every Dashboard/Page section that displays Data List records.

| Section | Data Source | Display Need | Selected Record Display Control | Selected Collection Presentation Reference | Required Business Fields | Selection Reason | Detail/Open Behavior | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Section> | <Data List/Form Report/Document Library metadata list> | <Cards/table/status board/activity history/roadmap/etc.> | Data table / Collection / Kanban / Vertical timeline / Horizontal timeline | <approved Dashboard Collection reference ID or not applicable> | <Business fields only, no runtime IDs> | <Reason> | <Open/edit/detail behavior> | <Local/runtime proof boundary> |
| Dense audit table | <Data List or Form Report> | Dense operational audit-table review | Collection | collection_control_responsive | <Audit fields> | Native desktop/tablet table with mobile cards for dense operational review | <Open behavior or No row open> | <Local/runtime proof boundary> |

Rules:

- Allowed selected controls are Data table, Collection, Kanban, Vertical timeline, and Horizontal timeline.
- Prefer Collection over Data table when both can satisfy the requirement, unless a dense native table/grid is specifically required.
- When the selected record display control is Collection, the Selected Collection Presentation Reference must be one of `collection_control_responsive_card_grid`, `collection_control_responsive`, `collection_control_responsive_multiple_select`, `collection_control_card_with_multiselect_toolbar`, or `Event Pipeline Grid-Table` from `docs/reference/dashboard-dataset-presentation-golden-references.json`. Search/fulltext is planned as behavior inside the selected approved template, not as a separate Collection template ID.
- For Collection rows, state the required business fields and selection rationale. The rationale must use the selected template's `whenToUse`, `whenNotToUse`, `requiredBusinessSignals`, and `suitableSourceResourceTypes` guidance, such as card browsing, dense row/column scanning, free-text search, multiselect/bulk operation, or high-fidelity primary operations table.
- The App Plan generator must write this rationale at generation time. It must not emit a row that only names `collection_control_responsive`, `collection_control_responsive_card_grid`, or another approved template ID without a matching business signal such as dense row/column scanning, card browsing, multiselect/bulk operation, work queue, record list, or primary operations table.
- Select exactly one approved Collection presentation reference per Dashboard dataset region. Do not list multiple possible templates for the same region and do not leave the template choice to generation.
- The exact approved reference must appear in this concrete Dashboard region/control row. Mentioning it only in a separate Dashboard Golden Reference Selection or summary table does not satisfy the generation contract.
- App Plan selection is a business decision only. It must not include generated `ListID`, `LayoutID`, `PageID`, action type codes, JSON property paths, placeholder IDs, or runtime payload fields in this App Plan table.
- Use Kanban for status, lane, queue, or work-board patterns.
- Use Vertical Timeline for activity, history, audit, event feed, and chronological log patterns.
- Use Horizontal Timeline for roadmap, phase, lifecycle, and milestone progression patterns.
- Use Data table only when a true tabular data grid is required.
- The reason for the selected control must be stated.

#### Item Template Dynamic Controls

Required for every Collection, Kanban, Vertical Timeline, or Horizontal Timeline control.

| Host Control | Source List | Business Label | Source Field | Expected Dynamic Display Control Category | Display Purpose | Proof Boundary or Deferred Note |
| --- | --- | --- | --- | --- | --- | --- |
| <Collection/Kanban/Timeline> | <List> | <Business label> | <Field> | dynamic field / dynamic user / dynamic image / dynamic file / dynamic date / supported display category | <Purpose> | <Proof/deferred note> |

Rules:

- Dynamic display needs stay at business/control-type category level in the App Plan. Low-level control properties and runtime binding payloads belong to Page Function Plan, Blueprint, resource generation, and validation.
- Allowed Dynamic display categories must come from plugin-known/export-proven controls, such as dynamic field, dynamic user, dynamic image, dynamic file, dynamic date, or a supported display category if documented.
- Status or badge display is allowed only when backed by supported Yeeflow controls/categories or marked for proof/deferment.
- Collection, Kanban, Vertical Timeline, and Horizontal Timeline controls must not have empty item templates.
- Every visible item-template value must map to a Dynamic control and a real source field.
- User/person displays must bind to User/person fields.
- Status, priority, and condition displays must bind to supported Choice fields or supported display fields.
- Unsupported Dynamic control types or property paths must be marked `export-learning-required`, `runtime-proof-required`, or `deferred`.

#### Collection and Kanban Item Actions

Required for Collection and Kanban controls.

| Host Control | Action Name | Business Purpose | Source/Target Business Object | Expected User Outcome | Supported Yeeflow Action Category When Known | Proof Boundary or Deferred Note |
| --- | --- | --- | --- | --- | --- | --- |
| <Collection/Kanban> | No Collection/Kanban item actions required / <Action> | <Purpose> | <Object> | <Outcome> | open detail, add data list item, open dashboard/form, run form action, runtime-proof-required, export-learning-required, deferred | Runtime action execution is separate proof |

Rules:

- Collection/Kanban actions must use plugin-known/export-proven action categories or be marked `runtime-proof-required`, `export-learning-required`, or `deferred`.
- Vague action wording such as `Open detail/slide panel where supported` or `Update row/status where supported` is not generation-ready unless marked `runtime-proof-required`, `export-learning-required`, or `deferred`.
- If an action writes data, state the source/target business object and expected user outcome at business level. Concrete runtime payload fields belong later.
- Runtime execution proof is separate from package validation.
- If no custom actions are required, explicitly write: `No Collection/Kanban item actions required`.

## 15. Application Navigation Plan

Application layout template: `application-layout-sidebar-workspace-1`.

#### Application Color Pattern Selection

| Color Role | Base Color | Light Model | Source / Rationale |
| --- | --- | --- | --- |
| Primary | <business-appropriate #RRGGBB> | Luminance | Business-domain primary rationale, such as travel approval, procurement, asset operations, finance, or workforce management |
| Secondary | <business-appropriate #RRGGBB> | Luminance | Business-domain secondary/accent rationale that complements the primary color |
| Neutral | <low-chroma #RRGGBB> | Luminance | Neutral UI-state rationale for forms, borders, read-only fields, and subtle backgrounds |

Rules:

- Every App Plan must choose the application color pattern used by the Type `0` `application style` theme in `Themes[]`.
- Only Primary, Secondary, and Neutral base colors are selected here. Success, warning, and danger remain Yeeflow semantic colors and must stay green, yellow, and red.
- `Light Model` must be exactly `Luminance` for Primary, Secondary, and Neutral.
- Primary and Secondary base colors must not be too light or too dark. Prefer OKLCH lightness `0.42-0.68`; values outside `0.35-0.82` are not generation-ready.
- Neutral must remain low-chroma. It must use OKLCH lightness `0.65-0.88` and chroma no greater than `0.06`.
- If the user provides approved brand colors, use those colors when they pass readability ranges.
- If the user does not provide brand colors, choose Primary, Secondary, and Neutral from the business function and explain the rationale. Do not keep the generic Yeeflow defaults only because the user was silent.
- The generic fallback palette `#0065FF` / `#00D1FF` / `#B3B7C0` may be used only when the business domain cannot be inferred or when the user explicitly requests/approves the Yeeflow default palette.
- Recommended examples: Business Travel / approval / reimbursement apps can use Primary `#1E40AF`, Secondary `#0F766E`, Neutral `#94A3B8`; procurement/vendor apps can use Primary `#0F766E`, Secondary `#1D4ED8`, Neutral `#94A3B8`; asset/operations apps can use Primary `#1D4ED8`, Secondary `#0F766E`, Neutral `#94A3B8`.
- Generated packages must write these selected base colors into the Type `0` `application style.Config` stringified JSON and preserve the Soft outline controls default control style binding.

| Navigation Order | Group | Item | Yeeflow Resource Type | Target Resource | Visible | Icon | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <Group> | <Item> | Data list / Dashboard / Approval form / Report | <Resource> | Yes/No | <Icon> | <Notes> |

Runtime navigation rules:

- New generated apps use the `application-layout-sidebar-workspace-1` golden reference by default unless the user explicitly selects another approved Yeeflow application layout.
- The package must materialize the template through `ListSet.LayoutView.sortVer` and `ListSet.LayoutView.attrs`, including the 46px header, `h6-semi-bold` title typography, left navigation menu, and reference header/navigation colors.
- Every visible business navigation group and menu item must include a suitable FontAwesome icon. Do not leave the `Icon` cell blank for visible menu entries.
- Groups use `Type: "classes"` with `list`.
- Dashboards/pages use `Type: 103`.
- Approval forms use `Type: 105` with `ListID = Forms[].Key`.
- Data lists use `Type: 1`.
- Do not use runtime group `children` or `Childs`.
- Every intended resource must be reachable through navigation or explicitly hidden/deferred.

## 16. Target Users, Roles, Groups, and Permissions

| Role | User Group | Resource/Page/Form | View | Create | Edit | Delete/Archive | Approve | Admin | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Role> | <Group> | <Resource> | Yes/No | Yes/No | Yes/No | Yes/No | Yes/No | Yes/No | <Notes> |

Rules:

- Use placeholders, empty groups, requester/current-user expressions, or post-import setup unless real IDs are approved.
- Do not generate real users, emails, group members, departments, or job positions unless explicitly authorized and schema support is proven.

## 17. Plugin Capability and Standards Compliance

| Capability Area | Plugin Knowledge Source | Supported Shapes Used | Unsupported/Unproven Items | Validator/Check | Notes |
| --- | --- | --- | --- | --- | --- |
| Data list fields | <Skill/doc/reference> | <Types> | <Items> | <Validator> | <Notes> |
| Approval variables/forms | <Skill/doc/reference> | <Types> | <Items> | <Validator> | <Notes> |
| Workflow nodes | <Skill/doc/reference> | <Nodes> | <Items> | <Validator> | <Notes> |
| Dashboard controls | <Skill/doc/template> | <Controls> | <Items> | <Validator> | <Notes> |

Rules:

- The App Plan must be based on plugin-supported capabilities.
- All App Plan resource types, field types, variable types, controls, Dynamic controls, workflow nodes, form actions, Collection/Kanban actions, Sub List actions, property paths, bindings, and configuration shapes must come from the active plugin's known skills, standards, validators, template library, control/property knowledge base, extension registry, or export-proven references.
- Business-friendly labels must be separate from exact implementation types. Use split columns such as `Business Label`, `Exact Yeeflow Field Type`, `Exact Yeeflow Variable Type`, `Exact Yeeflow Control Type`, `Exact Yeeflow Action Type`, `Support Source`, `Proof Label`, and `Fallback / Deferred Reason` where needed.
- Slash-combined or vague implementation wording such as `Title/Text`, `Currency/Number`, `User/person`, `Attachment/File upload`, `Document library / Data list`, `Type 1/document library`, `Lookup/read-only dynamic field`, `Document/list section`, `Open detail/slide panel where supported`, or `Update row/status where supported` is not generation-ready unless marked `runtime-proof-required`, `export-learning-required`, or `deferred`.
- If a requirement depends on an unknown capability, mark it as export-learning-required, runtime-proof-required, or deferred.
- Do not plan invented control shapes, Dynamic control types, field types, variable types, workflow nodes, action schemas, property paths, bindings, or configuration shapes.
- Unknown shapes must be marked `export-learning-required`, `runtime-proof-required`, or `deferred` and must not be treated as generation-ready.

### Custom Code and Custom Service Planning

Use this section whenever the application may require client-side Custom Code, Form Action Custom Code, or backend Custom Service. If none are required, include one row marked `Not planned` and do not materialize any custom-code or custom-service resource, action step, workflow action, navigation entry, variable, or placeholder artifact from that row.

#### Custom Capability Decision Matrix

| Requirement / Use Case | Native Yeeflow Capability Considered | Why Native Capability Is Enough or Insufficient | Selected Capability | Host Surface | Planning Status | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- |
| <Business use case> | <Controls/expressions/form action/workflow action/reporting> | <Reason> | None / Custom Code control / Form Action Custom Code step / Custom Service | <Dashboard / approval form / data list form / workflow / scheduled workflow / AI/Copilot future proof> | Planned / Not planned / Deferred / export-learning-required / runtime-proof-required | <Proof required> |

Decision rules:

- Prefer native Yeeflow controls, expressions, Summary/Data Analytics, form actions, workflow actions, and data-list operations when they satisfy the requirement.
- Use a Custom Code control only for client-side UI behavior or immediate client-side calculation that cannot be built with native controls or expressions.
- Use a Form Action Custom Code step only for client-side action logic that must run inside a form action and can be proven with `execute(context, fieldsValues)`.
- Use Custom Service only for backend execution, heavier processing, file processing, server-side Yeeflow SDK operations, or integration with third-party systems through configured connection variables.
- Treat Custom Service invocation as server-side queued execution. Do not use it when the business requirement needs immediate client-side feedback unless the plan includes a user-facing waiting/progress pattern or a Custom Code fallback.
- Do not add Custom Code or Custom Service during materialization unless it is planned in the tables below or explicitly marked as a user-approved runtime-proof-required item.

#### Custom Code Plan

| Custom Code Name | Surface | Host Page/Form/Action | Business Purpose | Inputs / Required Fields | Writable Outputs | Native Fallback Considered | Security / Privacy Notes | Runtime Proof Required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Name> | Custom Code control / Form Action Custom Code step | <Host> | <Purpose> | <Fields/variables/temp variables/parameters> | <Fields/variables/temp variables or None> | <Fallback> | <Notes> | <Proof> |

Custom Code rules:

- Custom Code control scripts must use `render(context, fieldsValues, readonly)` and be hosted as page/form controls.
- Form Action Custom Code step scripts must use `execute(context, fieldsValues)` and be hosted inside `formdef.actions[].steps[]`.
- Do not generate `render(...)`-only code for a Form Action Custom Code step.
- Do not generate `execute(...)`-only code for a Custom Code control.
- Approval form outputs use form/workflow variables. Data list custom form outputs may use data-list fields or temp variables when export-proven. Dashboard outputs use temp variables.

#### Custom Service Plan

| Custom Service Name | Business Purpose | Connection Variables | Input Variables | Output Variables | Backend / Integration Work | Native Fallback Considered | Security / Privacy Notes | Runtime Proof Required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Name> | <Purpose> | <Connection IDs and types, or None> | <Text/Number/Boolean/Date-Time/File/Image/Rich text variables> | <Text/Number/Boolean/Date-Time/File/Image/Rich text variables> | <Yeeflow SDK / external API / file processing> | <Fallback> | <Notes> | <Proof> |

Custom Service rules:

- Custom Service code must use `main({ connections, params, modules })`.
- `DraftConfig` must define `params`, `connections`, and `outputs`; output keys returned by `main` must match configured output IDs.
- Use connection variables for HTTP API/OAuth 2.0 integrations. Do not hardcode credentials, tokens, tenant-specific secrets, or private/internal URLs.
- Use `modules.yeeSDKClient` for Yeeflow backend operations when the SDK shape is proven. Use `modules.fetch(..., { connection })` or the export-proven connection shape for third-party calls.
- Preserve 19-digit Yeeflow IDs as strings.

#### Custom Service Invocation Plan

| Invocation Name | Host Surface | Host Page/Form/Workflow | Service | Input Binding Source | Output Binding Target | Queue / Waiting UX | Follow-up Action | Proof Boundary |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Name> | Approval form action / Data list form action / Dashboard form action / Approval workflow / Data list workflow / Scheduled workflow / AI Agent or Copilot future proof | <Host> | <Service> | <Variables/list fields/temp variables/static values> | <Variables/temp variables/workflow variables/list update via follow-up> | <Expected wait behavior> | <Set Data List / refresh / notification / none> | <Proof> |

Invocation binding rules:

- Form action invocation uses `type: "invokeservice"`.
- Workflow invocation uses `stencil.id = "InvokeCode"`.
- Approval form action input/output bindings use `__variables_`.
- Custom data list form action inputs may use `__list_` for data-list fields or `__temp_` for temp variables; outputs use `__temp_` unless a focused export proves another target.
- Dashboard form action inputs and outputs use `__temp_`.
- Data list workflow invocation may bind current item fields with `exprType: "list_field"` and should write returned external IDs to workflow variables before a downstream Set Data List / `ContentList` action updates the current item.
- Scheduled workflow and task-form invocation must follow the same export-proven workflow/task-form rules or be marked `runtime-proof-required`.

#### Custom Capability Proof and Runtime Validation Plan

| Capability | Local Validation | Package Validation | Runtime Proof | Failure Handling / Fallback |
| --- | --- | --- | --- | --- |
| <Custom Code / Custom Service / invocation> | <Script lint/shape checks> | <Generated package placement/binding checks> | <User interaction/workflow run/service response proof> | <Fallback> |

Proof rules:

- Package schema/signing/install success does not prove Custom Code execution or Custom Service execution.
- Runtime proof must show parameter binding, output writing, error handling, and any downstream refresh/update behavior.
- Unknown AI Agent/Copilot invocation of Custom Service remains future-proof-required until a focused export/runtime proof exists.

## 18. Generation Contract and Hard Gates

### Output Package

- Default output: `.yapk`
- `.yap` only if explicitly requested
- Package generation expected in this task: Yes/No
- Planning-only task: Yes/No

### Required Gates

| Gate | Required | Pass Criteria | Blocks Generation/Signing/Install |
| --- | --- | --- | --- |
| Functional spec approved | Yes | Approved, answered, or user-default-approved-for-generation | Generation |
| App Plan approved | Yes | Approved by user | Generation |
| Business Clarification Gate | Yes | Planning mode may pass `default-applied-for-planning`; generation mode requires answered, not applicable, deferred with reason/fallback/proof impact, or `user-default-approved-for-generation`; `validate-business-clarification-gate.mjs --mode generation` passes before generation | Generation |
| Functional Spec to App Plan traceability | Yes | `validate-functional-spec-to-app-plan-traceability.mjs` passes | Generation |
| Generation Readiness Review | Yes | `validate-generation-readiness-review.mjs` passes | Design/generation |
| Plugin capability compliance | Yes | No invented unsupported shapes; any unsupported resource/control/action/property shape is marked `export-learning-required`, `runtime-proof-required`, or `deferred` | Generation |
| Schema validation | Yes | Package schema passes | Signing |
| API-issued ID provenance | For generated-final `.yapk` | All generated IDs API-issued and reported | Signing/install |
| Upgrade ID stability | For upgrades | Existing semantic IDs preserved | Upgrade |
| Approval form gate | If approval required | Forms, task pages, workflow, DefResource present | Signing/install |
| Navigation runtime gate | Yes | Runtime navigation metadata valid | Signing/install |
| Dashboard grid-table Collection gate | If planned | Header + Collection wrapper and detail links valid | Signing/install |
| Root padding gate | For dashboards/custom forms | Token-array root padding valid | Signing/install |
| Plan-to-package conformance | Yes | Planned resources implemented or explicitly deferred | Handoff |
| Advanced capability gate | If planned | AI Agents, Copilots, custom code, custom CSS, integrations, notifications, schedule workflows, and external execution are plugin-supported, export-proven, runtime-proof-required, or deferred | Generation/handoff |

- Business decision gates answered/user-default-approved-for-generation or no blockers: Yes/No

### Advanced Capability Gate

- AI Agents, Copilots, custom code, custom CSS, integrations, notifications, schedule workflows, external calls, advanced controls, Dynamic controls, Collection/Kanban actions, Sub List actions, property paths, bindings, and configuration shapes must cite plugin-known skills, standards, validators, template library entries, control/property knowledge base entries, extension registry entries, or export-proven references before generation.
- Unknown or partially proven advanced capabilities must be marked `export-learning-required`, `runtime-proof-required`, or `deferred`.
- Do not invent unsupported control, resource, action, workflow-node, schedule, AI, Copilot, custom-code, notification, integration, property, binding, or configuration shapes.
- Runtime execution proof for AI, notifications, schedule workflows, external integrations, custom code, and permission enforcement is separate from package generation, signing, install/import/upgrade acceptance, and browser render proof.

## 19. Validation Plan

| Validation Layer | Scope | Tool/Method | Expected Result | Proof Boundary |
| --- | --- | --- | --- | --- |
| App-plan conformance | Resources vs plan | <Validator/manual> | pass/fail | Local proof |
| Business clarification | Decision gates | `validate-business-clarification-gate.mjs` | pass/fail | Planning readiness only |
| Generation readiness | 13 resource areas and review gates | `validate-generation-readiness-review.mjs` | pass/fail | Planning readiness only |
| Functional Spec to App Plan traceability | Requirement categories vs planned resources | `validate-functional-spec-to-app-plan-traceability.mjs` | pass/fail | Planning traceability only |
| Package schema | `.yapk`/`.yap` schema | <Validator> | pass/fail | Local proof |
| Control/property fidelity | Controls and bindings | <Validator> | pass/fail | Local proof |
| Runtime UI | Browser load/screenshots | <Runtime test> | pass/fail | Runtime proof |
| Workflow execution | Workflow test | <Runtime/manual> | pass/fail | Execution proof |

Include:

- Data-list/schema validation:
- Approval-form validation:
- Form report validation:
- Schedule workflow validation:
- AI Agent/Copilot validation:
- Custom data list form validation:
- Data list workflow validation:
- Notification validation:
- View validation:
- Dashboard/control validation:
- Navigation validation:
- Permission validation:
- Signing validation:
- Upgrade-check/upgrade-apply validation:
- Runtime verification:

Report wording requirement:

- Functional Specification structure: pass/fail
- App Plan resource order: pass/fail
- Functional Spec to App Plan traceability: pass/fail
- Generation Readiness structural check: pass/fail
- Business Clarification Gate for planning: pass/fail
- Business Clarification Gate for generation: pass/fail
- Overall generation readiness: pass only if all required planning gates pass for generation

If Generation Readiness structural check passes but Business Clarification Gate for generation fails, write exactly: `Overall generation readiness: blocked by Business Clarification Gate`. Do not write only `Validation passed` or `all passed` when planning defaults are only applied for planning.

## 20. Proof Boundary

Separate these proof levels:

- Plan approval:
- Local generation validation:
- Package schema validation:
- ID provenance proof:
- Navigation runtime metadata proof:
- Dashboard/control proof:
- Signing proof:
- API install/import/upgrade acceptance:
- Runtime materialization/render proof:
- Workflow execution proof:
- Scheduled workflow execution proof:
- Notification delivery proof:
- AI Agent/Copilot execution proof:
- External integration execution proof:
- Permission enforcement proof:

Boundary rule:

- Signing/install/upgrade acceptance does not prove runtime UI fidelity, workflow execution, notification delivery, AI execution, permission enforcement, or external integration behavior.
- Business Clarification Gate, Generation Readiness Review, and Functional Spec to App Plan traceability prove planning readiness only. They must pass before full-page canonical design images, page implementation blueprints, resource generation, decoded resource-vs-blueprint parity, signing, install/import/upgrade, or runtime proof, but they do not prove generated package validity or runtime behavior.

## 21. Assumptions

- Business assumptions:
- Data assumptions:
- Role/user assumptions:
- UI/control assumptions:
- Workflow assumptions:
- Package/output assumptions:

## 22. Deferred or Runtime-Proof Items

| Item | Category | Reason | User Impact | Fallback | Required Proof or Follow-up |
| --- | --- | --- | --- | --- | --- |
| <Item> | deferred/runtime-proof-required/post-import-config | <Reason> | <Impact> | <Fallback> | <Proof/follow-up> |

Include:

- Anything not generated.
- Anything generated but not runtime-proven.
- Anything requiring browser/manual verification.
- Anything requiring tenant-specific users/groups/positions/configuration.
- Anything requiring custom code, custom CSS, scheduled execution, notification delivery, AI execution, or external integration proof.

## 23. Recommended Next Prompt

Use this prompt after the plan is approved:

`Use @yeeflow-app-builder to generate <Application Name> from <app plan path>. Treat the approved Functional Specification and App Plan as the implementation contract. Generate the complete planned application unless an item is explicitly deferred. Use .yapk by default unless .yap is explicitly requested. Follow the resource generation order in the App Plan. Use only plugin-supported Yeeflow resource types, field types, variable types, controls, Dynamic controls, workflow nodes, form actions, Collection/Kanban actions, Sub List actions, property paths, bindings, and configuration shapes. Validate schema, ID provenance, navigation runtime metadata, approval forms, form reports, schedule workflows, AI/Copilot resources, custom data list forms, Sub List actions, data-list workflows, notifications, views, dashboard control selection, item-template Dynamic controls, Collection/Kanban actions, grid-table Collection patterns, root padding, graph, UI quality, app-plan conformance, package wrapper/signing readiness, and source/dist consistency. Do not run live Yeeflow API calls, write operations, install/import/upgrade, or runtime tests unless explicitly authorized. Report local validation, signing status, API acceptance, runtime proof, deferred items, and known risks separately.`

## Collection content density

Follow `docs/standards/collection-02.md#native-collection-density-standard`. For native Table/Card Collections, plan field-aware widths after actual container placement, using native `cw/cwu`; limit narrative Dynamic fields through `t-len` only with a complete-value route. Collection selection rows accept optional `Column widths` (JSON map), `Text lengths` (JSON map) and `Full value access` (`source-list`, `record-detail`, `none`). The real materializer consumes these fields and rejects invalid overrides. Keep 10-record defaults, choice styles, scoped toolbar/menu behavior, sorting, source bindings and mobile card content intact. Pixel/percentage examples are mapping evidence, not universal layout defaults.

---

<a id="approval-workflow-designer-editability-standard"></a>

<!-- source: docs/standards/collection-01.md#approval-workflow-designer-editability-standard -->

# Approval Workflow Designer Editability Standard

Status: generated-final hard contract for standalone `.ywf` and Approval workflows embedded in `.yapk`.

## Scope

An importable workflow is not necessarily editable or publishable. Generated Approval workflows must preserve the current Workflow Designer node hitboxes, workflow action outputs, and data-mutation semantics in addition to graph connectivity and visual layout.

Standalone `.ywf` generation and full-application `.yapk` materialization must call the same workflow Designer-shape utilities. Do not hand-build a second graph shape for standalone output.

## Designer Node Shape

Every non-`SequenceFlow` node must include:

- matching non-empty `id` and `resourceid`;
- numeric `position.x` and `position.y`;
- `bounds.upperLeft` and `bounds.lowerRight` with positive width and height;
- `bounds.upperLeft` equal to `position`;
- a root `graphposition` whose `x/y` follow the runtime-proven content-boundary inset contract.

For generated workflows, use `graphposition.x = minNodeX + 90` and `graphposition.y = minNodeY + 45`. `graphposition.width/height` equal the unscaled node-content span, subject only to the established `470 x 185` minimum for very small workflows. Do not infer saved `graphposition` from the browser DOM stage translation: fresh-tab runtime proof shows those are different coordinate concepts. Both legacy `minNode - margin` values and the obsolete `safeInset - minNode * graphzoom` formula are signing blockers.

Default export-observed sizes used by the shared builder are:

- event nodes: `36 x 36`;
- gateway nodes: `46 x 46`;
- task/action nodes: `190 x 86`.

Missing bounds are a Designer hit-testing defect: the canvas may render, while nodes cannot be selected or configured and adjacent nodes cannot be added reliably.

## SequenceFlow Reference Shape

Every graph reference must use the canonical export shape:

```json
{ "id": "<shape-id>", "resourceid": "<shape-id>" }
```

For each `SequenceFlow`:

- `source` and `target` must each contain matching `id` and `resourceid`;
- `incoming[0]` must be the same canonical node reference as `source`;
- `outgoing[0]` must be the same canonical node reference as `target`;
- the source node must include the flow in its canonical `outgoing[]`;
- the target node must include the flow in its canonical `incoming[]`;
- camel-case `resourceId` is not a substitute for a missing `id` or lowercase `resourceid`.

The shared generator must pass all graph shapes through
`normalizeApprovalWorkflowGraphReferences()` before DefResource encoding. A graph that can be
reconstructed indirectly from node `outgoing[]` is still broken when the SequenceFlow source
endpoint itself is not canonical; Designer may render only some nodes, omit connectors, and make
the canvas non-editable.

The standalone `build-ywf-wrapper.js` entrypoint must run the same normalizer before Base64
encoding. Standalone wrapper generation is not allowed to preserve malformed input references.

## Query Data Outputs

A `QueryData` node that writes results to workflow variables must declare those variables in `DefResource.variables.basic` before publish:

- `result.listName` must resolve when `result.listParent = "__variables_"`;
- the variable type must equal `result.vartype`;
- multiple results must select at least one `result.fields[]` entry;
- `result.totalCount` must resolve to a `number` variable;
- workflow-variable counts use `result.querycount_prefix = "__variables_"`.

The shared materializer must create these variables. A validator-only fix is insufficient.

## Content List Actions

Content List operations must express a real mutation:

- `add` and `edit` require at least one `listdatas[]` field mapping;
- every mapping requires `Columns`, `Per: "0"`, and non-empty `Data`;
- `edit` and `remove` require at least one `wheres[]` target-record condition;
- an action named Update/Edit must not use `type: "add"`;
- unresolved edit/remove target criteria are generation blockers, not permission to guess a record.

## Assignment And Wrapper Shape

- Assignee Expression Buttons must use the shared assignee serializer; `${{...}}` and plain nested JSON are forbidden.
- Approval tasks normally omit `tasktype`; Complete tasks use `tasktype: "complete"`.
- Standalone `.ywf` wrappers use `Settings: null` through the standard wrapper builder.
- Start/end nodes preserve export-shaped email defaults when email is disabled.

## Required Gates

Before signing, install, import, or publish claims, run:

```bash
node validate-ywf-def.js <decoded-def.json> --mode final
node scripts/validate-approval-workflow-publish-readiness.mjs --resource <decoded-def.json>
node scripts/test-approval-workflow-publish-readiness-gates.mjs
```

Generated-final blockers include:

- `APPROVAL_WORKFLOW_NODE_BOUNDS_MISSING`
- `APPROVAL_WORKFLOW_NODE_BOUNDS_POSITION_MISMATCH`
- `APPROVAL_WORKFLOW_GRAPHPOSITION_ORIGIN_MISMATCH`
- `APPROVAL_WORKFLOW_GRAPHPOSITION_CONTENT_SPAN_MISMATCH`
- `APPROVAL_WORKFLOW_GRAPHPOSITION_DIMENSIONS_INVALID`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_SOURCE_ID_MISSING`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_SOURCE_IDENTITY_MISMATCH`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_TARGET_ID_MISSING`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_TARGET_IDENTITY_MISMATCH`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_INCOMING_REF_INVALID`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_OUTGOING_REF_INVALID`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_INCOMING_ENDPOINT_MISMATCH`
- `APPROVAL_WORKFLOW_SEQUENCEFLOW_OUTGOING_ENDPOINT_MISMATCH`
- `QUERYDATA_RESULT_VARIABLE_NOT_FOUND`
- `QUERYDATA_COUNT_VARIABLE_NOT_FOUND`
- `QUERYDATA_RESULT_FIELDS_EMPTY`
- `CONTENTLIST_EMPTY_LISTDATAS`
- `CONTENTLIST_EMPTY_WHERES`
- `CONTENTLIST_OPERATION_SEMANTICS_MISMATCH`
- workflow assignee expression serialization errors

Passing local validators proves structural Designer readiness only. Designer open/edit/publish remains a distinct runtime proof layer.

## Persisted Designer Readback

Generated input remains strict by default. When validating an actual persisted Designer response, use `node validate-ywf-def.js <decoded-def.json> --mode final --input-origin designer-readback`. Only the canvas content-span difference becomes a warning in this mode; positive dimensions, origin, node geometry and connector references remain blocking checks. Record a separate Designer visual inspection before claiming runtime readiness. Never use this option to waive generated-input failures.

LoopBody entry flows reside inside that body's `children`. Its outgoing entry reference must resolve there, originate at the same body's start port, and be reciprocated by its child target. An unrelated container's flow cannot satisfy this reference.

Approval field planning preserves explicit `Required` through host field projection and writes the Boolean to `attrs.required`. Submission UI validation and workflow/API validation are distinct: a required Sub List can block the browser Submit button, while interface-started workflows need an explicit server-side guard before human tasks or data writes. A server-side amount calculation must run before approval if it is the authoritative amount; trusting a supplied Total does not prove amount consistency.

---

<a id="approval-workflow-final-layout-closure-standard"></a>

<!-- source: docs/standards/collection-01.md#approval-workflow-final-layout-closure-standard -->

# Approval Workflow Final Layout Closure Standard

## Purpose

Prevent Approval Forms from being persisted with a workflow that is structurally accepted but unreadable, unroutable, or not editable in Workflow Designer.

## Required generation sequence

1. Finish the semantic topology before layout: classify each path as approved completion, final rejection, return/rework, branch, or post-approval system action.
2. Materialize every final node and SequenceFlow.
3. Layout the complete node set using the workflow golden-reference lanes. Do not append a node to an already laid-out graph.
4. Route only the final SequenceFlows. Local forward, merge, and local rejected links use rounded auto-routing; long, cross-row, backward, and return links use safe explicit `vertices[]`. Apply `docs/standards/collection-01.md#approval-workflow-route-orthogonality-standard` so local lower-right Rework branches remain vertex-free and explicit routes are orthogonal.
5. Recalculate `graphposition` from the final node bounds.
6. Validate the decoded definition before save, then retrieve, decode, and validate the persisted definition after save.

Any graph edit after step 3 restarts at step 3. API acceptance alone is never an exception.

## Layout policy

- Generated sequential flows may contain at most five execution nodes on one main backbone row. Fold later steps into a lower readable row.
- A row transition is a planned backward/return route. It must travel through an open adjacent row gap or a justified external lane; it must not cross node bounds.
- System actions belong in their local action lane. They must not be appended indefinitely to the right of a completed approval chain.
- The normal End is a local completion point beside the final source group, not a fixed far-right endpoint.
- Rejected is terminal. Returned/Rework is non-terminal and must route to a planned clarification/rework node or earlier task; it must not target a historical shared rejection endpoint.

## End with Rejection contract

An `EndRejectEvent` may receive empty-vertex rounded lines only from nearby Approval Tasks on the same local lane. A target is non-local when any of these is true:

- its x position is left of the source;
- its source/target distance exceeds the local reject x or y allowance;
- source and target are on different workflow lanes.

A non-local rejected or return line requires explicit safe `vertices[]`. If a safe route is not available, create a local endpoint or remodel the topology. Never use a long line merely because the target is an `EndRejectEvent`.

## Hard validation gates

Run both commands against the exact generated-final package/definition:

```bash
node scripts/validate-approval-workflow-publish-readiness.mjs --package <generated-final.yapk> --plan <yeeflow-app-plan.md>
node scripts/validate-workflow-layout-golden-reference.mjs --package <generated-final.yapk>
```

Treat at least these findings as blocking:

- `WORKFLOW_LAYOUT_REJECT_ENDPOINT_VERTICES_MISSING`
- `WORKFLOW_LAYOUT_BACKWARD_FLOW_VERTICES_MISSING`
- `WORKFLOW_LAYOUT_ROUTE_Y_CROSSES_INTERMEDIATE_ROW`
- `WORKFLOW_LAYOUT_ROUTE_X_CROSSES_INTERMEDIATE_COLUMN`
- `WORKFLOW_LAYOUT_END_REJECT_SOURCE_SPAN_TOO_WIDE`
- `WORKFLOW_LAYOUT_END_REJECT_SOURCE_LANES_MISMATCH`
- `WORKFLOW_LAYOUT_GRAPHPOSITION_ORIGIN_MISMATCH`
- `WORKFLOW_LAYOUT_LOCAL_FORWARD_VERTICES_UNNECESSARY`
- `WORKFLOW_LAYOUT_VERTEX_SEGMENT_DIAGONAL`
- `WORKFLOW_LAYOUT_ROUTE_SOURCE_EXIT_NOT_VERTICAL`
- `WORKFLOW_LAYOUT_FORWARD_NODE_CLEARANCE_TOO_SMALL`

## Evidence contract

| Evidence level | What it proves | What it does not prove |
| --- | --- | --- |
| `apiAccepted` | API accepted the payload. | Diagram layout, Designer behavior, workflow execution. |
| `persistedReadback` | Saved payload was retrieved and still passes static closure. | Designer rendering or runtime routing. |
| `designerOpen` | Workflow Designer opens and the full graph is readable/editable. | Request submission and assignee routing. |
| `browserWorkflowRuntime` | A disposable request traversed the tested Submit/Approve/Reject/Return path. | Untested paths, notifications, or production authorizations. |

For any graph with multiple approval tasks, a branch, a return/rework path, or system actions, `designerOpen` is mandatory before claiming workflow readiness. `browserWorkflowRuntime` is mandatory before claiming route execution.

---

<a id="approval-workflow-route-orthogonality-standard"></a>

<!-- source: docs/standards/collection-01.md#approval-workflow-route-orthogonality-standard -->

# Approval Workflow Route Orthogonality Standard

## Purpose

Prevent generated Approval workflow diagrams from persisting a valid sequence-flow topology whose visible route is diagonal, stale after layout movement, or compressed beyond usable Designer readability.

## Route classification

Classify geometry from final source and target positions, not from the display label alone.

- A local forward branch targets a nearby node to the right. It uses empty `vertices[]`, including an outcome labelled `Returned for Rework` when it reaches a lower-right rework action.
- A true return/backward route targets a prior/leftward node or must cross an occupied row or column. It may use explicit `vertices[]`.
- A local rejection to a nearby `EndRejectEvent` remains vertex-free; a non-local terminal route follows the existing safe-route rules.

## Explicit route contract

When vertices are required:

1. Generate them only after every final node position and bound is known.
2. Every pair of adjacent vertices must share x or y; diagonal vertex segments are forbidden.
3. If the first vertex exits above or below the source card, its x must match the source center within 12px.
4. Use an open row-gap midpoint for horizontal return segments and an open column-gap midpoint for long vertical segments.
5. Recompute the whole connector after any node move; never copy a previous layout's vertices.

## Spacing contract

For same-row forward SequenceFlows, use at least 110px card-to-card clearance. The Audit Management Golden Reference uses roughly 130–160px and should be preferred. If the workflow exceeds the intended canvas width, fold subsequent steps into another readable row; do not reduce clearance.

## Required gates

Run `scripts/validate-workflow-layout-golden-reference.mjs` before save and on persisted readback. Treat these as blockers:

- `WORKFLOW_LAYOUT_LOCAL_FORWARD_VERTICES_UNNECESSARY`
- `WORKFLOW_LAYOUT_VERTEX_SEGMENT_DIAGONAL`
- `WORKFLOW_LAYOUT_ROUTE_SOURCE_EXIT_NOT_VERTICAL`
- `WORKFLOW_LAYOUT_FORWARD_NODE_CLEARANCE_TOO_SMALL`

The validator proves serialized layout geometry. Workflow Designer-open is required to prove the visible routing result; browser workflow runtime is separate proof of execution.

---

<a id="choice-field-option-materialization-standard"></a>

<!-- source: docs/standards/collection-01.md#choice-field-option-materialization-standard -->

# Choice Field Option Materialization Standard

## Scope

This standard applies to generated Data List, Document Library, Approval Form, Public Form, and page-control choice fields. It covers single select, radio, checkbox, and tag option materialization.

## App Plan Contract

- `Choice Values` represents an ordered enum, not one display sentence.
- Each business option becomes one `Rules.choices[]` entry.
- Plan display delimiters may be comma, semicolon, Chinese comma, ideographic comma, or line break.
- Parenthetical planning notes such as `Planning Default` or `规划默认` are metadata and must be removed from runtime labels.
- A default value belongs in the plan's `Default Value` column. It must not be encoded by appending a planning note to an option.

## Generated Runtime Contract

- `Rules.choices` contains one object per option with a stable key, value, and color.
- `Rules.color_choices` contains the same values in the same order, including when `show_color` is false.
- Values are non-empty and unique after case-insensitive normalization.
- Explicit business options are not capped or silently truncated.
- Runtime option text cannot contain planning annotations.
- A single option entry cannot contain a delimiter-separated list of multiple business values.

Example:

```json
{
  "choices": [
    { "key": "1", "value": "Annual Leave", "color": "#2563eb" },
    { "key": "2", "value": "Sick Leave", "color": "#10b981" },
    { "key": "3", "value": "Personal Leave", "color": "#f59e0b" },
    { "key": "4", "value": "Other", "color": "#ef4444" }
  ],
  "color_choices": [
    { "key": "1", "value": "Annual Leave", "color": "#2563eb" },
    { "key": "2", "value": "Sick Leave", "color": "#10b981" },
    { "key": "3", "value": "Personal Leave", "color": "#f59e0b" },
    { "key": "4", "value": "Other", "color": "#ef4444" }
  ],
  "displayStyle": "dropdown",
  "show_color": false
}
```

## Hard Gates

- `CHOICE_OPTION_CONTAINS_MULTIPLE_VALUES`
- `CHOICE_OPTION_PLANNING_ANNOTATION_PRESENT`
- `CHOICE_OPTION_DUPLICATE`
- `CHOICE_COLOR_OPTIONS_MISSING`
- `CHOICE_COLOR_OPTIONS_MISMATCH`

The same parser and gates apply to full `.yapk` generation and standalone exports. Fix the App Plan or shared builder; do not patch only one generated package.

---

<a id="collection-toolbar-style-standard"></a>

<!-- source: docs/standards/collection-01.md#collection-toolbar-style-standard -->

# Collection toolbar and row-menu styling

## Proof and applicability

This opt-in fallback was verified on an existing Dashboard Collection in a desktop browser after save and exact persisted readback. It is not a fresh-package import, all-host, mobile or full-theme proof. Prefer export-backed native Style properties. Only use the fallback when that path is insufficient and the observed DOM matches the selectors below.

## Ownership and repair

Single-control exceptions belong to that control's `attrs.common.css`. Yeeflow expands `selector` to the control wrapper scope. Shared appearance may use a semantic CSS class; CSS IDs must be unique per page and are not shared style labels. Never style all `.ak-form-button-btn` descendants of a Collection card to change just its Add button: that also changes row Drop bars and menu buttons.

Use `scripts/lib/collection-toolbar-style.mjs` with explicit control objects and roles. `applyCollectionToolbarStyle(button, 'add')` makes its 40px minimum-height box inline-flex and centers content. `applyCollectionToolbarStyle(filter, 'select')` unifies the 40px single-select shell, zero vertical padding, and 38px rendered/placeholder/selected-value boxes. Keep native placeholder display/hiding untouched; forcing `display:block!important` exposes placeholder text over selected values. Remove the specific superseded ancestor rules only after reviewing their other consumers. The helper preserves unrelated CSS and bindings and is idempotent; it does not remove arbitrary legacy CSS automatically.

For a right-edge row menu, call `configureCollectionRowMenu(dropbar)`. `attrs.settings.autoposition` is a scalar boolean. Position is a responsive array; a value only at index 3 is mobile-only. Fill missing desktop and mobile placements with `bottomRight`, retain explicit existing placements and other settings. Check actual placement after opening: automatic positioning may flip to `topRight`.

On dark menus only, `applyDarkMenuButtonStyle(actionButton)` sets white foreground in normal/hover/active with a control-scoped priority override and icon/text inheritance. The observed page default `.pageCraft_1 .comp-wrapper .ak-form-button-btn` used primary blue even though the menu button's saved normal color was white. Do not assume a successful save or inherited parent foreground proves actual button color. Avoid globally overriding all page buttons or hardcoding runtime `ak-dynamic-*` classes. White-on-light menus require a different reviewed palette; do not use this helper indiscriminately.

`inspectCollectionToolbarScope` warns about broad Container button height/color selectors and traverses both children and Collection tablecols. It is an advisory review helper, not an automatic proof that all arbitrary CSS is safe. Use explicit opt-in repair/generation and do not restyle every existing Collection during unrelated app generation.

## Acceptance and remaining gaps

Run `node scripts/test-collection-toolbar-style.mjs` and the native-rich-content suite in source and distribution. Local tests cover scope, repeatability, preservation of action/filter metadata, both native trees, responsive placement and placeholder visibility rules. They are not browser visual tests.

Browser acceptance must verify Add text/icon alignment; each Select empty, selected and cleared state; filter intersection; right/bottom-edge menu containment; readable menu text/icon and hover/active states. Compare data outside the declared change scope before save and read back after save. Query updates are asynchronous: wait for the known expected rows to stabilize before measuring or opening their menus. An early row count may still be the previous result; a row replacement may dismiss an open menu.

Observed desktop results: Add content has equal top/bottom space; three selected filters have zero center displacement; their intersection yields the expected record and clearing restores placeholders; a 180px menu flips to topRight and remains in the viewport; normal-state Edit text and icon compute to white. Separate hover/active visual proof, mobile visual proof, exhaustive filter candidate coverage, Owner accessibility text and actual Edit-versus-View routing remain pending. This training does not publish or install a plugin release.

## Generated-artifact validation

Source Golden templates keep their exact exported mobile-only placement. Generated responsive Collections may additionally use desktop bottomRight with native autoposition enabled while preserving mobile bottomRight. The dataset presentation gate accepts this bounded generated variant; it still rejects mobile drift, unsupported placement and a disabled auto-position flag on this variant.

---

<a id="custom-capability-app-plan-planning-standard"></a>

<!-- source: docs/standards/collection-01.md#custom-capability-app-plan-planning-standard -->

# Custom Code and Custom Service App Plan Planning Standard

This standard defines when Yeeflow App Builder may include Custom Code or Custom Service in an application plan and generated package.

## Planning Requirement

Custom Code and Custom Service are advanced capabilities. They must be explicitly planned before generation.

The App Plan must include:

- a native-capability decision explaining why standard Yeeflow controls, expressions, form actions, workflow actions, Summary/Data Analytics, data-list operations, or CSS are enough or insufficient;
- the selected capability: none, Custom Code control, Form Action Custom Code step, or Custom Service;
- the host surface and exact business placement;
- input parameters, fields, variables, temp variables, connection variables, and output targets;
- security, privacy, tenant-specific configuration, and maintenance notes;
- runtime proof requirements.

Do not add Custom Code or Custom Service during materialization as an unplanned shortcut.

## When To Use Custom Code

Use a Custom Code control when the requirement needs client-side UI behavior or immediate client-side calculation that native Yeeflow controls and expressions cannot provide.

Use a Form Action Custom Code step when the requirement needs custom client-side action logic inside a form action and can be implemented with `execute(context, fieldsValues)`.

Do not use Custom Code for backend integrations, long-running server-side work, secret-bearing operations, or heavy file processing. Use Custom Service instead.

## When To Use Custom Service

Use Custom Service when the requirement needs backend execution, Node.js 22 logic, third-party integration through configured connection variables, server-side Yeeflow SDK operations, file processing, HTML generation, Excel parsing, or system-to-system synchronization.

Custom Service execution is server-side and queue-based. If the user needs immediate client-side response, the plan must either use Custom Code or include a waiting/progress/fallback UX.

## Surface Contracts

| Capability | Host | Required Entry Point / Shape |
| --- | --- | --- |
| Custom Code control | Page/form control tree | `render(context, fieldsValues, readonly)` |
| Form Action Custom Code step | `formdef.actions[].steps[]` | `execute(context, fieldsValues)` |
| Custom Service | `.ycs` backend service | `main({ connections, params, modules })` |
| Custom Service form action invocation | Approval form, data list custom form, dashboard form action | `type: "invokeservice"` |
| Custom Service workflow invocation | Approval/Data list/Scheduled workflow | `stencil.id = "InvokeCode"` |

## Binding Rules

- Approval form action Custom Service bindings use `__variables_`.
- Data list custom form action Custom Service inputs may use `__list_` or `__temp_`; outputs use `__temp_` unless a focused export proves another target.
- Dashboard form action Custom Service bindings use `__temp_`.
- Data list workflow Custom Service invocation may use `exprType: "list_field"` for current item fields and workflow variables for outputs.
- External integration Custom Service must use connection variables. Do not hardcode credentials, bearer tokens, client secrets, tenant-specific URLs, or private/internal URLs.
- Preserve Yeeflow 19-digit IDs as strings.

## Placeholder Rule

Rows marked `Not planned`, `N/A`, `None`, or `Not applicable` are planning placeholders only. They must not materialize into resources, action steps, workflow nodes, navigation items, variables, or package artifacts.

## Proof Boundary

Local package validation, signing, install/import/upgrade acceptance, and browser render proof do not prove Custom Code or Custom Service execution.

Runtime proof must demonstrate:

- parameter binding works;
- the code/service executes;
- outputs are written to the planned target;
- downstream refresh/update behavior works;
- failures produce clear errors or configured fallback behavior.

---

<a id="custom-code-form-action-step-runtime-standard"></a>

<!-- source: docs/standards/collection-01.md#custom-code-form-action-step-runtime-standard -->

# Custom Code Form Action Step Runtime Standard

## Scope

This standard covers Yeeflow **Form Action Custom Code steps**. It is separate from Yeeflow **Custom Code controls**.

Use this standard when custom TypeScript is executed as a step inside a form action. Do not apply Custom Code control rules blindly to this surface.

Current proof boundary:

- Runtime script contract: product-team specification.
- Package storage shape for a Custom Code action step: not yet export-proven by a decoded `.yap` / `.yapk` sample.
- Until the package storage shape is export-proven, generators may write the runtime script and planning guidance, but must not claim final package materialization support for Custom Code action steps.

## Surface Split

| Surface | Host | Primary entrypoint | Required output |
| --- | --- | --- | --- |
| Custom Code control | Page/form control tree, `type: "codein"` | `render(context, fieldsValues, readonly)` | UI/control rendering |
| Form Action Custom Code step | `formdef.actions[].steps[]` | `execute(context, fieldsValues)` | Action-side business execution |

Rules:

- A Custom Code control must include `render(...)`.
- A Form Action Custom Code step must include `execute(...)`.
- A Form Action Custom Code step must not be rejected merely because it does not include `render(...)`.
- A Custom Code control must not be generated with only `execute(...)`.
- If both entrypoints are missing, fail validation.

## Runtime Stack

Use the Yeeflow runtime assumptions from the product specification:

- React 15.6
- AntDesign 2.13
- TypeScript

When helper components are needed, use class components. Do not use React hooks, function components as stateful components, `forwardRef`, `memo`, or other modern React APIs unless a working Yeeflow runtime export proves support.

Only AntDesign 2.13 is built in. Third-party CSS must be loaded through `context.modules.loadCSS(...)` or implemented as scoped local styles.

## Entrypoint Contract

Preferred generated shape:

```ts
import * as React from "react";

export default class CodeInApplication implements CodeInComp {
  description() {
    return "Run a focused form action custom code step.";
  }

  inputParameters(): InputParameter[] {
    return [];
  }

  execute(context: CodeInContext, fieldsValues: any): Promise<void> | void {
    // Business logic goes here.
  }
}
```

Compatible named-export shape:

```ts
export class CodeInApplication implements CodeInComp {
  execute(context: CodeInContext, fieldsValues: any): Promise<void> | void {
    // Business logic goes here.
  }
}
```

Rules:

- Put the main action-side business flow in `execute(...)` or in helpers called by `execute(...)`.
- `execute(...)` may return `void` or `Promise<void>`.
- `render(...)` is not required for this surface.
- Keep generated scripts readable and editable.
- Avoid side effects outside the declared form action purpose.

## Context API

Use only runtime APIs that are available through `context`.

Expected context areas include:

- `context.modules.fetch`
- `context.modules.loadScript`
- `context.modules.loadCSS`
- `context.modules.yeeSDKClient`
- field read/write helpers such as `getFieldValue(...)`, `setFieldValue(...)`, and `setFieldsValue(...)` when available
- filter variables
- temp variables
- `formContext`
- `params`
- `connections`

All Yeeflow system interactions must go through `context.modules.yeeSDKClient`.

If SDK method names, parameters, or return shapes are unknown, use `getYeeSDKAPIDetails` when the authoring environment exposes it. If the SDK shape still cannot be proven, block generation or mark the implementation as requiring SDK confirmation. Do not guess tenant data mutation APIs.

## Input Parameters

Supported parameter types in this runtime contract:

- `string`
- `number`
- `variable`

Rules:

- Use `string` for static copy and static ids.
- Use `number` for numeric configuration when Yeeflow parameter metadata supports it.
- Use `variable` for field, temp variable, filter variable, or expression-editor inputs.
- Normalize runtime parameter values before using them. Variable parameters may resolve to primitives, arrays, objects, expression wrappers, or current variable values.
- For writable targets, resolve the target key separately from the current runtime value.

## Action-Side Safety Rules

Before generating a Custom Code action step, prefer native Yeeflow mechanisms:

1. standard field configuration
2. expressions
3. native form action steps
4. query data steps
5. submit steps
6. workflow actions
7. Custom Code action step

Do not use a Custom Code action step for approval routing, critical financial calculations, persistence, security, permission logic, or tenant-wide data mutation unless the requirement explicitly approves it and no native workflow or server-side action can satisfy the requirement.

When generated:

- Keep writes scoped to declared target fields or variables.
- Do not mutate readonly task data unless explicitly required.
- Avoid infinite loops or actions that retrigger themselves.
- Handle empty, null, and missing values safely.
- Keep external calls disabled unless the requirement explicitly allows them.
- Use diagnostics when the Yeeflow editor/tooling exposes diagnostics.

## Planning Requirements

App Plans that include Form Action Custom Code steps must state:

- `Custom Code Surface: form_action_step`
- host form/page
- host form action name
- trigger path
- custom code step purpose
- input parameters
- fields/variables read
- fields/variables written
- SDK calls required
- native fallback considered
- runtime proof required

If the App Plan only needs a visible custom UI component, use `Custom Code Surface: control` instead.

## Validator Requirements

Generators and validators must enforce:

- Control scripts require `render(...)`.
- Form Action Custom Code step scripts require `execute(...)`.
- A Form Action Custom Code step with only `render(...)` fails.
- A Custom Code control with only `execute(...)` fails.
- Missing `CodeInApplication` export fails.
- React hooks and stateful function components fail for generated reusable scripts unless runtime proof exists.
- Unknown SDK calls require explicit SDK-detail proof.
- Unresolved placeholders fail.
- Package materialization for action-step custom code stays gated until the form action step JSON storage shape is export-proven.

---

<a id="custom-service-nodejs22-runtime-standard"></a>

<!-- source: docs/standards/collection-01.md#custom-service-nodejs22-runtime-standard -->

# Custom Service Node.js 22 Runtime Standard

## Scope

This standard covers Yeeflow **Custom Service** scripts exported as `.ycs`.

Custom Service is a backend service surface. It is different from both existing Yeeflow custom-code surfaces:

| Surface | Runtime host | Required entrypoint |
| --- | --- | --- |
| Custom Code control | Page/form control tree | `render(context, fieldsValues, readonly)` |
| Form Action Custom Code step | Form action step | `execute(context, fieldsValues)` |
| Custom Service | Backend service sandbox | `main({ connections, params, modules }: ServiceContext)` |

Do not apply Custom Code control `render(...)` rules or Form Action Custom Code step `execute(...)` rules to Custom Service scripts.

## Export Shape

The studied `.ycs` files use this top-level structure:

```json
{
  "Name": "Service name",
  "Description": "Service description",
  "ImplType": 0,
  "DraftCode": "export async function main(...) { ... }",
  "DraftConfig": "{\"params\":[],\"connections\":[],\"outputs\":[]}",
  "ExtData": null
}
```

Rules:

- `DraftCode` is a TypeScript/JavaScript string.
- `DraftCode` must export `async function main(...)` or a compatible `function main(...)`.
- `DraftConfig` is a JSON string, not an object, and contains `params`, `connections`, and `outputs`.
- `ExtData` may be `null`.
- Preserve large numeric IDs as strings when a service references Yeeflow resources.

## Runtime Contract

The product-team runtime specification identifies:

- Language/runtime: Node.js 22 LTS style syntax.
- Execution environment: isolated ClearScript V8 sandbox.
- Primary business logic entrypoint: `main`.
- Context argument shape: `{ connections, params, modules }`.

Preferred generated entrypoint:

```ts
export async function main({ connections, params, modules }: ServiceContext) {
  return {};
}
```

Rules:

- If current code is empty or `main` is missing, create `export async function main({ connections, params, modules }: ServiceContext) { return {}; }` first.
- Put the primary business flow directly in `main` or in helpers called by `main`.
- Return an object whose keys match configured output IDs.
- Validate `params` before use.
- Use optional chaining and safe defaults for nested values.
- Throw clear `Error` messages for blocking validation failures.

## Sandbox And Security Rules

Generated Custom Service code must not use Node.js built-ins or global Node-only objects:

- no `require(...)`
- no `import ...` for Node standard libraries such as `fs`, `path`, or `crypto`
- no `process`
- no `global`
- no `__dirname` or `__filename`
- no `setTimeout` or `setInterval` unless explicitly provided in `ServiceContext`

Credential and network rules:

- Never hardcode credentials, API keys, bearer tokens, passwords, or client secrets.
- If an external HTTP request requires authorization, use `connections[connectionId]` and pass it to `modules.fetch(..., { connection })`.
- Do not hardcode private/internal URLs or fallback addresses.
- Block internal network / SSRF hazards such as `localhost`, `127.0.0.1`, `::1`, `169.254.169.254`, RFC1918 private ranges, `.local`, and `.internal`.

## System Interaction Rules

All Yeeflow system interactions must go through `modules.yeeSDKClient`.

If SDK method names, parameter names, or response shapes are not fully proven, use `getYeeSDKAPIDetails` where the authoring environment exposes it, or mark the implementation as blocked. Do not guess mutation APIs.

Export-backed examples show:

- `modules.yeeSDKClient.files.getContent(fileId)` for reading file parameters.
- `modules.yeeSDKClient.files.upload({ fileName, file })` for generated file outputs.
- `modules.yeeSDKClient.lists.getFields(listId)` for list metadata.
- `modules.yeeSDKClient.lists.createItemsBatch({ listId, items })` for batch list creation.
- `modules.yeeSDKClient.lists.queryItems({ listId, fields, filters, sorts, pageIndex, pageSize })` for list queries.

## File Parameters

File input values use the standard file object shape:

```json
{
  "id": "file-id",
  "name": "file-name.pdf",
  "fileSize": 1024
}
```

Rules:

- A file parameter may be a single file object or an array of file objects.
- Read content with `modules.yeeSDKClient.files.getContent(fileValue.length ? fileValue[0].id : fileValue.id)`.
- The returned `response.data` is an `ArrayBuffer`.
- Generated file outputs should be uploaded with `modules.yeeSDKClient.files.upload({ fileName, file })`.

## DraftConfig Rules

Use `DraftConfig` to declare service IO.

Observed export-backed parameter and output types include:

- `text`
- `file`
- `number`

Product-supported Custom Service input and output variable types include:

- Text
- Number
- Boolean
- Date/Time
- File
- Image
- Rich text

Connection variables are configured separately from input and output variables. A service may have zero or more connection variables.

Product runtime input definitions may use `string`, `number`, and `variable` when authoring through code interfaces. For `.ycs` export generation, preserve the `.ycs` UI/export type names observed in `DraftConfig`.

Example:

```json
{
  "params": [
    { "id": "subListJson", "type": "text", "desc": "JSON array from a Sub List." }
  ],
  "connections": [],
  "outputs": [
    { "id": "htmlTable", "type": "text", "desc": "Generated HTML table." }
  ]
}
```

Rules:

- Every value returned by `main` should correspond to an output ID.
- Every required runtime input should be declared in `params`.
- Connections required for external calls should be declared in `connections` and read from `connections[connectionId]`.
- Do not treat output IDs as ordinary input values.

## Connection Variable Rules

Custom Service connection variables are declared in `DraftConfig.connections[]`:

```json
{
  "connections": [
    {
      "id": "sharePointConnection",
      "type": "http",
      "desc": "SharePoint OAuth connection used to authenticate and access the specified SharePoint site and list."
    }
  ]
}
```

Runtime code reads the configured connection by ID:

```ts
const sharePointConnection = connections?.sharePointConnection;
if (!sharePointConnection) {
  throw new Error("Required connection missing: sharePointConnection");
}
```

Rules:

- Use stable semantic connection IDs such as `sharePointConnection`; do not use display labels as code identifiers.
- Validate every required connection before calling external systems.
- Do not hardcode OAuth tokens, API keys, or bearer credentials.
- Pass the configured connection object to `modules.fetch`:

```ts
const response = await modules.fetch(url, {
  method: "GET",
  connection: sharePointConnection
});
```

The SharePoint export proves the pattern for Microsoft Graph calls: resolve the SharePoint site, then create list items through Graph API endpoints using `modules.fetch(..., { connection })`.

## Invocation Surface Rules

Custom Service can be invoked by Yeeflow actions. The exported examples prove these invocation shapes:

### Form Action Step

Approval submission forms and custom data list forms use a form action step:

```json
{
  "type": "invokeservice",
  "attrs": {
    "serviceId": "2072624440809492481",
    "params": [],
    "outputs": []
  }
}
```

Rules:

- `attrs.serviceId` is the Custom Service ID.
- `attrs.params[]` entries must use parameter IDs from the Custom Service `DraftConfig.params[]`.
- `attrs.outputs[]` entries must use output IDs from `DraftConfig.outputs[]`.
- Do not rename service params or outputs at invocation time.

Approval form variable input:

```json
{
  "value": {
    "value": { "prefix": "__variables_", "value": "Expense_Details" },
    "variable": null
  }
}
```

Approval form variable output:

```json
{ "prefix": "__variables_", "value": "field_SummaryInfo" }
```

Data list custom form field input:

```json
{
  "value": {
    "value": { "prefix": "__list_", "value": "Text1" },
    "variable": null
  }
}
```

Data list custom form temp-variable output:

```json
{ "prefix": "__temp_", "value": "var_HTMLTable" }
```

Dashboard form actions use the same `invokeservice` step shape, but Dashboard pages only have temp variables in the studied product guidance. Use `__temp_` bindings for Dashboard inputs and outputs unless a future export proves another binding surface.

### Workflow Action Node

Approval workflows use an Invoke custom service workflow node:

```json
{
  "stencil": { "id": "InvokeCode" },
  "properties": {
    "serviceId": "2072624440809492481",
    "params": [],
    "outputs": []
  }
}
```

Workflow variable input:

```json
{
  "type": 1,
  "value": {
    "exprType": "variable",
    "valueType": "list",
    "id": "Expense_Details",
    "type": "expr"
  }
}
```

Workflow variable output:

```json
{ "prefix": "__variables_", "value": "EmailHTMLItems" }
```

Data list workflows use the same Invoke custom service workflow action shape. A focused Data list workflow export also proves `properties.connections[]` for connection variable selection:

```json
{
  "id": "sharePointConnection",
  "type": "http",
  "desc": "SharePoint OAuth connection used to authenticate and access the specified SharePoint site and list.",
  "value": {
    "connectionid": "2056384963824988160",
    "userconnectionid": "0"
  }
}
```

Data list workflow inputs can bind current item fields through list-field expressions:

```json
{
  "type": 1,
  "value": {
    "exprType": "list_field",
    "valueType": "input",
    "prop": "Title",
    "id": "Title",
    "type": "expr"
  }
}
```

Static string inputs use expression token arrays:

```json
{
  "type": 2,
  "value": [
    { "type": "str", "value": "https://example.sharepoint.com/" }
  ]
}
```

After the service returns an output to a workflow variable, a downstream `ContentList` / Set Data List action may write a generated link or value back to the current data list item. The Supplier List export proves this for a SharePoint item URL composed from a static URL prefix plus the returned `Supplier_ID` workflow variable.

Scheduled workflows use the same Invoke custom service workflow action shape by platform rule, but this study did not include a Scheduled workflow export. Keep Scheduled workflow invocation product-rule-backed until a focused export or runtime test proves it.

## Execution Semantics

Custom Service is executed on the server side. Calling it from a form action or workflow action sends a request to the server and the service is processed through backend queue execution. If a page or form needs immediate client-side behavior, such as instant arithmetic, instant UI state changes, or low-latency validation, prefer:

- a Custom Code control; or
- a Form Action Execute custom code step.

Use Custom Service when server-side execution, Yeeflow SDK access, file processing, external connections, or reusable backend logic is the right model.

## Learned Examples

### Insert Excel Data to Data List

Export-backed capabilities:

- reads an uploaded Excel file parameter;
- loads SheetJS through `modules.loadScript(...)`;
- parses the first worksheet;
- normalizes a mapping JSON input;
- validates target fields through `lists.getFields(...)`;
- creates rows through `lists.createItemsBatch(...)`;
- returns `{ "Added Number": addedNumber }`.

Runtime-sensitive notes:

- External script loading must use an explicitly allowed public URL.
- Batch creation API shape is export-backed in the example but still should be treated cautiously for broad generation.

### Sub List to HTML Table

Export-backed capabilities:

- reads `subListJson` as either an array or a JSON string;
- detects business columns dynamically;
- escapes HTML;
- renders an email-compatible inline-style table;
- returns `{ htmlTable }`.

Runtime-sensitive notes:

- HTML generation must escape user-provided content.
- The service is display-transform only and does not mutate Yeeflow data.

## Planning Requirements

When a generated App Plan includes Custom Service, it must state:

- `Custom Service Surface: custom_service`
- service name and purpose
- caller candidates, such as form action, workflow action, AI Agent, or Copilot
- connection variables and their external system purpose
- input parameters
- output parameters
- connection requirements
- Yeeflow SDK calls required
- native fallback considered
- security and data-scope notes
- proof boundary: export-proven, validator-backed, runtime-tested, or deferred

Current training scope proves the invocation schema for selected Form Action and Workflow Action surfaces, but does not prove backend queue latency, broad package materialization, AI Agent invocation, or Copilot invocation.
When App Plans include Custom Service invocation, also state:

- caller surface: Approval form action, Task form action, Data list custom form action, Dashboard form action, workflow action, AI Agent, or Copilot;
- binding source for each input: approval variable, workflow variable, data list field, or temp variable;
- binding target for each output;
- whether backend queue latency is acceptable or a client-side Custom Code surface is preferred.

## Validator Requirements

Generators and validators must enforce:

- `.ycs` top-level export fields exist.
- `DraftConfig` parses as JSON string and contains arrays for `params`, `connections`, and `outputs`.
- `DraftCode` includes `function main` or `async function main`.
- `main` returns an object or has an explicit object-return path.
- no `render(...)` or `execute(...)` entrypoint is required for Custom Service.
- no Node built-ins or forbidden globals are used.
- no hardcoded credentials or private/internal URL targets are present.
- file handling uses `modules.yeeSDKClient.files`.
- Yeeflow data interactions use `modules.yeeSDKClient`.
- form action Custom Service invocation uses `type = "invokeservice"` with `attrs.serviceId`, `attrs.params[]`, and `attrs.outputs[]`.
- workflow Custom Service invocation uses `stencil.id = "InvokeCode"` with `properties.serviceId`, `properties.params[]`, and `properties.outputs[]`.
- workflow Custom Service connection selections use `properties.connections[]` with `id`, `type`, `desc`, and `value.connectionid`.
- invocation parameter/output binding prefixes match the host surface: `__variables_`, `__list_`, or `__temp_`.

`Approval form workflow sample-V1.6.yapk` additionally export-proves Scheduled Workflow `InvokeCode` with a List workflow variable passed as an expression (`valueType = "list"`) and a text service output stored under the `__variables_` prefix. This proves serialization only; scheduled execution and service runtime remain separate proof boundaries.

## Proof Boundary

This training is **export-proven** and **validator-backed** for `.ycs` structure and runtime script contract.

This training is **export-proven** for:

- invoking Custom Service from Approval Submission form Form Action;
- invoking Custom Service from Approval workflow action;
- invoking Custom Service from a custom data list form Form Action.
- declaring and using Custom Service connection variables in `.ycs`;
- invoking Custom Service from Data list workflow with a selected connection variable and list-field/static parameter bindings.

This training is **product-rule/user-guidance-backed** for:

- invoking Custom Service from Approval task forms;
- invoking Custom Service from Scheduled workflow actions;
- invoking Custom Service from Dashboard form actions with temp variables.

It is not yet runtime-proven for:

- backend queue timing or throughput under load;
- invoking Custom Service from AI Agent or Copilot;
- external connection execution;
- broad package materialization inside `.yapk` or `.yap` app generation.

---

<a id="dashboard-collection-choice-pagination-standard"></a>

<!-- source: docs/standards/collection-01.md#dashboard-collection-choice-pagination-standard -->

# Dashboard Collection choice styles and pagination

## Planning and composition rules

For Collection choice fields such as Channel and Status, prefer conditional Container + Text badges when a compact visual category/status is useful. Determine single/multiple selection from source schema, not a field label. Map actual stored option values separately from visible labels. Use source/configured choices and known historical values; do not invent states or change records to match a design. Record the choice field, stored shape, palette, fallback and multi-value presentation in the Collection plan. Respect an explicit request for plain text or a different presentation.

The badge Container owns the Dynamic style condition and uses its own ID as controlId. Set all three channels independently: background (`normal.bgcolor`), border (`normal.border.color`, with width/type), and text (`normal.color`) in JSON action_style. Its Text inherits color and binds the real field. Border color need not equal text color. Preserve native columns, sorting, current-item expressions, action bindings, responsive item trees and schema-compatible Dynamic controls required by the selected template. Do not replace a whole native Collection with an imitation table to obtain badges.

Use `buildCollectionNativeBadge` for the established single-choice Container/Text pattern. Use `buildCollectionChoiceStyleRule` and `buildChoiceStyleCondition` in `scripts/lib/dashboard-collection-presentation.mjs` for condition/style construction. Single-choice uses equality. For a schema-verified array of scalar option values, multi-choice uses `arrayIndex(field, exactOption) >= 0`; do not compare the entire array to one string or use substring matching (Web must not match Webinar). Delimited text, lookup-object arrays, or unknown storage require explicit mapping evidence; the helper rejects unverified multi-choice storage.

For multi-choice presentation, prefer one badge per selected option after verifying item/display-rule shapes. If styling a combined label instead, plan mutually exclusive combination conditions or an explicit verified priority; do not depend on undocumented ordering of multiple matching color rules. Keep all selected values visible and define unknown/empty behavior. Unknown values use a neutral fallback rather than disappearing; empty does not mean Draft/Approved. Dynamic display rules control visibility, while Dynamic style controls appearance; one does not replace the other. Per-option rendering, conflict resolution and responsive wrapping require focused runtime proof before promotion. The membership/style builders are locally validated from known expression/schema shapes; this round does not establish multi-choice runtime behavior.

## Default pagination

New ordinary Dashboard record Collections default to **10 records per page**. The serialized property is `attrs.data.ps = 10`; `attrs.pagination.p` is pagination visual style, not page size. Golden templates may contain 9/16/20: those are source defaults, not user overrides. Apply the default after cloning/mapping, not by changing every source template.

An explicit Collection plan `Records per page` or `Page size` column overrides 10 and must be a positive integer. Plan and generation helpers carry the value into the resource. Preserve pagination display, data filters, sorting, search, sources and page style. Never interpret page size as permission to truncate/delete records. Paging must still make subsequent records reachable.

Exceptions: current-item detail Collections stay at 1; intentionally limited/top-N regions, dedicated print datasets and user-requested pagination behavior keep their semantic contracts. This default is for new generation. Existing dashboard upgrades preserve user-configured page sizes unless changing pagination is part of the authorized scope. Approval/Data List form Collections are outside this Dashboard policy.

Use `applyDashboardCollectionPagination` for new datasets. The full-app Dashboard builder and shared standalone path call it after native template cloning; master-detail left lists use it while selected-detail remains 1. Do not bypass the shared builder for standalone pages.

## Regression and proof

`scripts/test-dashboard-collection-presentation.mjs` exercises actual Dashboard materialization for default 10, explicit override and master-detail 1, plus invalid sizes, existing/limited preservation, exact multi-choice membership, and separate border/background/text. It runs through the native-rich training suite in source and distribution.

Toolbar scope, Select alignment, automatic menu placement and foreground contrast follow `docs/standards/collection-01.md#collection-toolbar-style-standard`. Generated known dark row menus now use the tested autoposition and scoped foreground helper. Add/Select height fallback remains opt-in on the owning control rather than being applied to every template.

Proof: previous desktop toolbar save/readback/render observations remain valid only for that live repair. This round is local training and generated-resource regression. New page-size runtime navigation, multi-choice badges, hover/active states and mobile behavior are separate acceptance items. No plugin publication or live dashboard change is implied.

## Collection content density

Follow `docs/standards/collection-02.md#native-collection-density-standard`. For native Table/Card Collections, plan field-aware widths after actual container placement, using native `cw/cwu`; limit narrative Dynamic fields through `t-len` only with a complete-value route. Collection selection rows accept optional `Column widths` (JSON map), `Text lengths` (JSON map) and `Full value access` (`source-list`, `record-detail`, `none`). The real materializer consumes these fields and rejects invalid overrides. Keep 10-record defaults, choice styles, scoped toolbar/menu behavior, sorting, source bindings and mobile card content intact. Pixel/percentage examples are mapping evidence, not universal layout defaults.

---

<a id="dashboard-collection-schema-dependency-closure-standard"></a>

<!-- source: docs/standards/collection-01.md#dashboard-collection-schema-dependency-closure-standard -->

# Dashboard Collection Schema and Dependency Closure Standard

## Schema-bound clone rule

Golden Collection templates provide structure and style, not reusable business fields. After cloning, recursively inspect every `__ctx_coll` expression, including headings, Dynamic controls, progress values, visibility conditions, filters, sorting, actions, and operation menus.

- A display expression referencing a source-template field must be remapped to a real field from the selected Data List schema.
- An unprovable source-template condition must be removed as a complete condition block; do not leave partial formula tokens.
- Grid header/item cells with invalid bindings must be removed together and their column tracks normalized.
- Progress controls require a real numeric/percent field.

`DASH_DATASET_COLLECTION_CTX_FIELD_MISSING` is always a generated-final blocker.

## Lookup-aware Dynamic field contract

A Dashboard `dynamic-field` that reads a Collection, Kanban, or Timeline current item (`attrs.source = "3"`) must be type-aware. The stored value of a Lookup is a target-record identity, not its business label.

- Resolve `attrs["obj-f"]` on the host dataset. When it is a Lookup, resolve the Lookup rule's target list and configured display field (for example `Rules.listid` and `Rules.listfield`).
- Set `attrs["dis-f"]` to that resolved target display field. Do not hard-code `Title`; `Title` is correct only when the Lookup itself configures `Title` as its display field.
- During clone/remap, retain the current application's source field mapping, then derive `dis-f` from the remapped Lookup metadata. Never preserve a source-template `dis-f` without resolving the target list again.
- Do not use a source-field display name, a lookup ID, or a target field that is absent from the resolved target list as `dis-f`.

The generated-final Dashboard gate fails closed on `DASH_DYNAMIC_LOOKUP_SOURCE_LIST_UNRESOLVED`, `DASH_DYNAMIC_LOOKUP_TARGET_UNRESOLVED`, `DASH_DYNAMIC_LOOKUP_DISPLAY_FIELD_REQUIRED`, `DASH_DYNAMIC_LOOKUP_DISPLAY_FIELD_UNRESOLVED`, and `DASH_DYNAMIC_LOOKUP_DISPLAY_FIELD_MISMATCH`.

After `component_save`, distinguish API acceptance and persisted readback from a Designer-open check of the Dynamic field's Display field and a browser row-render smoke using a real associated record. The first two levels do not establish that users see the intended lookup label.

## Designer-native column contract

A Dashboard Collection is not correct merely because rows render or `component_save` accepts its JSON. Its selected Golden Reference determines the required Designer control tree.

- Create and update a Collection by cloning the complete selected template subtree. Never reconstruct `attrs.tablecols` from a field list or from a visually similar table.
- Preserve the template's native Collection regions (`header`, `body`, `table`, `pagination`, and the template-specific item/Card tree) unless the template explicitly marks a region editable.
- When the selected Golden Reference uses Designer-native `list-column` nodes (currently `collection_control_responsive_multiple_select`), every retained/generated column must keep `type: "list-column"`, a unique UUID `id`, and a unique UUID `mapkey`. A non-selection column must contain a Dynamic child with `attrs.source = "3"` and a resolved `attrs["obj-f"]` target field.
- This rule is template-aware. Do not inject `list-column` wrappers into a different approved template whose exported shape does not use them; doing so creates an unsupported hybrid. Instead, clone that template's own column shape exactly.
- On an update, modify only the approved business slots. Preserve all unknown native attributes and container hierarchy. Replacing the Collection or rebuilding its columns is a failure unless a new approved template is explicitly selected and fully re-materialized.

Run the Designer-column contract before save and again after `component_get` readback. `DASHBOARD_COLLECTION_DESIGNER_COLUMN_INVALID` and `DASHBOARD_COLLECTION_DESIGNER_COLUMN_DYNAMIC_BINDING_INVALID` are fail-closed. A focused Designer-open check must confirm that every column and Dynamic child can be selected and that no `formcraft.control.undefined` node appears. Browser row rendering is separate evidence and cannot replace Designer-open proof.

## Operation dependency closure

Keeping Edit/Delete/multiselect operations also keeps their complete dependency graph: Collection actions, page actions, filter variables, temp variables, confirmation state, and current-item `ListDataID` context.

Template instances must namespace page variables per page and dataset region. Validators must recognize a namespaced declaration by semantic suffix, for example:

```text
var_<page>_<region>_isDeleteConfirmed
var_<page>_<region>_SelectedItems
var_<page>_<region>_SelectedItemsAmount
```

Validators must not require the unscoped source-template ID when a fully linked namespaced declaration exists. Conversely, an undeclared `__temp_` reference remains a hard failure.

## Unplanned template modules

Page-layout modules not selected by the App Plan must be removed with all descendants and dependencies. In particular, Data List Workbench forms must not retain Event Portfolio KPI cards or their `__temp_event_portfolio_*` expressions unless those KPI modules are explicitly planned and materialized.

## Required gates

Run before signing:

```bash
node scripts/test-dashboard-dataset-presentation-golden-references.mjs
node scripts/test-full-app-materialization-entrypoint-gates.mjs
node scripts/yapk-first-generation-preflight.mjs --package <package.yapk> --plan <yeeflow-app-plan.md> --id-provenance <report.json>
```

---

<a id="dashboard-dataset-composition-standard"></a>

<!-- source: docs/standards/collection-01.md#dashboard-dataset-composition-standard -->

# Dashboard Dataset Composition

Introduced in plugin 1.15.4. This contract applies to full-app YAPK, standalone YDP, generated-final validation and incremental Dashboard repairs. Use `scripts/lib/dashboard-dataset-composition.mjs`; run `scripts/test-dashboard-dataset-composition.mjs` and the existing Dashboard/standalone gates.

## One title owner

The registered `dataset-caption-v1` variant preserves `content_card_wrapper` (including 60/40 variants), its width/padding/background, and `section_content_area`. Set `attrs.dashboardCardVariant` to `dataset-caption-v1`. Omit the direct `section_title_area` only when the content slot contains exactly one Collection or Kanban with a nonempty section caption. Keep the entire component caption, operations, table header and dataset subtree. A Kanban lane heading or Collection column heading is not a section caption.

This conditional variant overrides the generic v1.1 mandatory outer-title rule. An unmarked card without a title still fails; a forged variant without a real caption, with mixed datasets, or with a retained outer title fails. Do not globally make all card titles optional. For multi-dataset or mixed chart/dataset sections, keep a meaningful grouping title. For captionless controls, keep the outer title.

Remove duplicated outer titles and the generated filler descriptions `Review <title> and open a record for details.` or `Review <title> records and related activity.`. Preserve independent scope/method descriptions, dynamic title expressions and configured outer operations. If these are needed, keep the titled card; do not silently discard them. A later explicitly planned consolidation may move unique explanatory content into the component caption, with dependency preservation and runtime review.

## Toolbar and responsive layout

Preserve reference child order: Search, then Add, then other actions. Normalize only recognized dataset operation rows (`op_normal`, `grid_table_col_operations`, and caption-bearing Kanban operation rows), never arbitrary page actions. Preserve Add permissions, collection action IDs, filter binding/variables, fulltext consumers, bulk-selection state, table columns and mobile Card layout. Source generation and incremental edits use the same normalizer and validator.

Retain Golden reference responsive caption/operations width and direction shapes. Use a wide one-column section for dense tables; use multi-column cards only for short comparable content. At narrow sizes the caption and operations must remain usable according to the selected responsive reference; do not force a search input to squeeze alongside a button or overwrite responsive slots with desktop-only widths. Desktop half-card and tablet/mobile visual checks must verify Search legibility, Add visibility, wrapping and overflow. Structural checks are not browser rendering proof.

## Empty layout cleanup

After materialization and any later action/template cleanup, remove empty known layout modules bottom-up, including `kpi_cards_kpi_row` and now-empty KPI/section wrappers. The final gate rejects leftovers. Do not equate zero query results with an empty control tree: retain bound Collection/Kanban and other real controls, meaningful text, Custom Code and configured action controls. Do not indiscriminately delete all containers or controls with empty children.

## Repair and evidence

For existing Dashboards, first read the exact application/page and preserve IDs, data bindings, actions and existing business changes. Apply the shared composition helper to the decoded resource, validate the result, and use the supported incremental save/readback route. Check layout and actions in Designer/runtime separately. Updating this plugin does not itself modify previously generated applications.

The regression uses tenant-neutral Golden references and synthetic structures. Kanban composition coverage is conditional structural coverage, not a new Kanban template or certified runtime interaction. No customer exports are distributed.

---

<a id="dashboard-page-layouts-custom-code-standard"></a>

<!-- source: docs/standards/collection-01.md#dashboard-page-layouts-custom-code-standard -->

# Custom Code Dashboard Golden Reference

`dashboard-page-layouts-custom-code` is a registered alternate page-level Dashboard template. It does not replace the default `dashboard-page-layouts-v1.1`.

The canonical exported page resource is `docs/reference/dashboard-page-layout-custom-code.template.json`; registry entry: `docs/reference/dashboard-page-layout-templates.json`. The detailed design and normalization provenance are in `docs/templates/dashboard-page-layouts-custom-code/`. Proof is export-backed and locally validator-backed; rendered Custom Code, reactive variables, native filter consumers and CSS-ID action invocation require runtime evidence on the actual generated page.

## Selection and requirement traceability

Prefer this template for an explicit user request to implement the Dashboard primarily through Custom Code. Consider it for a customer image/mockup combined with complex business presentation or interactions. A screenshot alone is not proof of behavior. Capture the request, reference image, business functions, interactions and device expectations in Functional Specification source/requirements sections with stable requirement markers. Do not put control JSON, variable IDs or CSS selectors into the Functional Specification.

App Plan section 14 must explicitly select this template in the existing Dashboard Page Layout Template Selection table and preserve the originating requirement markers. Include the three planning sections below, even when a particular integration is not needed; write an explicit `None — <reason>` row for unused integrations. Standalone Dashboard plans use the same tables and rules. No generic dataset or KPI should be synthesized merely because the page selects this template.

## Required planning tables

### Custom Code Dashboard Composition

| Dashboard Page | Requirement Marker | Header Title | Header Description | Header Operations | Retained Panels | Top Content | Bottom Content | Selection Rationale |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Page> | <REQ-...> | <Text or None> | <Text or None> | <Real actions or None> | <left_side_panel + primary_content_area + right_side_panel, or a subset retaining primary> | <Purpose or None> | <Purpose or None> | <Explicit request / image-backed complex page / custom business presentation> |

### Custom Code Module Plan

| Dashboard Page | Region | Module Key | Layout Pattern | Business Function | Input Values | Writable Outputs | Implementation Artifact | Change / Refresh Behavior |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Page> | <retained region> | <business module name> | stack / <registered region Grid label> | <bounded function> | <values or None> | <temp-variable purpose or None> | <planned Custom Code artifact> | <input-change reaction or static> |

### Custom Code Communication and Native Integration

| Dashboard Page | Kind | Producer / Trigger | Shared Value / Action Purpose | Consumers / Target | Initial / Reset Behavior | Timing / Refresh Rule |
| --- | --- | --- | --- | --- | --- | --- |
| <Page> | Temp variable / Native Data Filter / Native Button + Form Action / None | <module, filter or interaction> | <business state or action> | <consumers or actual target> | <behavior> | <change reaction and action ordering> |

A module plan is not executable Custom Code. Before materialization, produce the concrete control artifacts through the Custom Code generator and bind their parameters to declared page dependencies. Keep code/control payloads in a separate reviewed build context, not embedded in the business planning tables.

## Shared materialization contract

`scripts/lib/dashboard-custom-code-template.mjs` exports `buildCustomCodeDashboard({ name, composition })`. The full-app materializer dispatches to this helper for this exact template, taking a page-name keyed build-context object via `customCodeDashboards` or CLI `--custom-code-dashboards <file.json>`. A missing concrete composition fails; it never falls back to a generic Collection or empty placeholders.

The build-context `composition` contains:

- `panels`: retained canonical panel names, including `primary_content_area`.
- `header`: optional `title`, `description`, and concrete action-bound `operations` controls. Empty header parts are removed.
- `modules`: ordered entries `{ region, layout, controls }`; `layout` is `stack` for one or more vertically stacked full-width concrete controls, or a region-owned internal Grid's `nv_label`. Each entry's controls must be export-shaped, populated artifacts. Native filters and native action-host controls may coexist with Custom Code in a region.
- `dependencies`: declared `tempVars`, `filterVars`, `actions`, `formAction`, `exts`, and `ReportIds` as required by the concrete controls. Namespace them per page and module; do not supply source-tenant identities as generated identities.

The same helper is the page-body entry point for standalone preparation. Existing standalone Core build-result, source binding, issued identity, dependency closure and wrapper validation requirements still apply; this helper does not fabricate them or return a wrapper-ready Core artifact. Use the supported Core materialization context to carry the prepared body. Unsupported Core preparation is an explicit failure, not permission to handcraft its provenance. `build-ydp-wrapper.js` and `validate-ydp.js` continue to enforce that contract.

## Composition rules

- Title, description, individual operation controls, `Operations`, and the entire visible header are optional. Remove unused wrappers. Page metadata title remains required.
- Top/bottom regions and side panels are optional. Preserve `main > content > main_content_area > main_content_wrapper` and non-empty primary content. Keep meaningful native filter/action hosts during cleanup.
- Desktop tracks: all panels `1fr 2.5fr 1.5fr`; left + primary `1fr 2.5fr`; primary + right `2.5fr 1fr`; primary only `1fr`. Delete the actual unused tracks, not just the controls.
- Tablet tracks retain `1fr 3fr` when left + primary remain; right spans the next full row if also retained. Without left, stack retained panels in one column. Mobile is one column with explicit right span 1. These reduced-layout tablet variants require populated runtime verification.
- Internal Grid patterns are optional. Duplicate direct slots for vertically stacked modules. Preserve exported responsive attributes for retained internal Grids. The user accepted two tablet columns inside `left_side_3_columns_grid`.
- Recognize placeholders by `type=codein` AND `nv_label=custom_code_placeholder`; enumerate, do not treat the label as unique. Fill/rename or remove every placeholder. Generate new control IDs for clones while preserving existing IDs on in-place updates.
- Split code by business function. Share values through declared temporary variables bound to output targets and consumer inputs. Verify consumers update or re-query on changes and avoid stale-response races.
- Prefer native Data Filters for supported search/select/radio criteria; pass their own filter variables into consuming Custom Code parameters. Implement custom filtering only when native behavior does not meet the requirement.
- Prefer `CodeInContext.executeAction` for Custom Code record opening and page actions on supported hosts. Use the Custom Code Generator direct-form-action-execution reference. Define target actions and variables, verify context propagation, and serialize shared row inputs before calling. Keep native Button/CSS-ID triggers only for verified legacy compatibility; do not silently fall back after a failed direct call.

## Validation and evidence

The page-layout validator dispatches to `validateCustomCodeDashboard` for this registry entry. It checks the native shell, panel ordering, exact outer tracks, right spans, internal Grid shapes, unique control IDs and generated-final placeholder/script/empty-region rules. Common action-resolution and page background/padding checks continue to run. The source reference is allowed to contain empty placeholders; generated output is not.

Planning gates require all three sections for a selected Custom Code page. The standalone trace must select the same template and contain planned sections. Match the concrete modules and dependency wiring to the plan before delivery. Runtime tests must independently cover data, temp-variable reactions, filter propagation and native action invocation; local layout validation does not prove these behaviors.

---

<a id="dashboard-page-layouts-workbench-standard"></a>

<!-- source: docs/standards/collection-01.md#dashboard-page-layouts-workbench-standard -->

# Dashboard Page Layouts Workbench Standard

## Purpose

`dashboard-page-layouts-workbench` is an approved alternate Dashboard page layout for operational workbench pages. It coexists with the default `dashboard-page-layouts-v1.1` general Dashboard shell. The App Plan must explicitly select the page layout template for each Dashboard page before generation.

Use Workbench when the page needs a primary working area plus optional supporting context, such as queues, operational follow-up lists, filters, KPI cards, and compact analytics panels. Use the default `dashboard-page-layouts-v1.1` shell for general overview dashboards, broad reporting pages, and simpler section-first dashboards.

## Registered Source

The registry is:

`docs/reference/dashboard-page-layout-templates.json`

The complete extracted template resource is:

`docs/reference/dashboard-page-layout-workbench.template.json`

The template id and version are:

`dashboard-page-layouts-workbench`

The source was extracted from `Workbench Dashboard` in the manually exported `Application page layouts-V2.1.yapk`.

## Required Shell

Generated Workbench dashboards must copy the registered Workbench shell before business mapping. Preserve:

- page root `attrs.hideHeaderAll = true`
- root background `#f4f7fb`
- zero root page padding
- `main > content`
- canonical Workbench `content` padding
- `page_title_header`
- optional top `section_content_area` for filters or other valid top content
- optional top `kpi_cards_kpi_row`
- `main_work_queue_section`
- `main_work_queue_wrapper`
- `primary_working_area`
- optional `right_side_panel`

Do not copy a source app root shell, source IDs, source fields, or source domain labels from the template package.

## Workbench Structure

The primary structure is:

- `page_title_header`: page title, description, and optional operations.
- top `section_content_area`: usually filter controls such as `dashboard_standard_filter_group`; it may also host other approved top-level Workbench content when planned.
- top `kpi_cards_kpi_row`: optional KPI cards.
- `main_work_queue_section`: the main two-panel workbench region.
- `main_work_queue_wrapper`: parent of the primary area and optional right-side panel.
- `primary_working_area`: the main operational queue/analytics area.
- `right_side_panel`: optional supporting context panel.

## Header And Operations Contract

The Workbench Header is a fixed template module, not a convenient place to append a new container. Its direct-child structure is:

```text
page_title_header
├─ page_title_content  (attrs.style.widthtype = [null, "2", null, "1"])
└─ Operations          (present only when a real Header action is planned)
   └─ btn_operation_primary | btn_operation_normal
```

`page_title_content` must retain the registered inline-width shape. Omitting it makes the title content Full Width and prevents the operation area from aligning as the Workbench template intends.

For any planned Header action, copy `Operations` and clone either `btn_operation_primary` for the primary create/commit action or `btn_operation_normal` for a secondary action. Configure the action only after cloning its module. Do not append an action button or a generic `container` directly under `page_title_header`, and do not hand-create a replacement button style. Remove the entire `Operations` module only when no Header actions are planned.

Before `component_save`, run the Workbench page-layout validator. `DASH_WORKBENCH_PAGE_TITLE_CONTENT_NOT_INLINE`, `DASH_WORKBENCH_HEADER_OPERATIONS_MISSING`, `DASH_WORKBENCH_HEADER_ACTION_OUTSIDE_OPERATIONS`, and `DASH_WORKBENCH_OPERATION_BUTTON_VARIANT_INVALID` are generated-final blockers. After save, distinguish API acceptance/readback from Designer-open and browser action runtime evidence; click each Header operation before declaring it usable.

## Standard Filter Group

When a Workbench Dashboard has two or more page-level/global Data Filter controls, the filters must be placed inside `dashboard_standard_filter_group` in the top `section_content_area`.

For Workbench, `dashboard_standard_filter_group` must be copied from `docs/reference/data-filter-standard-filter-group.template.json` exactly as the standard `flex_grid` control:

- PC/laptop: four `1fr` columns
- tablet: two `1fr` columns
- mobile: one `1fr` column
- one `auto` row definition
- 16px column gap and 16px row gap
- `displayLabel = [null, false]`
- `nv_label = "dashboard_standard_filter_group"`

Each Grid cell may host one mapped Data Filter control. The generator may add rows through the Grid's normal wrapping behavior when there are more than four filters, but it must not replace the Grid with a container or use simplified `columns.count/type/minmax` properties. Those simplified Grid properties are not Designer-safe and are a generated-final error.

## Main Work Queue Columns

`main_work_queue_wrapper` starts from the two-column Workbench template because the optional `right_side_panel` may contain supporting context.

If the generated page removes `right_side_panel` or leaves it without real business content, `main_work_queue_wrapper` must also remove the second column track. In that case, the Grid should serialize as a single `1fr` column for PC/laptop, tablet, and mobile. A generated Workbench page with only `primary_working_area` but a retained empty second desktop column is invalid because it creates a blank right-side area in Designer/runtime.

`primary_working_area` may contain selected modules copied from the template, including `kpi_metrics_wrapper`, `1_columns_section`, `2_columns_section`, `3_columns_section`, `2_columns_60/40_section`, and `chart_cards_section`.

`right_side_panel` may contain selected modules copied from the template, including `kpi_metrics_wrapper`, `1_row_section`, `2_rows_section`, `3_rows_section`, and `chart_cards_section`.

## Controlled Slots

Business content may be mapped only inside approved Workbench business slots:

- `page_title_content`
- `Operations`
- `section_content_area`
- `section_title_header`
- `dashboard_standard_filter_group`
- `kpi_cards_kpi_row`
- `kpi_card_wrapper`
- `primary_working_area`
- `right_side_panel`
- `chart_cards_section`

Every other container is structural and must preserve the template's exported style, layout, typography, padding, direction, gap, and background contract.

## Repeatable And Removable Modules

The following Workbench modules may be removed, duplicated, or reordered based on business requirements:

- `content_card_wrapper`
- `content_card_60_wrapper`
- `content_card_40_wrapper`
- `1_columns_section`
- `2_columns_section`
- `3_columns_section`
- `2_columns_60/40_section`
- `1_row_section`
- `2_rows_section`
- `3_rows_section`
- `kpi_metrics_wrapper`
- `kpi_cards_kpi_row`
- `kpi_card_wrapper`
- `chart_cards_section`

New layout must be added by copying one of these approved modules. Do not invent new layout containers.

## Chart Cards Section

`chart_cards_section` is the preferred host for chart-like Data Analytics templates on Workbench dashboards. It may appear under `primary_working_area` or `right_side_panel`.

Rules:

- Use `chart_cards_section` when multiple chart-like Data Analytics templates are displayed together.
- Place no more than three chart-like Data Analytics templates in a single `chart_cards_section`.
- Add another `chart_cards_section` when more than three analytics cards are needed.
- Remove `chart_cards_section` when no chart-like Data Analytics template is planned or materialized.
- `chart_cards_section` must not appear outside `primary_working_area` or `right_side_panel`.
- Do not place `data_analytics_pivot_table_standard` / `pivot_table_standard` inside `chart_cards_section`. Pivot tables are table-like regions and must be hosted in `content_card_wrapper > section_content_area`, preferably inside `1_columns_section`; use `2_columns_section`, `2_columns_60/40_section`, or `3_columns_section` only when the pivot has few columns.

Data Analytics templates must still clone the approved Data Analytics golden reference subtree and include runtime `ReportIds`/`exts` model registration before signing readiness.

## Right Side Panel

`right_side_panel` is optional. Keep it only when it contains real business content such as context cards, alerts, analytics, related records, or actions.

If the entire `right_side_panel` has no planned business content, remove it from the generated page. An empty right-side panel is a generated-final error.

## Section Cleanup

Generated Workbench dashboards must not retain empty copied sections. Remove any empty:

- `section_content_area`
- `content_card_wrapper`
- `content_card_60_wrapper`
- `content_card_40_wrapper`
- `1_columns_section`
- `2_columns_section`
- `3_columns_section`
- `2_columns_60/40_section`
- `1_row_section`
- `2_rows_section`
- `3_rows_section`
- `kpi_metrics_wrapper`
- `chart_cards_section`

Template-source empty slots are allowed only in the registry template. They are not allowed in generated-final resources.

## KPI Cards

Workbench KPI cards use the generic `kpi_card_wrapper` from the Workbench template. KPI cards are demand-driven:

- If no KPI metrics are planned, remove KPI containers.
- If one to three KPI cards are planned, include exactly that number.
- If more KPI cards are needed, duplicate `kpi_card_wrapper` and map the business metric.
- Do not keep source KPI examples only because the template contains them.

Live KPI values require the same Summary-backed runtime contract as other Dashboard KPI cards.

## Collection And Data Analytics Components

Approved Dashboard Collection templates may be placed inside Workbench business content slots such as a `section_content_area` in the primary working area or right-side panel. Clone the selected Collection template exactly and map only approved editable regions.

Every `section_content_area` copied from the Workbench Dashboard golden reference must preserve `attrs.style.gap = [null, "--sp--s200"]`. Do not keep or regenerate the legacy `--sp--s0` gap for Workbench content slots.

Approved chart-like Data Analytics templates may be placed inside Workbench `chart_cards_section` when the page design requires grouped chart cards. Pivot table templates must be placed in a normal content-card section, not in `chart_cards_section`.

## Validation

Generated Workbench dashboards must pass:

- `scripts/validate-dashboard-page-layout-template.mjs`
- `scripts/test-dashboard-workbench-page-layout-template-gates.mjs`
- aggregate Dashboard hard gates before signing readiness

Do not sign or install a package whose generated Workbench page leaves empty section shells, empty chart sections, empty right-side panels, unresolved template placeholders, or template-provenance-only analytics.

---

<a id="dashboard-runtime-binding-standard"></a>

<!-- source: docs/standards/collection-01.md#dashboard-runtime-binding-standard -->

# Dashboard Runtime Binding Standard

Dashboard controls must be runtime-bound before a generated app is described as ready. A visually complete dashboard is not enough.

## Summary Controls

Every generated Summary control must be generated as a report-backed data control:

- the Summary control `id` is a UUID; semantic/layout-derived IDs such as `<LayoutID>_summary` are not runtime-proven
- `attrs.data.list` resolves to an included source list
- `attrs.field` or the export-proven equivalent resolves to a source field
- a matching layout-resource `Resource.exts[]` entry exists
- the matching extension uses `category: "___Pivot___"`
- the matching extension uses `key: "summary"`
- `exts[].i` equals the Summary control id
- `exts[].id` also equals the Summary control id
- `exts[].attr` includes `AppID`, `ListSetID`, `ListID`, `list`, and `source` for the current app and target list
- `exts[].attr.settings.values[]` includes a resolvable aggregate field, field type, and aggregate function such as `COUNT`, `SUM`, `AVG`, `MIN`, or `MAX`
- dashboard layout-resource `Resource.ReportIds[]` includes the Summary control id

The generator must not substitute static text, static numbers, or visual-only KPI cards for Summary controls when the app plan calls for calculated metrics. `runtimeModelProven: true` is allowed only when the Summary uses this UUID runtime shape and complete runtime metadata; it is not proof by itself.

## Filters

Every generated dashboard filter control must map to a stable page-level filter variable.

Recommended filter variable naming:

```text
filter_{PageName}_{FilterName}
```

Every declared `filterVars[]` entry must be consumed by at least one downstream control:

- Summary
- Collection
- Data table
- chart
- pivot

Consumer wiring:

- Summary controls consume filters through `exts[].attr.settings.Conditions[]`
- Collection and Data table controls consume exact filters through `attrs.data.filter[]`
- Collection and Data table controls consume keyword/search filters through `attrs.data.fulltext[]`
- chart and pivot consumers use their export-proven condition/filter structures

Filter producer controls do not count as consumers. A `search-filter`, `select-filter`,
or other Data Filter control with `binding = "__filter_<name>"` only declares/produces
the variable value. The generated page must either wire that variable into at least
one Summary, Collection, Data table, chart, or pivot consumer, or remove the unused
binding and omit the `filterVars[]` declaration. Generated-final packages must not
retain template-copied `filterVars[]` entries that are only referenced by filter
producer controls.

Every consumer condition must reference a valid field on the consumer source list. For lookup-backed fields, compare record ids/ListDataID-style values, not display labels.

## Validation Gates

Generated-final packages should fail on:

- `DASHBOARD_SUMMARY_MISSING_DATA_LIST`
- `DASHBOARD_SUMMARY_MISSING_FIELD`
- `DASHBOARD_SUMMARY_MISSING_EXT`
- `DASHBOARD_SUMMARY_EXT_MISMATCH`
- `DASHBOARD_SUMMARY_MISSING_VALUES`
- `DASHBOARD_SUMMARY_MISSING_REPORT_ID`
- `SUMMARY_CONTROL_ID_NOT_RUNTIME_SAFE`
- `SUMMARY_RUNTIME_MODEL_PROVEN_UNSUPPORTED_SHAPE`
- `SUMMARY_EXTS_SOURCE_METADATA_INCOMPLETE`
- `DASHBOARD_FILTER_VAR_DECLARED_NOT_CONSUMED`
- `DASHBOARD_FILTER_CONTROL_WITHOUT_FILTER_VAR`
- `DASHBOARD_FILTER_CONSUMER_INVALID_FIELD`
- `DASHBOARD_LOOKUP_FILTER_VALUE_NOT_RECORD_ID`
- `DASHBOARD_VISUAL_ONLY_KPI_CARD`

Use `scripts/validate-runtime-binding-lessons.mjs` as the focused reusable gate for these lessons. Its output is redacted and reports page name, control id, control title, source list, source field, filter variable, consumer controls, severity, and recommended fix where applicable.
