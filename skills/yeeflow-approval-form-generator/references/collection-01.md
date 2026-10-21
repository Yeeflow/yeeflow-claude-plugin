# skills/yeeflow-approval-form-generator/references collection 1

This file combines 6 documents from `skills/yeeflow-approval-form-generator/references/` for the Claude directory build. A reference to `skills/yeeflow-approval-form-generator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [custom-code-decision-guide.md](#custom-code-decision-guide)
- [examples-summary.md](#examples-summary)
- [metadata-and-wrapper-guide.md](#metadata-and-wrapper-guide)
- [operating-playbook.md](#operating-playbook)
- [requirement-decomposition-template.md](#requirement-decomposition-template)
- [validation-guide.md](#validation-guide)

---

<a id="custom-code-decision-guide"></a>

<!-- source: skills/yeeflow-approval-form-generator/references/collection-01.md#custom-code-decision-guide -->

# Custom Code Control Decision Guide

This guide defines when Codex should use Yeeflow custom code controls in generated approval forms, and how to do it safely. The goal is to produce powerful, user-friendly forms without adding custom code where native Yeeflow configuration is enough.

## 1. Purpose

Custom code controls are an advanced enhancement layer for Yeeflow approval forms.

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

Only requirements classified as custom client-side behavior should be candidates for custom code.

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

<a id="examples-summary"></a>

<!-- source: skills/yeeflow-approval-form-generator/references/collection-01.md#examples-summary -->

# Examples Summary

This file summarizes proven examples without bundling bulky generated `.ywf` files.

## Simple PR Note Approval v6

Source reference in project:

- `simple-pr-note-approval-def.v6.json`

Proves:

- one-step approval
- structured request page
- structured approval page
- current user/current date defaults
- request action panel and workflow history
- approval workflowControlPanel and workflowHistory
- page registration/publish metadata
- workflow graph layout metadata
- ContentList parent persistence

## Simple PR Amount Approval

Source reference in project:

- `simple-pr-amount-approval-def.json`

Proves:

- two-step approval
- Applicant Line Manager pattern
- static position approver pattern
- ContentList persistence to PR Records
- final validation and wrapper build against Procurement metadata

## Simple PR Line Item Approval

Source references in project:

- `simple-pr-line-item-approval-def.v4-alpha-labels.json`
- `docs/collection-02.md#generated-line-item-custom-code-row-label-pattern`

Proves:

- line-item list variable/listref/list control
- row subtotal calculation
- table total summary binding
- parent PR Records persistence
- child PR Details List persistence
- optional custom code control for row labels
- alphabetic row labels require text row field and readonly row control

## Employee Equipment Request

Source references in project:

- `employee-equipment-request-decomposition.md`
- `employee-equipment-request-def.draft.json`
- `employee-equipment-request-def.test-pr-mapped.json`
- `employee-equipment-request-test-mapping.md`

Proves:

- business requirement decomposition before generation
- draft mode with unresolved metadata placeholders
- explicit stop before wrapper build when metadata is missing
- sandbox-only mapping to existing PR Records / PR Details List when explicitly requested
- final validation and wrapper build after test mapping resolves placeholders

Do not treat sandbox mappings as production data models.

## Visitor Access Management v11 Approval Form

Source project references:

- `visitor-access-management-v11-approval-form-def.json`
- `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`
- `docs/collection-02.md#generated-yap-baseline-visitor-access-management-v5-fresh-compatible`

Proves generated app-level approval form support for:

- number variable + `input_number` control
- text variable + `radio` control
- dropdown style via `radio` + `attrs.displayStyle = "dropdown"`
- boolean variable + `switch` control
- conditional display on target control `attrs.control_display[]`
- readonly approval-page mirrors for persisted business fields
- `ContentList` mappings:
  - `VisitorEmail -> Text13`
  - `VisitorPhone -> Text14`
  - `NumberofVisitors -> Decimal1`
  - `AccessType -> Text15`
  - `RequiresEscort -> Bit1`

Known gaps:

- persisted conditional field
- multi-select choice
- user picker
- lookup inside a line-item table

---

<a id="metadata-and-wrapper-guide"></a>

<!-- source: skills/yeeflow-approval-form-generator/references/collection-01.md#metadata-and-wrapper-guide -->

# Metadata And Wrapper Guide

## Metadata Mapping

Generated drafts may contain placeholders for environment-specific values:

