# docs/studies collection 3

This file combines 12 documents from `docs/studies/` for the Claude directory build. A reference to `docs/studies/<name>.md` is the section that starts at the anchor `#<name>` below.

- [workflow-claim-task-action.md](#workflow-claim-task-action)
- [workflow-condition-editor-direct-value.md](#workflow-condition-editor-direct-value)
- [workflow-query-data-action.md](#workflow-query-data-action)
- [workflow-scheduled-vs-approval-data-list-actions.md](#workflow-scheduled-vs-approval-data-list-actions)
- [workflow-send-email-action.md](#workflow-send-email-action)
- [workflow-set-data-list-action.md](#workflow-set-data-list-action)
- [workflow-set-variable-action.md](#workflow-set-variable-action)
- [workflow-signal-event-action.md](#workflow-signal-event-action)
- [workflow-start-action-settings.md](#workflow-start-action-settings)
- [yap-schema-standard.md](#yap-schema-standard)
- [yeeflow-app-creation-rules-runtime-proof.md](#yeeflow-app-creation-rules-runtime-proof)
- [yeeflow-app-creation-rules.md](#yeeflow-app-creation-rules)

---

<a id="workflow-claim-task-action"></a>

<!-- source: docs/studies/collection-03.md#workflow-claim-task-action -->

# Workflow Claim Task Action Study

Proof boundary: this is export-proven, product-documented, config-reference-backed, and validator-backed learning only. No workflow was imported, published, triggered, claimed, approved, rejected, completed, reassigned, or emailed in this pass.

## Sources

| Source | Role | Proof label |
|---|---|---|
| `<downloads>/Workflow Actions Runtime Baseline (3)_Claim task.yap` | Export/schema source of truth | export-proven |
| `<downloads>/node-configurations.json` | Node configuration path/control-type reference | config-reference-backed |
| Yeeflow Help Center: Claim Task Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Product behavior and terminology | product-documented |
| Existing Assignment Task and task-form studies | Comparison reference | export-proven / validator-backed |

## Summary

The export contains four Claim Task nodes. All Claim Task nodes serialize as `CandidateTask`.

| Workflow host | WorkflowType | Claim Task count | Assignment Task comparison nodes | Notes |
|---|---:|---:|---:|---|
| Approval form workflow | `2` | 2 | 14 | Claim Task shares task form and `usertaskassignment[]` family with Assignment Task. |
| Data-list workflow | `1` | 2 | 1 | Data-list Claim Task can include direct user, applicant context expression, and list-item/Created By expression receiver sources. |
| Scheduled workflow | `3` | 0 | 1 | This export does not prove Scheduled Workflow Claim Task shape. |

## Claim Task Inventory

| Host | Internal type | Node label | Task type | Receiver/candidate source | Task form | Due date | Email config | Flow | Proof |
|---|---|---|---|---|---|---|---|---|---|
| Approval form | `CandidateTask` | Claim Task | `complete` | user-group expression | `WARTB Task4` | `duedatedefinition=120`, no `duedatetype` | fields present, disabled | 1 in / 1 out | export-proven |
| Approval form | `CandidateTask` | Claim Task | `approve` | user-group expression | `WARTB Task3` | `day`, value `5`, work-calendar true, `notifyrules=[]` | fields present, disabled | 1 in / 2 out | export-proven |
| Data-list | `CandidateTask` | Claim Task | `approve` | direct user + applicant line manager + Created By line manager | `Approval task` | `hour`, value `120`, `notifyrules=[]` | enabled with `to/subject/html` | 1 in / 2 out | export-proven |
| Data-list | `CandidateTask` | Claim Task | `complete` | direct user + applicant line manager + Created By line manager | `Approval task` | `hour`, value `120`, `notifyrules=[]` | enabled with `to/subject/html` | 1 in / 1 out | export-proven |

All IDs, user names, group names, and private organization references are redacted in normalized references.

## CandidateTask Mapping

`node-configurations.json` maps:

| Front-end action | Internal type | Config id | Usefulness | Proof |
|---|---|---|---|---|
| Claim Task | `CandidateTask` | `config_CandidateTask` | Confirms the internal type and expected property paths. | config-reference-backed + export-proven |
| Assignment Task | `MultiAssignmentTask` | `config_MultiAssignmentTask` | Useful comparison for shared task form, due date, and notification fields. | config-reference-backed + export-proven |
| Set variable | `SetVariableTask` | `config_SetVariableTask` | Useful for a future Set variable action study. | config-reference-backed |
| Set data list | `ContentList` | `config_ContentList` | Useful for a future Set data list action study. | config-reference-backed |
| Start | `StartNoneEvent` | `config_StartNoneEvent` | Useful shared Start action reference. | config-reference-backed |
| Connector/line | `SequenceFlow` | `config_SequenceFlow` | Useful for transition condition validation. | config-reference-backed |

The config reference is not runtime proof. The export wins when the config reference and actual export disagree.

Important mismatch: `config_CandidateTask` lists `properties.tasktype ` with a trailing space. The studied export uses `properties.tasktype` without trailing space for every Claim Task. Validators should warn if a trailing-space variant appears and preserve export field names when generating.

## Field Findings

| Field/path | Export finding | Config reference | Proof |
|---|---|---|---|
| `properties.name` | present as Claim Task node label | listed | export-proven + config-reference-backed |
| `properties.displayname` | not present in studied Claim Tasks | listed | config-reference-backed |
| `properties.usertaskassignment[]` | stores receivers/candidates | listed | export-proven + config-reference-backed |
| `properties.taskurl` | references task-form page IDs | listed | export-proven + config-reference-backed |
| `properties.tasktype` | `approve` and `complete` found | config has trailing-space path | export-proven |
| `properties.duedatetype` | `day` and `hour` found | listed | export-proven + config-reference-backed |
| `properties.duedatedefinition` | numeric values `5` and `120` found | listed | export-proven + config-reference-backed |
| `properties.isfromworkcalendar` | true found for day due date | listed | export-proven + config-reference-backed |
| `properties.notifyrules[]` | empty arrays found on due-date Claim Tasks | not listed in CandidateTask config | export-only |
| `properties.disablequickapproval` | not found in export | listed | config-reference-backed only |
| `properties.isenabledemail` | true/false found | listed | export-proven + config-reference-backed |
| `properties.to`, `subject`, `html` | present as expression/rich text fields | listed | export-proven + config-reference-backed |
| `properties.from`, `cc` | not found in studied Claim Tasks | listed | config-reference-backed only |
| `properties.files.list`, `files.variables` | not found | listed, list-workflow note | config-reference-backed only |

## Receiver/Candidate Sources

| Pattern | Found where | Normalized meaning | Runtime caveat |
|---|---|---|---|
| User group expression | Approval Claim Tasks | Candidate pool from user group users | Group expansion and claim ownership are not runtime-proven. |
| Direct user | Data-list Claim Tasks | A specific user can be part of receiver pool | Tenant-sensitive; do not commit real IDs. |
| Applicant line manager expression | Data-list Claim Tasks | Receiver derived from applicant context | Context resolution is not runtime-proven for Claim Task. |
| Created By line manager expression | Data-list Claim Tasks | Receiver derived from list-item Created By context | Data-list list-field receiver expansion is not runtime-proven. |

No position-based Claim Task receiver was found in this export, even though the config reference allows position methods.

## Claim Task vs Assignment Task

| Dimension | Assignment Task | Claim Task | Proof |
|---|---|---|---|
| Front-end purpose | Directly assigns work to one or more assignees. | Sends work to a receiver/candidate pool; a user claims ownership first. | product-documented |
| Internal type | `MultiAssignmentTask` | `CandidateTask` | export-proven + config-reference-backed |
| Main people field | `properties.usertaskassignment[]` means assignees. | `properties.usertaskassignment[]` means receivers/candidates. | export-proven |
| Task form | `properties.taskurl` references task form. | `properties.taskurl` references task form. | export-proven |
| Task type | approval/default or `complete`; previous studies also found approval/default by absence. | `approve` and `complete` found explicitly. | export-proven |
| Due date | same due date family, plus reminder rules in previous Assignment Task study. | `day`/`hour`, numeric definition, work-calendar flag, and empty `notifyrules[]` found. | export-proven |
| Email notification | `isenabledemail/to/subject/html` family. | same family found. | export-proven |
| Runtime ownership | assignee owns pending task directly. | receiver sees claimable task, then one claimant owns pending task. | product-documented, not runtime-proven here |
| Recommended use | Specific users must act. | A pool/team can voluntarily take ownership. | product-documented |

## Approval Workflow vs Data-List Workflow

| Capability | Approval workflow Claim Task | Data-list workflow Claim Task | Proof |
|---|---|---|---|
| Internal type | `CandidateTask` | `CandidateTask` | export-proven |
| Receiver field | `properties.usertaskassignment[]` | `properties.usertaskassignment[]` | export-proven |
| Task form field | `properties.taskurl` | `properties.taskurl` | export-proven |
| Receiver expressions | user group only in this export | direct user, applicant line manager, Created By line manager | export-proven |
| Data-list field values | not found | `listitem` / `CreatedBy` expression found | export-proven |
| Due date | `day` and plain numeric default found | `hour` found | export-proven |
| Email enabled | disabled in both approval Claim Tasks, fields still present | enabled in both data-list Claim Tasks | export-proven |
| File attachment fields | not found | not found | unproven |

## Help Center Comparison

| Concept | Product behavior | Export field/shape | Config reference | Match | Proof |
|---|---|---|---|---|---|
| Claim Task creates claimable work for a receiver pool | Receivers see task in Claim Tasks list and may claim it. | `CandidateTask.properties.usertaskassignment[]` stores receiver/candidate sources. | `config_CandidateTask.properties.usertaskassignment` | matched | product-documented + export-proven + config-reference-backed |
| Receiver options include users, job positions, groups, variables | Help article describes user picker, positions, groups, variables. | Export found user group expression, direct user, applicant expression, list-item expression. | Receiver schema allows direct/expression/position methods. | partially matched | product-documented + export-proven |
| Task form association | Claim Task selects a task form. | `properties.taskurl` resolves to task pages. | `properties.taskurl` | matched | product-documented + export-proven |
| Approval task | Claimed task can approve/reject and should have approved/rejected outgoing paths. | `tasktype="approve"` with two outgoing flows found. | `properties.tasktype` / config typo `tasktype ` | matched | product-documented + export-proven |
| Complete task | Claimed task can complete work items and may have one or more outgoing paths. | `tasktype="complete"` with one outgoing flow found. | `properties.tasktype` / config typo `tasktype ` | matched | product-documented + export-proven |
| Due date settings | supports hours/days/minutes/custom date variable. | `hour`, `day`, numeric definition, work-calendar flag found; minutes not found here. | due date fields listed | partially matched | product-documented + export-proven |
| Email notification | can send email notification to receivers. | `isenabledemail=true` plus `to/subject/html` found in data-list Claim Tasks. | email fields listed | matched | product-documented + export-proven |
| Quick completion | available for approval tasks per article. | `disablequickapproval` not found in export. | field listed | product-only/config-only | product-documented + config-reference-backed |

## Generation Rules

- Use `CandidateTask` for front-end Claim Task only when export-proven.
- Treat `properties.usertaskassignment[]` as receiver/candidate configuration, not direct final assignee ownership.
- Use Claim Task when a pool/team can voluntarily take ownership; use Assignment Task when specific users must take action directly.
- Preserve `properties.taskurl` and validate it resolves to a task-form page.
- Preserve `tasktype="approve"` or `tasktype="complete"` when present. Do not generate `properties.tasktype ` with a trailing space.
- Preserve due-date and email fields together when present.
- Preserve expression-button strings for user group, applicant, and list-item/Created By receiver sources. Do not convert them to static IDs.
- Do not invent users, groups, positions, departments, locations, or list fields. Use authorized read-only lookup only when needed.
- Do not claim claim-pool visibility, claim ownership locking, pending-task routing after claim, quick completion, or email delivery without a focused runtime baseline.

## Validator Recommendations

- Warning-first: validate `CandidateTask` receiver config as `properties.usertaskassignment[]`.
- Warn when receiver/candidate config is missing, empty, or malformed.
- Warn when `taskurl` is missing or does not resolve to a task form.
- Warn when explicit `tasktype` is not `approve` or `complete`.
- Warn on `properties.tasktype ` trailing-space variant.
- Warn when email is enabled but `to`, `subject`, or `html` is missing.
- Warn when due-date fields are inconsistent with `duedatetype`.
- Warn when list-item/Created By receiver expressions are used outside data-list workflows unless another export proves the host context.
- Keep runtime-sensitive behavior as warnings, not hard errors, in compatibility mode.

## Runtime Test Plan Addendum

Recommended next step is a focused runtime baseline for Claim Task designer/open/publish proof first:

- approval workflow Claim Task approval type with user-group receiver
- approval workflow Claim Task complete type with user-group receiver
- data-list workflow Claim Task approval and complete types with direct + expression receivers
- task-form selector/panel proof
- due-date and email configuration panel proof

Claim execution should remain deferred unless safe receivers are explicitly selected. Email delivery should remain out of scope unless safe recipients and delivery scope are explicitly approved.

## Runtime Baseline Update

The combined `Workflow Actions Batch Runtime Baseline` imported and opened successfully. The approval workflow designer rendered a `CandidateTask` Claim Task with receiver/candidate configuration and task-form selection, and the workflow published successfully. The data-list workflow designer also rendered a `CandidateTask` with direct user, applicant line manager, and Created By line manager candidate expressions plus task-form settings, and the data-list workflow published successfully.

This upgrades the Claim Task baseline to import/open/designer/publish-proven for the generated approval and data-list configuration shapes only. Claim execution, claim locking, pending-task routing, approve/reject/complete after claim, quick completion, and email delivery remain not runtime-proven.

---

<a id="workflow-condition-editor-direct-value"></a>

<!-- source: docs/studies/collection-03.md#workflow-condition-editor-direct-value -->

# Workflow Condition Editor Variable Conditions

Date studied: 2026-07-07

Source exports studied read-only:

- `Approval form condition sample 01.ywf`
- `workflow_condition_editor_sample.JSON`

## Scope

This study covers Yeeflow Workflow `SequenceFlow.properties.conditioninfo[]` rows where:

- the left operand is a Workflow variable selected from the Condition editor (`left.type = 1`);
- the right operand is either a direct/static value (`right.type = 0`) or an Expression editor value (`right.type = 2`).

The same rule applies to Approval form workflows, Data list workflows, and Scheduled workflows because they share the Workflow designer condition editor contract.

## Canonical Row Shape

```json
{
  "key": "774263d9-9944-410d-9fca-65d0225bf2f9",
  "pre": "and",
  "left": {
    "type": 1,
    "value": {
      "exprType": "variable",
      "valueType": "text",
      "id": "ProjectDetails",
      "type": "expr"
    }
  },
  "op": "s.=",
  "right": {
    "type": 0,
    "value": "abc"
  },
  "group": "string"
}
```

Required fields:

- `key`: stable condition row UUID for generated rows.
- `pre`: `and` or `or`.
- `left.type`: `1`.
- `left.value.exprType`: `variable`.
- `left.value.valueType`: the Workflow variable type.
- `left.value.id`: the Workflow variable id from `DefResource.variables`.
- `left.value.type`: `expr`.
- `op`: operator with the correct Condition editor type prefix.
- `right.type`: `0` for direct/static values, unless the operator is a null check.
- `right.type`: `2` for Expression editor values. `right.value` is an expression-token array.
- `group`: one of `general`, `string`, `number`, `boolean`, `datetime`.

## Condition Editor Type Mapping

The Workflow variable type is not always the same as the Condition editor group. The editor only exposes these groups:

| Condition editor group | Operator examples | Workflow variable types |
|---|---|---|
| `general` | `isNull`, `isNotNull` | `file`, `img`, `image`, `signature`, `signer` |
| `string` | `s.=`, `s.!=` | `text`, `list`, `richtext`, `metadata`, `mutiple-metadata`, `dict`, `lookup`, `user`, `groupselect`, `department`, `organization`, `location`, `costcenter` |
| `number` | `n.=`, `n.>=`, `n.>` | `number`, `decimal`, `currency`, `percent`, `rate` |
| `boolean` | `b.=`, `b.!=` | `boolean`, `bit`, `switch` |
| `datetime` | `dt.=`, `dt.!=`, `dt.>=`, `dt.<=`, `dt.>`, `dt.<`, `isNull`, `isNotNull` | `date`, `datetime`, `datepicker`, `time` |

IDs from selectable objects such as User, Department, Location, Cost Center, Metadata, Lookup, and Dictionary values are compared as `string` conditions. They are not `general` conditions merely because the UI value is an object in product terms.

## Direct Right Value Rules

- String conditions store literal values as strings, numbers, booleans, or arrays of strings for multi-select metadata.
- Number conditions store numeric literals as numbers or numeric strings.
- Boolean conditions may store `true`/`false` as booleans or strings. The export sample used `"true"`.
- Date Time conditions store direct date/time values as non-empty strings. Prefer the export-observed `YYYY-MM-DD HH:mm:ss` shape when the right value includes time, for example `"2026-07-07 04:45:58"`.
- General conditions are null checks only in this direct-value training round.
- `isNull`, `isNotNull`, `b.isTrue`, and `b.isFalse` do not include a right operand.

## Expression Editor Right Value Rules

Use `right.type = 2` only when the comparison value must be dynamic, calculated, function-derived, or assembled from multiple tokens. If the comparison value is fixed and known, prefer `right.type = 0` for a direct value.

Canonical expression right shape:

```json
{
  "right": {
    "type": 2,
    "value": [
      { "type": "str", "value": "ABC" },
      { "type": "op", "op": "&" },
      { "type": "str", "value": "D" }
    ]
  }
}
```

Export-observed golden reference patterns:

- String concatenation: `Name s.= "ABC" & "D"`.
- Numeric calculation: `Amount n.>= 1204 + 3`.
- Boolean dynamic result: `SensitiveData b.= iif(EquipmentType == "abc", true, false)`.
- Current user comparison: `ProjectManager s.= currentUser()`.
- Applicant Department: `ProjectDepartment s.= getUserAttr(Instance:Applicant, DepartmentID, [])`.
- Applicant Location: `ProjectLocation s.= getUserAttr(Instance:Applicant, LocationID, [])`.
- Cross-variable comparison: `EquipmentType s.= Workflow Variables:Select Project`.
- Relative Date Time: `EffectiveDate dt.>= dateAdd(now(), "month", 1)`.

`right.type = 2` values must be non-empty expression-token arrays. Do not serialize the expression as a raw string, legacy expression-button HTML, or a primitive value.

## Two-Layer Condition Groups

The Workflow Condition editor supports condition grouping with at most two layers:

1. top-level rows in `SequenceFlow.properties.conditioninfo[]`;
2. one child condition group layer in a top-level row's `conditions[]`.

There is no third nested condition group layer in the approved generation contract.

Export-proven top-level group wrapper shape:

```json
{
  "key": "5a50a125-3d96-46ad-990f-3f6b56ff3f3c",
  "pre": "and",
  "left": null,
  "op": "isNull",
  "right": null,
  "conditions": [
    {
      "key": "c82e2492-5826-401f-a0d4-3a84a7072966",
      "pre": "and",
      "left": {
        "type": 1,
        "value": {
          "exprType": "variable",
          "valueType": "date",
          "id": "EffectiveDate",
          "type": "expr"
        }
      },
      "op": "dt.>=",
      "right": {
        "type": 0,
        "value": "2026-07-07 05:21:53"
      },
      "group": "datetime"
    }
  ]
}
```

Rules:

- The group wrapper row uses `left: null`, `op: "isNull"`, and `right: null`.
- The group wrapper's `pre` connects the entire group to the previous top-level condition row.
- Each child condition row has its own `pre` value, either `and` or `or`.
- Use a child group with `pre: "and"` rows for grouped ranges, for example `EffectiveDate >= X AND EffectiveDate < Y`.
- Use a child group with `pre: "or"` rows for grouped alternatives, for example `Name == Food OR Name == Drink`.
- Every child condition row follows the same variable, group, operator, and right operand rules as a top-level row.
- A group must include at least one child condition.
- Do not put `conditions[]` inside another child condition group.

## Assignment Task Outcome Conditions

Assignment task outgoing branches such as `Approved`, `Rejected`, and `Completed` are not normal workflow-variable comparisons. They must compare the current task's `Outcome` property against a task outcome button value.

Export-proven shape:

```json
{
  "key": "5b2e9de3-fb38-43c5-abf6-fe57b49c243c",
  "pre": "and",
  "left": "<input type=\"button\" data=\"${&quot;type&quot;:&quot;task&quot;,&quot;param&quot;:{&quot;defid&quot;:&quot;<source task id>&quot;}, &quot;prop&quot;:&quot;Outcome&quot;}\" expr=\"__\" tabindex=\"-1\" value=\"Line manager approval:Outcome\">",
  "op": "s.=",
  "right": "<input type=\"button\" data=\"Approved\" expr=\"__\" tabindex=\"-1\" value=\"Task outcome:Approved\">"
}
```

Rules:

- `left` must be an export-style expression-button HTML string.
- `left.data` must contain `type = "task"`, `param.defid = <source Assignment task id>`, and `prop = "Outcome"`.
- `right` must be an export-style Task outcome expression-button string.
- `right.value` must match the outgoing branch: `Task outcome:Approved`, `Task outcome:Rejected`, or `Task outcome:Completed`.
- Do not generate a simplified workflow variable token such as `left.value.id = "Outcome"` for Assignment task output branches.

## Generation Rules

Generated direct-value workflow conditions must:

1. Resolve `left.value.id` to an existing Workflow variable before packaging.
2. Set `group` from the Workflow variable type using the mapping above.
3. Select an operator compatible with that group.
4. Use `right.type = 0` for fixed direct/static values.
5. Use `right.type = 2` only for dynamic, calculated, function-derived, or value-processed comparison operands.
6. Use `pre = "and"` by default; use `or` only when the business rule requires it.
7. Use top-level condition group wrappers when a business rule needs a nested set of child `and` or `or` rows. Keep condition nesting to two layers only.
8. Keep visible connector labels concise. Store the full logic in `conditioninfo`.
9. For Date Time workflow variables such as `EffectiveDate`, use `group: "datetime"` and the `dt.` operator family for equality and ordering comparisons. Use `right: null` for `isNull` / `isNotNull`.
10. For Assignment task outcome branches, use the task Outcome expression-button shape, not the workflow-variable left wrapper.
11. For business fan-out branches from the same workflow node, generate explicit coverage for all possible cases. Yeeflow workflow does not have an implicit `else` or `default` branch; an unconditioned outgoing SequenceFlow is not a valid fallback. If a variable such as `TravelType` has options `type1`, `type2`, `type3`, and `type4`, and the planned branches cover only `TravelType == type1` and `TravelType == type2`, generate another conditioned branch such as `TravelType != type1 AND TravelType != type2` to cover the remaining values. If all known options are individually routed, no complement branch is required.

Do not:

- use `s.=` for number variables;
- use `n.=` for text/status/string variables;
- treat User/Department/Location/Lookup values as `general`;
- leave `right` as a raw primitive when the Condition editor right wrapper is required;
- use `right.type = 2` for fixed literal values that should be direct `right.type = 0`;
- generate third-level nested `conditions[]` groups;
- generate group wrapper rows with real `left` operands or literal right operands;
- generate `left.value.id = "Outcome"` for Approval task `Approved` / `Rejected` / `Completed` branches;
- materialize a condition that references a missing Workflow variable.
- rely on a blank/unconditioned SequenceFlow as a workflow `else` / `default` branch;
- generate variable equality fan-out branches without either covering all known option values or adding an explicit `!= coveredValue` complement branch.

## Branch Coverage Rules

Workflow branch coverage is a logic-safety gate, not a visual layout rule. It applies to Approval form workflows, Data list workflows, and Scheduled workflows.

Required pattern for partial option routing:

```text
TravelType == type1 -> Branch A
TravelType == type2 -> Branch B
TravelType != type1 AND TravelType != type2 -> Branch C
```

The complement branch must contain real `conditioninfo[]` rows. Do not omit `conditioninfo` and call that line a default branch.

Validation requirements:

- For a source node with multiple business SequenceFlow branches, collect direct equality conditions by workflow variable.
- If the variable has known choices and every choice has a matching equality branch, the fan-out is covered.
- If only some choices are covered, at least one outgoing branch must contain `AND` not-equals rows for every already covered equality value.
- If choices are not known but the generator creates multiple equality branches for a variable, require the explicit not-equals complement branch.
- Skip Assignment task outcome branches because `Approved`, `Rejected`, and `Completed` are validated by the task outcome condition contract.

## Proof Boundary

This training is export-proven and validator-backed. It does not claim runtime execution proof for every operator or every expression function. Expression-editor right operands (`right.type = 2`) are learned from the provided export examples for generation and validation shape.

---

<a id="workflow-query-data-action"></a>

<!-- source: docs/studies/collection-03.md#workflow-query-data-action -->

# Workflow Query Data Action

Original source export: `AI Agent and Copilot Local Resource Baseline8.yap`

Focused extension sources: `Approval form workflow sample-V1.6.yapk`, `Approval form workflow sample-v1.7.yapk`, and `Approval form workflow sample-v1.10.yapk`.

Classification: export-proven action and orchestration shapes only. Query execution was not runtime-tested in this branch.

## Node Shape

Query data uses workflow node `stencil.id = "QueryData"`.

Observed properties:

| Property | Observed value / pattern |
| --- | --- |
| `name` | `Query Innovation ideas` |
| `appid` | app ID number |
| `listsetid` | containing application/listset ID |
| `listid` | target data list ID |
| `listtype` | `1` for data list |
| `filters` | array, empty in this export |
| `sorts` | array, empty in this export |
| `result.type` | `multiple` |
| `result.pageIndex` | `1` |
| `result.pageSize` | `1000` |
| `result.listName` | target workflow variable ID, `QueryItems` |
| `result.listParent` | `__variables_` |
| `result.vartype` | `text` |
| `result.fields[]` | selected field descriptors |
| `result.totalCount` | count variable ID, `QueryAmount` |
| `result.querycount_prefix` | `__variables_` |

## Target List

The action targets the local `Innovation Ideas` data list. Selected fields include all generated business fields plus system audit fields:

- `Title` / Idea Title
- `Text1` / Category
- `Text2` / Description
- `Text3` / Expected Benefit
- `Text4` / Status
- `Bit1` / Ready for Review
- `CreatedBy`, `Created`, `ModifiedBy`, `Modified`

## Output Variables

The multiple-query result is saved into workflow variable `QueryItems` as text. The result count is saved into numeric variable `QueryAmount`.

## V1.6 Result Modes

The focused V1.6 export adds three distinct result contracts:

- multiple count-only: empty row target plus `totalCount` number workflow variable;
- single to variables: `fieldMap` maps source fields to workflow variable IDs;
- multiple to List variable: `vartype = "list"`, `listParent = "__variables_"`, `fieldMap` targets ListRef fields, and `fields = null`.

It also proves a Scheduled Workflow chain of `QueryData -> Loop(list) -> MailTask`, plus count branching followed by `InvokeCode -> MailTask`. See `docs/standards/collection-03.md#workflow-query-data-golden-reference-standard` and `docs/reference/workflow-query-data-golden-references.json`.

## Generation Rules

- Resolve `properties.listid` to an included data list.
- Keep `result.listParent = "__variables_"` when writing to workflow variables.
- For multiple results into a text variable, include `result.fields[]` so the JSON output is shaped intentionally.
- For multiple results into a List variable, use `result.fieldMap`, a declared List variable linked to `variables.listref`, and keep `result.fields = null`.
- Count-only multiple results intentionally omit a row target; validators must not require `listName` when `totalCount` is the only output.
- Single results require a non-empty `fieldMap` to declared, type-compatible workflow variables.
- Use `properties.sorts` for the V1.6 export shape.
- Dynamic Lookup filters compare source `ListDataID` to the stored Lookup identity through expression-token arrays with `showCus: false`.
- Validate filters and sorts as arrays.
- V1.10 proves Data List (`listtype = 1`), Document Library (`16`), and Form Report (`32`) sources. Data Report remains deferred until its focused training round.
- Workflow `sorts[]` and Form Action `querydata_sorts[]` support at most two entries. Preserve `SortName` and boolean `SortByDesc`.
- Preserve large numeric IDs as strings.
- Do not claim query execution proof until a generated package has runtime-tested the workflow safely.

## Export Discrepancy

`Query Campaigns` exports with `filters: []`, although the accompanying business description says it filters by `SelectedCampaign`. The empty filter is not promoted as a Lookup-filter reference. `Get Campaign Owner` is the canonical V1.6 Lookup filter reference.

## V1.7 Data List Workflow Addendum

V1.7 proves a `WorkflowType = 1` count-only QueryData node that filters a related Event lookup by current Campaign `ListDataID`, stores the count in a number workflow variable, branches on `> 0` versus `<= 0`, and then uses ContentList edit/add. It also proves List Loop from a Data List Sub List (`__list_`), multiple-values Loop, fixed-times Loop, current iteration (`LoopIndex`), current item fields (`LoopItem.<field>`), and nested actions under `LoopBody.children[]`.

The two host `FlowMappings[]` rows use `Setting.NewTrigger = true` and non-null FieldName values that resolve to dedicated `flowstatus` fields. This proves that null and resolving flowstatus mappings are valid; a normal business field remains invalid.

## V1.10 Document Library and Form Report Addendum

V1.10 proves a Data List Workflow that queries an included Document Library (`listtype = 16`) and Form Report (`listtype = 32`), stores both multiple-row results in List workflow variables backed by matching ListRef definitions, and saves both result counts. It also proves non-default `pageIndex` values, Page Size values `3` and `1000`, and exactly two sorts per query. The `View event` custom form contains single-result Form Action queries against the same source types. Form Report is a query source, not a Form Action host.

---

<a id="workflow-scheduled-vs-approval-data-list-actions"></a>

<!-- source: docs/studies/collection-03.md#workflow-scheduled-vs-approval-data-list-actions -->

# Workflow Actions: Scheduled vs Approval and Data List

## Purpose

This quick export-learning pass compares Scheduled Workflow Start and Assignment Task action shapes against the already learned approval-form and data-list workflow behavior.

Source export:

```text
<downloads>/Workflow Actions Runtime Baseline (1).yap
```

Comparison sources:

- `docs/studies/collection-02.md#workflow-approval-vs-data-list-actions`
- `docs/studies/collection-03.md#workflow-start-action-settings`
- `docs/studies/collection-02.md#workflow-assignment-task-generation-rules`
- `docs/studies/collection-02.md#scheduled-workflow-generation-rules`
- `docs/studies/collection-02.md#scheduled-workflow-resource-structure`

Help Center references:

- Yeeflow Help Center: Start Action, https://<yourdomain>.yeeflow.com/<redacted-path>
- Yeeflow Help Center: Assignment Task Action, https://<yourdomain>.yeeflow.com/<redacted-path>
- Yeeflow Help Center: Add assignee with Assignee Editor, https://<yourdomain>.yeeflow.com/<redacted-path>

Raw `.yap` exports, decoded payloads, private IDs, user names, email addresses, tenant IDs, credentials, and raw API data are not committed.

## Export Inventory

| Item | Finding | Proof level |
|---|---|---|
| Export | `Workflow Actions Runtime Baseline (1).yap` decoded read-only | export-proven |
| App shell | `Workflow Actions Runtime Baseline` | export-proven |
| Scheduled Workflow count | 1 | export-proven |
| Scheduled Workflow name | `Schedule workflow sample` | export-proven |
| Scheduled Workflow type | `WorkflowType = 3`, `ListID = 0` | export-proven + validator-backed |
| Schedule settings | `TimeZone`, `Times[]`, `StartDate`, `EndDate`, `Frequency`, `Interval` | export-proven |
| Start action | One `StartNoneEvent` | export-proven |
| Assignment Task | One `MultiAssignmentTask` | export-proven |
| Other nodes | Three `SequenceFlow`, one `EndNoneEvent`, one `EndRejectEvent` | export-proven |

## Start Action Comparison

| Capability | Approval form workflow | Data-list workflow | Scheduled workflow | Proof level | Notes |
|---|---|---|---|---|---|
| Node type | `StartNoneEvent` | `StartNoneEvent` | `StartNoneEvent` | export-proven | Same broad workflow node family. |
| Incoming flow | none | none | none | export-proven + validator-backed | Help Center also describes Start as having no incoming connector. |
| Outgoing flow | one or more | one | one | export-proven + validator-backed | Scheduled export has one outgoing sequence flow. |
| Allow terminate | `terminate` and `terminate-conditions` found in approval export | absent in data-list export | absent in scheduled export | export-proven | Scheduled matches data-list absence in this export. |
| Allow recall | `revoke-conditions` found in approval export | absent in data-list export | absent in scheduled export | export-proven | Do not add recall fields to scheduled workflows from approval-form assumptions. |
| Conditions | approval Start condition rows found | not found in data-list Start | not found in scheduled Start | export-proven | Scheduled condition support remains unproven here. |
| Email notification | `isenabledemail`, `to`, `subject`, `html` | same field family present | same field family present | product-documented + export-proven | Email delivery was not tested. |
| Workflow host context | approval request/applicant context | list item trigger context | schedule resource context with application/applicant-style labels in expressions | export-proven | The scheduled export did not expose data-list field expression sources. |

## Assignment Task Comparison

| Capability | Approval form workflow | Data-list workflow | Scheduled workflow | Proof level | Notes |
|---|---|---|---|---|---|
| Action type | `MultiAssignmentTask` | `MultiAssignmentTask` | `MultiAssignmentTask` | export-proven | Same action family. |
| Assignee config | `properties.usertaskassignment[]` | same property family | same property family | export-proven + validator-backed | Scheduled export has one assignee entry. |
| Assignee source | users, groups, positions, applicant/context expressions | direct/expression/list-field sources | applicant line manager expression | export-proven | Scheduled export proves only the applicant-line-manager expression shape for this host. |
| Data-list field source | not applicable | Created By/list-item expression found | not found | export-proven | Supports the expectation that scheduled expression context is not data-list-field based in this export. |
| Task type | absent `tasktype` approval/default; `tasktype="complete"` in focused export | absent/default in studied data-list export | absent/default | export-proven | Complete task in Scheduled Workflow remains unproven here. |
| Appointed order | sequential/default patterns found | absent/default in studied data-list export | absent/default | export-proven | Scheduled sequential/parallel runtime remains untested. |
| Completion controls | `approveway` values and custom percentage found | `allapprove` found | `allapprove`, `approvepercentage: 100` | export-proven | Scheduled custom percentage not found in this export. |
| Due date/reminder | due-date and reminder fields found | due-date/reminder fields found | `duedatedefinition: 120`; no `duedatetype` or `notifyrules` | export-proven | Scheduled reminder rules not found. |
| Email notification | email fields found on some tasks | email fields found | `isenabledemail: false`; field present | export-proven | Scheduled Assignment Task email enabled shape not found. |
| Task form | approval task forms | data-list task forms with list-bound controls | `taskurl` page reference | export-proven | Scheduled task form page exists in `pageurls[]`; no list-bound task form behavior found. |

## Expression Context Comparison

| Source category | Approval form workflow | Data-list workflow | Scheduled workflow | Proof level | Notes |
|---|---|---|---|---|---|
| Applicant/request context | found | found in some labels | found | export-proven | Scheduled Start uses `Applicant:Email`; Assignment Task uses `Applicant:Line Manager`. |
| Workflow/instance context | found | found | found | export-proven | Scheduled Start subject references workflow name. |
| Data-list field values | not found | found, including Created By/list item context | not found | export-proven | No scheduled workflow list-field assignee source found in this export. |
| Workflow variables | found in earlier scheduled workflow AI/email learning | not the focus here | not found in this scheduled Start/Assignment Task sample | export-proven | Scheduled variable context remains proven by earlier scheduled workflow docs, not this export. |
| User/org reference sources | user, group, department, position, location shapes found in prior approval exports | user/group/list-field shapes found in prior data-list export | applicant line-manager expression only | export-proven | No API lookup was needed because committed results are category-level only. |

## Product-Behavior Reference

The Help Center says Start actions initiate workflows, have no incoming connector, and can be configured with email notification. The scheduled export matches the no-incoming-flow and email-field concepts, but does not prove terminate/recall fields for Scheduled Workflow.

The Help Center says Assignment Tasks can assign work to users, groups, positions, or expressions, can be approval or complete tasks, can use appointed order, and can have due dates and notifications. The scheduled export matches the Assignment Task action family and expression assignee storage, but only proves a narrow applicant-line-manager assignee plus default approval settings for Scheduled Workflow.

## Generation Rules

- Model Scheduled Workflow resources as `Data.Forms[]` entries with `WorkflowType = 3` and `ListID = 0`.
- Preserve Scheduled Workflow Start actions as `StartNoneEvent` with no incoming flow and at least one outgoing flow.
- For Scheduled Workflow Start actions, preserve email fields when present: `isenabledemail`, `to`, `subject`, and `html`.
- Do not copy approval-form `terminate`, `terminate-conditions`, or `revoke-conditions` fields into Scheduled Workflow Start actions unless a scheduled export proves them.
- Preserve Scheduled Workflow Assignment Tasks as `MultiAssignmentTask` with `properties.usertaskassignment[]` when found.
- Preserve applicant/context expression-button strings for Scheduled Workflow Assignment Tasks; do not convert them into static user IDs.
- Do not generate data-list field/Created By assignee expressions inside Scheduled Workflows unless another scheduled export proves a list/query context that provides those values.
- Do not execute, publish, or trigger Scheduled Workflows during export learning.
- Treat Scheduled Workflow Assignment Task routing, task creation, email delivery, due-date behavior, and appointed-order behavior as unproven until a focused runtime baseline explicitly tests them.

## Validator and Inspector Recommendations

- `scripts/inspect-scheduled-workflows.mjs` now reports redacted Scheduled Workflow Start and Assignment Task shapes.
- Validators should continue warning, not hard-erroring, for Scheduled Workflow Start absence of terminate/recall fields.
- Validators should accept `MultiAssignmentTask` in `WorkflowType = 3` workflows using the same warning-first assignee checks as approval and data-list workflows.
- Validators should warn for scheduled assignment expressions that expose data-list field context unless a scheduled export proves the context.
- Validators should warn that Scheduled Workflow execution, task routing, and email delivery require a separate runtime baseline.

## Runtime Boundary

This branch does not generate, import, publish, execute, or trigger a Scheduled Workflow. All Scheduled Workflow Start and Assignment Task findings are export-proven and validator-backed only. Existing Scheduled Workflow import/open/designer proof from earlier AI Assistant learning remains separate and does not prove Assignment Task routing.

Recommended next step: merge this as a quick export-learning milestone if review is clean, then rebuild the plugin because skill/docs/inspector mirrors changed.

## Claim Task Note

`Workflow Actions Runtime Baseline (3)_Claim task.yap` includes approval-form and data-list `CandidateTask` examples, but no Scheduled Workflow Claim Task. The user expectation that Scheduled Workflow Claim Task should be same/similar to approval/application workflow remains unproven by export in this pass.

Do not generate Scheduled Workflow Claim Task as a proven shape until a scheduled export contains `CandidateTask`, or until a focused scheduled runtime/design proof is explicitly scoped. Scheduled Workflow Assignment Task learning remains limited to `MultiAssignmentTask` from the previous scheduled workflow export.

## Set Variable Note

`Workflow Actions Runtime Baseline (4)_Set variable.yap` includes approval-form and data-list `SetVariableTask` examples, but no Scheduled Workflow Set variable node. The scheduled workflow in that export still has workflow variables, but this does not prove scheduled Set variable serialization, expression context, or runtime mutation behavior.

Do not generate Scheduled Workflow Set variable as a proven shape until a scheduled export contains `SetVariableTask`, or until a focused scheduled runtime/design proof is explicitly scoped.

## Set Data List Note

`Workflow Actions Runtime Baseline (5)_Set data list.yap` includes approval-form and data-list `ContentList` examples, but no Scheduled Workflow Set data list node. The studied scheduled workflow in that export has no `ContentList` action.

Do not generate Scheduled Workflow Set data list as a scheduled-workflow-proven shape until a scheduled export contains `ContentList`, or until a focused scheduled designer/open proof is explicitly scoped. Approval-form and data-list workflow Set data list findings remain export-proven for those hosts only.

## Signal Event Note

`Workflow Actions Runtime Baseline (6)_Signal event.yap` includes one approval-form workflow `SignalEvent`, but no data-list or Scheduled Workflow Signal event. The studied scheduled workflow in that export has no `SignalEvent` action.

Do not generate Scheduled Workflow Signal event as a proven shape. The current Signal event proof is limited to approval-form workflow, where it has no incoming flow, one outgoing flow, and `properties.eventdefinitions[]` containing `RevokeEventDefinition` and `CancelEventDefinition`. Scheduled workflow recall/terminate event handling remains unproven.

---

<a id="workflow-send-email-action"></a>

<!-- source: docs/studies/collection-03.md#workflow-send-email-action -->

# Workflow Send Email Action

Source export: `<downloads>/AI Agent and Copilot Local Resource Baseline8.yap`

Classification: export-proven action shape only. Email sending was not runtime-tested.

## Node Shape

Send email uses workflow node `stencil.id = "MailTask"`.

Observed properties:

| Property | Description |
| --- | --- |
| `name` | node display name |
| `to` | recipient rich-text string; can be a fixed email text or expression-button HTML |
| `subject` | subject rich-text string; can be literal or expression-button HTML |
| `html` | body HTML/rich-text string |
| `cc` | supported by normalized action reference, not present in the studied nodes |
| `from` | supported by normalized action reference, not present in the studied nodes |
| `files` | supported by normalized action reference, not present in the studied nodes |

## Weekly Test Email

`Weekly information update` contains one `MailTask`:

- node name: `Send Test Email`
- recipient: fixed email text, redacted as `<REDACTED_EMAIL>`
- subject: `Test email`
- body: simple HTML paragraphs

This is useful for action anatomy but must not be executed during automated tests because the recipient is a real fixed address in the source export.

## Daily Generated Email

`Daily information update` contains one `MailTask`:

- recipient: fixed email text, redacted as `<REDACTED_EMAIL>`
- subject: expression-button HTML referencing workflow variable `EmailSubject`
- body: expression-button HTML referencing workflow variable `EmailBody`

The workflow variable expression button pattern is:

```html
<input type="button" data="${&quot;type&quot;:&quot;variable&quot;, &quot;param&quot;:{&quot;id&quot;:&quot;EmailSubject&quot;}}" expr="__" tabindex="-1" value="Workflow Variables:Subject">
```

## Safety Rules

- Redact fixed recipients in docs and normalized references.
- Warn on fixed email recipients in compatibility validation.
- Do not run Scheduled Workflow or Send email actions unless recipients are explicitly safe test recipients.
- For generated packages, prefer placeholder/example recipients or keep the workflow disabled/non-executed until safe runtime behavior is proven.
- Validate `to`, `subject`, and `html` are present before any runtime test.

---

<a id="workflow-set-data-list-action"></a>

<!-- source: docs/studies/collection-03.md#workflow-set-data-list-action -->

# Workflow Set Data List Action Study

Proof boundary: this is export-proven, product-documented, config-reference-backed, and validator-backed learning only. No workflow was imported, published, triggered, submitted, or executed in this pass. No approval request was submitted, no data-list item was created/updated/deleted, and no email was sent.

## Sources

| Source | Role | Proof label |
|---|---|---|
| `/Users/Renger/Downloads/Workflow Actions Runtime Baseline (5)_Set data list.yap` | Export/schema source of truth | export-proven |
| `/Users/rengerhu/Downloads/Leave Management-v1.1.yapk` | Approval, Data List, and Scheduled Workflow host coverage | export-proven |
| `/Users/rengerhu/Downloads/Leave Management-v1.3.yapk` | Type 16 Document Library target, file mapping, folder paths, and Loop upload coverage | export-proven |
| `/Users/rengerhu/Downloads/Leave Management-v1.4.yapk` | Direct bulk Sub list-to-record writes for Approval and Data List workflows | export-proven |
| `/Users/Renger/Downloads/node-configurations.json` | Node configuration path/control-type reference | config-reference-backed |
| Yeeflow Help Center: Set Data List Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Product behavior and terminology | product-documented |
| Yeeflow Help Center: Set data list step, https://<yourdomain>.yeeflow.com/<redacted-path> | Related form-action behavior for current list and selected data source | product-documented |
| Yeeflow Help Center: Set Variable Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Related workflow variable distinction | product-documented |
| Existing workflow Set variable, Claim Task, Assignment Task, task-form, and workflow-host studies | Comparison/reference | export-proven / validator-backed |

## Summary

The baseline and Leave Management v1.1 exports contain Set data list nodes. All Set data list nodes serialize as `ContentList`.

| Workflow host | WorkflowType | `ContentList` count | Operations found | Target modes found | Notes |
|---|---:|---:|---|---|---|
| Approval form workflow | `2` | 6 | `add`, `edit`, `remove` | `select` | Targets included data lists. Proves selected-data-source add/update/delete, filters, numeric operation codes, and approval-form sub-list value mappings. |
| Data-list workflow | `1` | 4 | `add`, `edit` | `current`, `select` | Proves current-list mode, selected-list mode, list-field expression values, and data-list sub-list value mappings. |
| Scheduled workflow | `3` | 4 | `add`, `edit`, `remove` | `select` | Leave Management v1.1 proves selected-target add/remove and Loop-body edit with Multiply/Divide mappings. |

The studied app includes these relevant data sources:

- approval form `Workflow Action Approval Test`
- data list `Purchase Requests Runtime Test`
- data list `Products`
- data list `Save Sub Items`
- form report `Another approval form report`

All exported numeric IDs and private references are redacted in normalized references.

## Node Inventory

| Host | Internal type | Node label | `listtype` | `type` | Target | Mappings | Filters | Notes | Proof |
|---|---|---|---|---|---|---:|---:|---|---|
| Approval form | `ContentList` | Set Data List_1 | `select` | `add` | Purchase Requests Runtime Test | 9 | 0 | Creates a target record from workflow variables and instance context. | export-proven |
| Approval form | `ContentList` | Set Data List | `select` | `edit` | Products | 1 | 1 | Updates Product amount with numeric operation code `2`. | export-proven |
| Approval form | `ContentList` | Set Data List_2 | `select` | `edit` | Purchase Requests Runtime Test | 4 | 1 | Updates target record by Form ID; includes numeric operation codes `3` and `4`. | export-proven |
| Approval form | `ContentList` | Set Data List | `select` | `edit` | Products | 1 | 1 | Updates Product amount with numeric operation code `1`. | export-proven |
| Approval form | `ContentList` | Set Data List_3 | `select` | `remove` | Purchase Requests Runtime Test | 0 | 1 | Deletes by Form ID filter. High-impact behavior not runtime-tested. | export-proven |
| Approval form | `ContentList` | Set Data List_5 | `select` | `add` | Save Sub Items | 6 | 0 | Maps approval-form sub-list fields into detail-record fields. | export-proven |
| Data-list workflow | `ContentList` | Set Data List | `current` | `add` | Purchase Requests Runtime Test | 4 | absent | Current-list mode updates/mutates the host list context; value expressions can read list fields. | export-proven |
| Data-list workflow | `ContentList` | Set Data List | `select` | `add` | Products | 3 | 0 | Adds to another data list using list-field and workflow-variable values. | export-proven |
| Data-list workflow | `ContentList` | Set Data List | `select` | `edit` | Products | 1 | 1 | Updates selected Products row by Title filter. | export-proven |
| Data-list workflow | `ContentList` | Set Data List | `select` | `add` | Save Sub Items | 6 | 0 | Products-list workflow maps data-list sub-list fields to multiple detail records. | export-proven |
| Data-list workflow | `ContentList` | Update default usage | `select` | `remove` | Leave Usage Statistics | 0 | 1 | Deletes a selected target row with an exact filter. | export-proven (Leave Management v1.1) |
| Scheduled workflow | `ContentList` | Update Information | `select` | `edit` | Employee Leave Balances | 1 | 1 | Loop-body numeric mappings prove Multiply (`Per=3`) and Divide (`Per=4`). | export-proven (Leave Management v1.1) |
| Scheduled workflow | `ContentList` | Set Data List Sample | `select` | `add`, `remove` | Leave Usage Statistics / Employee Leave Balances | varies | varies | Scheduled selected-target maintenance shapes. | export-proven (Leave Management v1.1) |

Leave Management v1.3 proves selected Type 16 Document Library targets for add/edit/remove, plus a Document Library host workflow using current-record update. File mutation behavior remains runtime-proof-required.

Leave Management v1.4 proves direct bulk Sub list writes without a Loop wrapper. A selected-target `ContentList` `add` emits one target row for every source row when mapping tokens use `key: "_list.<child field>"`. Approval/Scheduled workflows use `exprType="variable"` with the Workflow List variable ID; Data List workflows use `exprType="list_field"` with the current host Sub list field ID. A parent-form/list mapping such as Applicant/Employee remains alongside the row mappings to retain source-row provenance.

## Data Source Target Selection

| Pattern | Export shape | Host found | Notes | Proof |
|---|---|---|---|---|
| Selected data source | `properties.listtype="select"` plus `properties.appid`, `properties.listsetid`, `properties.listid` | approval and data-list workflows | Used for `Purchase Requests Runtime Test`, `Products`, and `Save Sub Items`. | export-proven |
| Current list | `properties.listtype="current"` plus current host list `properties.listid` and no selected metadata | data-list workflow | Found on Purchase Requests Runtime Workflow. Product docs say current-list mode is list-form/list-context only. | export-proven + product-documented |
| Document library target | `listtype="select"` plus Type 16 target metadata and `_Path` / `Text4` mappings | Approval and Data List workflows | Leave Management v1.3 proves single-file add, Loop multi-file add, folder-scoped edit/remove, and native current-document update. | export-proven; runtime execution required |

Generation rule: use `listtype="current"` only when a data-list/list-context host is present and the intent is to mutate the current item/list context. Use `listtype="select"` for other data lists or future document-library targets.

## Operation Types

| Operation | Export shape | Field mappings | Filters | Safety note | Proof |
|---|---|---|---|---|---|
| Add | `properties.type="add"` | `properties.listdatas[]` | usually empty/absent | Creates records/items; runtime mutation not tested here. | export-proven |
| Update/Edit | `properties.type="edit"` | `properties.listdatas[]` | `properties.wheres[]` found on every studied selected-list edit | Treat missing/broad filters as high risk. | export-proven |
| Delete/Remove | `properties.type="remove"` | `listdatas` may be absent/object/unused | `properties.wheres[]` found | High-impact destructive action; require explicit intent and safe filters before generation/runtime. | export-proven |

The Help Center describes Set Data List as the workflow action for adding, updating, or deleting records in a selected data list, and notes that update/delete behavior depends on conditions. Product docs differ slightly on missing-filter behavior across workflow action and form-action articles, so generation should warn strongly and require explicit safe filters for edit/remove actions rather than relying on either interpretation.

## Field Mapping Shape

Add and edit operations use `properties.listdatas[]`. Each mapping entry found in the export includes:

| Field | Meaning | Export finding |
|---|---|---|
| `Columns` | target field name on the selected/current data source | found on every mapping |
| `Per` | operation code for field assignment or numeric operation | codes `0`, `1`, `2`, `3`, `4` found |
| `Data` | expression-token array for the mapped value | found on every mapping |

Value sources found in `Data`:

- fixed text and number tokens
- workflow variable tokens
- application/instance context tokens, such as Form ID, Submitter, and Submission time
- data-list field tokens with `exprType="list_field"`
- expression operators, such as text concatenation
- sub-list/detail-row field references

## Numeric Operation Codes

The export proves that numeric field mappings still use `listdatas[].Data` for the right-side value and store the operation mode in `listdatas[].Per`.

| `Per` code | Product/UI meaning | Export evidence | Proof |
|---|---|---|---|
| `0` | Value/default set | found across text, date, lookup, user, and number fields | export-proven code; product/user-understanding-backed label |
| `1` | Increase | found on Decimal field mappings | export-proven code; product/user-understanding-backed label |
| `2` | Decrease | found on Decimal field mappings | export-proven code; product/user-understanding-backed label |
| `3` | Multiply | found on Decimal field mappings | export-proven code; product/user-understanding-backed label |
| `4` | Divide | found on Decimal field mappings | export-proven code; product/user-understanding-backed label |

The `node-configurations.json` reference only lists `Per` enum `["0"]`, so the export is the source of truth for codes `1..4`. Validators should warn on unknown codes and should validate numeric-operation use against target field metadata when available.

## Filter / `wheres[]` Shape

Edit and remove operations use `properties.wheres[]`.

Found condition fields:

| Field | Export finding |
|---|---|
| `key` | condition identifier, redacted in normalized refs |
| `pre` | logical connector, `and` found |
| `left` | target field name, such as `ListDataID`, `Text1`, or `Title` |
| `op` | operator code, `0` found for equals |
| `right` | expression-token array or static token array |
| `showCus` | `false` found |
| `conditions` | supported by config reference, not populated in this export |

No studied edit/remove node has an empty filter. Validators should still warn strongly when edit/remove has missing or empty `wheres[]`, because broad or destructive mutation is a business-safety risk.

## Sub-List To Multiple Records Pattern

The export includes two Set data list nodes that map sub-list/detail-row values into `Save Sub Items` records:

| Source host | Target | Source value shape | Target fields | Proof |
|---|---|---|---|---|
| Approval form workflow | Save Sub Items | workflow variable references named like `Workflow Variables:Sub List Items:field1` | `Text2`, `Decimal2`, `Text3` plus parent/form/product fields | export-proven |
| Data-list workflow | Save Sub Items | `exprType="list_field"` with `valueType="list"` and names like `List Fields:Sub List Items:field1` | `Text2`, `Decimal2`, `Text3` plus current row/product fields | export-proven |

The product expectation is that sub-list rows can be saved to multiple target records. This export proves the mapping schema but does not execute it, so record count and row iteration behavior remain not runtime-proven.

## Approval Workflow vs Data-List Workflow

| Capability | Approval form workflow | Data-list workflow | Proof |
|---|---|---|---|
| Internal type | `ContentList` | `ContentList` | export-proven |
| Selected data source | found | found | export-proven |
| Current list mode | not found | found | export-proven |
| Add operation | found | found | export-proven |
| Edit operation | found | found | export-proven |
| Remove operation | found | found | export-proven, including Leave Management v1.1 |
| List-field values | not applicable except workflow variables/sub-list variable references | found as `exprType="list_field"` | export-proven |
| Sub-list values | approval sub-list workflow variable references | data-list sub-list list-field references | export-proven |
| Document library target | selected Type 16 target found | selected Type 16 target found | export-proven shape; runtime mutation unproven |

## Set Data List vs Set Variable

| Dimension | Set variable | Set data list |
|---|---|---|
| Internal type | `SetVariableTask` | `ContentList` |
| Target | workflow variables | data-source records and fields |
| Left side | workflow variable assignment row | data-list/document-library field mapping |
| Right side | expression-token array in `variablesetting[].value` | expression-token array in `listdatas[].Data` |
| Data-list workflow list fields | may be read as right-side values | may be read as right-side values and may target current/selected list fields |
| Field mutation | not a Set variable behavior | core Set data list behavior |
| Runtime proof in this branch | none | none |

Generation rule: if the business requirement says to change data-list fields or create/update/delete records, use Set data list / `ContentList`, not Set variable / `SetVariableTask`.

## Node Configuration Reference

`node-configurations.json` maps Set data list to `ContentList` and lists these useful paths:

- `properties.listtype`
- `properties.appid`
- `properties.listsetid`
- `properties.listid`
- `properties.type`
- `properties.listdatas`
- `properties.wheres`

The reference is useful for control-type mapping and path discovery. The export remains source of truth when the reference is incomplete: the reference lists `listdatas[].Per` enum `["0"]`, but the export proves numeric operation codes `1`, `2`, `3`, and `4`.

## Help Center Comparison

| Concept | Product behavior | Export-proven field/shape | Config path | Match | Proof |
|---|---|---|---|---|---|
| Set Data List purpose | Add, update, or delete records from a data list. | `ContentList.properties.type` values `add`, `edit`, `remove` | `properties.type` | matched | product-documented + export-proven |
| Target data source | Select application and data source. | selected target metadata in `appid`, `listsetid`, `listid` | `properties.appid/listsetid/listid` | matched | product-documented + export-proven |
| Current list option | Current list applies to list/form context. | `listtype="current"` in data-list workflow | `properties.listtype` | matched | product-documented + export-proven |
| Selected data source can be data list/document library | Product docs mention data list or document library for related form-action step. | only data-list targets found | same target metadata paths | partial | product-documented; document-library target unproven here |
| Field mapping | User selects fields and values through expression editor. | `listdatas[].Columns` + `listdatas[].Data` | `properties.listdatas` | matched | product-documented + export-proven |
| Update/delete filters | Conditions locate affected records. | `wheres[]` with `pre`, `left`, `op`, `right` | `properties.wheres` | matched | product-documented + export-proven |
| Sub-list values | Product docs describe using sub-list fields in Set Data List workflows. | approval and data-list sub-list field references found | `properties.listdatas[].Data` | matched | product-documented + export-proven schema |

## Validator Recommendations

- Warning-first: validate `ContentList`.
- Warn when `properties.listtype` is not `current` or `select`.
- Warn when `properties.type` is not `add`, `edit`, or `remove`.
- For selected-data-source mode, warn when `appid`, `listsetid`, or `listid` is missing.
- For add/edit, warn when `listdatas[]` is missing, not an array, empty, or has entries missing `Columns`, `Per`, or `Data`.
- Validate `Data` as expression-token arrays where possible; tolerate export-proven `exprType="list_field"` tokens.
- Warn when `Per` uses an unknown numeric operation code.
- When field metadata is available, warn if numeric operation codes `1..4` are used on non-number fields.
- For edit/remove, warn strongly when `wheres[]` is missing or empty.
- Validate `wheres[]` condition fields warning-first: `pre`, `left`, `op`, `right`, and nested `conditions`.
- Accept nested conditions but warn on unknown operators.
- Recognize current-list mode only in data-list/list-context workflows unless another export proves more.
- Recognize sub-list-to-multiple-record mappings as export-proven schema but not runtime-proven execution.
- Recognize document-library target only when an export proves it.

## Generation Rules

- Use `ContentList` for front-end Set data list only when export-proven.
- Preserve `properties.listtype`.
- Use `listtype="current"` for current data-list/list context only.
- Use `listtype="select"` plus selected metadata for external data lists or future document-library targets.
- Preserve `properties.type` as `add`, `edit`, or `remove`.
- Add/edit operations should include `properties.listdatas[]`.
- Edit/remove operations should include explicit safe `properties.wheres[]` filters.
- Do not generate delete/remove unless the user explicitly asks for destructive behavior and provides safe filter intent.
- Use `listdatas[].Columns` for target fields and `listdatas[].Data` for expression-editor values.
- Preserve `listdatas[].Per` numeric operation codes. Use codes `1..4` only for number fields when metadata supports it.
- For sub-list-to-record generation, preserve parent/request reference mappings so detail records can be associated back to the source.
- Do not claim record creation/update/delete, current-list mutation, document-library mutation, or sub-list row iteration behavior without focused runtime proof.

## Signal Event Cleanup Note

`Workflow Actions Runtime Baseline (6)_Signal event.yap` proves one approval-form `SignalEvent` branch that connects to a Set data list / `ContentList` edit action. The downstream action uses `listtype="select"`, `type="edit"`, one `listdatas[]` mapping, and one nonempty `wheres[]` filter.

This is an export-proven compensation/cleanup schema only. It supports generation guidance that recall/terminate event branches can use Set data list to release or reverse related records, but the workflow event firing, numeric operation execution, and record mutation remain not runtime-proven. Keep the normal high-impact Set data list safeguards: edit/remove actions need explicit safe filters, and destructive cleanup should use disposable records in runtime tests.

## Runtime Test Plan Addendum

Recommended future runtime baseline:

- designer/open proof for approval workflow selected-list add/edit/remove nodes
- designer/open proof for data-list workflow current-list Set data list node
- designer/open proof for selected external Products and Save Sub Items targets
- designer/open proof for field mappings, numeric operation modes, and filters
- designer/open proof for approval-form and data-list sub-list-to-record mappings
- optional execution proof only with disposable lists/items and explicit safe filters

Execution proof should be split by risk:

- add-to-disposable-list proof can be considered first
- edit proof requires a single disposable target row and a narrow filter
- remove proof should remain deferred unless the user explicitly approves destructive testing on disposable data
- document-library mutation should wait for a dedicated export and runtime plan

## Runtime Baseline Update

The combined `Workflow Actions Batch Runtime Baseline` imported and opened successfully. The approval workflow designer rendered selected-list `ContentList` nodes for add, edit, remove, sub-list/detail-row mapping, and Signal-event cleanup settings, and the approval workflow published successfully. The data-list workflow designer rendered current-list and selected-list `ContentList` nodes, and the data-list workflow published successfully.

This upgrades the generated Set data list shapes to import/open/designer/publish-proven only. Record creation, update, delete/remove, current-list mutation, sub-list row iteration, numeric operation execution, document-library mutation, downstream cleanup execution, and Products workflow triggering remain not runtime-proven.

---

<a id="workflow-set-variable-action"></a>

<!-- source: docs/studies/collection-03.md#workflow-set-variable-action -->

# Workflow Set Variable Action Study

Proof boundary: this is export-proven, product-documented, config-reference-backed, and validator-backed learning only. No workflow was imported, published, triggered, submitted, or executed in this pass.

## Sources

| Source | Role | Proof label |
|---|---|---|
| `<downloads>/Workflow Actions Runtime Baseline (4)_Set variable.yap` | Export/schema source of truth | export-proven |
| `<downloads>/node-configurations.json` | Node configuration path/control-type reference | config-reference-backed |
| Yeeflow Help Center: Set Variable Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Product behavior and terminology | product-documented |
| Yeeflow Help Center: Set Data List Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Related action distinction | product-documented |
| Existing workflow action studies | Assignment, Claim, Start, task-form comparison | export-proven / validator-backed |

## Summary

The export contains seven Set variable nodes. All Set variable nodes serialize as `SetVariableTask`.

| Workflow host | WorkflowType | Set variable count | Current workflow assignments | Another approval workflow assignments | Notes |
|---|---:|---:|---:|---:|---|
| Approval form workflow | `2` | 5 | 4 nodes | 1 node | Proves single-variable, multiple-variable, and another approval workflow settings. |
| Data-list workflow | `1` | 2 | 1 node | 1 node | Proves workflow-variable targets and data-list field values on the right side of expressions. |
| Scheduled workflow | `3` | 0 | 0 | 0 | This export does not prove Scheduled Workflow Set variable shape. |

The sample also includes target support resources:

- approval form `Workflow Action Approval Test`
- data list `Purchase Requests Runtime Test`
- target approval form `Another approval from`
- report `Another approval form report`, which exposes submitted approval request form IDs for the target approval form

All exported numeric IDs and private references are redacted in normalized references.

## Set Variable Node Inventory

| Host | Internal type | Node label | `formtype` | Variables set | Variable types | Value sources | Flow | Proof |
|---|---|---|---|---:|---|---|---|---|
| Approval form | `SetVariableTask` | Set Variable | `current` | 1 | text | fixed string + workflow variable expression | 1 in / 1 out | export-proven |
| Approval form | `SetVariableTask` | Set Variable | `current` | 1 | text | fixed string | 1 in / 1 out | export-proven |
| Approval form | `SetVariableTask` | Set Variable | `current` | 1 | number | `iif`, `isNullOrEmpty`, current workflow variable | 1 in / 1 out | export-proven |
| Approval form | `SetVariableTask` | Set Variable | `current` | 3 | number, text | fixed values, workflow variables, functions | 1 in / 2 out | export-proven |
| Approval form | `SetVariableTask` | Set Variable | `custom` | 2 | number, text | workflow variable expression + fixed value | 1 in / 1 out | export-proven |
| Data-list workflow | `SetVariableTask` | Set Variable | `current` | 2 | number, text | workflow variable expression + fixed number | 1 in / 1 out | export-proven |
| Data-list workflow | `SetVariableTask` | Set Variable | `custom` | 4 | date, number, text | fixed values + data-list field expressions | 1 in / 1 out | export-proven |

## Current Workflow Variables

`formtype="current"` targets variables in the current workflow. The export proves both single-variable and multiple-variable settings in one action.

Each row under `properties.variablesetting[]` includes:

| Field | Meaning | Export finding |
|---|---|---|
| `idx` | variable row/reference id | present on every assignment |
| `id` | target workflow variable id | present on every assignment |
| `name` | target workflow variable display name | present on every assignment |
| `type` | target workflow variable type | `text`, `number` found |
| `editable` | variable metadata flag | `true` found |
| `value` | right-side expression token array | static tokens, workflow-variable tokens, operator tokens, and function tokens found |

Value expression examples found:

- fixed string token, such as a text literal
- fixed number token
- concatenation with `&`
- workflow variable token, such as `Workflow Variables:RequestType`
- function tokens, including `iif` and `isNullOrEmpty`

## Another Approval Workflow Variables

`formtype="custom"` targets another approval form workflow instance. The export proves another-workflow variable setting from both approval and data-list workflow hosts.

Target metadata shape:

| Field/path | Export finding | Proof |
|---|---|---|
| `properties.data.AppID` | selected application id | export-proven, redacted |
| `properties.data.ListSetID` | selected application/listset id | export-proven, redacted |
| `properties.data.ProcKey` | target approval form key, `ABC-ANO` in this export | export-proven |
| `properties.formids` | target submitted approval request/workflow instance form id | export-proven, redacted |
| `properties.variablesetting[]` | target variables in the other approval workflow | export-proven |

The target approval form is `Another approval from`. The export also includes `Another approval form report`, whose fields include a `Form ID` field and the target approval-form variables. The report is useful for locating submitted target approval request form IDs, but this export does not prove runtime lookup or mutation behavior.

Current limitation to preserve: based on the user-provided product understanding and this export, another-workflow Set variable should be treated as another approval workflow only. Do not claim data-list or scheduled workflow variable updates through `formtype="custom"` until a focused export or runtime baseline proves it.

## Data-List Workflow Findings

The data-list workflow Set variable nodes still target workflow variables, not list fields.

| Capability | Export finding | Proof |
|---|---|---|
| Set current workflow variables | data-list `formtype="current"` node sets workflow variables | export-proven |
| Set another approval workflow variables | data-list `formtype="custom"` node targets `ABC-ANO` | export-proven |
| Use list field values as right-side expressions | `exprType="list_field"` tokens found for Start / Issue Date and Category / Type values | export-proven |
| Set data-list fields directly | no Set variable node targets a data-list field | not found |

Generation rule: in data-list workflows, use Set variable for workflow variables. Use Set data list / `ContentList` when the intent is to add, update, or delete data-list records or fields.

## Set Variable vs Set Data List

| Dimension | Set variable | Set data list |
|---|---|---|
| Front-end action | Set variable | Set data list |
| Internal type | `SetVariableTask` | `ContentList` |
| Primary purpose | Update workflow variables | Add, update, or delete data-list/document-library records |
| Left side | workflow variable | data-list field/record mapping |
| Right side | expression editor value | expression editor value for selected list fields |
| Data-list workflow behavior | can read list fields as expression values, but still targets workflow variables | writes data-list fields/records |
| Proof | export-proven in this study | export-proven in `Workflow Actions Runtime Baseline (5)_Set data list.yap`; not runtime-proven |

`Workflow Actions Runtime Baseline (5)_Set data list.yap` proves the Set data list side of this distinction. Front-end Set data list serializes as `ContentList`, uses `properties.listtype` for current/selected data source mode, uses `properties.type` for `add`/`edit`/`remove`, uses `properties.listdatas[]` for target field mappings, and uses `properties.wheres[]` for update/delete filters. Data-list workflows can use list fields as right-side values in both Set variable and Set data list, but only Set data list targets list fields/records.

Generation rule update: when a data-list workflow must mutate the current item/list fields, use Set data list `listtype="current"` if the current-list context is export-proven for that generated host. When it must mutate another list or future document library target, use Set data list `listtype="select"` with selected source metadata. Keep Set variable reserved for workflow variable mutation.

## Node Configuration Reference

`node-configurations.json` maps Set variable to `SetVariableTask` and lists these useful paths:

- `properties.formtype`
- `properties.data.AppID`
- `properties.data.ListSetID`
- `properties.data.ProcKey`
- `properties.formids`
- `properties.variablesetting`

This reference is useful for expected path validation, but it is not runtime proof. The export remains the source of truth for concrete field names and value shapes.

## Help Center Comparison

| Concept | Product behavior | Export-proven field/shape | Config path | Match | Proof |
|---|---|---|---|---|---|
| Set selected workflow variables | Set Variable action configures workflow variable values. | `SetVariableTask.properties.variablesetting[]` | `properties.variablesetting` | matched | product-documented + export-proven |
| Current approval/workflow target | Product UI lets users select current approval form/workflow context. | `formtype="current"` | `properties.formtype` | matched | product-documented + export-proven |
| Another approval form target | Product UI supports another approval form with target app/form and submitted form id. | `formtype="custom"`, target `data`, `formids` | `properties.data.*`, `properties.formids` | matched | product-documented + export-proven |
| Static or dynamic values | Right-side value uses expression editor. | `variablesetting[].value` expression-token arrays | `properties.variablesetting` | matched | product-documented + export-proven |
| Multiple variables in one action | Product understanding says one action can set multiple variables. | nodes with 3 and 4 `variablesetting[]` entries found | `properties.variablesetting` | matched | export-proven |
| Data-list field values as expression values | Data-list workflow can use list fields as expression values. | `exprType="list_field"` tokens found in data-list workflow values | not specific in SetVariableTask config | matched | export-proven |
| Data-list field writes | Use Set data list for record/field updates. | No Set variable node targets list fields; Set Data List article documents record updates. | `ContentList` paths | matched by absence/product docs | product-documented + export-proven absence |

## Validator Recommendations

- Warning-first: validate `SetVariableTask`.
- Warn when `properties.formtype` is not `current` or `custom`.
- Warn when `properties.variablesetting` is missing, empty, or not an array.
- Warn when assignments are missing `idx`, `id`, `name`, `type`, or `value`.
- Validate `variablesetting[].value` as an expression-token array.
- Warn on unknown variable types rather than failing compatibility mode.
- Warn when `formtype="custom"` lacks `properties.data.AppID`, `ListSetID`, `ProcKey`, or `formids`.
- Warn that `exprType="list_field"` values in data-list workflows are export-proven as right-side expressions but runtime mutation is not proven.
- Do not hard-error compatibility exports for another-workflow targets until runtime behavior is proven.

## Generation Rules

- Use `SetVariableTask` for front-end Set variable only when export-proven.
- Preserve `formtype="current"` for current workflow variables.
- Preserve `formtype="custom"` for another approval workflow instance variables.
- One Set variable node may contain one or multiple rows in `properties.variablesetting[]`.
- Treat `variablesetting[].id` as the target workflow variable, not a data-list field.
- Treat `variablesetting[].value` as the expression-editor right side.
- In data-list workflows, list fields may appear as `exprType="list_field"` value tokens.
- Do not use Set variable to write data-list fields; generate Set data list / `ContentList` for list-field mutation.
- Do not claim variable mutation, another-workflow update, or submitted-form targeting behavior without a focused runtime baseline.

## Runtime Test Plan Addendum

Recommended future runtime baseline:

- designer/open proof for current-workflow Set variable in approval form workflow
- designer/open proof for multiple variables in one Set variable action
- designer/open proof for another approval workflow target metadata and form id setting
- designer/open proof for data-list workflow values sourced from list fields
- optional execution proof only with safe submitted requests and no unintended data mutation

Until then, this study remains export-proven/config-reference-backed/validator-backed only.

## Runtime Baseline Update

The combined `Workflow Actions Batch Runtime Baseline` imported and opened successfully. The approval workflow designer rendered `SetVariableTask` nodes for current-workflow variable settings and another-approval-workflow target settings, and the approval workflow published successfully. The data-list workflow designer rendered a `SetVariableTask` using current-workflow variable settings with list-context value expressions in the generated package, and the data-list workflow published successfully.

This upgrades the generated Set variable shapes to import/open/designer/publish-proven only. Variable mutation, another approval workflow instance mutation, submitted form ID runtime resolution, and any list-field write behavior remain not runtime-proven.

---

<a id="workflow-signal-event-action"></a>

<!-- source: docs/studies/collection-03.md#workflow-signal-event-action -->

# Workflow Signal Event Action Study

Proof boundary: this is export-proven, product-documented, config-reference-backed, and validator-backed learning only. No workflow was imported, published, triggered, submitted, recalled, terminated, or executed in this pass. No downstream Set data list cleanup action was executed and no email was sent.

## Sources

| Source | Role | Proof label |
|---|---|---|
| `<downloads>/Workflow Actions Runtime Baseline (6)_Signal event.yap` | Export/schema source of truth | export-proven |
| `<downloads>/node-configurations.json` | Node configuration path/control-type reference | config-reference-backed |
| Yeeflow Help Center: Signal Event Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Product behavior and terminology | product-documented |
| Yeeflow Help Center: Start Action, https://<yourdomain>.yeeflow.com/<redacted-path> | Start terminate/recall relationship | product-documented |
| Yeeflow Help Center: Manage your requests, https://<yourdomain>.yeeflow.com/<redacted-path> | Request terminate/recall behavior | product-documented |
| Existing Start action and Set data list studies | Comparison/reference | export-proven / validator-backed |

Raw exports, decoded payloads, private IDs, user names, emails, tenant IDs, credentials, and raw API responses are not committed.

## Summary

The export contains one Signal event node. It serializes as `SignalEvent`, appears only in the approval form workflow `Workflow Action Approval Test`, has no incoming flow, has one outgoing flow, and listens for both cancel/terminate and revoke/recall event definitions.

| Workflow host | WorkflowType | `SignalEvent` count | Event definitions found | Downstream first action | Proof |
|---|---:|---:|---|---|---|
| Approval form workflow | `2` | 1 | `RevokeEventDefinition`, `CancelEventDefinition` | `ContentList` edit | export-proven |
| Data-list workflow | `1` | 0 | none | none | export-proven absence in this export |
| Scheduled workflow | `3` | 0 | none | none | export-proven absence in this export |

## Signal Event Inventory

| Field / behavior | Export finding | Proof |
|---|---|---|
| Front-end action | Signal event | product-documented + export-proven mapping |
| Internal type | `SignalEvent` | export-proven + config-reference-backed |
| Host workflow | Approval form workflow, `WorkflowType = 2` | export-proven |
| Node label | `Signal event` | export-proven |
| Incoming flow count | `0` | export-proven |
| Outgoing flow count | `1` | export-proven |
| Event definition path | `properties.eventdefinitions` | export-proven + config-reference-backed |
| Revoke/recall event | `RevokeEventDefinition` | export-proven |
| Cancel/terminate event | `CancelEventDefinition` | export-proven |
| Downstream first action | `ContentList` named `Set Data List_6` | export-proven |

## No-Incoming Graph Behavior

Signal event is a special event source, not a normal action reached through the primary Start branch.

| Graph rule | Export finding | Validation guidance |
|---|---|---|
| Start action has no incoming flow | `StartNoneEvent` still has no incoming flow | Existing Start rule remains. |
| Signal event has no incoming flow | `SignalEvent.incoming[]` is empty and no `SequenceFlow` targets it | Allow `SignalEvent` as a no-incoming event-source component. |
| Signal event has outgoing flow | one `SequenceFlow` leaves the Signal event | Warn if missing because no compensation branch can run. |
| Signal event can coexist with Start | approval workflow contains both `StartNoneEvent` and `SignalEvent` | Graph connectivity validation should treat Start and Signal event as valid component roots. |
| Normal action nodes | downstream `ContentList` is reached from Signal event | Normal actions still need valid sequence references in their branch. |

## Event Definition Config

The export stores selected events as an array:

```json
{
  "properties": {
    "eventdefinitions": [
      "RevokeEventDefinition",
      "CancelEventDefinition"
    ]
  }
}
```

`node-configurations.json` also maps Signal event to `config_SignalEvent` with `properties.eventdefinitions` and the same enum values. The config reference says at least one event should be selected. Treat unknown values as warnings unless an import-breaking failure is proven.

## Relation To Start Action

| Concept | Start action | Signal event | Proof |
|---|---|---|---|
| Internal type | `StartNoneEvent` | `SignalEvent` | export-proven |
| Primary purpose | Starts the normal workflow after submission | Starts an event branch after cancel/revoke event | product-documented + export-proven |
| Incoming flow | none | none | export-proven |
| Terminate/recall settings | `terminate`, `terminate-conditions`, `revoke-conditions` on approval Start when present | `eventdefinitions[]` chooses cancel/revoke listeners | export-proven |
| Relationship | Start controls whether submitter can terminate/recall | Signal event listens for those events and runs follow-up actions | product-documented; runtime not tested |

The studied approval Start action includes `terminate: false`, `terminate-conditions: null`, and one `revoke-conditions[]` row. The Signal event listens for both `RevokeEventDefinition` and `CancelEventDefinition`. Validators should warn if a Signal event listens for cancel/revoke but the approval Start settings make that event unavailable when this can be inspected, because such a branch may never trigger. This is a design warning, not runtime proof.

## Downstream Cleanup Pattern

The Signal event flows to a Set data list / `ContentList` edit action.

| Downstream action | Export shape | Safety note | Proof |
|---|---|---|---|
| Action type | `ContentList` | Reuses Set data list schema. | export-proven |
| Target mode | `listtype="select"` | Selected data-list target IDs are redacted. | export-proven |
| Operation | `type="edit"` | Mutating action; do not execute in learning pass. | export-proven |
| Mapping | one `listdatas[]` row updates `Decimal1` with `Per="2"` | Numeric operation code `2` was learned as Decrease in Set data list study. | export-proven schema |
| Filter | one `wheres[]` row filters by `ListDataID` using a workflow variable | Nonempty filter reduces broad-update risk; execution remains untested. | export-proven |

This supports the compensation/cleanup pattern: a recall/terminate event can run a follow-up branch to release or reverse previously created/updated data. The sample uses Set data list edit, but record mutation and numeric operation execution are not runtime-proven here.

## Help Center Comparison

| Concept | Product behavior | Export/config field | Match | Proof |
|---|---|---|---|---|
| Signal event handles cancelled/recalled workflow events | Help Center describes cancelled/recalled triggers | `properties.eventdefinitions[]` | matched | product-documented + export-proven |
| At least one event must be selected | Help Center and config reference say one or both events | two array entries found | matched | product-documented + config-reference-backed + export-proven |
| Supported follow-up actions | Help Center lists Assignment/Claim Task, Send Email, Set Variable, Set Content List, HTTP Request | export shows Set data list / `ContentList` | partial; one action type found | product-documented + export-proven |
| End action not needed after Signal event branch | Help Center says end actions do not take effect on trigger event | studied branch contains no end after the Signal event branch | matched in this export; runtime not tested | product-documented + export-proven |
| Start terminate/recall behavior | Start docs and request-management docs distinguish terminate vs recall | approval Start fields plus Signal event eventdefinitions | related but not executed | product-documented + export-proven |

## Generation Rules

- Use `SignalEvent` only for Signal event actions when export-proven.
- Treat Signal event as an event source in approval workflows; do not require an incoming flow.
- Preserve at least one outgoing flow to a compensation/follow-up branch.
- Preserve `properties.eventdefinitions[]` and use only export/config-backed values until another export proves more:
  - `CancelEventDefinition`
  - `RevokeEventDefinition`
- Use Signal event for recall/terminate compensation patterns, such as updating or releasing records created earlier in the workflow.
- Validate downstream Set data list edit/remove filters carefully; broad update/delete after recall/terminate is high risk.
- Do not use Signal event in data-list or scheduled workflows unless an export or focused runtime proof demonstrates that host.
- Do not claim recall/terminate execution, compensation execution, Set data list mutation, or email behavior without a focused runtime baseline.

## Validator Recommendations

- Warning-first: validate `SignalEvent`.
- Warn when `properties.eventdefinitions` is missing, not an array, empty, or contains unknown values.
- Warn when a Signal event has incoming flows.
- Warn when a Signal event has no outgoing flow.
- Allow Signal event as a no-incoming workflow graph component root, similar to Start action.
- Warn if Signal event appears outside approval-form workflows unless a future export proves that host.
- Warn if Signal event listens for cancel/revoke but Start terminate/revoke configuration appears unavailable.
- If downstream Set data list edit/remove actions are present, reuse Set data list broad-filter checks.

## Runtime Boundary

This pass does not import, publish, submit, recall, terminate, trigger, or execute the workflow. It does not prove that Signal event branches run at runtime, that `CancelEventDefinition` or `RevokeEventDefinition` fire, that downstream actions execute, or that data-list updates occur.

Recommended next step: merge this export-learning branch if review is clean, then run a focused combined runtime baseline for Claim Task + Set variable + Set data list + Signal event designer/open/publish proof. Recall/terminate execution and cleanup mutation should remain deferred until explicitly scoped with disposable requests and safe target records.

## Runtime Baseline Update

The combined `Workflow Actions Batch Runtime Baseline` imported and opened successfully. The approval workflow designer rendered a `SignalEvent` with no incoming flow and one outgoing flow to a downstream `ContentList` cleanup branch. The Signal event panel opened and showed both Canceled and Recalled selected. The approval workflow published successfully.

This upgrades the generated Signal event shape to import/open/designer/publish-proven only. Recall or terminate triggering, event firing, downstream cleanup mutation, and any email or task behavior remain not runtime-proven.

---

<a id="workflow-start-action-settings"></a>

<!-- source: docs/studies/collection-03.md#workflow-start-action-settings -->

# Workflow Start Action Settings

## Purpose

This study documents Start action settings found in `Test ABC (3).yap`, focused on allow terminate, allow recall, condition storage, and Start email notification configuration.

Proof boundary:

- `export-proven`: found in `Test ABC (3).yap`.
- `product-documented`: described in Yeeflow Help Center.
- `validator-backed`: checked by local validators after this study.
- `runtime-proven`: not claimed here.

No workflow was submitted, no request was recalled or terminated, and no email delivery was tested.

## Sources

Source export:

```text
<downloads>/Test ABC (3).yap
```

Comparison exports:

```text
<downloads>/Test ABC.yap
<downloads>/Test ABC (1).yap
<downloads>/Test ABC (2).yap
```

Reference:

- Yeeflow Help Center: Start Action
  https://<yourdomain>.yeeflow.com/<redacted-path>

## Export Inventory

`Test ABC (3).yap` decoded successfully with large numeric IDs preserved as strings.

| Item | Count / value | Proof |
|---|---:|---|
| Approval forms | 1 | export-proven |
| Workflow type | 2 | export-proven |
| Workflow shapes | 38 | export-proven |
| Start nodes | 1 | export-proven |
| Assignment Task nodes | 12 | export-proven |
| Start incoming flows | 0 | export-proven |
| Start outgoing flows | 12 | export-proven |

## Start Action Fields

The Start node is stored as a `StartNoneEvent` shape. Settings are in `StartNoneEvent.properties`.

| Field | Observed shape | Notes | Proof |
|---|---|---|---|
| `name` | `Start` | Display label. | export-proven |
| `isenabledemail` | `true` | Enables start/submission notification configuration. | export-proven |
| `to` | expression-button rich text | Contains applicant and submitter email expressions; redacted in reusable docs. | export-proven |
| `subject` | expression-button rich text | References workflow/instance name. | export-proven |
| `html` | expression-button HTML body | References applicant, workflow name, form number, and form URL. | export-proven |
| `taskurl` | UUID-like page reference | Redact as `<REDACTED_TASK_FORM_PAGE_ID>`. | export-proven |
| `terminate` | `false` | Allow Terminate toggle value. | export-proven |
| `terminate-conditions` | `null` | No terminate condition rows configured in the source export. | export-proven |
| `revoke-conditions` | condition array | Allow Recall condition rows. | export-proven |

## Allow Terminate

The export uses:

```json
{
  "terminate": false,
  "terminate-conditions": null
}
```

The Help Center describes Allow Terminate as a submitter-facing option that can be controlled by a condition editor. In this export, the field is present and disabled. Runtime terminate behavior is not tested.

## Allow Recall

The export uses `revoke-conditions` to store the recall condition. One condition row was observed.

Redacted normalized shape:

```json
{
  "revoke-conditions": [
    {
      "key": "<REDACTED_CONDITION_ID>",
      "pre": "and",
      "left": {
        "type": 1,
        "value": {
          "exprType": "variable",
          "valueType": "text",
          "id": "RequestNo",
          "type": "expr"
        }
      },
      "op": "s.!=",
      "right": {
        "type": 2,
        "value": [
          {
            "type": "str",
            "value": "<REDACTED_CONDITION_LITERAL>"
          }
        ]
      },
      "group": "string"
    }
  ]
}
```

This matches the newer operand-wrapper condition style: left operand uses `type: 1` direct selector; right operand uses `type: 2` expression editor tokens. Runtime recall behavior is not tested.

## Start Email Notification

Start email settings are configured on the Start node itself.

| Concept | Export shape | Notes | Proof |
|---|---|---|---|
| Enabled | `isenabledemail: true` | Enables submission/start notification. | export-proven |
| Recipients | `to` expression buttons | Applicant email and submitter email labels were observed; values redacted. | export-proven |
| Subject | `subject` expression-button rich text | Includes workflow name token. | export-proven |
| Body | `html` rich text | Includes applicant name, workflow name, form number, and form URL tokens. | export-proven |

Email delivery was not tested and must not be claimed.

## Help Center Comparison

| Help Center concept | Export-proven field/shape | Match status | Notes | Proof level |
|---|---|---|---|---|
| Start action has no incoming connector | `incoming: []` | matched | Export has zero incoming flows. | product-documented + export-proven |
| Start action initiates workflow | `StartNoneEvent` with 12 outgoing flows | matched | Runtime submit not tested. | product-documented + export-proven |
| Allow Terminate | `terminate`, `terminate-conditions` | matched | Export shows disabled terminate plus null conditions. | product-documented + export-proven |
| Allow Recall | `revoke-conditions` | partially matched | Export proves condition storage, but no explicit recall boolean was found. | product-documented + export-proven |
| Condition Editor | condition row with operand wrappers | matched | Shape aligns with existing workflow condition learning. | export-proven + validator-backed |
| Enable Email | `isenabledemail`, `to`, `subject`, `html` | matched | Delivery not tested. | product-documented + export-proven |

## Generation Rules

- Preserve Start node `incoming: []` and at least one outgoing sequence flow.
- Preserve `terminate` and `terminate-conditions` together when found.
- Preserve `revoke-conditions` as condition rows; do not convert to legacy HTML condition strings.
- Preserve `isenabledemail`, `to`, `subject`, and `html` together for Start notification.
- Do not generate real fixed email recipients in reusable packages.
- Do not claim terminate, recall, condition gating, or email delivery behavior without a focused runtime pass.

## Validator Recommendations

Validators should warn, not hard-error in compatibility mode, for:

- Start nodes with incoming flows
- Start nodes without outgoing flows
- non-boolean `terminate`
- non-array/non-null `terminate-conditions`
- non-array/non-null `revoke-conditions`
- email-enabled Start nodes missing `to`, `subject`, or `html`

## Known Gaps

- No enabled terminate example was found.
- No explicit recall boolean separate from `revoke-conditions` was found.
- Start condition runtime gating was not tested.
- Start email delivery was not tested.

## Signal Event Relationship

`Workflow Actions Runtime Baseline (6)_Signal event.yap` adds a Signal event comparison point for approval-form workflows.

| Concept | Approval Start action | Signal event | Proof |
|---|---|---|---|
| Internal type | `StartNoneEvent` | `SignalEvent` | export-proven |
| Incoming flow | none | none | export-proven |
| Outgoing flow | normal workflow branch | event compensation branch | export-proven |
| Terminate/recall storage | `terminate`, `terminate-conditions`, `revoke-conditions` | `properties.eventdefinitions[]` | export-proven |
| Cancel/terminate event | Start controls whether terminate is available | Signal listens with `CancelEventDefinition` | product-documented + export-proven config |
| Recall/revoke event | Start controls recall conditions | Signal listens with `RevokeEventDefinition` | product-documented + export-proven config |
| Downstream cleanup | not a Start concern | may connect to `ContentList`, Set Variable, task, email, or HTTP branches | product-documented; `ContentList` edit export-proven |

Generation rule: use Start action settings to control submitter-facing terminate/recall availability, and use Signal event only as a separate event source branch for recall/terminate follow-up logic. Warn if a Signal event listens for an event that the Start settings appear to make unavailable. Do not claim recall/terminate execution or downstream cleanup execution without focused runtime proof.

Normalized references:

- `docs/studies/normalized/workflow-signal-event.bundle.json#signal-event-basic.normalized.json`
- `docs/studies/normalized/workflow-signal-event.bundle.json#signal-event-basic.normalized.json`
- `docs/studies/normalized/workflow-signal-event.bundle.json#signal-event-basic.normalized.json`

## Data-List Workflow Start Action

`Purchase Requests.ydl` adds a data-list workflow Start action comparison point.

| Concept | Approval form workflow from `Test ABC (3).yap` | Data-list workflow from `Purchase Requests.ydl` | Proof |
|---|---|---|---|
| Start stencil | `StartNoneEvent` | `StartNoneEvent` | export-proven |
| Incoming flow | none | none | export-proven |
| Outgoing flow | one or more outgoing sequence flows | one outgoing sequence flow | export-proven |
| Allow terminate | `terminate` and `terminate-conditions` fields found | fields absent | export-proven difference |
| Allow recall | `revoke-conditions` field found | field absent | export-proven difference |
| Email notification | `isenabledemail`, `to`, `subject`, `html` | same field family present | product-documented + export-proven |
| Trigger context | approval request/form submission | data-list `FlowMappings[]` new-item trigger | export-proven difference |

Generation rule: keep data-list workflow Start action settings separate from approval form workflow Start settings. Do not add terminate or recall fields to a data-list workflow Start action unless a focused data-list export proves those fields. Start notification delivery remains untested.

Normalized references:

- `docs/studies/normalized/workflow-start-action.bundle.json#start-action-data-list-workflow.normalized.json`
- `docs/studies/normalized/workflow-start-action.bundle.json#start-action-data-list-email-notification.normalized.json`

## Scheduled Workflow Start Action

`Workflow Actions Runtime Baseline (1).yap` adds a Scheduled Workflow Start action comparison point.

| Concept | Approval form workflow from `Test ABC (3).yap` | Data-list workflow from `Purchase Requests.ydl` | Scheduled Workflow from `Workflow Actions Runtime Baseline (1).yap` | Proof |
|---|---|---|---|---|
| Workflow type | `WorkflowType = 2` | `WorkflowType = 1` | `WorkflowType = 3`, `ListID = 0` | export-proven |
| Start stencil | `StartNoneEvent` | `StartNoneEvent` | `StartNoneEvent` | export-proven |
| Incoming flow | none | none | none | export-proven |
| Outgoing flow | one or more | one | one | export-proven |
| Allow terminate | `terminate` and `terminate-conditions` fields found | fields absent | fields absent | export-proven difference |
| Allow recall | `revoke-conditions` field found | field absent | field absent | export-proven difference |
| Email notification | `isenabledemail`, `to`, `subject`, `html` | same field family present | same field family present | product-documented + export-proven |
| Expression context | applicant/request context | list-item context can appear elsewhere in data-list workflow | applicant/application-style expressions in Start email | export-proven |

Generation rule: for Scheduled Workflow Start actions, preserve `StartNoneEvent` graph behavior and email fields when present, but do not add approval-form terminate/recall fields unless another Scheduled Workflow export proves them. Do not execute or trigger Scheduled Workflows during export-learning passes.

Normalized reference:

- `docs/studies/normalized/workflow-start-action.bundle.json#start-action-scheduled-workflow.normalized.json`

---

<a id="yap-schema-standard"></a>

<!-- source: docs/studies/collection-03.md#yap-schema-standard -->

# Yeeflow YAP Schema Standard

Proof level: product-schema-backed, product-rule-backed, validator-backed after the checks in this branch. This is not runtime-import-proven.

## Sources

- Product schema reference: `<downloads>/yap-schema.json`
- Latest product schema reference: `<downloads>/yap-v1-schema_v2.json`
- Product rules reference: `<downloads>/Yeeflow App Creation Rules.md`
- Requested `/mnt/data/...` attachment paths were not present in this desktop workspace; the same provided files were available from `<downloads>/`.

## Wrapper

The top-level `.yap` wrapper is a `ListTemplateDownloadResponse` object. The schema requires:

- `Title`
- `Description`
- `IconUrl`
- `IsListSet`
- `Resource`

`Resource` must be a string starting with `[______gizp______]`. The suffix is base64 gzip text that decodes to the resource object.

Hard validation errors:

- missing required wrapper property
- `Resource` not a string
- `Resource` missing the `[______gizp______]` prefix
- `Resource` cannot be base64/gzip decoded
- decoded `Resource` is not valid JSON
- decoded `Resource.Data` is missing or not parseable JSON

## Decoded Resource

Decoded `Resource.Data` is a JSON string containing `ListExportInfo`.

`ListExportInfo.Item` is required. `Childs`, `Forms`, `FormReports`, `DataReports`, `FormNewReports`, `OtherModules`, and app metadata arrays must be arrays when present. The schema uses `additionalProperties: false` in several places, but that should not be enforced globally yet because the schema is a standard reference and existing valid exports may contain product fields that are not fully covered. Unknown properties are warning-level unless product team marks them import-breaking.

## ListExportItem

`ListExportItem` requires:

- `Defs`
- `Layouts`

Both must be arrays. Empty arrays are valid.

Product-team import failure feedback confirmed:

- `Item.Defs: null` is invalid.
- `Item.Layouts: null` is invalid.
- Use `[]` when there are no definitions or layouts.

Hard validation errors:

- missing `Item`
- missing `Defs`
- missing `Layouts`
- `Defs === null`
- `Layouts === null`
- `Defs` not array
- `Layouts` not array
- same rules for every `Childs[]` resource

## CustomListModel

`yap-v1-schema_v2.json` adds schema-backed `CustomListModel` constraints for list-like resources:

- `Flags` is fixed to `1`.
- `Status` is fixed to `1` when present. The v2 schema defines the fixed value, but the field is not marked required; known imported packages may omit it.
- `Type` is limited to:
  - `1` List
  - `16` Document
  - `32` WorkflowReport
  - `64` DataReport
  - `128` EPUser
  - `1024` ListSet

Business Travel runtime practice made `ListModel.Flags = 1` import-sensitive: the package imported after root and child list resources were repaired to include `Flags = 1`. Validators should hard-fail generated packages when `ListModel.Flags` is missing or not `1`.

Follow-up user testing proved the same fixed Business Travel package imports, opens, opens the `Business Travel Request` workflow, and publishes successfully after schema, variable-reference, and publish-safe assignment repairs. That proof is package-specific; workflow execution, request submission, routing, data mutation, and true Finance Manager position assignment remain unproven.

Validation severity:

- `Flags` missing or not `1`: hard error for generated packages.
- `Status` present and not `1`: hard error.
- `Status` missing: warning or accepted, because schema v2 fixes the value but does not require the property and an imported Business Travel package omitted it.
- invalid `Type`: hard error.

## ListDefinitionModel

Schema-backed and app-creation rules remain mandatory for generated list fields:

- `DisplayName`, `FieldName`, and `InternalName` are unique within the same list.
- `DisplayName`, `FieldName`, and `InternalName` are at most 255 characters.
- `InternalName` matches `[a-zA-Z0-9_]`.
- `FieldName` ends with a numeric suffix matching `FieldIndex` for generated non-system fields.
- Supported schema field `Type` values are the 28 product-rule field types.

Compatibility note: the Data List field export-learning milestone found `metadata` as a single metadata field type. Keep `metadata` accepted as export-proven even though the schema/rules 28-type list names `mutiple-metadata`.

## Forms And NoRule

`ProcessFormInfo.Key` must match `[a-zA-Z0-9_]` and stay within 255 characters.

`ProcessFormInfo.NoRule` uses the process number format object:

```json
{
  "Prefix": "test_{date}_{index}",
  "StartIndex": 1,
  "CustomLength": 8,
  "AutoIncrement": 1
}
```

`ProcessFormNoRule` requires `Prefix`, `StartIndex`, `CustomLength`, and `AutoIncrement`. `Prefix` must include `{index}`.

Hard validation errors:

- malformed or missing required `NoRule` where approval/process form generation requires it
- missing `{index}` in `NoRule.Prefix`
- invalid process key characters

## OtherModules

Schema-backed `OtherModules[]` entries have:

- `Type`
- `Data`

Known types are `Connections`, `Agents`, and `Knowledges`. `Data` should be an array for these modules.

## App Agent/Copilot Resource Permissions

Access app resources is represented by agent/copilot components where:

- component `Type = 2`
- component `SubType = 10`
- `Settings` is JSON
- `Settings.resources` or `Settings.resources.permissions` contains permission groups

Schema-backed bitwise masks:

- `approvalForms`: Submit `1`, ReadTasks `16`, ProcessTasks `32`
- `dataLists`: Submit `1`, Edit `2`, Delete `4`, Read `8`
- `documentLibraries`: Submit `1`, Edit `2`, Delete `4`, Read `8`
- `aiAgents`: Submit `1`

Hard validation errors are appropriate when these groups contain non-integer permissions or bits outside the allowed mask.

Open product conflict:

- `yap-schema.json` says `formReports` and `dataReports` allow `0` or Read `8`.
- Updated `Yeeflow App Creation Rules.md` says `formReports` and `dataReports` allow Submit `1`.

Until product team resolves this conflict, validators should warn on the mismatch for `formReports` and `dataReports`, not hard-fail either `1` or `8`.

## Generator Rules

Generators that emit `.yap`, `.ydl`, or `.ywf` packages must:

- emit `Item.Defs` and `Item.Layouts` as arrays, never `null`
- emit child resource `Defs` and `Layouts` as arrays, never `null`
- emit root and child `ListModel.Flags = 1`
- emit `ListModel.Status = 1` when they choose to include `Status`
- keep `ListModel.Type` within the v2 schema allowed set
- preserve the FieldIndex/FieldName suffix synchronization gate
- keep list identifiers unique and valid
- emit valid process keys
- emit `NoRule` as the required object when approval/process forms need numbering
- include `{index}` in `NoRule.Prefix`
- keep app resource permission flags within schema-backed bitwise masks
- pass schema-standard validation before import attempts

## Proof Boundary

This branch is product-schema-backed and product-rule-backed. Validator and generator guidance is hardened. Runtime import/open behavior is not proven by this pass and requires a focused import test later, especially for a package that previously failed with `Item.Defs: null`.

---

<a id="yeeflow-app-creation-rules-runtime-proof"></a>

<!-- source: docs/studies/collection-03.md#yeeflow-app-creation-rules-runtime-proof -->

# Yeeflow App Creation Rules Runtime Proof

Milestone: focused runtime import test for fixed workflow field-rule package

Date: 2026-05-25

## Scope

This pass tested the fixed workflow-actions package created after the product-team app creation rules update. It targets the data-list field generation issue where a generated field used a mismatched `FieldIndex` and `FieldName` numeric suffix, causing Yeeflow to reject manually added fields with:

> The value for the field already exists, please enter a new value.

Form Report runtime testing is explicitly out of scope for this pass.

## Package

- Repo package path: `workflow-actions-runtime-baseline-field-rules-fixed.v1.yap`
- Import package copied to: `<downloads>/workflow-actions-runtime-baseline-field-rules-fixed.v1.yap`
- Imported runtime app name used for this proof: `Workflow Actions Field. RulesProof`
- Target data list: `Purchase Requests Runtime Test`

The generated `.yap` package is a local runtime artifact and must remain ignored/uncommitted.

## Local Validation

- `scripts/inspect-app-creation-rules.mjs`: pass, 2 lists, 2 forms, 0 errors, 0 warnings.
- `validate-yap-package.js --mode generator --stage final`: pass with warnings, 0 errors, 19 warnings.
- `validate-yap-graph.js --mode generator --stage final`: pass with warnings, 0 errors, 14 warnings.
- `scripts/inspect-yap-materialization.mjs`: non-target app-shell finding `NO_DASHBOARD_NAV`.

No `FIELD_NAME_FIELDINDEX_MISMATCH`, malformed or missing `NoRule`, or invalid process-key errors were reported by the focused app-creation/package/graph validators.

## Runtime Result

- Import: succeeded.
- App open: succeeded.
- Approval form open: succeeded on `Workflow Action Approval Test`.
- Data list open: succeeded on `Purchase Requests Runtime Test`.
- Manual field creation panel: opened from `+ New column`.
- Manual field save: succeeded for a new single-line field named `Field Rules Runtime Proof`.
- Previous duplicate-value error: not observed.
- UI success message: `Added Successfully`.

## Proof Boundary

Runtime-proven in this pass:

- Fixed package imports into Yeeflow.
- Imported app opens.
- Included approval form opens.
- Target data list opens.
- A new manually added list field can be saved without the previous duplicate-value error.

Validator-backed:

- FieldIndex/FieldName suffix synchronization.
- Unique `DisplayName`, `FieldName`, and `InternalName` within generated lists.
- Valid `InternalName` and process-key character sets.
- Valid approval-form `NoRule` object shape with `{index}` in `Prefix`.

Not proven in this pass:

- Workflow routing, task assignment, approval submission, email delivery, data-list workflow execution, or record mutation behavior.
- Form Report import/open/designer behavior.
- Form Report field-source resolution.

## Guidance

Treat the app creation rules from `docs/studies/collection-03.md#yeeflow-app-creation-rules` as generation-blocking for new packages. A generated workflow/data-list package that fails these checks must not be imported. This focused runtime proof upgrades the fixed workflow field-rule package from local-generation-fixed to runtime-import-proven and data-list-field-creation-proven, but only for the field creation path tested here.

---

<a id="yeeflow-app-creation-rules"></a>

<!-- source: docs/studies/collection-03.md#yeeflow-app-creation-rules -->

# Yeeflow App Creation Rules

Source: product-team rules document, `<downloads>/Yeeflow App Creation Rules.md`.

Note: the user-provided `/mnt/data/Yeeflow App Creation Rules.md` path was not present in this local workspace; the same named Downloads file was readable and used as the product-rule source. The original reference document was not modified.

Proof labels used here:

- `product-rule-backed`: directly from the product-team rules document or product-team issue feedback.
- `validator-backed`: implemented in local validators or the focused inspector.
- `local-generation-fixed`: generated package source was patched and local validation can prove the generated shape.
- `not runtime-import-proven`: no broad runtime import/open test was run in this pass.

## Supported List Field Types

`product-rule-backed`

The supported list field `Type` values are:

| Category | Type values |
| --- | --- |
| Text and input | `input`, `textarea`, `richtext`, `hyperlink` |
| Numeric and financial | `input_number`, `currency`, `percent`, `calculated-column`, `rate` |
| Selection and choice | `switch`, `checkbox`, `radio`, `select`, `tag` |
| Date and time | `datepicker`, `time` |
| Identity and organization | `identity-picker`, `organization-picker`, `cost-center-picker`, `signer` |
| Uploads and media | `file-upload`, `icon-upload` |
| Advanced and system | `lookup`, `mutiple-metadata`, `location-picker`, `flowstatus`, `autonumber`, `list` |

Unknown `Type` values are warning-first because older export evidence can contain uncommon or product-internal controls.

## Primitive Length Constraints

`product-rule-backed`

Underlying primitive field type constraints:

| Primitive `FieldType` | Length range |
| --- | --- |
| `Text` | 1-300 characters |
| `Bit` | 1-50 characters |
| `Decimal` | 1-200 characters |
| `Datetime` | 1-200 characters |

These are product rules. This pass did not add broad value-length hard failures because generated packages usually define schemas, not user-entered values. Identifier limits below are enforced directly.

## Identifier Rules

`product-rule-backed`, `validator-backed`

Within a single list:

- `DisplayName` must be unique.
- `FieldName` must be unique.
- `InternalName` must be unique.
- `InternalName` may contain only letters, numbers, and underscores: `[a-zA-Z0-9_]`.
- `DisplayName`, `FieldName`, and `InternalName` must each be at most 255 characters.

Generated-package validation treats duplicate identifiers, invalid `InternalName`, and over-length identifiers as hard errors.

## FieldIndex And FieldName Synchronization

`product-rule-backed`, `validator-backed`

For generated non-system list fields, the numeric suffix at the absolute end of `FieldName` must match `FieldIndex`.

Examples:

- Valid: `FieldIndex: 11`, `FieldName: "Text11"`.
- Invalid: `FieldIndex: 11`, `FieldName: "Text6"`.

System/native fields such as `Title` are excluded from this suffix rule. `Title` keeps `FieldIndex: 0` and native metadata.

Known failure verified read-only:

- Package: `<downloads>/Workflow Actions Runtime Baseline (1).yap`.
- List: `Purchase Requests Runtime Test`.
- Product-reported blocker: users could not create a manual new field and saw "The value for the field already exists, please enter a new value."
- Verified mismatches include `FieldIndex: 11` with `FieldName: "Text6"`, plus additional mismatched typed storage names in the same generated list.
- Proof: `product-rule-backed`, `validator-backed`; not runtime-fixed in this pass.

## Process Key Rules

`product-rule-backed`, `validator-backed`

Process keys, including `Data.Forms[].Key`, `FlowKey` when present, and decoded workflow `defkey`, must:

- contain only letters, numbers, and underscores: `[a-zA-Z0-9_]`;
- be at most 255 characters.

Generated-package validation treats invalid process keys as hard errors.

## Approval Form NoRule

`product-rule-backed`, `validator-backed`

Approval form `Forms[].NoRule` must be an object:

```json
{
  "Prefix": "test_{date}_{index}",
  "StartIndex": 1,
  "CustomLength": 8,
  "AutoIncrement": 1
}
```

`NoRule.Prefix` may use these dynamic placeholders:

- Date placeholders: `{date}`, `{yyyy}`, `{yy}`, `{mm}`, `{dd}`, `{yymmdd}`, `{mmdd}`.
- Sequence placeholder: `{index}`.

Strict rule: `NoRule.Prefix` must include `{index}` anywhere in the prefix string.

Known failure verified read-only:

- Package: `<downloads>/form-report-runtime-baseline.v1.yap`.
- Form: `Form Report Source Approval`.
- Verified shape: `Forms[0].NoRule` is boolean `true`.
- Required shape: object with `Prefix`, `StartIndex`, `CustomLength`, and `AutoIncrement`; `Prefix` includes `{index}`.
- Proof: `product-rule-backed`, `validator-backed`; runtime import remains blocked and not fixed by runtime proof in this pass.

## Validator Behavior

`validator-backed`

Hard errors in generated final validation:

- duplicate `DisplayName`, `FieldName`, or `InternalName` within the same list;
- `DisplayName`, `FieldName`, or `InternalName` longer than 255 characters;
- invalid `InternalName` characters;
- generated non-system field with missing or mismatched `FieldName` numeric suffix;
- invalid process key characters;
- process key longer than 255 characters;
- approval form `NoRule` missing or not an object;
- missing `{index}` in `NoRule.Prefix`;
- invalid `NoRule.StartIndex`, `NoRule.CustomLength`, or `NoRule.AutoIncrement`.

Warnings:

- unknown list field `Type`;
- unknown optional `NoRule` fields;
- compatibility-mode cases where legacy exports may need review rather than blocking.

Updated validation surfaces:

- `validate-yap-package.js`
- `validate-ydl-list.js`
- `validate-ywf-def.js`
- `scripts/inspect-app-creation-rules.mjs`

## Generator Guidance

`product-rule-backed`, `validator-backed`

All future generators that create Yeeflow app/list/workflow packages must:

- allocate `FieldIndex` and `FieldName` together;
- ensure the final numeric suffix in `FieldName` equals `FieldIndex`;
- never reuse `DisplayName`, `FieldName`, or `InternalName` inside a list;
- generate `InternalName` values using only `[a-zA-Z0-9_]`;
- emit process keys using only `[a-zA-Z0-9_]`;
- emit approval form `NoRule` as the required object;
- include `{index}` in every generated `NoRule.Prefix`;
- stop before import when these checks fail.

Patched generator sources in this pass:

- `tools/generators/generate-workflow-actions-combined-runtime-baseline.mjs`
- `tools/generators/generate-workflow-actions-batch-runtime-baseline.mjs`

The Form Report runtime baseline generator is not present on current `main`; the previous generated package issue is covered by validator/inspector checks and documented as pending product-feedback/runtime follow-up.

Ignored local fixed package checks:

- `workflow-actions-runtime-baseline-field-rules-fixed.v1.yap` was regenerated from the combined workflow-actions generator. App creation rule inspector passed with zero findings; package validation and graph validation passed with warnings only. This is local-generation-fixed, not runtime-import-proven.
- `form-report-runtime-baseline-norule-fixed.v1.yap` was produced as an ignored local repair of the blocked Form Report package's `Forms[0].NoRule`. App creation rule inspector passed with warnings only for unsupported `metadata` field `Type` values. Full package validation still fails on pre-existing Form Report field-source resolution errors, so only the NoRule shape is locally fixed; the Form Report runtime baseline remains not runtime-import-proven.

## Proof Boundary

This pass is `product-rule-backed` and `validator-backed`.

It is not runtime-import-proven. A future focused runtime pass should regenerate fixed packages, import them, and only then upgrade proof to import/open/designer/runtime behavior.

## Follow-Up Field Type Expansion From Data Lists (2).yap

`Data Lists (2).yap` export-proves a single metadata data-list custom field with `Type = "metadata"`, `FieldType = "Bigint"`, and Rules containing `source` plus `categoryId`. This is an export-proven addition/exception to the earlier product-team supported type list, which named `mutiple-metadata` but did not name single `metadata`. Validators should accept `metadata` as a known field type while preserving the v0.5.12 generation-blocking gates for FieldIndex/FieldName synchronization, unique identifiers, valid InternalName, valid process keys, and valid NoRule.
