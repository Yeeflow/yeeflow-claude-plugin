# skills/yeeflow-custom-code-generator/references collection 1

This file combines 8 documents from `skills/yeeflow-custom-code-generator/references/` for the Claude directory build. A reference to `skills/yeeflow-custom-code-generator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [direct-form-action-execution.md](#direct-form-action-execution)
- [output-modes.md](#output-modes)
- [yeeflow-code-to-guide-rules.md](#yeeflow-code-to-guide-rules)
- [yeeflow-custom-code-standard.md](#yeeflow-custom-code-standard)
- [yeeflow-data-and-dashboard-patterns.md](#yeeflow-data-and-dashboard-patterns)
- [yeeflow-parameter-and-save-target-rules.md](#yeeflow-parameter-and-save-target-rules)
- [yeeflow-product-ui-checklist.md](#yeeflow-product-ui-checklist)
- [yeeflow-uiux-guidelines.md](#yeeflow-uiux-guidelines)

---

<a id="direct-form-action-execution"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#direct-form-action-execution -->

# Direct Form Action execution from Custom Code

## Contract and evidence

Product-team contract supplied on 2026-10-06 for `codeInContext`:

```ts
/** Execute an action by id, or by its unique name. String references prefer id. */
executeAction: (action: string | { id?: string; name?: string }) => Promise<void>;
```

This is a host API, not an MCP tool or a plugin-installed runtime implementation. Generate direct calls only for a host exposing this API. The runtime argument is called `context` in the existing `render(context, fieldsValues, readonly)` examples; bind the actual CodeInContext instance there. Do not invent a global `codeInContext` or a `context.codeInContext` property. Verify the host context has `executeAction` before invoking it. This rule does not change the `render` versus action-step `execute` entrypoints.

## Reference selection

Prefer `await context.executeAction({ id: actionId })` for generated applications. Allocate or read the real action ID, define the action in the owning form/page, and keep the reference updated during cloning/remapping. `{ name: actionName }` requires a unique name in the target host scope. String references resolve by ID first, then unique name. For an intentional name call, use the explicit name object to avoid collisions with IDs. Generate exactly one nonempty id or name; behavior for both/neither is unspecified. Do not fabricate IDs, assume cross-form lookup, or assume action names remain stable after rename.

```ts
// Inside a user event handler with the actual host context in scope:
if (typeof context.executeAction !== 'function') {
  throw new Error('This page requires direct Form Action support.');
}
await context.executeAction({ id: configuredActionId });
// Or, when the unique name is intentional:
await context.executeAction({ name: 'Open selected record' });
```

## Three supported generation patterns

1. Button: use an asynchronous handler, disable repeated clicks while pending, catch and present failures, and release the busy lock in `finally`. Preserve readonly and explicit confirmation requirements for mutations. Never call actions from render.
2. Value change: compare the meaningful previous and next values; do not fire on initial render or on unrelated changes. Track action-induced updates to prevent cycles; debounce when appropriate and serialize operations. Do not use a general rerender or an unguarded effect as the trigger. User-triggered saves/deletes must not silently become automatic writes.
3. Custom collection row: capture the clicked row identity, write it into a declared writable host variable, then invoke the action that reads that variable. Use a single shared busy lock/queue across rows when they share the same variable; per-row locks are insufficient. Do not assume native Collection current-row context is established by clicking a custom React row.

The API takes one action reference only. It has no record payload, variables argument, cancellation token, or business return value. Do not emit `executeAction(action, { recordId })`, infer outputs from Promise<void>, or invent an action parameter schema. Use declared host fields/temp variables for inputs and outputs, with existing surface-specific writable-target rules. `await` on a setter returning void does not prove the action sees the new value: use verified host propagation/readback before execution. If the host offers no proven ordering mechanism, mark row-context integration unverified rather than adding a guessed sleep.

Use `examples/form-action-dispatch.mjs` as a portable orchestration helper; it receives an adapter named `prepareContext` supplied by the generated component. That adapter is not a Yeeflow API. Its contract is to complete only after the target host can read the intended variables. It must reject on failed write/readback. The helper rejects overlapping calls instead of overwriting shared row context. Catch its rejection in the UI. Keep busy state and error text visible as appropriate. Use existing React 15.6 / Ant Design 2.13 conventions.

## Planning, generation, and validation

Record owning surface, action ID or unique name, action definition, event trigger, readonly behavior, variable mapping, confirmation requirements, concurrency policy, value-change loop guard, and refresh/readback needs in the app plan. The Dashboard Communication and Native Integration section must include these mappings. Generate the native action and declared variables together with the Custom Code; an action call alone does not create its target action. Revalidate IDs after any clone/remap. The helper's shape checks are not action-definition closure validation: compare every generated action reference with the actual owning form/page actions.

On hosts with this API, direct execution is preferred over hidden buttons and CSS-ID DOM clicks. Preserve old button-based behavior only for an existing, verified legacy host or an explicit compatibility requirement; never silently fall back to a DOM click after direct execution fails, because a partial action may already have executed. Native buttons remain appropriate visible UI when requested. Do not remove a button until its old callers and accessibility behavior have been accounted for.

For Form Action Custom Code steps, do not generate self-invocation or indirect action cycles. Support for nested execution and its completion semantics must be verified before emitting nested actions. A control's readonly value and local checks improve UX; host permissions remain authoritative.

## Required runtime acceptance (not yet proven by unit tests)

Test Dashboard, data-list custom form, and approval form separately before claiming each supported. Include ID, unique-name and string-ID-precedence calls; missing action and duplicate-name errors; old host API absence; rejected action; double-clicks across two rows; variable visibility; initial render; action-induced value changes; readonly/permission denial; selected-record editor opening; and result refresh/readback. Establish whether Promise completion means dispatch, action-chain completion, or dialog closure. A resolved Promise does not by itself prove a saved record, completed workflow, or rendered editor. Product contract plus mocked tests is not tenant runtime proof.

---

<a id="output-modes"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#output-modes -->

# Output modes

## Code-only mode
Return a full Yeeflow custom code file.
Use this for:
- new code
- updated code
- fixed code
- redesigned code

Use code-only when the user explicitly asks for only code, or when a non-reusable code-generation request does not ask for documentation.

## Reusable template mode
When the user asks for a reusable Yeeflow custom code template or template-library artifact, create project files instead of only returning pasted code:
- `templates/<template-folder>/<template-code-file>.tsx`
- `templates/<template-folder>/<template-name>-user-guide.md`
- optional `templates/<template-folder>/<template-name>-example-config.md`

Keep all deliverables for one template in the same folder. The guide should explain business purpose, placement, parameters, setup, expected output, examples, assumptions, checklist, and troubleshooting.

## Guide-only mode
Use this by default when the user provides an existing Yeeflow custom code file and asks for documentation, a user guide, setup instructions, or an explanation of what the custom code does.

In guide-only mode:
- inspect the existing code first
- generate the user guide without changing code
- clearly distinguish observed code behavior from inferred usage and assumptions
- save the guide next to the code when working directly in project files and the user asks for a file

## Diff mode
Use only when the user explicitly asks for:
- exact changes only
- changed sections only
- patch
- diff

## Debug mode
For debugging requests:
1. state the likely cause briefly
2. return corrected code immediately after
3. include targeted logs only if they materially help verification

---

<a id="yeeflow-code-to-guide-rules"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#yeeflow-code-to-guide-rules -->

# Yeeflow code-to-guide rules

Use this reference when the user provides an existing Yeeflow custom code file and asks for documentation, setup guidance, a user guide, or an explanation of how to use it.

## Trigger phrases

Use code-to-guide mode for requests like:
- generate a user guide from this code
- document this existing custom code
- explain how to configure this custom code
- what does this Yeeflow custom code do?
- create documentation for this legacy/custom/customer-delivered template
- add a guide for an existing template

Default output is guide-only unless the user also asks for code changes.

## What to inspect

Inspect these code areas before writing the guide:
- `description()` for template/control name and intended purpose
- `inputParameters()` for configurable parameters, parameter ids, types, and descriptions
- `requiredFields(params)` for current-form dependencies and writable target capture
- `render(context, fieldsValues, readonly)` for placement, props, readonly behavior, and component structure
- reads from `context.params`, `fieldsValues`, and `context.getFieldValue`
- writes through `context.setFieldValue`, variable/temp-variable setters, object setters, or `fieldsValues` fallback
- data queries through `context.modules.yeeSDKClient`, especially `lists.queryItems(...)`
- helper methods for normalization, parsing, formatting, dedupe, validation, and target resolution
- rendered UI, labels, empty/loading/error states, keyboard or mouse interactions, and responsive behavior

## What to extract

For each guide, extract:
- template/control name
- short business-friendly description
- purpose and business problem solved
- supported placement if observable or inferable
- input parameters with type, required/optional status, purpose, and examples
- required fields and why they are required
- save/output behavior, including exact output formats where visible
- data source/query behavior
- UI behavior and interactions
- assumptions and limitations
- testing checklist
- troubleshooting notes

## Evidence discipline

Do not invent unsupported features. Separate:
- **Observed in code**: directly visible in the code.
- **Inferred likely usage**: likely based on naming, UI, or behavior.
- **Assumptions / needs confirmation**: unclear tenant-specific behavior, missing context, or behavior that depends on Yeeflow runtime APIs.

If placement is not clear, say `Not explicit in code` and then list likely placement based on code behavior.

If a parameter description is vague or missing, infer from usage but mark it as inferred.

If save behavior is display-only, say clearly that the code does not appear to save values.

## Guide structure

Use this guide structure unless the user requests a different format:

1. Template/control name
2. Short description
3. Purpose / what this custom code is for
4. Supported placement
5. When to use
6. Input parameter table
7. Detailed explanation of each parameter
8. Step-by-step configuration guide
9. Result / expected output
10. Real business examples
11. Notes / assumptions / limitations
12. Testing checklist
13. Troubleshooting

## Parameter table guidance

Include columns:
- parameter name
- type
- required or optional
- purpose
- example value

Classify parameters using the same parameter categories used for code generation:
- plain text
- expression editor
- writable target
- numeric config
- boolean behavior

For expression-editor parameters, explain that values may come from variables, temp variables, form fields, complex variables, or expressions when the code supports that.

For writable targets, explain target resolution and save behavior only if it is visible in the code. Do not claim dashboard temp-variable support unless the code implements or clearly intends it.

## Saving guides in reusable template projects

When working in a project folder and the user asks to create the guide file, save it next to the code:

```text
templates/<template-folder>/<template-name>-user-guide.md
```

If the code file is outside `templates/`, choose a nearby Markdown filename such as:

```text
<custom-code-file-name-without-extension>-user-guide.md
```

Do not modify the code unless the user explicitly asks for code changes.

---

<a id="yeeflow-custom-code-standard"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#yeeflow-custom-code-standard -->

# Yeeflow custom code standard

## Surface selection

Yeeflow has two custom-code surfaces with different entrypoints.

| Surface | Host | Required entrypoint | Purpose |
| --- | --- | --- | --- |
| Custom Code control | Page/form control tree, `type: "codein"` | `render(context, fieldsValues, readonly)` | Render UI or page/form interaction helpers |
| Form Action Custom Code step | `formdef.actions[].steps[]` | `execute(context, fieldsValues)` | Run action-side business logic |

Classify the target surface before generating code. Do not apply Custom Code control rules to Form Action Custom Code steps.

## Custom Code control entry structure

Use this structure by default for a visible Custom Code control:

```ts
export class CodeInApplication implements CodeInComp {
  description() { ... }
  requiredFields(params?: CodeInParams) { ... }
  inputParameters(): InputParameter[] { ... }
  render(context: CodeInContext, fieldsValues: any, readonly: boolean) {
    return <Component ... />;
  }
}
```

## Form Action Custom Code step entry structure

Use this structure for a script that runs inside a Form Action Custom Code step:

```ts
export default class CodeInApplication implements CodeInComp {
  description() { ... }
  inputParameters(): InputParameter[] { ... }
  execute(context: CodeInContext, fieldsValues: any): Promise<void> | void {
    ...
  }
}
```

Rules:

- `execute(...)` is mandatory for `form_action_step`.
- `render(...)` is not required for `form_action_step`.
- A script with only `render(...)` is invalid for a Form Action Custom Code step.
- A script with only `execute(...)` is invalid for a visible Custom Code control.
- Use helpers called by `execute(...)` for larger action-side workflows.
- Keep package materialization claims gated until the Form Action Custom Code step JSON storage shape is export-proven.

## Runtime conventions
- Target the Yeeflow custom-code runtime: React 15.6, AntDesign 2.13, and TypeScript.
- Read parameters from `context.params`.
- Pass parameter values into the React component as props.
- Read current form values from `fieldsValues` and `context.getFieldValue(...)`.
- Use `context.modules.yeeSDKClient` for list data operations.
- Use `context.modules.fetch`, `context.modules.loadScript`, and `context.modules.loadCSS` only through the runtime-provided `context.modules` object.
- When SDK method names, parameter shapes, or return values are unknown, use `getYeeSDKAPIDetails` where the authoring environment exposes it. If the SDK shape still cannot be proven, block generation or mark the script as requiring SDK confirmation. Do not guess tenant data mutation APIs.
- Inline `<style>` blocks are acceptable for self-contained outputs, but keep CSS safely inside quoted strings. Prefer `['.class{...}', '.child{...}'].join('')` over raw multiline CSS template literals when targeting Yeeflow's pasted-code resolver.
- Favor `React.Component` class components for reusable controls. Avoid `React.createRef()` and use callback refs for older Yeeflow React runtimes.
- Avoid hooks, `forwardRef`, `memo`, and modern React-only APIs unless the user's working Yeeflow example already proves support.
- Make sure the final output starts with the import/export code and includes `export class CodeInApplication implements CodeInComp`; otherwise Yeeflow may report `No exports found`.

## Input parameter conventions
Observed parameter types include:
- `string`
- `number`
- `variable`

Define only the parameters actually needed by the component. Before choosing a type, classify parameters by runtime use:
- plain text parameters: use `string`
- expression-editor parameters: usually use `variable`
- writable save/output targets: usually use `variable` and resolve as write targets
- numeric config parameters: use `number` when numeric metadata is supported; otherwise use `string` plus safe parsing
- boolean behavior parameters: often use `variable` plus boolean normalization when dynamic behavior is useful

Use practical ids like:
- `yearFieldName`
- `monthFieldName`
- `dataListId`
- `fieldofDept`

Expression-editor parameters may return primitives, arrays, `{ value }`, `{ label }`, `{ key }`, field/variable binding objects, temp-variable binding objects, or expression wrappers. Include reusable normalization helpers when using them.

Writable target parameters such as `saveToField`, `outputField`, or `resultTarget` are not ordinary values. Resolve the writable target key/name before calling a setter, and do not assume a dropdown-selected variable behaves like a manually typed string.

## requiredFields guidance
Return only current-form field names that the component truly depends on.
Examples:
- year field
- month field
- department field

If no current-form field dependency exists, return `[]`.

For reusable templates with writable target parameters, `requiredFields(params)` can also help capture configured field/temp-variable target names before expression parameters evaluate to current runtime values. Keep this capture focused and defensive.

## Safe access pattern
Use helper methods to safely read field values.
Typical order:
1. `fieldsValues[fieldName]`
2. `context.getFieldValue(fieldName)`
3. safe fallback

For save/write behavior, check the target placement. Approval forms, data list forms, dashboard variables, and dashboard temp variables may expose different setter methods. Use target resolution and defensive setter selection when the template supports multiple placements.

## Output expectation
Default to one complete Yeeflow custom code file.
If the user explicitly asks for changed sections only, return only the changed sections.

When the user provides existing Yeeflow custom code and asks for documentation, default to guide-only output. Inspect `description()`, `inputParameters()`, `requiredFields()`, `render(...)`, data access, save/output behavior, and UI interactions before writing the guide. Do not rewrite code unless requested.

For Form Action Custom Code step documentation, inspect `execute(...)`, action inputs, field/variable writes, SDK calls, trigger timing, and native fallback before writing the guide.

## Validation expectations

Validators should enforce:

- Custom Code controls require `render(...)`.
- Form Action Custom Code steps require `execute(...)`.
- Missing `CodeInApplication` export fails.
- Hooks and stateful function components fail for generated reusable code unless runtime proof exists.
- Unknown SDK calls require SDK-detail proof or a blocked status.
- Unresolved placeholders fail.
- A Form Action Custom Code step without export-proven package storage shape must not be advertised as package-ready.

## Direct native action dispatch

See [Direct Form Action execution](collection-01.md#direct-form-action-execution) for the product-provided `executeAction` contract, event and selected-record patterns, compatibility boundary, and runtime acceptance requirements.

---

<a id="yeeflow-data-and-dashboard-patterns"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#yeeflow-data-and-dashboard-patterns -->

# Yeeflow data and dashboard patterns

## List query pattern
Use `context.modules.yeeSDKClient.lists.queryItems(...)`.
Prefer a paginated helper for dashboards and large lists.

Typical pattern:
- `listId`
- `dataListId` when needed for runtime compatibility
- `fields`
- `fieldIds` or `selectedFields` when needed for runtime compatibility
- `pageIndex`
- `pageNo` when needed for runtime compatibility
- `pageSize`
- optional `filters`
- optional `sorts`

For configurable lookup/search controls:
- include identity fields such as `ListDataID`
- try the filtered query first, but if a `contains` filter returns no usable rows, run a limited broad fallback query and match locally on the configured display field
- treat empty filtered results as a possible filter-shape mismatch, not always as true no data
- document field id/name assumptions in a code comment

## Common safeguards
- render safe defaults when list id or field ids are missing
- avoid crashing on empty query results
- normalize strings, numbers, dates, and booleans with helper methods
- normalize expression-editor parameter values from primitives, arrays, and object shapes such as `value`, `label`, and `key`
- map field ids dynamically when the request is configurable
- read item values from direct row fields, `values`/`Values`, `fields`/`Fields`, and array-style `fieldValues`/`FieldValues`
- match returned field keys defensively, including case-insensitive keys and variants such as `FieldID`, `fieldCode`, and `ListDataId`
- normalize primitive, array, and object cell values; object cells may use `value`, `fieldValue`, `dataValue`, `valueText`, `text`, `display`, `name`, or `title` style keys

## Dynamic table pattern
For operational tables:
- build row models first
- then merge queried data into those rows
- preserve safe default values such as `0` or `N/A`
- use footer totals for numeric columns when requested
- use explicit row flags like `hasMatchedRecord` when styling matched rows

## Dashboard pattern
`antd` is acceptable for dashboards.
Useful components include:
- `Card`
- `Row`
- `Col`
- `Table`
- `Tag`
- `Progress`
- `Spin`
- `Alert`

Recommended dashboard structure:
1. KPI cards
2. distribution modules
3. trend modules
4. detailed analysis table

Dashboard variable guidance:
- Dashboard temp variables may not save through the same API path as form fields.
- Do not blindly assume `context.setFieldValue()` works for dashboard variables or temp variables.
- When templates write to dashboard variables/temp variables, resolve writable targets separately from expression-editor runtime values and try available variable/temp-variable setters defensively.

## Chart guidance
When charts are requested:
- use chart types only where they improve readability
- pie/donut for limited distributions
- horizontal bars for longer labels
- line/area for time trends
- keep colors restrained and business-readable

---

<a id="yeeflow-parameter-and-save-target-rules"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#yeeflow-parameter-and-save-target-rules -->

# Yeeflow parameter and save target rules

Use this reference when generating reusable Yeeflow custom code with expression-editor parameters, dynamic field references, variables, temp variables, or save/output targets.

## Parameter categories

Classify parameters before writing `inputParameters()`.

| Category | Typical Yeeflow type | Examples | Runtime handling |
| --- | --- | --- | --- |
| Plain text | `string` | label text, placeholder text, no-result text, helper copy | Trim to string with a default. |
| Expression editor | `variable` | `dataListId`, `displayField`, `valueField`, dynamic field/variable refs | Normalize from primitives, arrays, and expression objects. |
| Writable target | usually `variable` | `saveToField`, `outputField`, `resultTarget`, `newItemsField` | Resolve the configured target key separately from the current value. |
| Numeric config | usually `string` | `maxResults`, `pageSize`, `refreshSeconds` | Parse with safe fallback. |
| Boolean behavior | often `variable` | `multiSelect`, `allowManualEntry`, `showToolbar` | Normalize true/false from strings, numbers, booleans, arrays, and objects. |

## Expression editor values

Expression-editor and variable-picker parameters may arrive at runtime as:
- plain string
- number
- boolean
- array
- `{ value }`
- `{ label }`
- `{ key }`
- field binding object
- variable binding object
- temp variable binding object
- expression wrapper object
- the current value of the selected variable rather than the selected variable name

Do not rely on a single `String(context.params.someParam)` conversion. Use reusable helpers:
- `normalizeToString(value)`
- `normalizeToBoolean(value, fallback)`
- `normalizeToNumber(value, fallback)`
- `safeParseJson(value)`
- `resolveParameterValue(value)`

## Read/config parameter normalization

For normal read/config parameters, `value` is often the desired result.

Suggested precedence:
1. primitive string/number/boolean
2. array first item or joined values, depending on the template
3. object keys such as `value`, `Value`, `key`, `Key`, `id`, `Id`, `fieldId`, `FieldId`, `name`, `Name`, `label`, `Label`, `title`, `Title`
4. safe fallback

## Writable target resolution

Writable target parameters are different. They describe where to write, not what value to read.

For writable targets, prioritize target metadata before current value:
1. configured target captured by `requiredFields(params)` if available
2. `fieldId`, `FieldId`, `fieldName`, `FieldName`
3. `variableId`, `VariableId`, `variableName`, `VariableName`
4. `tempVariableId`, `TempVariableId`, `tempVariableName`, `TempVariableName`
5. `key`, `Key`, `id`, `Id`, `name`, `Name`, `code`, `Code`, `label`, `Label`
6. nested `target`, `binding`, `field`, `variable`, `tempVariable`, `data`, or `metadata`
7. plain string fallback only if it looks like a target key/name

Filter out candidates that are clearly values rather than targets:
- empty strings
- `[]`
- `{}`
- strings beginning with `[` or `{`
- JSON-like payloads containing `":` or `","`
- very long serialized strings

## requiredFields and target capture

When a writable target is selected from the Yeeflow variable dropdown, `context.params.targetParam` may evaluate to the current field/variable value at runtime. `requiredFields(params)` may still receive enough raw configuration to capture the field/temp-variable key.

Pattern:
1. In `requiredFields(params)`, collect writable target candidates from configured target parameters.
2. Return those configured target names where appropriate.
3. Store a target map on the application instance if the runtime preserves it.
4. In the React component, also use `fieldsValues` keys as an ordered fallback for configured output targets.

## Write methods

Do not assume every placement supports `context.setFieldValue` in the same way.

When a template supports forms and dashboards, try relevant setter paths defensively if exposed:
- `context.setFieldValue`
- `context.setFormFieldValue`
- `context.setValue`
- `context.setVariableValue`
- `context.setVariable`
- `context.setTempVariableValue`
- `context.setTempVariable`
- `context.setDashboardVariableValue`
- `context.setPageVariableValue`
- object-style setters such as `setFieldsValue`, `setVariables`, or `setTempVariables`

Check these methods on possible host objects:
- `context`
- `context.formContext`
- `context.pageContext`
- `context.dashboardContext`
- `context.variableContext`
- `context.variables`
- `context.tempVariables`
- `context.runtimeContext`

Use `fieldsValues[target] = value` only as a last fallback. It may update the local render context but should not be treated as guaranteed persistence.

## Dashboard temp variables

Dashboard temp variables may behave differently from approval form fields and data list form fields.

Important rules:
- Treat dashboard temp-variable output as a separate save path.
- Test saving separately from UI rendering.
- Expect selected temp-variable parameters to evaluate to the temp variable's current value.
- Recover the writable target key from configuration metadata, `requiredFields`, or defensively from `fieldsValues`.

## Smart Lookup Picker lessons

The first reusable template showed these concrete failure modes:
- Plain string parameters were too limited for reusable list ids, field ids, save fields, and booleans.
- Save targets chosen from the variable dropdown did not behave like manually typed strings.
- Dashboard temp-variable targets needed target-key resolution before writing.
- UI selection could work while save output remained empty, so save behavior needs its own tests.
- Search could return no visible options because row values were nested, differently cased, or stored under `fieldValue` / `dataValue` rather than direct keys.

---

<a id="yeeflow-product-ui-checklist"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#yeeflow-product-ui-checklist -->

# Yeeflow product ui checklist

Before returning Yeeflow UI code, check:

1. **structure**
- is the first visible area immediately useful?
- are sections grouped clearly?
- is the layout readable on desktop and reasonable on smaller widths?

2. **hierarchy**
- are headings, KPI values, labels, and detail text visually separated?
- does the user know what matters first?

3. **component patterns**
- are cards, tables, panels, and lists consistent?
- are paddings and borders restrained and even?

4. **visual restraint**
- no heavy shadows
- no loud gradients
- no noisy decorations

5. **realism**
- realistic labels
- realistic business usage
- no filler text or fake futuristic claims

6. **Yeeflow fit**
- does it feel like part of one Yeeflow product system?
- does it look implementable by a frontend team?

---

<a id="yeeflow-uiux-guidelines"></a>

<!-- source: skills/yeeflow-custom-code-generator/references/collection-01.md#yeeflow-uiux-guidelines -->

# Yeeflow ui and ux guidelines

Apply these rules to Yeeflow forms, tables, dashboards, portals, settings pages, editors, and product surfaces.

## Desired feel
The result should feel:
- modern
- calm
- structured
- trustworthy
- task-oriented
- polished
- scalable
- enterprise-ready
- product-led

## Avoid
Do not make the UI feel:
- generic
- cluttered
- overdecorated
- glow-heavy
- noisy
- overly futuristic
- disconnected from practical business use

## Visual rules
Rely on:
- spacing
- hierarchy
- grouping
- proportion
- alignment
- restrained depth
- consistent reusable patterns

Avoid:
- heavy shadows
- loud gradients
- decorative noise
- random card styles
- overexpressive effects

## Color rules
- use Yeeflow blue `#146FF6` as the main anchor color
- keep most surfaces neutral
- use subtle borders and readable text grays
- use additional accent colors sparingly

## Typography
- use clean scalable sans-serif typography
- keep labels concise and realistic
- avoid decorative editorial styles in product UI

## Interaction
- hover and focus states should be visible but restrained
- keep behavior predictable
- keep layouts responsive and intentionally designed
