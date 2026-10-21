# docs collection 2

This file combines 23 documents from `docs/` for the Claude directory build. A reference to `docs/<name>.md` is the section that starts at the anchor `#<name>` below.

- [custom-code-control-decision-guide.md](#custom-code-control-decision-guide)
- [data-list-generator-operating-playbook.md](#data-list-generator-operating-playbook)
- [form-actions-phase-1-study.md](#form-actions-phase-1-study)
- [form-actions-phase-2-query-data-filter-patch-study.md](#form-actions-phase-2-query-data-filter-patch-study)
- [generated-approval-form-baseline-v6.md](#generated-approval-form-baseline-v6)
- [generated-approval-form-controls-test-baseline.md](#generated-approval-form-controls-test-baseline)
- [generated-approval-form-controls-test-v2-baseline.md](#generated-approval-form-controls-test-v2-baseline)
- [generated-approval-form-controls-test-v3-baseline.md](#generated-approval-form-controls-test-v3-baseline)
- [generated-approval-form-controls-test-v4-baseline.md](#generated-approval-form-controls-test-v4-baseline)
- [generated-approval-form-controls-test-v6-baseline.md](#generated-approval-form-controls-test-v6-baseline)
- [generated-data-list-approval-form-integration-pattern.md](#generated-data-list-approval-form-integration-pattern)
- [generated-data-list-baseline-asset-inventory-v5.md](#generated-data-list-baseline-asset-inventory-v5)
- [generated-form-actions-phase-2-query-submit-test-v1-baseline.md](#generated-form-actions-phase-2-query-submit-test-v1-baseline)
- [generated-it-hardware-capex-request-text-standard-baseline.md](#generated-it-hardware-capex-request-text-standard-baseline)
- [generated-line-item-custom-code-row-label-pattern.md](#generated-line-item-custom-code-row-label-pattern)
- [generated-related-data-lists-staged-lookup-pattern.md](#generated-related-data-lists-staged-lookup-pattern)
- [generated-yap-baseline-visitor-access-management-v5-fresh-compatible.md](#generated-yap-baseline-visitor-access-management-v5-fresh-compatible)
- [it-hardware-capex-request-runtime-v2-ui-study.md](#it-hardware-capex-request-runtime-v2-ui-study)
- [procurement-management-yap-study.md](#procurement-management-yap-study)
- [validate-ydl-against-yap.md](#validate-ydl-against-yap)
- [validate-ydl-list.md](#validate-ydl-list)
- [validate-ywf-def-against-yap.md](#validate-ywf-def-against-yap)
- [validate-ywf-def.md](#validate-ywf-def)

---

<a id="custom-code-control-decision-guide"></a>

<!-- source: docs/collection-02.md#custom-code-control-decision-guide -->

# Custom Code Control Decision Guide

This guide defines when Codex should use Yeeflow Custom Code controls in generated dashboards, approval forms, data-list custom forms, and public forms, and how to do it safely. The goal is to produce powerful, user-friendly experiences without adding custom code where native Yeeflow configuration is enough.

This guide is for the **Custom Code control** surface. Form Action Custom Code steps use a separate `execute(...)` runtime contract. See `docs/standards/collection-01.md#custom-code-form-action-step-runtime-standard`.

## 1. Purpose

Custom code controls are an advanced enhancement layer for Yeeflow pages and forms.

They can add client-side behavior that is difficult or impossible to express with standard form controls, control attributes, formulas, actions, workflow steps, lookup settings, or validation rules.

Custom code should not replace native Yeeflow configuration. Codex should use it only when standard Yeeflow capabilities are insufficient for the requirement.

## 2. Decision Hierarchy

Codex should evaluate requirements in this order:

| Layer | Use for |
| --- | --- |
| A. Standard form controls | Text input, number input, date picker, people picker, file upload, lookup, sub-list/table, rich text, workflow panel, history. |
| B. Control attrs / readonly / defaults / validation | Required fields, readonly fields, current user defaults, current date defaults, placeholders, simple validation, display options. |
| C. Calculated fields and list summaries | Row subtotal, total amount, simple arithmetic, sum/average/count summaries, cross-field calculations supported by Yeeflow formulas. |
| D. Lookup configuration and filters | Supplier selection, purchase category lookup, dependent lookup filtering, display fields, lookup value binding. |
| E. Form actions | Set variables, update form values, trigger supported client/form actions, call configured actions when the form changes or loads. |
| F. Workflow actions | Approval routing, set variable tasks, ContentList persistence, document generation, delay, server/workflow-side decisions. |
| G. AI actions | Draft summaries, approval suggestions, risk hints, generated details, extraction or classification where AI is explicitly useful. |
| H. Custom code control | Client-side interaction behavior that cannot be modeled safely with the layers above. |
| I. Form Action Custom Code step | Action-side custom logic that cannot be modeled with native form action steps. |

Custom code is the last option, not the first tool.

## 3. When Custom Code Is Appropriate

| Requirement type | Example | Why custom code may be needed | Safer alternative if available |
| --- | --- | --- | --- |
| Auto row labels | Show `A`, `B`, `C` or custom row labels in a sub-list. | Native sub-lists may not auto-fill a row label field. | Use native row index display if Yeeflow supports it. |
| Complex row-level validation | Validate row fields using custom conditions not supported by rules. | Native validation may not inspect enough row context. | Native required rules, field validation, or workflow validation. |
| Cross-row validation | Warn when duplicate items are selected across line items. | Native rules may not compare all rows. | Lookup constraints, unique source data, or server-side validation. |
| Dynamic UI hints | Show contextual help based on amount, category, or row count. | Native hints may be static only. | Static help text or conditional visibility if available. |
| Client-side formatting | Format a value while the user types. | Native input masks may not support the format. | Built-in control formatting or field type configuration. |
| Unsupported conditional behavior | Change local UI behavior based on several fields and rows. | Native rules may not support the condition shape. | Conditional visibility/read-only rules if available. |
| Custom sub-list interaction | Renumber rows after add/delete/reorder. | The list state must be inspected client-side. | Native sub-list row numbering if available. |
| Advanced input normalization | Clean, normalize, or split user input before saving to fields. | Native controls may not normalize in the needed way. | Form actions or workflow SetVariableTask. |

## 4. When Custom Code Should Not Be Used

| Requirement | Use instead | Reason |
| --- | --- | --- |
| Required fields | Control validation / required attrs | Native validation is safer and clearer. |
| Readonly fields | Control readonly attrs | Native readonly behavior is inspectable and stable. |
| Current user/default date | Control default attrs | Tested patterns exist for `currentUser` and `currentDate`. |
| Simple formulas | Calculated fields | Native formulas are easier to validate and persist. |
| Row subtotal | List row calculation | Native calculation should own business math when possible. |
| Total amount | List summary / calculated total field | Totals affect approvals and persistence, so native formulas are preferred. |
| Approval routing | Workflow graph/gateways/tasks | Routing must be server/workflow-defined, not client-side. |
| Record persistence | ContentList workflow nodes | Persistence belongs in workflow actions. |
| Lookup selection | Lookup control configuration | Lookup source, filters, and display fields should be native. |
| Document generation | GenerateDocument workflow node | Documents depend on server resources and templates. |
| AI summary generation | AI action / workflow AI node | AI behavior should be explicit and auditable. |

## 5. Business Requirement Interpretation Rules

For each business requirement, Codex should classify it before choosing an implementation layer:

- data model
- form UI
- validation
- calculation
- lookup
- workflow routing
- persistence
- AI assistance
- custom client-side behavior

Only requirements classified as custom client-side behavior should be candidates for a Custom Code control.

Requirements classified as action-side execution must be evaluated against native form action steps first. If native steps are insufficient and custom code is explicitly needed inside a form action, use the Form Action Custom Code step standard instead of this control standard.

If a requirement can be implemented as calculation, workflow routing, persistence, lookup, or validation, Codex should use the native Yeeflow model first.

## 6. Custom Code Risk Model

| Risk level | Examples | Rule |
| --- | --- | --- |
| Low risk | Display-only helpers, row labels, contextual UI hints. | Generally acceptable when native options are missing. |
| Medium risk | Input normalization, non-critical validations, dynamic visibility. | Use only with clear scope, defensive code, and sandbox testing. |
| High risk | Approval logic, financial calculations, persistence logic, security/permission logic, external API calls, overwriting critical business fields. | Do not implement with custom code unless explicitly approved and no native server/workflow alternative exists. |

High-risk logic should not be implemented with custom code by default.

## 7. Custom Code Control Structure

The learned Yeeflow custom code control structure is:

```json
{
  "type": "codein",
  "label": "Alphabetic row label automation",
  "displayLabel": true,
  "attrs": {
    "codein-script": "import * as React from 'react';\n\nexport class CodeInApplication implements CodeInComp { ... }",
    "codein-script-param": {
      "listId": "PurchaseDetailsList",
      "HTMLId": "splia_sublist_alpha_label",
      "snField": "No"
    }
  }
}
```

Key fields:

- `type: "codein"` identifies the custom code control.
- `attrs["codein-script"]` stores the source code or compiled code.
- `attrs["codein-script-param"]` stores control configuration.
- `listId` usually identifies a sub-list variable.
- `snField` identifies a target row field.
- `HTMLId` may be present for compatibility or DOM-targeted code, but not all new-style controls need it.

Placement:

- Put request-form automation controls on the request page only unless approval-page behavior is explicitly required.
- Do not run mutating code on readonly approval/task pages unless the requirement explicitly says so.

Old compiled code starts with a minified bundle such as:

```text
codeInModules=function(...)
```

New-style readable source code starts with:

```ts
import * as React from 'react';

export class CodeInApplication implements CodeInComp
```

Generated custom code should prefer new-style readable source.

Export-backed Smart Lookup Picker learning adds these placement-specific patterns:

| Context | JSON location | Output target prefix |
| --- | --- | --- |
| Dashboard page | `Item.Layouts[].LayoutInResources[0].Resource` | `__temp_` dashboard temp variables |
| Approval form | `Forms[].DefResource.pageurls[].formdef` | `__variables_` form/workflow variables |
| Data-list custom form | `Childs[].Layouts[].LayoutInResources[0].Resource` | `__list_` list fields |
| Public form | Not proven in the Smart Lookup Picker export | Requires focused proof |

The Smart Lookup Picker export stores script source directly in `attrs["codein-script"]` and parameters in `attrs["codein-script-param"]`. It does not prove a separate script library/resource ID pattern.

## 8. Custom Code Generation Rules

When Codex decides custom code is needed, it must:

- first describe why native Yeeflow options are insufficient
- generate the custom code separately before embedding it into a Def
- explain the Yeeflow APIs used
- define all parameters and defaults
- keep the behavior narrowly scoped
- avoid broad side effects
- avoid infinite update loops
- handle empty, null, and missing state safely
- avoid running on readonly approval pages unless intended
- preserve user-entered values unless explicitly modifying a target field
- keep generated code readable and editable
- include sandbox test cases

For code generation, use:

- `export class CodeInApplication implements CodeInComp`
- `description()`
- `inputParameters()`
- `requiredFields(params)`
- `render(context, fieldsValues, readonly)`
- `React.Component` for broad runtime compatibility

Do not require `render(...)` for Form Action Custom Code steps. Those scripts require `execute(context, fieldsValues)` and are governed by `docs/standards/collection-01.md#custom-code-form-action-step-runtime-standard`.

## 9. Validation Rules

Recommended validator checks:

- detect all `codein` controls
- verify referenced list variables exist
- verify target row fields exist
- check field type compatibility
- check readonly behavior when code auto-generates values
- warn when code modifies critical fields
- warn when custom code exists without a declared reason
- warn when custom code uses unknown APIs
- warn or fail when custom code includes external API calls
- warn when old compiled code appears in a generated new-style template
- fail final-mode validation if custom code references unresolved placeholders
- fail when a Custom Code control script has no `render(...)`
- fail when a Form Action Custom Code step script has no `execute(...)`
- warn when a Custom Code control is missing a script reference or embedded script
- warn when required input parameters declared by the script are missing
- warn when parameter names are duplicated or not declared by the script
- warn when a public-form control uses list queries without explicit runtime proof

For sub-list row-label custom code:

- `listId` must resolve to a list variable.
- `snField` must resolve to a row field in the related listref.
- alphabetic labels require a text-compatible row field.
- generated row label controls should be readonly.

## 10. Output Requirements For Future Generated Forms

When Codex uses custom code, it should output:

A. Requirement being solved  
B. Why native Yeeflow configuration is insufficient  
C. Custom code design  
D. Code  
E. Parameters  
F. Def changes  
G. Validation report  
H. Test plan  
I. Risks and fallback

Codex should not hide custom code inside a generated `.ywf` without first explaining and validating it.

## 11. Examples

### Auto Row Number / Row Label

Custom code justified: yes, when Yeeflow does not provide native row numbering for the desired format.

Native alternative: built-in row index display, if available.

Recommended approach:

- Use a `codein` control on the request page.
- Read the sub-list rows from `fieldsValues[listId]`.
- Write only the configured row label field.
- Make the target row field readonly.
- Use `number` for numeric labels and `text` for alphabetic labels.

### Cross-Row Duplicate Item Warning

Custom code justified: sometimes.

Native alternative: lookup source constraints, unique item rules, or workflow validation if available.

Recommended approach:

- Use custom code only for a non-blocking or client-side warning unless blocking behavior is explicitly tested.
- Do not rely on client-side code as the only enforcement for critical procurement rules.

### Dynamic Help Text Based On Amount

Custom code justified: low risk if display-only.

Native alternative: static help text, conditional visibility, or AI-generated guidance if supported.

Recommended approach:

- Use a display-only `codein` control.
- Read the amount field.
- Render a small hint.
- Do not modify approval routing or financial values.

### Advanced Input Formatting

Custom code justified: sometimes.

Native alternative: native control formatting, masks, or validation rules.

Recommended approach:

- Normalize only the intended target field.
- Preserve the user's value where possible.
- Clearly document whether formatting happens during typing, on blur, or before submit.

## 12. Final Rules

- Prefer native Yeeflow features.
- Use custom code only for interaction behavior that cannot be modeled natively.
- Never put approval routing in custom code.
- Never put persistence logic in custom code.
- Avoid putting financial calculations in custom code unless explicitly approved.
- Always validate generated Def structure.
- Always test custom code in a sandbox.
- Always keep custom code readable and narrowly scoped.

---

<a id="data-list-generator-operating-playbook"></a>

<!-- source: docs/collection-02.md#data-list-generator-operating-playbook -->

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

### Native Title Field Rule

Every generated Yeeflow data list must preserve `Title` as the native primary/display field. Do not generate `Title` as an ordinary active custom field.

Required generated pattern:

```json
{
  "FieldName": "Title",
  "Status": 0,
  "IsSystem": true,
  "IsIndex": true
}
```

The display label may still be business-specific, such as `Request No.`, `Name`, `Equipment Name`, or `Center / Department Name`. The underlying metadata must remain native/system/indexed. Additional business text fields should use `Text1`, `Text2`, and later text slots.

Heep Hong IT eWorkflow Option A v7 proved the failure mode: generated child lists with `Title` as `Status: 1`, `IsSystem: false`, and `IsIndex: false` imported and exposed metadata successfully, but Yeeflow grid row loading failed at `api/crafts/datas/{AppID}/{ListID}/query` with HTTP `400`. Option A v8 fixed the data-grid query by restoring the native `Title` pattern.

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

<a id="form-actions-phase-1-study"></a>

<!-- source: docs/collection-02.md#form-actions-phase-1-study -->

# Yeeflow Form Actions Phase 1 Study

Source export studied read-only: `<downloads>/Expression Sublist Summary Workflow Test v1.yap`.

Focus form: `Expression Sublist Summary Workflow Test v1`.

Focus submission page: `Submit Purchase Line Items`.

## Summary

The manual export adds a first export-backed form action pattern on an approval form submission page. The page stores action definitions directly on `page.formdef.actions`, page-load wiring at `page.formdef.formAction.onLoad`, temp variable references at `variables.tempVars`, and button action wiring at `action_button.attrs.control_action`.

This is Phase 1 coverage only. It proves the exported structures for action buttons, page-load actions, temp variable references, `setvar` steps, and `confirm` steps. It also contains one `listitem` action step, but that step should be treated as out of scope for Phase 1 generation until a dedicated list-item form action test is run.

## Button Style Patterns

All studied button controls use:

- control type: `action_button`
- inline width: `attrs.common.positioning.widthtype = [null, "2"]`
- label text at `label`
- style code at `attrs["button-style"]`
- optional icon at `attrs.icon`
- optional icon type at `attrs["icon-type"]`
- optional icon position at `attrs["icon-posi"]`

Observed style codes:

| Pattern | Label Example | Button Style | Notes |
| --- | --- | --- | --- |
| Primary | `Start`, `Click button action` | `"2"` | Use for primary command or main action button. |
| Soft secondary | `On hold`, `Set default request title` | `"3"` | Use for secondary action with filled but softer treatment. |
| Outline primary | `Save` | `"4"` | Use for save/confirm-like action where outline is preferred. |
| Neutral outline | `Verify` | `"5"` | Use for verification/check commands. |
| Dashed/utility | `Import`, `New item`, `Next` | `"6"` | Use for import/add/next utility commands; add icons when helpful. |

Observed icon buttons:

- `New item`: `attrs.icon = "fa-regular fa-plus"`, `attrs["icon-type"] = "3"`, `attrs["icon-posi"] = "1"`
- `Next`: `attrs.icon = "fa-regular fa-arrow-right"`, `attrs["icon-type"] = "3"`, `attrs["icon-posi"] = "2"`

Generation rules:

- Use inline width by default for form action buttons.
- Use business-readable labels.
- Add meaningful `nv_label` names even when the studied export omitted them.
- Only add `attrs.control_action` when the referenced form action exists.

## Button Click Action Trigger

Button click wiring is direct:

```json
{
  "type": "action_button",
  "attrs": {
    "control_action": "05bc684c-e435-44a5-818e-b1a953175385"
  }
}
```

The target action is a sibling entry in `page.formdef.actions[]` with the same `id`.

Observed button-triggered actions:

| Button Label | Action Name | Action ID | Main Step Type |
| --- | --- | --- | --- |
| `Click button action` | `change value1` | `05bc684c-e435-44a5-818e-b1a953175385` | `setvar` |
| `Set default request title` | `Change request title` | `34df076d-d801-4159-840c-48cb1e7533f7` | `setvar` |
| `Save` | `Show confirmation dialog` | `c988aa6a-7505-4bb5-b405-b24e955e6e15` | `confirm` |
| `New item` | `Add new data list item` | `51aec2a8-3d41-4c64-a47e-c231626e7184` | `listitem` |

## Page Load Action Trigger

Page-load wiring is stored on the page form definition:

```json
{
  "formAction": {
    "onLoad": "28687bbc-31cc-443c-8285-b0ac7dfbbfc0"
  }
}
```

The target action exists in `page.formdef.actions[]`.

Observed use case:

- action name: `Page Load Action`
- step name: `Set default values`
- step type: `setvar`
- sets multiple workflow variables using `attrs.setvar_multi = true` and `attrs.setvar_array[]`

Generation rules:

- Use page-load actions for safe initialization only.
- Common cases: set default title, initialize status text, prefill notes, initialize temp variables, or prefill current-user information.
- Do not use page-load actions for persisted business side effects unless a later export proves that behavior safe.

## Temp Variables

Temp variables are referenced in the top-level form variable collection:

```json
{
  "variables": {
    "tempVars": [
      { "idx": "8b87913a-c5e0-48d6-9df9-453dece7d7c9", "id": "var_Value1" },
      { "idx": "4b55c928-5621-4d37-a0b8-0672608977c5", "id": "var_StartBtnClickCount" },
      { "idx": "a32c09de-6d10-465e-bb7d-27d5adad1aa7", "id": "var_DialogResult" }
    ]
  }
}
```

Expression tokens reference them using an `__temp_` prefix:

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

- Define the logical temp variable in `variables.tempVars[]`.
- Reference it in expressions as `__temp_${id}`.
- Treat temp variables as front-end form runtime state.
- Do not rely on temp variables for persisted business data unless copied into workflow variables or persisted by an explicit action.

## Form Action Structure

Observed action shape:

```json
{
  "id": "uuid",
  "name": "Business readable action name",
  "steps": []
}
```

Steps run in array order. A step may include:

- `name`
- `condition`
- `continue`
- `attrs`

Observed Phase 1 step types:

- `setvar`
- `confirm`

Observed but deferred step type:

- `listitem`

## Set Variable Step

Single target shape:

```json
{
  "type": "setvar",
  "name": "Set request title",
  "condition": [/* expression tokens */],
  "attrs": {
    "setvar_var": { "exprType": "variable", "valueType": "text", "id": "RequestTitle", "type": "expr", "name": "Workflow Variables:Request Title" },
    "setvar_val": [/* expression tokens */]
  }
}
```

Multi target shape:

```json
{
  "type": "setvar",
  "name": "Set default values",
  "attrs": {
    "setvar_multi": true,
    "setvar_array": [
      {
        "var": { "exprType": "variable", "valueType": "number", "id": "AverageUnitPrice", "type": "expr", "name": "Workflow Variables:Average Unit Price" },
        "value": [{ "type": "num", "value": "100" }]
      }
    ]
  }
}
```

Generation rules:

- Use single-target shape for one value.
- Use `setvar_multi = true` plus `setvar_array[]` when setting multiple values on page load.
- Use expression-token arrays for values.
- Use optional `condition` expression tokens to guard a step.
- Use `continue: true` only when later steps should continue after the condition branch.

## Show Confirm Dialog Step

Observed shape:

```json
{
  "type": "confirm",
  "attrs": {
    "confirm_qs": [
      { "type": "str", "value": "This is an important operation, please confirm!" }
    ],
    "confirm_rs": {
      "exprType": "variable",
      "valueType": "string",
      "id": "__temp_var_DialogResult",
      "type": "expr",
      "name": "var_DialogResult"
    }
  }
}
```

Generation rules:

- Use `confirm_qs` for the message expression tokens.
- Use `confirm_rs` to store the result in a temp variable.
- Display the result through a Text/heading control using the learned Text control pattern and an expression in `attrs.headc.title.variable`.
- Confirm result value shape needs a focused runtime test before promising boolean vs text semantics in generated apps.

## Form Actions vs Workflow Actions

Form actions are front-end designer/runtime logic attached to form pages and controls.

Workflow actions are backend process graph nodes such as ContentList, SetVariableTask, assignment tasks, integrations, and workflow transitions.

Temp variables are form-runtime state. Workflow variables are process/form data and can be used for workflow routing and ContentList persistence.

## Validator Recommendations

Warning-level checks are safe now for:

- action button references missing form action
- page load action references missing form action
- action with empty steps
- `setvar` without target/value
- `confirm` without message or result target
- temp variable expression target missing from `variables.tempVars`
- action button triggering a form action without meaningful `nv_label`

Hard errors should wait until a generated Phase 1 runtime test proves the minimum action shape for generated packages.

## Generation Status

The export proves structure, but this pass does not yet prove a Codex-generated form-action package. Generate `Form Actions Phase 1 Test v1` next using the plan/spec created in this branch.

---

<a id="form-actions-phase-2-query-data-filter-patch-study"></a>

<!-- source: docs/collection-02.md#form-actions-phase-2-query-data-filter-patch-study -->

# Form Actions Phase 2 Query Data Filter Patch Study

## Source

Read-only export:

`<downloads>/Form Actions Phase 2 Query Submit Test v1 Filter Patch.yap`

The app was manually updated in the designer after the first runtime baseline. The user set the Query data step's `Data filter -> Condition` to:

```text
Active Equals ON
```

Runtime result after manual patch: the multiple query returned only active rows (`SRC-002`, `SRC-001`) and Loaded Count became `2`.

## Root Cause

The generated package wrote a singular filter helper:

```json
"querydata_filter": [
  {
    "left": "Bit1",
    "op": "0",
    "right": "ON"
  }
]
```

That property stayed in the patched export, but it did not populate the designer's Query data `Data filter -> Condition` model and runtime ignored it.

## Working Export-Backed Property

The patched export adds a plural property on the same Query data step:

```json
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
```

Important details:

- Property name is `querydata_filters`, plural.
- `left` uses the source list field name, such as `Bit1`.
- `op: "0"` is the exported Equals operator.
- Bit/Yes-No ON exports as `right: "true"`, not `right: "ON"`.
- `key` is a UUID-like condition id and should be generated fresh.
- `pre: "and"` and `showCus: true` are present on the working condition.

## Generation Rule

For Query data step Data filters:

- Generate `attrs.querydata_filters`.
- Do not rely on `attrs.querydata_filter`.
- For an Active Bit field, generate:

```json
{
  "key": "<fresh-guid>",
  "pre": "and",
  "left": "Bit1",
  "op": "0",
  "right": "true",
  "showCus": true
}
```

## Runtime Status

Proven by the user-confirmed patched runtime:

- Query data filters work when configured through the Query data step `Data filter -> Condition`.
- `Active Equals ON` filters the source list to active rows only.

The original generated test remains useful as a root-cause baseline: it proves the singular `querydata_filter` path is insufficient.

---

<a id="generated-approval-form-baseline-v6"></a>

<!-- source: docs/collection-02.md#generated-approval-form-baseline-v6 -->

# Generated Approval Form Baseline V6

This document records the current baseline for AI-generated Yeeflow approval form `.ywf` packages, proven through the Simple PR Note Approval v6 iteration.

The baseline artifact is:

- Decoded Def: `simple-pr-note-approval-def.v6.json`
- Wrapper: `simple-pr-note-approval.v6.ywf`

## 1. What V6 Proves

V6 proves that Codex can generate a decoded Yeeflow approval form Def that:

- imports successfully as a `.ywf` wrapper
- publishes successfully
- renders a request form and an approval task form
- includes publish-ready page metadata
- includes connected workflow designer nodes and arrows
- uses a cleaner workflow layout
- defaults Applicant to the current user
- defaults Submission Date to the current date
- includes request-page Save draft / Submit actions
- includes request and approval workflow history
- writes approved data to an existing Yeeflow data list through `ContentList`
- validates structurally and against extracted `.yap` metadata

This is the minimum baseline for future generated approval forms.

## 2. Required Decoded Def Structure

A generated decoded Def must include:

- `defkey`, matching wrapper `FlowKey`
- `workflowType: 2`
- `variables.basic`
- `variables.listref`
- `variables.filter`
- `variables.tempVars`
- `pageurls`
- `childshapes`
- `graphposition`
- `graphzoom`
- `lineType`

Required page types:

- request page: `pageurls[].type === 1`
- approval page: `pageurls[].type === 2`

Required workflow node classes for a simple one-step approval:

- `StartNoneEvent`
- `SetVariableTask`
- `MultiAssignmentTask`
- `ContentList`
- `EndNoneEvent`
- `EndRejectEvent`
- `SequenceFlow`

## 3. Request Page UI Pattern

The request page should use one top-level container with three sections:

```text
Request page
└─ top-level container
   ├─ header container
   │  ├─ form title heading
   │  └─ form description heading
   ├─ fields container
   │  └─ responsive flex_grid
   │     ├─ Applicant
   │     ├─ Submission Date
   │     ├─ Request No.
   │     ├─ Estimated Amount
   │     ├─ Subject
   │     └─ Reason
   └─ action/history container
      ├─ workflowControlPanel
      └─ workflowHistory
```

Recommended page background:

```json
{
  "background": {
    "type": "classic",
    "classic": {
      "color": "var(--c--neutral-light)"
    }
  }
}
```

Use conservative neutral design tokens by default. Avoid hardcoded decorative colors unless the user asks for custom styling.

## 4. Approval Page UI Pattern

The approval page should mirror the request page structure:

```text
Approval page
└─ top-level container
   ├─ header container
   │  ├─ approval title heading
   │  └─ approval description heading
   ├─ fields container
   │  └─ responsive flex_grid
   │     ├─ Applicant readonly
   │     ├─ Submission Date readonly
   │     ├─ Request No. readonly
   │     ├─ Estimated Amount readonly
   │     ├─ Subject readonly
   │     └─ Reason readonly
   └─ action/history container
      ├─ workflowControlPanel
      └─ workflowHistory
```

Do not use request-only wording on the approval page. The approval page action panel is for task completion, not Save draft / Submit.

## 5. Applicant Current-User Pattern

Request Applicant control:

```json
{
  "type": "identity-picker",
  "label": "Applicant",
  "binding": "Applicant",
  "readonly": true,
  "value": "CurrentUser",
  "attrs": {
    "default": "currentUser"
  },
  "displayLabel": [null, true]
}
```

Generator rule:

- use this pattern for requester/applicant fields by default
- preserve the `Applicant` variable as a user/identity variable
- do not make Applicant editable unless explicitly requested

## 6. Submission Date Current-Date Pattern

Request Submission Date control:

```json
{
  "type": "datepicker",
  "label": "Submission Date",
  "binding": "SubmissionDate",
  "readonly": true,
  "attrs": {
    "default": "currentDate",
    "showtime": true,
    "dateformat": "0",
    "date_type": "0"
  },
  "displayLabel": [null, true]
}
```

Generator rule:

- prefer `attrs.default: "currentDate"` over hardcoded static dates
- avoid static timestamp values in generated drafts
- preserve readonly behavior unless explicitly requested otherwise

## 7. Workflow Graph Layout Pattern

Simple one-step approval layout:

```json
{
  "graphposition": {
    "x": 220,
    "y": 186,
    "width": 1130,
    "height": 355
  },
  "positions": {
    "Start": { "x": -85, "y": -10 },
    "SetVariableTask": { "x": 215, "y": -10 },
    "ApprovalTask": { "x": 215, "y": 135 },
    "ContentList": { "x": 550, "y": 135 },
    "EndNoneEvent": { "x": 835, "y": 135 },
    "EndRejectEvent": { "x": 215, "y": 285 }
  }
}
```

SequenceFlow rules:

- every SequenceFlow has `id === resourceid`
- every SequenceFlow has explicit `source` and `target`
- every source node lists the flow in `outgoing`
- every target node lists the flow in `incoming`
- straight paths may omit `vertices`
- direct rejection path can use `vertices: []`

Recommended simple flow:

```text
Start -> Set Request No.
Set Request No. -> Approval
Approval -> Approved path -> ContentList -> End
Approval -> Rejected path -> EndRejectEvent
```

## 8. Page Registration / Publish Metadata Pattern

Each `pageurls[]` entry must include:

```json
{
  "id": "<uuid>",
  "type": 1,
  "name": "",
  "title": "Request page title",
  "pagetype": 1,
  "formdef": {
    "pagetype": 1,
    "ver": 2,
    "title": "Request page title",
    "exts": [],
    "actions": [],
    "name": ""
  }
}
```

Approval page:

```json
{
  "id": "<uuid>",
  "type": 2,
  "title": "Approval page title",
  "pagetype": 1,
  "formdef": {
    "pagetype": 2,
    "ver": 2,
    "title": "Approval page title",
    "exts": [],
    "actions": [],
    "name": ""
  }
}
```

Task URL rules:

- `StartNoneEvent.properties.taskurl` points to the request page `id`
- `MultiAssignmentTask.properties.taskurl` points to the approval page `id`
- use UUID-shaped page IDs
- do not use `FlowKey + "1"` or `FlowKey + "2"` as page IDs

## 9. ContentList Mapping Pattern

Approved persistence uses a `ContentList` node.

Required properties:

- `type: "add"` for create
- `appid`
- `listsetid`
- `listid`
- `listtype`
- `listdatas`
- `wheres: []`

Workflow variable expression format:

```html
<input type="button" data="${&quot;type&quot;:&quot;variable&quot;, &quot;param&quot;:{&quot;id&quot;:&quot;RequestNo&quot;}}" expr="__" tabindex="-1" value="Workflow Variables:Request No.">
```

Generator rules:

- target `Columns` values must be real internal field names
- source variable IDs must exist in `variables.basic`
- list IDs and field names must come from metadata or explicit user mapping
- never guess app/list/field identifiers

## 10. Validation Pipeline

Before wrapping:

```bash
node validate-ywf-def.js ./simple-pr-note-approval-def.v6.json --mode final
node validate-ywf-def-against-yap.js ./simple-pr-note-approval-def.v6.json ./procurement-metadata.json --mode final --profile generator
```

Before handoff:

```bash
node build-ywf-wrapper.js \
  ./simple-pr-note-approval-def.v6.json \
  ./simple-pr-note-approval.v6.ywf \
  --flow-name "Simple PR Note Approval" \
  --flow-key SPRNA \
  --workflow-type 2 \
  --description "Sandbox generated simple PR note approval - v6 structured responsive form UI"
```

Expected validator result:

- structural validation: `pass`
- app-context validation: `pass`
- wrapper round-trip: `pass`
- placeholders remaining: `0`

## 11. Known Constraints

- V6 is proven for a simple one-step approval workflow.
- Data-list lookup controls with additional field auto-fill are now proven separately by the Simple PR Line Item Approval lookup study, not by the original v6 baseline.
- Full app-level `.yap` generation is not proven.
- Complex line-item table persistence needs additional import/export round-trip testing.
- Document generation nodes need separate sandbox validation.
- AI action nodes need separate sandbox validation.
- Yeeflow may suffix flow keys on repeated imports, such as `SPRNA2`, `SPRNA3`, or `SPRNA4`.
- Static export-time date values should not be copied into generated templates.
- Style settings should stay conservative until a formal Yeeflow design system mapping is defined.

## 12. Rules For Future Generated Approval Forms

- Generate a normalized approval spec before generating Yeeflow Def JSON.
- Generate decoded Def JSON first; do not generate `.ywf` directly from business text.
- Use placeholders only in draft mode.
- Final mode must contain no unresolved placeholders.
- Use UUID-shaped page IDs and generated node IDs.
- Keep `FlowKey` and `defkey` equal.
- Always include request and approval pages for approval forms.
- Always include request `workflowControlPanel`.
- Include request `workflowHistory` by default.
- Always include approval `workflowControlPanel` and `workflowHistory`.
- Use current-user Applicant and current-date Submission Date by default.
- Use structured container/header/grid UI for both request and approval pages.
- Use two-column desktop/tablet grid and one-column mobile grid.
- Use full-width spans for long text fields.
- Use lookup controls for existing app records only when source metadata is confirmed.
- Use readonly target controls for lookup-derived additional fields by default.
- Preserve workflow graph visual metadata.
- Validate structurally and against app metadata before wrapping.
- Never guess Yeeflow app/list/field/approver/template IDs.

## 13. What To Test After Import

After manual sandbox import:

- form imports without JSON/package errors
- request page opens
- Applicant defaults to current user and is readonly
- Submission Date defaults to current date and is readonly
- Request No. is readonly and populated after workflow number assignment
- Save as draft and Submit actions appear on request page
- request page workflow history appears
- approval task page opens from assigned task
- approval page fields are readonly
- approval page action panel works
- approval page workflow history appears
- workflow designer nodes and arrows are connected
- workflow designer layout is readable
- publish succeeds
- submit creates an approval instance
- approval path creates the expected PR Records item
- rejection path ends rejected
- no unexpected unmapped placeholders or missing resource errors appear

---

<a id="generated-approval-form-controls-test-baseline"></a>

<!-- source: docs/collection-02.md#generated-approval-form-controls-test-baseline -->

# Generated Approval Form Controls Test Baseline

Date: 2026-05-14

## Package

- App name: Approval Form Controls Test
- Generator: `tools/generators/generate-approval-form-controls-test-v1.mjs`
- Decoded app definition: `approval-form-controls-test-app-def.v1.json`
- Approval form definition: `approval-form-controls-test-approval-form-def.v1.json`
- Data list definition: `approval-form-controls-test-list-def.v1.json`
- Generated package: `Approval Form Controls Test v1.generated.yap`
- User-accessible copy: `<downloads>/Approval Form Controls Test v1.generated.yap`
- ID family: `451...`
- FlowKey/form key: `AFC1`
- Source baseline: `design-system-request-tracker-app-def.v1.json`

Generated artifacts are kept out of the Git baseline unless explicitly allowlisted. The generator script and this runtime baseline are the tracked sources of truth.

## What This Proves

This package validates that the AI Training approval-control learning can produce a small working Yeeflow app with representative native controls, a data list, an app-level approval form, a simple workflow, and `ContentList` persistence.

The app follows the current Yeeflow generation rules:

- Fresh local ID family.
- Fresh FlowKey/form key.
- `Data.Forms[].ListID = 0`.
- Native Title metadata preserved on the data list.
- Page-level background rule applied.
- `Main`, `Content`, `Form body`, and `Form bottom` structure used.
- Learned Text control standard used for display text.
- Normal controls grouped in two-column grids.
- Long controls placed full-row.
- Meaningful `nv_label` values used.
- Action Panel and Flow History included in the form bottom.

## Included Controls

The v1 proof package includes:

- `input`
- `textarea`
- `richtext`
- `input_number`
- `currency`
- `percent`
- `radio`
- `checkbox`
- `switch`
- `datepicker`
- `daterange`
- `file-upload`
- `identity-picker`
- internal `lookup`
- `calculated`
- `workflowControlPanel`
- `workflowHistory`

Runtime-sensitive controls intentionally included for proof:

- `richtext`
- `checkbox`
- `percent`
- `file-upload`
- `identity-picker`
- `lookup`
- `calculated`

Deferred controls:

- `icon-upload`
- `organization-picker`
- `location-picker`
- `cost-center-picker`
- `metadata`
- `mutiple-metadata`
- `lookup-list`
- `list/sublist`

## Validation Results

Local checks passed:

- `node --check tools/generators/generate-approval-form-controls-test-v1.mjs`
- `node --check validate-yap-package.js`
- `node --check validate-yap-graph.js`
- `node --check validate-ywf-def.js`
- `node --check validate-ydl-list.js`
- `node --check workflow-action-config-validator.js`

Package and component validation:

- `validate-yap-package.js approval-form-controls-test-app-def.v1.json --mode generator --stage final`: `pass_with_warnings`, 0 errors, 19 warnings.
- `validate-yap-graph.js approval-form-controls-test-app-def.v1.json --mode generator --stage final`: `pass`, 0 errors, 0 warnings.
- `validate-ywf-def.js approval-form-controls-test-approval-form-def.v1.json --mode final`: `pass_with_warnings`, 0 errors, 18 warnings.
- `validate-ydl-list.js approval-form-controls-test-list-def.v1.json --mode generator --stage final`: `pass_with_warnings`, 0 errors, 2 warnings.
- `workflow-action-config-validator.js`: `pass`, 9 checked nodes, 0 errors, 0 warnings.

Wrapper build and round-trip validation:

- `build-yap-wrapper.js`: `pass`.
- Wrapper JSON, resource prefix, base64, gzip, resource JSON, and `Resource.Data` validation all passed.
- Decoded wrapper content matched the source app definition.
- Wrapped package validation: `pass_with_warnings`, 0 errors, 19 warnings.
- Wrapped graph validation: `pass`, 0 errors, 0 warnings.

The remaining warnings are expected for this proof package because it intentionally includes schema-supported controls that were selected for runtime validation.

## Runtime Result

Runtime testing was completed in `https://<yourdomain>.yeeflow.com/`.

Verified:

- App imported successfully.
- Imported app opened successfully.
- `Overview` app page rendered.
- `Control Test Requests` data list opened without `datas/query` 400.
- Seed sample row rendered.
- `Approval Form Controls Test` approval form opened.
- Representative controls rendered, including rich text editor, percent, checkbox, switch, datepicker, daterange, file upload, identity picker, internal lookup, and calculated subtotal.
- Submitted runtime test request:
  - Request Title: `Approval controls runtime test`
  - Quantity: `3`
  - Unit Price: `12`
  - Required Options: `Laptop`
  - Calculated Sub total updated to `36`
- Submit completed and showed the Yeeflow success state.
- Pending approval task opened.
- Review task page rendered readonly submitted values, Action Panel, and Flow History.
- Approval completed successfully.
- Pending task list became empty.
- `ContentList` created a persisted data-list record.
- Persisted record appeared in `Control Test Requests` with:
  - Request Title: `Approval controls runtime test`
  - Required Options: `["Laptop"]`
  - Active Request: `OFF`
  - Quantity: `3.00`
  - Sub total: `36.00`
  - Needed By: `14/05/2026`

Runtime status: passed.

## Known Gaps

- The checkbox field persisted as a JSON-like array display (`["Laptop"]`) in the list view. This is runtime-valid but should be studied further if a cleaner list-view display format is required.
- `Priority`, `Confidence`, `Service Window`, and `Related Record` were rendered but not filled in the runtime submission, so this baseline proves rendering and workflow compatibility, not every value-shape persistence path.
- File upload rendered safely, but no file was uploaded in this run.
- Organization, location, cost center, metadata, lookup-list, and sublist patterns remain deferred until their export-backed runtime patterns are proven.

## Generator Rules Confirmed

- Use the AI Training learned control shapes for representative native controls.
- Keep runtime-sensitive controls in warning mode until repeated generated baselines prove them broadly safe.
- Use calculated controls for formula-like fields such as subtotal.
- Use an internal packaged list for lookup proof when no external dependency is needed.
- Preserve the validation-plus-runtime loop before promoting a control type from schema-supported to generation-safe.

---

<a id="generated-approval-form-controls-test-v2-baseline"></a>

<!-- source: docs/collection-02.md#generated-approval-form-controls-test-v2-baseline -->

# Generated Approval Form Controls Test v2 Baseline

Date: 2026-05-14

## Package

- App name: Approval Form Controls Test v2
- Generator: `tools/generators/generate-approval-form-controls-test-v2-advanced-inputs.mjs`
- Decoded app definition: `approval-form-controls-test-v2-app-def.json`
- Approval form definition: `approval-form-controls-test-v2-approval-form-def.json`
- Data list definition: `approval-form-controls-test-v2-list-def.json`
- Generated package: `Approval Form Controls Test v2.generated.yap`
- User-accessible copy: `<downloads>/Approval Form Controls Test v2.generated.yap`
- ID family: `452...`
- FlowKey/form key: `AFC2`

Generated `.yap` and decoded package JSON artifacts stay out of Git. This document and the generator script are the tracked baseline.

## What This Proves

This stage proves the advanced input control batch after the successful v1 representative-control baseline.

Controls proved:

- `percent`
- `time`
- `hyperlink`
- `rate`
- `calculated`

Partially proved:

- `daterange`: from/to values rendered, accepted input, and appeared on the approval task. The generated list view did not expose both mapped date fields, so list-view persistence remains a follow-up proof.

## Validation Results

Local checks passed:

- `node --check tools/generators/generate-approval-form-controls-test-v2-advanced-inputs.mjs`
- `node --check validate-yap-package.js`
- `node --check validate-yap-graph.js`
- `node --check validate-ywf-def.js`
- `node --check validate-ydl-list.js`
- `node --check workflow-action-config-validator.js`
- JSON parse checks for `fixtures/runtime-test-specs/approval-form-control-runtime-coverage.json` and `fixtures/runtime-test-specs/approval-form-controls-runtime-test-spec.json`

Package and component validation:

- `validate-yap-package.js approval-form-controls-test-v2-app-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors, 31 warnings.
- `validate-yap-graph.js approval-form-controls-test-v2-app-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors, 4 warnings.
- `validate-ywf-def.js approval-form-controls-test-v2-approval-form-def.json --mode final`: `pass_with_warnings`, 0 errors, 18 warnings.
- `validate-ydl-list.js approval-form-controls-test-v2-list-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors, 6 warnings.
- `workflow-action-config-validator.js`: `pass`, 9 checked nodes, 0 errors, 0 warnings.

Wrapper build and round-trip validation:

- `build-yap-wrapper.js`: `pass`.
- Wrapper JSON, resource prefix, base64, gzip, resource JSON, and `Resource.Data` validation all passed.
- Decoded wrapper content matched the source app definition.
- Wrapped package validation: `pass_with_warnings`, 0 errors, 31 warnings.
- Wrapped graph validation: `pass_with_warnings`, 0 errors, 4 warnings.

Warnings are expected because this stage intentionally exercises schema-supported controls that were not previously runtime-proven.

## Runtime Result

Runtime testing was completed in `https://<yourdomain>.yeeflow.com/`.

Verified:

- App imported successfully.
- Imported app opened successfully.
- `Overview` app page rendered.
- `Advanced Input Test Requests` data list opened without `datas/query` 400.
- Seed sample row rendered.
- `Approval Form Controls Test v2` approval form opened.
- Percent, date range, time, hyperlink, rate, and calculated controls rendered.
- Submitted runtime test request:
  - Request Title: `Advanced inputs runtime test`
  - Base Amount: `200`
  - Adjustment Percent: `25.00%`
  - Calculated Score updated to `50`
  - Satisfaction Rate: `3.5`
  - Preferred Time: `10:30:00`
  - Follow-up Window: `21/05/2026` to `22/05/2026`
  - Reference Link: `https://example.com/runtime-test`
  - Notes: `Runtime test for advanced input controls.`
- Submit completed and showed the Yeeflow success state.
- Pending approval task opened.
- Review task page rendered readonly submitted values, Action Panel, and Flow History.
- Approval completed successfully.
- Pending task list became empty.
- `ContentList` created a persisted data-list record.
- Persisted record appeared in `Advanced Input Test Requests` with:
  - Request Title: `Advanced inputs runtime test`
  - Base Amount: `200.00`
  - Adjustment Percent: `25.00%`
  - Calculated Score: `50.00`
  - Satisfaction Rate: `3.5`
  - Preferred Time: `10:30:00`
  - Reference Link: `Open link`

Runtime status: passed for Stage 2, with date range persistence visibility kept as a follow-up check.

## Generator Rules Confirmed

- Percent values can be generated with the learned percent control shape and mapped to Decimal fields.
- Time values can be generated with the learned time control shape and mapped to Datetime fields for simple time display.
- Hyperlink controls can accept a URL, render as an open link on the task page, and persist as a list-view link.
- Rate controls can generate a star rating and persist a numeric value.
- Calculated controls can safely reference a numeric source and a percent source; Yeeflow evaluated `200 * 25%` as `50`.
- Date range controls should continue to be generated cautiously until both mapped date fields are visible and verified in a list-view proof.

## Next Stage

Proceed to Stage 3 only after this baseline is committed:

- `file-upload`
- `icon-upload`

Use a small safe file and keep upload/media controls isolated from other unproven control families.

---

<a id="generated-approval-form-controls-test-v3-baseline"></a>

<!-- source: docs/collection-02.md#generated-approval-form-controls-test-v3-baseline -->

# Generated Approval Form Controls Test v3 Baseline

Date: 2026-05-14

## Package

- App name: Approval Form Controls Test v3
- Generator: `tools/generators/generate-approval-form-controls-test-v3-upload-media.mjs`
- Decoded app definition: `approval-form-controls-test-v3-app-def.json`
- Approval form definition: `approval-form-controls-test-v3-approval-form-def.json`
- Data list definition: `approval-form-controls-test-v3-list-def.json`
- Generated package: `Approval Form Controls Test v3.generated.yap`
- User-accessible copy: `<downloads>/Approval Form Controls Test v3.generated.yap`
- ID family: `453...`
- FlowKey/form key: `AFC3`

Generated `.yap` and decoded package JSON artifacts stay out of Git. This document and the generator script are the tracked baseline.

## What This Proves

This stage proves the upload/media control batch after the successful v1 representative-control baseline and v2 advanced-input baseline.

Controls proved for workflow-form usage:

- `file-upload`
- `icon-upload`

Scope note: binary file/image persistence into data-list fields was intentionally deferred. v3 proves render, upload, submit, submitted-page display, reviewer-task display, approval completion, and text-summary `ContentList` persistence.

## Validation Results

Local checks passed:

- `node --check tools/generators/generate-approval-form-controls-test-v3-upload-media.mjs`
- `node --check validate-yap-package.js`
- `node --check validate-yap-graph.js`
- `node --check validate-ywf-def.js`
- `node --check validate-ydl-list.js`
- `node --check workflow-action-config-validator.js`

Package and component validation:

- `validate-yap-package.js approval-form-controls-test-v3-app-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `validate-yap-graph.js approval-form-controls-test-v3-app-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `validate-ywf-def.js approval-form-controls-test-v3-approval-form-def.json --mode final`: `pass_with_warnings`, 0 errors.
- `validate-ydl-list.js approval-form-controls-test-v3-list-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `workflow-action-config-validator.js`: `pass`, 9 checked nodes, 0 errors, 0 warnings.

Wrapper build and round-trip validation:

- `build-yap-wrapper.js`: `pass`.
- Wrapper JSON, resource prefix, base64, gzip, resource JSON, and `Resource.Data` validation all passed.
- Decoded wrapper content matched the source app definition.
- Wrapped package validation and graph validation passed with expected upload/control support warnings only.

## Runtime Result

Runtime testing was completed in `https://<yourdomain>.yeeflow.com/`.

Verified:

- App imported successfully.
- Imported app opened successfully.
- `Upload Media Test Requests` data list opened without `datas/query` 400.
- Seed sample row rendered.
- `Approval Form Controls Test v3` approval form opened after publishing.
- `file-upload` rendered as the upload attachment control.
- `icon-upload` rendered as the upload image control.
- Uploaded safe test file: `<downloads>/yeeflow-upload-stage3-test.txt`.
- Uploaded safe test image: `<downloads>/yeeflow-upload-stage3-test.png`.
- Submit completed successfully.
- Submitted request detail showed:
  - file name `yeeflow-upload-stage3-test.txt`
  - image preview for the uploaded test PNG
- Reviewer task opened and displayed:
  - request title
  - requester
  - uploaded file name
  - uploaded image preview
  - notes
  - decision notes
  - Action Panel
  - Flow History
- Approval completed successfully.
- Submitted request status changed to `Completed`.
- `ContentList` created a persisted data-list row:
  - Request Title: `Upload media runtime test`
  - Upload Proof Scope: `File and image upload submitted in workflow form; binary persistence deferred.`

Runtime status: passed for Stage 3 workflow-form upload/media usage. Binary upload persistence to data-list file/image fields remains a future isolated proof.

## Generator Rules Confirmed

- `file-upload` can be generated from the AI Training export-backed shape with `attrs.ver`, `attrs.file_multiple`, `attrs.file_maxcount`, and `attrs.upload_btn`.
- `icon-upload` can be generated from the AI Training export-backed shape with `attrs.controlmultiple` and `attrs.maxselection`.
- Upload/media controls should usually span a full row in generated approval forms.
- Persist upload/media proof conservatively unless binary field persistence has been separately proven.
- For Stage 3, text-summary `ContentList` persistence is enough to prove workflow completion without claiming binary list persistence.

## Next Stage

Proceed to Stage 4 only after this baseline is committed:

- `organization-picker`
- `location-picker`
- `cost-center-picker`

Keep the next package isolated because these controls depend on tenant metadata.

---

<a id="generated-approval-form-controls-test-v4-baseline"></a>

<!-- source: docs/collection-02.md#generated-approval-form-controls-test-v4-baseline -->

# Generated Approval Form Controls Test v4 Baseline

Date: 2026-05-14

## Package

- App name: Approval Form Controls Test v4
- Generator: `tools/generators/generate-approval-form-controls-test-v4-pickers.mjs`
- Decoded app definition: `approval-form-controls-test-v4-app-def.json`
- Approval form definition: `approval-form-controls-test-v4-approval-form-def.json`
- Data list definition: `approval-form-controls-test-v4-list-def.json`
- Generated package: `Approval Form Controls Test v4.generated.yap`
- User-accessible copy: `<downloads>/Approval Form Controls Test v4.generated.yap`
- ID family: `454...`
- FlowKey/form key: `AFC4`

Generated `.yap` and decoded package JSON artifacts stay out of Git. This document and the generator script are the tracked baseline.

## What This Proves

This stage tests the people and organization picker batch after the successful v1 representative-control baseline, v2 advanced-input baseline, and v3 upload/media baseline.

Controls tested:

- `identity-picker`
- `organization-picker`
- `cost-center-picker`
- `location-picker`

Runtime status:

- `identity-picker`: proven for editable single-select workflow-form usage.
- `organization-picker`: partially proven. It rendered and opened with tenant department data visible, but selected value retention was not proven in this sandbox.
- `cost-center-picker`: partially proven. It rendered and opened, but the sandbox returned no cost center rows.
- `location-picker`: partially proven. It rendered and opened, but the sandbox returned no matching location data.

Direct picker-value persistence into data-list picker fields was intentionally deferred. v4 uses safe text-summary `ContentList` persistence while proving form render, picker open/selection where possible, task display, approval completion, and list-record creation.

## Validation Results

Local checks passed:

- `node --check tools/generators/generate-approval-form-controls-test-v4-pickers.mjs`
- `node --check validate-yap-package.js`
- `node --check validate-yap-graph.js`
- `node --check validate-ywf-def.js`
- `node --check validate-ydl-list.js`
- `node --check workflow-action-config-validator.js`

Package and component validation:

- `validate-yap-package.js approval-form-controls-test-v4-app-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `validate-yap-graph.js approval-form-controls-test-v4-app-def.json --mode generator --stage final`: `pass`, 0 errors.
- `validate-ywf-def.js approval-form-controls-test-v4-approval-form-def.json --mode final`: `pass_with_warnings`, 0 errors.
- `validate-ydl-list.js approval-form-controls-test-v4-list-def.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `workflow-action-config-validator.js`: passed during generator validation.

Expected warnings:

- Picker controls are environment-dependent.
- `organization-picker`, `cost-center-picker`, and `location-picker` remain runtime-sensitive for selection/persistence until tenant metadata is available and export-backed retention attrs are proven.
- `flex_grid` and token-color warnings are known validator warnings for the current generated design-system layout.

Wrapper build and round-trip validation:

- `build-yap-wrapper.js`: passed.
- Wrapper JSON, resource prefix, base64, gzip, resource JSON, and `Resource.Data` validation all passed.
- Decoded wrapper content matched the source app definition.
- Wrapped package validation and graph validation passed with expected warnings only.

## Runtime Result

Runtime testing was completed in `https://<yourdomain>.yeeflow.com/`.

Verified:

- App imported successfully.
- Imported app opened successfully.
- `Picker Test Requests` data list opened without `datas/query` 400.
- Seed sample row rendered.
- `Approval Form Controls Test v4` approval form opened.
- `Requester` identity-picker resolved to `Renger from Yeeflow`.
- `Selected User` identity-picker opened the user selector, selected `Renger from Yeeflow`, retained the selection, and displayed it on submitted and reviewer pages.
- `organization-picker` rendered and opened the department picker. The tenant selector showed `Default`; selected value retention was not proven.
- `cost-center-picker` rendered and opened the cost center picker. No cost center rows were available in the sandbox.
- `location-picker` rendered and opened the location picker/dropdown. No matching location data was available in the sandbox.
- Submit completed successfully.
- Reviewer task opened.
- Reviewer task displayed:
  - request title
  - requester
  - selected user
  - notes
  - decision notes
  - Action Panel
  - Flow History
- Approval completed successfully.
- Submitted request status changed to `Completed`.
- `ContentList` created a persisted data-list row:
  - Request Title: `Picker controls runtime test`
  - Requester: runtime internal user ID value
  - Picker Proof Scope: `Picker controls submitted in workflow form; direct picker persistence deferred.`

Runtime status: passed for identity-picker workflow-form usage and partial for organization/cost-center/location picker render/open behavior.

## Generator Rules Confirmed

- Editable single-select `identity-picker` can be generated with the AI Training export-backed shape using `multiple: false` and `identity-maxselection: 1`.
- Current-user readonly identity display remains safe for requester context.
- Tenant-metadata pickers should be generated as optional unless selectable metadata and persistence are already proven.
- For `organization-picker`, render/open can be generated with the export-backed tree-select shape, but selected-value retention needs a focused working export before promotion to proven.
- For `cost-center-picker` and `location-picker`, render/open is safe enough for isolated tests, but value selection and direct persistence depend on tenant metadata availability.
- Persist safe text summaries until direct picker field persistence is proven.

## Next Stage

Proceed to Stage 5 only after this baseline is committed:

- `metadata`
- `mutiple-metadata`
- `tag`

Keep metadata/tag testing isolated because it depends on source/category metadata and multi-value shapes.

---

<a id="generated-approval-form-controls-test-v6-baseline"></a>

<!-- source: docs/collection-02.md#generated-approval-form-controls-test-v6-baseline -->

# Generated Approval Form Controls Test v6 Baseline

Date: 2026-05-14

Package: `Approval Form Controls Test v6.generated.yap`

Purpose: prove generated approval-form lookup and list/listref controls using internal packaged Yeeflow data lists.

## Scope

Included:

- `Reference Products` source data list with sample records.
- `Lookup List Test Requests` target persistence list.
- `Approval Form Controls Test v6` approval form.
- Internal single-select lookup control bound to `Reference Products`.
- Lookup additional-field/autofill mappings for product code, category, and unit price.
- List/listref line item table with product, quantity, unit price, and line note.
- Simple reviewer approval workflow.
- `ContentList` persistence into the target list.

Deferred:

- `lookup-list`
- embedded `data-list` display
- direct child-row-to-data-list persistence
- metadata, multi-metadata, and tag controls

## Local Validation

Commands passed:

- `node --check tools/generators/generate-approval-form-controls-test-v6-lookup-list.mjs`
- `node --check validate-yap-package.js`
- `node --check validate-yap-graph.js`
- `node --check validate-ywf-def.js`
- `node --check validate-ydl-list.js`
- `node --check workflow-action-config-validator.js`
- `node validate-yap-package.js approval-form-controls-test-v6-app-def.json --mode generator --stage final`
- `node validate-yap-graph.js approval-form-controls-test-v6-app-def.json --mode generator --stage final`
- `node validate-ywf-def.js approval-form-controls-test-v6-approval-form-def.json --mode final`
- `node validate-ydl-list.js approval-form-controls-test-v6-products-list-def.json --mode generator --stage final`
- `node validate-ydl-list.js approval-form-controls-test-v6-request-list-def.json --mode generator --stage final`
- `node build-yap-wrapper.js approval-form-controls-test-v6-app-def.json "Approval Form Controls Test v6.generated.yap" --title "Approval Form Controls Test v6" --description "Lookup and list control runtime proof package" --validation-mode generator`

Known warnings:

- `flex_grid` remains schema-supported but locally unclassified by the validator.
- Standalone extracted list defs warn about missing wrapper `ListSetID`.
- Identity-picker current-user usage is environment-dependent but already proven in earlier stages.
- Resolved token hex warnings are accepted for generated IDs.

## Runtime Result

Status: passed.

Runtime URL after import:

- App/listset: `https://<yourdomain>.yeeflow.com#/list-set/<workspace-id>/<listset-id>/<list-id>`
- Target list: `https://<yourdomain>.yeeflow.com#/list-set/<workspace-id>/<listset-id>/<list-id>`

Verified:

- App imported successfully.
- App opened.
- `Reference Products` opened and loaded sample rows.
- `Lookup List Test Requests` opened without `datas/query` 400.
- Approval form opened.
- Lookup dropdown opened and showed packaged products.
- Selected `Standard Laptop`.
- Lookup display value appeared on the form.
- Additional mappings populated:
  - Product Code: `LAP-STD`
  - Product Category: `Hardware`
  - Unit Price: `USD 1,250.00`
- List/listref row rendered.
- Added and edited one line item row:
  - Product: `Standard Laptop`
  - Quantity: `2`
  - Unit Price: `1,250.00`
  - Line Note: `Runtime sublist row`
- Submitted successfully.
- Submitted request detail displayed lookup and list row values.
- Reviewer task opened and displayed lookup and list row values.
- Approval completed successfully.
- `ContentList` created a target row.

## Generator Rules Learned

Use a packaged internal lookup when the source list is included in the same `.yap`.

For lookup display and derived values:

- set the lookup source to the packaged source list
- use the source display field for the picker label
- use `attrs.addition[]` to populate readonly workflow variables for additional fields
- validate every source and target field before wrapper build

Persistence rule:

- Do not map a raw lookup variable to a plain text field when the expected value is the display name.
- In this runtime test, direct lookup-to-text persistence stored the internal local row ID (`2054943200723742740`).
- Persist readable values through addition/autofill variables or explicit summary variables.
- Direct raw lookup persistence is acceptable only when storing the local row ID is intended or the target field is proven lookup-compatible.

List/listref rule:

- A generated list/listref control can render a table, add a row, edit child values, submit, display values on reviewer task, and complete approval.
- Direct child-row-to-data-list persistence is not proven by this baseline.
- For now, persist a text summary through `ContentList` when durable row storage is needed.

## Coverage Update

Promoted:

- `lookup`: proven for internal packaged single-select lookup with addition mappings.
- `list` / `listref`: proven for workflow-form table render/add/edit/review/approval, with text-summary persistence.

Deferred:

- `lookup-list`
- embedded `data-list` display
- metadata / multi-metadata / tag

---

<a id="generated-data-list-approval-form-integration-pattern"></a>

<!-- source: docs/collection-02.md#generated-data-list-approval-form-integration-pattern -->

# Generated Data List + Approval Form Integration Pattern

## 1. Scenario

This pattern was proven with the **Department Access Request** approval form and a dedicated **Department Access Requests** data list.

The full scenario includes:

- A Department Access Request approval form.
- A dedicated Department Access Requests data list used as the workflow persistence target.
- A Departments data list used as the lookup source.
- A Department lookup control on the approval form.
- A Department Code field auto-filled through the lookup control additional-field mapping.
- A workflow `ContentList` / Set Data List action that creates a row in the dedicated Department Access Requests list after approval.

The key lesson is that the approval form should not target the generated pre-import list IDs directly. Yeeflow may remap list and field IDs during import, so the final approval-form persistence target must be patched from the exported-back `.ydl` metadata.

## 2. Staged Generation Process

Use this staged process when a generated approval form needs a generated storage list:

1. Generate the data list first as a `.ydl` package.
2. Import the data list into the sandbox manually.
3. Export the imported data list back from Yeeflow.
4. Decode the exported-back `.ydl`.
5. Extract the real/remapped list metadata:
   - `AppID`
   - `ListSetID`
   - `ListID`
   - field `FieldName`
   - field `InternalName`
   - field `FieldID`
6. Patch the approval form `ContentList` target to the exported-back list metadata.
7. Validate the decoded approval form structurally:

   ```bash
   node validate-ywf-def.js ./department-access-request-def.v2-dedicated-list.json --mode final
   ```

8. Validate the decoded approval form against the generated list metadata:

   ```bash
   node validate-ywf-def-against-yap.js \
     ./department-access-request-def.v2-dedicated-list.json \
     ./department-access-requests-list-exported-metadata.json \
     --mode final \
     --profile generator
   ```

9. Build the `.ywf` only after both validations pass.
10. Import, publish, and test the approval form manually in the sandbox.

## 3. Data List Generation Details

The Department Access Requests data list included these fields:

| Display name | FieldName | Purpose |
| --- | --- | --- |
| Request No. | `Title` | Stores the approval request number. |
| Applicant | `Text1` | Stores the requester/applicant. |
| Submission Date | `Datetime1` | Stores the original submission date. |
| Department | `Text2` | Lookup to Departments. |
| Department Code | `Text3` | Stores the derived code from Departments. |
| Access Reason | `Text4` | Stores the requester reason. |
| Needed By Date | `Datetime2` | Stores the requested deadline. |
| Approval Status | `Text5` | Stores status such as Approved. |
| Approved Date | `Datetime3` | Stores workflow approval time. |
| Created From Workflow | `Text6` | Marks workflow-created rows. |
| Notes | `Text7` | Optional notes. |

The generated data list also included:

- A Department lookup field configured to the existing Departments list.
- A custom detail form based on the Asset Inventory v5 data-list form pattern.
- Views such as all requests, approved requests, pending requests, and requests by department.
- Sample records for sandbox testing.
- External Departments lookup record IDs excluded from `Resource.ReplaceIds`.

For lookup sample data, standalone `.ydl` packages must preserve external lookup IDs. If external lookup record IDs are included in `Resource.ReplaceIds`, Yeeflow may remap them during import and the lookup can display as `(Deleted)`.

## 4. Approval Form Patch Details

The Department Access Request approval form originally wrote to sandbox Procurement PR Records. Stage 2 patched only the workflow persistence target and mappings.

The `ContentList` node was changed to target:

- AppID: `41`
- ListSetID: `2041029207826120704`
- ListID: `2053754514355138562`
- Node name: `Create Department Access Request Record`
- Operation: `add`

Field mappings:

| Source | Target field |
| --- | --- |
| `RequestNo` | `Title` |
| `Applicant` | `Text1` |
| `SubmissionDate` | `Datetime1` |
| `field_9` Department lookup | `Text2` |
| `DepartmentCode` | `Text3` |
| `AccessReason` | `Text4` |
| `NeededByDate` | `Datetime2` |
| literal `Approved` | `Text5` |
| `now()` | `Datetime3` |
| literal `Yes` | `Text6` |

The patch preserved:

- Department lookup control configuration.
- Department Code additional-field mapping.
- Request page UI.
- Approval page UI.
- Workflow graph layout.
- Approval assignment.
- Page registration metadata.
- Action panel and workflow history.

## 5. Validation Rules

Before building the final patched approval `.ywf`, validate:

- No unresolved placeholders remain.
- `ContentList` target list exists in exported-back metadata.
- Every mapped target field exists.
- Source workflow variables exist.
- Source and target field types are compatible.
- Lookup source metadata resolves.
- Lookup display/sort field resolves.
- Lookup additional field source resolves.
- Lookup additional field target variable resolves.
- Generated list metadata comes from exported-back `.ydl`, not pre-import generated IDs.

Do not use pre-import generated list IDs for production-like approval form testing. They are useful for building the first list package, but the final approval form must use the imported/exported-back list metadata.

## 6. Sandbox Test Result

This pattern was validated with the Department Access Request test:

- Data list import passed.
- Approval form import passed.
- Approval form publish passed.
- Departments lookup worked.
- Department Code additional-field auto-fill worked.
- Approval workflow created a row in the dedicated Department Access Requests list.

## 7. Generator Rule

For any generated approval form that needs its own storage list:

1. Generate the data list first.
2. Import and export it back from Yeeflow.
3. Extract the exported-back list and field metadata.
4. Patch the approval form `ContentList` target and mappings.
5. Validate structurally.
6. Validate against exported-back generated-list metadata.
7. Build the final `.ywf`.

Never target pre-import generated IDs for production-like testing. Always use the real/remapped IDs from the exported-back `.ydl`.

## 8. Future Upgrade

This staged process should later become app-level `.yap` generation for multi-component packages.

That future app-level generator should package:

- Data lists.
- Approval forms.
- Lookup relationships.
- Persistence mappings.
- Views.
- Custom forms.
- Sample data.
- Dependency/remap metadata.

Until that exists, staged `.ydl` generation followed by exported-back metadata patching is the safe path.

---

<a id="generated-data-list-baseline-asset-inventory-v5"></a>

<!-- source: docs/collection-02.md#generated-data-list-baseline-asset-inventory-v5 -->

# Generated Data List Baseline: Asset Inventory v5

This document records the first successful generated Yeeflow `.ydl` data-list baseline with fields, views, sample records, custom form registration, custom form assignment, and rendered custom form controls.

For related standalone lists with lookups, use the later staged lookup baseline in `docs/collection-02.md#generated-related-data-lists-staged-lookup-pattern`. Asset Inventory v5 remains the baseline for single-list generation and custom form registration.

## 1. What v5 Proves

Asset Inventory v5 proves that Codex can generate a sandbox-testable Yeeflow data-list package when the decoded `.ydl` structure follows real export patterns closely.

Proven:

- Generated `.ydl` imports successfully.
- Fields are created.
- Views are created.
- Sample records are created.
- A custom form is created.
- The custom form is assigned to:
  - New Item
  - Edit Item
  - View Item
- The custom form opens in the designer.
- Custom form controls render instead of being dropped or showing an empty drag area.
- The wrapper round-trips through gzip/base64 `Resource` decoding.
- `validate-ydl-list.js --mode generator --stage final` passes.

The working package is:

- `asset-inventory-def.import-debug-v5.json`
- `asset-inventory.import-debug-v5.ydl`

## 2. Critical Custom Form Registration Rules

Generated data-list custom forms must follow this pattern.

Custom form layout:

- `Layout.Type = 1`
- `Layout.LayoutView = null`
- `Layout.Ext2 = "{\"src\":true}"`
- `Layout.IsItemPerm = false`
- `Layout.LayoutInResources` must exist and contain one resource entry.

Custom form embedded resource:

- `LayoutInResources[0].ID` must exist.
- `LayoutInResources[0].RefId` must exist.
- `LayoutInResources[0].Resource` must exist.
- `LayoutInResources[0].ID` must equal `Layout.LayoutID`.
- `LayoutInResources[0].RefId` must equal `Layout.LayoutID`.
- `LayoutInResources[0].Resource` is a JSON string, not a raw object.

List display settings:

- `Item.ListModel.LayoutView` stores the New/Edit/View assignment.
- Use the custom form `LayoutID` in:
  - `add`
  - `edit`
  - `view`

Example:

```json
{
  "add": "2037000000000002006",
  "edit": "2037000000000002006",
  "view": "2037000000000002006",
  "opentype": {
    "add": "modal",
    "view": "slide"
  },
  "modalsize": {
    "view": 2
  }
}
```

Form `Resource` JSON must include:

- `children`
- `attrs`
- `title`
- `filterVars`
- `ver`
- `tempVars`

Example shape:

```json
{
  "children": [],
  "attrs": {},
  "title": "Asset Detail Form",
  "filterVars": [],
  "ver": 2,
  "tempVars": []
}
```

Controls inside `children` should use UUID-style IDs. Bound controls must bind to storage `FieldName` values such as `Title`, `Text1`, `Datetime1`, or `Bit1`, and should include the matching `fieldID`.

## 3. Failed Versions And Lessons

### v2

Result:

- The list imported.
- Custom form registration was incomplete.
- Display settings were still using Default form.

Lesson:

- A custom form layout alone is not enough.
- `Item.ListModel.LayoutView` must assign New/Edit/View to the custom form `LayoutID`.
- `LayoutInResources` must include real-export-style metadata.

### v3

Result:

- The custom form registered.
- New/Edit/View assignment worked.
- The form opened, but the form content was still not reliable.

Lesson:

- Real embedded custom forms use:
  - `LayoutView: null`
  - `LayoutInResources[0].ID`
  - `LayoutInResources[0].RefId`
  - `LayoutInResources[0].Resource`

### v4

Result:

- Assignment was preserved.
- Yeeflow accepted the custom form layout.
- On export-back, Yeeflow dropped `LayoutInResources` and exported it as `[]`.

Lesson:

- `LayoutInResources[0].ID` and `RefId` cannot be separate generated resource IDs.
- They must match the custom form `LayoutID`.

### v5

Result:

- Import works.
- New/Edit/View assignment works.
- Custom form designer opens.
- Custom form controls render.

Lesson:

- The custom form resource identity must be tied directly to the layout identity.

## 4. Required Validator Checks

`validate-ydl-list.js` must enforce or warn on these checks in generator/final mode.

Hard checks:

- Assigned custom forms cannot have empty `children`.
- Custom form `LayoutInResources` must exist.
- `LayoutInResources[0].Resource` must exist and parse as JSON.
- `LayoutInResources[0].ID` must equal `Layout.LayoutID`.
- `LayoutInResources[0].RefId` must equal `Layout.LayoutID`.
- Bound custom form controls must reference existing fields.
- New/Edit/View assignment must point to a valid custom form layout when a generated custom form is intended for use.

Warnings or strict generator checks:

- Custom form `LayoutView` should be `null`.
- `Ext2` should be `{"src":true}`.
- `IsItemPerm` should be `false`.
- Custom form control IDs should be UUID-style.

## 5. Generation Rules

Generated data-list packages should:

- Generate `ListModel` metadata close to real exports.
- Populate `Resource.ReplaceIds`.
- Use Yeeflow-style large numeric string IDs.
- Use UUID-style custom form control IDs.
- Bind custom form controls to valid `FieldName` values.
- Include matching `fieldID` on bound controls.
- Use `container`, `text`, and `flex_grid` controls for usable forms.
- Use two-column desktop/tablet grids and one-column mobile grids where appropriate.
- Use `attrs.common.grid.position` with `cSpan: 2` for full-width long text controls.
- Keep custom form `LayoutInResources[0].ID` and `RefId` equal to `LayoutID`.

For the Asset Inventory baseline, the custom form includes:

- Asset Inventory section:
  - Asset Name
  - Asset Tag
  - Category
  - Serial Number
  - Manufacturer
  - Model
- Assignment and Lifecycle section:
  - Assigned To
  - Department
  - Location
  - Status
  - Condition
  - Active
- Purchase and Warranty section:
  - Purchase Date
  - Warranty Expiry
  - Purchase Cost
  - Vendor
- Notes section:
  - Notes

## 6. Sandbox Test Checklist

After importing a generated `.ydl` package in sandbox:

1. Confirm the list imports successfully.
2. Confirm fields are created.
3. Confirm views are created.
4. Confirm sample records import if included.
5. Confirm the custom form appears in list forms.
6. Confirm New/Edit/View display settings use the custom form.
7. Open the custom form designer.
8. Confirm controls render on the canvas.
9. Create a new item and save it.
10. Edit an item and save changes.
11. View an item and confirm the form opens correctly.
12. Export the list back to `.ydl`.
13. Decode the export-back file and verify `LayoutInResources` remains populated.
14. Confirm `LayoutInResources[0].ID` and `RefId` still match the remapped `LayoutID`.

If export-back shows `LayoutInResources: []`, the custom form resource was not accepted by import and the generated package is not production-candidate.

---

<a id="generated-form-actions-phase-2-query-submit-test-v1-baseline"></a>

<!-- source: docs/collection-02.md#generated-form-actions-phase-2-query-submit-test-v1-baseline -->

# Form Actions Phase 2 Query Submit Test v1 Baseline

## Generated Package

- App: `Form Actions Phase 2 Query Submit Test v1`
- Runtime import label used for the final retest: `Form Actions Phase 2 Query Submit Test v1 Retest`
- Generator: `tools/generators/generate-form-actions-phase-2-query-submit-test-v1.mjs`
- Workspace package: `/Users/Renger/Documents/Codex Projects/AI Agent and Copilot templates/form-actions-phase-2-query-submit-test-v1.yap`

The `.yap`, generated package JSON, decoded form/list JSON, and validation JSON remain ignored and are not intended for Git staging.

## Local Validation

Ran successfully:

- `node --check tools/generators/generate-form-actions-phase-2-query-submit-test-v1.mjs`
- `node tools/generators/generate-form-actions-phase-2-query-submit-test-v1.mjs`
- `node scripts/smoke-expression-validation.mjs`
- `node validate-yap-package.js form-actions-phase-2-query-submit-test-v1-app-def.json --mode generator --stage final`
- `node validate-yap-graph.js form-actions-phase-2-query-submit-test-v1-app-def.json --mode generator --stage final`
- `node validate-ywf-def.js form-actions-phase-2-query-submit-test-v1-approval-form-def.json --mode final`
- `node validate-ydl-list.js form-actions-phase-2-query-submit-test-v1-source-list-def.json`
- `node validate-ydl-list.js form-actions-phase-2-query-submit-test-v1-target-list-def.json`
- `node workflow-action-config-validator.js form-actions-phase-2-query-submit-test-v1-app-def.json`
- `node build-yap-wrapper.js ...`
- wrapper package and graph validation

Validator result summary:

- package validation: pass with warnings, 0 errors
- graph validation: pass, 0 errors
- approval form validation: pass with warnings, 0 errors
- source/target list validation: pass with warnings, 0 errors
- wrapper round trip: pass

## Runtime Result

Runtime target: `https://<yourdomain>.yeeflow.com/`

Result: pass after regenerating with the corrected Query data filter path.

Passed:

- app imports
- app opens
- source data list opens and sample rows load
- target data list opens without `datas/query` 400
- approval form opens
- Query data multiple item action runs from a button click
- Query data multiple result collection maps into the `Query Results` sub list
- query result count maps into a display/workflow value
- Query data `Active == true` filter returns only active rows
- Query data single item action runs from a button click
- single-query field mapping populates form values from an active row
- `arraySum` over the filtered query result collection runs and returned `2000`
- `JSONStringfy` displays the query collection JSON
- Save changes action succeeds and does not advance workflow
- custom Submit form action submits into the workflow
- reviewer task opens for the current user
- approval completes
- ContentList creates a readable target-list record
- Text controls remained editor-safe and inline-width

Historical partial result from the first generated package:

- The Query data `Active == true` filter was ignored by runtime because the generated package did not configure the actual Query data step `Data filter -> Condition` setting. The generator wrote an `attrs.querydata_filter` helper array, but the designer/runtime did not treat that as the Query data step's Data filter condition.
- `vLookup` remains deferred because no safe export-backed expression token shape has been proven.

User follow-up with patched export:

- Manually opening the Query data step and setting `Data filter -> Condition` to `Active Equals ON` made the filter work.
- The patched export stores the working condition in `attrs.querydata_filters` plural:

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

Regenerated retest confirmation:

- all Query data steps use `attrs.querydata_filters`
- no generated Query data step uses `attrs.querydata_filter`
- the Active filter uses `left: "Bit1"`, `op: "0"`, `right: "true"`, and `showCus: true`

## Runtime Evidence

Source list loaded three packaged records:

- `SRC-001`, `Laptop replacement`, `Approved`, `1200`, `Active = true`
- `SRC-002`, `Software license renewal`, `Approved`, `800`, `Active = true`
- `SRC-003`, `Archived monitor request`, `Closed`, `300`, `Active = false`

Multiple Query action result:

- sub list loaded only active records: `SRC-002` and `SRC-001`
- inactive `SRC-003` was excluded
- Loaded Count displayed `2`
- this proves query execution, selected-field mapping, count output, list-variable population, and filter correctness

Single Query action result:

- Query Title: `SRC-002`
- Query Request Title: `Software license renewal`
- Query Approval Status: `Approved`
- Query Amount: `800`
- Query Final Notes: `Annual renewal`

Expression action result:

- `arraySum(__temp_var_CollectionofQueryItems, "Amount", [], [])` returned `2000`
- `JSONStringfy(__temp_var_CollectionofQueryItems)` displayed:

```json
[{"Title":"SRC-001","Amount":"1200"},{"Title":"SRC-002","Amount":"800"}]
```

Save changes result:

- button action returned a success toast
- the workflow did not advance

Submit / approval / persistence result:

- custom Submit form button submitted a workflow successfully during the first corrected run
- final all-action proof used native submit after rerunning the Query multiple, Query single, and `arraySum` actions
- reviewer task opened for `Renger from Yeeflow`
- reviewer approval completed
- target list row was created with:
  - Request Title: `Phase 2 final all query actions`
  - Loaded Count: `2.00`
  - Selected Query Title: `SRC-002`
  - Selected Query Status: `Approved`
  - Query Amount Sum: `2000.00`
  - Approval Status: `Approved`
  - Created From Workflow: `Yes`

## Generation Guidance

Runtime-proven for generated approval forms:

- `querydata` multiple can populate a form list/sublist variable.
- `querydata_totalcount` can populate a count value for display and later persistence when copied into a workflow variable.
- `querydata` single can map selected fields into form/workflow variables.
- Query result collections can be aggregated with `arraySum`.
- Query result collections can be displayed/debugged with export-backed `JSONStringfy`.
- Query data filters are runtime-proven when generated as `attrs.querydata_filters` plural.
- `submit` with default attrs can submit the form/workflow from an action button.
- `submit` with `attrs.submitType = "3"` can Save changes without progressing workflow.

Filter correction:

- Query filters belong in the Query data step's `Data filter -> Condition` configuration.
- Generate `attrs.querydata_filters` plural.
- Do not rely on the generated `attrs.querydata_filter` singular helper path; runtime ignored it in this baseline.
- For Bit/Yes-No source fields, use `right: "true"` for ON, not `right: "ON"`.

---

<a id="generated-it-hardware-capex-request-text-standard-baseline"></a>

<!-- source: docs/collection-02.md#generated-it-hardware-capex-request-text-standard-baseline -->

# Generated IT Hardware CAPEX Request Text Standard Baseline

Package: `IT Hardware CAPEX Request v4 Text Standard`

Status: generated from the latest CAPEX v3 UI refresh generator after studying `Text Style Sample.ywf`.

## What Changed

All generated native Text controls now use the export-backed Yeeflow Text style shape:

- `type: "heading"`
- `label: "Text"`
- `attrs.headc.title.value` for static text
- `attrs.heads.ty = [null, "<preset>"]`
- `attrs.heads.color = "<token-or-color>"`
- `attrs.common.positioning.widthtype = [null, "2"]`
- meaningful `nv_label`

This replaces the previous generated shape that used `attrs.heads.color = [null, "..."]` and, in one attempted fix, `attrs.heads.ty` as a plain string.

## Generated Files

- `tools/generators/generate-it-hardware-capex-request-v3.mjs`
- `it-hardware-capex-request-app-def.v4-text-standard.json`
- `it-hardware-capex-request-approval-form-def.v4-text-standard.json`
- `it-hardware-capex-request-list-def.v4-text-standard.json`
- `it-hardware-capex-request-generation-report.v4-text-standard.json`
- `it-hardware-capex-request.v4-text-standard.generated.yap`
- `/Users/Renger/Downloads/IT Hardware CAPEX Request v4 Text Standard.yap`

## Validation

Local validation passed on 2026-05-14:

- `node --check scripts/inspect-ywf-text-controls.mjs`: pass
- `node --check tools/generators/generate-it-hardware-capex-request-v3.mjs`: pass
- `node --check validate-ywf-def.js`: pass
- JSON parse checks for the decoded text-style sample inspection files and v4 generated JSON files: pass
- `validate-ywf-def.js text-style-sample-decoded-def.json --mode final`: not applicable as a final approval workflow because the sample export is a Text style sample form, not a complete approval process with submit/task pages and approval panels.
- `validate-ywf-def.js it-hardware-capex-request-approval-form-def.v4-text-standard.json --mode final`: `pass_with_warnings`, 0 errors, no Text-control warnings.
- `validate-yap-package.js it-hardware-capex-request-app-def.v4-text-standard.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `validate-yap-graph.js it-hardware-capex-request-app-def.v4-text-standard.json --mode generator --stage final`: pass, 0 errors.
- `validate-ydl-list.js it-hardware-capex-request-list-def.v4-text-standard.json --mode generator --stage final`: `pass_with_warnings`, 0 errors.
- `workflow-action-config-validator.js --smoke`: pass.
- `build-yap-wrapper.js`: pass, wrapper round-trip decoded app equals source.

Focused Text structural check:

- Generated Text control count: 35.
- Old unsafe `attrs.heads.color = [null, "..."]` count: 0.
- Invalid `attrs.heads.ty` count: 0.
- Non-inline generated Text count: 0.

## Runtime

Runtime verification on `https://<yourdomain>.yeeflow.com` passed for the Text-control bug fix.

- Imported `/Users/Renger/Downloads/IT Hardware CAPEX Request v4 Text Standard.yap` successfully.
- Opened the imported app successfully.
- Opened the generated approval form successfully.
- Designer URL verified: `https://<yourdomain>.yeeflow.com#/admin/model/designer?appid=<app-id>&defkey=<definition-key>&listsetid=<listset-id>`
- Selected the generated `Draft` metric Text control in the form designer.
- Typography settings popup opened successfully.
- Text shadow settings popup opened successfully.

This directly verifies the previous issue where generated Text controls rendered visually but the Typography and Text shadow designer buttons did not respond.

Design-system carry-forward rule:

- The CAPEX V2/V4 form-design learning remains global: full-page backgrounds belong on page/form background settings, not on the `Main` container.
- Future generated approval forms, task pages, data-list custom forms, and dashboard pages should keep `Main` structural and put section/card/header backgrounds on those specific containers only.

---

<a id="generated-line-item-custom-code-row-label-pattern"></a>

<!-- source: docs/collection-02.md#generated-line-item-custom-code-row-label-pattern -->

# Generated Line-Item Custom Code Row Label Pattern

This note documents the current generator baseline for automatic row labels in Yeeflow approval-form sub-lists. It is based on the tested Simple PR Line Item Approval exports and the working custom code control behavior.

## 1. Numeric Row-Numbering Pattern

Use numeric row numbering when the first column should store `1`, `2`, `3`, and so on.

Observed working pattern:

```json
{
  "type": "codein",
  "label": "Row number automation",
  "displayLabel": true,
  "attrs": {
    "codein-script": "codeInModules=function(...)",
    "codein-script-param": {
      "listId": "PurchaseDetailsList",
      "HTMLId": "splia_sublist_row_number",
      "snField": "No"
    }
  }
}
```

For numeric labels, the `No` row field can be `number`, and the row control can be `input_number`. The field should still be readonly when the value is generated by custom code.

## 2. Alphabetic Row-Label Pattern

Use alphabetic row labels when the first column should store Excel-style values:

```text
A, B, C, ... Z, AA, AB, AC ...
```

The key difference is schema compatibility. Alphabetic labels must be stored in a text field. If the row field remains numeric, Yeeflow may coerce `A`, `B`, `C` into `0`.

Working pattern:

```json
{
  "type": "codein",
  "label": "Alphabetic row label automation",
  "displayLabel": true,
  "attrs": {
    "codein-script": "import * as React from 'react';\n\nexport class CodeInApplication implements CodeInComp { ... }",
    "codein-script-param": {
      "listId": "PurchaseDetailsList",
      "HTMLId": "splia_sublist_alpha_label",
      "snField": "No"
    }
  }
}
```

The code should:

- read rows from `fieldsValues[listId]`
- skip rows marked deleted, such as `_del`
- convert the visible row index into an alphabetic label
- update only `listId[index].snField`
- avoid running in readonly mode
- avoid unnecessary repeated writes

## 3. When `No.` Should Be Number Vs Text

Use `number` when:

- the label is numeric only
- the value is persisted to a numeric target field
- the field is displayed with `input_number`

Use `text` when:

- the label may contain letters
- the custom code writes values such as `A`, `Z`, or `AA`
- the value is persisted to a text-compatible target field

Generator rule:

```text
numeric row labels -> No.type = number
alphabetic row labels -> No.type = text
```

## 4. Readonly Row-Control Rule

Generated row labels should be readonly in the sub-list UI, regardless of numeric or alphabetic format.

Readonly is stored on the row control:

```json
{
  "control": {
    "type": "input",
    "label": "No.",
    "binding": "No",
    "readonly": true,
    "attrs": {
      "list_field": true,
      "list_field_binding": "PurchaseDetailsList",
      "list_control_id": "splia-control-purchase-details-list"
    }
  }
}
```

The listref/list-variable `editable` value may remain `true`. In the tested export, readonly on the control prevented user editing while still allowing custom code to write with `context.setFieldValue(...)`.

## 5. New-Style `codein` Custom Code Structure

New-style Yeeflow custom code should be readable source code, not the old minified compiled bundle.

Recommended structure:

```tsx
import * as React from 'react';

export class CodeInApplication implements CodeInComp {
  description() {
    return 'Fill alphabetic row labels in the first column of a sub-list.';
  }

  inputParameters() {
    return [
      { id: 'listId', type: 'string', desc: 'Variable ID of the sub-list.' },
      { id: 'snField', type: 'string', desc: 'Row field ID to store the label.' }
    ];
  }

  requiredFields(params) {
    return params && params.listId ? [params.listId] : [];
  }

  render(context, fieldsValues, readonly) {
    return React.createElement(AlphabeticRowLabeler, {
      context,
      readonly,
      listId: context.params.listId,
      snField: context.params.snField,
      rows: fieldsValues ? fieldsValues[context.params.listId] : []
    });
  }
}
```

Use `React.Component` rather than hooks for broad Yeeflow runtime compatibility.

## 6. `codein-script` And `codein-script-param` Usage

`attrs["codein-script"]` stores the custom code source or compiled script.

`attrs["codein-script-param"]` stores the runtime configuration:

```json
{
  "listId": "PurchaseDetailsList",
  "HTMLId": "splia_sublist_alpha_label",
  "snField": "No"
}
```

For the new-style alphabetic row-label code:

- `listId` is required.
- `snField` is required.
- `HTMLId` may remain present for compatibility, but the tested source code does not depend on it.

## 7. `listId` / `snField` Behavior

`listId` identifies the sub-list variable:

```text
PurchaseDetailsList
```

The code reads rows from:

```ts
fieldsValues[listId]
```

`snField` identifies the row field to update:

```text
No
```

The code writes row labels using the Yeeflow row-field path format:

```ts
context.setFieldValue(listId + '[' + rowIndex + '].' + snField, label);
```

Example:

```text
PurchaseDetailsList[0].No = A
PurchaseDetailsList[1].No = B
```

## 8. Validator Rules To Add

Add checks to `validate-ywf-def.js`:

- Detect row-label custom code controls by `type: "codein"` and `codein-script-param.listId` / `snField`.
- Confirm `listId` exists as a `variables.basic` list variable.
- Confirm the list variable points to an existing `variables.listref`.
- Confirm `snField` exists in the listref row fields.
- If the code appears alphabetic, require the row field type to be `text`.
- If the code appears alphabetic, require list control `list-fields[].control.type` to be `input`, not `input_number`.
- Require the target row control to be `readonly: true`.
- Confirm request and approval list metadata are consistent for the target row field.
- Warn if `codein-script` uses old compiled code when the generation mode expects new-style source.

Add checks to `validate-ywf-def-against-yap.js`:

- If the generated row label is persisted through `ContentList`, resolve the target field.
- For alphabetic labels, require the target normalized type to be `text` or compatible.
- Fail or warn if alphabetic labels map to a numeric target field.

## 9. Import Test Checklist

After importing a generated alphabetic row-label form into a Yeeflow sandbox:

1. Open the request form.
2. Add one line item and confirm `No.` shows `A`.
3. Add three line items and confirm `A`, `B`, `C`.
4. Delete the middle row and confirm visible rows relabel to `A`, `B`.
5. Add more than 26 rows and confirm `Z`, `AA`, `AB`.
6. Confirm the `No.` cell is readonly to the user.
7. Submit the request.
8. Confirm the approval page displays the line labels readonly.
9. Confirm parent and detail records are still created.
10. If row labels are persisted, confirm the target list stores the text labels correctly.

## 10. Risks And Limitations

- Structural validators cannot execute Yeeflow custom code. Runtime behavior still requires sandbox import testing.
- Alphabetic labels require a text-compatible field. Numeric fields may display or persist labels as `0`.
- The row field can be readonly in the UI and still writable by custom code, but this should be retested after Yeeflow runtime changes.
- Reorder behavior depends on whether Yeeflow exposes reordered row state through `fieldsValues[listId]`.
- If deleted rows remain in the internal list state, the code must skip Yeeflow delete markers such as `_del`.
- If alphabetic row labels must be persisted, the target detail-list field must exist and must be text-compatible.
- Old compiled `codeInModules=function(...)` scripts can work, but generator-produced custom code should prefer new-style source with `export class CodeInApplication implements CodeInComp`.

---

<a id="generated-related-data-lists-staged-lookup-pattern"></a>

<!-- source: docs/collection-02.md#generated-related-data-lists-staged-lookup-pattern -->

# Generated Related Data Lists: Staged Lookup Pattern

This document records the successful Departments -> Employees staged `.ydl` generation pattern for related standalone Yeeflow data lists with lookup sample data.

## 1. Departments -> Employees Staged Result

The successful flow was:

1. Generate the independent reference list first: `Departments`.
2. Import `Departments` into the sandbox.
3. Export `Departments` back from Yeeflow.
4. Decode the exported-back package and extract the real remapped metadata:
   - `AppID`
   - `ListSetID`
   - `ListID`
   - display field, such as `Title`
   - sample/reference row `ListDataID` values
5. Patch the dependent list: `Employees`.
6. Configure `Employees.Department` as a lookup to the real imported `Departments` list.
7. Use real imported Department record `ListDataID` values in Employee sample data.
8. Exclude those external Department IDs from the Employees wrapper `Resource.ReplaceIds`.
9. Validate and build the Employees wrapper.
10. Import Employees and verify sample rows display Department values correctly.

This proved that related standalone `.ydl` packages can be generated safely when imported in stages and patched with export-back metadata.

## 2. Correct Lookup Sample Value Shape

For a single lookup field, the sample data value is a plain string containing the target record `ListDataID`.

Example:

```json
{
  "Text4": "2053735610870284294"
}
```

No extra JSON object, display cache, label value, or lookup metadata object is required.

In the successful Employees test:

| Employee | Department | Department `ListDataID` |
| --- | --- | --- |
| Avery Lim | Information Technology | `2053735610870284293` |
| Jamie Koh | Human Resources | `2053735610870284294` |
| Morgan Tan | Finance | `2053735610870284295` |

## 3. ReplaceIds Rule

`Resource.ReplaceIds` tells Yeeflow which IDs should be remapped during standalone import.

Local generated IDs should be included in `Resource.ReplaceIds`, including:

- generated list IDs
- generated field IDs
- generated layout IDs
- generated custom form resource IDs
- generated sample row `ListDataID` values for the list being imported

External resolved dependency IDs must not be included in `Resource.ReplaceIds`, including:

- external lookup target `ListSetID`
- external lookup target `ListID`
- external lookup target display field `FieldID`
- external lookup target record `ListDataID` values used by sample data

If external lookup record IDs are included in `Resource.ReplaceIds`, Yeeflow remaps them during standalone import. The dependent sample rows then point to newly remapped IDs that do not exist in the already-imported reference list, and the lookup displays as `(Deleted)`.

## 3.1 App-Level `.yap` Internal Lookup Contrast

The standalone `.ydl` rule above applies when the lookup target records already exist outside the dependent package.

For an app-level `.yap` package where both the lookup target list and dependent list are packaged together, the rule is different:

- target sample record IDs are local package IDs
- target sample record IDs should be included in `Resource.ReplaceIds`
- dependent sample lookup values may reference those local target sample record IDs
- Yeeflow remaps target records and dependent lookup values together during app import

This was confirmed by the `Department Access Management_v5` export-back:

| Request | Generated lookup value | Exported-back lookup value | Exported target exists |
| --- | --- | --- | --- |
| DAR-1001 | `2060010000000011001` | `2053789859452960773` | yes |
| DAR-1002 | `2060010000000011002` | `2053789859452960774` | yes |
| DAR-1003 | `2060010000000011003` | `2053789859452960775` | yes |

If the grid display appears blank even though exported-back lookup values match exported-back target records, classify it as a runtime display/index/cache issue unless the item form lookup value or manually edited row is also broken.

## 4. Validator And Builder Updates

`validate-ydl-list.js` now supports resolved external lookup sample values when a dependency map is provided.

It allows non-empty external lookup sample values only when:

- the dependency map resolves the lookup target list
- the dependency map provides target sample/reference record IDs
- the sample lookup value matches one of those target record IDs

It fails when:

- external lookup sample values are unknown
- target reference records are not declared
- a wrapper contains external lookup sample values in `Resource.ReplaceIds`

`build-ydl-wrapper.js` now uses the dependency map to exclude resolved external IDs from `Resource.ReplaceIds`.

This keeps local generated IDs remappable while preserving already-imported external dependency IDs.

## 5. Safe Generation Rules

For standalone `.ydl` packages with external lookup sample data:

- Require a dependency map.
- Require target list metadata.
- Require target display field metadata.
- Require target sample/reference record IDs.
- Use real target `ListDataID` values in the dependent list sample rows.
- Exclude those target IDs from the dependent package `Resource.ReplaceIds`.
- Validate the decoded draft before build.
- Validate the wrapper after build.
- Do not build if lookup records are unresolved.

If reference records are unavailable, either omit sample rows or leave the lookup field empty in sample rows.

## 6. Sandbox Checklist

Use this checklist for related standalone `.ydl` tests:

1. Import the reference list first.
2. Confirm reference sample records import.
3. Export the reference list back from Yeeflow.
4. Extract the remapped reference `ListSetID`, `ListID`, display field, and record `ListDataID` values.
5. Patch the dependent list lookup Rules.
6. Patch dependent sample rows with real target `ListDataID` values if sample lookup data is required.
7. Build the dependent wrapper with the dependency map.
8. Confirm external target IDs are absent from dependent `Resource.ReplaceIds`.
9. Import the dependent list.
10. Verify the lookup field opens and searches the reference list.
11. Verify sample rows display lookup values instead of `(Deleted)`.
12. Export the dependent list back.
13. Confirm exported-back lookup values remain the real target `ListDataID` values.

## 7. Packaging Boundary

This staged pattern is suitable for sandbox testing of related standalone `.ydl` packages.

For multi-list production packages, app-level `.yap` generation may be safer because Yeeflow can remap related resources inside one application package. Until app-level package generation is proven, use staged `.ydl` import/export for related lists.

---

<a id="generated-yap-baseline-visitor-access-management-v5-fresh-compatible"></a>

<!-- source: docs/collection-02.md#generated-yap-baseline-visitor-access-management-v5-fresh-compatible -->

# Generated `.yap` Baseline: Visitor Access Management v5 Fresh-Compatible

This document records the first successful generated Visitor Access Management app-level `.yap` baseline.

## 1. Test Summary

The Visitor Access Management learning track proved an important app-level packaging rule: structural compatibility must be paired with fresh local IDs and fresh approval form keys.

Observed sequence:

- Earlier expanded Visitor app packages failed during import with `Created failed`.
- Isolation packages also failed, including reduced packages with only Departments, two lists without forms, blank request lookup samples, and full package without request samples.
- A control package based directly on the known-working Department Access Management v5 structure imported successfully.
- `visitor-access-management.v4-v5-compatible.yap` imported but opened as a blank app. It reused the old `206...` generated ID family and the `DAR` approval form key from an already-imported generated app.
- `visitor-access-management.v5-fresh-compatible.yap` imported and opened successfully after switching to a fresh local ID family and a fresh form key.

This means Yeeflow can accept a package whose root shell is close enough to import while still leaving components detached if IDs or form keys collide with already-imported generated artifacts.

## 2. Working Package

Working package:

```text
visitor-access-management.v5-fresh-compatible.yap
```

Working decoded source:

```text
visitor-access-management-app-def.v5-fresh-compatible.json
```

Confirmed sandbox result:

- App imports.
- App opens from the workspace card.
- App header and navigation render.
- `Departments` list opens.
- `Departments` list shows 3 sample departments.
- `Visitor Access Requests` list opens from navigation.
- `Visitor Access Request` approval form opens from navigation.

## 3. What The Working Baseline Includes

The v5 fresh-compatible baseline includes:

- root app shell
- root Type `103` app page/navigation structure
- `Departments` data list
- `Visitor Access Requests` data list
- Department lookup in the proven request-list `Text2` field slot
- Department Code in `Text3`
- Visit Purpose in `Text4`
- Visit Date in `Datetime2`
- Visitor Access Request approval form
- approval workflow `ContentList` persistence to `Visitor Access Requests`
- fresh approval page UUIDs
- fresh workflow node IDs
- fresh form key `VAV`

The working baseline intentionally stays close to the proven Department Access Management v5-compatible schema. It does not yet use the expanded 16-field Visitor request schema.

## 4. Key Rules Learned

Fresh ID and key rules:

- Each generated `.yap` app test should use a fresh local ID family.
- Each generated approval form inside a `.yap` should use a fresh FlowKey/form key.
- Do not reuse a local ID family from an already-imported generated app.
- Do not reuse an approval form key from an already-imported generated app unless intentionally updating the same app and the update behavior is understood.
- Reused IDs can import but leave the app blank or components detached.
- Use the proven v5-compatible app schema as the expansion baseline.

Native `Title` field rule:

- Keep `FieldName: "Title"` as Yeeflow's native primary/display field in every generated child data list.
- The required pattern is `Status: 0`, `IsSystem: true`, and `IsIndex: true`.
- Business labels such as `Request No.`, `Name`, or `Department Name` may be displayed on `Title`, but the metadata must remain native/system/indexed.
- Heep Hong IT eWorkflow Option A v7 proved that treating `Title` as a normal custom field can import and render metadata while breaking the row-data query endpoint. Option A v8 fixed that by restoring this baseline pattern.

Generator implication:

- A generated package should not be considered sandbox-ready only because local validators pass.
- The generator should track recently generated/imported ID families when that information is available.
- The generator should warn or fail when a new app package reuses a known generated/imported app ID family or form key.

## 5. Current Unproven Area

The expanded Visitor request schema is still unproven.

Known facts:

- The expanded 16-field Visitor request schema failed during import.
- The exact failing field/control pattern is not yet known.
- The successful v5 baseline uses the smaller v5-compatible schema and the proven field slots.

Future expansion should proceed one field at a time. Do not jump directly from the working v5-compatible baseline to a wide request schema.

## 6. Validator And Generator Recommendations

Recommended validator checks:

- Warn or fail if a generated app uses an ID family already seen in prior generated/imported test artifacts, when detectable.
- Warn or fail if a generated approval form reuses a FlowKey/form key already seen in prior generated/imported app tests, when detectable.
- Report ID family prefixes in validation output so humans can spot accidental reuse.
- Treat fresh generated ID families as a sandbox-readiness requirement for app-level package tests.

Recommended generator rules:

- Allocate a fresh root ListSetID/app ID family for every new app-level `.yap` test.
- Allocate fresh child list IDs, field IDs, layout IDs, sample record IDs, approval form IDs, page IDs, and workflow node IDs from that family.
- Allocate a fresh short form key per generated approval form, such as `VAV` for Visitor Access Request.
- Keep internal references consistent before build and include local IDs in `ReplaceIds`.
- Do not use a prior generated package as a template by only changing labels while preserving IDs, unless the goal is a controlled import/update experiment.

## 7. Visitor Access v6 Expansion Plan

Recommended next expansion test:

1. Start from `visitor-access-management.v5-fresh-compatible.yap`.
2. Add exactly one additional field to the `Visitor Access Requests` list.
3. Add the corresponding approval form variable/control only if the field should be collected on the request page.
4. Add the corresponding `ContentList` mapping only if the field should persist after approval.
5. Run component validation.
6. Run `validate-yap-package.js` in generator/final mode.
7. Run `validate-yap-graph.js` in generator/final mode.
8. Build a new `.yap` wrapper only after validation passes.
9. Import-test in sandbox.
10. Export back and compare before adding the next field.

Repeat one field at a time until the failing field/control pattern is identified.

## 8. Sandbox Checklist

For every Visitor Access expansion package:

- Import package.
- Confirm import popup shows logo, name, and description.
- Confirm app card appears.
- Open the app from the workspace card.
- Confirm root header and navigation render.
- Open `Departments`.
- Open `Visitor Access Requests`.
- Open `Visitor Access Request` approval form.
- Submit or draft-test the form if workflow testing is in scope.
- Export the app back.
- Compare app shell, child resources, approval form registration, navigation, `ReplaceIds`, and remapped IDs.

## 9. v6 One-Field Expansion Result

The first v6 package added exactly one new field, `Visitor Name`, to the working Visitor Access baseline.

Intended v6 change:

- Add `Visitor Name` to the `Visitor Access Requests` data list.
- Use field slot `Text8`.
- Preserve proven fields:
  - `Text2`: Department lookup
  - `Text3`: Department Code
  - `Text4`: Visit Purpose
  - `Datetime2`: Visit Date
- Add `Visitor Name` to the custom list form.
- Add `VisitorName` to the approval form request page.
- Add readonly `VisitorName` to the approval page.
- Add `VisitorName -> Text8` to workflow `ContentList` persistence.
- Add safe sample values.

### v6 Failure Lesson

The initial v6 package imported and opened, and the data lists appeared, but the `Visitor Access Request` approval form was missing from navigation.

Root cause:

```json
"ListID": "2100030000000000001"
```

The generated app-level approval form row incorrectly set `Data.Forms[0].ListID` to the approval form `ProcModelID`.

Correct rule:

```json
"ListID": 0
```

For app-level approval form registration inside `.yap`, `Data.Forms[].ListID` must remain `0`. The approval form identity belongs in `ProcModelID`, not `ListID`.

### v6 Formfix Lesson

Changing `Data.Forms[0].ListID` back to `0` was structurally correct, and the validator was updated to catch the earlier mistake.

However, the formfix package reused the same `210...` ID family and `VAN` form key after the broken v6 package had already been imported. That package opened as a blank/detached app shell.

Rule reinforced:

- After any generated `.yap` package has been imported, successful or failed, do not reuse that local ID family for another import-test package.
- Do not reuse that FlowKey/form key either.
- A package can import and open the root shell while child components detach because Yeeflow has already seen the generated IDs.

### v6.1 Success

`visitor-access-management.v6.1-add-visitor-name-fresh.yap` succeeded.

Confirmed successful settings:

- Fresh ID family: `211...`
- Fresh root app/listset ID: `2110000000000000001`
- Fresh request list ID: `2110020000000001000`
- Fresh form key: `VAW`
- Approval form `Data.Forms[].ListID = 0`
- Approval form `ProcModelID = 2110030000000000001`
- `Visitor Name` field slot: `Text8`
- `Visitor Name` field ID: `2110020000000001012`

Confirmed sandbox behavior:

- App imports.
- App opens.
- Navigation renders.
- `Departments` appears.
- `Visitor Access Requests` appears.
- `Visitor Access Request` approval form appears in navigation.
- `Visitor Name` renders on the approval form request page.

This validates the one-field expansion strategy.

## 10. Current Generator Rules

For generated app expansion tests:

- Start from a known-good baseline.
- Add exactly one field or behavior change at a time.
- Use a fresh ID family for every import-test package.
- Use a fresh FlowKey/form key for every import-test package.
- Keep app-level approval form `Data.Forms[].ListID = 0`.
- Put approval form identity in `ProcModelID`.
- Preserve proven field slots unless deliberately testing a new slot.
- Validate package, graph, approval form Def, and wrapper round trip before import.

## 11. v7 One-Field Expansion Result

`visitor-access-management.v7-add-visitor-company.yap` succeeded as the next one-field expansion from the working v6.1 baseline.

What changed:

- Added `Visitor Company`.
- Used data-list field slot `Text9`.
- Added workflow variable `VisitorCompany`.
- Added request page control.
- Added approval page readonly control.
- Added custom list form control.
- Added sample row values.
- Added workflow `ContentList` mapping: `VisitorCompany -> Text9`.

Confirmed successful settings:

- Fresh ID family: `212...`
- Fresh root app/listset ID: `2120000000000000001`
- Fresh request list ID: `2120020000000001000`
- Fresh form key: `VAX`
- Approval form `Data.Forms[].ListID = 0`
- Approval form `ProcModelID = 2120030000000000001`
- `Visitor Company` field slot: `Text9`
- `Visitor Company` field ID: `2120020000000001013`

Confirmed sandbox behavior:

- App imported.
- App opened.
- Navigation rendered.
- `Visitor Access Request` approval form appeared.
- `Visitor Name` rendered.
- `Visitor Company` rendered.
- `ContentList` persisted `VisitorCompany` to `Text9`.

## 12. Expansion Rule Confirmation

The v7 result confirms that one-field expansion from a known-good baseline is valid.

Safe text-field expansion pattern:

- Use a fresh ID family.
- Use a fresh FlowKey/form key.
- Preserve `Data.Forms[].ListID = 0`.
- Add the new text field to `Defs`.
- Add the new field to views if needed.
- Add the new field to the custom list form.
- Add the matching workflow variable.
- Add request-page and readonly approval-page controls.
- Add the `ContentList` mapping.
- Add sample row values only when they are local and safe.
- Re-run package, graph, approval form, and wrapper validations.

Current proven Visitor Access request schema:

| Field | Slot |
| --- | --- |
| Department lookup | `Text2` |
| Department Code | `Text3` |
| Visit Purpose | `Text4` |
| Visitor Name | `Text8` |
| Visitor Company | `Text9` |
| Access Area | `Text10` |
| Host Employee | `Text11` |
| Visitor Contact | `Text12` |
| Visit Date | `Datetime2` |

## 13. v8-v10 Incremental Expansion Results

After v7, the Visitor Access Management app was expanded one field at a time from the latest known-good baseline.

### v8 Success

`visitor-access-management.v8-add-access-area.yap` succeeded.

- Added `Access Area` as `Visitor Access Requests.Text10`.
- Used fresh ID family `213...`.
- Used fresh FlowKey/form key `VAY`.
- Preserved app-level approval form `Data.Forms[].ListID = 0`.
- Added the list field, custom list form control, approval request page control, approval page readonly control, sample row values, and `ContentList` mapping.
- App imported and opened correctly.
- `Access Area` rendered in the approval form.
- `ContentList` persisted `AccessArea` to `Text10`.

### v9 Success

`visitor-access-management.v9-add-host-employee.yap` succeeded.

- Added `Host Employee` as `Visitor Access Requests.Text11`.
- Used fresh ID family `214...`.
- Used fresh FlowKey/form key `VAZ`.
- Preserved app-level approval form `Data.Forms[].ListID = 0`.
- Added the list field, custom list form control, approval request page control, approval page readonly control, sample row values, and `ContentList` mapping.
- App imported and opened correctly.
- `Host Employee` rendered in the approval form.
- `ContentList` persisted `HostEmployee` to `Text11`.

### v10 Success

`visitor-access-management.v10-add-visitor-contact.yap` succeeded.

- Added `Visitor Contact` as `Visitor Access Requests.Text12`.
- Used fresh ID family `215...`.
- Used fresh FlowKey/form key `VBA`.
- Preserved app-level approval form `Data.Forms[].ListID = 0`.
- Added the list field, custom list form control, approval request page control, approval page readonly control, sample row values, and `ContentList` mapping.
- App imported and opened correctly.
- `Visitor Contact` rendered in the approval form.
- `ContentList` persisted `VisitorContact` to `Text12`.

## 14. Proven Incremental Expansion Chain

The current successful expansion chain is:

| Version | Field | Slot | ID family | FlowKey |
| --- | --- | --- | --- | --- |
| v6.1 | Visitor Name | `Text8` | `211...` | `VAW` |
| v7 | Visitor Company | `Text9` | `212...` | `VAX` |
| v8 | Access Area | `Text10` | `213...` | `VAY` |
| v9 | Host Employee | `Text11` | `214...` | `VAZ` |
| v10 | Visitor Contact | `Text12` | `215...` | `VBA` |

Current proven Visitor Access request schema:

| Field | Slot |
| --- | --- |
| Department lookup | `Text2` |
| Department Code | `Text3` |
| Visit Purpose | `Text4` |
| Visitor Name | `Text8` |
| Visitor Company | `Text9` |
| Access Area | `Text10` |
| Host Employee | `Text11` |
| Visitor Contact | `Text12` |
| Visit Date | `Datetime2` |

Generator rule:

- Expand from a known-good baseline one change at a time.
- Use a fresh ID family and fresh FlowKey/form key for every import-test package.
- Keep app-level approval form `Data.Forms[].ListID = 0`.
- Preserve proven field slots unless deliberately testing a new field type or slot.
- Update the data-list field, data-list views where useful, custom list form, approval request page, approval readonly page, workflow variable, sample rows, and `ContentList` mapping together.
- Validate the package, graph, and approval form before wrapper build.
- Build only after validation passes and round-trip comparison confirms decoded source equality.

Next recommended tests:

- Convert `Access Area` from text to a choice/dropdown.
- Test `Host Employee` as a user picker or lookup.
- Add dashboard/report resources only after the core app remains stable.

## 15. Manual v10 Field-Type Pattern Export

The exported file `/Users/rengerhu/Downloads/Visitor Access Management v10.yap` was manually modified in Yeeflow after the generated v10 baseline. It provides the first app-level export evidence for several field/control types.

Reference study:

- `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`

Confirmed from the export:

- Approval form number control uses variable `type = "number"` and control `type = "input_number"`.
- Number formatting/min/default settings are stored on the control as attrs/value.
- Data-list number field uses `FieldType = "Decimal"` and `Type = "input_number"`; the observed slot was `Decimal1`.
- Approval form single-select radio uses variable `type = "text"` and control `type = "radio"`.
- Approval form dropdown uses the same `radio` control with `attrs.displayStyle = "dropdown"`.
- Approval form switch uses variable `type = "boolean"` and control `type = "switch"`.
- Data-list switch field uses `FieldType = "Bit"` and `Type = "switch"`; the observed slot was `Bit1`.
- Conditional display is stored on the target control at `attrs.control_display[]`.

Important limitation:

- The manual export did not persist the new fields through `ContentList`.
- The new request-page controls were not mirrored onto the approval page.
- A generated package should add readonly approval-page mirrors and type-compatible `ContentList` mappings when persistence is required.

## 16. v11 Multi-Field, Multi-Type Expansion Result

`visitor-access-management.v11-five-fields-multitype.yap` succeeded as the first generated multi-field, multi-type expansion from the working v10 baseline.

Confirmed successful settings:

- Fresh ID family: `216...`
- Fresh root app/listset ID: `2160000000000000001`
- Fresh request list ID: `2160020000000001000`
- Fresh FlowKey/form key: `VBB`
- Approval form `Data.Forms[].ListID = 0`
- Approval form `ProcModelID = 2160030000000000001`
- `ReplaceIds` count: `46`

Confirmed runtime behavior:

- App imported and opened.
- Root navigation rendered.
- Data lists appeared.
- Approval form appeared.
- Multi-field expansion worked.
- Multi-type controls worked.
- The v11 wrapper round-trip, package validation, graph validation, and extracted approval form validation all passed before import testing.

### Proven v11 Field/Control Types

v11 moves the field-type study from manual export evidence to generated package evidence.

| Type | Approval control | Data-list field pattern | Proven in v11 |
| --- | --- | --- | --- |
| Text | `input` | `FieldType = "Text"`, `Type = "input"` | yes |
| Number | `input_number` | `FieldType = "Decimal"`, `Type = "input_number"` | yes |
| Single select dropdown | `radio` plus `attrs.displayStyle = "dropdown"` | `FieldType = "Text"`, `Type = "radio"` | yes |
| Switch/boolean | `switch` | `FieldType = "Bit"`, `Type = "switch"` | yes |
| Conditional display | target control `attrs.control_display[]` | approval-form-only behavior | yes |

### Proven v11 Storage Mappings

| Business field | Storage slot | Field/control type |
| --- | --- | --- |
| Visitor Email | `Text13` | text/input |
| Visitor Phone | `Text14` | text/input |
| Number of Visitors | `Decimal1` | Decimal/input_number |
| Access Type | `Text15` | Text/radio dropdown |
| Requires Escort | `Bit1` | Bit/switch |

### Proven v11 ContentList Mappings

| Approval variable | Target field |
| --- | --- |
| `VisitorEmail` | `Text13` |
| `VisitorPhone` | `Text14` |
| `NumberofVisitors` | `Decimal1` |
| `AccessType` | `Text15` |
| `RequiresEscort` | `Bit1` |

### Sample Value Shapes

| Field type | Sample value shape |
| --- | --- |
| Decimal | numeric values such as `1`, `2`, `4` |
| Choice/dropdown | selected option text such as `Vendor Service` |
| Bit/switch list sample | string `"1"` or `"0"` |
| Boolean approval control | boolean `true` or `false` |

### Conditional Display

`Escort User` was added as a form-only conditional field:

- workflow variable: `EscortUser`
- request-page control: text input
- visibility rule: show when `RequiresEscort == true`
- storage location: target control `attrs.control_display[]`
- persistence: not persisted in v11

This keeps v11 as a five persisted-field expansion while still proving conditional display in a generated app package.

### Updated Generator Rule

Multi-field expansion is allowed after the underlying field/control types have been proven by export-backed examples and at least one generated import test.

For generated `.yap` app expansion:

- Start from a known-good baseline.
- Use a fresh ID family and fresh FlowKey/form key for every import-test package.
- Keep app-level approval form `Data.Forms[].ListID = 0`.
- Preserve proven slots unless deliberately testing a new slot or field type.
- Update data-list fields, custom list forms, approval request pages, approval readonly pages, workflow variables, sample rows, and `ContentList` mappings together.
- Validate package, graph, extracted approval form Def, and wrapper round-trip before import testing.

Remaining gaps after v11:

- Persisted conditional field, such as `EscortUser -> Text16`.
- Multi-select choice.
- User picker.
- Lookup inside a line-item table.
- Dashboards and reports beyond the root Type `103` shell page.

---

<a id="it-hardware-capex-request-runtime-v2-ui-study"></a>

<!-- source: docs/collection-02.md#it-hardware-capex-request-runtime-v2-ui-study -->

# IT Hardware CAPEX Request Runtime V2 UI Study

Source studied read-only:

- `<downloads>/IT Hardware CAPEX Request Runtime V2.yap`

Comparison target:

- `it-hardware-capex-request-approval-form-def.v2.json`

This export is the manually improved version of the generated IT Hardware CAPEX Request app. The submission form was changed in Yeeflow and exported back. The V2 export should now be treated as the current UI-quality evidence for generated Yeeflow approval forms.

## Export Shape

- Wrapper file type: JSON data.
- Resource payload: compressed `Resource` with `[______gizp______]` prefix.
- Decoded form: `IT Hardware CAPEX Request`.
- `Data.Forms[].ListID = 0`.
- Submission page: `IT Hardware CAPEX Request Form`, `type = 1`.
- Task page: `IT Hardware CAPEX Request Task`, `type = 2`.

Large numeric IDs were preserved as source values during study. No raw export is bundled in this repo or skills.

## Page Background

Manual V2 adds page-level background to the submission form:

```json
{
  "formdef": {
    "attrs": {
      "container": {
        "cw": "2",
        "padding": [null, {
          "top": "--sp--s0",
          "right": "--sp--s0",
          "bottom": "--sp--s0",
          "left": "--sp--s0"
        }]
      },
      "background": {
        "type": "classic",
        "classic": {
          "color": "var(--c--neutral-light)"
        }
      }
    }
  }
}
```

Generation rule:

- Put full-page form background on `page.formdef.attrs.background`.
- Keep `Main` primarily structural.
- Avoid using `Main.attrs.common.background` for the full-page background.
- `Content` may still carry neutral background and padding when it is acting as the page content surface.

## Form Header

Manual V2 inserts a distinct `Form header` container inside `Form body`. The generated v2 form placed `Request summary panel` directly under `Form body`.

Manual V2 `Form header` pattern:

```json
{
  "type": "container",
  "nv_label": "Form header",
  "attrs": {
    "style": {
      "gap": [null, "--sp--s0"],
      "overflow": [null, "hidden"]
    },
    "common": {
      "background": {
        "normal": {
          "type": "classic",
          "classic": { "color": "#ffffff" }
        }
      },
      "border": {
        "normal": {
          "type": "1",
          "width": [null, { "top": 1, "right": 1, "bottom": 1, "left": 1 }],
          "color": "#e7e9eb",
          "radius": [null, { "top": 12, "right": 12, "bottom": 12, "left": 12 }]
        }
      }
    }
  }
}
```

Generation rule:

- Wrap the request summary and metric row in `Form header`.
- Use `attrs.style.overflow = [null, "hidden"]` when rounded corners and nested visual panels are used.
- Give `Form header` its own background, border, and radius.

## Request Summary Panel And Custom CSS

Manual V2 uses custom CSS on `Request summary panel`:

```json
{
  "nv_label": "Request summary panel",
  "attrs": {
    "common": {
      "css": "selector\n{\n    background-image: linear-gradient(to right, oklch(0.546 0.245 262.881) 0%, oklch(0.541 0.281 293.009) 100%) !important;\n}",
      "ty": {
        "normal": {
          "color": "var(--c--background)"
        }
      }
    }
  }
}
```

Generation rule:

- Use native background/border/radius first.
- When the design requires a gradient and Yeeflow native background settings do not support it, set `attrs.common.css` on the target container.
- Use the `selector { ... }` CSS shape shown above.
- Set container text color intentionally through `attrs.common.ty.normal.color`; child headings/text can inherit unless they require local overrides.

## Request Metric Row

Manual V2 keeps `Request metric row` as a row container and styles child metric cards.

Row:

- `nv_label = "Request metric row"`
- `type = "container"`
- `attrs.style.direction = [null, "row"]`
- `attrs.style.gap = [null, "--sp--s200"]`
- `attrs.common.padding = [null, { top/right/bottom/left: "--sp--s250" }]`

Metric card:

- `type = "container"`
- `nv_label` examples: `Status metric`, `Cost metric`, `Owner metric`, `Next step metric`
- `attrs.style.direction = [null, "column"]`
- `attrs.style.gap = [null, "--sp--s050"]`
- `attrs.common.padding = --sp--s300` on all sides
- `attrs.common.background.normal.classic.color = "var(--c--neutral-light)"`
- `attrs.common.border.normal.type = "1"`
- `attrs.common.border.normal.color = "var(--c--neutral-light-active)"`
- `attrs.common.border.normal.radius = 8` on all corners

Metric labels/values are `heading` controls. After studying `Text Style Sample.ywf`, generated metric text should use:

- label: `attrs.heads.ty = [null, "xs-medium"]`
- value: `attrs.heads.ty = [null, "h5-medium"]`
- color: plain string token such as `attrs.heads.color = "var(--c--text)"`
- both use `attrs.common.positioning.widthtype = [null, "2"]`

## Text Control Quality

Manual V2 corrected generated text controls by adding explicit width/typography style.

Corrected heading:

```json
{
  "type": "heading",
  "nv_label": "Applicant Information heading",
  "attrs": {
    "heads": {
      "ty": [null, "h5-medium"],
      "color": "var(--c--text)"
    },
    "common": { "positioning": { "widthtype": [null, "2"] } }
  }
}
```

Corrected descriptive text:

```json
{
  "type": "text-editor",
  "nv_label": "Applicant Information description",
  "attrs": {
    "common": {
      "padding": [null, {
        "top": "--sp--s0",
        "right": "--sp--s0",
        "bottom": "--sp--s0",
        "left": "--sp--s0"
      }],
      "positioning": { "widthtype": [null, "2"] }
    },
    "ty": { "size": [null, 14] },
    "color": "var(--c--neutral-dark-hover)"
  }
}
```

Generation rule:

- Generated `heading` and `text-editor` controls should default to `attrs.common.positioning.widthtype = [null, "2"]`.
- Use `Text Style Sample.ywf` as the focused Text control source of truth: `attrs.heads.ty = [null, "h5-medium"]` for named presets, `attrs.heads.color = "var(--c--text)"` as a plain string, and `attrs.common.positioning.widthtype = [null, "2"]`.
- Dynamic Text controls use the same native `heading` control with `attrs.headc.title.variable[]`; links live under `attrs.headc.link`.
- Old generated shapes with `attrs.heads.color = [null, "var(--c--text)"]` should be replaced because they can leave Yeeflow designer style popups unresponsive.
- Generated visual Text/heading controls should use native-like UUID IDs where safe, not generator-specific `ctrl-*` IDs, because the manually added working controls use plain UUIDs and export back with the full style shape intact.
- Keep helper/description text padding at zero.
- Use meaningful `nv_label` values for every generated heading and helper text.

## Icon Badge Pattern

Manual V2 wraps section icons in a square container.

Wrapper:

```json
{
  "type": "container",
  "attrs": {
    "style": {
      "widthtype": [null, "3"],
      "width": [null, 46],
      "height": [null, "2"],
      "cushei": [null, 46],
      "align_items": [null, "center"],
      "justify_content": [null, "center"],
      "gap": [null, "--sp--s0"]
    },
    "common": {
      "border": {
        "normal": {
          "radius": [null, { "top": 16, "right": 16, "bottom": 16, "left": 16 }]
        }
      },
      "background": {
        "normal": {
          "type": "classic",
          "classic": { "color": "var(--c--primary-light)" }
        }
      }
    }
  }
}
```

Icon:

```json
{
  "type": "icon",
  "attrs": {
    "icon": {
      "icon": "fa-regular fa-file-lines",
      "size": [null, 16],
      "normal": { "pcolor": "var(--c--primary)" }
    },
    "common": {
      "positioning": { "widthtype": [null, "2"] }
    }
  }
}
```

Generation rule:

- Use square icon containers for section badges.
- Set container width and height to the same px value.
- Center children with `align_items` and `justify_content`.
- Set icon width inline.
- Icon size should be roughly half the wrapper width.

## Field Grid Layout

Manual V2 moves normal field controls into `flex_grid` containers.

Standard grid:

```json
{
  "type": "flex_grid",
  "label": "Grid",
  "attrs": {
    "ver": 1,
    "columns": {
      "1": { "list": [{ "value": 1, "unit": "fr" }, { "value": 1, "unit": "fr" }], "last": { "value": 1, "unit": "fr" } },
      "2": { "list": [{ "value": 1, "unit": "fr" }, { "value": 1, "unit": "fr" }], "last": { "value": 1, "unit": "fr" } },
      "3": { "list": [{ "value": 1, "unit": "fr" }], "last": { "value": 1, "unit": "fr" } }
    },
    "rows": { "1": { "list": [{ "unit": "auto" }], "last": { "unit": "auto" } } },
    "cgap": { "1": 10 },
    "cgapU": { "1": "px" }
  }
}
```

Generation rule:

- Standard form field sections use a `flex_grid`.
- Use two desktop/tablet columns and one mobile column.
- Set `displayLabel = [null, false]` on generated `flex_grid` layout controls so the grid caption is not displayed.
- Put normal controls in the grid.
- Place textarea, rich text, list/sublist, and long helper content outside the two-column grid or make them span the full row once column-span export evidence is available.

## Repaired Runtime-Sensitive Controls

Manual V2 replaced several generated controls by re-adding native Yeeflow controls.

Observed repaired patterns:

| Control | Generated v2 | Manual V2 |
| --- | --- | --- |
| Location | `location-picker` with `attrs.required` and `attrs.placeholder` | `location-picker` with empty `attrs` and a new generated binding such as `LocationID` / `delivery_location2` |
| Cost center | `cost-center-picker` with `attrs.required` and `attrs.placeholder` | `cost-center-picker` with empty `attrs` and new binding `field_81` |
| Icon upload / image | `icon-upload` with `attrs.maxCount` | `icon-upload` with `attrs.controlmultiple = true` and new binding `supporting_images2` |
| File upload | no `attrs.ver` | `file-upload` with `attrs.ver = 1` |

Generation rule:

- Treat location, department/organization, cost center, metadata, and image/icon upload as runtime-sensitive until more focused export/import tests prove stable generated shapes.
- Prefer fallback text fields when production-like reliability matters.
- If native controls are explicitly requested, use the minimal exported-back shape and document that environment metadata may still be needed.

## Calculated Field

Manual V2 replaced the editable Subtotal input with a native calculated control:

```json
{
  "type": "calculated",
  "label": "Sub total",
  "binding": "subtotal",
  "attrs": {
    "calculated": [
      {
        "exprType": "variable",
        "valueType": "number",
        "id": "quantity",
        "type": "expr",
        "name": "Workflow Variables:Quantity"
      },
      { "type": "op", "op": "*" },
      {
        "exprType": "variable",
        "valueType": "number",
        "id": "unit_price",
        "type": "expr",
        "name": "Workflow Variables:Unit Price"
      }
    ]
  }
}
```

Generation rule:

- `Subtotal = Quantity * Unit Price`.
- Fields named `Subtotal`, `Total`, `Amount`, `Balance`, `Difference`, or `Duration` should be reviewed for calculation intent.
- Do not generate editable input controls for calculated-looking fields without a documented reason.
- If a safe Yeeflow calculated expression cannot be built, generate readonly display and document calculation deferred.

## Root-Cause Classification

Bug/risk fixes:

- Full-page background was missing at page level.
- Request summary lacked a wrapper `Form header`.
- Text controls lacked enough positioning/typography information for consistent rendering.
- Normal fields were placed directly in field containers instead of grids.
- Some runtime-sensitive controls used generated attrs that did not match the manually re-added controls.
- `Subtotal` was editable instead of calculated.

Design improvements:

- Gradient request summary through `attrs.common.css`.
- Metric row inside header, with clear card treatments.
- Square icon badges.
- Better text hierarchy and section grouping.

Known gaps:

- Column span property for long controls inside `flex_grid` still needs a focused export if the generator wants to place them inside the grid.
- Department/organization and metadata controls were not present in this V2 export, so they remain runtime-sensitive and should fall back or require focused proof.

---

<a id="procurement-management-yap-study"></a>

<!-- source: docs/collection-02.md#procurement-management-yap-study -->

# Procurement Management `.yap` Structure Study

Source export studied read-only: `/Users/rengerhu/Downloads/Procurement Management.yap`

No import, UI operation, package generation, base64 encoding, or modification of the original export was performed.

## 1. Application Package Anatomy

The `.yap` file is plain JSON, not a ZIP archive.

Top-level wrapper:

| Field | Observed value / purpose |
| --- | --- |
| `Title` | `Procurement Management` |
| `Description` | `null` |
| `IconUrl` | JSON string with icon/background/color data |
| `IsListSet` | Boolean app/listset flag |
| `Resource` | Main package payload string |

`Resource` is prefixed with `[______gizp______]`, then base64-encoded gzip data. After base64 decode and gunzip, it becomes JSON with:

| Resource field | Observed shape |
| --- | --- |
| `MainListType` | `1024` |
| `AppID` | `41` |
| `ReplaceIds` | 421 IDs to remap on import |
| `ReportIds` | 31 report/resource IDs to remap |
| `FormKeys` | 18 workflow/form keys |
| `Data` | JSON string containing app resources |
| `SimplePortal` | `null` |

`Data` parses into:

| Section | Count | Purpose |
| --- | ---: | --- |
| `Item` | 1 | Main app/listset resource |
| `Childs` | 20 | Data lists, lookup lists, document libraries, report/list resources |
| `Forms` | 18 | Approval/list/job workflows with embedded decoded definitions |
| `FormReports` | 0 | No legacy form reports observed |
| `DataReports` | 1 | Data report pipeline definition |
| `FormNewReports` | 1 | Form/report definition tied to `PR` |
| `AppGroups` | 1 | App grouping/navigation metadata |
| `AppThemes` | 1 | Theme config |
| `OtherModules` | 2 | Includes AI Agent resources and another module |

Approval forms are embedded in `Data.Forms[].DefResource` as decoded JSON strings, not `.ywf` wrappers and not base64 `Def` payloads. Each embedded form definition has the same inner shape as decoded `.ywf` Def JSON: `childshapes`, `pageurls`, `variables`, `defkey`, `workflowType`, graph layout, etc.

Important precision note: some IDs in the `.yap` are JSON numbers larger than JavaScript's safe integer range. Future tooling should use lossless JSON parsing for identifiers or preserve raw strings; do not rely on normal JS numeric parsing for exact ID round-trips.

## 2. Resource Inventory

Main app/listset ID observed: `1906896262421815296`; AppID: `41`.

| Resource name | Resource type | Internal ID | ListSetID | ListID | Related form/workflow | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| Procurement Management | app/listset | AppID `41` | `1906896262421815296` | `1906896262421815296` | all resources | Type `1024` |
| PO Details List | data/detail list | list resource | `1906896262421815296` | `1906896273986686977` | PO, STSP | PO line items/detail rows |
| PR Records | transaction list | list resource | same | `1906896274213179393` | PR, PO, PaymentList_PO | PR header persistence |
| PR Details List | detail list | list resource | same | `1906896274422894593` | PR, PO | PR line items |
| PO Records | transaction list | list resource | same | `1906896274515169281` | PO, UPDTSP, PaymentList_PO | PO header persistence |
| PO-Payment Terms | transaction/detail list | list resource | same | `1906896274674552832` | PO, PA, PaymentList_PO | Payment schedule rows |
| PA Records | transaction list | list resource | same | `1906896274842324993` | PA | Payment approval records |
| Product Item | lookup/master list | list resource | same | `1906896802502549505` | PR item lookup | Product catalog |
| Supplier list _Pre | staging/master list | list resource | same | `1906933798931406851` | PM-NSF, risk reports | Pre-approved supplier staging |
| Supplier and PO Details | report/source resource | list resource | same | `1912027630285627393` | DataReport | Type `64` |
| Public Form Config | config list | list resource | same | `1960150936844115972` | supplier public form | Config metadata |
| Public Form Data Verification | verification list | list resource | same | `1960151032734294017` | PRO-SAW, PM-SRSYNC | Duplicate/submission verification |
| Supplier registration | supplier registration list | list resource | same | `1960152092957876226` | PM-SRSYNC, SM-GRR_1_2 | Public supplier submission source |
| Purchase Category | lookup/master list | list resource | same | `1960206948123807745` | PR | Purchase category lookup |
| Supplier List | master list | list resource | same | `1960231695198068738` | PO, PA, PRO-SAW, PM-SRSYNC | Approved suppliers |
| Template Files | document library | list resource | same | `1960538801452093441` | PO generation dependency | Type `16` |
| Compliance | data list | list resource | same | `1960570968060203009` | not clearly linked in inspected workflows | Compliance reference |
| PO Documents | document library | list resource | same | `1960582738103504896` | PO | Generated PO file storage |
| PR | form report/list resource | list resource | same | `1965675169026936833` | PR report, AI Agent query | Type `32` |
| Token List | credential/config list | list resource | same | `2001104767822860289` | RSTP/RSTJ/SP sync | Contains token/client fields; no values exposed |
| Approval rules | config/lookup list | list resource | same | `2049386197824581633` | not clearly wired in inspected approvals | Approver config |
| Supplier and PO Details | DataReport | `1912027630285627400` | same | inputs: PO Details, Supplier list _Pre | dashboard/report source | Joins supplier to PO detail data |
| PR | FormNewReport | `1965675169026936800` | same | `1965675169026936833` | PR | Report over PR workflow variables |
| Purchase Request Insight Agent | AI Agent | `1966368581843030016` | same | `1965675169026936833` | PR AI action | Uses PR Query tool |

## 3. Approval Form Inventory

| Form | FlowKey | Type | Request page | Approval page(s) | basic/listref | Approval stages | Persistence / special logic |
| --- | --- | ---: | --- | --- | --- | --- | --- |
| Purchase Request | `PR` | 2 | Purchase Request | PR Approval Form | 18 / 1 | Line Manager, Budget Owner, Finance, Purchasing Dept | Adds PR Records and PR Details; AI summary/action; amount gateway |
| Purchase Order Request | `PO` | 2 | Purchase Order Request | Purchase Order Approval | 38 / 2 | PO Approval | Adds/edits PO, PR, payment terms, PO detail rows; GenerateDocument; PO library save |
| Payment Request | `PA` | 2 | Payment Request | Payment Request Approval | 19 / 1 | Payment Approval | Adds/removes PA Records; updates payment terms and paid status |
| New Supplier Form | `PM-NSF` | 2 | Supplier Selection Form | Supplier Approval Form | 31 / 3 | Initial Review, First-level Evaluation, Detailed Evaluation, Risk Assessment, Approval and Feedback | Adds Supplier list _Pre |
| Supplier Approval Workflow | `PRO-SAW` | 1 | none | Requester Revising, Supplier Approval | 3 / 0 | Requester Revising, Supplier Admin Verification, Finance Verification, HOD Approval | Updates Supplier List status, duplicate key, risk report; AI risk report |
| Update PR Status to Closed | `PRClosed` | 1 | none | none | 1 / 0 | none | List-trigger style status update to PR Records |
| Update PO Status to Paid | `POClosed` | 1 | none | none | 1 / 0 | none | List-trigger style status update to PO Records |
| Generate risk report | `SM-GRR_1` | 1 | none | none | 2 / 0 | none | AI risk report to Supplier list _Pre |
| Generate risk report | `SM-GRR_1_2` | 1 | none | none | 2 / 0 | none | AI risk report to Supplier registration |
| Generate risk report | `SM-GRR_1_2_1` | 1 | none | none | 2 / 0 | none | AI risk report to Supplier List |
| Payment Update to PO | `PaymentList_PO` | 1 | none | none | 0 / 0 | none | List-field-trigger updates PR/PO paid amounts |
| Supplier Information Update | `PM-SRSYNC` | 1 | none | none | 0 / 0 | none | Syncs Supplier registration into Supplier List |
| Purchase Category Data SP To Yeeflow Job | `SPY` | 3 | SharePoint Update To Yeeflow | none | 7 / 3 | none | Scheduled SharePoint sync job |
| Product Item SP To Yeeflow Job | `PIJob` | 3 | SharePoint Update To Yeeflow | none | 8 / 3 | none | Scheduled product sync job |
| Refresh SharePoint Token | `RSTP` | 1 | none | none | 3 / 1 | none | Updates Token List |
| Refresh SharePoint Token Job | `RSTJ` | 3 | Refresh SharePoint Token Job | none | 0 / 0 | none | Scheduled token refresh |
| PO Data To SP | `UPDTSP` | 1 | none | none | 4 / 2 | none | SharePoint update helper |
| PO Details Data To SP | `STSP` | 1 | none | none | 5 / 2 | none | SharePoint detail update helper |

## 4. Data List and Field Mapping Study

Important lists and representative fields:

| List | ListID | Field display label | Internal field name | Field type/control | Used by |
| --- | --- | --- | --- | --- | --- |
| PR Records | `1906896274213179393` | PR#, Applicant, Request Date, Subject, Purchase Category, Reason, Est. Total Amount, PR Link, Actual Ordered Amt, Actual Payment Amt | `PR_No_`, `Applicant`, `Request_Date`, `Subject`, `Purchase_Category`, `Reason`, `Estimate_Total_Amount`, `PR_Details`, `Total_Amount_of_Actual_Orders`, `Total_Amount_of_Actual_Payment` | text/user/date/number/hyperlink | PR add, PO lookup/edit, payment update |
| PR Details List | `1906896274422894593` | Item, Description, Qty., Unit Price, Subtotal, PR No., Status, Unit | `Name`, `Style`, `Quantity`, `Unit_Price`, `Estimate_Subtotal_Amount`, `PR_No_`, `Status`, `Unit` | text/number/radio | PR detail add, PO item lookup/edit |
| PO Records | `1906896274515169281` | PO No., PR No., Applicant, Supplier, Bank fields, Contract dates/files, PO Total Amount, Paid/Pending, PO file | `PO_No_`, `PR_No_`, `PO_Applicant`, `Supplier`, `Openning_Bank`, `Bank_Account_No_`, `Contract_Start_Date`, `Contract_Attachments`, `PO_Total_Amount`, `PO_Total_Paid_Amount`, `PO_file` | text/user/lookup/file/number | PO add/edit, PA/payment update, SharePoint sync |
| PO Details List | `1906896273986686977` | PR No., PO No., Purchase Item, Order Qty, Unit Price, Subtotal, Supplier, Unit | `PR_No_`, `PO_No_`, `Name`, `Order_Quantity`, `Order_Unit_Price`, `Subtotal_Amount`, `Supplier`, `Unit` | text/number/lookup | PO detail add, DataReport, SharePoint detail sync |
| PO-Payment Terms | `1906896274674552832` | Percentage, Payment Amount, Payment Term, PR#, PO#, PA#, Supplier, Paid Date | `Percentage___`, `Payment_Amount`, `Payment_Terms`, `PR_No_`, `PO_No_`, `PA_Payment_No_`, `Supplier`, `Paid_Date` | percent/number/text/lookup/date | PO add, PA lookup/edit |
| PA Records | `1906896274842324993` | PR#, PO#, PA No., PA Link, Payment Amount, Applicant, Supplier, Paid Date | `PR_No_`, `PO_No_`, `PA_No_`, `Payment_Details`, `Payment_Amount`, `Applicant`, `Supplier`, `Paid_Date` | text/hyperlink/number/user/lookup/date | PA add/update/remove |
| Product Item | `1906896802502549505` | Item Name, Unit, Description, Product Category, Price USD, Unit Price, Margin, Status | `Title`, `SKU`, `Description`, `Product_Type`, `Price_USD`, `Unit_cost`, `Margin`, `Status` | text/lookup/currency/calculated/radio | PR line item lookup |
| Purchase Category | `1960206948123807745` | Purchase Category, CategoryID, Description, Status, SPID | `Title`, `CategoryID`, `Description`, `Status`, `SPID` | text/radio | PR category lookup |
| Supplier List | `1960231695198068738` | Supplier Name, contacts, tier/status, address, bank fields, product/service lists, files, approval status | many `Text*` fields | text/radio/file/list | PO/PA lookup, supplier approval/sync |
| Supplier list _Pre | `1906933798931406851` | Supplier Name, contacts, tier/status, address, risk report, bank fields | many `Text*` fields | text/radio/richtext/date | PM-NSF, risk reports, DataReport |
| Template Files | `1960538801452093441` | Upload File | `FileInfo` | file-upload | PO generation dependency |
| PO Documents | `1960582738103504896` | Name, Upload File | `Title`, `FileInfo` | input/file-upload | PO generated file save |
| Token List | `2001104767822860289` | clientid, client_secret, token fields, access_token | `clientid`, `client_secret`, `access_token`, etc. | text/textarea/date | token refresh/sync workflows; do not expose values |

## 5. Approval Form to Data List Relationship Map

Primary ContentList relationships:

| Approval form | ContentList node | Operation | Target list | Representative mappings |
| --- | --- | --- | --- | --- |
| PR | PR Record | add | PR Records | `PRid -> PR#`, `Applicant -> Applicant`, `Subject -> Subject`, `PurchaseCategory -> Purchase Category`, `EstimateTotalAmount -> Est. Total Amount`, Form Url -> PR Link |
| PR | PR Details Records | add | PR Details List | `PurchaseDetailsList.Name -> Item`, `.Quantity -> Qty.`, `.UnitPrice -> Unit Price`, `.EstimateSubtotalAmount -> Subtotal`, `PRid -> PR No.` |
| PO | Add_PO Record | add | PO Records | `POid -> PO No.`, `PRNO -> PR No.`, `Supplier -> Supplier`, contract fields/files, `POTotalAmount -> PO Total Amount` |
| PO | Add_PA terms | add | PO-Payment Terms | `PaymentTermList.Percentage -> Percentage`, `.PaymentAmount -> Payment Amount`, `.PaymentTerms -> Payment Term`, `POid -> PO#` |
| PO | Update_PR Items | edit | PR Details List | where `ListDataID == ChoosePurchaseItems`; sets `Status = Ordered` |
| PO | Add_PO Items | add | PO Details List | `POPurchaseDetailsList.Item/Quantity/OrderUnitPrice/SubtotalAmount -> PO detail row fields` |
| PO | Release/Update PR Record | edit | PR Records | where `PR_No_ == PRNO`; updates ordered/processing amounts and status |
| PO | Delete PO Record | remove | PO Records | where `PO_No_ == POid` |
| PO | Approved_PO Record | edit | PO Records | where `PO_No_ == POid`; sets status Approved, approved date, PO file |
| PO | Save PO to Library | add | PO Documents | `_Path = SupplierName`, `Name = POid`, `FileInfo = PO_File` |
| PA | Add_PA | add | PA Records | `PRNo`, `PONo`, `PAid`, Form Url, `PaymentAmount`, Applicant, Supplier |
| PA | Update_PO-PaymentTerm | edit | PO-Payment Terms | where `ListDataID == Payment_List.PaymentItemId`; updates PA#, PA link, status |
| PA | Update_PA | edit | PA Records | where `PA_No_ == PAid`; sets Paid and Paid Date |
| PA | Clear_PA | remove | PA Records | where `PA_No_ == PAid` |
| PM-NSF | Create supplier record | add | Supplier list _Pre | business/contact/finance variables -> supplier staging fields |
| PRO-SAW | Set Data List / Supplier approved / Update risk report | add/current-list update pattern | Supplier List | status changes, approver comments, AIResponse -> Risk Report |
| PM-SRSYNC | Update/Add Supplier Information | edit/add | Supplier List | list field values from Supplier registration -> Supplier List |
| PaymentList_PO | Update PR/PO paid amounts | edit | PR Records / PO Records | list_field `Payment_Amount` -> paid amount fields |

Observed data expression formats:

- Literal string in `Data`: `"Approved"`, `"Pending"`, `"Processing"`.
- Workflow variable HTML button: `<input type="button" data="${&quot;type&quot;:&quot;variable&quot;,...}" ...>`.
- List row workflow variable: same HTML with `"t":"list"` and `prop` for row field.
- Expression array: `[{"exprType":"variable","valueType":"file","id":"PO_File","type":"expr"}]`.
- List-trigger field expression: `[{"exprType":"list_field","prop":"Decimal2","id":"Payment_Amount","type":"expr"}]`.
- Function expression: `[{"type":"func","func":"now","params":[]}]`.

## 6. Lookup Relationship Map

| Form/control | Variable | Lookup source | AppID/ListSetID/ListID | Display field | Used for |
| --- | --- | --- | --- | --- | --- |
| PR / Purchase Category | `Select_PurchaseCategory` | Purchase Category | `41 / 1906896262421815296 / 1960206948123807745` | `Title` | Category selection; filters active/on status via `Text3 = ON` |
| PR line / Purchase Item | `PurchaseDetailsList.ProductService` | Product Item | `41 / 1906896262421815296 / 1906896802502549505` | `Title` | Line item catalog lookup; filtered by selected category and active/on status |
| PO / Choose Purchase Request | `ChoosePurchaseRequest` | PR Records | `41 / 1906896262421815296 / 1906896274213179393` | `Text1` | Select approved/open PR; excludes Closed and requires remaining quantity |
| PO / Choose PR Items | `ChoosePurchaseItems` | PR Details List | `41 / 1906896262421815296 / 1906896274422894593` | `Text1` | Select pending PR line rows into `POPurchaseDetailsList` |
| PO / Supplier | `Supplier` | Supplier List | `41 / 1906896262421815296 / 1960231695198068738` | `Title` | Select active supplier |
| PA / Supplier | `Supplier` | Supplier List | same | `Title` | Filter payment terms by supplier |
| PA / Payment Records | `ChoosePaymentTerm` | PO-Payment Terms | `41 / 1906896262421815296 / 1906896274674552832` | `Text2` | Select pending payment schedule rows into `Payment_List` |
| Supplier form / Product/Service | `list_ProductsandService.ps_category` | external/missing in this package | `41 / 1792014809381478401 / 1792014845504000003` | `Title` | Supplier offered products/services; source list not included in package inventory |

## 7. Nested List / Detail Row Pattern Study

| Form | List variable | listref | Row fields | Total/summary | Persisted target |
| --- | --- | --- | --- | --- | --- |
| PR | `PurchaseDetailsList` | `listref_1` | Name, Description, Quantity, UnitPrice, EstimateSubtotalAmount, SKU, No, ProductService | `EstimateSubtotalAmount` total -> `EstimateTotalAmount`; `No` max -> `SumOfItem` | PR Details List |
| PO | `POPurchaseDetailsList` | `listref_1` | Item, Description, Quantity, EstimatedUnitPrice, OrderUnitPrice, SubtotalAmount, Remarks, PRitemID, No, Unit | PO total amount variable | PO Details List; also edits PR Details rows |
| PO | `PaymentTermList` | `listref_2` | Percentage, PaymentAmount, PaymentTerms | total contract payment validation | PO-Payment Terms |
| PA | `Payment_List` | `listref_1` | PO, PaymentTerm, Percentage, Amount, PaymentItemId, PR, PRlink, POlink | Payment Amount derived from selected terms | PO-Payment Terms updates |
| PM-NSF | `list_ProductsandService` | `listref_1` | ps_category, ps_Description | none observed | stored as list field in supplier registration/staging, not child ContentList rows |
| PM-NSF | `list_Catalogs` | `listref_2` | catalog_name, catalog_file | none observed | list field |
| PM-NSF | `list_ClientReferences` | `listref_3` | client company/contact/email/phone/service/feedback | none observed | list field |

Important pattern: ContentList `add` can persist one workflow list variable into multiple target list rows by using expression-button values with `"t":"list"` and a row `prop`. Parent-child linkage is usually a copied key such as `PRid`, `POid`, `PRNO`, or row `ListDataID`, not a formal foreign-key object.

## 8. Dashboard and Report Dependency Study

`DataReports[0]` is `Supplier and PO Details`. It reads:

- PO Details List `1906896273986686977`: Supplier (`Text6`), Status (`Title`), Subtotal Amount (`Decimal3`).
- Supplier list _Pre `1906933798931406851`: Supplier Name (`Title`), Supplier Tier (`Text4`), ListDataID.

It joins PO detail Supplier to supplier ListDataID and outputs merged fields. The report resource stores both a `Settings` JSON string and a graph-like `Resource` JSON with `input`, `join`, `output`, and `SequenceFlow` nodes.

`FormNewReports[0]` is `PR`, tied to `DefKey: PR`, and includes system workflow fields plus PR variables. It references app list/report resource `1965675169026936833`.

No dashboard UI resource was deeply decoded beyond these report resources, but the app package contains `AppGroups` and `AppThemes`, implying app navigation/theme metadata is included.

## 9. Application-Level Dependency Model

| Identifier | Classification | Notes |
| --- | --- | --- |
| `AppID` | environment-specific / import-remapped | `41` appears throughout; unsafe to guess for another tenant |
| Main listset ID | environment-specific / import-remapped | `1906896262421815296`; used as `ListSetID` and main app ListID |
| Child `ListID`s | package resources / import-remapped | Included in package, but new IDs may be assigned on import |
| Field internal names | package-local within lists, must be validated | Examples: `PR_No_`, `Text1`; cannot infer safely from labels alone |
| Field IDs | package-local / import-remapped | Included in list `Defs`; may change |
| Form `Key` / `defkey` | stable semantic key | Examples `PR`, `PO`, `PA`; should match |
| Page IDs/control IDs/node IDs | package-local safe to generate if unique | Must be internally consistent |
| Position/user IDs | environment-specific | Finance Manager position ID is not portable |
| AI Agent ID | environment-specific or package module remapped | Agent is included in `OtherModules`, but workflow references must be remapped consistently |
| Document library IDs/template file IDs | environment-specific/package resource | Libraries included, actual templates/files need verification |
| SharePoint/token/credential IDs | environment-specific and sensitive | Token List contains credential fields; values should not be copied blindly |
| `ReplaceIds`/`ReportIds` | import remapping hints | Strong evidence app import has an ID replacement phase |

## 10. Comparison With Previous `.ywf` Study

| Topic | What `.ywf` showed | What `.yap` adds | Generator implication |
| --- | --- | --- | --- |
| Approval form structure | Single wrapper with base64 `Def` | Forms embedded as raw decoded JSON strings in `Forms[].DefResource` | Generator can reuse decoded Def model, but app package wrapper differs |
| App/list dependencies | Placeholders needed for ContentList IDs | Full list resources and field definitions can be included | Metadata can be extracted from app exports instead of hand-entered |
| ContentList mappings | Target app/list IDs and field internal names | Target lists and fields can be resolved to labels/types | Validator should check target field existence/type |
| Lookup lists | Controls had app/list/listfield references | Source lookup lists are packaged and field filters are visible | Generator can build lookup dependency maps automatically |
| Dashboards/reports | Not present in standalone forms | DataReport and FormNewReport resources included | Future generator may support app-level reports |
| Resource IDs | Many unknown environment dependencies | `ReplaceIds` indicates import remapping | Final generation must preserve a remapping manifest |
| AI resources | Form actions/nodes referenced agents | Agent definition appears in `OtherModules` | AI dependencies can be packaged or mapped, but IDs must be reconciled |
| Metadata replacement | Manual placeholder map | Full app export can supply candidate App/List/Field metadata | Add `.yap` metadata extractor and type validator |

## 11. Impact on the Travel Request Generator

Recommended improvements:

- Add app-export metadata extraction so AppID/ListSetID/ListID/field internal names can be selected from a `.yap` inventory rather than typed by hand.
- Add field type validation for ContentList mappings: variable type/file/user/lookup/number should match target list field type/control.
- Add app-level dependency validation: verify all lookup source lists, ContentList target lists, document libraries, and AI agent IDs exist in the supplied app metadata.
- Add optional detail-row persistence generation for TravelExpenseList, following PR/PO patterns: `TravelExpenseList.Amount -> child detail list row Amount`, plus parent Travel Request No. linkage.
- Treat a full app-level `.yap` generator as a later phase. It must handle list definitions, form definitions, report resources, agents/modules, and import remapping.
- For the current Travel `.ywf` path, keep metadata placeholder replacement but enrich the metadata template with target field type and source variable type.

## 12. Generator Roadmap Recommendation

| Tool | Purpose | Inputs | Outputs | When to use | Why it matters |
| --- | --- | --- | --- | --- | --- |
| `inspect-yap-package.js` | Decode `.yap` wrapper and summarize sections | `.yap` path | JSON/Markdown anatomy report | First pass on any app export | Avoids guessing container format |
| `extract-yap-metadata.js` | Extract app/list/field/form/report/agent metadata | `.yap` path | `*-metadata.json` | Before mapping generated forms to an app | Provides real IDs/internal names/types |
| `compare-yap-resources.js` | Compare two app exports | source `.yap`, target `.yap` | diff of lists/fields/forms/reports | Before/after sandbox import or version upgrade | Detects remapping and drift |
| `generate-dependency-map-from-yap.js` | Build dependency map candidates from app metadata | decoded Def draft + `.yap` metadata | dependency map with candidate matches | Before `apply-ywf-metadata.js` | Reduces manual placeholder filling |
| `validate-ywf-def-against-yap.js` | Validate a decoded Def against app metadata | decoded Def + extracted metadata | validation report | Before final `.ywf` wrapping | Checks real target list/field/type references |
| `compare-import-roundtrip.js` | Compare generated/imported/exported package | generated draft + post-import export | round-trip differences | After sandbox import/export | Proves importer interpretation |

## 13. Safety Rules

Can be learned from `.yap`:

- App package container format.
- List/report/form/resource inventory.
- Field internal names and field types from exported lists.
- Existing workflow-to-list persistence patterns.
- Lookup dependencies and filters.
- AI agent/module presence and workflow references.

Can be reused:

- Structural patterns for decoded form definitions.
- ContentList mapping shapes.
- Lookup/list/listref patterns.
- Report graph structure as examples.
- App metadata as sandbox mapping candidates when the target environment is the same export/import context.

Must not be copied blindly:

- AppID, ListSetID, ListID into a different environment.
- User, position, group, tenant, or workspace IDs.
- Token/client/secret fields or credential values.
- AI Agent IDs unless the agent is included and remapped correctly.
- Document template/file/library IDs without confirming files exist after import.

Must be remapped per environment:

- App/list/listset IDs.
- Field IDs and, when lists differ, internal field names.
- Approver positions/users/groups.
- External connection/token resources.
- AI agent/tool/resource IDs.
- Report IDs and dashboard/resource IDs.

Remain sandbox-only until proven:

- Full `.yap` generation.
- Generated document workflows.
- SharePoint/API/token workflows.
- AI-agent-backed approval suggestions.
- Detail-row persistence where parent-child linkage has not been round-trip tested.

---

<a id="validate-ydl-against-yap"></a>

<!-- source: docs/collection-02.md#validate-ydl-against-yap -->

# validate-ydl-against-yap.js

`validate-ydl-against-yap.js` validates a Yeeflow `.ydl` data-list export or decoded data-list draft against app metadata.

It is read-only. It does not import, modify, or generate `.ydl`, `.yap`, or `.ywf` packages.

## Usage

```bash
node validate-ydl-against-yap.js "./Communication Records.ydl" ./nhic-ydl-metadata.json --mode compatibility
```

Generator-strict mode:

```bash
node validate-ydl-against-yap.js "./generated-list.decoded.json" ./nhic-yap-metadata.json --mode generator
```

## Inputs

The first argument may be:

- a `.ydl` wrapper file
- a decoded Yeeflow data-list package, provided the local inspector can parse it in a future workflow

The second argument is app metadata. Supported shapes:

- metadata from `extract-yap-metadata.js`
- compatible metadata from `extract-ydl-metadata.js`

When using `.ydl`-only metadata, app-level coverage is limited to the supplied list exports. Full `.yap` metadata is preferred for final generator validation.

## Modes

`compatibility`

Use for real historical exports. External dependencies and older structures are reported as warnings/dependencies where possible.

`generator`

Use for generated drafts. Missing lookup targets, unresolved workflow target lists, missing source ListSetID, and other dependency gaps become hard failures.

## Checks

The validator checks:

- source AppID and ListID against metadata
- missing standalone ListSetID as a dependency
- lookup target list existence
- lookup target ListSetID consistency when available
- lookup display field existence
- lookup search field existence
- lookup sample shape vs `multiple`
- custom form bound field references
- custom form lookup/list control dependencies
- custom code controls as risks/dependencies
- view column/filter/sort field references
- list workflow ContentList target lists and field mappings
- QueryData target list references
- AI and HTTP/API workflow nodes
- workflow field expression references where parseable

## Output

The command prints:

```json
{
  "status": "pass | pass_with_warnings | fail",
  "mode": "compatibility | generator",
  "errors": [],
  "warnings": [],
  "dependencies": [],
  "resolvedReferences": {
    "lookupTargets": [],
    "workflowTargets": [],
    "formReferences": [],
    "viewReferences": [],
    "sampleReferences": []
  },
  "summary": {
    "lookupRelationshipsChecked": 0,
    "workflowTargetsChecked": 0,
    "customFormsChecked": 0,
    "viewsChecked": 0,
    "sampleReferencesChecked": 0
  }
}
```

## Limitations

- `.ydl`-derived metadata cannot prove all app-level resources exist unless the related lists were supplied.
- Record-level lookup sample validation is limited unless target list sample records are available in the metadata.
- Workflow expression parsing is best-effort.
- AI agent and HTTP/API dependencies are reported, not validated against a live environment.

## Recommended Use

1. Extract full app metadata from `.yap` when available.
2. Validate generated `.ydl` drafts in `generator` mode.
3. Stop if lookup targets, workflow target lists, field mappings, custom code risks, or placeholders remain unresolved.
4. Use `compatibility` mode only for studying real exports or import/export round-trip differences.

---

<a id="validate-ydl-list"></a>

<!-- source: docs/collection-02.md#validate-ydl-list -->

# validate-ydl-list.js

`validate-ydl-list.js` is a read-only validator for decoded Yeeflow data-list packages and standalone `.ydl` exports.

It is intended to run before any future `.ydl` wrapper generation or import testing.

## Usage

Validate a real exported `.ydl` in tolerant compatibility mode:

```bash
node validate-ydl-list.js "./Portfolio Management.ydl" --mode compatibility
```

Validate a generated decoded `Resource.Data` JSON draft:

```bash
node validate-ydl-list.js "./generated-list.decoded.json" --mode generator --stage draft
```

Validate a generated decoded `Resource.Data` JSON final candidate:

```bash
node validate-ydl-list.js "./generated-list.decoded.json" --mode generator --stage final
```

## Modes

`compatibility`

Use for studying real historical Yeeflow exports. External lookup targets, workflow target lists, sparse metadata, and old layout artifacts are reported as warnings/dependencies where possible.

`generator`

Use for generated drafts. The validator is stricter: unresolved lookup targets, unresolved workflow target lists, and unresolved placeholders fail validation unless future tooling declares and resolves those dependencies.

## Stages

`draft`

Use for generated drafts that may intentionally contain metadata placeholders. In `generator + draft`, unresolved placeholders are reported in `placeholders[]` and as warnings, while real structural issues remain errors.

`final`

Use for final candidates before wrapper build or sandbox import. In `generator + final`, unresolved placeholders are hard errors.

The default stage is `final`.

## Input Types

The validator supports:

- a `.ydl` wrapper file with a top-level `Resource` field using `[______gizp______]` + base64 + gzip.
- a decoded Resource JSON object with a `Data` JSON string.
- a decoded `Resource.Data` JSON object containing `Item`.

Large numeric IDs are preserved as strings during JSON parsing.

## Checks

The validator checks:

- wrapper JSON validity and Resource gzip/base64 decoding for `.ydl` inputs
- decoded Resource and `Resource.Data` JSON validity
- `Data.Item`, `Item.ListModel`, `Item.Defs`, `Item.Layouts`, and `Item.ListDatas`
- list identity fields such as title, AppID, ListID, ListSetID, and MainListType
- field IDs, names, internal names, display labels, field/control types, duplicate names, and JSON Rules
- normalized field type detection
- choice metadata for radio/dropdown/checkbox fields
- lookup metadata such as appid, listsetid, listid, and display/list field
- view `LayoutView` parsing, displayed columns, filters, sorts, default view
- custom form Resource parsing and recursive control binding checks
- list workflow DefResource parsing, ContentList nodes, QueryData nodes, AI nodes, and field/list references
- sample data record shape, field names, lookup values, multi-choice values, and date/datetime values
- placeholder strings matching `^__.*REQUIRED.*__$`

## Output

The command prints JSON:

```json
{
  "status": "pass | pass_with_warnings | fail",
  "mode": "compatibility | generator",
  "stage": "draft | final",
  "errors": [],
  "warnings": [],
  "placeholders": [],
  "dependencies": [],
  "summary": {
    "fields": 0,
    "views": 0,
    "customForms": 0,
    "workflows": 0,
    "sampleRecords": 0,
    "lookupRelationships": 0
  }
}
```

## Current Limitations

- It validates known `.ydl` structures learned from the NHIC exports; uncommon Yeeflow list features may need new rules.
- It does not yet accept a separate dependency mapping file.
- It does not prove that external lookup targets exist in a live Yeeflow environment.
- It reports AI and HTTP/API workflow nodes, but does not validate runtime credentials or agent availability.
- It does not build, import, or modify any package.

## Recommended Workflow

1. Inspect exports with `inspect-ydl-package.js`.
2. Extract reusable metadata with `extract-ydl-metadata.js`.
3. Validate real exports in `compatibility` mode.
4. Validate generated decoded list drafts with `--mode generator --stage draft`.
5. Validate final candidates with `--mode generator --stage final`.
6. Stop before wrapper build if final validation fails, placeholders remain, or dependencies are unresolved.

---

<a id="validate-ywf-def-against-yap"></a>

<!-- source: docs/collection-02.md#validate-ywf-def-against-yap -->

# Yeeflow Def App-Context Validator

Validate a decoded Yeeflow approval form `Def` JSON draft against extracted `.yap` metadata.

This validator is read-only. It does not import into Yeeflow, operate the UI, modify `.ywf` / `.yap` / metadata files, base64 encode, or create packages.

## Usage

```bash
node validate-ywf-def-against-yap.js \
  ./travel-request-def.sandbox.json \
  ./procurement-metadata.json \
  --mode final \
  --profile generator
```

Use `draft` mode while placeholders are expected:

```bash
node validate-ywf-def-against-yap.js ./draft-def.json ./app-metadata.json --mode draft
```

## Profiles

`--profile generator` is the default and is strict. Use it for generated packages.

```bash
node validate-ywf-def-against-yap.js ./def.json ./metadata.json --mode final --profile generator
```

In generator profile:

- `SOURCE_ROW_FIELD_NOT_FOUND` remains an error.
- missing workflow variables remain errors.
- missing target fields remain errors.
- final mode fails if placeholders remain.

`--profile compat` is for studying real exports that may contain historical or stale mappings.

```bash
node validate-ywf-def-against-yap.js ./def.json ./metadata.json --mode final --profile compat
```

In compat profile, `SOURCE_ROW_FIELD_NOT_FOUND` is downgraded to `SOURCE_ROW_FIELD_NOT_FOUND_COMPAT` only when the target field resolves and the source list variable exists, so the only missing piece is the row field itself. The warning includes suggested aliases, but the validator never rewrites mappings automatically.

## What It Checks

ContentList targets:

- `appid` matches the metadata app ID when present.
- `listsetid` matches the main ListSetID or a known list ListSetID.
- `listid` exists in `metadata.lists[]`.
- target list names are resolved where possible.

ContentList field mappings:

- each `listdatas[].Columns` value resolves to a target list field.
- source expressions are classified as literal, workflow variable, list row variable, expression array, list-trigger field, function, or unknown.
- workflow variable references exist in `variables.basic`.
- list row variable references exist in the bound `variables.listref`.
- likely source/target type mismatches are reported as warnings.

Where conditions:

- edit/remove `wheres[].left` fields exist in the target list, except `ListDataID`.
- where-right expressions reference existing variables/list fields when parseable.

Lookup sources:

- lookup source list IDs exist in metadata.
- lookup display fields exist in the source list.

Document dependencies:

- `GenerateDocument` nodes are detected.
- document IDs are matched against known document libraries/dependencies when possible.
- template files are warned as not fully verifiable unless metadata exposes them clearly.

AI references:

- AI nodes and AI form actions are detected.
- Agent IDs are checked against `metadata.aiReferences` when available.

Placeholders:

- draft mode reports unresolved `__...REQUIRED...__` placeholders as warnings.
- final mode fails if placeholders remain.

## Output

The script prints a JSON report:

```json
{
  "status": "pass | pass_with_warnings | fail",
  "mode": "draft",
  "profile": "generator",
  "errors": [],
  "warnings": [],
  "resolvedReferences": {
    "contentListTargets": [],
    "fieldMappings": [],
    "lookupSources": [],
    "documentDependencies": [],
    "aiReferences": []
  },
  "summary": {
    "contentListNodesChecked": 0,
    "fieldMappingsChecked": 0,
    "lookupSourcesChecked": 0,
    "documentDependenciesChecked": 0,
    "aiReferencesChecked": 0,
    "typeWarnings": 0,
    "compatWarnings": 0
  }
}
```

## Recommended Pipeline

```bash
node extract-yap-metadata.js "./Procurement Management.yap" --out ./procurement-metadata.json --md ./procurement-metadata.md
node validate-ywf-def.js ./decoded-def.json --mode final
node validate-ywf-def-against-yap.js ./decoded-def.json ./procurement-metadata.json --mode final --profile generator
```

This app-context validator complements the structural validator. A Def can be structurally valid but still reference missing app resources or incompatible list fields.

Recommendation: use `generator` for generated packages and release gates. Use `compat` for studying real exports and import-history artifacts where you want a report rather than a hard stop on known legacy inconsistencies.

---

<a id="validate-ywf-def"></a>

<!-- source: docs/collection-02.md#validate-ywf-def -->

# Decoded Yeeflow Def Validator

Validate decoded Yeeflow approval form `Def` JSON drafts before base64 wrapping or `.ywf` wrapper generation.

This tool is intentionally read-only. It does not import into Yeeflow, operate the UI, create final `.ywf` files, or base64 encode drafts.

It also loads `workflow-action-configurations.normalized.json` through `workflow-action-config-validator.js` to validate workflow action properties against the normalized node/action reference.

## Usage

```bash
node validate-ywf-def.js ./travel-request-def.json --mode draft
node validate-ywf-def.js ./travel-request-def.json --mode final
```

With a dependency mapping file:

```bash
node validate-ywf-def.js ./travel-request-def.json --mode draft --dependency-map ./travel-request-dependencies.json
```

## Modes

- `draft`: unresolved `__...REQUIRED...__` placeholders are allowed and reported.
- `final`: unresolved placeholders fail validation.

## Output

The script prints a structured JSON report:

```json
{
  "status": "pass | pass_with_warnings | fail",
  "mode": "draft",
  "errors": [],
  "warnings": [],
  "placeholders": [],
  "summary": {
    "variables": 0,
    "listrefs": 0,
    "pages": 0,
    "controls": 0,
    "workflowNodes": 0,
    "sequenceFlows": 0,
    "approvalTasks": 0,
    "contentListNodes": 0
  }
}
```

## Scope

The validator checks decoded `Def` structure, variables and listrefs, page/control bindings, approval page workflow controls, task URL page references, workflow graph connectivity, workflow designer layout readiness, approval/reject paths, sequence-flow condition shape, ContentList mappings, SetVariableTask variable references, normalized workflow action required properties, enum values, value types, `QueryData` filters, `Loop`/`Delay` condition shapes, unsafe external/credential actions, and placeholder policy.

Publish-ready page checks include:

- Every `pageurls[]` entry has page-level `pagetype`.
- Request page entries include `name`, matching real exports where this is often an empty string.
- `formdef.pagetype` matches the page `type`.
- `formdef.ver` is `2`, matching the inspected current approval exports.
- `formdef.exts`, when present, is an array.
- Request pages include `workflowControlPanel` for Save as draft / Submit.
- Request pages should include `workflowHistory`; missing history is currently a warning.
- Readonly request Applicant controls use `value: "CurrentUser"` and `attrs.default: "currentUser"`.
- Readonly request Submission Date controls use `attrs.default: "currentDate"` and `attrs.date_type: "0"`.
- Datepicker controls with static timestamp values but no current-date default are warned.

Workflow designer layout readiness includes:

- `graphposition` has numeric `x`, `y`, `width`, and `height`.
- Every non-`SequenceFlow` workflow node has `id === resourceid`.
- Every non-`SequenceFlow` workflow node has top-level `position: { "x": number, "y": number }`.
- Every node `incoming`/`outgoing` SequenceFlow reference includes matching `id` and `resourceid`.
- Every `SequenceFlow` has `id === resourceid`.
- Every `SequenceFlow` has explicit `source` and `target` references to existing non-SequenceFlow nodes.
- Every `SequenceFlow.source`, `target`, `incoming[0]`, and `outgoing[0]` uses canonical matching `id` + lowercase `resourceid`; camel-case `resourceId` cannot replace either field.
- Every `SequenceFlow.incoming[0]` matches its source node and every `SequenceFlow.outgoing[0]` matches its target node.
- Every `SequenceFlow.source` matches the source inferred from node `outgoing`.
- Every `SequenceFlow.target` node includes the flow in its `incoming`.
- Workflow Condition editor variable rows with `left.type = 1` and `left.value.exprType = "variable"` resolve the selected workflow variable, use the correct Condition editor `group`, use the matching operator family (`s.`, `n.`, `b.`, `dt.`, or general null checks), and use either `right.type = 0` for compatible direct literal values or `right.type = 2` for non-empty Expression editor token arrays.
- Workflow Condition editor group rows support at most two layers: top-level `conditioninfo[]` rows plus one child `conditions[]` layer. Group wrapper rows must use `left: null`, `op: "isNull"`, `right: null`, and a non-empty child array. Empty groups, third-level groups, and group wrappers with real left/right operands are invalid.
- Assignment task `Approved`, `Rejected`, and `Completed` outgoing conditions must use export-style task Outcome expression-button HTML tied to the source task id. Generic/simple `Outcome` workflow-variable tokens fail with `APPROVAL_CONDITION_LEFT_TASK_REF_INVALID`.

It cannot prove that unresolved Yeeflow environment resources are correct. App IDs, ListSetIDs, ListIDs, field internal names, approver IDs, document templates, Agent IDs, and live import behavior must still be verified separately before final `.ywf` generation.
