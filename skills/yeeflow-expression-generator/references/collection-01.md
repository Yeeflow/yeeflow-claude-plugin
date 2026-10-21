# skills/yeeflow-expression-generator/references collection 1

This file combines 5 documents from `skills/yeeflow-expression-generator/references/` for the Claude directory build. A reference to `skills/yeeflow-expression-generator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [expression-editor-ui-contexts.md](#expression-editor-ui-contexts)
- [expression-function-reference.md](#expression-function-reference)
- [expression-operator-reference.md](#expression-operator-reference)
- [expression-recipes.md](#expression-recipes)
- [expression-token-model.md](#expression-token-model)

---

<a id="expression-editor-ui-contexts"></a>

<!-- source: skills/yeeflow-expression-generator/references/collection-01.md#expression-editor-ui-contexts -->

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

- use `<plugin-root>/yeeflow-expression-functions.normalized.json` and `<plugin-root>/yeeflow-expression-function-knowledge-base.normalized.json` for generation-safe functions
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

Use this context token with `getUserAttr` only after the target page is an approval-form expression context. Attribute selector parameters are descriptor objects such as `{ "key": "Email", "label": "Email" }`.

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

<a id="expression-function-reference"></a>

<!-- source: skills/yeeflow-expression-generator/references/collection-01.md#expression-function-reference -->

# Yeeflow Expression Function Reference

Sources:

- first-pass expression foundation: `expression training data generator.txt`
- second-pass enriched function knowledge base: `expression training data generator_2.txt`
- expression-editor screenshots for UI context and observed function-tab behavior

Use the normalized machine-readable reference in `<plugin-root>/yeeflow-expression-functions.normalized.json` for validators and generators.
Use `<plugin-root>/yeeflow-expression-function-knowledge-base.normalized.json` for richer bilingual descriptions, parameter names, business scenarios, and keywords.

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

Attribute parameters are descriptor objects, not plain strings:

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
      [{ "key": "Email", "label": "Email" }],
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
      [{ "type": "func", "func": "getUserAttr", "params": [[{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }], [{ "key": "DepartmentID", "label": "Department" }], [{ "type": "str", "value": "N/A" }]] }],
      [{ "key": "Name", "label": "Name" }],
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
      [{ "type": "func", "func": "dateAdd", "params": [[{ "type": "func", "func": "getUserAttr", "params": [[{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }], [{ "key": "LatestHireDate", "label": "Boarding Date" }], [{ "type": "str", "value": "N/A" }]] }], [{ "type": "str", "value": "year" }], [{ "type": "num", "value": "1" }]] }],
      [{ "type": "str", "value": "MMM DD, YYYY" }]
    ]
  }
]
```

---

<a id="expression-operator-reference"></a>

<!-- source: skills/yeeflow-expression-generator/references/collection-01.md#expression-operator-reference -->

# Expression Operator Reference

Allowed operators:

- `+`
- `-`
- `*`
- `/`
- `(`
- `)`
- `and`
- `or`
- `>`
- `>=`
- `<`
- `<=`
- `==`
- `!=`
- `&`

---

<a id="expression-recipes"></a>

<!-- source: skills/yeeflow-expression-generator/references/collection-01.md#expression-recipes -->

# Yeeflow Expression Generation Rules

Use `<plugin-root>/yeeflow-expression-functions.normalized.json`, `<plugin-root>/yeeflow-expression-function-knowledge-base.normalized.json`, `<plugin-root>/yeeflow-expression-operators.normalized.json`, and `yeeflow-expression-utils.js` before generating expressions.

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
- Export-back note from updated `Approval Form Controls Test v6`: sub list row calculations use `exprType: "variable_ctx"` current-object tokens with `ctx` equal to the parent list variable id. Sub list summaries live in `attrs["list-fields-summary"]`; a numeric summary can bind to a top-level number variable through `{ "prefix": "__variables_", "value": "TotalAmount" }`, and that variable can drive workflow numeric `conditioninfo`.

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
| Current user email/name/profile | `getUserAttr(Context:Current User, attr, fallback)` | Attribute params are descriptor objects such as `{ "key": "Email", "label": "Email" }`. |
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
      [{ "key": "Email", "label": "Email" }],
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
      [{ "type": "func", "func": "getUserAttr", "params": [[{ "id": "CurrentUser", "exprType": "application", "valueType": "string", "type": "expr", "name": "Context:Current User" }], [{ "key": "DepartmentID", "label": "Department" }], [{ "type": "str", "value": "N/A" }]] }],
      [{ "key": "Name", "label": "Name" }],
      [{ "type": "str", "value": "N/A" }]
    ]
  }
]
```

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

<a id="expression-token-model"></a>

<!-- source: skills/yeeflow-expression-generator/references/collection-01.md#expression-token-model -->

# Expression Token Model

Expressions are JSON arrays of Yeeflow tokens.

- string: `{ "type": "str", "value": "abc" }`
- number: `{ "type": "num", "value": "123" }`
- boolean: `{ "type": "bool", "value": true }`
- operator: `{ "type": "op", "op": "+" }`
- function: `{ "type": "func", "func": "now", "params": [] }`
- variable: `{ "exprType": "variable", "valueType": "number", "id": "Amount", "type": "expr", "name": "Workflow Variables:Amount" }`

Allowed variable value types are `number`, `text`, `date`, and `boolean`.