```text
__APP_ID_REQUIRED_*__
__LISTSET_ID_REQUIRED_*__
__LIST_ID_REQUIRED_*__
__FIELD_INTERNAL_NAME_REQUIRED_*__
__POSITION_ID_REQUIRED_*__
__DOCUMENT_TEMPLATE_ID_REQUIRED_*__
__AGENT_ID_REQUIRED_*__
```

Placeholders are allowed only in draft decoded Def JSON. They are forbidden in final `.ywf` wrappers.

Use a dependency mapping file to track each placeholder:

- dependency key
- dependency type
- required value
- where used in Def
- whether required for final export
- notes

## Applying Metadata

Use:

```bash
node <plugin-root>/scripts/apply-ywf-metadata.js \
  ./source-def.json \
  ./filled-metadata.json \
  ./source-def.sandbox.json
```

The script verifies:

- every metadata entry is ready
- required values are not empty
- semantic locators match expected current values
- every placeholder in the Def is covered
- no unresolved placeholders remain in output

## Wrapper Build

Only after final validation passes:

```bash
node scripts/build-ywf-wrapper.js \
  ./validated-def.json \
  ./output.ywf \
  --flow-name "Form Name" \
  --flow-key KEY \
  --workflow-type 2 \
  --description "Description"
```

The builder checks:

- decoded Def JSON validity
- no unresolved placeholders
- FlowKey equals decoded `defkey`
- WorkflowType equals decoded `workflowType`
- base64 `Def` encoding
- wrapper JSON validity
- round-trip decode equals source Def

Never use the wrapper builder to bypass validation.

---

<a id="operating-playbook"></a>

<!-- source: skills/yeeflow-approval-form-generator/references/collection-01.md#operating-playbook -->

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

<a id="requirement-decomposition-template"></a>

<!-- source: skills/yeeflow-approval-form-generator/references/collection-01.md#requirement-decomposition-template -->

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

| Lookup field | Source app/listset/list | Display field | Value field | Filters | Dependency status |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

Questions:

- Does the lookup source list already exist?
- Which internal field stores the display value?
- Are dependent filters required?
- Are source IDs available from app metadata?

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

<a id="validation-guide"></a>

<!-- source: skills/yeeflow-approval-form-generator/references/collection-01.md#validation-guide -->

# Validation Guide

Use these validators before any wrapper build.

## Structural Def Validation

Run:

```bash
node <plugin-root>/skills/yeeflow-application-generator/scripts/validate-ywf-def.js ./form-def.json --mode draft
node <plugin-root>/skills/yeeflow-application-generator/scripts/validate-ywf-def.js ./form-def.json --mode final
```

Draft mode may contain placeholders. Final mode must not.

Checks include:

- required decoded Def sections
- variable/listref uniqueness and consistency
- page/control binding validity
- list control metadata
- request and approval pages
- taskurl references
- workflow node connectivity
- SequenceFlow targets and source inference
- approved/rejected approval paths
- ContentList structure
- SetVariableTask variable references
- unresolved placeholders

## App-Context Validation

First extract metadata from a `.yap` export:

```bash
node <plugin-root>/scripts/extract-yap-metadata.js "./Application.yap" --out ./app-metadata.json --md ./app-metadata.md
```

Then validate:

```bash
node <plugin-root>/scripts/validate-ywf-def-against-yap.js ./form-def.json ./app-metadata.json --mode final --profile generator
```

Use `--profile generator` for generated packages. Use `--profile compat` only when studying real exports that contain known compatibility quirks.

Checks include:

- ContentList target app/listset/list existence
- target field internal names
- source workflow variables
- source list-row variables
- source/target type compatibility
- lookup sources
- lookup display/sort fields
- lookup additional-field source fields
- lookup additional-field target variables
- document dependencies
- AI references
- placeholders in draft/final modes

When an approval form targets a newly generated data list, validate against metadata extracted from the imported/exported-back `.ydl`. Do not validate or build the final `.ywf` against pre-import generated list IDs.

## Required Validation Gate

A decoded Def is wrapper-ready only when:

- structural validation passes in final mode
- app-context validation passes in final/generator mode when target app metadata is involved
- `ContentList` targets and mappings resolve against exported-back generated-list metadata when the storage list was generated
- no placeholders remain
- FlowKey matches `defkey`
- WorkflowType matches `workflowType`

Do not build `.ywf` if any of these fail.
