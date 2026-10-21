# docs collection 1

This file combines 10 documents from `docs/` for the Claude directory build. A reference to `docs/<name>.md` is the section that starts at the anchor `#<name>` below.

- [ai-training-approval-form-control-study.md](#ai-training-approval-form-control-study)
- [app-plan-generation-contract.md](#app-plan-generation-contract)
- [app-plan-standard-template.md](#app-plan-standard-template)
- [apply-ywf-metadata.md](#apply-ywf-metadata)
- [approval-form-and-yap-field-type-pattern-study.md](#approval-form-and-yap-field-type-pattern-study)
- [approval-form-control-runtime-coverage-matrix.md](#approval-form-control-runtime-coverage-matrix)
- [approval-form-generator-operating-playbook.md](#approval-form-generator-operating-playbook)
- [approval-form-requirement-decomposition-template.md](#approval-form-requirement-decomposition-template)
- [build-ydl-wrapper.md](#build-ydl-wrapper)
- [build-ywf-wrapper.md](#build-ywf-wrapper)

---

<a id="ai-training-approval-form-control-study"></a>

<!-- source: docs/collection-01.md#ai-training-approval-form-control-study -->

# AI Training Approval Form Control Study

Source studied: `<downloads>/AI Training.yap`

Study scope: approval form only. The original export was decoded read-only; no app generation, import, or UI operation was performed.

Derived inspection artifacts were written locally and are ignored by Git because they may contain export payload detail:

- `ai-training-decoded-resource.json`
- `ai-training-decoded-data.json`
- `ai-training-approval-form-def.json`
- `ai-training-approval-form-control-inspection.json`
- `ai-training-approval-form-control-inspection.md`

The reusable inspector is tracked as `scripts/inspect-yap-approval-controls.mjs`.

## Export Summary

- Wrapper title: `AI Training`
- Root app/listset ID: preserved as string
- Approval form found: `Approval Form`
- Form key: `AI_Training_Approval`
- `Data.Forms[].ListID`: `0`
- Pages: one submission page, title `Approval Form`
- Variables: 38 `variables.basic` entries plus 3 `variables.listref` entries
- Controls inspected: 77 top-level/nested controls in the page tree
- Large numeric IDs preserved as strings during decode: 2

The export contains one child data list named `Customer Leads`. The list was inspected only to understand lookup/data-list control references.

## Control Inventory

| Control type | Count | Variable type(s) observed | Notes |
| --- | ---: | --- | --- |
| `input` | 2 | `text` | Standard text entry; one nested list-field instance. |
| `textarea` | 1 | `text` | Uses `attrs.edit.fhlay = "auto"` and `attrs.edit.textarea_minrows = 2`. |
| `richtext` | 1 | `text` | Uses `attrs.edit.fh = [null, 150]`; runtime-sensitive for generation. |
| `radio` | 2 | `text` | Radio button and dropdown are same control with different `attrs.displayStyle`. |
| `checkbox` | 2 | `text` | Multi-choice checkbox/dropdown with `attrs.choices` and `attrs.color_choices`; runtime-sensitive until generated proof. |
| `switch` | 3 | `boolean`, nested list `text` | Normal switches bind boolean variables. `attrs.displayStyle = "decision"` produces tick-style switch. |
| `input_number` | 1 | `number` | Uses `attrs.displayThousandths = "1"`. |
| `percent` | 1 | `number` | Uses number variable; no extra attrs in this sample. |
| `currency` | 1 | `number` | Uses `attrs.currencyCode`, `attrs.displayFormat`, and `attrs.displayThousandths`. |
| `datepicker` | 3 | `date`, nested list `text` | Date-only uses `attrs.date_type = "0"`; date-time adds `attrs.showtime = true`. |
| `time` | 1 | `date` | Uses date variable with time value shape. |
| `daterange` | 2 | `date` | Main binding is the From variable; To variable is in `attrs["binding-date-range"]`. |
| `file-upload` | 1 | `file` | Uses `attrs.ver = 1`, `attrs.file_multiple = true`, `attrs.file_maxcount = 10`. |
| `icon-upload` | 2 | `img`, nested list `number` | Top-level image uses `attrs.controlmultiple = true`, `attrs.maxselection = 10`. |
| `identity-picker` | 3 | `user`, nested list `number` | Multiple user picker uses `attrs.multiple = true`, `attrs["identity-maxselection"] = 20`. |
| `organization-picker` | 2 | `groupselect` | Department picker uses `attrs["metadata-treeselect"] = true`; multiple adds `attrs.multiple = true`. |
| `location-picker` | 1 | `location` | Minimal attrs; environment-dependent. |
| `cost-center-picker` | 2 | `costcenter` | Single/multiple controlled by `attrs.multiple`. |
| `metadata` | 2 | `metadata` | Uses `attrs.source`, `attrs.categoryId`, and `attrs["metadata-treeselect"] = true`. |
| `mutiple-metadata` | 1 | `mutiple-metadata` | Same category/source pattern as metadata, multiple-specific control type. |
| `lookup` | 3 | `lookup` | Current-app dropdown, current-app button, and external dropdown patterns. |
| `lookup-list` | 1 | `lookup` | Multi-select lookup-list with `attrs.addition[]`; treat as runtime-sensitive. |
| `list` | 3 | `list` | Traditional, lookup-derived, and dynamic sublist patterns. |
| `data-list` | 1 | display only | Displays `Customer Leads` fields with caption/search/add settings. |
| `heading` | 5 | display only | Section text labels inside dynamic sublist area. |
| `aktabs` / `ak-tabs-tab` | 1 / 4 | layout | Four tabs group basic, system, data, and sublist examples. |
| `flex_grid` | 10 | layout | Used heavily for grouping controls. |
| `workflowControlPanel` / `workflowHistory` | 1 / 1 | workflow UI | Present on the submission page. |

## Variable Binding Model

Observed primary model:

```text
control.binding -> variables.basic[].id
```

Examples:

| Control | Variable type | Key attrs |
| --- | --- | --- |
| `input` | `text` | no required attrs for the simple single-line case |
| `textarea` | `text` | `attrs.edit.fhlay`, `attrs.edit.textarea_minrows` |
| `richtext` | `text` | `attrs.edit.fh` |
| `radio` | `text` | `attrs.choices`, `attrs.show_color`; dropdown adds `attrs.displayStyle = "dropdown"` |
| `checkbox` | `text` | `attrs.choices`, `attrs.color_choices`, `attrs.displayStyle` |
| `switch` | `boolean` | optional `attrs.comment-allow = false`; tick style uses `attrs.displayStyle = "decision"` |
| `input_number` | `number` | `attrs.displayThousandths = "1"` |
| `percent` | `number` | no extra attrs observed |
| `currency` | `number` | `attrs.currencyCode`, `attrs.displayFormat`, `attrs.displayThousandths` |
| `datepicker` | `date` | `attrs.date_type = "0"`; date-time adds `attrs.showtime = true` |
| `time` | `date` | `attrs.required = false` in this sample |
| `daterange` | `date` | `binding` is From; To is `attrs["binding-date-range"]` |
| `file-upload` | `file` | `attrs.ver`, `attrs.file_multiple`, `attrs.file_maxcount`, `attrs.upload_btn` |
| `icon-upload` | `img` | `attrs.controlmultiple`, `attrs.maxselection` |
| `identity-picker` | `user` | multiple uses `attrs.multiple`, `attrs["identity-maxselection"]` |
| `organization-picker` | `groupselect` | `attrs["metadata-treeselect"]`, optional `attrs.multiple` |
| `location-picker` | `location` | minimal attrs; requires environment proof |
| `metadata` | `metadata` | `attrs.source`, `attrs.categoryId`, `attrs["metadata-treeselect"]` |
| `cost-center-picker` | `costcenter` | `attrs.multiple` |
| `lookup` | `lookup` | `attrs.appid`, `attrs.listsetid`, `attrs.listid`, `attrs.listfield`, filters/search/display attrs |
| `list` | `list` | `variables.basic[].value` references `variables.listref[].id` |

## Layout Patterns

The form uses `aktabs` with four `ak-tabs-tab` children:

- `Tab 1 - Basic Fields`
- `Tab 2 - System Related`
- `Tab 3 - Data Related`
- `Tab 4 - Sublists`

Inside tabs, `flex_grid` controls group related controls. A common exported grid shape uses:

```json
{
  "attrs": {
    "ver": 1,
    "columns": {
      "1": { "list": [{ "value": 1, "unit": "fr" }], "last": { "value": 1, "unit": "fr" } },
      "2": { "list": [{ "value": 1, "unit": "fr" }, { "value": 1, "unit": "fr" }], "last": { "value": 1, "unit": "fr" } },
      "3": { "list": [{ "value": 1, "unit": "fr" }], "last": { "value": 1, "unit": "fr" } }
    },
    "rows": {
      "1": { "list": [{ "unit": "auto" }], "last": { "unit": "auto" } }
    },
    "cgap": { "1": 10 },
    "cgapU": { "1": "px" }
  }
}
```

This confirms that real exports can store `flex_grid.attrs.columns` and `attrs.rows` as responsive keyed objects. Current validators may warn because the normalized schema reference expects arrays. Do not convert this into a generator hard error.

Generation rule: for generated form field grids, continue using the design-system default two-column field sections with hidden grid captions. Treat this sample as evidence for control grouping, not as a replacement for the current UI quality standard.

## Lookup Patterns

Current-app lookup dropdown:

- `attrs.appid = 41`
- `attrs.listsetid` points to the app/root listset
- `attrs.listid` points to `Customer Leads`
- `attrs.listfield = "Title"`
- `attrs["sort-first"] = { "SortName": "Created", "SortByDesc": true }`
- `attrs["search-scope"] = "3"`
- `attrs["search-fields"] = ["Title", "Text1"]`
- `attrs.displayStyle = "dropdown"`

Current-app lookup button adds:

- `attrs.displayStyle = "button"`
- `attrs.btn_text = { "value": "Choose One", "variable": null }`

External lookup dropdown uses external `ListSetID`/`ListID`, `attrs.link = "none"`, `attrs["search-scope"] = "1"`, and `attrs.listfilter[]`.

Generator rule: local lookup controls can use this as an export-backed shape only when the source list and display/search fields resolve. External lookup IDs must not be included in generated `ReplaceIds`.

## Lookup-List And Sublist Patterns

`lookup-list` uses:

- `attrs.multiple = true`
- source list metadata fields
- `attrs.addition[]` where shown fields have `IsShow = true`, `RelationName`, and `Order`

The sublist controls use:

- `control.type = "list"`
- `control.binding` to a `variables.basic` entry of `type = "list"`
- that variable's `value` points to a `variables.listref` entry
- sublist field controls include `attrs.list_field = true`, `attrs.list_field_binding`, and `attrs.list_control_id`

Generator rule: sublists and lookup-list are now better documented, but remain runtime-sensitive. Use the sample for inspection and validation warnings. Generate only after a focused minimal package proves the exact shape.

## Runtime-Sensitive Controls

The following are export-backed from this sample but should remain warning-first for generation:

- `richtext`
- `checkbox`
- `percent`
- `file-upload`
- `icon-upload`
- `identity-picker`
- `organization-picker`
- `location-picker`
- `cost-center-picker`
- `metadata`
- `mutiple-metadata`
- `lookup-list`
- `list`
- `data-list`
- `aktabs`
- `ak-tabs-tab`
- `action_button`

Safe generated defaults remain the previously proven smaller set unless the user explicitly asks to test these controls in staged packages.

## Validator Notes

`validate-ywf-def.js ai-training-approval-form-def.json --mode draft` currently fails because this exported sample is not a final generated approval workflow:

- it has no type `2` approval/task page
- lookup-list `attrs.addition[]` uses relation names from a listref-style projection rather than direct `variables.basic` targets
- several controls are schema-supported but runtime-unproven for generated packages

These are useful findings, not reasons to mutate the export. Validator policy should stay:

- keep missing task page as an error for final generated approval forms
- treat the lookup-list relation-name shape as a documented recommendation until a generated lookup-list package proves it
- keep runtime-sensitive controls as warnings or stop conditions for production-like generation
- update the normalized control schema later if `flex_grid.attrs.rows`, `ak-tabs-tab.label_var`, and list internals are confirmed by a smaller generated/runtime test

## Generator Rules Learned

1. For simple value controls, generate `binding` values that match `variables.basic[].id`.
2. Keep variable type compatible with the control:
   - text: `input`, `textarea`, `richtext`, `radio`, `checkbox`
   - number: `input_number`, `percent`, `currency`
   - boolean: `switch`
   - date: `datepicker`, `daterange`, `time`
   - file/img/user/groupselect/location/costcenter/metadata/lookup/list for corresponding native advanced controls.
3. Use `radio` for both button-style single choice and dropdown-style single choice; dropdown is `attrs.displayStyle = "dropdown"`.
4. Use `checkbox` for multi-choice; dropdown multi-choice is `attrs.displayStyle = "dropdown"`.
5. For `daterange`, create two date variables. Bind the control to the From variable and set the To variable in `attrs["binding-date-range"]`.
6. For `currency`, use number variable plus `attrs.currencyCode`, `attrs.displayFormat`, and `attrs.displayThousandths`.
7. For `file-upload`, include `attrs.ver = 1`, `attrs.file_multiple`, `attrs.file_maxcount`, and `attrs.upload_btn`.
8. For `identity-picker`, multiple selection uses `attrs.multiple = true` and `attrs["identity-maxselection"]`.
9. For `organization-picker`, use `attrs["metadata-treeselect"] = true`; multiple selection adds `attrs.multiple = true`.
10. For `metadata` and `mutiple-metadata`, include `attrs.source`, `attrs.categoryId`, and `attrs["metadata-treeselect"] = true`; do not guess category IDs.
11. For `cost-center-picker`, set `attrs.multiple` explicitly.
12. For lookup controls, require resolved `appid`, `listsetid`, `listid`, `listfield`, sort/search/link/modal metadata.
13. For sublists, generate `variables.listref` and list-field child controls together; do not hand-roll a list control without a matching listref.

## Stop Conditions For Future Generation

Stop before generating these patterns when:

- lookup source list, display field, or search fields do not resolve
- metadata category IDs are unknown
- department/location/cost-center source dependencies are not available
- file/image upload behavior must persist to a data list but no export-backed target field pattern exists
- sublist row fields or listref structure cannot be generated consistently
- validator errors would need to be ignored rather than understood

---

<a id="app-plan-generation-contract"></a>

<!-- source: docs/collection-01.md#app-plan-generation-contract -->

# App Plan Generation Contract

Every generated Yeeflow application plan must follow the canonical `docs/standards/collection-01.md#app-plan-standard-template` unless the user explicitly requests a lightweight plan. `docs/collection-01.md#app-plan-standard-template` is a compatibility entrypoint only. A lightweight plan is allowed only when the user explicitly asks for a quick outline. Lightweight plans must still include Data Model and Lists, Forms and Approval Forms, Application Navigation, UI/UX and Control Mapping, Generation Contract and Hard Gates, Proof Boundary, and Assumptions/Deferred Items.

`Generation Contract and Hard Gates` is binding for later YAPK/YAP generation and validation; it is not background guidance.

Before design images, page implementation blueprints, resource generation, decoded resource-vs-blueprint parity, signing, install/upgrade, or runtime proof, the planning gates are executable:

- `scripts/validate-business-clarification-gate.mjs --spec <functional-spec.md> --plan <app-plan.md>`
- `scripts/validate-generation-readiness-review.mjs --plan <app-plan.md>`
- `scripts/validate-functional-spec-to-app-plan-traceability.mjs --spec <functional-spec.md> --plan <app-plan.md>`

These validators prove planning readiness and traceability only. They do not prove package schema validity, package conformance, signing/API acceptance, install/upgrade success, or runtime behavior.

The App Plan must explicitly plan how Data List records are displayed on pages before implementation begins. Each record-list section must choose Data table, Collection, Kanban, Vertical timeline, or Horizontal timeline and state the reason. Collection, Kanban, Vertical Timeline, and Horizontal Timeline controls must plan item-template Dynamic controls with source fields. Collection and Kanban controls must plan item actions or explicitly state `No Collection/Kanban item actions required`. Approval Forms and Custom Data List forms that use Sub List controls must plan Sub List list actions or explicitly state `No custom Sub List actions required`.

All App Plan resource types, field types, variable types, controls, Dynamic controls, workflow nodes, form actions, Collection/Kanban actions, Sub List actions, property paths, bindings, and configuration shapes must come from the active plugin's known skills, standards, validators, template library, control/property knowledge base, extension registry, or export-proven references. Unknown shapes must be marked `export-learning-required`, `runtime-proof-required`, or `deferred` and must not be treated as generation-ready.

Separate business-friendly labels from exact Yeeflow implementation wording. A plan may use `Owner/person` as a business label only if the same row has dedicated implementation columns such as `Exact Yeeflow Field Type` and `Exact Yeeflow Control Type` with plugin-supported values like `User` and a supported user picker control, plus support/proof metadata. Do not combine exact implementation columns as `Exact Yeeflow Field Type / Control Type`, `Exact Yeeflow Variable Type / Control Type`, or similar. Slash-combined or vague implementation wording such as `Title/Text`, `Title / input control`, `Currency/Number`, `User/person`, `Attachment/File upload`, `Document library / Data list`, `Type 1/document library`, `Lookup/read-only dynamic field`, `Document/list section`, `Open detail/slide panel where supported`, or `Update row/status where supported` is not generation-ready unless the row is marked `runtime-proof-required`, `export-learning-required`, or `deferred` with a fallback/deferred reason.

Document Library planning must select one clear Yeeflow resource type and record the package/navigation representation when known. If Document Library representation is not confirmed, mark it `export-learning-required` or `runtime-proof-required` and do not treat that item as generation-ready.

Use this section in addition to the normal purpose, roles, process, lists/fields, list views, custom list forms, list workflows, scheduled workflows, notifications, forms, dashboards/pages, navigation, UI/UX mapping, custom code/custom CSS decisions, AI Agent/Copilot decisions, golden references, permissions, integrations, assumptions, deferred items, and proof boundary sections.

Northpeak reference note: `northpeak-resource-operations-plan.md` is the style reference for a complete plan because it uses numbered sections, plan status, target roles, process flow, navigation, detailed lists/fields, forms, dashboards/pages, UI/control mapping, golden template strategy, composition checklist, actions/workflow logic, permissions, integrations, document/attachment decisions, reports, validation, proof boundary, assumptions, deferred/runtime-proof items, and a recommended next prompt. Reuse that structure, not its business-specific content.

## Template

```md
## Generation Contract and Hard Gates

### Output Package
- Default output: `.yapk`
- `.yap` only if explicitly requested
- Package generation expected in this task: Yes/No
- Planning-only task: Yes/No
- Package handoff requires API signing when credentials are available: Yes/No

### YAPK Signing Gate
- Required when generating `.yapk` and OAuth/API-key access is available: Yes/No
- Required endpoints: `POST /utils/apppackage/setsign`, `POST /utils/apppackage/verifysign`
- Placeholder `Sign` may be described as upload-ready: No
- Local schema validation alone proves upload readiness: No
- Signing/install acceptance proves ID provenance or navigation runtime metadata completeness: No

### API-Issued Content ID Provenance Gate
- Required for generated-final `.yapk`: Yes
- Required API: `GET /utils/generate/ids?count=<n>`
- Local sequential counters, hardcoded generated IDs, copied sample/export IDs, random values, timestamps, UUID fallback, and deterministic local-only seeds allowed: No
- Required proof artifact: `dist/<app-name>-id-provenance-report.json`
- Source marker required: `api-generated`
- Non-API numeric generated content IDs allowed: No
- Generation must stop before signing, install, upgrade-check, or handoff when provenance validation fails: Yes

### YAPK Upgrade ID Stability Gate
- Required for generated-final `.yapk` upgrade/new-version output: Yes/No
- Previous package path:
- Previous ID lineage/provenance manifest:
- New ID lineage manifest:
- Existing resource IDs preserved for dashboards, approval forms, data lists, fields, layouts, workflows, user groups, AI Agents, Copilots, navigation, and persisted relationship IDs: Yes/No
- Only newly added resources receive new API-issued IDs: Yes/No
- Removed IDs reused for different objects: No
- Missing previous package/manifest may still be called upgrade-safe: No
- Generation must stop before signing, upgrade-check, upgrade, install-like writes, or handoff when ID stability validation fails: Yes

### Approval Form Contract
- Approval required: Yes/No
- Approval form:
- Request page:
- Task pages:
- Approval statuses:
- Workflow steps:
- Assignment task assignee plan:
  - Task name:
  - Assignment type: line manager / department manager / location manager / job position / explicit user / requester/submitter / other supported expression
  - Required job position name, if applicable:
  - Source: discovered existing job position / user-selected existing job position / admin-created after confirmation / unresolved / N/A
  - Proof status:
  - Fallback or blocker:
- Workflow control panel required: Yes/No
- Workflow history required: Yes/No
- DefResource required: Yes/No
- If deferred, user-approved staged build required: Yes/No
- Job-position assignments may use invented IDs or names: No
- Missing job positions block generation until resolved by discovery, user selection, or explicitly confirmed admin creation/update: Yes
- Job-position writes require explicit confirmation and confirmed system-admin permission: Yes
- Manager-based assignees must use supported expression-editor patterns and validator-backed shapes: Yes

### Navigation Runtime Contract
- Navigation Runtime Metadata Gate required for generated-final `.yapk`: Yes
- Group shape: `ID`, `AppID`, `ListSetID`, `Type: "classes"`, `Title`, `Icon`, and `list`
- Group `ID` must be API-issued and present in the ID provenance manifest
- Group `AppID` must equal the package/root AppID
- Group `ListSetID` must equal the current root `ListSet.ListID`
- Do not use `children`/`Childs` for runtime grouped navigation
- Every child item must include `AppID`, `Title`, `ListID`, `ListSetID`, and `Type`
- Page/dashboard entries: `Type: 103`, include `LayoutID`, and use `ListID = LayoutID`
- Approval form entries: `Type: 105`, `ListID = Forms[].Key`
- Data list entries: `Type: 1`, `ListID = Childs[].List.ListID`
- Unreachable resources allowed only if documented hidden/deferred
- Generation must stop before signing, install, upgrade-check, or handoff when navigation runtime metadata validation fails: Yes

### Dashboard Grid-Table Collection Pattern Contract
- Required when a dashboard record-list section claims the grid-table Collection pattern: Yes
- Dashboard record-list control choice stated for each section: Data table / Collection / Kanban / Vertical timeline / Horizontal timeline / none
- Record Display Control Selection reason stated for each Data List record section: Yes/No
- Use dashboard `data-list` control for grid-table dashboard sections: No, unless Data table is explicitly requested
- Item-template Dynamic controls planned for every Collection, Kanban, Vertical Timeline, or Horizontal Timeline: Yes/No
- Collection/Kanban item actions planned or explicitly marked `No Collection/Kanban item actions required`: Yes/No
- Header `flex_grid` paired with each grid-table Collection: Yes/No
- Header `flex_grid` and Collection wrapped in one container: Yes/No
- Wrapper sets `attrs.container.gap = 0`: Yes/No
- Wrapper sets `attrs.style.gap = [null, 0]`: Yes/No
- Row-click detail behavior planned: Yes/No
- If row-click detail is planned, Collection includes `attrs.data.link`, `attrs.data.opentype = "slide"`, and `attrs.data.modalsize = 2`: Yes/No
- If row-click detail is planned, each source data list has a concrete Type `1` custom detail layout: Yes/No
- Duplicate dashboard header hidden with `attrs.hideHeaderAll = true` when app shell/navigation provides context: Yes/No/Not applicable
- Dashboard title uses visible title typography such as `attrs.heads.ty = [null, "h5-medium"]`: Yes/No
- Text controls include width/positioning where needed, token typography, and plain-string color or validated color token: Yes/No
- Internal helper metadata enumerable in encoded package objects: No
- Type `1` custom detail layout `LayoutView = null` allowed: No
- Generation must stop before signing, install, upgrade-check, or handoff when planned dashboard grid-table Collection validation fails: Yes

### Plan-to-Package Conformance Contract
- Planned lists must exist: Yes/No
- Planned fields must exist: Yes/No
- Planned forms must exist: Yes/No
- Planned approval form must exist when required: Yes/No
- Planned task pages must exist: Yes/No
- Planned dashboards/pages must exist: Yes/No
- Planned navigation groups must exist: Yes/No
- Planned print pages must be reachable or intentionally hidden: Yes/No
- Planned workflows/actions must be implemented or explicitly deferred: Yes/No
- Planned record display controls, item-template Dynamic controls, Collection/Kanban actions, and Sub List list actions must be implemented or explicitly deferred: Yes/No
- Planned data-list views, custom list forms, public forms, and notifications must be implemented or explicitly deferred: Yes/No
- Planned scheduled workflows must be implemented or explicitly deferred: Yes/No
- Planned AI Agents, Copilots, knowledge resources, custom code, custom CSS, and golden/template references must be implemented or explicitly deferred: Yes/No
- Planned permissions/integrations must be implemented or explicitly deferred: Yes/No

### Advanced Capability Contract
- Custom code/control required: Yes/No/Deferred
- Custom CSS required: Yes/No/Deferred
- Data-list workflows required: Yes/No/Deferred
- Scheduled workflows required: Yes/No/Deferred
- Notifications required: Yes/No/Deferred
- AI Agent/Copilot/knowledge required: Yes/No/Deferred
- Golden/template references named for each advanced feature: Yes/No
- Plugin-supported type/property/action rule satisfied for controls, Dynamic controls, action types, property paths, bindings, and configuration shapes: Yes/No
- Runtime proof required before claiming execution/delivery works: Yes/No

### Proof Boundary Contract
- App plan approval status:
- Local schema validation status:
- ID provenance proof status:
- Navigation runtime metadata proof status:
- Dashboard grid-table Collection validation status:
- Wrapper gap validation status:
- Detail layout link validation status:
- Dashboard header visibility validation status:
- Dashboard title/text style validation status:
- Schema helper-leak validation status:
- App-plan conformance status:
- UI/control quality validation status:
- Data-list views/forms/workflows/notifications validation status:
- Scheduled workflow validation status:
- AI Agent/Copilot validation status:
- Custom code/custom CSS validation status:
- Golden/template conformance status:
- API signing status:
- API signature verification status:
- API install/import acceptance status:
- Application access link status after successful install/import:
  - Selected workspace: name/category/redacted ID preview
  - Installed/imported ListSetID: safe value or unavailable
  - Link: `<tenant-url>/#/list-set/41/<listset-id>` only when tenant URL comes from OAuth/session context and ListSetID is safely resolved
  - Fallback: `Application link: unavailable; ListSetID or tenant URL was not safely resolved.`
  - Boundary: API install/import success is not browser runtime proof; open the link and verify navigation, dashboards, lists, forms, and workflows.
- Runtime UI inspection status:
- Runtime/designer visual proof status:
- Evidence: ID allocation manifest and validator results
- Boundary: signing and install acceptance do not prove ID provenance, navigation runtime metadata completeness, or dashboard runtime/designer visual fidelity
- Workflow/notification/AI/custom-code execution proof status:
- Workflow assignment runtime proof status:
  - Assignment task assignee plan present: Yes/No
  - Job positions discovered/confirmed/user-selected/admin-created after confirmation/unresolved:
  - Manager expressions validator-backed: Yes/No
  - Runtime/browser workflow routing verified: Yes/No
  - Boundary: Workflow assignment correctness is not proven until runtime/browser verification.
- Deferred items:
- Known risks:

### Runtime Inspection Checklist
- App opens:
- Navigation groups render:
- Approval form appears:
- Request page opens:
- Task pages exist:
- Lists open:
- Dashboards/pages render:
- Dashboard header hidden when planned:
- Dashboard title size/style matches plan:
- Grid-table header and Collection have no visible gap:
- Collection row click opens planned slide detail:
- Detail modal size matches plan:
- Navigation refresh still renders the dashboard:
- Print pages are reachable if intended:
- Generated app matches approved plan:
```

## Validation Expectation

Use `scripts/validate-app-plan-resource-order.mjs <plan.md>` to check required Markdown headings, Yeeflow resource generation order, Placeholder planning, Form Report separation, record display control selection, item-template Dynamic control planning, Collection/Kanban action decisions, Sub List action decisions, plugin-supported type/property rules, and hard-gate text before package generation. Use `scripts/validate-business-clarification-gate.mjs`, `scripts/validate-generation-readiness-review.mjs`, and `scripts/validate-functional-spec-to-app-plan-traceability.mjs` to enforce business decision closure, 13-area planning readiness, App Plan control/action/property decisions, and Functional Specification to App Plan traceability before package generation. Use `scripts/validate-app-plan-conformance.mjs` after package generation to compare the generated app against the approved plan. Generation reports must keep local validation, API signing, API install/import acceptance, and runtime UI inspection as separate proof levels.

Planning validation reports must distinguish `Generation Readiness structural check: pass/fail` from `Overall generation readiness: pass/blocked`. They must also report `Business Clarification Gate for planning: pass/fail` separately from `Business Clarification Gate for generation: pass/fail`. If defaults are only `default-applied-for-planning`, the report must say `Business Clarification Gate for generation: blocked until user approves defaults or answers gates` and `Overall generation readiness: blocked by Business Clarification Gate` even when the structural generation-readiness validator passes.

Business Clarification reports must distinguish raw findings from unique unresolved decisions. If the same gate appears in the Functional Specification and the Yeeflow App Plan, use `uniqueUnresolvedGateCount` and `uniqueUnresolvedGateKeys` for the user-facing count, while keeping raw findings and `gateOccurrences` for diagnostics.

## Custom Code Dashboard template selection

`dashboard-page-layouts-custom-code` is an explicit alternate Dashboard template. Preserve the Functional Specification requirement marker and use the Composition, Module Plan, and Communication and Native Integration tables from `docs/standards/collection-01.md#dashboard-page-layouts-custom-code-standard` for both App Plan and standalone Dashboard Plan. Materialize through the shared Custom Code page builder with populated control artifacts; no generic dataset/header fallback. Keep the existing Core issued-ID, source-binding and standalone wrapper gates.

## Collection content density

Follow `docs/standards/collection-02.md#native-collection-density-standard`. For native Table/Card Collections, plan field-aware widths after actual container placement, using native `cw/cwu`; limit narrative Dynamic fields through `t-len` only with a complete-value route. Collection selection rows accept optional `Column widths` (JSON map), `Text lengths` (JSON map) and `Full value access` (`source-list`, `record-detail`, `none`). The real materializer consumes these fields and rejects invalid overrides. Keep 10-record defaults, choice styles, scoped toolbar/menu behavior, sorting, source bindings and mobile card content intact. Pixel/percentage examples are mapping evidence, not universal layout defaults.

---

<a id="app-plan-standard-template"></a>

<!-- source: docs/collection-01.md#app-plan-standard-template -->

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

| Field Order | Business Label | Field Name | Field ID / Variable ID | Exact Yeeflow Variable Type | Exact Yeeflow Control Type | Support Source | Proof Label | Fallback / Deferred Reason | Binding | Read Only | Required | Default Value | Placeholder | Dynamic Display | Custom Validation | Lookup Target | Lookup Display Field | Additional Lookup Fields | Sublist/Summary Notes | Description |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <Business label> | <Field> | <ID> | <Exact variable type> | <Exact control type> | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Binding> | Yes/No | Yes/No | <Default> | <Placeholder text> | <Rule> | <Rule> | <List> | <Field> | <Fields> | <Notes> | <Description> |

Submission form rules:

- Variable and control types must follow active plugin Approval Form standards.
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

| Field Order | Business Label | Field Name | Field ID / Variable ID | Exact Yeeflow Variable Type | Exact Yeeflow Control Type | Support Source | Proof Label | Fallback / Deferred Reason | Binding | Read Only | Required | Default Value | Placeholder | Dynamic Display | Custom Validation | Lookup Target | Lookup Display Field | Additional Lookup Fields | Sublist/Summary Notes | Description |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | <Business label> | <Field> | <ID> | <Exact variable type> | <Exact control type> | <Plugin skill/doc/export reference> | validator-backed/runtime-proof-required/export-learning-required/deferred | <Reason or N/A> | <Binding> | Yes/No | Yes/No | <Default> | <Placeholder text> | <Rule> | <Rule> | <List> | <Field> | <Fields> | <Notes> | <Description> |

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
| <Parent list> | <Parent View Item form> | <Child list> | <Child lookup FieldName> | <Visible section title> | collection_control_grid_table | <Search fields or No> | <Add button label or No> | `<Child lookup FieldName> = current ListDataID` |

Rules:

- Generate reverse-related sections only for View Item or Workbench View Item forms, not New/Edit forms.
- Use this table when a child list has a lookup to the host list and users need to inspect or create those child records from the host record page.
- The Collection Template must be an approved grid-table Collection template for list-like child records.
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
| <Dashboard> | dashboard-page-layouts-v1.1 / dashboard-page-layouts-workbench / dashboard-page-layouts-two-panel-workspace / dashboard-page-layouts-three-panel-workspace / dashboard-print-multi-record-table-v1 | General overview / operational workbench / master-detail workspace / multi-record print page | <Sections/regions> | Yes/No | Yes/No | <Reason using template guidance> | Generated-final validation |

Rules:

- Select exactly one Dashboard page layout template per Dashboard page from `docs/reference/dashboard-page-layout-templates.json`.
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
| Dense audit table | <Data List or Form Report> | Dense operational audit-table review | Collection | collection_control_grid_table | <Audit fields> | Dense operational row/column scanning for audit-table review | <Open behavior or No row open> | <Local/runtime proof boundary> |
| Responsive task table | <Data List> | Native table on computer/laptop/tablet and cards on mobile | Collection | collection_control_responsive | <Title, owner, dates, progress, status> | Responsive operational table needs sortable native columns on larger devices and readable cards on mobile | <Open/edit behavior or No row open> | <Export/configuration proof; runtime breakpoint verification required> |

Rules:

- Allowed selected controls are Data table, Collection, Kanban, Vertical timeline, and Horizontal timeline.
- Prefer Collection over Data table when both can satisfy the requirement, unless a dense native table/grid is specifically required.
- When the selected record display control is Collection, the Selected Collection Presentation Reference must be one of `collection_control_responsive_card_grid`, `collection_control_responsive`, `collection_control_card_with_multiselect_toolbar`, `collection_control_grid_table`, `collection_control_grid_table_with_multiselect`, or `Event Pipeline Grid-Table` from `docs/reference/dashboard-dataset-presentation-golden-references.json`. Search/fulltext is planned as behavior inside the selected approved template, not as a separate Collection template ID.
- For Collection rows, state the required business fields and selection rationale. The rationale must use the selected template's `whenToUse`, `whenNotToUse`, `requiredBusinessSignals`, and `suitableSourceResourceTypes` guidance, such as card browsing, native desktop/tablet table plus mobile cards, dense row/column scanning, free-text search, multiselect/bulk operation, or high-fidelity primary operations table.
- The App Plan generator must write this rationale at generation time. It must not emit a row that only names `collection_control_grid_table`, `collection_control_responsive`, `collection_control_responsive_card_grid`, or another approved template ID without a matching business signal such as dense row/column scanning, native table plus mobile cards, card browsing, multiselect/bulk operation, work queue, record list, or primary operations table.
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

---

<a id="apply-ywf-metadata"></a>

<!-- source: docs/collection-01.md#apply-ywf-metadata -->

# Yeeflow Metadata Applicator

Apply filled sandbox metadata to a decoded Yeeflow approval form `Def` JSON draft.

This tool is intentionally limited to decoded JSON drafts. It does not import into Yeeflow, operate the UI, base64 encode `Def`, or create final `.ywf` files.

## Usage

```bash
node apply-ywf-metadata.js \
  ./travel-request-def.json \
  ./travel-request-sandbox-metadata.json \
  ./travel-request-def.sandbox.json
```

## Metadata Requirements

Each placeholder entry must include:

- `placeholder`: unresolved placeholder matching `__...REQUIRED...__`
- `requiredValue`: real sandbox value to apply
- `status`: must be `ready`
- `whereUsedByNode`: at least one semantic locator

Workflow node locators match by:

- `nodeName`
- `nodeStencil`
- `propertyPath`
- `expectedCurrentValue`

Before replacement, the script confirms `expectedCurrentValue` still matches the current source Def value. If the source draft has drifted, the script stops and reports the mismatch.

## Safety Checks

The script fails without writing output when:

- any metadata entry is still `status: "missing"`
- any `requiredValue` is empty
- any semantic locator cannot be found
- any locator's `expectedCurrentValue` does not match the source Def
- any placeholder exists in the source Def but is not covered by metadata
- unresolved `__...REQUIRED...__` placeholders remain after replacement

## Output

The script prints a structured JSON report:

```json
{
  "status": "pass | fail",
  "source": "./travel-request-def.json",
  "metadata": "./travel-request-sandbox-metadata.json",
  "output": "./travel-request-def.sandbox.json",
  "replacementsApplied": 0,
  "errors": [],
  "warnings": [],
  "remainingPlaceholders": []
}
```

After a successful metadata application, run decoded Def validation:

```bash
node validate-ywf-def.js ./travel-request-def.sandbox.json --mode final --dependency-map ./travel-request-dependencies.json
```

---

<a id="approval-form-and-yap-field-type-pattern-study"></a>

<!-- source: docs/collection-01.md#approval-form-and-yap-field-type-pattern-study -->

# Approval Form and App-Level Field Type Pattern Study

Source export:

- `/Users/rengerhu/Downloads/Visitor Access Management v10.yap`

Baseline compared:

- `visitor-access-management-app-def.v10-add-visitor-contact.json`
- `visitor-access-management.v10-add-visitor-contact.yap`

Scope:

- Read-only decode and comparison.
- No import.
- No UI operation.
- No new generated `.yap`.

## 1. Summary

The manually updated Visitor Access Management v10 export confirms several field/control patterns that were not yet proven in generated app-level `.yap` packages.

Confirmed approval-form controls:

- number: `input_number`
- single-select radio: `radio`
- single-select dropdown: `radio` with `attrs.displayStyle = "dropdown"`
- boolean switch: `switch`
- dynamic display rule: target control `attrs.control_display[]`

Confirmed data-list fields:

- number: `FieldType = "Decimal"`, `Type = "input_number"`, slot `Decimal1`
- switch: `FieldType = "Bit"`, `Type = "switch"`, slot `Bit1`

Important limitation:

- The manually added approval controls were added only to the request page.
- They were not copied to the approval/task page.
- `Number of Visitors` and `Requires Escort` were added to the data list, but not mapped in workflow `ContentList`.
- `Access Type (Radio)`, `Access Type (Dropdown)`, and `Escort User` were approval-form-only variables/controls in this export.

## 2. Decode and Compare

The updated exported app decoded successfully.

Manual export app identity:

| Item | Value |
| --- | --- |
| Root ListSetID | `2053834045676859392` |
| Departments ListID | `2053834050673455104` |
| Visitor Access Requests ListID | `2053834050677649417` |
| Approval form key | `VBA` |
| Approval form `ListID` | `0` |
| Approval form `ProcModelID` | `2053834050681843700` |
| ReplaceIds count | `43` |

Compared to generated v10:

| Area | Change |
| --- | --- |
| Root app/navigation | Same app structure; IDs remapped by import/export. |
| Data list fields | Added `Decimal1` and `Bit1` to Visitor Access Requests. |
| Data list views | `All Requests` includes `Decimal1` and `Bit1`; other views were mostly unchanged. |
| Data list custom form | No new controls for `Decimal1` or `Bit1` were found in the custom list form. |
| Approval variables | Added 5 variables: number, two text choices, boolean, text. |
| Request page | Added 5 controls after the existing text-field block. |
| Approval page | New controls were not present. |
| Workflow graph | Same nodes and sequence flows. |
| ContentList | No mappings added for the new variables/fields. |

## 3. Number Field Pattern

### Data List Side

`Number of Visitors` was added to the Visitor Access Requests list:

```json
{
  "FieldName": "Decimal1",
  "InternalName": "Number_of_Visitors",
  "DisplayName": "Number of Visitors",
  "FieldType": "Decimal",
  "Type": "input_number",
  "DefaultValue": null,
  "Rules": {
    "displayThousandths": "1",
    "rounded-to": "0",
    "number_min": 0
  }
}
```

Sample data raw values in this export:

| Row | Raw `Decimal1` |
| --- | --- |
| `VAR-1001` | `""` |
| `VAR-1002` | `""` |
| `VAR-1003` | `""` |

The export did not include number sample values for the list field.

### Approval Form Side

Workflow variable:

```json
{
  "id": "NumberofVisitors",
  "name": "Number of Visitors",
  "type": "number",
  "editable": true
}
```

Request page control:

```json
{
  "type": "input_number",
  "label": "Number of Visitors",
  "binding": "NumberofVisitors",
  "attrs": {
    "placeholder": "Please identify the number of visitors",
    "displayThousandths": "1",
    "rounded-to": "0",
    "number_min": 0
  },
  "value": 1
}
```

Observed meaning:

- `attrs.displayThousandths = "1"` enables thousands display formatting.
- `attrs["rounded-to"] = "0"` stores whole-number formatting.
- `attrs.number_min = 0` stores minimum value.
- Default value is stored on the control as `value: 1`, not in the variable definition.

### Reusable Generation Pattern

Approval form number field:

```json
{
  "idx": "<uuid>",
  "id": "NumberofVisitors",
  "name": "Number of Visitors",
  "type": "number",
  "editable": true
}
```

```json
{
  "id": "<uuid>",
  "type": "input_number",
  "label": "Number of Visitors",
  "binding": "NumberofVisitors",
  "attrs": {
    "placeholder": "Please identify the number of visitors",
    "displayThousandths": "1",
    "rounded-to": "0",
    "number_min": 0
  },
  "value": 1
}
```

Data list number field:

```json
{
  "FieldName": "Decimal1",
  "FieldType": "Decimal",
  "Type": "input_number",
  "Rules": "{\"displayThousandths\":\"1\",\"rounded-to\":\"0\",\"number_min\":0}"
}
```

ContentList mapping, if persistence is required:

```json
{
  "Per": "0",
  "Columns": "Decimal1",
  "Data": "<input type=\"button\" data=\"${&quot;type&quot;:&quot;variable&quot;, &quot;param&quot;:{&quot;id&quot;:&quot;NumberofVisitors&quot;}}\" expr=\"__\" tabindex=\"-1\" value=\"Workflow Variables:Number of Visitors\">"
}
```

## 4. Single Select Radio Pattern

Workflow variable:

```json
{
  "id": "AccessType1",
  "name": "Access Type (Single Select with Radio Button)",
  "type": "text",
  "editable": true
}
```

Request page control:

```json
{
  "type": "radio",
  "label": "Access Type (Single Select with Radio Button)",
  "binding": "AccessType1",
  "attrs": {
    "choices": ["Choice1", "Choice2", "Choice3"],
    "show_color": false
  }
}
```

Observed meaning:

- Single select uses a text variable.
- Radio-button layout uses control `type = "radio"` without `attrs.displayStyle = "dropdown"`.
- Values are stored as the selected choice string.
- `attrs.choices[]` is a plain string array.

Reusable pattern:

```json
{
  "idx": "<uuid>",
  "id": "AccessType",
  "name": "Access Type",
  "type": "text",
  "editable": true
}
```

```json
{
  "id": "<uuid>",
  "type": "radio",
  "label": "Access Type",
  "binding": "AccessType",
  "attrs": {
    "choices": ["General Visit", "Interview", "Vendor Service"],
    "show_color": false
  }
}
```

## 5. Single Select Dropdown Pattern

Workflow variable:

```json
{
  "id": "AccessType2",
  "name": "Access Type (Single Select with Dropdown list)",
  "type": "text",
  "editable": true
}
```

Request page control:

```json
{
  "type": "radio",
  "label": "Access Type (Single Select with Dropdown list)",
  "binding": "AccessType2",
  "attrs": {
    "choices": ["Choice1", "Choice2", "Choice3"],
    "show_color": false,
    "displayStyle": "dropdown",
    "placeholder": "Please select access type",
    "color_choices": [
      { "value": "Choice1", "key": "<uuid>" },
      { "value": "Choice2", "key": "<uuid>" },
      { "value": "Choice3", "key": "<uuid>" }
    ]
  }
}
```

Observed meaning:

- Dropdown and radio share `type = "radio"` and text variable type.
- Dropdown is controlled by `attrs.displayStyle = "dropdown"`.
- `attrs.color_choices[]` appears in this export for dropdown choices and pairs each value with a UUID key.
- Values are still the selected choice string.

Reusable pattern:

```json
{
  "id": "<uuid>",
  "type": "radio",
  "label": "Access Type",
  "binding": "AccessType",
  "attrs": {
    "choices": ["General Visit", "Interview", "Vendor Service"],
    "show_color": false,
    "displayStyle": "dropdown",
    "placeholder": "Please select access type",
    "color_choices": [
      { "value": "General Visit", "key": "<uuid>" },
      { "value": "Interview", "key": "<uuid>" },
      { "value": "Vendor Service", "key": "<uuid>" }
    ]
  }
}
```

## 6. Radio vs Dropdown Comparison

| Topic | Radio layout | Dropdown layout |
| --- | --- | --- |
| Variable type | `text` | `text` |
| Control type | `radio` | `radio` |
| Choices | `attrs.choices[]` | `attrs.choices[]` |
| Layout switch | no `displayStyle` or non-dropdown | `attrs.displayStyle = "dropdown"` |
| Placeholder | optional | present in export |
| Color metadata | `show_color: false` | `show_color: false` plus `color_choices[]` |
| Raw value | selected string | selected string |

Generator recommendation:

- Use dropdown first for compact enterprise forms.
- Include both `choices[]` and matching `color_choices[]` when generating dropdowns.
- Use radio layout when the choices are very few and should be visible without opening a menu.

## 7. Switch / Boolean Pattern

### Data List Side

`Requires Escort` was added to the Visitor Access Requests list:

```json
{
  "FieldName": "Bit1",
  "InternalName": "Requires_Escort",
  "DisplayName": "Requires Escort",
  "FieldType": "Bit",
  "Type": "switch",
  "DefaultValue": null,
  "Rules": {}
}
```

Sample data raw values:

| Row | Raw `Bit1` |
| --- | --- |
| `VAR-1001` | `"0"` |
| `VAR-1002` | `"0"` |
| `VAR-1003` | `"0"` |

The list sample shape uses string `"0"` for false. Earlier generated/real list examples also showed `"1"` for true.

### Approval Form Side

Workflow variable:

```json
{
  "id": "RequiresEscort",
  "name": "Requires Escort",
  "type": "boolean",
  "editable": true
}
```

Request page control:

```json
{
  "type": "switch",
  "label": "Requires Escort",
  "binding": "RequiresEscort",
  "attrs": {
    "displayStyle": "default"
  },
  "readonly": false,
  "value": false
}
```

Observed meaning:

- Approval form variable type is `boolean`.
- Approval form switch raw default is boolean `false`.
- Data list `Bit` raw sample values use string `"0"`/`"1"`.

Reusable pattern:

```json
{
  "idx": "<uuid>",
  "id": "RequiresEscort",
  "name": "Requires Escort",
  "type": "boolean",
  "editable": true
}
```

```json
{
  "id": "<uuid>",
  "type": "switch",
  "label": "Requires Escort",
  "binding": "RequiresEscort",
  "attrs": {
    "displayStyle": "default"
  },
  "readonly": false,
  "value": false
}
```

ContentList mapping, if persistence is required:

```json
{
  "Per": "0",
  "Columns": "Bit1",
  "Data": "<input type=\"button\" data=\"${&quot;type&quot;:&quot;variable&quot;, &quot;param&quot;:{&quot;id&quot;:&quot;RequiresEscort&quot;}}\" expr=\"__\" tabindex=\"-1\" value=\"Workflow Variables:Requires Escort\">"
}
```

## 8. Dynamic Display Rule Pattern

`Escort User` is a normal text variable/control whose visibility is controlled by `attrs.control_display[]`.

Workflow variable:

```json
{
  "id": "EscortUser",
  "name": "Escort User",
  "type": "text",
  "editable": true
}
```

Target control:

```json
{
  "type": "input",
  "label": "Escort User",
  "binding": "EscortUser",
  "attrs": {
    "placeholder": "Please provide the name",
    "control_display": [
      {
        "id": "<uuid>",
        "controlId": "<target-control-id>",
        "formulas": [
          {
            "exprType": "variable",
            "valueType": "boolean",
            "id": "RequiresEscort",
            "type": "expr",
            "name": "Workflow Variables:Requires Escort"
          },
          {
            "type": "op",
            "op": "=="
          },
          {
            "type": "bool",
            "value": true
          }
        ],
        "actions": {
          "id": "<uuid>",
          "type": 1,
          "attrs": {
            "style_regulation_action": "style_regulation_action_show",
            "style_regulation_action_color": null,
            "action_style": null,
            "icon_type": null
          }
        }
      }
    ]
  }
}
```

Observed meaning:

- The rule lives on the target control, not globally on the page.
- `controlId` repeats the target control ID.
- The source variable formula uses `exprType = "variable"`, `valueType = "boolean"`, `id = "RequiresEscort"`.
- Boolean true is represented as `{ "type": "bool", "value": true }`.
- Show behavior uses `actions.type = 1` and `style_regulation_action_show`.

Reusable pattern:

```json
{
  "attrs": {
    "control_display": [
      {
        "id": "<rule-uuid>",
        "controlId": "<target-control-id>",
        "formulas": [
          { "exprType": "variable", "valueType": "boolean", "id": "RequiresEscort", "type": "expr", "name": "Workflow Variables:Requires Escort" },
          { "type": "op", "op": "==" },
          { "type": "bool", "value": true }
        ],
        "actions": {
          "id": "<action-uuid>",
          "type": 1,
          "attrs": {
            "style_regulation_action": "style_regulation_action_show",
            "style_regulation_action_color": null,
            "action_style": null,
            "icon_type": null
          }
        }
      }
    ]
  }
}
```

## 9. Approval Page Behavior

The new fields were not copied to the approval page in this manual export.

| Field/control | Request page | Approval page |
| --- | --- | --- |
| Number of Visitors | present, editable `input_number` | not present |
| Access Type (Radio) | present, editable `radio` | not present |
| Access Type (Dropdown) | present, editable `radio` dropdown | not present |
| Requires Escort | present, editable `switch` | not present |
| Escort User | present, conditional `input` | not present |

Generator recommendation:

- For generated approval workflows, mirror submitted business fields on the approval/task page as readonly unless there is a clear reason not to.
- For conditional fields, either:
  - copy the same display rule to the approval page, or
  - show the readonly field unconditionally if reviewers need full context.

## 10. ContentList Persistence

No new `ContentList` mappings were added in the manual export.

Existing mappings persisted only the previously generated v10 fields through `Text12`.

To persist the new data-list fields:

| Source variable | Target field |
| --- | --- |
| `NumberofVisitors` | `Decimal1` |
| `RequiresEscort` | `Bit1` |

If a future generated app needs to persist access type or escort user, corresponding data-list fields must be created first. This export did not create data-list fields for `AccessType1`, `AccessType2`, or `EscortUser`.

## 11. Validator Recommendations

### `validate-ywf-def.js`

Recommended checks:

- `input_number` controls must bind to a variable with `type = "number"`.
- Number controls should allow known attrs:
  - `displayThousandths`
  - `rounded-to`
  - `number_min`
  - `number_max` if present in future exports
- Number control default `value`, if present, must be numeric.
- `radio` controls must bind to `type = "text"` variables for single select.
- `radio.attrs.choices` must exist and be a non-empty string array.
- Dropdown radio controls should use `attrs.displayStyle = "dropdown"`.
- Dropdown `color_choices[]`, if present, should align with `choices[]`.
- `switch` controls must bind to `type = "boolean"` variables.
- `switch.value`, if present, must be boolean.
- `attrs.control_display[]` source variables must exist.
- Dynamic display formulas must use compatible value types.
- Dynamic display `controlId` must match the target control ID.

### `validate-yap-package.js`

Recommended checks:

- Data-list `Decimal` + `input_number` fields should have parseable numeric rules.
- Data-list `Bit` + `switch` fields should use `Bit1`, `Bit2`, etc. style slots and parseable rules.
- Approval-form number variables mapped to `ContentList` should target `Decimal` fields.
- Approval-form boolean variables mapped to `ContentList` should target `Bit` fields.
- Choice variables mapped to data-list fields should target text/radio-compatible fields with compatible choices.
- If a generated package adds request-page fields but not approval-page readonly mirrors, warn unless intentionally marked request-only.

### `validate-yap-graph.js`

Recommended checks:

- Add edges from form controls to workflow variables.
- Add edges from dynamic display target controls to source variables.
- Add edges from `ContentList` source variables to target fields.
- Report type compatibility on `ContentList` edges:
  - `number -> Decimal/input_number`
  - `boolean -> Bit/switch`
  - `text -> Text/input/radio`
- Warn when data-list fields exist for form variables but `ContentList` does not map them.

## 12. Generator Rules

Number field:

- Use approval variable `type = "number"`.
- Use approval control `type = "input_number"`.
- Store formatting/min settings in `attrs`.
- Store default value as control `value` when needed.
- Use data-list `FieldType = "Decimal"` and `Type = "input_number"`.
- Persist to a `Decimal*` target through `ContentList` when storage is required.

Single-select radio:

- Use approval variable `type = "text"`.
- Use control `type = "radio"`.
- Use `attrs.choices[]`.
- Omit `displayStyle = "dropdown"` for visible radio-button layout.

Single-select dropdown:

- Use approval variable `type = "text"`.
- Use control `type = "radio"`.
- Add `attrs.displayStyle = "dropdown"`.
- Include `attrs.choices[]`.
- Include `attrs.color_choices[]` with stable UUID keys when generating dropdowns.

Switch field:

- Use approval variable `type = "boolean"`.
- Use approval control `type = "switch"`.
- Use default boolean `value` on the control when needed.
- Use data-list `FieldType = "Bit"` and `Type = "switch"`.
- Use data-list sample values `"1"` and `"0"` for true/false.

Conditional display:

- Put the rule on the target control under `attrs.control_display[]`.
- Reference source variable by variable ID.
- Use boolean formulas for switch-driven display.
- Use show action `style_regulation_action_show`.

Approval page:

- Mirror request fields as readonly on approval pages in generated approval workflows.
- Decide explicitly whether dynamic display rules should be mirrored or whether reviewers should always see derived context.

ContentList:

- Create target data-list fields before mapping new approval variables.
- Validate field type compatibility before wrapper build.
- Do not assume request-page-only controls are persisted.

## 13. Current Readiness

Now confirmed from export:

- Approval number control shape.
- Approval radio single-select shape.
- Approval dropdown single-select shape.
- Approval switch shape.
- Dynamic show rule shape for switch equals true.
- Data-list Decimal/input_number shape.
- Data-list Bit/switch shape.

Still needs a generated import test:

- Full generated app package containing these field types.
- Approval page readonly mirrors for these field types.
- `ContentList` persistence of `number -> Decimal` and `boolean -> Bit`.
- Choice persistence to data-list target fields.

## 14. Generated v11 Confirmation

`visitor-access-management.v11-five-fields-multitype.yap` completed the generated import/runtime proof for the patterns studied in the manual v10 export.

Confirmed generated package settings:

- Fresh ID family: `216...`
- Fresh FlowKey/form key: `VBB`
- App-level approval form `Data.Forms[].ListID = 0`
- Package, graph, approval-form, and wrapper round-trip validations passed before import testing.
- App imported and passed runtime testing.
- Multi-field expansion worked.
- Multi-type controls worked.

### Proven Generated Field/Control Types

| Pattern | Generated approval form | Generated data list | Status |
| --- | --- | --- | --- |
| text/input | `type = "input"` with text variable | `Text13`, `Text14` | proven |
| number/input_number | number variable + `input_number` control | `Decimal1`, `FieldType = "Decimal"`, `Type = "input_number"` | proven |
| single select dropdown | text variable + `radio` control + `attrs.displayStyle = "dropdown"` | `Text15`, `FieldType = "Text"`, `Type = "radio"` | proven |
| switch/boolean | boolean variable + `switch` control | `Bit1`, `FieldType = "Bit"`, `Type = "switch"` | proven |
| conditional display | target control `attrs.control_display[]` | form-only behavior | proven |

### Proven Storage And ContentList Mappings

| Source variable | Target storage field |
| --- | --- |
| `VisitorEmail` | `Text13` |
| `VisitorPhone` | `Text14` |
| `NumberofVisitors` | `Decimal1` |
| `AccessType` | `Text15` |
| `RequiresEscort` | `Bit1` |

### Proven Sample Value Shapes

| Field type | Generated sample shape |
| --- | --- |
| Decimal | numeric values such as `1`, `2`, `4` |
| Choice/dropdown | selected option text |
| Bit/switch in data-list samples | string `"1"` / `"0"` |
| Boolean approval control value | boolean `true` / `false` |

### Conditional Display Confirmation

`Escort User` was generated as a form-only conditional field:

- source switch variable: `RequiresEscort`
- target text variable/control: `EscortUser`
- condition: `RequiresEscort == true`
- storage location: `Escort User` control `attrs.control_display[]`
- persistence: not mapped in v11

This confirms the rule shape but leaves persisted conditional fields for a future test.

### Updated Readiness

The following are now proven for generated app-level `.yap` packages:

- `number -> Decimal/input_number` persistence.
- `boolean -> Bit/switch` persistence.
- `text/radio dropdown -> Text/radio` persistence.
- Approval-page readonly mirrors for the generated fields.
- Conditional display using `attrs.control_display[]`.

Still needs future examples:

- Persisting the conditional `EscortUser` target field.
- Multi-select choice.
- User picker.
- Lookup inside a line-item table.
- Dashboards and reports.

---

<a id="approval-form-control-runtime-coverage-matrix"></a>

<!-- source: docs/collection-01.md#approval-form-control-runtime-coverage-matrix -->

# Approval Form Control Runtime Coverage Matrix

Date: 2026-05-14

Sources:

- `docs/collection-01.md#ai-training-approval-form-control-study`
- `docs/collection-02.md#generated-approval-form-controls-test-baseline`
- `docs/collection-02.md#generated-approval-form-controls-test-v2-baseline`
- `docs/collection-02.md#generated-approval-form-controls-test-v3-baseline`
- `docs/collection-02.md#generated-approval-form-controls-test-v4-baseline`
- `docs/collection-02.md#generated-approval-form-controls-test-v6-baseline`
- `control-configurations.normalized.json`
- `field-configurations.normalized.json`

This matrix separates export-backed control anatomy from generated runtime proof. A control is not considered generation-safe just because it appeared in `AI Training.yap`; it becomes runtime-proven only after a generated package imports, opens, renders the control, submits, completes the approval task, and persists the expected value when persistence is in scope.

## Status Legend

| Status | Meaning |
| --- | --- |
| Proven | Generated package passed import/open/render/submit/approval/persistence for the control or infrastructure behavior. |
| Partially proven | Generated package rendered the control, but value entry or persistence was not fully exercised. |
| Schema-supported but untested | Normalized schema or export anatomy exists, but no focused generated runtime package has proved it yet. |
| Runtime-sensitive | Shape depends on tenant metadata, uploads, source lists, tree data, listrefs, or unresolved runtime behavior. |
| Deferred | Do not generate by default until a focused export/import proof exists. |

## Matrix

| Control | Variable type | AI Training | v1 included | Runtime status | Reason / next test |
| --- | --- | --- | --- | --- | --- |
| `input` | `text` | Yes | Yes | Proven | Request Title submitted and persisted to `Title`. |
| `textarea` | `text` | Yes | Yes | Proven | Rendered on submit/review pages. Persist long text in a later form-focused run if needed. |
| `richtext` | `text` | Yes | Yes | Partially proven | Editor rendered after loading, but formatted value entry/persistence was not exercised. |
| `input_number` | `number` | Yes | Yes | Proven | Quantity accepted numeric input and persisted. |
| `currency` | `number` | Yes | Yes | Proven | Unit Price accepted USD value and participated in calculation/persistence. |
| `percent` | `number` | Yes | Yes | Proven | v2 accepted `25.00%`, displayed it on the approval task, used it in calculation, and persisted it to the list view. |
| `radio` | `text` | Yes | Yes | Partially proven | Dropdown rendered in v1, but value was not selected. Test in a choice-focused package. |
| `checkbox` | `text` | Yes | Yes | Proven | Selection rendered, reviewed, and persisted as a JSON-like array string. |
| `switch` | `boolean` | Yes | Yes | Proven | Rendered and persisted OFF/ON-compatible values. |
| `datepicker` | `date` | Yes | Yes | Proven | Date rendered and persisted. |
| `daterange` | `date` | Yes | Yes | Partially proven | v2 accepted from/to values and displayed them on the approval task; a later package should expose both mapped date fields in the list view. |
| `time` | `date` | Yes | No | Proven | v2 accepted `10:30:00`, displayed it on the approval task, and persisted it to the list view. |
| `file-upload` | `file` | Yes | Yes | Proven | v3 rendered, uploaded `yeeflow-upload-stage3-test.txt`, submitted, displayed the uploaded file on submitted/reviewer pages, approved, and created the text persistence record. Binary list persistence remains deferred. |
| `icon-upload` | `img` | Yes | No | Proven | v3 rendered, uploaded `yeeflow-upload-stage3-test.png`, displayed the image preview on submitted/reviewer pages, approved, and created the text persistence record. Binary list persistence remains deferred. |
| `identity-picker` | `user` | Yes | Yes | Proven | Current user resolved in v1/v4; v4 editable single-select user picker retained `Renger from Yeeflow`, displayed on submitted/reviewer pages, and completed approval. |
| `organization-picker` | `groupselect` | Yes | No | Partially proven | v4 rendered and opened the department selector with `Default` visible; selection did not retain after OK in this sandbox, so value behavior remains environment-sensitive. |
| `location-picker` | `location` | Yes | No | Partially proven | v4 rendered and opened the location picker; sandbox returned no matching location data. |
| `cost-center-picker` | `costcenter` | Yes | No | Partially proven | v4 rendered and opened the cost center selector; sandbox returned no cost center rows. |
| `metadata` | `metadata` | Yes | No | Deferred | Stage 5 intentionally skipped for now; requires known `source` and `categoryId`; do not guess metadata category IDs. |
| `mutiple-metadata` | `mutiple-metadata` | Yes | No | Deferred | Stage 5 intentionally skipped for now; same metadata source risk plus multi-value shape. |
| `lookup` | `lookup` | Yes | Yes | Proven | v6 proved packaged internal lookup selection, display, addition/autofill mappings, review display, approval completion, and target record creation. Raw lookup-to-text persistence stored the local row ID, so use addition/summary fields for readable persistence. |
| `lookup-list` | `lookup` | Yes | No | Deferred | Not included in v6; relation-name addition shape still needs an isolated generated proof. |
| `list` / `listref` | `list` | Yes | No | Proven | v6 proved table render, add row, edit row, submitted/reviewer display, approval completion, and text-summary persistence. Direct child-row persistence remains deferred. |
| `data-list` | n/a | Yes | No | Deferred | Not included in v6; embedded display should be isolated after lookup/listref proof. |
| `signer` | `signer` | No | No | Deferred | Schema-supported but not in AI Training; needs focused signer export. |
| `tag` | `tag` | No | No | Deferred | Stage 5 intentionally skipped for now; use text/checkbox fallback until a generated package proves tag source behavior. |
| `rate` | `number` | No | No | Proven | v2 rendered the star rating, accepted `3.5`, displayed it on review, and persisted it to the list view. |
| `hyperlink` | `text` | No | No | Proven | v2 accepted a URL, displayed it as an open link on review, and persisted it as a list-view link. |
| `calculated` | `number` | No | Yes | Proven | v1 proved subtotal and v2 proved percent-based calculation: `200 * 25% = 50`. |
| `aktabs` / tab controls | n/a | Yes | No | Schema-supported but untested | Test separately as a layout-control package after value controls. |
| `workflowControlPanel` | n/a | Yes | Yes | Proven | Submit and approval actions completed successfully. |
| `workflowHistory` | n/a | Yes | Yes | Proven | Flow history rendered on review task page. |

The machine-readable version is `fixtures/runtime-test-specs/approval-form-control-runtime-coverage.json`.

## Consolidated Coverage Summary

Runtime-proven for generated packages:

- Basic values and layout: `input`, `textarea`, `input_number`, `currency`, `checkbox`, `switch`, `datepicker`, `workflowControlPanel`, `workflowHistory`
- Advanced inputs: `percent`, `time`, `hyperlink`, `rate`, `calculated`
- Upload/media workflow-form usage: `file-upload`, `icon-upload`
- People picker: editable single-select `identity-picker`
- Lookup/list controls: internal packaged single-select `lookup`, workflow-form `list` / `listref`

Partially proven:

- `richtext`: editor rendered, but formatted value persistence is not proven.
- `radio`: dropdown rendered, but selected value persistence still needs a focused choice proof.
- `daterange`: from/to values rendered and reviewed, but both mapped fields still need list-view persistence proof.
- `organization-picker`, `location-picker`, `cost-center-picker`: controls rendered/opened, but usable selection depends on tenant metadata and retention attrs.

Skipped/deferred:

- Stage 5 was intentionally skipped: `metadata`, `mutiple-metadata`, `tag`.
- `lookup-list`, embedded `data-list`, `signer`, and `aktabs` remain separate future packages.

Persistence-safe posture:

- Use direct `ContentList` mappings for proven primitive fields such as text, textarea, number, currency, percent, checkbox-as-text, switch, date/time, hyperlink, rate, and calculated values.
- Use text-summary persistence for file/image uploads unless binary list-field persistence is explicitly proven.
- Use text-summary persistence for identity/picker controls unless direct native picker fields are explicitly proven.
- Use `attrs.addition[]` autofill variables or explicit summary variables for lookup display text and related readable values.
- Use list/listref for workflow-form row capture and review display, but persist a text summary until direct child-row persistence is export-proven.

Render-only or selection-only posture:

- Do not claim runtime-safe persistence for `richtext`, `radio`, `daterange`, organization/location/cost-center pickers, metadata, tags, lookup-list, embedded data-list, signer, tabs, or direct sublist row persistence until an isolated generated package proves the exact behavior.

## V1 Proven Set

`Approval Form Controls Test v1` proved:

- `input`
- `textarea` render
- `input_number`
- `currency`
- `checkbox`
- `switch`
- `datepicker`
- `identity-picker`
- `calculated`
- `workflowControlPanel`
- `workflowHistory`

It partially proved:

- `richtext`
- `radio`
- `file-upload`
- `lookup`

## V2 Proven Set

`Approval Form Controls Test v2 - Advanced Input Controls` proved:

- `percent`
- `time`
- `hyperlink`
- `rate`
- `calculated`

It partially proved:

- `daterange` value entry and review rendering. A later package should expose both mapped date fields in the list view before promoting it to fully proven persistence.

## V3 Proven Set

`Approval Form Controls Test v3 - Upload and Media Controls` proved:

- `file-upload`
- `icon-upload`

Runtime evidence:

- Imported `Approval Form Controls Test v3.generated.yap` successfully.
- Opened the imported app and `Upload Media Test Requests` without `datas/query` 400.
- Published and opened `Approval Form Controls Test v3`.
- Uploaded a tiny safe text file: `yeeflow-upload-stage3-test.txt`.
- Uploaded a tiny safe image file: `yeeflow-upload-stage3-test.png`.
- Submitted the form successfully.
- Submitted request detail showed the uploaded file name and image preview.
- Reviewer task opened and displayed both uploaded values.
- Approval completed successfully.
- `ContentList` created a new `Upload Media Test Requests` row with the expected text summary.

Scope note: v3 proves workflow-form upload/media usage through render, upload, submit, task display, and approval completion. Binary file/image persistence into a data-list field was intentionally out of scope and remains deferred.

## V4 Runtime Set

`Approval Form Controls Test v4 - People and Organization Picker Controls` proved:

- `identity-picker` editable single-select workflow-form usage

It partially proved:

- `organization-picker`
- `location-picker`
- `cost-center-picker`

Runtime evidence:

- Imported `Approval Form Controls Test v4.generated.yap` successfully.
- Opened the imported app and `Picker Test Requests` without `datas/query` 400.
- Opened `Approval Form Controls Test v4` approval form.
- `Requester` identity-picker resolved to the current user.
- `Selected User` identity-picker opened the user selector, selected `Renger from Yeeflow`, retained the value, displayed it on the submitted request and reviewer task, and completed approval.
- `organization-picker` rendered and opened the department selector with `Default` visible, but the selected department did not retain after confirmation in this sandbox.
- `cost-center-picker` rendered and opened the cost center selector, but the sandbox returned no rows.
- `location-picker` rendered and opened the native picker/dropdown, but the sandbox returned no matching location data.
- Submit completed successfully.
- Reviewer task opened and displayed the selected user plus notes.
- Approval completed successfully.
- `ContentList` created a new `Picker Test Requests` row with the expected text summary.

Scope note: v4 proves identity-picker workflow-form usage and partial render/open behavior for tenant-metadata pickers. Direct picker-value persistence and organization/cost-center/location selection remain deferred until the tenant has selectable metadata or a focused working export proves the required retention attrs.

## V6 Runtime Set

`Approval Form Controls Test v6 - Lookup and List Controls` proved:

- `lookup`
- `list` / `listref`

Runtime evidence:

- Imported `Approval Form Controls Test v6.generated.yap` successfully.
- Opened the imported app, `Reference Products`, and `Lookup List Test Requests` without `datas/query` 400.
- `Reference Products` loaded packaged sample rows for `27-inch Monitor`, `Docking Station`, and `Standard Laptop`.
- Opened `Approval Form Controls Test v6` approval form.
- Lookup picker opened and showed packaged source records.
- Selected `Standard Laptop`; submit page displayed the selected lookup value.
- Lookup `attrs.addition` mappings populated `Product Code = LAP-STD`, `Product Category = Hardware`, and `Unit Price = USD 1,250.00`.
- Added one list/sublist row and edited `Product`, `Quantity`, `Unit Price`, and `Line Note`.
- Submitted the form successfully.
- Submitted request detail and reviewer task displayed the lookup value and list row values.
- Reviewer approval completed successfully.
- `ContentList` created a new `Lookup List Test Requests` row.

Generator rule learned: a raw lookup variable mapped into a plain text data-list field persisted the internal local row ID (`2054943200723742740`) rather than the display text. For business-readable persistence, map addition/autofill values or explicit text summary variables; use direct lookup persistence only when storing the local row ID is intentional or the target field is a proven lookup-compatible field.

Scope note: v6 did not include `lookup-list` or embedded `data-list` display. Those remain deferred for isolated packages.

## Remaining Staged Runtime Tests

Stage 5 is intentionally skipped for now:

- `metadata`
- `mutiple-metadata`
- `tag`

The next package should be Stage 7: signer and special controls, unless lookup-list or embedded data-list display becomes higher priority.

Later metadata/tag work should resume only with export-backed source/category/tag behavior.

---

<a id="approval-form-generator-operating-playbook"></a>

<!-- source: docs/collection-01.md#approval-form-generator-operating-playbook -->

# Approval Form Generator Operating Playbook

This is the master operating playbook for Codex-generated Yeeflow approval forms. It consolidates the proven v6 baseline, requirement decomposition, custom-code decision rules, `.yap` metadata study, validators, metadata application, and wrapper build workflow.

Reference docs:

- `docs/collection-02.md#generated-approval-form-baseline-v6`
- `docs/collection-01.md#approval-form-requirement-decomposition-template`
- `docs/collection-02.md#custom-code-control-decision-guide`
- `docs/collection-02.md#generated-line-item-custom-code-row-label-pattern`
- `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`
- `docs/collection-02.md#procurement-management-yap-study`
- `procurement-metadata.json`
- `docs/collection-02.md#validate-ywf-def`
- `docs/collection-02.md#validate-ywf-def-against-yap`
- `docs/collection-01.md#apply-ywf-metadata`
- `docs/collection-01.md#build-ywf-wrapper`

## 1. Purpose

Codex can generate Yeeflow approval forms, but only through a staged, validated, native-first process.

The goal is not to generate a `.ywf` directly from a business prompt. The safe path is:

1. decompose the business requirement
2. identify native Yeeflow capabilities
3. resolve app/list/field metadata
4. generate a decoded Def JSON draft
5. validate structure and app-context dependencies
6. build a wrapper only after validation passes
7. test in a Yeeflow sandbox
8. compare the exported-back package before treating the pattern as proven

## 2. Proven Capabilities

The current generator work has proven these capabilities:

| Capability | Status |
| --- | --- |
| One-step approval | Proven with Simple PR Note Approval. |
| Two-step approval | Proven with Simple PR Amount Approval and Simple PR Line Item Approval. |
| Structured request/approval pages | Proven with v6 baseline. |
| Responsive grid UI | Proven with v6 baseline using section containers and `flex_grid`. |
| Current user default | Proven for Applicant fields. |
| Current date default | Proven for Submission Date fields. |
| Workflow action panel/history | Proven on request and approval pages. |
| Clean workflow designer layout | Proven after export/import layout correction. |
| Page registration/publish metadata | Proven after fixing request page key/url pattern. |
| ContentList parent persistence | Proven against PR Records. |
| Line-item table | Proven with `PurchaseDetailsList`. |
| Parent/child persistence | Proven with PR Records and PR Details List. |
| Custom code control when justified | Proven for sub-list row labels. |
| App metadata extraction from `.yap` | Proven with Procurement Management export. |
| App-context validation | Proven with `procurement-metadata.json`. |
| `.ywf` wrapper generation | Proven with round-trip validation. |

## 3. Standard Generation Pipeline

```text
Business requirement
  -> requirement decomposition
  -> metadata selection/extraction
  -> decoded Def JSON draft
  -> structural validation
  -> app-context validation
  -> wrapper build
  -> sandbox import
  -> runtime test
  -> export-back comparison
```

Codex should not skip stages. In particular, it should not create a final `.ywf` while placeholders remain or while validators fail.

## 4. Native-First Decision Rule

Codex must prefer native Yeeflow features before adding custom code.

Decision hierarchy:

1. Standard form controls
2. Control attrs, readonly, defaults, validation
3. Calculated fields and list summaries
4. Lookup configuration and filters
5. Form actions
6. Workflow actions
7. AI actions
8. Custom code control

Use custom code only for client-side interaction behavior that cannot be modeled safely with the native layers above.

### Lookup Control Rule

Approval forms can use native lookup controls when the requester should select an existing app record, such as Department, Supplier, Project, Product, or Cost Center.

Confirmed source type:

- data list

Confirmed data-list lookup metadata:

- `attrs.appid`
- `attrs.listsetid`
- `attrs.listid`
- `attrs.listfield`
- optional `attrs["sort-first"]`
- optional `attrs.addition[]`

Use choices/radio/dropdown instead when values are static, small, and do not need a maintained source list.

Use lookup additional fields to auto-fill related values into form variables. Derived target controls should usually be readonly. For example, a Department lookup can map source field `Text1` / FieldID `2053735610866089989` into target variable `DepartmentCode`.

Do not guess lookup source IDs, display fields, sort fields, or additional source field IDs. Require confirmed app/list/field metadata or a staged dependency-resolution process.

Ask whether lookup selections and derived fields should be persisted. Context-only derived fields may not need `ContentList` persistence. If persisted, validate the lookup runtime value shape and target field compatibility before final wrapper build.

## 5. Required Artifacts

For generated approval forms, Codex should produce these artifacts as appropriate:

| Artifact | Purpose |
| --- | --- |
| Requirement decomposition | Shows how the business request maps to data, UI, workflow, persistence, AI, and custom-code decisions. |
| Decoded Def JSON | The generated Yeeflow form/workflow definition before base64 wrapping. |
| Dependency mapping | Lists app/list/field/user/position/template dependencies and placeholders. |
| Metadata mapping | Filled mapping file used by `apply-ywf-metadata.js` when placeholders are resolved separately. |
| Validation reports | Structural and app-context validation results. |
| `.ywf` wrapper | Created only after validation passes and unresolved placeholders are gone. |

## 6. Approval Form Template Rules

### Request Page

Use the v6 structured UI baseline:

- top-level container
- header container
- title text
- description text
- fields container
- responsive `flex_grid`
- line-item section when needed
- bottom action/history container
- request-page `workflowControlPanel`
- request-page `workflowHistory`

System field defaults:

- Applicant: readonly, default current user
- Submission Date: readonly, default current date
- Request No.: readonly, generated from FlowNo

### Approval Page

Use a parallel review-focused layout:

- top-level container
- header title and review description
- readonly business fields
- readonly line-item table when present
- approval-page `workflowControlPanel`
- approval-page `workflowHistory`

Do not use request-only wording such as "Save draft" on the approval page.

### Page Registration Metadata

Generated approval forms must include publish-ready page metadata:

- request page key/id pattern expected by Yeeflow
- approval page key/id pattern expected by Yeeflow
- `StartNoneEvent.properties.taskurl` pointing to the request page reference
- `MultiAssignmentTask.properties.taskurl` pointing to the approval page reference

Missing page registration can cause publish-time errors such as request `pageUrl is null`.

### Layout Rules

Use conservative UI defaults:

- neutral page background
- section containers
- responsive two-column grid on desktop/tablet
- one-column grid on mobile
- full-width spans for long text, descriptions, and tables
- no experimental decorative colors unless requested

## 7. Workflow Design Rules

### One-Step Approval

Recommended flow:

```text
Start
  -> Set Request No.
  -> Approval Task
  -> Create Record
  -> End

Approval Task rejected
  -> EndRejectEvent
```

### Two-Step Approval

Recommended flow:

```text
Start
  -> Set Request No.
  -> Line Manager Approval
  -> Finance Approval
  -> Create Record
  -> End

Line Manager rejected
  -> EndRejectEvent

Finance rejected
  -> EndRejectEvent
```

### Conditional Approval

Use native workflow gateways and SequenceFlow conditions. Do not implement conditional approval routing in custom code.

Example:

```text
Manager Approval
  -> Gateway
      -> Finance Approval when amount > threshold
      -> Create Record when amount <= threshold
```

### Persistence Nodes

Use `ContentList` workflow nodes for persistence:

- parent record creation
- child/detail row creation
- record updates
- record removal when explicitly required

Persistence should occur after final approval unless the business requirement says otherwise.

### Rejection Paths

Every approval task should have:

- approved SequenceFlow
- rejected SequenceFlow
- rejected path to `EndRejectEvent`

### Graph Layout Spacing

Workflow graph metadata matters for Yeeflow designer usability.

Layout rules:

- avoid placing all nodes in one horizontal line
- use clear vertical separation for approval tasks
- route approved paths horizontally to persistence/end nodes
- route rejected paths downward to `EndRejectEvent`
- include SequenceFlow visual metadata/dockers/vertices as learned from real exports
- keep `graphposition` large enough for all nodes and connectors

## 8. Line-Item Table Rules

Line-item tables require:

- a `variables.basic` list variable
- a matching `variables.listref`
- request page list control
- approval page readonly list control
- `attrs["list-fields"]`
- `attrs["list-variables"]`
- consistent row field metadata across listref and page controls

### Row Calculations

Use native row formulas for:

- quantity * unit price
- subtotal
- row-level calculated values

### Total Summaries

Use native list summary/calculated total patterns for:

- estimated total amount
- subtotal sum
- count/amount summary when supported

### Child Persistence

Use `ContentList` nodes for detail-row creation. Map each row field using list-row variable expressions.

Do not persist detail rows with custom code.

### Optional Custom Row Labels

Auto row labels are optional and customer-specific.

Numeric labels:

- `No.` may be number
- row control may be `input_number`
- generated row control should be readonly

Alphabetic labels:

- `No.` must be text
- row control should be `input`
- generated row control should be readonly
- target persistence field must be text-compatible if persisted

## 9. Custom Code Rules

Use `docs/collection-02.md#custom-code-control-decision-guide` as the detailed policy.

Custom code is appropriate for:

- display-only helpers
- row labels
- dynamic UI hints
- non-critical client-side validation
- custom sub-list interactions not supported natively

Custom code should not be used for:

- approval routing
- record persistence
- critical financial calculations
- security/permission logic
- external API calls without explicit approval

When custom code is needed, Codex must output:

A. Requirement being solved
B. Why native Yeeflow configuration is insufficient
C. Custom code design
D. Code
E. Parameters
F. Def changes
G. Validation report
H. Test plan
I. Risks and fallback

Generated code should prefer new-style readable source:

```ts
import * as React from 'react';

export class CodeInApplication implements CodeInComp {
  description() {}
  inputParameters() {}
  requiredFields(params) {}
  render(context, fieldsValues, readonly) {}
}
```

## 10. Validation Gates

### `validate-ywf-def.js`

Use for structural validation of decoded Def JSON.

Checks include:

- required Def sections
- variable uniqueness
- listref consistency
- page/control bindings
- request/approval pages
- taskurl references
- workflow graph connectivity
- SequenceFlow target validity
- approval paths
- ContentList structure
- lookup control source metadata
- lookup additional-field target variables
- readonly warnings for derived lookup target controls
- placeholders

### `validate-ywf-def-against-yap.js`

Use for app-context validation against extracted `.yap` metadata.

Checks include:

- ContentList target app/list/listset existence
- target field internal names
- source variable/list-row references
- type compatibility
- lookup source references
- lookup display/sort/source fields against app metadata
- lookup additional-field source fields and target variables
- document/AI references when available

Use `--profile generator` for generated packages. Use `--profile compat` only when studying real exports with known compatibility quirks.

### `apply-ywf-metadata.js`

Use when a decoded Def draft contains placeholders that need environment-specific values.

Rules:

- metadata item status must be `ready`
- required values cannot be empty
- semantic locators must match expected placeholder values
- output must have no unresolved placeholders in final mode

### `build-ywf-wrapper.js`

Use only after decoded Def validation passes.

Checks include:

- decoded Def JSON validity
- no unresolved placeholders
- FlowKey equals `defkey`
- WorkflowType equals `workflowType`
- base64 wrapper generation
- round-trip decode equals source Def

## 11. Sandbox Testing Checklist

After wrapper build, test in a Yeeflow sandbox:

1. Import the `.ywf`.
2. Confirm request page opens.
3. Confirm approval page opens.
4. Publish the workflow.
5. Submit a request.
6. Confirm defaults: Applicant, Submission Date, Request No.
7. Confirm required fields and validation.
8. Confirm line-item table behavior if present.
9. Confirm approval routing.
10. Confirm rejection path.
11. Confirm ContentList parent persistence.
12. Confirm ContentList child/detail persistence.
13. Confirm custom code behavior if present.
14. Export the imported form back from Yeeflow.
15. Compare exported-back Def against generated Def and update patterns if Yeeflow normalized anything important.

## 12. Stop Conditions

Codex must stop and not build a final `.ywf` when:

- placeholders remain
- app/list/field metadata is missing
- lookup source metadata or additional source fields are unresolved
- lookup sample/value persistence shape is unknown for a persisted lookup field
- structural validation fails
- app-context validation fails
- FlowKey does not match `defkey`
- WorkflowType does not match `workflowType`
- custom code is unreviewed
- custom code contains high-risk behavior without explicit approval
- target app metadata is unavailable
- credential-like or sensitive resources are involved
- production import is requested without sandbox proof

In stop cases, Codex should report:

- the blocker
- affected file/path/node/control
- required missing metadata or decision
- whether draft-mode generation can continue

## 13. Readiness Levels

| Level | Meaning | Allowed output |
| --- | --- | --- |
| Draft-ready | Business requirement is decomposed and unresolved dependencies are identified. | Requirement decomposition, dependency map, partial decoded Def. |
| Validation-ready | Decoded Def is complete enough for validators. | Decoded Def JSON and validation report. |
| Wrapper-ready | Final-mode validators pass and placeholders are gone. | `.ywf` wrapper may be built. |
| Sandbox-import-ready | Wrapper round-trip validation passes. | User may manually import into sandbox. |
| Production-candidate | Sandbox import, publish, runtime test, persistence test, and export-back comparison pass. | Candidate for controlled production workflow planning. |

## 14. Next Roadmap

Recommended future work:

| Area | Why it matters |
| --- | --- |
| Conditional approval patterns | Needed for amount thresholds, category-specific routing, and skip logic. |
| Document generation | Needed for PO/PDF generation and template/library dependencies. |
| AI summary/recommendation patterns | Needed for approval suggestions, risk summaries, and generated request summaries. |
| Lookup-heavy forms | Needed for supplier, product, category, purchase order, and payment-term scenarios. |
| Lookup variants | Multi-select, data report, form report, document library, filtered lookup, lookup inside line-item table, and persisted lookup values still need real export examples. |
| Reusable template pack generation | Allows Codex to package proven decoded Def patterns into a maintained library. |
| Export-back comparison tool | Automatically detects Yeeflow normalization after import/publish/manual correction. |
| Validator custom-code checks | Detect row-label schema mismatch, readonly issues, high-risk APIs, and missing declared reasons. |
| App-level package generation study | Future `.yap` package generation may reduce manual app/list setup, but requires more import/export proof. |

## 15. Field Type Patterns From App-Level Export

The manual Visitor Access Management v10 export added number, radio/dropdown, switch, and conditional display controls to an approval form. See `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`.

Confirmed approval-form patterns:

- Number field:
  - workflow variable `type = "number"`
  - control `type = "input_number"`
  - format/min settings in `attrs`
  - default value on control `value`
- Single-select radio:
  - workflow variable `type = "text"`
  - control `type = "radio"`
  - options in `attrs.choices[]`
- Single-select dropdown:
  - same `radio` control
  - `attrs.displayStyle = "dropdown"`
  - options in `attrs.choices[]`
  - optional `attrs.color_choices[]`
- Switch:
  - workflow variable `type = "boolean"`
  - control `type = "switch"`
  - boolean default on control `value`
- Dynamic display:
  - target control stores `attrs.control_display[]`
  - source variable formula can reference a boolean variable
  - show action uses `style_regulation_action_show`

Generator rules:

- Mirror request fields onto approval pages as readonly unless the field is intentionally request-only.
- Persist fields only when a compatible target data-list field exists.
- Validate `number -> Decimal/input_number`, `boolean -> Bit/switch`, and `text/radio -> Text/radio` compatibility before wrapper build.
- Do not assume a request-page control is persisted; `ContentList` must map it explicitly.

## 16. Generated v11 Multi-Type Confirmation

`Visitor Access Management v11` confirms that the manual field-type patterns can be generated, validated, imported, and runtime-tested inside an app-level `.yap` package.

Successful v11 approval-form settings:

- Fresh FlowKey/form key: `VBB`
- App-level form row `Data.Forms[].ListID = 0`
- Approval form validation: pass
- Package and graph validation: pass
- Request page rendered the multi-type controls
- Approval page included readonly mirrors for the persisted fields
- Workflow `ContentList` persisted the new values into the generated request list

Proven approval control patterns:

| Business field | Variable | Approval control | Target storage |
| --- | --- | --- | --- |
| Visitor Email | `VisitorEmail` text | `input` | `Text13` |
| Visitor Phone | `VisitorPhone` text | `input` | `Text14` |
| Number of Visitors | `NumberofVisitors` number | `input_number` | `Decimal1` |
| Access Type | `AccessType` text | `radio` with `displayStyle = "dropdown"` | `Text15` |
| Requires Escort | `RequiresEscort` boolean | `switch` | `Bit1` |

Proven sample/control value shapes:

- `input_number` values are numeric.
- Dropdown/radio values are selected option text.
- Switch controls use boolean `true` or `false` in the approval form.
- Data-list `Bit` sample values use `"1"` or `"0"`.

Conditional display rule:

- `EscortUser` was generated as a form-only text variable/control.
- It is shown when `RequiresEscort == true`.
- The rule is stored on the target control under `attrs.control_display[]`.
- It is intentionally not persisted in v11 because no storage target field was added.

Generator rule update:

- Multi-field approval-form expansion is allowed after field/control types are proven by export evidence and import-tested generated packages.
- Still use fresh FlowKey/form keys for every app-level import test.
- Still validate decoded approval Def, app package, and app graph before wrapper build.
- Do not persist a conditional field unless a compatible target storage field exists and the mapping is explicitly added.

## Operating Rule Summary

- Decompose first.
- Native first.
- Metadata before final mode.
- Validate decoded Def before wrapping.
- Build `.ywf` only after validation passes.
- Sandbox test before production consideration.
- Export back and compare before declaring a pattern proven.

---

<a id="approval-form-requirement-decomposition-template"></a>

<!-- source: docs/collection-01.md#approval-form-requirement-decomposition-template -->

# Approval Form Requirement Decomposition Template

Before Codex generates a Yeeflow approval form, it should decompose the business requirement into structured design decisions. This keeps generation native-first, dependency-aware, and validation-ready.

## 1. Process Overview

| Item | Value |
| --- | --- |
| Form name |  |
| Flow key |  |
| Business purpose |  |
| Requester role |  |
| Expected outcome |  |
| Target app/list/resource |  |
| Workflow type | Approval form |

Questions:

- What business event starts the request?
- What should exist after final approval?
- Is this a new request, an update to an existing record, or both?

## 2. Data Model

| Category | Requirement | Native field/control | Notes |
| --- | --- | --- | --- |
| System fields | Applicant, submission date, request number | Current user, current date, FlowNo | Usually generated by default. |
| Header fields |  | Text, long text, number, date, user, file |  |
| Line-item tables |  | Sub-list/list variable | Include row fields and calculations. |
| Lookup fields |  | Lookup control | Requires source list metadata. |
| Calculated fields |  | Calculated control/list summary | Prefer native formulas. |
| Files/attachments |  | File upload | Confirm persistence target if needed. |

For each field:

| Business field | Variable ID | Type | Required | Readonly | Default | Persisted target |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |

For each line-item table:

| Table name | List variable ID | Row field | Type | Required | Readonly | Formula | Persisted target |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |

## 3. Form UI

Request page design:

| Section | Fields/controls | Layout | Readonly/default behavior | Notes |
| --- | --- | --- | --- | --- |
| Header | Title, description | Container/text controls |  | Use v6 structured pattern. |
| Requester info | Applicant, submission date, request no. | Responsive grid | readonly defaults |  |
| Details | Business fields | Responsive grid |  | Use full-width span for long text. |
| Line items | Sub-list | Section container |  | Use readonly generated row labels if needed. |
| Action/history | workflowControlPanel, workflowHistory | Bottom container |  | Request page includes submit/save draft panel. |

Approval page design:

| Section | Fields/controls | Layout | Readonly/default behavior | Notes |
| --- | --- | --- | --- | --- |
| Header | Approval title, description | Container/text controls |  | Review-focused copy. |
| Request details | Business fields | Responsive grid | readonly |  |
| Line items | Sub-list | Section container | readonly |  |
| Action/history | workflowControlPanel, workflowHistory | Bottom container |  | Approval task behavior. |

Use the v6 baseline unless a business requirement calls for a different layout:

- top-level container
- header container
- section containers
- responsive `flex_grid`
- two columns on desktop/tablet
- one column on mobile
- full-width spans for long text and line-item tables

## 4. Validation

| Rule type | Requirement | Native Yeeflow feature | Custom code candidate? | Notes |
| --- | --- | --- | --- | --- |
| Required fields |  | Required attrs | No |  |
| Field type checks |  | Field type/control type | No |  |
| Date rules |  | Validation/formula/workflow check | Maybe | Prefer native. |
| Amount rules |  | Formula/gateway/workflow validation | No for critical rules |  |
| Row-level validation |  | Sub-list field validation | Maybe | Use custom code only if native rules cannot model it. |
| Cross-row validation |  | Native if available | Maybe | Duplicate checks may need custom code or workflow validation. |
| Duplicate checks |  | Lookup/list constraints/server validation | Maybe | Do not rely on client-only checks for critical enforcement. |

## 5. Calculations

| Calculation | Source fields | Target field | Native formula/list summary? | Notes |
| --- | --- | --- | --- | --- |
| Simple field formula |  |  |  |  |
| Row calculation |  |  |  |  |
| Table total |  |  |  |  |
| Conditional calculation |  |  |  |  |

Rule:

- Use native calculated fields and list summaries whenever possible.
- Do not put financial or approval-critical calculations in custom code unless explicitly approved and no native option exists.

## 6. Lookups

| Lookup field | Source app/listset/list | Display field | Sort field | Single/multi | Filters | Dependency status |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |

Additional field mappings:

| Lookup field | Source field name | Source field ID | Target variable | Target control readonly? | Persisted? |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

Questions:

- Does the lookup source list already exist?
- Which internal field stores the display value?
- Which source storage field is used for display, such as `Title`?
- Should results be sorted, and by which source storage field?
- Are dependent filters required?
- Are source IDs available from app metadata?
- Should selected lookup values be persisted?
- Should derived additional fields be persisted, or are they context-only?

Rules:

- Use lookup when the user should select an existing app record.
- Use choice/radio/dropdown when values are static and small.
- Confirmed approval-form lookup source type is data list.
- Single-select data-list lookup is confirmed by no multi-select flag and `RelationFieldIsMultiple: false` in additional mappings.
- Additional mappings live in `lookup.attrs.addition[]`.
- Additional mapping source fields use source `FieldName` and `FieldID`.
- Additional mapping targets workflow/form variables by `RelationName`.
- Derived target controls should usually be readonly.
- Do not guess lookup source IDs, display fields, sort fields, or additional source field IDs.

## 7. Workflow Routing

| Stage | Task name | Approver source | Approved path | Rejected path | Conditions | Page |
| --- | --- | --- | --- | --- | --- | --- |
| 1 |  | Applicant manager / user / position / group |  | EndRejectEvent |  | Approval page |
| 2 |  |  |  | EndRejectEvent |  | Approval page |

Routing design:

- Use native workflow nodes for all approval routing.
- Use `MultiAssignmentTask` for approval stages.
- Use `InclusiveGateway` or equivalent branches for conditions.
- Use explicit approved/rejected SequenceFlow conditions.
- Do not implement approval routing in custom code.

## 8. Persistence

| Persistence action | Target list | Operation | Where condition | Field mapping | Metadata required |
| --- | --- | --- | --- | --- | --- |
| Parent record |  | add/edit/remove |  |  | appId, listsetId, listId, internal field names |
| Detail rows |  | add/edit/remove |  |  | parent-child/linkage field |

Mapping table:

| Target list | Target field | Target internal name | Source variable/list row | Source type | Notes |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

Rules:

- Use `ContentList` workflow nodes for persistence.
- Parent records and detail rows should be persisted after final approval unless the business process says otherwise.
- Do not put record persistence in custom code.

## 9. AI Assistance

| AI need | Trigger | Inputs | Outputs | Form action or workflow action | Notes |
| --- | --- | --- | --- | --- | --- |
| AI summary |  |  |  |  |  |
| AI recommendation |  |  |  |  |  |
| AI risk score |  |  |  |  |  |
| AI document generation |  |  |  |  |  |

Guidance:

- Use form actions for interactive/request-page assistance.
- Use workflow AI nodes for approval-time or server-side workflow assistance.
- Declare AI agent/tool dependencies before final mode.

## 10. Custom Code Candidates

Only requirements classified as custom client-side behavior should be candidates for custom code.

| Requirement | Native alternative | Why native is insufficient | Risk level | Target fields/lists | Placement | Test plan |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  | Low / Medium / High |  | Request page / approval page |  |

Custom code rules:

- Use native options first.
- Generate code separately before embedding it.
- Explain APIs used.
- Avoid broad side effects.
- Avoid infinite loops.
- Handle empty/null states.
- Avoid running on readonly approval pages unless intended.
- Never use custom code for approval routing or persistence.

## 11. Dependencies

| Dependency | Required value | Source | Status | Placeholder if missing |
| --- | --- | --- | --- | --- |
| App ID |  | app metadata / export |  | `__APP_ID_REQUIRED__` |
| ListSet ID |  | app metadata / export |  | `__LISTSET_ID_REQUIRED__` |
| List ID |  | app metadata / export |  | `__LIST_ID_REQUIRED__` |
| Field internal names |  | app metadata / export |  | `__FIELD_INTERNAL_NAME_REQUIRED_*__` |
| Users/groups/positions |  | tenant metadata/export |  | `__POSITION_ID_REQUIRED_*__` |
| Document templates/libraries |  | app export / document library |  | `__DOCUMENT_TEMPLATE_ID_REQUIRED_*__` |
| AI agents/tools |  | Yeeflow AI metadata |  | `__AGENT_ID_REQUIRED_*__` |
| External connections |  | connection inventory |  | `__CONNECTION_ID_REQUIRED_*__` |

## 12. Generation Decision

| Decision | Answer |
| --- | --- |
| Can generate decoded Def now? | Yes / No |
| Mode | draft / final |
| Placeholders needed? | Yes / No |
| App metadata available? | Yes / No |
| Structural validator required? | Yes |
| App-context validator required? | Yes for final mode |
| Wrapper build allowed? | Yes only after validation passes and placeholders are resolved |

Blocking rules:

- Do not build final `.ywf` with unresolved placeholders.
- Do not guess app/list/field IDs.
- Do not build wrapper if decoded Def validation fails.
- Do not import into Yeeflow from Codex unless explicitly requested and allowed.

## 13. Output Format

Codex should output:

A. Requirement interpretation
B. Decomposition table
C. Native feature plan
D. Custom code decision
E. Dependency list
F. Draft generation readiness
G. Risks and assumptions

## Filled Example: Simple PR Line Item Approval

### A. Requirement Interpretation

Simple PR Line Item Approval lets an employee submit a PR note with purchase detail rows. The request goes to line manager approval and finance approval. After final approval, the workflow creates a PR Records parent item and PR Details List child rows.

### B. Decomposition Table

| Area | Decision |
| --- | --- |
| Process | Two-step approval with parent and detail-row persistence. |
| Data model | Header fields plus `PurchaseDetailsList` line-item table. |
| UI | v6 structured request and approval pages with responsive grids. |
| Validation | Required subject and required line-item amount-related fields. |
| Calculation | Row subtotal = quantity * unit price; total = sum of subtotals. |
| Lookup | None in the simple generated example. |
| Workflow | Line Manager Approval, Finance Approval, rejected paths to EndRejectEvent. |
| Persistence | `ContentList` add parent PR record, then add PR detail rows. |
| AI | None. |
| Custom code | Optional row label automation for the `No.` column. |

### C. Native Feature Plan

| Requirement | Native implementation |
| --- | --- |
| Applicant default | Current user default + readonly control attrs. |
| Submission date | Current date default + readonly control attrs. |
| Request number | FlowNo SetVariableTask. |
| Line-item table | List variable + listref + request/approval list controls. |
| Row subtotal | Native calculated row field. |
| Estimated total | Native list summary/calculated total. |
| Approval routing | Native MultiAssignmentTask nodes and SequenceFlows. |
| Persistence | Native ContentList workflow nodes. |

### D. Custom Code Decision

| Requirement | Decision |
| --- | --- |
| Auto numeric row number | Custom code justified only if native row numbering is unavailable. |
| Auto alphabetic row label | Custom code justified because native row labels did not provide `A`, `B`, `AA` behavior. |
| Row label field type | Numeric labels can use `number`; alphabetic labels must use `text`. |
| Readonly rule | Auto-generated `No.` control should be readonly. |
| Risk | Low, because it only writes a display/helper row field. |

### E. Dependency List

| Dependency | Status |
| --- | --- |
| Procurement app metadata | Available from `procurement-metadata.json`. |
| PR Records list | Resolved. |
| PR Details List | Resolved. |
| Parent field mappings | Resolved. |
| Detail-row field mappings | Resolved. |
| Finance Manager assignment pattern | Reused from the passing two-step example. |

### F. Draft Generation Readiness

Final-mode generation is allowed when:

- no placeholders remain
- `validate-ywf-def.js --mode final` passes
- `validate-ywf-def-against-yap.js --mode final --profile generator` passes
- wrapper round-trip validation passes

### G. Risks And Assumptions

- Custom code runtime behavior must be confirmed by sandbox import.
- Approval and persistence logic must remain in workflow nodes.
- If alphabetic labels are persisted later, target list field type must be text-compatible.
- Production use requires sandbox import/export round-trip verification first.

## Final Rules

- Native first.
- Custom code only when justified.
- No approval routing or persistence in custom code.
- No final `.ywf` until validation passes.
- App metadata is required for final mode.
- Sandbox import/export round-trip is required before production use.

---

<a id="build-ydl-wrapper"></a>

<!-- source: docs/collection-01.md#build-ydl-wrapper -->

# build-ydl-wrapper.js

`build-ydl-wrapper.js` creates a Yeeflow `.ydl` wrapper from a final-validated decoded data-list package JSON.

It is a local package builder only. It does not import into Yeeflow, operate the UI, or modify source exports.

## Usage

```bash
node build-ydl-wrapper.js \
  ./asset-inventory-def.test-generated-ids.json \
  ./asset-inventory.test-generated-ids.ydl \
  --title "Asset Inventory" \
  --description "Sandbox generated Asset Inventory data list"
```

## Input

The input may be either:

- decoded `Resource.Data` JSON containing `Item`
- Resource-level JSON containing `Data` as a JSON string

The builder preserves the decoded data object exactly in `Resource.Data`.

## Wrapper Format

The generated `.ydl` wrapper uses:

```json
{
  "Title": "Asset Inventory",
  "Description": "Sandbox generated Asset Inventory data list",
  "IconUrl": null,
  "IsListSet": false,
  "Resource": "[______gizp______]<base64-gzip-resource-json>"
}
```

The Resource JSON contains:

- `MainListType`
- `AppID`
- `ReplaceIds`
- `ReportIds`
- `FormKeys`
- `Data`
- `SimplePortal`

## Safety Rules

The builder refuses to write a wrapper when:

- input JSON cannot be parsed
- unresolved placeholders matching `^__.*REQUIRED.*__$` remain
- `validate-ydl-list.js --mode generator --stage final` fails
- title is missing
- AppID is missing
- ListID is missing
- `Item.Defs` is missing
- `Item.Layouts` is missing
- `Item.ListDatas` is missing

## Round-Trip Validation

After writing, the builder immediately:

1. reads the generated `.ydl`
2. validates wrapper JSON
3. checks the Resource prefix
4. base64-decodes Resource
5. gunzips Resource
6. parses Resource JSON
7. parses `Resource.Data`
8. confirms decoded data equals the source decoded data package
9. confirms no placeholders remain
10. runs `validate-ydl-list.js --mode generator --stage final` against the generated `.ydl`

## Output

The command prints a JSON report:

```json
{
  "status": "pass | fail",
  "inputData": "...",
  "outputWrapper": "...",
  "title": "...",
  "errors": [],
  "warnings": [],
  "roundTrip": {
    "wrapperJsonValid": true,
    "resourcePrefixValid": true,
    "resourceBase64Valid": true,
    "resourceGunzipValid": true,
    "resourceJsonValid": true,
    "resourceDataJsonValid": true,
    "decodedEqualsSource": true,
    "placeholdersRemaining": 0,
    "finalValidationPassed": true
  }
}
```

## Important Limitation

Passing wrapper build means the local package structure round-trips and validates. It does not mean the `.ydl` is production-ready.

Generated standalone IDs are sandbox/test-only until Yeeflow import/export round-trip behavior is proven and production metadata is confirmed.

---

<a id="build-ywf-wrapper"></a>

<!-- source: docs/collection-01.md#build-ywf-wrapper -->

# Yeeflow `.ywf` Wrapper Builder

Build a Yeeflow `.ywf` wrapper from a final validated decoded `Def` JSON file, then immediately round-trip validate the wrapper.

This tool does not import into Yeeflow, operate the UI, modify the source decoded Def, or resolve placeholders. It should only be used after metadata has been applied and final decoded-Def validation has passed.

## Usage

```bash
node build-ywf-wrapper.js \
  ./travel-request-def.sandbox.json \
  ./travel-request.sandbox.ywf \
  --flow-name "Travel Request Approval" \
  --flow-key TR \
  --workflow-type 2 \
  --description "Sandbox generated Travel Request Approval"
```

## Required Inputs

- decoded Def JSON path
- output `.ywf` path
- `--flow-name`
- `--flow-key`
- `--workflow-type`
- optional `--description`

## Pre-Write Validation

The script refuses to write a wrapper if:

- the decoded Def file is not valid JSON
- `defkey` is missing
- `workflowType` is missing
- any placeholder matching `__...REQUIRED...__` remains
- `--flow-key` does not equal `def.defkey`
- `--workflow-type` does not equal `def.workflowType`

## Wrapper Shape

```json
{
  "Def": "<base64 encoded decoded Def JSON>",
  "FlowName": "<FlowName>",
  "FlowKey": "<FlowKey>",
  "WorkflowType": 2,
  "Description": "<Description or empty string>",
  "Icon": "",
  "Img": null,
  "Settings": null
}
```

## Round-Trip Validation

After writing, the script reads the `.ywf` back and checks:

- wrapper JSON is valid
- `Def` exists
- `Def` base64-decodes into valid JSON
- decoded `Def` equals the original decoded Def object
- wrapper `FlowKey` equals decoded `defkey`
- wrapper `WorkflowType` equals decoded `workflowType`
- no unresolved placeholders remain

## Output

The script prints a structured JSON report:

```json
{
  "status": "pass | fail",
  "inputDef": "./travel-request-def.sandbox.json",
  "outputWrapper": "./travel-request.sandbox.ywf",
  "flowName": "Travel Request Approval",
  "flowKey": "TR",
  "workflowType": 2,
  "errors": [],
  "warnings": [],
  "roundTrip": {
    "wrapperJsonValid": true,
    "defBase64Valid": true,
    "decodedJsonValid": true,
    "decodedEqualsSource": true,
    "flowKeyMatches": true,
    "workflowTypeMatches": true,
    "placeholdersRemaining": 0
  }
}
```

## Recommended Pipeline

```bash
node apply-ywf-metadata.js ./travel-request-def.json ./travel-request-sandbox-metadata.json ./travel-request-def.sandbox.json
node validate-ywf-def.js ./travel-request-def.sandbox.json --mode final --dependency-map ./travel-request-dependencies.json
node build-ywf-wrapper.js ./travel-request-def.sandbox.json ./travel-request.sandbox.ywf --flow-name "Travel Request Approval" --flow-key TR --workflow-type 2
```
