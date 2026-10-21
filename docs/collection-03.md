# docs collection 3

This file combines 15 documents from `docs/` for the Claude directory build. A reference to `docs/<name>.md` is the section that starts at the anchor `#<name>` below.

- [workflow-action-configuration-reference.md](#workflow-action-configuration-reference)
- [workflow-action-generation-rules.md](#workflow-action-generation-rules)
- [workflow-transition-condition-patterns-study.md](#workflow-transition-condition-patterns-study)
- [yap-app-generation-first-test-plan.md](#yap-app-generation-first-test-plan)
- [yeeflow-application-design-system.md](#yeeflow-application-design-system)
- [yeeflow-application-generation-review-checklist.md](#yeeflow-application-generation-review-checklist)
- [yeeflow-application-layout-standards.md](#yeeflow-application-layout-standards)
- [yeeflow-application-package-generation-rules.md](#yeeflow-application-package-generation-rules)
- [yeeflow-application-style-token-standards.md](#yeeflow-application-style-token-standards)
- [yeeflow-application-ui-ux-standards.md](#yeeflow-application-ui-ux-standards)
- [yeeflow-approval-form-design-standards.md](#yeeflow-approval-form-design-standards)
- [yeeflow-approval-form-ui-ux-patterns.md](#yeeflow-approval-form-ui-ux-patterns)
- [yeeflow-control-configuration-reference.md](#yeeflow-control-configuration-reference)
- [yeeflow-control-field-generation-rules.md](#yeeflow-control-field-generation-rules)
- [yeeflow-control-to-field-mapping.md](#yeeflow-control-to-field-mapping)

---

<a id="workflow-action-configuration-reference"></a>

<!-- source: docs/collection-03.md#workflow-action-configuration-reference -->

# Yeeflow Workflow Action Configuration Reference

Source: `<downloads>/node-configurations.json` (read-only user-supplied reference).

This reference normalizes the official workflow action configuration JSON into generator-friendly node/action rules. Use `workflow-action-configurations.normalized.json` as the machine-readable companion.

## Inventory

- Actions inventoried: 28
- Schema version: `2026-05-13.workflow-action-configurations.v1`

| Action | Risk | Required Properties | Enum Properties | Conditional Properties | Nested Schemas |
| --- | --- | ---: | ---: | ---: | ---: |
| `AI` | external_or_credential_sensitive | 0 | 4 | 13 | 3 |
| `AcrobatSign` | external_or_credential_sensitive | 0 | 0 | 0 | 0 |
| `AddWatermark` | document_dependency | 0 | 3 | 2 | 1 |
| `AzureOpenAI` | external_or_credential_sensitive | 0 | 4 | 7 | 1 |
| `CandidateTask` | standard | 0 | 3 | 10 | 1 |
| `Connector` | external_or_credential_sensitive | 0 | 0 | 0 | 0 |
| `ContentList` | standard | 0 | 2 | 6 | 2 |
| `ConvertToPdf` | document_dependency | 0 | 1 | 1 | 1 |
| `Delay` | standard | 0 | 3 | 7 | 1 |
| `DocuSign` | external_or_credential_sensitive | 0 | 5 | 7 | 0 |
| `DocumentRecognition` | external_or_credential_sensitive | 0 | 5 | 3 | 1 |
| `EndNoneEvent` | standard | 1 | 1 | 6 | 0 |
| `EndRejectEvent` | standard | 1 | 1 | 6 | 0 |
| `GenerateDocument` | document_dependency | 0 | 2 | 1 | 1 |
| `HttpRequest` | external_or_credential_sensitive | 0 | 2 | 2 | 0 |
| `InclusiveGateway` | standard | 1 | 0 | 0 | 0 |
| `Loop` | standard | 0 | 2 | 4 | 2 |
| `LoopBreak` | standard | 0 | 0 | 0 | 0 |
| `MailTask` | standard | 0 | 1 | 1 | 0 |
| `MultiAssignmentTask` | standard | 1 | 4 | 12 | 2 |
| `PandaDoc` | external_or_credential_sensitive | 0 | 4 | 4 | 1 |
| `QueryData` | standard | 0 | 4 | 9 | 3 |
| `ResponseTo` | standard | 0 | 0 | 0 | 0 |
| `SequenceFlow` | standard | 1 | 0 | 0 | 1 |
| `SetVariableTask` | standard | 0 | 1 | 4 | 1 |
| `SignalEvent` | standard | 0 | 0 | 1 | 1 |
| `StartNoneEvent` | standard | 1 | 1 | 8 | 2 |
| `StartWorkflowTask` | standard | 0 | 2 | 4 | 1 |

## AI

- Control types: `AI`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values
- Conditional: `properties.image.type`, `properties.image.value`, `properties.user.value`, `properties.assistant.type`, `properties.assistant.value`, `properties.file`, `properties.response.value`, `properties.data.AppID`, `properties.data.ListSetID`, `properties.data.AgentID`, `properties.enableSearch`, `properties.inputVariables`, `properties.outputVariables`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.image.type` | `string` | no |  | properties | Fixed value 2, indicating that the value of properties.image.value is determined by Yeeflow Calculation expression.(When properties.type is 'img') |
| `properties.image.value` | `string` | no |  | properties | Yeeflow Calculation expression.(When properties.type is 'img') |
| `properties.user.type` | `enum` | no | `0`, `2` |  | Select the type of user message. |
| `properties.user.value` | `string` | no |  | properties | User message.(When properties.user.type is 0, it is text. When properties.user.type is 2, its value is determined by a Yeeflow Calculation expression.) |
| `properties.assistant.type` | `enum` | no | `0`, `2` | properties | Select the type of assistant instructions.(When properties.type is not 'agent') |
| `properties.assistant.value` | `string` | no |  | properties | Assistant instructions.(When properties.type is not 'agent'. When properties.assistant.type is 0, it is text. When properties.assistant.type is 2, its value is determined by a Yeeflow Calculation expression.) |
| `properties.file` | `array` | no |  | properties | Defines different types of expression configurations, supports Yeeflow Calculation expression or workflow variable(When properties.type is 'file' or properties.type is 'chat') |
| `properties.response.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the response to. |
| `properties.response.value` | `string` | no |  | properties | select the variable id to save the response to.Only 'text' type variable is supported.(When properties.type is not 'agent') |
| `properties.type` | `enum` | no | `chat`, `img`, `file`, `agent` |  | Select the event type of AI model. |
| `properties.data.AppID` | `number` | no |  | properties | The AppID of the Azure AI Document Intelligence resource.(When properties.type is 'agent') |
| `properties.data.ListSetID` | `string` | no |  | properties | The listset ID of the Azure AI Document Intelligence resource.(When properties.type is 'agent') |
| `properties.data.AgentID` | `string` | no |  | properties | The AgentID of the Azure AI Document Intelligence resource.(When properties.type is 'agent') |
| `properties.enableSearch` | `boolean` | no |  | properties | Enable search.(When properties.type is 'chat' && properties.file is  empty) |
| `properties.inputVariables` | `array` | no |  | properties; type is 1, Yeeflow Calculation expression when type is 2)"}},"additionalProperties":false}},"additionalProperties":false},"minItems":1} | Defines configuration for input variables (When properties.type is 'agent') |
| `properties.outputVariables` | `array` | no |  | properties; type is __variables__, List Field ID when type is __list__)"}},"additionalProperties":false}},"additionalProperties":false},"minItems":1} | Defines configuration for output variables (When properties.type is 'agent') |

Nested schema notes:
- `properties.file`: Defines different types of expression configurations, supports Yeeflow Calculation expression or workflow variable(When properties.type is 'file' or properties.type is 'chat')
- `properties.inputVariables`: Defines configuration for input variables (When properties.type is 'agent')
- `properties.outputVariables`: Defines configuration for output variables (When properties.type is 'agent')

Export-backed scheduled workflow note: `AI Agent and Copilot Local Resource Baseline8.yap` proves `properties.type = "agent"` can invoke an app-contained AI Agent from a Scheduled Workflow. The action stores the Agent reference in `properties.data.AgentID`, passes workflow variables through `properties.inputVariables[].value`, maps Agent outputs back to workflow variables through `properties.outputVariables[].value.prefix = "__variables_"`, and may enable AI Assistant context enrichment under `properties.context.selected`.

## AcrobatSign

- Control types: `AcrobatSign`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.auth.type` | `string` | no |  |  | Fixed value 'apiKey'. This field must be set to 'apiKey' to indicate API key authentication for AcrobatSign. |
| `properties.auth.value` | `string` | no |  |  | The API key to use for authentication. |
| `properties.signType` | `string` | no |  |  | The type of signing process (e.g., SEQUENCE). |
| `properties.agreementName` | `string` | no |  |  | The name of the agreement to sign. Rich Text Editor is supported. |
| `properties.signers` | `string` | no |  |  | The signers of the agreement. Rich Text Editor is supported. |
| `properties.signCcs` | `string` | no |  |  | The CC recipient of the agreement. Rich Text Editor is supported. |
| `properties.signFiles.list` | `array` | no |  |  | Retrieve source files from  list field.Supports type file-upload |
| `properties.signFiles.variables` | `array` | no |  |  | Retrieve source files from  variable.Supports type file. |
| `properties.signedFile` | `string` | no |  |  | Save the signed file to a variable.Supports type file. |
| `properties.signedStatus` | `string` | no |  |  | Save the signed status to a variable.Supports type text. |
| `properties.signedAuditTrailFile` | `string` | no |  |  | Save the signed audit trail file to a variable.Supports type file. |
| `properties.ext` | `string` | no |  |  | Extended parameters. Rich Text Editor is supported. |

## AddWatermark

- Control types: `AddWatermark`
- Support status: partially supported; requires document/template dependency validation
- Conditional: `properties.watermarks`, `properties.outputFileName.type`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.saveToVariable.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the result to. |
| `properties.saveToVariable.value` | `string` | no |  |  | select the variable id to save the result to.Only 'file' type variable is supported. |
| `properties.inputFile.value` | `object` | no |  |  | If type is 1, it is a variable configuration with format {"exprType":"variable","valueType":"file","id":"Variable ID","type":"expr"}; if type is 2, it is a Yeeflow calculation expression. |
| `properties.inputFile.type` | `enum` | no | `1`, `2` |  | select the type of variable to save the result to. |
| `properties.watermarks` | `array` | no |  | type is image)","additionalProperties":false},"content1":{"type":"object","properties":{"type":{"type":"integer","const":2,"description":"Content type: 2 - Expression array"},"value":{"type":"array","description":"Yeeflow Calculation expression"}},"description":"First line text configuration (when type is text)","additionalProperties":false},"content2":{"type":"object","properties":{"type":{"type":"integer","const":2,"description":"Content type: 2 - Expression array"},"value":{"type":"array","description":"Yeeflow Calculation expression"}},"description":"Second line text configuration (when type is text)","additionalProperties":false},"style":{"type":"string","description":"Watermark style","enum":["repeat","single"]},"position":{"type":"string","enum":["top-left","top-center","top-right","center-center","bottom-left","bottom-center","bottom-right"],"description":"Watermark position (when style is single)"},"repeatPattern":{"type":"string","enum":["full-page","diagonal"],"description":"Repeat pattern (when style is repeat)"},"offsetX":{"type":"integer","description":"Horizontal offset (px) (when style is single)"},"offsetY":{"type":"integer","description":"Vertical offset (px) (when style is single)"},"horizontal":{"type":"integer","description":"Horizontal spacing (when style is repeat)"},"vertical":{"type":"integer","description":"Vertical spacing (when style is repeat)"},"fontSize":{"type":"integer","description":"Font size (px) (when type is text)"},"fontWeight":{"type":"string","enum":["100","200","300","400","500","600","700","800","900","0","1","2"],"description":"Font weight (when type is text), 0:default, 1:normal, 2:bold"},"width":{"type":"integer","description":"Width of the watermark image (px) (when type is image)"},"rotation":{"type":"integer","description":"Rotation angle (degrees)"},"transparency":{"type":"integer","description":"Transparency (0-100%)"},"color":{"type":"string","description":"Font color (when type is text)"}}},"minItems":1} | Defines the configuration of document watermarks, supporting both text and image types. |
| `properties.outputFileType` | `enum` | no | `pdf`, `original` |  | Set the type of output file. |
| `properties.outputFileName.value` | `array` | no |  |  | Yeeflow calculation expression to evaluate the value to pass.  |
| `properties.outputFileName.type` | `number` | no |  | properties | Fixed value 2 when properties.outputFileName.value is present, indicating that properties.outputFileName.value is an array from the expression editor. |

Nested schema notes:
- `properties.watermarks`: Defines the configuration of document watermarks, supporting both text and image types.

## AzureOpenAI

- Control types: `AzureOpenAI`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values
- Conditional: `properties.image.type`, `properties.image.value`, `properties.user.value`, `properties.assistant.type`, `properties.assistant.value`, `properties.file`, `properties.response.value`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.image.type` | `string` | no |  | properties | Fixed value 2, indicating that the value of properties.image.value is determined by Yeeflow Calculation expression.(When properties.type is 'img') |
| `properties.image.value` | `string` | no |  | properties | Yeeflow Calculation expression.(When properties.type is 'img') |
| `properties.user.type` | `enum` | no | `0`, `2` |  | Select the type of user message. |
| `properties.user.value` | `string` | no |  | properties | User message.(When properties.user.type is 0, it is text. When properties.user.type is 2, its value is determined by a Yeeflow Calculation expression.) |
| `properties.assistant.type` | `enum` | no | `0`, `2` | properties | Select the type of assistant instructions.(When properties.type is not 'agent') |
| `properties.assistant.value` | `string` | no |  | properties | Assistant instructions.(When properties.type is not 'agent'. When properties.assistant.type is 0, it is text. When properties.assistant.type is 2, its value is determined by a Yeeflow Calculation expression.) |
| `properties.file` | `array` | no |  | properties | Defines different types of expression configurations, supports Yeeflow Calculation expression or workflow variable(When properties.type is 'file' or properties.type is 'chat') |
| `properties.response.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the response to. |
| `properties.response.value` | `string` | no |  | properties | select the variable id to save the response to.Only 'text' type variable is supported.(When properties.type is not 'agent') |
| `properties.connectionId` | `string` | no |  |  | The connection ID of the resource. |
| `properties.type` | `enum` | no | `chat`, `img` |  | Select the event type of AI model. |

Nested schema notes:
- `properties.file`: Defines different types of expression configurations, supports Yeeflow Calculation expression or workflow variable(When properties.type is 'file' or properties.type is 'chat')

## CandidateTask

- Control types: `CandidateTask`
- Support status: supported for structural validation
- Conditional: `properties.usertaskassignment`, `properties.duedatedefinition`, `properties.duedateexpress`, `properties.isfromworkcalendar`, `properties.from`, `properties.to`, `properties.cc`, `properties.subject`, `properties.files.list`, `properties.html`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.displayname` | `string` | no |  |  | The title or heading displayed at the top of a task form that is generated or initiated from a task list or workflow item. You can use the rich text editor to retrieve the task title. |
| `properties.usertaskassignment` | `array` | no |  | type is position)"},"value":{"type":"string","description":"Value (user ID, organization ID, position ID, or rich text)"},"title":{"type":"string","description":"Display title, may contain rich text | Defines the configuration for the Task assignee editor, supporting multiple selection methods and rich text |
| `properties.taskurl` | `string` | no |  |  | The URL of the task form that is generated or initiated from a task list or workflow item.  |
| `properties.tasktype` | `enum` | no | `approve`, `complete` |  | The type of task to be performed. If set to 'approve', 'Approve' and 'Reject' buttons will be displayed. If set to 'complete', a 'Complete' button will be displayed. |
| `properties.duedatetype` | `enum` | no | `hour`, `express`, `day`, `minute` |  | Specifies the due date type for the task. |
| `properties.duedatedefinition` | `number` | no |  | properties | Specifies the due date for the task. The format depends on the due date type. (When properties.duedatetype is hour, day, or minute) |
| `properties.duedateexpress` | `string` | no |  | properties | Specifies the express completion date for the task by a rich text editor. (When properties.duedatetype is express) |
| `properties.isfromworkcalendar` | `boolean` | no |  | properties | Specifies whether the express completion date is from the work calendar. (When properties.duedatetype is day) |
| `properties.disablequickapproval` | `boolean` | no |  |  | Specifies whether to disable quick approval.if false, the workflow task will allow direct task processing in notification message cards. |
| `properties.isenabledemail` | `boolean` | no |  |  | Specifies whether to enable email notifications. |
| `properties.from` | `enum` | no | `0`, `-1` | properties | Specifies the sender of the notification message.(When properties.isenabledemail is true) |
| `properties.to` | `string` | no |  | properties | Specifies the recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.cc` | `string` | no |  | properties | Specifies the carbon copy recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.subject` | `string` | no |  | properties | Specifies the subject of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.files.list` | `array` | no |  | list workflow task) | Specifies the file fields to be attached in the notification message.(When list workflow task) |
| `properties.files.variables` | `array` | no |  |  | Specifies the variables to be replaced in the notification message. |
| `properties.html` | `string` | no |  | properties | Specifies the content of the notification message by a rich editor.(When properties.isenabledemail is true) |

Nested schema notes:
- `properties.usertaskassignment`: Defines the configuration for the Task assignee editor, supporting multiple selection methods and rich text

## Connector

- Control types: `Connector`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |

## ContentList

- Control types: `ContentList`
- Support status: supported for structural validation
- Conditional: `properties.appid`, `properties.listsetid`, `properties.listid`, `properties.type`, `properties.listdatas`, `properties.wheres`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.listtype` | `enum` | no | `current`, `select` |  | Specifies the data source for the operation. If set to 'Current list', it uses the list associated with the current context. If set to 'Select a data list', it allows selecting a specific list. |
| `properties.appid` | `number` | no |  | properties | The AppID of the associated data source. (When properties.listtype is 'select') |
| `properties.listsetid` | `string` | no |  | properties | The ListSetID of the associated data source. (When properties.listtype is 'select') |
| `properties.listid` | `string` | no |  | properties | The ListID of the associated data source. (When properties.listtype is 'select') |
| `properties.type` | `enum` | no | `add`, `edit`, `remove` | properties | The operation type of the associated data source. (When properties.listtype is 'select') |
| `properties.listdatas` | `array` | no |  | properties | Configuration of data table column data.(When properties.type is 'add' or 'edit' or when properties.listtype is 'current') |
| `properties.wheres` | `array` | no |  | properties; op is 6/7)"},"showCus":{"type":"boolean","title":"Show Custom Config","description":"Whether to display custom configuration for this condition (optional field)"},"conditions":{"type":"array","title":"Nested Filter Conditions","description":"Sub-filter conditions nested under the current condition","items":{"$ref":"#/items"}}},"required":["pre","left","op"],"additionalProperties":false},"minItems":0,"uniqueItems":false} | A list of filter conditions for data filtering, supporting nested conditions(When properties.type is 'add' or 'edit') |

Nested schema notes:
- `properties.listdatas`: Configuration of data table column data.(When properties.type is 'add' or 'edit' or when properties.listtype is 'current')
- `properties.wheres`: A list of filter conditions for data filtering, supporting nested conditions(When properties.type is 'add' or 'edit')

## ConvertToPdf

- Control types: `ConvertToPdf`
- Support status: partially supported; requires document/template dependency validation
- Conditional: `properties.outputFileName.type`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.saveToVariable.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the result to. |
| `properties.saveToVariable.value` | `string` | no |  |  | select the variable id to save the result to.Only 'file' type variable is supported. |
| `properties.inputFiles` | `array` | no |  |  | Configuration for input file selection |
| `properties.outputFileName.value` | `array` | no |  |  | Yeeflow calculation expression to evaluate the value to pass.  |
| `properties.outputFileName.type` | `number` | no |  | properties | Fixed value 2 when properties.outputFileName.value is present, indicating that properties.outputFileName.value is an array from the expression editor. |

Nested schema notes:
- `properties.inputFiles`: Configuration for input file selection

## Delay

- Control types: `Delay`
- Support status: supported for structural validation
- Conditional: `properties.duration.count`, `properties.duration.unit`, `properties.until.subType`, `properties.until.specific.date`, `properties.until.specific.time`, `properties.until.dynamic.time`, `properties.condition.conditions`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.type` | `enum` | no | `duration`, `until`, `condition` |  | Specifies the type of delay logic. Required. |
| `properties.duration.count` | `string` | no |  | properties; Required when properties | The numeric value for the delay duration by a rich editor. Required when properties.type is 'duration'. |
| `properties.duration.unit` | `enum` | no | `Month`, `Week`, `Day`, `Hour`, `Minute`, `Second` | properties; Required when properties | The time unit for the delay duration. Required when properties.type is 'duration'. |
| `properties.until.subType` | `enum` | no | `specific`, `dynamic` | properties; Required when properties | Specifies the source type for the 'Until' date. (Required when properties.type is 'until') |
| `properties.until.specific.date` | `string` | no |  | properties; Required when properties | The specific date to wait until. (Required when properties.type is 'until' and properties.until.subType is 'specific') |
| `properties.until.specific.time` | `string` | no |  | properties; Required when properties | The specific time of day to wait until. (Required when properties.type is 'until' and properties.until.subType is 'specific') |
| `properties.until.specific.delayTime` | `string` | no |  |  | The actual delay time calculated from 'properties.until.specific.date' and 'properties.until.specific.time', converted to server time. |
| `properties.until.dynamic.time` | `string` | no |  | properties; Required when properties | Calculate the target date/time. Rich Text. Required when properties.type is 'until' and subType is 'dynamic'. |
| `properties.condition.conditions` | `array` | no |  | type is 1, this is the selected process variable; type is 2, this is the Yeeflow Calculation expression; type is 0, this is the fixed value | A list of conditions that must be satisfied for the workflow to proceed. |

Nested schema notes:
- `properties.condition.conditions`: A list of conditions that must be satisfied for the workflow to proceed.

## DocuSign

- Control types: `DocuSign`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values
- Conditional: `properties.sourceFile.type`, `properties.sourceFile.value`, `properties.templateId`, `properties.fieldMappings`, `properties.recipients`, `properties.subject`, `properties.html`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.connectionId` | `string` | no |  |  | The connection ID of the resource. |
| `properties.documentSource` | `enum` | no | `local`, `template` |  | The source of the document to sign. |
| `properties.sourceFile.type` | `enum` | no | `1`, `2` | properties | The type of the source file.(When properties.documentSource is 'local') |
| `properties.sourceFile.value` | `string` | no |  | properties | The value of the source file.(When properties.documentSource is 'local') |
| `properties.templateId` | `string` | no |  | properties | The ID of the template to use for signing.(When properties.documentSource is 'template') |
| `properties.fieldMappings` | `array` | no |  | properties | The field mappings to use for signing.(When properties.documentSource is 'template') |
| `properties.documentFolder` | `string` | no |  |  | The connection folder to use for signing |
| `properties.documentName` | `string` | no |  |  | The document name to use for signing. Rich Text Editor is supported. |
| `properties.ext` | `string` | no |  |  | The file extension to use for signing.Rich Text Editor is supported. |
| `properties.sendDocument` | `boolean` | no |  |  | Send the document to the signers for signing. |
| `properties.recipients` | `string` | no |  | properties | The recipients to use for signing. Rich Text Editor is supported.(When properties.sendDocument is true) |
| `properties.subject` | `string` | no |  | properties | Specifies the subject of the notification message.(When properties.sendDocument is true) |
| `properties.html` | `string` | no |  | properties | Specifies the body of the notification message.(When properties.sendDocument is true) |
| `properties.status.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the signed status to. |
| `properties.status.value` | `string` | no |  |  | select the variable id to save the signed status to.Only 'text' type variable is supported. |
| `properties.signedFile.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the signed file to. |
| `properties.signedFile.value` | `string` | no |  |  | select the variable id to save the signed file to.Only 'file' type variable is supported. |
| `properties.signedAuditTrail.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the signed audit trail to. |
| `properties.signedAuditTrail.value` | `string` | no |  |  | select the variable id to save the signed audit trail to.Only 'file' type variable is supported. |

## DocumentRecognition

- Control types: `DocumentRecognition`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values
- Conditional: `properties.modelType`, `properties.connectionId`, `properties.pageRange`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.inputFile.value` | `object` | no |  |  | If type is 1, it is a variable configuration with format {"exprType":"variable","valueType":"file","id":"Variable ID","type":"expr"}; if type is 2, it is a Yeeflow calculation expression. |
| `properties.inputFile.type` | `enum` | no | `1`, `2` |  | select the type of variable to save the result to. |
| `properties.serviceProvider` | `enum` | no | `in`, `azure` |  | Select the type of document to recognize. |
| `properties.modelType` | `enum` | no | `pre`, `custom` | properties | Select the type of model to use.(When properties.serviceType is 'in'.) |
| `properties.modelId` | `string` | no |  |  | The resource ID of the Azure AI Document Intelligence resource. |
| `properties.connectionId` | `string` | no |  | properties | The connection ID of the Azure AI Document Intelligence resource.(When properties.serviceType is 'azure', this field is required.) |
| `properties.rangeType` | `enum` | no | `all`, `range` |  | The range type of the document to recognize. |
| `properties.pageRange` | `number` | no |  | properties | The range of pages of the document to recognize.(When properties.rangeType is 'all', this value is 'all'.) |
| `properties.saveToVariable.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the result to. |
| `properties.saveToVariable.value` | `string` | no |  |  | select the variable id to save the result to.Only 'text' type variable is supported. |
| `properties.mappingFields` | `array` | no |  |  | Defines variable binding configuration, including name, type, and binding information |

Nested schema notes:
- `properties.mappingFields`: Defines variable binding configuration, including name, type, and binding information

## EndNoneEvent

- Control types: `EndNoneEvent`
- Support status: supported for structural validation
- Required: `properties.name`
- Conditional: `properties.from`, `properties.to`, `properties.cc`, `properties.subject`, `properties.files.list`, `properties.html`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | yes |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.isenabledemail` | `boolean` | no |  |  | Specifies whether to enable email notifications. DEFAULT is false. |
| `properties.from` | `enum` | no | `0`, `-1` | properties | Specifies the sender of the notification message.(When properties.isenabledemail is true) |
| `properties.to` | `string` | no |  | properties | Specifies the recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.cc` | `string` | no |  | properties | Specifies the carbon copy recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.subject` | `string` | no |  | properties | Specifies the subject of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.files.list` | `array` | no |  | list workflow task) | Specifies the file fields to be attached in the notification message.(When list workflow task) |
| `properties.files.variables` | `array` | no |  |  | Specifies the variables to be replaced in the notification message. |
| `properties.html` | `string` | no |  | properties | Specifies the content of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.endprocess` | `boolean` | no |  |  | Determines whether the process should be terminated immediately upon reaching this node. If set to true, the entire process instance ends, and all concurrent active activities are cancelled. DEFAULT is false. |

## EndRejectEvent

- Control types: `EndRejectEvent`
- Support status: supported for structural validation
- Required: `properties.name`
- Conditional: `properties.from`, `properties.to`, `properties.cc`, `properties.subject`, `properties.files.list`, `properties.html`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | yes |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.isenabledemail` | `boolean` | no |  |  | Specifies whether to enable email notifications. DEFAULT is false. |
| `properties.from` | `enum` | no | `0`, `-1` | properties | Specifies the sender of the notification message.(When properties.isenabledemail is true) |
| `properties.to` | `string` | no |  | properties | Specifies the recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.cc` | `string` | no |  | properties | Specifies the carbon copy recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.subject` | `string` | no |  | properties | Specifies the subject of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.files.list` | `array` | no |  | list workflow task) | Specifies the file fields to be attached in the notification message.(When list workflow task) |
| `properties.files.variables` | `array` | no |  |  | Specifies the variables to be replaced in the notification message. |
| `properties.html` | `string` | no |  | properties | Specifies the content of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.endprocess` | `boolean` | no |  |  | Determines whether the process should be terminated immediately upon reaching this node. If set to true, the entire process instance ends, and all concurrent active activities are cancelled. DEFAULT is false. |

## GenerateDocument

- Control types: `GenerateDocument`
- Support status: partially supported; requires document/template dependency validation
- Conditional: `properties.generateFileName.type`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.saveToVariable.prefix` | `enum` | no | `__variables_`, `__list_` |  | select the type of variable to save the result to. |
| `properties.saveToVariable.value` | `string` | no |  |  | select the variable id to save the result to.Only 'file' type variable is supported. |
| `properties.generateType` | `enum` | no | `pdf`, `word` |  | Set the document type to generate.  |
| `properties.templateFiles` | `array` | no |  |  | List of template file paths. Contains file information and file paths; file variables can also be bound to Variables of the current form. |
| `properties.generateFileName.value` | `array` | no |  |  | Yeeflow calculation expression to evaluate the value to pass.  |
| `properties.generateFileName.type` | `number` | no |  | properties | Fixed value 2 when properties.generateFileName.value is present, indicating that properties.generateFileName.value is an array from the expression editor. |

Nested schema notes:
- `properties.templateFiles`: List of template file paths. Contains file information and file paths; file variables can also be bound to Variables of the current form.

## HttpRequest

- Control types: `HttpRequest`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values
- Conditional: `properties.httpauth.attrs.user`, `properties.httpauth.attrs.pwd`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.httpaddress` | `string` | no |  |  | The HTTP address to send the request to. You can use the Rich Text Editor to configure the address. |
| `properties.httpMethod` | `enum` | no | `GET`, `PATCH`, `POST`, `PUT`, `DELETE` |  | The HTTP method to use for the request. |
| `properties.httpauth.type` | `enum` | no | `none`, `userpwd`, `yeeoffice` |  | The type of authentication to use for the request. |
| `properties.httpauth.attrs.user` | `string` | no |  | properties | The username to use for authentication.(When properties.httpauth.type is 'userpwd') |
| `properties.httpauth.attrs.pwd` | `string` | no |  | properties | The password to use for authentication.(When properties.httpauth.type is 'userpwd') |
| `properties.httpheader` | `string` | no |  |  | The HTTP headers to include in the request. You can use the Rich Text Editor to configure the headers. |
| `properties.httpdata` | `string` | no |  |  | The body of the request. You can use the Rich Text Editor to configure the body. |
| `properties.httpstatus` | `string` | no |  |  | The bound variable ID. Only number type is supported. |
| `properties.httpresult` | `string` | no |  |  | The bound variable ID. Support type text, list, dict. |

## InclusiveGateway

- Control types: `InclusiveGateway`
- Support status: supported for structural validation
- Required: `properties.name`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | yes |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |

## Loop

- Control types: `Loop`
- Support status: supported for structural validation
- Conditional: `properties.loopValue.type`, `properties.loopValue.value`, `properties.continueCondition`, `properties.breakCondition`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.loopType` | `enum` | no | `list`, `values`, `number` |  | Specifies the type of loop logic. |
| `properties.loopValue.prefix` | `enum` | no | `__variables_`, `__list_` |  | Set the type of variable to save the loop value. |
| `properties.loopValue.type` | `number` | no |  | properties | Data source type for the loop. When properties.loopType is 'values' or 'number', fixed as 2, indicating that properties.loopValue.value is a Yeeflow Calculation expression. When properties.loopType is 'list', the value should be ignored, as the list variable i |
| `properties.loopValue.value` | `string` | no |  | properties | Data source for the loop. When properties.loopType is 'list', this is a list type process variable. When properties.loopType is 'values', this is a Yeeflow Calculation expression that must return an array. When properties.loopType is 'number', this is a Yeeflo |
| `properties.continueCondition` | `string` | no |  | type is 1, this is the selected process variable; type is 2, this is the Yeeflow Calculation expression; type is 0, this is the fixed value | Process this iteration only if the condition is true; otherwise skip it and continue. |
| `properties.breakCondition` | `string` | no |  | true, stop the loop and continue to the next step; type is 1, this is the selected process variable; type is 2, this is the Yeeflow Calculation expression; type is 0, this is the fixed value | Check this before each iteration; when true, stop the loop and continue to the next step. |

Nested schema notes:
- `properties.continueCondition`: Process this iteration only if the condition is true; otherwise skip it and continue.
- `properties.breakCondition`: Check this before each iteration; when true, stop the loop and continue to the next step.

## LoopBreak

- Control types: `LoopBreak`
- Support status: supported for structural validation

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |

## MailTask

- Control types: `MailTask`
- Support status: supported for structural validation
- Conditional: `properties.files.list`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.from` | `enum` | no | `0`, `-1` |  | Specifies the sender of the notification message. |
| `properties.to` | `string` | no |  |  | Specifies the recipient of the notification message by a rich editor. |
| `properties.cc` | `string` | no |  |  | Specifies the carbon copy recipient of the notification message by a rich editor. |
| `properties.subject` | `string` | no |  |  | Specifies the subject of the notification message by a rich editor. |
| `properties.files.list` | `array` | no |  | list workflow task) | Specifies the file fields to be attached in the notification message.(When list workflow task) |
| `properties.files.variables` | `array` | no |  |  | Specifies the variables to be replaced in the notification message. |
| `properties.html` | `string` | no |  |  | Specifies the content of the notification message by a rich editor. |

Export-backed scheduled workflow note: `AI Agent and Copilot Local Resource Baseline8.yap` proves `MailTask` inside Scheduled Workflow graphs stores recipients in `properties.to`, subjects in `properties.subject`, and body HTML in `properties.html`. Subject/body may be expression-button HTML that references workflow variables. Fixed literal recipients must be treated as runtime-sensitive and redacted in documentation.

## MultiAssignmentTask

- Control types: `MultiAssignmentTask`
- Support status: supported for structural validation
- Required: `properties.name`
- Conditional: `properties.duedatedefinition`, `properties.duedateexpress`, `properties.isfromworkcalendar`, `properties.approvepercentage`, `properties.automaticapproveddefinition`, `properties.notifyrules`, `properties.from`, `properties.to`, `properties.cc`, `properties.subject`, `properties.files.list`, `properties.html`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | yes |  |  | Node Name |
| `properties.displayname` | `string` | no |  |  | The title or heading displayed at the top of a task form that is generated or initiated from a task list or workflow item. You can use the rich text editor to retrieve the task title. |
| `properties.usertaskassignment` | `array` | no |  |  | Defines the configuration for the Task assignee editor, supporting multiple selection methods and rich text |
| `properties.taskurl` | `string` | no |  |  | The URL of the task form that is generated or initiated from a task list or workflow item.  |
| `properties.tasktype` | `enum` | no | `approve`, `complete` |  | The type of task to be performed. If set to 'approve', 'Approve' and 'Reject' buttons will be displayed. If set to 'complete', a 'Complete' button will be displayed. |
| `properties.duedatetype` | `enum` | no | `hour`, `express`, `day`, `minute` |  | Specifies the due date type for the task. |
| `properties.duedatedefinition` | `number` | no |  | properties | Specifies the due date for the task. The format depends on the due date type. (When properties.duedatetype is hour, day, or minute). Important: This value is descriptive and does not support logic-based conditional checks for completion status. |
| `properties.duedateexpress` | `string` | no |  | properties | Specifies the express completion date for the task by a rich text editor. (When properties.duedatetype is express) |
| `properties.isfromworkcalendar` | `boolean` | no |  | properties | Specifies whether the express completion date is from the work calendar. (When properties.duedatetype is day) |
| `properties.disablequickapproval` | `boolean` | no |  |  | Specifies whether to disable quick approval.if false, the workflow task will allow direct task processing in notification message cards. |
| `properties.issequential` | `boolean` | no |  |  | Specifies whether the task is sequential. If set to true, the task is completed in sequence. If set to false, the task is completed in parallel. DEFAULT is false. |
| `properties.isallowreassign` | `boolean` | no |  |  | Specifies whether the task is reassignable. If set to true, the task can be reassigned to another user. If set to false, the task cannot be reassigned. DEFAULT is false. |
| `properties.isallowsign` | `boolean` | no |  |  | Specifies whether the task is signable. If set to true, the task can be signed by the assignee. If set to false, the task cannot be signed. DEFAULT is false. |
| `properties.approveway` | `enum` | no | `allapprove`, `anyprocess`, `anyapprove`, `anyreject`, `custompercentage` |  | Specifies whether the task is commentable. If set to true, the task can be commented on. If set to false, the task cannot be commented on. |
| `properties.approvepercentage` | `number` | no |  | properties | Set the percentage of approvers to approve the task.Number 0-100 (when properties.approveway is custompercentage) |
| `properties.allowskip` | `boolean` | no |  |  | Specifies whether the task is skippable. If set to false, the task can be skipped. If set to true, the task cannot be skipped. |
| `properties.automaticapproveddefinition` | `boolean` | no |  | the approver is the applicant or has approved the previous task | If set to true, automatically approve the current task when the approver is the applicant or has approved the previous task. |
| `properties.isallowrecalled` | `boolean` | no |  |  | Specifies whether the task is recallable. |
| `properties.notifyrules` | `array` | no |  | relative=1 or -1)"},"type":{"type":"string","enum":["day","hour","minute"],"description":"Set the offset type for the reminder time (when relative=1 or -1)"}},"additionalProperties":true,"description":"Action trigger time configuration"},"from":{"type":"string","enum":["0","-1"],"description":"Configure sender: -1 for system default sender, 0 for Yeeflow mail service (when actiontype=0)"},"to":{"type":"string","description":"Recipient configuration (when actiontype=0)"},"subject":{"type":"string","description":"Reminder subject (email subject/notification title), supports HTML and dynamic expressions (when actiontype=0)"},"content":{"type":"string","description":"If actiontype=1, values are Approved, Rejected (tasktype=complete, only Completed) | Defines the configuration of workflow task reminders after task due date, including email, notification, etc. |
| `properties.isenabledemail` | `boolean` | no |  |  | Specifies whether to enable email notifications. |
| `properties.from` | `enum` | no | `0`, `-1` | properties | Specifies the sender of the notification message.(When properties.isenabledemail is true) |
| `properties.to` | `string` | no |  | properties | Specifies the recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.cc` | `string` | no |  | properties | Specifies the carbon copy recipient of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.subject` | `string` | no |  | properties | Specifies the subject of the notification message by a rich editor.(When properties.isenabledemail is true) |
| `properties.files.list` | `array` | no |  | list workflow task) | Specifies the file fields to be attached in the notification message.(When list workflow task) |
| `properties.files.variables` | `array` | no |  |  | Specifies the variables to be replaced in the notification message. |
| `properties.html` | `string` | no |  | properties | Specifies the content of the notification message by a rich editor.(When properties.isenabledemail is true) |

Nested schema notes:
- `properties.usertaskassignment`: Defines the configuration for the Task assignee editor, supporting multiple selection methods and rich text
- `properties.notifyrules`: Defines the configuration of workflow task reminders after task due date, including email, notification, etc.

## PandaDoc

- Control types: `PandaDoc`
- Support status: partially supported; external or credential-dependent and must not be bundled with sensitive values
- Conditional: `properties.sourceFile.type`, `properties.sourceFile.value`, `properties.templateId`, `properties.fieldMappings`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.connectionId` | `string` | no |  |  | Connection ID for PandaDoc integration. This field is required to establish a connection with the PandaDoc service. |
| `properties.documentSource` | `enum` | no | `local`, `template` |  | Specifies the source of the document to be processed. Options are 'local' (upload a file) or 'template' (use an existing PandaDoc template). This field is required. |
| `properties.sourceFile.type` | `enum` | no | `1`, `2` | 'properties; Required when 'properties | Source file type. 1 indicates File Variable, 2 indicates Expression. Required when 'properties.documentSource' is 'local'. |
| `properties.sourceFile.value` | `string` | no |  | properties; 'properties; Required when 'properties | Source file content. When properties.sourceFile.type is 1, this is a file type process variable. When properties.sourceFile.type is 2, this is a Yeeflow Calculation expression. Required when 'properties.documentSource' is 'local'. |
| `properties.templateId` | `string` | no |  | 'properties; Required when 'properties | The ID of the PandaDoc template to use. This field is required when 'properties.documentSource' is set to 'template'. It is hidden if 'properties.documentSource' is set to 'local'. |
| `properties.fieldMappings` | `array` | no |  | 'properties | Defines the variable mappings between the workflow data and the PandaDoc template fields. This configuration is visible and applicable only when 'properties.documentSource' is 'template' and a 'properties.templateId' is selected. |
| `properties.documentFolder` | `string` | no |  |  | Specifies the folder path or ID where the generated document should be stored in PandaDoc. |
| `properties.documentName` | `string` | no |  |  | Defines the name of the generated document by a rich editor. |
| `properties.ext` | `string` | no |  |  | Extended parameters for the PandaDoc API call, typically used for advanced configurations. Supports rich text. |
| `properties.sendDocument` | `boolean` | no |  |  | If set to true, the document will be sent immediately after generation. If false, it may be created as a draft. |
| `properties.recipients` | `string` | no |  |  | Specifies the recipients of the document by a rich editor. This field is required and defines who will receive the document for signing or viewing. |
| `properties.subject` | `string` | no |  |  | The subject line of the email sent to recipients by a rich editor. This field is required and supports rich text. |
| `properties.html` | `string` | no |  |  | The body content of the email sent to recipients by a rich editor. |
| `properties.status.prefix` | `enum` | no | `__variables_`, `__list_` |  | The type of the variable bound to the Signed Status (e.g., '__variables_'). |
| `properties.status.value` | `string` | no |  |  | The ID of the variable bound to the Signed Status. Only 'text' type variable is supported. |
| `properties.signedFile.prefix` | `enum` | no | `__variables_`, `__list_` |  | The type of the variable to save the signed file to (e.g., '__variables_'). |
| `properties.signedFile.value` | `string` | no |  |  | The ID of the variable to save the signed file to. Only 'file' type variable is supported. |

Nested schema notes:
- `properties.fieldMappings`: Defines the variable mappings between the workflow data and the PandaDoc template fields. This configuration is visible and applicable only when 'properties.documentSource' is 'template' and a 'properties.templateId' is selected.

## QueryData

- Control types: `QueryData`
- Support status: supported for structural validation
- Conditional: `properties.filters`, `properties.result.type`, `properties.result.listName`, `properties.result.listParent`, `properties.result.fields`, `properties.result.totalCount`, `properties.result.querycount_prefix`, `properties.result.pageIndex`, `properties.result.pageSize`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.appid` | `number` | no |  |  | The AppID of the associated data source.  |
| `properties.listsetid` | `string` | no |  |  | The ListSetID of the associated data source.  |
| `properties.listid` | `string` | no |  |  | The ListID of the associated data source.  |
| `properties.filters` | `array` | no |  | properties; op is 6/7)"},"showCus":{"type":"boolean","title":"Show Custom Config","description":"Whether to display custom configuration for this condition (optional field)"},"conditions":{"type":"array","title":"Nested Filter Conditions","description":"Sub-filter conditions nested under the current condition","items":{"$ref":"#/items"}}},"required":["pre","left","op"],"additionalProperties":false},"minItems":0,"uniqueItems":false} | A list of filter conditions for data filtering, supporting nested conditions(When properties.type is 'add' or 'edit') |
| `properties.datasource` | `array` | no |  |  | Defines the sorting rules for data. |
| `properties.result.type` | `enum` | no | `single`, `multiple` | properties | The type of the result. (When properties.listsetid has value) |
| `properties.result.fieldMap` | `object` | no |  |  | The mapping of field names in the result set to the desired output field names. eg: {"ListDataID":"field_4","Title":"field_1"} |
| `properties.result.listName` | `string` | no |  | properties | The specific variable or field ID to store the multi-select results. Only fields of type 'text'(list workflow:textarea) and 'list' are supported. (When properties.result.type is 'multiple') |
| `properties.result.listParent` | `enum` | no | `__variables_`, `__list_` | properties | Defines where to store the multi-select results(When properties.result.type is 'multiple'). Choose between standard workflow variables or specific fields from the associated records in a list workflow. |
| `properties.result.fields` | `array` | no |  | properties | Field assignment configuration for query results (When properties.result.type is 'multiple' and the selected field in properties.result.listName is of type text) |
| `properties.result.vartype` | `enum` | no | `text`, `list` |  | The type of the variable specified in properties.result.listName.  |
| `properties.result.totalCount` | `string` | no |  | properties | Save the result count to a variable. (When properties.result.type is 'multiple') |
| `properties.result.querycount_prefix` | `enum` | no | `__variables_`, `__list_` | properties | The prefix of the variable id to store the total count. (When properties.result.type is 'multiple') |
| `properties.result.pageIndex` | `number` | no |  | properties | Data index to be queried. (When properties.result.type is 'multiple') |
| `properties.result.pageSize` | `number` | no |  | properties | Data count per query. (When properties.result.type is 'multiple') |

Nested schema notes:
- `properties.filters`: A list of filter conditions for data filtering, supporting nested conditions(When properties.type is 'add' or 'edit')
- `properties.datasource`: Defines the sorting rules for data.
- `properties.result.fields`: Field assignment configuration for query results (When properties.result.type is 'multiple' and the selected field in properties.result.listName is of type text)

Export-backed workflow note: `AI Agent and Copilot Local Resource Baseline8.yap` proves Scheduled Workflow multiple-result JSON text output. `Approval form workflow sample-V1.6.yapk` additionally proves Approval Workflow count-only and single-to-variable modes plus Scheduled Workflow List-variable output. Count-only keeps row-target properties empty/null and writes only `totalCount`. List output uses `result.vartype = "list"`, `result.fieldMap`, a declared List variable linked to `variables.listref`, and `result.fields = null`. The V1.6 sort array is `properties.sorts`.

## ResponseTo

- Control types: `ResponseTo`
- Support status: supported for structural validation

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |

## SequenceFlow

- Control types: `SequenceFlow`
- Support status: supported for structural validation
- Required: `properties.name`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | yes |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.conditioninfo` | `array` | no |  |  | Defines the logic conditions for the Sequence Flow. The workflow engine evaluates these conditions to determine if the path should be taken. It supports nested conditions for complex logic. |

Nested schema notes:
- `properties.conditioninfo`: Defines the logic conditions for the Sequence Flow. The workflow engine evaluates these conditions to determine if the path should be taken. It supports nested conditions for complex logic.

## SetVariableTask

- Control types: `SetVariableTask`
- Support status: supported for structural validation
- Conditional: `properties.data.AppID`, `properties.data.ListSetID`, `properties.data.ProcKey`, `properties.formids`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.formtype` | `enum` | no | `current`, `custom` |  | Specifies the workflow type. If 'custom' is selected, another workflow must be specified. |
| `properties.data.AppID` | `number` | no |  | properties | The AppID of the associated data source. (When properties.formtype is 'custom') |
| `properties.data.ListSetID` | `string` | no |  | properties | The ListSetID of the associated data source. (When properties.formtype is 'custom') |
| `properties.data.ProcKey` | `string` | no |  | properties | The ProcKey of the associated data source. (When properties.formtype is 'custom') |
| `properties.formids` | `string` | no |  | properties | Form for setting variables by a rich editor. (When properties.formtype is 'custom') |
| `properties.variablesetting` | `array` | no |  |  | Set the value of Variables |

Nested schema notes:
- `properties.variablesetting`: Set the value of Variables

## SignalEvent

- Control types: `SignalEvent`
- Support status: supported for structural validation
- Conditional: `properties.eventdefinitions`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.eventdefinitions` | `array` | no |  | the application is either Canceled by a submitter or Recalled by the initiator | Defines which workflow actions will trigger this signal. You can select multiple triggers. The event fires when the application is either Canceled by a submitter or Recalled by the initiator. |

Nested schema notes:
- `properties.eventdefinitions`: Defines which workflow actions will trigger this signal. You can select multiple triggers. The event fires when the application is either Canceled by a submitter or Recalled by the initiator.

## StartNoneEvent

- Control types: `StartNoneEvent`
- Support status: supported for structural validation
- Required: `properties.name`
- Conditional: `properties.terminate-conditions`, `properties.revoke-conditions`, `properties.from`, `properties.to`, `properties.cc`, `properties.subject`, `properties.files.list`, `properties.html`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | yes |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.taskurl` | `string` | no |  |  | The ID of the submission form. Only supported in approval workflow. |
| `properties.terminate` | `boolean` | no |  |  | Allow Terminate. Default is false. Only supported in approval workflow.If false, the workflow can be terminated by the user. |
| `properties.terminate-conditions` | `array` | no |  | properties; type is 1, this is the selected Custom variable(MUST follow the same reference format as in SequenceFlow conditions); type is 2, this is the Yeeflow Calculation expression; type is 0, this is the fixed value | Conditions to allow termination. Not supported in List Workflow.(When properties.terminate is false). |
| `properties.revoke` | `boolean` | no |  |  | Allow Recall. Default is false. Only supported in approval workflow.If false, the workflow can be recalled by the user. |
| `properties.revoke-conditions` | `array` | no |  | properties; type is 1, this is the selected process variable; type is 2, this is the Yeeflow Calculation expression; type is 0, this is the fixed value | Conditions to allow recall. Not supported in List Workflow.(When properties.revoke is false). |
| `properties.isenabledemail` | `boolean` | no |  |  | Specifies whether to enable email notifications. DEFAULT is false. |
| `properties.from` | `enum` | no | `0`, `-1` | properties | Specifies the sender of the notification message.(when properties.isenabledemail is true) |
| `properties.to` | `string` | no |  | properties | Specifies the recipient of the notification message by a rich editor.(when properties.isenabledemail is true) |
| `properties.cc` | `string` | no |  | properties | Specifies the carbon copy recipient of the notification message by a rich editor.(when properties.isenabledemail is true) |
| `properties.subject` | `string` | no |  | properties | Specifies the subject of the notification message by a rich editor.(when properties.isenabledemail is true) |
| `properties.files.list` | `array` | no |  | list workflow task) | Specifies the file fields to be attached in the notification message.(When list workflow task) |
| `properties.files.variables` | `array` | no |  |  | Specifies the variables to be replaced in the notification message. |
| `properties.html` | `string` | no |  | properties | Specifies the content of the notification message by a rich editor.(when properties.isenabledemail is true) |

Nested schema notes:
- `properties.terminate-conditions`: Conditions to allow termination. Not supported in List Workflow.(When properties.terminate is false).
- `properties.revoke-conditions`: Conditions to allow recall. Not supported in List Workflow.(When properties.revoke is false).

## StartWorkflowTask

- Control types: `StartWorkflowTask`
- Support status: supported for structural validation
- Conditional: `properties.itemtype`, `properties.applicantuser`, `properties.itemids.value`, `properties.itemids.type`

| Path | Type | Required | Enum Values | Applies When | Description |
| --- | --- | --- | --- | --- | --- |
| `properties.name` | `string` | no |  |  | Node Name |
| `properties.documentation` | `string` | no |  |  | Node description |
| `properties.workflowtype` | `enum` | no | `1`, `2` |  | The type of the workflow to start. |
| `properties.data.AppID` | `number` | no |  |  | The AppID of the associated data source. If properties.workflowtype is '2' and properties.itemtype is '1', AppID is the AppID of the current list. |
| `properties.data.ListSetID` | `string` | no |  |  | The ListSetID of the associated data source. If properties.workflowtype is '2' and properties.itemtype is '1', ListSetID is the ListSetID of the current list. |
| `properties.data.ProcKey` | `string` | no |  |  | The ProcKey of the associated data source. If properties.workflowtype is '2' and properties.itemtype is '1', ProcKey is the ProcKey of the current list. |
| `properties.data.ListID` | `string` | no |  |  | The ListID of the associated data source. If properties.workflowtype is '2' and properties.itemtype is '1', ListID is the ListID of the current list. |
| `properties.itemtype` | `enum` | no | `1`, `2` | properties | Specifies the data item type. If properties.itemtype is '2', a data item must be specified.(When properties.workflowtype is '2') |
| `properties.variablesetting` | `array` | no |  |  | Set the value of Variables |
| `properties.applicantuser` | `string` | no |  | executing this workflow through the rich text editor | Set the user identity used when executing this workflow through the rich text editor |
| `properties.fallbackuser` | `string` | no |  |  | Select a fallback user( User ID) to run this workflow if the selected user is unavailable. |
| `properties.itemids.value` | `array` | no |  | properties | yeeflow calculation expression to get the data item IDs to be processed. (When properties.itemtype is '2') |
| `properties.itemids.type` | `number` | no |  | properties | Fixed value 2 when properties.itemids.value is present, indicating that properties.itemids.value is an array from the expression editor. |

Nested schema notes:
- `properties.variablesetting`: Set the value of Variables

---

<a id="workflow-action-generation-rules"></a>

<!-- source: docs/collection-03.md#workflow-action-generation-rules -->

# Yeeflow Workflow Action Generation Rules

Use this with `workflow-action-configurations.normalized.json` when generating or validating Yeeflow workflows. The source JSON is the official action configuration reference for this workspace.

## General Rules

- Preserve the exact workflow node type in `shape.stencil.id`; it must match a configured action type unless a real export proves a new type.
- Every generated workflow node should include `properties.name` when the action reference marks it required, and should include stable `id`, `resourceid`, `incoming`, `outgoing`, and `position` layout metadata.
- Validate property value types against the normalized reference before wrapper build. Treat `enum` as a string-compatible enum and reject values outside the configured keys.
- Apply conditional properties from the reference. When an action selects a mode such as `ContentList.type = add`, `Delay.type = duration`, or `QueryData.result.type = multiple`, include the matching nested configuration.
- Keep expressions and rich-text expressions as expression payloads; do not replace them with guessed IDs or literal secrets.
- Do not bundle passwords, API keys, connection credentials, tokens, or tenant-specific secret values. External/credential actions are dependency-only until resolved in a sandbox export/import cycle.

## Action Families

- Supported for structural generation/validation: `StartNoneEvent`, `EndNoneEvent`, `EndRejectEvent`, `SequenceFlow`, `ExclusiveGateway`, `InclusiveGateway`, `MultiAssignmentTask`, `CandidateTask`, `MailTask`, `SetVariableTask`, `ContentList`, `QueryData`, `SignalEvent`, `ResponseTo`, `Delay`, `Loop`, `LoopBreak`, `StartWorkflowTask`.
- Partially supported because document/template dependencies must be resolved from real metadata: `GenerateDocument`, `ConvertToPdf`, `AddWatermark`, `DocumentRecognition`.
- Partially supported and blocked for generated packages unless the user provides safe dependency metadata without sensitive values: `AI`, `AzureOpenAI`, `Connector`, `HttpRequest`, `AcrobatSign`, `DocuSign`, `PandaDoc`.

## Specific Validation Rules

- `MultiAssignmentTask` and `CandidateTask`: generated approval workflow task nodes must reference a real task form/page before handoff. Mirror the task page ID across `properties.taskurl`, `properties.taskUrl`, and `properties.TaskUrl`; set task-node `properties.pagetype = 1`; and ensure the referenced `Data.Forms[].DefResource.pageurls[]` task page uses outer `pagetype = 1`. Missing/null TaskUrl and a referenced task page with outer `pagetype = 2` are generated-final publish-readiness errors.
- `ContentList`: require target metadata for selected-list operations; `add` and `edit` require `listdatas`; `edit` and `remove` require `wheres`; each mapping must name a target column/field and provide a source value/expression.
- `QueryData`: require source metadata when the action is used; export-proven sources are Data List (`listtype = 1`), Document Library (`16`), and Form Report (`32`), while Data Report remains focused-learning-required. `filters` and `sorts` must be arrays; at most two sorts are supported. `result.type` must be `single` or `multiple`. Count-only multiple mode uses only `totalCount`. Single mode maps fields to declared workflow variables. List mode maps fields into a declared List variable backed by `variables.listref`. Text/JSON multiple mode uses selected `fields[]`. V1.7 proves all Loop modes: list uses `__variables_` or Data List-only `__list_`; values/number use `loopValue.type = 2` plus an expression array and no prefix. Every Loop requires a resolving LoopBody, and nested actions under `LoopBody.children[]` must be generated and validated recursively. Follow `docs/standards/collection-03.md#workflow-query-data-golden-reference-standard`.
- `MailTask`: require a recipient, subject, and body before runtime execution. Fixed literal email recipients are runtime-sensitive and must be redacted in docs; do not execute generated tests unless the recipient is explicitly safe.
- `AI`: for AI Assistant actions in `agent` mode, require `properties.data.AgentID`, `properties.inputVariables[]`, and `properties.outputVariables[]`; resolve `AgentID` to an included app-level AI Agent before generation is considered safe.
- `SequenceFlow`: `conditioninfo` must be an array when present; each condition object should include an operator plus left/right operands unless it is a recognized task outcome expression from a real export.
- `Delay`: `type` must be `duration`, `until`, or `condition`; duration mode requires count and unit; until mode requires a specific or dynamic time shape; condition mode requires `condition.conditions` array.
- `Loop`: `loopType` must be `list`, `values`, or `number`; loop values must include prefix/type/value; `continueCondition` and `breakCondition` must parse as condition arrays when present.
- External actions: flag `HttpRequest`, `Connector`, signing providers, `AI`, and `AzureOpenAI` as unsafe for generated final packages unless the package contains only non-sensitive dependency placeholders.

## Generator Stop Conditions

- Stop before building a `.ywf`, `.ydl`, or `.yap` wrapper if required action properties are missing in final mode.
- Stop if an enum value is outside the normalized reference.
- Stop if `ContentList` or `QueryData` targets do not resolve to package or exported-back metadata.
- Stop if an AI Assistant workflow action references an Agent that is not included in the package.
- Stop before runtime testing if a scheduled workflow can send real email, call live AI, or run automatically without an explicitly safe test plan.

## Quota Usage Lifecycle Pattern

When a business decision says quota, entitlement, budget, or benefit usage is occupied on submission:

- create the usage/occupation row when the workflow starts or immediately after the submit/start Set Variable step, not only after final approval
- query future usage by applicant identity + numeric cycle/year + active usage status
- count in-progress/occupied and approved/confirmed statuses; exclude released/rejected statuses
- store a correlation key such as application number, request instance key, form instance id, or workflow instance id so the same usage row can be updated later
- on final approval, use `ContentList` edit to mark the matching usage row approved/confirmed when runtime-safe
- on rejection/cancel, use `ContentList` edit/remove to release or reject the matching usage row when runtime-safe
- if `ContentList` edit/remove has not been runtime-proven for the scenario, document the HR manual release fallback and create a focused learning task instead of leaving the usage list unused

## Branch Coverage Pattern

For review nodes with multiple outgoing `SequenceFlow` branches:

- branch variables must be required, auto-derived, or protected by an explicit conditioned fallback route
- cover approve/reject plus yes/no/empty/unexpected values for policy-routing flags
- route unknown or empty exception flags to a specialist review/fallback path rather than allowing a dead end
- validate that every outgoing condition family eventually reaches an end or persistence node

Example: if `HasCustomPackageProduct` drives Finance/Benefits Review, the standard branch can use `Approved + HasCustomPackageProduct = No`, while the Finance/fallback branch can use `Approved + HasCustomPackageProduct != No` to cover `Yes`, empty, and unexpected values.

Yeeflow does not have an implicit `else` or `default` SequenceFlow. The fallback route above must still be a normal conditioned branch. For option variables such as `TravelType`, if only `TravelType == type1` and `TravelType == type2` are routed, add a third branch with `TravelType != type1 AND TravelType != type2`. Do not leave an outgoing SequenceFlow unconditioned and treat it as default.

## SequenceFlow Condition Operand Pattern

`Implant Application Request (4).ywf` proves the latest workflow arrow condition UI supports independent operand modes for both sides of each condition row:

- `type: 0`: direct/static value, selected option value, or date-picker value
- `type: 1`: direct field/workflow-variable selector, observed on the left side
- `type: 2`: expression editor operand, used on either side for variables, calculations, functions, or dynamic comparisons

For new generated workflow conditions, prefer these wrapper object shapes over legacy frontend `<input type="button" ...>` expression-button strings. Use `left.type = 1` and `right.type = 0` for simple routing such as `ApplicationType == Family`. Use `type = 2` on whichever side needs an expression editor, such as date tenure, numeric thresholds, calculated quota cycle, or expression-to-expression comparisons.

See `docs/collection-03.md#workflow-transition-condition-patterns-study` and `docs/collection-04.md#yeeflow-workflow-generation-rules` for examples.

For direct Workflow-variable-left plus direct-value-right conditions, derive the Condition editor group from the variable type, not from the visible label:

- User, Department/groupselect, Location, Cost Center, Lookup, Metadata, Dictionary, List, and Text variables use `group: "string"` and `s.` operators.
- Number/currency/percent variables use `group: "number"` and `n.` operators.
- Boolean variables use `group: "boolean"` and `b.` operators.
- Date/time variables use `group: "datetime"` and `dt.` operators. Supported direct-value operators are `dt.=`, `dt.!=`, `dt.>=`, `dt.<=`, `dt.>`, `dt.<`, `isNull`, and `isNotNull`; direct right values are date/time strings and null checks use `right: null`.
- File/Image/Signature variables use `group: "general"` with `isNull` or `isNotNull`.

Use `right.type = 0` for fixed values and `right.type = 2` for Expression editor values. Choose `right.type = 2` only for dynamic or processed comparisons such as string concatenation, numeric calculation, Boolean `iif`, current user, applicant attributes through `getUserAttr`, cross-variable comparison, or date functions such as `dateAdd(now(), "month", 1)`. The `right.value` for type `2` must be a non-empty expression-token array.

For grouped Condition editor logic, generate at most two layers. A top-level condition group row uses `left: null`, `op: "isNull"`, `right: null`, and a non-empty `conditions[]` child array. The parent row's `pre` connects the group to the surrounding top-level rows; child rows use their own `pre` values. Use child `and` rows for grouped range/all-required clauses and child `or` rows for alternative values. Third-level nested `conditions[]` groups are invalid.

For Assignment Task outgoing `Approved`, `Rejected`, and `Completed` lines, do not use a simple Workflow variable named `Outcome`. The left side must be the export-style task Outcome expression-button for the source task (`type = task`, `param.defid = <source task id>`, `prop = Outcome`), `op` must be `s.=`, and the right side must be the matching `Task outcome:*` expression-button.

The focused reference is documented in `docs/studies/collection-03.md#workflow-condition-editor-direct-value`; generated-final validators should block mismatched group/operator/value shapes before wrapper build.
- Stop if external/credential actions contain literal secrets or unresolved connection IDs.
- Do not generate a new app solely from this reference; use it to validate a separately decomposed workflow requirement.

---

<a id="workflow-transition-condition-patterns-study"></a>

<!-- source: docs/collection-03.md#workflow-transition-condition-patterns-study -->

# Workflow Transition Condition Patterns Study

Source export studied read-only: `<downloads>/Implant Application Request (4).ywf`

Date studied: 2026-05-17

Scope: approval form `Implant Application Request`, workflow `SequenceFlow` named `Family Application Occupies Quota`, the original generated condition row, and four manually configured condition rows added by the user. The uploaded export was not modified.

## Transition Located

- SequenceFlow resource id: `efi-flow-0002a`
- Name: `Family Application Occupies Quota`
- Source: `efi-node-set-0002`
- Target: `efi-node-occupy-usage-0006`
- Business intent: route family applications through the quota occupation step before HR Review.

The export stores workflow arrow conditions in:

```text
childshapes[].properties.conditioninfo[]
```

Each condition row includes:

- `key`: condition row id
- `pre`: joiner such as `and`
- `left`: left operand
- `op`: operator, for example `s.=` for string equals
- `right`: right operand
- `group`: value group such as `string` in the newer rows

## Operand Type Legend

The latest condition UI can configure each side independently.

| Export type | Meaning | Observed side | Notes |
|---|---|---|---|
| legacy string HTML | Old generated expression-button string | left, sometimes right in older task outcome rows | Example: `<input type="button" ... value="Workflow Variables:Application Type">`. Existing exports may contain this, but new generation should prefer wrapper objects when possible. |
| `type: 0` | Direct/static value mode | right | Used for direct text/static value, selected option value, or date picker value. The value is stored in `value`. |
| `type: 1` | Direct field/workflow-variable selector | left | Used when the left side is selected from the workflow variable/field dropdown. The selected variable token is stored in `value`. |
| `type: 2` | Expression editor mode | left or right | Used when either side is switched to the expression editor. The expression token array is stored in `value`. |

## Rows Studied

| Row | Pattern | Left operand | Right operand | Use case | Export-backed JSON notes |
|---:|---|---|---|---|---|
| 0 | Legacy generated simple condition | HTML expression-button string for `Workflow Variables:Application Type` | Primitive string `Family` | Historical simple routing generated by Codex | Works as older/simple style but does not expose the left/right mode metadata from the latest UI. Avoid for new generated conditions when wrapper object shape is available. |
| 1 | Direct variable == static/selected value | `{ "type": 1, "value": { "exprType": "variable", "valueType": "text", "id": "ApplicationType", "type": "expr" } }` | `{ "type": 0, "value": "Family" }` | Simple routing such as `ApplicationType == Family` or `HasCustomPackageProduct == Yes` | Use this for a single workflow variable compared with a literal/option/date value. |
| 2 | Expression left == expression right | `{ "type": 2, "value": [Workflow Variables:Application Type token] }` | `{ "type": 2, "value": [{ "type": "str", "value": "Family" }] }` | Both sides are expression-editor operands; useful when either side may later become computed | Expression tokens include full `name` metadata in the manual export. |
| 3 | Expression left == expression right, reversed sample | `{ "type": 2, "value": [{ "type": "str", "value": "Family" }] }` | `{ "type": 2, "value": [Workflow Variables:Application Type token] }` | Demonstrates that both sides can be expression editor operands independently | Valid shape evidence, but business generation should keep the variable on the left for readability unless a computed-left rule requires otherwise. |
| 4 | Expression left == static/selected value | `{ "type": 2, "value": [Workflow Variables:Application Type token] }` | `{ "type": 0, "value": "Family" }` | Left side needs expression editor, right side is a static/option/date value | Use when the left side is calculated or selected via expression editor and the right side is literal. |

## Exact Example Shapes

### Direct Variable Left + Static Right

```json
{
  "key": "ac709892-7ebe-4960-b780-978c94b35921",
  "pre": "and",
  "left": {
    "type": 1,
    "value": {
      "exprType": "variable",
      "valueType": "text",
      "id": "ApplicationType",
      "type": "expr"
    }
  },
  "op": "s.=",
  "right": {
    "type": 0,
    "value": "Family"
  },
  "group": "string"
}
```

### Expression Left + Expression Right

```json
{
  "key": "ddb1e9fa-2958-4bc4-878c-fe540bfeb558",
  "pre": "and",
  "left": {
    "type": 2,
    "value": [
      {
        "exprType": "variable",
        "valueType": "text",
        "id": "ApplicationType",
        "type": "expr",
        "name": "Workflow Variables:Application Type"
      }
    ]
  },
  "op": "s.=",
  "right": {
    "type": 2,
    "value": [
      {
        "type": "str",
        "value": "Family"
      }
    ]
  },
  "group": "string"
}
```

### Expression Left + Static Right

```json
{
  "key": "951a7cca-ae11-49a2-b388-45957411d5e2",
  "pre": "and",
  "left": {
    "type": 2,
    "value": [
      {
        "exprType": "variable",
        "valueType": "text",
        "id": "ApplicationType",
        "type": "expr",
        "name": "Workflow Variables:Application Type"
      }
    ]
  },
  "op": "s.=",
  "right": {
    "type": 0,
    "value": "Family"
  },
  "group": "string"
}
```

## Generation Rules

Use direct variable left + static/option/date right when:

- routing depends on one selected workflow variable or field
- examples: `ApplicationType == Family`, `HasCustomPackageProduct == Yes`
- use `left.type = 1`, `right.type = 0`, and the appropriate `group`

Use direct variable left + expression right when:

- the left side is a selected workflow variable and the right side is dynamic
- examples: `ApplicationAmount >= ApprovalThreshold`, `QuotaCycleNo == calculated current cycle`
- use `left.type = 1`, `right.type = 2`

Use expression left + static/option/date right when:

- the left side needs calculation before comparison
- examples: `dateDiff(BoardingDate, now(), "year", []) > 0`, `year(ApplicationDate) >= 2026`
- use `left.type = 2`, `right.type = 0`

Use expression left + expression right when:

- both sides are dynamic
- examples: `year(RequestDate) >= year(BoardingDate)`, `TotalApplicationAmount > RemainingQuota`
- use `left.type = 2`, `right.type = 2`

Avoid legacy/simple HTML-button string conditions when:

- either side needs expression editor
- comparing dates by year/month
- comparing calculated values
- comparing lookup/autofill-derived values
- branch coverage requires fallback logic
- generating new workflows where the wrapper object shape is available

## Branch Coverage Guidance

- Every multi-branch node must cover all meaningful outcomes.
- Routing variables must be required, auto-derived, or protected by fallback branches.
- Empty/null/unexpected values must not cause dead-end workflows.
- If a branch uses computed values, prefer expression operands instead of forcing intermediate variables unless persistence/debugging requires the intermediate variable.
- For HR Review custom package routing, use a clear standard branch and a safe fallback branch. Example: `Approved + HasCustomPackageProduct == No` routes to standard persistence; `Approved + HasCustomPackageProduct != No` routes to Finance/Benefits Review so `Yes`, empty, and unexpected values are not dead-ended.

## Validation Notes

Add warning-level checks for generated workflows:

- condition rows using legacy HTML button strings when a new operand wrapper would be safer
- operand wrapper objects with missing `type` or `value`
- `type: 2` expression operands whose `value` is not an expression-token array
- `type: 1` direct selectors whose `value` is not a variable/field token object
- duplicated or contradictory condition rows on the same transition
- multi-branch nodes that only cover yes/no and lack empty/unexpected fallback where the routing variable can be blank

## Runtime Status

This is an export-backed structure study only. No new runtime test package was generated in this pass.

---

<a id="yap-app-generation-first-test-plan"></a>

<!-- source: docs/collection-03.md#yap-app-generation-first-test-plan -->

# Yeeflow `.yap` App Generation First Test Plan

This document defines the first safe `.yap` generation test for Codex.

Recommended test app:

**Department Access Management**

Planned components:

1. Departments data list
2. Department Access Requests data list
3. Department Access Request approval form

This plan is design-only. Do not generate or build a `.yap` package until the app-level wrapper builder and validators are ready.

## 1. Why This Is The Right First `.yap` Test

This is the right first `.yap` generation test because it combines the smallest useful set of app-level components:

- a reference data list: Departments
- a transactional/storage data list: Department Access Requests
- an approval form and workflow that reads from one list and writes to another

It uses patterns already proven independently:

- generated `.ydl` data lists
- generated data-list custom forms
- related data-list lookup fields
- generated `.ywf` approval forms
- approval-form lookup controls
- lookup additional field mapping
- `ContentList` persistence from approval workflow to data list
- app/list/field/layout/form ID remapping through `ReplaceIds`

It avoids the sensitive and higher-variance parts of `.yap` exports:

- dashboards
- data reports
- form reports
- AI Agents
- Copilots
- Connections
- Knowledges
- document libraries
- document generation
- external HTTP actions

The key thing this test should prove is that an app-level `.yap` package can remove the staged standalone import/export remapping problem. Because the Departments list, Department Access Requests list, and approval form are packaged together, internal lookup and `ContentList` references should use generated IDs consistently and be remapped together during import.

The v5 sandbox test proved the base app shell, navigation, child lists, and approval form can import and open. Export-back also proved app-level internal lookup sample values remap consistently when both source and target lists are packaged in the same `.yap`.

## 2. Target App Structure

Root application:

- App title: `Department Access Management`
- `AppID`: `41`
- root `ListSetID`: generated large numeric string ID for the sandbox package
- root `Data.Item`: app/listset resource with `ListModel.Type = 1024`

Child resources:

1. `Departments`
   - data list
   - stores department reference data
   - contains Department Name and Department Code
   - may include safe sample rows

2. `Department Access Requests`
   - data list
   - stores approved request records
   - includes a Department lookup field pointing to Departments
   - uses the proven Asset Inventory v5 custom form registration pattern

3. `Department Access Request`
   - approval form/workflow in `Data.Forms[]`
   - request page includes Department lookup and readonly Department Code
   - approval workflow writes approved records to Department Access Requests

## 3. Required Resource Graph

Required nodes:

- `app:Department Access Management`
- `list:Departments`
- `field:Departments.Title`
- `field:Departments.Text1` or equivalent Department Code field
- `list:Department Access Requests`
- `field:Department Access Requests.Department`
- `field:Department Access Requests.Department Code`
- `form:Department Access Request`
- request page
- approval page
- workflow `ContentList` node

Required edges:

- app -> Departments
- app -> Department Access Requests
- app -> Department Access Request approval form
- Department Access Requests.Department lookup -> Departments
- Department Access Requests.Department lookup display field -> Departments.Title
- approval form Department lookup -> Departments
- approval form Department lookup display/sort field -> Departments.Title
- approval form lookup additional source field -> Departments.Department Code
- approval form lookup additional target variable -> DepartmentCode
- approval form `ContentList` -> Department Access Requests
- approval form `ContentList` mappings -> Department Access Requests target fields

No edges should point to external lists or external apps in this first test.

## 4. ID And `ReplaceIds` Strategy

All local package IDs should be generated as large numeric string IDs and included in `Resource.ReplaceIds`.

Local IDs include:

- root app/listset ID
- child list IDs
- field IDs
- layout IDs
- custom form resource IDs
- approval form process/workflow IDs
- approval form page IDs where Yeeflow expects remapping
- report/form keys if present, though reports are excluded from this test
- sample record `ListDataID` values if sample data is included

Internal references must use the generated IDs consistently before packaging:

- Department Access Requests.Department lookup must point to the generated Departments ListID.
- Approval form Department lookup must point to the generated Departments ListID.
- Approval form additional field mapping must point to the generated Departments Department Code field ID/name.
- Approval workflow `ContentList` must point to the generated Department Access Requests ListID.
- `ContentList` target mappings must use generated Department Access Requests field names/IDs consistently.

No external dependency IDs should be included in `ReplaceIds`.

Because both data lists and the approval form are packaged together, Yeeflow should remap local IDs as one app graph. This should avoid the standalone `.ydl` problem where a dependent list points to a pre-import ListID that Yeeflow later remaps.

Internal lookup sample data rule:

- When a lookup target list is packaged inside the same `.yap`, target sample record IDs are local package IDs.
- Include those target sample record IDs in `ReplaceIds`.
- Dependent sample rows may store the target sample `ListDataID` as a plain string.
- Export-back from `Department Access Management_v5` confirmed Yeeflow remapped both target sample record IDs and dependent lookup values consistently.
- If the grid display appears blank while export-back values are correct, treat it as a runtime display/index/cache issue unless row forms or newly edited rows also fail.

## 5. Normalized App Spec

Proposed normalized spec shape:

```json
{
  "app": {
    "title": "Department Access Management",
    "appId": "41",
    "listSetId": "__GENERATED_LOCAL_LISTSET_ID__",
    "description": "Sandbox generated app for department access request management."
  },
  "dataLists": [
    {
      "key": "departments",
      "title": "Departments",
      "listId": "__GENERATED_DEPARTMENTS_LIST_ID__",
      "fields": [],
      "views": [],
      "customForms": [],
      "sampleData": []
    },
    {
      "key": "departmentAccessRequests",
      "title": "Department Access Requests",
      "listId": "__GENERATED_REQUESTS_LIST_ID__",
      "fields": [],
      "views": [],
      "customForms": [],
      "sampleData": []
    }
  ],
  "approvalForms": [
    {
      "key": "DAR",
      "title": "Department Access Request",
      "workflowType": 2,
      "defResource": {}
    }
  ],
  "relationships": [
    {
      "type": "dataListLookup",
      "source": "departmentAccessRequests.Department",
      "targetList": "departments",
      "displayField": "departments.Title"
    },
    {
      "type": "approvalFormLookup",
      "source": "DAR.Department",
      "targetList": "departments",
      "displayField": "departments.Title"
    },
    {
      "type": "lookupAdditionalField",
      "sourceField": "departments.DepartmentCode",
      "targetVariable": "DepartmentCode"
    },
    {
      "type": "workflowContentList",
      "sourceForm": "DAR",
      "targetList": "departmentAccessRequests"
    }
  ],
  "replaceIds": [],
  "dependencies": []
}
```

The first test should have an empty `dependencies` array except for runtime assumptions such as importing into a sandbox Yeeflow tenant.

## 6. Validation Pipeline

Before any `.yap` wrapper build:

1. Validate each generated child data list resource with `validate-ydl-list.js`.
   - Use generator/final mode for final child resources.
   - Confirm fields, views, custom forms, lookup metadata, and sample data.

2. Validate the approval form Def with `validate-ywf-def.js`.
   - Use final mode.
   - Confirm request page, approval page, variables, workflow graph, action panel/history, lookup control, additional field mapping, and `ContentList`.

3. Validate the assembled app package with `validate-yap-package.js`.
   - Use generator/final mode.
   - Confirm root app/ListSet, child resources, forms, `ReplaceIds`, and package structure.

4. Validate the assembled app relationship graph with `validate-yap-graph.js`.
   - Use generator/final mode.
   - Confirm lookup targets, display fields, approval-form lookup source, additional field mapping, `ContentList` target list, and mapped fields.

All four validation layers must pass before wrapper build.

## 7. Build Pipeline

Future build pipeline:

1. Generate decoded Departments child list resource.
2. Generate decoded Department Access Requests child list resource.
3. Generate decoded Department Access Request approval form `DefResource`.
4. Assemble decoded `.yap` `Resource.Data`:
   - `Data.Item`: root app/listset
   - `Data.Childs[]`: Departments and Department Access Requests
   - `Data.Forms[]`: Department Access Request approval form
   - reports/dashboards/modules omitted
5. Validate components.
6. Validate assembled app package.
7. Validate app graph.
8. Build `.yap` wrapper later with a future `build-yap-wrapper.js`.
9. Do not build if `build-yap-wrapper.js` does not exist or if any validator fails.

## 8. Stop Conditions

Stop before final `.yap` generation if any of these are true:

- unresolved IDs remain
- placeholders remain in final mode
- Departments lookup target is missing
- Department Code source field is missing
- Department Access Requests lookup target is missing
- approval form lookup source list or display field is missing
- approval form additional field mapping cannot resolve
- approval workflow `ContentList` target list is missing
- `ContentList` target fields are missing
- structural validation fails
- graph validation fails
- local generated IDs are missing from `ReplaceIds`
- external IDs appear in `ReplaceIds`
- sensitive resources are accidentally included
- root app/ListModel metadata is unclear
- dashboards, reports, AI resources, document libraries, or external HTTP actions are requested before the base app test passes

## 9. Expected Test Checklist

After a future `.yap` build/import:

1. App imports successfully.
2. `Departments` list appears.
3. `Departments` sample data appears.
4. `Department Access Requests` list appears.
5. `Department Access Requests.Department` lookup resolves to Departments.
6. Department Access Request approval form imports.
7. Approval form publishes.
8. Request page Department lookup works.
9. Department Code auto-fills from the selected Department.
10. Line manager approval route works.
11. Rejection path ends correctly.
12. Approved path creates a Department Access Requests record.
13. Created record preserves:
    - Request No.
    - Applicant
    - Submission Date
    - Department
    - Department Code
    - Access Reason
    - Needed By Date
    - Approval Status
14. Export the app back and compare:
    - remapped IDs
    - lookup references
    - `ContentList` target references
    - custom form registration
    - approval form page registration
    - `ReplaceIds`

## 10. Tool Roadmap

Recommended next tools:

1. `build-yap-wrapper.js`
   - Wrap decoded app `Resource.Data` into a `.yap` using the confirmed gzip/base64 `Resource` format.
   - Run round-trip validation before writing or immediately after writing.

2. `compare-yap-export-back.js`
   - Compare generated `.yap` decoded data with exported-back Yeeflow `.yap`.
   - Identify remapped IDs, dropped resources, rewritten metadata, and changed references.

3. `generate-yap-from-spec.js`
   - Later-stage generator that converts normalized app specs into decoded `.yap` drafts.
   - Should only be introduced after the Department Access Management hand-built first test succeeds.

The immediate next build target should be `build-yap-wrapper.js`, but only after enough decoded app package structure has been assembled and validated locally.

## 11. v5 Baseline Result

`Department Access Management_v5` is the first successful generated `.yap` baseline.

Confirmed:

- import popup showed logo, name, and description
- app installed into the workspace
- app opened from the workspace card
- root app header and navigation rendered
- Type `103` overview page opened
- `Departments` list opened and displayed 3 sample rows
- `Department Access Requests` list opened and displayed sample request rows
- `Department Access Request` approval form opened and rendered the request page

The successful app shell required:

- non-null wrapper `IconUrl`
- root `CustomType = ""`
- root `Perm = 0`
- root `WorkspaceID`
- populated root `LayoutView` navigation
- at least one root Type `103` app page layout
- `AppTags`, `AppMetadatas`, and `AppComponents` arrays
- non-empty `AppThemes`
- populated root `CreatedBy` and `ModifiedBy`

Type `103` root app pages use a distinct resource-registration rule:

- `LayoutID` is included in `ReplaceIds`
- `LayoutInResources[0].ID` and `RefId` are separate resource IDs
- those `LayoutInResources` resource IDs are excluded from `ReplaceIds`

Remaining runtime follow-up:

- If the `Department Access Requests.Department` lookup display is blank in the grid, refresh/reopen the app, open a sample row form, and create or edit one row to confirm whether runtime lookup display is healthy. Do not change the package sample-value rule unless export-back or form behavior proves the data is broken.

## 12. Visitor Access Management v5 Fresh-Compatible Follow-Up

`visitor-access-management.v5-fresh-compatible.yap` is the first successful Visitor Access Management app-level baseline.

Learning sequence:

- Expanded Visitor app packages failed during import.
- Isolation packages failed.
- A control package based on the working Department Access Management v5 structure imported.
- A v5-compatible Visitor package that reused the old `206...` local ID family and `DAR` key imported but opened as a blank app.
- The fresh-compatible v5 package imported and opened after using a fresh local ID family and fresh approval form key `VAV`.

Fresh ID policy for future app-level tests:

- Every generated `.yap` app test should use a fresh local ID family.
- Every generated approval form inside a generated `.yap` should use a fresh FlowKey/form key.
- Do not reuse local generated IDs from an app that has already been imported into the sandbox.
- Reused local IDs can import but leave the app blank or components detached.
- Validators and generators should warn or fail on known ID-family/key reuse when prior generated/imported artifacts are detectable.

Visitor Access v6 expansion plan:

- Start from the v5 fresh-compatible baseline.
- Add exactly one extra Visitor request field to the list and approval form.
- Validate and build.
- Import-test and export back.
- Repeat one field at a time until the expanded schema is proven.

## 13. Visitor Access v6.1 One-Field Expansion Result

The first successful expansion after the v5 fresh-compatible baseline is `Visitor Access Management v6.1`.

What was added:

- `Visitor Name`
- data-list field slot: `Visitor Access Requests.Text8`
- approval form variable: `VisitorName`
- request page control
- approval page readonly control
- workflow `ContentList` mapping: `VisitorName -> Text8`
- sample row values

Failure and fix sequence:

- v6 added `Visitor Name`, but the approval form was missing after import.
- Root cause: app-level approval form `Data.Forms[0].ListID` was set to `ProcModelID`.
- Correct rule: app-level approval form registration must keep `Data.Forms[].ListID = 0`.
- v6 formfix restored `ListID = 0`, but reused the same `210...` ID family and `VAN` key after the broken v6 package had already been imported.
- That reuse produced a blank/detached app shell.
- v6.1 used a fresh `211...` ID family and fresh form key `VAW`.
- v6.1 imported, opened, showed navigation, showed the approval form, and rendered `Visitor Name`.

Updated expansion rule:

- Every import-test package must get a fresh local ID family, even if the previous package failed.
- Every import-test approval form must get a fresh FlowKey/form key.
- Keep app-level approval form `ListID = 0`.
- Add one field/change at a time and preserve proven field slots unless the test is specifically about a new slot.

## 14. Visitor Access v7 Expansion Result

`Visitor Access Management v7` successfully expanded the working v6.1 baseline by one additional text field.

v7 change:

- Added `Visitor Company` as `Visitor Access Requests.Text9`.
- Used fresh ID family `212...`.
- Used fresh FlowKey/form key `VAX`.
- Preserved app-level approval form `Data.Forms[].ListID = 0`.
- Added `VisitorCompany -> Text9` to workflow `ContentList`.

Confirmed result:

- App imported and opened correctly.
- App navigation rendered.
- Approval form appeared.
- `Visitor Name` rendered.
- `Visitor Company` rendered.
- `ContentList` persisted `VisitorCompany` to `Text9`.

Expansion rule confirmed:

- One-field expansion from a known-good generated `.yap` baseline is the safe path.
- Text field expansion into a new `Text*` slot is safe when the data list, custom form, approval form request page, approval page readonly view, sample rows, and `ContentList` mapping are updated consistently.

Current proven Visitor request schema:

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

## 15. Visitor Access v8-v10 Expansion Results

The Visitor Access Management app has now passed three more one-field expansion tests from the latest known-good baseline.

| Version | Field added | Slot | ID family | FlowKey | Result |
| --- | --- | --- | --- | --- | --- |
| v8 | Access Area | `Text10` | `213...` | `VAY` | Imported/opened; field rendered and persisted. |
| v9 | Host Employee | `Text11` | `214...` | `VAZ` | Imported/opened; field rendered and persisted. |
| v10 | Visitor Contact | `Text12` | `215...` | `VBA` | Imported/opened; field rendered and persisted. |

The v10 package confirms the current text-field expansion chain:

- v6.1 `Visitor Name -> Text8`
- v7 `Visitor Company -> Text9`
- v8 `Access Area -> Text10`
- v9 `Host Employee -> Text11`
- v10 `Visitor Contact -> Text12`

Current proven Visitor request schema:

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
- Update the list field definition, list views where useful, custom list form, approval request page, approval readonly page, workflow variable, `ContentList` mapping, and sample rows together.
- Validate with `validate-yap-package.js`, `validate-yap-graph.js`, and `validate-ywf-def.js` before wrapper build.

Next recommended tests:

- Convert `Access Area` from a simple text field to a choice/dropdown.
- Test `Host Employee` as a user picker or lookup.
- Add dashboard/report resources only after the core app remains stable.

## 16. Visitor Access v11 Multi-Type Expansion Result

`Visitor Access Management v11` is the first successful generated multi-field, multi-type app expansion.

v11 started from the working v10 baseline and added five persisted fields in one package:

| Field | Slot | Type |
| --- | --- | --- |
| Visitor Email | `Text13` | text/input |
| Visitor Phone | `Text14` | text/input |
| Number of Visitors | `Decimal1` | Decimal/input_number |
| Access Type | `Text15` | Text/radio dropdown |
| Requires Escort | `Bit1` | Bit/switch |

Successful package settings:

- Fresh ID family: `216...`
- Fresh FlowKey/form key: `VBB`
- App-level approval form `Data.Forms[].ListID = 0`
- No unresolved placeholders
- No dashboards, reports, AI resources, connections, document libraries, list workflows, custom code, or external actions

v11 also included `Escort User` as an approval-form-only conditional/display field. It is shown when `RequiresEscort == true` using the target control `attrs.control_display[]` rule and is not persisted in v11.

Confirmed ContentList mappings:

| Source variable | Target field |
| --- | --- |
| `VisitorEmail` | `Text13` |
| `VisitorPhone` | `Text14` |
| `NumberofVisitors` | `Decimal1` |
| `AccessType` | `Text15` |
| `RequiresEscort` | `Bit1` |

The v11 result changes the expansion guidance:

- One-field expansion remains the safest way to isolate an unknown field/control type.
- Multi-field expansion is acceptable after the individual field/control types have been proven from export evidence and validator checks.
- A multi-field package must still use a fresh ID family and fresh FlowKey.
- Data-list definitions, custom list form controls, approval request controls, approval readonly controls, workflow variables, sample values, and `ContentList` mappings must be updated as a consistent set.

Current proven Visitor schema:

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
| Visitor Email | `Text13` |
| Visitor Phone | `Text14` |
| Number of Visitors | `Decimal1` |
| Access Type | `Text15` |
| Requires Escort | `Bit1` |
| Visit Date | `Datetime2` |

---

<a id="yeeflow-application-design-system"></a>

<!-- source: docs/collection-03.md#yeeflow-application-design-system -->

# Yeeflow Application Design System

This is the standard design system for generated Yeeflow application packages.

It consolidates:

- the focused layout export `UI and UX design (1).yap`
- root design tokens from `<downloads>/root_styles.txt`
- the Yeeflow Design System Alignment skill
- proven generated-app baselines for Visitor Access Management, Department Access Management, Knowledge Base Phase 1, Service Desk Pro dashboard stages, and Heep Hong IT eWorkflow Option A v9

The source `.yap` and `root_styles.txt` were studied read-only. Do not bundle either source artifact into generated apps, do not modify the originals, and do not inject the full root stylesheet into generated packages.

## Design Intent

Generated Yeeflow apps should feel calm, structured, trustworthy, operational, polished, and enterprise-ready.

Design quality should come from:

- clear page structure
- predictable navigation
- restrained color
- neutral surfaces
- spacing and alignment
- reusable section patterns
- realistic business labels and sample data

Avoid loud gradients, decorative effects, one-off palettes, hype-heavy AI wording, and visible implementation labels.

## Source Priority

Use sources in this order:

1. Runtime-proven Yeeflow export structure.
2. The `UI and UX design (1).yap` shell pattern.
3. Root style tokens from `root_styles.txt`.
4. Proven generated-app baselines.
5. Yeeflow brand/design-system guidance.

When these conflict, preserve proven Yeeflow runtime structure first. For example, the mini UI/UX export proves the `Main` / `Content` shell, but generated data lists must still preserve the stricter native `Title` metadata rule proven by later app baselines.

## Standard Page Shell

Generated dashboard, data-list custom form, and approval-form pages should use:

- hidden header bar by default where proven for dashboard pages
- full-width content area for forms: `attrs.container.cw = "2"`
- zero page padding with `--sp--s0`
- top-level container named `Main`
- child container named `Content`
- semantic section containers inside `Content`

Designer-facing names are stored in `nv_label`. Do not add visible text labels only to describe layout containers.

## Page Background Standard

Full-page background color belongs on the page/form background setting, not on the top-level `Main` container.

- dashboard pages: set the background on the embedded page JSON, such as `page.attrs.background`
- data-list custom forms: set the background on the custom form page JSON, such as `form.attrs.background`
- approval submission and task pages: set the background on `page.formdef.attrs.background`

`Main` is a structural/layout parent. It should provide layout direction, gap, alignment, and containment, but it should not simulate the full page background. If a specific visual section, card, header, summary panel, or content band needs its own background, set that background on that section/card/header container.

## Application Standards

Every generated application should include:

- complete root app shell metadata
- non-null wrapper icon
- populated root navigation
- clear business navigation names
- complete `Data.AppTags`, `Data.AppMetadatas`, `Data.AppComponents`, and `Data.AppThemes`
- fresh local ID family
- fresh approval form key when forms are included
- no raw exports, secrets, credentials, zip files, or environment files in commits

Navigation names should describe user work, not implementation:

- use `Overview`, `Requests`, `Catalog`, `Submit Request`, `Settings`, `Help Guide`
- avoid `Data List`, `Page 1`, field slot names, IDs, or internal type names in production-like generated apps

Navigation styling should preserve contrast. When the root header uses `LayoutView.attrs.appearance.bgc` and `LayoutView.attrs.appearance.color`, set the navigation menu to the inverse pair:

- `LayoutView.attrs["navigator-menu"].bgc = LayoutView.attrs.appearance.color`
- `LayoutView.attrs["navigator-menu"].color = LayoutView.attrs.appearance.bgc`

For the standard generated app shell, the header background is `var(--c--primary-light)`, the header text/icon color is `var(--c--primary)`, the nav background is `var(--c--primary)`, and nav text/icons are `var(--c--primary-light)`.

## Dashboard Standards

Dashboard pages should use:

- Type `103` root app page layouts
- `Ext2 = "{\"src\":true}"`
- embedded page JSON in `LayoutInResources[0].Resource`
- `attrs.hideHeaderAll = true`
- zero page padding
- page-level background on embedded page attrs when a full-page background is needed
- `Main` -> `Content`
- optional `Page header`, `Summary section`, `Body section`, `Collection section`, and `Empty state` containers

KPI cards, lists, filters, Collection controls, and operational cards should live inside `Content`.

Use Collection controls for repeatable dashboard content when the source list is included in the package and the binding has been proven. Collection item templates should use meaningful `nv_label` values such as `Collection item`, `Table row`, `Status badge`, and `KPI card`.

## Data List Form Standards

Every generated data list should include at least:

- `Edit Item`
- `View Item`

Display settings should map:

- New item -> `Edit Item`
- Edit item -> `Edit Item`
- View item -> `View Item`

Both forms should use:

- `attrs.container.cw = "2"`
- zero padding
- page-level background on the custom form attrs when a full-page background is needed
- `Main` -> `Content`

`Edit Item` is input optimized. `View Item` is readonly/display optimized. Both should use the same shell so generated apps feel coherent.

Native `Title` metadata remains a hard runtime rule:

- `FieldName: "Title"`
- `Status: 0`
- `IsSystem: true`
- `IsIndex: true`
- `FieldIndex: 0` where generator controls metadata

## Approval Form Standards

Submission pages and task pages should use:

- `attrs.container.cw = "2"`
- zero padding
- page-level background for full-page form backgrounds
- `Main`
- `Content`
- `Form body`
- `Form bottom`

`Form body` contains requester fields, readonly mirrored fields, reviewer decision fields, and section containers.

When a submission form has a top request summary, use a `Form header` container above the main field sections. `Form header` should provide background, border, rounded corners, and overflow clipping. Put `Request summary panel` and `Request metric row` inside it.

The manually improved IT Hardware CAPEX Runtime V2 export proved these additional form-quality defaults:

- use `page.formdef.attrs.background` for full-page background instead of `Main.attrs.common.background`
- apply the same page-background rule to dashboard pages and data-list custom forms
- use `attrs.common.css` on a specific container when a gradient/background image is needed and native settings cannot express it
- generate `heading` and `text-editor` controls with inline width positioning by default
- wrap section icons in square centered badge containers
- put normal fields inside two-column `flex_grid` controls
- keep textarea, rich text, and list/sublist controls full row
- use native `calculated` controls for calculated values such as `Subtotal = Quantity * Unit Price`

`Form bottom` appears at the end of `Content` and contains by default:

1. `workflowControlPanel`
2. `workflowHistory`

Task pages should mirror submitted request fields as readonly where appropriate. Workflow logic should not be changed by visual-improvement work unless explicitly requested.

### Form Actions

Generated approval forms may use native form actions when an export-backed pattern exists.

Form Actions Phase 1 proves:

- inline `action_button` controls with native style codes
- `action_button.attrs.control_action` for button click actions
- `page.formdef.formAction.onLoad` for page load initialization
- `page.formdef.actions[]` as the form action definition list
- temp variables under `variables.tempVars[]`
- `setvar` and `confirm` action steps

Use form actions for front-end form behavior such as default values, confirmation dialogs, and temporary UI state. Keep workflow actions for process/backend behavior. Apply the same design rules to action buttons: inline width by default, meaningful `nv_label`, token-aligned styling, and no page background on `Main`.

## Token Standards

Use root style tokens as design guidance. Preserve token names exactly:

- primary: `--c--primary`
- secondary: `--c--secondary`
- success: `--c--success`
- warning: `--c--warning`
- danger: `--c--danger`
- neutral: `--c--neutral`
- background: `--c--background`
- text: `--c--text`
- font sizes: `--fs--base`, `--fs--h6`, `--fs--h5`, `--fs--h4`
- spacing: `--sp--s100`, `--sp--s150`, `--sp--s200`, `--sp--s300`, `--sp--s400`
- weights: `--fw--regular`, `--fw--medium`, `--fw--semi-bold`, `--fw--bold`

Do not require token references where Yeeflow exports resolved hex values. Validator checks for token alignment should be warnings until runtime proof says otherwise.

## Style Defaults

Use these defaults for generated app surfaces:

| Use | Default |
| --- | --- |
| page background | `--c--neutral-light` or `--c--background` |
| section/card background | `--c--background` |
| neutral border | `--c--neutral-light-active` |
| primary action | `--c--primary` |
| success status | `--c--success` / `--c--success-light` |
| warning status | `--c--warning` / `--c--warning-light` |
| danger status | `--c--danger` / `--c--danger-light` |
| body text | `--c--text` or `--c--text-normal` |
| muted text | `--c--neutral-dark` |
| body font | `--fs--base` |
| helper text | `--fs--s` or `--fs--xs` |
| section heading | `--fs--l` or `--fs--h6` |
| page heading | `--fs--h3` through `--fs--h1` |
| field gap | `--sp--s150` or `--sp--s200` |
| card padding | `--sp--s200` or `--sp--s300` |
| section gap | `--sp--s300` or `--sp--s400` |
| control radius | `6px` |
| card radius | `8px` |
| large panel radius | `12px` |

Prefer neutral borders before shadows. Use shadows sparingly.

## Naming Standards

Use consistent `nv_label` names:

- `Main`
- `Content`
- `Form header`
- `Page header`
- `Summary section`
- `Body section`
- `Form body`
- `Form bottom`
- `Action panel`
- `Flow history`
- `Collection section`
- `Collection`
- `Collection item`
- `Table header`
- `Table row`
- `Status badge`
- `KPI card`
- `Field group`
- `Readonly section`
- `Empty state`

Visible headings should be business-friendly and concise, such as `Request details`, `Approval decision`, `Open tickets`, or `Recent articles`.

## Baseline Lessons

Use these proven lessons:

- Department Access Management v5 proves complete root app shell, navigation, related lists, lookup relationships, approval form, and `ContentList` persistence.
- Visitor Access Management v5 and v11 prove fresh ID/form key discipline and richer field/control expansion.
- Knowledge Base Phase 1/v4 proves dashboard Collections can render local list sample rows when scope is kept conservative.
- Service Desk Pro Stage N proves staged dashboard expansion, KPI bindings, filters, Settings/Help pages, and local source lists.
- Heep Hong IT eWorkflow Option A v9 proves the five-list purchase requisition structure, workflow action validation, and the need to defer risky detail-row persistence until proven.
- Design System Request Tracker DSX proves the first combined design-system package: dashboard, local data list, Edit/View custom list forms, approval form controls, workflow action validation, manual form publish, submit/approve path, and `ContentList` persistence to a local list.

## Validator Policy

Generated-app design-system validation should warn for:

- missing `Main`
- missing `Content`
- full-page-like background on `Main`
- missing page-level background when `Main` carries a background
- missing meaningful `nv_label`
- missing `Edit Item` / `View Item` custom list forms
- New/Edit/View display settings not following standard
- approval page missing `Form body` / `Form bottom`
- Action Panel / Flow History outside `Form bottom`
- excessive arbitrary hard-coded colors
- dashboard/form page not full width or zero padding where detectable

Keep warnings non-blocking until a focused generated design-system package proves the rule at import/runtime. The Design System Request Tracker DSX baseline proves the core layout/list/form/workflow path, but generated app-level approval forms may still require publishing in Yeeflow Form Builder after import before submit/approve runtime testing.

## Shared Text Control Standard

Use `docs/collection-04.md#yeeflow-text-control-generation-standards` for all generated approval forms, data-list custom forms, dashboards, and app pages. The focused `Text Style Sample.ywf` export supersedes earlier CAPEX text-control guesses:

- native Text is `type: "heading"` / `label: "Text"`
- inline width is `attrs.common.positioning.widthtype = [null, "2"]`
- named typography presets use `attrs.heads.ty = [null, "<preset>"]`
- custom typography uses an object under `attrs.heads.ty`
- text color uses a plain string under `attrs.heads.color`
- dynamic content uses `attrs.headc.title.variable[]`

Avoid old generated shapes that store `attrs.heads.color` as `[null, color]`, because they can render but leave designer style popups unresponsive.

## Shared Form Action Standard

Use `docs/collection-04.md#yeeflow-form-action-generation-rules` for generated approval-form front-end actions.

Phase 1 runtime-proven capabilities:

- action buttons
- button click triggers
- page load triggers
- temp variables
- Set variable steps
- Show confirm dialog steps

Phase 2 export-backed capabilities:

- Query data steps
- query multiple and query single modes
- query result count variables
- query result collections
- query result mapping into form list variables and workflow variables
- Submit form steps
- Save changes submit mode

Keep the boundary clear:

- form actions are front-end form logic
- workflow actions are backend/process graph logic
- temp variables are form-runtime state and should not be treated as persisted business data
- Submit form / Save changes belongs to approval, data-list, and public forms, not dashboards
- Query data output should use explicit selected fields and explicit mapping
- query aggregates should use `arraySum`, not `arraySub`

## Generation Boundary

Do not generate a new app from this document alone. Start from a small focused package, validate it, import-test only when explicitly requested, publish imported approval forms when Yeeflow marks them unpublished, export back when useful, and then promote proven rules from warning to stronger generator behavior.

---

<a id="yeeflow-application-generation-review-checklist"></a>

<!-- source: docs/collection-03.md#yeeflow-application-generation-review-checklist -->

# Yeeflow Application Generation Review Checklist

Use this checklist before packaging, after local validation, and after runtime testing for generated Yeeflow applications. It is a review aid for future generation tasks; it does not replace the package/form/list/workflow validators.

## 1. Scope And Package Type

- [ ] The requested app type is clear: new/cloned `.yap` package or existing-app `.yapk` upgrade package.
- [ ] For new/cloned `.yap`, the package uses a fresh ID family and a fresh safe FlowKey/form key.
- [ ] For existing-app upgrades, `.yapk` is treated as read-only/server-generated unless a valid Yeeflow Version management baseline and signing/encoding proof exists.
- [ ] The app does not include dead configuration lists; every v1 list is used by forms, workflow, dashboard, reports, or documented runtime proof.
- [ ] Master/reference lists named in the requirement are real active lists, not placeholder concepts. Each one has an owner, fields, views/forms, and sample/reference rows when lookups depend on it.
- [ ] Line items have an explicit persistence model: workflow sublist summary only, direct child-row persistence, or a separate transaction item list.
- [ ] Availability/stock/capacity logic is clearly classified as manual review only, query-based availability, or inventory/reservation based.
- [ ] Review-only availability is not described as true stock decrement, reservation, or inventory control.
- [ ] Dashboard scope is meaningful for v1 and runtime-safe, or explicitly deferred.
- [ ] Dashboard KPI/summary/report/queue/chart promises are mapped to functional dashboard controls, not static Text placeholders.
- [ ] Business-critical capabilities are either implemented in v1 or marked as required runtime proof items with fallback behavior.

## 2. Local Validation Gate

- [ ] Newly generated `.yap` uses strict generator/import-readiness validation, not compatibility validation.
- [ ] Package wrapper validates with `validate-yap-package.js --mode generator --stage final`.
- [ ] App graph validates with `validate-yap-graph.js --mode generator --stage final`.
- [ ] `scripts/inspect-yap-import-readiness.mjs` passes when available.
- [ ] `scripts/inspect-yap-schema-standard.mjs` and `scripts/inspect-app-creation-rules.mjs` pass.
- [ ] Approval form definitions validate.
- [ ] Data list definitions validate.
- [ ] Every generated child data list also passes standalone `validate-ydl-list`; app-level package validation alone is not accepted.
- [ ] Every generated Type `1` data list includes `ListModel.ListType = 1`.
- [ ] Every generated Type `1` data list keeps native `Title` metadata as `Status = 0`, `IsSystem = true`, `IsIndex = true`, and `FieldIndex = 0`.
- [ ] Duplicate field names/internal names are absent.
- [ ] Duplicate display names inside a data list are absent or explicitly accepted as a materialization risk.
- [ ] Every generated child-list `FieldID` is unique across the whole `.yap`, not only within its own list.
- [ ] Every generated field's `ListID` equals its parent data-list `ListID`.
- [ ] `TenantID`, `CreatedBy`, and `ModifiedBy` retain real baseline metadata and are not included in `Resource.ReplaceIds`.
- [ ] Root Type `103` dashboard/page layout is owned by the root app/ListSet `ListID`, and its navigation entry points to an existing root layout.
- [ ] Root/custom page/form/dashboard `LayoutInResources[].ID` and `RefId` match the owning `LayoutID` where inline resources are used.
- [ ] Data-view columns resolve to real fields or explicitly allowed system fields; stale pseudo-field columns are removed before handoff.
- [ ] Dashboard dynamic-display rules and filters reference resolvable controls, fields, and page filter variables.
- [ ] `scripts/inspect-yap-materialization.mjs` passes before Yeeflow runtime import.
- [ ] Lookup targets, display fields, dependency maps, and sample lookup target rows resolve.
- [ ] Referenced master/reference lists are present and non-empty when form lookup selection is part of v1.
- [ ] Generated data-list custom forms include current-standard `Edit Item` and `View Item`; New/Edit maps to `Edit Item`, View maps to `View Item`.
- [ ] Workflow action configuration validates.
- [ ] Expression smoke checks cover generated formulas, query filters, workflow conditions, and sublist summaries where applicable.
- [ ] Dashboard structure inspection confirms KPI/count/total cards use `summary` controls with matching `exts`, queue/report sections use `data-list` or proven `collection` controls, planned chart sections use real chart controls when the chart model is known, and no KPI value is hardcoded as Text `0`, `0.00`, `N/A`, or placeholder content.
- [ ] The package is not sent to Yeeflow runtime testing until local validation passes, unless the task is explicitly an isolation/proof experiment.

## 3. Runtime Test Gate

Before claiming runtime proof, complete `docs/collection-04.md#yeeflow-runtime-test-checklist-template` for the tested package.

Minimum runtime smoke proof:

- [ ] App imports and opens.
- [ ] The app does not open to `Start to build with Components`.
- [ ] Child data lists show their generated custom fields, not only empty list shells.
- [ ] Dashboards render and functional controls are data-bound to expected source lists; page render alone is not dashboard proof.
- [ ] Data lists open without `datas/query` 400 responses.
- [ ] Approval forms open from the app and, when needed, from submitted workflow tasks.
- [ ] At least one valid submit path completes without workflow node errors.

Production-like runtime proof:

- [ ] Requester/applicant logic works for the intended requester and proxy scenarios.
- [ ] Product/sublist row calculations and summary totals update after row commit/blur.
- [ ] Policy-critical totals are recalculated through safe preflight actions before routing, quota, or persistence decisions.
- [ ] Query data filters use the runtime-proven filter shape and return only expected rows.
- [ ] Workflow branches cover normal, exception, empty, and unexpected values.
- [ ] Submit, approve, reject, return, and resubmission paths pass when included.
- [ ] `ContentList` and usage/audit list lifecycle events persist expected rows and readable values.
- [ ] Dashboard KPIs match source list data after runtime records change.
- [ ] Operational queues and advanced reporting sections render real list/table/collection data or empty states from the expected source lists.
- [ ] Chart runtime tests use representative source rows before acceptance. Record whether each chart renders with data, loads with no data, or fails to load; an empty dataset alone is not a chart-model failure.

## 4. Runtime Safety Review

- [ ] Temp variables are used only for frontend form state, not backend persistence, audit records, dashboard sources, or workflow routing that must survive submission.
- [ ] FlowKey/form key text does not collide with reserved JSON property names.
- [ ] Wrapped or export-back inspection shows no corrupted `prefix` / `pr<id>x` binding keys.
- [ ] Requester/applicant variables, profile snapshots, and quota/eligibility calculations use the intended identity variable rather than accidental Current User context after submission.
- [ ] Hardcoded tenant user IDs are avoided unless export-backed and valid for the target tenant.
- [ ] Attachment controls include tested behavior or explicit guidance/fallback when binary persistence is not proven.

## 5. Form Design Quality Gate

- [ ] Approval forms use page-level background and the learned `Main` / `Content` / `Form body` / `Form bottom` shell.
- [ ] Header, request summary, and metric/status rows are present when the business process needs quick review context.
- [ ] Normal fields use two-column grids; long text, upload, rich text, sublist, and guidance controls use full-row layout.
- [ ] Workflow Action Panel and Flow History are placed in `Form bottom`.
- [ ] Text and icon controls use learned inline-width behavior and meaningful designer `nv_label` values.
- [ ] Runtime-sensitive controls have fallback behavior or focused proof notes.
- [ ] The runtime form has no visible caption clutter, overlapping content, unreadable colors, or layout regressions.

## 6. Reporting Language

- [ ] Use "locally validated" when only local validators and wrapper checks passed.
- [ ] Use "runtime smoke-tested" only after import/open/dashboard/list/form and at least one valid workflow path are tested.
- [ ] Use "runtime baseline passed" only when branch, persistence, dashboard KPI, and included return/rejection paths have evidence.
- [ ] List every blocked or deferred runtime item explicitly in the final report.

---

<a id="yeeflow-application-layout-standards"></a>

<!-- source: docs/collection-03.md#yeeflow-application-layout-standards -->

# Yeeflow Application Layout Standards

This document defines reusable layout structure for generated Yeeflow applications.

## Global Shell

Generated visual surfaces should use this shell where supported:

```text
Page or form
└─ Main
   └─ Content
      └─ semantic sections
```

`Main` is the structural root parent. `Content` holds visible user-facing content. Both names are stored as `nv_label`.

## Page Background Rule

When a generated page needs a full-page background, configure it on the page/form background setting rather than on `Main`.

- dashboard page background: embedded page `attrs.background`
- data-list custom form background: custom form `attrs.background`
- approval form background: `page.formdef.attrs.background`

`Main` should stay a layout container. Section, card, content, and header backgrounds are still allowed on their own containers.

## Page Settings

Dashboard pages:

- hide header bar: `attrs.hideHeaderAll = true`
- padding: all sides `--sp--s0`
- page background: page-level `attrs.background` when needed
- no dashboard content-width setting is required yet because the studied export did not prove `cw`

Form pages:

- content width: `attrs.container.cw = "2"`
- padding: all sides `--sp--s0`
- page background: form-level `attrs.background` when needed

## Standard Sections

Use semantic section containers inside `Content`:

- `Page header`
- `Summary section`
- `Body section`
- `Form body`
- `Form bottom`
- `Collection section`
- `Field group`
- `Readonly section`
- `Empty state`

Sections should group real user tasks. Do not create decorative nested cards.

## Dashboard Layout

Default dashboard structure:

```text
Main
└─ Content
   ├─ Page header
   ├─ Summary section
   ├─ Body section
   └─ Empty state or Collection section
```

Use `Summary section` for KPI cards and short operational status. Use `Body section` for larger tables, Collections, charts, or guidance.

## Data List Form Layout

Default custom list form structure:

```text
Main
└─ Content
   ├─ Page header
   ├─ Field group
   └─ Readonly section
```

`Edit Item` should prioritize editable controls. `View Item` should prioritize readable fields and status/context.

## Approval Form Layout

Default approval form structure:

```text
Main
└─ Content
   ├─ Form body
   │  ├─ Request summary
   │  ├─ Request details
   │  └─ Approval decision
   └─ Form bottom
      ├─ workflowControlPanel
      └─ workflowHistory
```

Submission pages may omit reviewer-only fields. Task pages should mirror request fields as readonly and place decision fields in `Form body`.

## Responsive Guidance

Generated apps should prefer:

- one-column mobile structures
- two-column form groups only when field labels and values remain readable
- dashboard cards that wrap predictably
- Collections over ad hoc repeated static rows for list-like data

Avoid layouts that rely on precise viewport width or negative spacing.

## Validator Guidance

Validators may warn for missing shell containers, missing section names, incorrect form page width, non-zero page padding, `Main` carrying full-page-like background, missing page-level background when `Main` has a background, or workflow controls outside `Form bottom`.


Dashboard/app page root content-area padding is a hard gate: every generated or upgraded Type 103 dashboard/app page must serialize `Pages[].LayoutInResources[].Resource` with root `attrs.container.cw = "2"` and `attrs.container.padding = [null, { top: "--sp--s0", right: "--sp--s0", bottom: "--sp--s0", left: "--sp--s0" }]`. Scalar padding, object/numeric padding, numeric array padding, `attrs.common.padding`, or `attrs.style.padding` alone are invalid for the root content area. Normalize existing dashboard/app page roots to this exact token-array shape before signing, installing, importing, or upgrading. Inner layout containers may keep intentional spacing.

Data-list custom form root content-area padding uses the same hard gate: every generated or upgraded New, Edit, View, Detail, or custom form under `Data.Childs[].Layouts[].LayoutInResources[].Resource` or `Childs[].Layouts[].LayoutInResources[].Resource` must parse to a root with `attrs.container.cw = "2"` and the same `--sp--s0` token-array padding. Scalar zero, numeric object zero, `attrs.common.padding`, or `attrs.style.padding` alone remain compatibility fallbacks only and do not satisfy generated-final validation. Inner form sections, cards, grids, controls, and content wrappers may keep intentional spacing.

---

<a id="yeeflow-application-package-generation-rules"></a>

<!-- source: docs/collection-03.md#yeeflow-application-package-generation-rules -->

# Yeeflow Application Package Generation Rules

## Canonical Schema Files

YAPK schema: `schemas/yapk-schema.json`

YAP schema: `schemas/yap-schema.json`

These filenames are stable references for package generation, validation, and inspection. When the product schema changes, replace the file contents while keeping these paths unchanged. Keep package rules separate: YAPK uses `AppExportPackageInfo`, Brotli `AppPackageInfo`, `Childs[].Fields`, and `PortalInfo: null`; YAP uses the YAP wrapper, `[______gizp______]` gzip `ListExportResult`, `Defs`, and `SimplePortal: null`.

## Application Icon Rule

Generated Yeeflow application packages must use FontAwesome icon mode for the top-level wrapper `IconUrl`. Image-based application icons are forbidden for generated-final `.yap` and `.yapk` output.

`IconUrl` must be a JSON string with this shape:

```json
{"b":"#E6F0FF","i":"fa-solid fa-screwdriver-wrench","c":"#0065FF"}
```

Required icon fields:

- `b`: icon background color.
- `i`: FontAwesome class string with a style class and icon class, such as `fa-solid fa-screwdriver-wrench`, `fa-regular fa-calendar-check`, `fa-solid fa-laptop`, or `fa-solid fa-boxes-stacked`.
- `c`: icon foreground color.

The selected icon must match the generated application business domain. Use documented mapping or rationale in the App Plan or generation report, for example Facility Maintenance -> `fa-solid fa-screwdriver-wrench`, Leave/Calendar -> `fa-regular fa-calendar-check`, IT Hardware -> `fa-solid fa-laptop`, Inventory/Assets -> `fa-solid fa-boxes-stacked`, Procurement -> `fa-solid fa-cart-shopping`, Customer/Account Health -> `fa-solid fa-chart-line`, or Document Library -> `fa-regular fa-file-lines`. The App Plan may record the selected icon rationale, but it must not include generated package IDs.

`IconUrl` must not be empty, null, a plain image URL, an `https://img.yeeflow.com/...` URL, a data image, SVG markup, emoji, or any non-FontAwesome token. Package validation and signing readiness must block image-icon packages.

## Package Type Decision

Ask whether the user wants a new application or an upgrade to an existing application.

Use `.yap` when:

- the user wants to create/import a new Yeeflow application
- the package should become a separate cloned app
- fresh IDs and import-time replacement are desired

Use `.yapk` when:

- the user wants to upgrade or modify an already imported Yeeflow application
- the user is using Application Settings -> Version management -> Upgrade application
- the package must map onto an existing app instead of creating a new clone

## `.yap` New Application Rules

For generated new-app packages:

- treat `schemas/yap-schema.json` as the base structural schema only; schema pass is necessary but not sufficient
- also apply `docs/standards/collection-03.md#yap-generation-contract` and `docs/standards/collection-03.md#yap-export-shaped-application-generation-standard`
- generate from an export-like baseline/reference shape; if no safe baseline exists, refuse import-qualified claims
- generate fresh local IDs
- use Yeeflow-compatible long numeric-string local IDs and preserve export-observed string IDs for `pageurls` and workflow `childshapes`
- allocate data-list `FieldID` values from a global app-level field ID allocator; do not reset the field ID range per list
- preserve the parent data-list `ListID` on every field; changing `FieldID` must not change `field.ListID`
- preserve real baseline `TenantID`, `CreatedBy`, and `ModifiedBy` metadata; do not remap tenant/user metadata into the generated ID family
- use a FlowKey/form key that cannot collide with reserved JSON property names
- include import-remappable IDs in `Resource.ReplaceIds`
- keep `Resource.ReplaceIds` limited to generated app-resource IDs, not tenant/user metadata
- rebuild `Resource.ReplaceIds` from the final decoded package after every generation or repair step; never reuse a baseline package's stale `ReplaceIds`
- generated-final packages must fail validation if any local root app, child list, field, layout, form/process, workflow shape, app group, or local sample record ID is missing from `Resource.ReplaceIds`
- keep app-level approval forms as `Data.Forms[].ListID = 0`
- include full export-style root/list/layout/field/form/process metadata and app-level structures even when empty or null
- parse decoded/stringified runtime-critical fields before output, including `DefResource`, field `Rules`, form `Settings`, `LayoutView`, and layout/view `Ext1` / `Ext2` / `Ext3`
- validate decoded app data, graph, child lists, approval forms, workflow actions, expressions, and wrapper round trip
- run materialization inspection before runtime import
- import through the new application/import flow

## YAP App Materialization Rules

The Custom Code Template Showcase runtime investigation proved that a generated package can pass broad JSON/package checks while importing as an empty app shell or lists without fields. Treat these as hard rules for generated `.yap` packages:

- Every `ListID` must be unique.
- Every `FieldID` must be unique across the whole application.
- Every `field.ListID` must equal the parent data list `ListID`.
- Every `FieldName`, `InternalName`, and `DisplayName` must be unique inside its own data list.
- Do not remap `TenantID`, `CreatedBy`, or `ModifiedBy` as generated app-resource IDs.
- Do not include `TenantID`, `CreatedBy`, or `ModifiedBy` in `Resource.ReplaceIds`.
- Treat stale or incomplete `Resource.ReplaceIds` as an async import failure risk. The broad Service Desk form-workspace YAP test proved that API `Status = 0` with `Completed = false` can still fail later when child list, layout, field, form/process, workflow shape, or sample record IDs are absent from `ReplaceIds`.
- `Resource.ReplaceIds` must be regenerated after the final package mutation, not copied from a successful baseline or earlier repair candidate.
- API status `0` with `Completed=false` is only API accepted/queued proof, not import success. Generated reports must state the proof boundary explicitly.
- Root Type `103` dashboard/page layout ownership must connect to the root app/ListSet `ListID`.
- Root navigation must reference real packaged dashboard/page, child-list, and approval-form resources.
- Run `scripts/inspect-yap-materialization.mjs` before runtime import.
- Do not proceed to custom-code component runtime testing if app materialization fails.

## `.yapk` Existing Upgrade Rules

For existing-app upgrade packages:

- start from a `.yapk` generated by Yeeflow Version management for the target app
- preserve app identity fields such as `PackageId`, `TenantID`, `AppID`, and `ListID` unless a Yeeflow-generated version package proves a different rule
- preserve the installed app `ListSetID`; if generated `ListSetID` differs from the installed/previous application, stop
- preserve stable object IDs; do not apply fresh-ID generation to existing app objects as if the package were a `.yap`
- load the previous package and ID lineage/provenance manifest before generation
- classify the package as an upgrade when the user requested an update; do not silently create a fresh-install package
- match existing resources by stable semantic key and keep their IDs exactly
- keep existing pages/resources byte-stable when the declared scope is a sandbox/test page only
- allocate new API-issued IDs only for newly added resources
- fail closed when semantic matching is ambiguous or when the previous package/manifest is missing
- never reuse removed IDs for different objects
- rebuild `Resource.ReplaceIds` from final package contents after all edits
- use Application Settings -> Version management -> Upgrade application for runtime testing
- do not generate a `.yap` when the user asked to upgrade the current app, unless they explicitly want a separate clone

## Generated-Final `.yapk` Hard Gates

For generated-final `.yapk` application output, generation must stop before signing, install, upgrade-check, or handoff unless both hard gates pass:

- Application FontAwesome Icon Gate: top-level wrapper `IconUrl` must pass `node scripts/validate-application-icon.js --package dist/<app>.yapk` and use the required JSON string shape with `b`, `i`, and `c`. Image URL icons, missing fields, non-FontAwesome values, and domain-mismatched icons without an approved rationale fail before signing.
- API-Issued Content ID Provenance Gate: every numeric generated application content ID is allocated through `GET /utils/generate/ids?count=<n>` and recorded in `dist/<app-name>-id-provenance-report.json` with `sourceMarker: "api-generated"`.
- Navigation Runtime Metadata Gate: every runtime navigation group includes API-issued `ID`, `AppID`, `ListSetID`, `Type: "classes"`, `Title`, `Icon`, and `list[]`; every child includes `AppID`, `Title`, `ListID`, `ListSetID`, and `Type`; targets resolve to `Pages[].LayoutID`, `Forms[].Key`, or `Childs[].List.ListID`.

Local ID fallback is forbidden for generated-final output, including local sequential counters, local `id()` helpers, hardcoded generated IDs, copied sample/export IDs, random values, timestamps, UUID fallback, and deterministic local-only seeds.

Required validators:

```bash
node scripts/validate-yapk-id-provenance.mjs --package dist/<app>.yapk --manifest dist/<app>-id-provenance-report.json
node scripts/validate-application-icon.js --package dist/<app>.yapk
node scripts/validate-yapk-navigation-runtime-metadata.mjs --package dist/<app>.yapk --id-provenance dist/<app>-id-provenance-report.json
node scripts/validate-yapk-upgrade-id-stability.mjs --previous-package dist/<app>-previous.yapk --previous-manifest dist/<app>-previous-id-lineage.json --new-package dist/<app>.yapk --new-manifest dist/<app>-id-lineage.json
node scripts/inspect-yapk-upgrade-app-identity.mjs --package dist/<app>.yapk --lineage dist/<app>-lineage.json
```

`validate-yapk-upgrade-id-stability.mjs` and `inspect-yapk-upgrade-app-identity.mjs` are required for upgrade/new-version output, not first-generation output. `setsign` / `verifysign` prove wrapper/resource signature only. Package API install/upgrade acceptance proves action acceptance only. Neither proves ID provenance, upgrade ID continuity, ListSetID/app identity stability, navigation runtime metadata completeness, or runtime UI materialization.

For UI-heavy packages, also apply `docs/standards/collection-02.md#ui-summary-kpi-runtime-hard-gates`. High-quality UI requires a page-by-page implementation contract, export-proven control/style shapes, and runtime screenshot evidence. Summary/KPI controls require designer-shaped metadata and runtime evidence. Dynamic visible KPI binding is proven only for the exact UUID Summary v1.0.1 shape with UUID Summary IDs, matching `Resource.ReportIds[]`, matching `Resource.exts[]`, dashboard `Resource.tempVars[]`, designer-shaped `attrs.save_var`, visible `attrs.headc.title.variable[]`, complete field metadata, before/after mutation proof, and refreshed/recalculated runtime evidence. Other shapes remain unproven unless focused runtime proof exists, and fallback KPI values must be explicitly labeled as fallback.

Facility Maintenance dashboard review hard gates apply at generation/package/reporting time only. Do not add these requirements to Functional Specification or App Plan documents. Before signing readiness, signing, install/import, upgrade, or final success reporting for generated dashboard packages, run `node scripts/validate-dashboard-generation-hard-gates.mjs --package dist/<app>.yapk` and include `--report <final-report>` when a final/install/upgrade/runtime report is produced. This gate fails missing select/radio/checkbox filter display/value/style metadata, raw Container `attrs.style.widthtype` strings or missing flex layout keys, KPI icon placeholders that are Heading/Text instead of native Icon controls, Summary controls without designer-selected aggregate fields and matching visible KPI bindings, and canonical application URLs that use install/import operation IDs instead of decoded package `$.ListSet.ListID`.

## Current `.yapk` Limitation

The first studied `.yapk` version package stores `Resource` as an opaque high-entropy base64 payload, not as `[______gizp______]` gzip data. Existing `.yap` decoder/builder scripts cannot decode or rebuild that resource.

Safe current behavior:

- inspect wrapper metadata
- validate package structure
- preserve opaque `Resource` and `Sign`
- treat offline wrapper edits as study-only unless runtime-proven for the exact package
- compare multiple Yeeflow-generated `.yapk` versions to understand package identity and feasibility

Unsafe until proven:

- modifying app lists, dashboards, forms, workflows, or app resource internals inside `.yapk`
- editing wrapper metadata such as `Version`, `Notes`, `Description`, or `Date`
- recalculating or replacing `Sign`
- fresh-ID regeneration for existing app upgrades
- claiming app-content upgrade runtime readiness from local validation alone

Runtime update: a metadata-only proof package that preserved `Resource`, `Sign`, `PackageId`, `TenantID`, `AppID`, and `ListID`, but edited `Version`, `Notes`, `Description`, and `Date`, was rejected by Yeeflow Upgrade application after the metadata preview step. This suggests Yeeflow validates more than the opaque `Resource` and may include wrapper metadata in package integrity checks. Until proven otherwise, `.yapk` generation must preserve the exact Yeeflow-generated wrapper or use a Yeeflow-supported version-generation/signing path.

Multi-version feasibility update: three Yeeflow-generated `.yapk` packages from the same app lineage showed stable `TenantID`, `AppID`, `ListID`, app title/description/icon, and author, while `PackageId`, `Version`, `Notes`, `Date`, `Sign`, and `Resource` changed. All resources stayed opaque/high-entropy, normal decompression failed, and searched app strings were not readable. Classification is **not locally generatable with current knowledge**. Use Yeeflow Version management to generate official `.yapk` upgrade packages.

Product schema follow-up: `yapk-schema.json` defines `.yapk` as `AppExportPackageInfo` and describes top-level `Resource` as a Brotli compressed string whose decompressed JSON should match `AppPackageInfo`. This is the target schema for `yeeflow-yapk-package-generator`, but readable historical artifacts did not verify the Brotli path in the current study. Continue treating app-content `.yapk` mutation as unsupported until a Resource decode/edit/Brotli encode/sign/verify/runtime-upgrade path is proven.

## Required User Guidance

When a user asks to modify a Yeeflow app, clarify the target package type:

```text
Should I create a new `.yap` application package, or an upgrade `.yapk` package for the existing imported app?
```

If they choose `.yapk`, request a baseline `.yapk` from Version management and explain that object identity should be preserved.

---

<a id="yeeflow-application-style-token-standards"></a>

<!-- source: docs/collection-03.md#yeeflow-application-style-token-standards -->

# Yeeflow Application Style Token Standards

This document defines how generated Yeeflow applications should use root style tokens.

The source is `<downloads>/root_styles.txt`. Treat it as read-only evidence and do not inject the full stylesheet into generated apps.

## Color Semantics

| Semantic use | Preferred token |
| --- | --- |
| primary action / selected state | `--c--primary` |
| secondary accent | `--c--secondary` |
| success / approved / active good state | `--c--success` |
| warning / pending / attention | `--c--warning` |
| danger / rejected / failed | `--c--danger` |
| neutral borders and muted structure | `--c--neutral` and neutral variants |
| page/card background | `--c--background` |
| body text | `--c--text` or `--c--text-normal` |

Use light variants for badge backgrounds and base tokens for badge text/borders where style attributes allow it.

## Theme Light Models

Generated `Data.AppThemes[].Config` should use `lightmodel: "Luminance"` for Neutral as well as Primary and Secondary. Do not generate Neutral with `Lightness`; the lighter neutral steps can become visually weak or inconsistent with the Yeeflow root style scale.

Recommended generated theme colors:

```json
{
  "primary": { "value": "#0065FF", "lightmodel": "Luminance" },
  "secondary": { "value": "#00D1FF", "lightmodel": "Luminance" },
  "neutral": { "value": "#B3B7C0", "lightmodel": "Luminance" }
}
```

## App Header And Navigation

Root app navigation must keep enough contrast between the header and the navigator menu. When the header uses `LayoutView.attrs.appearance`, generated apps should invert those colors for `LayoutView.attrs["navigator-menu"]`:

- header background: `attrs.appearance.bgc`
- header text/icon color: `attrs.appearance.color`
- navigator background: `attrs["navigator-menu"].bgc = attrs.appearance.color`
- navigator text/icon color: `attrs["navigator-menu"].color = attrs.appearance.bgc`

For the standard generated design-system theme this means:

```json
{
  "appearance": { "bgc": "var(--c--primary-light)", "color": "var(--c--primary)" },
  "navigator-menu": { "bgc": "var(--c--primary)", "color": "var(--c--primary-light)" }
}
```

Do not leave the navigator text color implicit when the navigator background is set. The Design System Request Tracker runtime showed that an implicit nav text color can match the nav background and make menu labels unreadable.

## Typography

Use:

- `--fs--base` for normal body text and field labels
- `--fs--s` / `--fs--xs` for helper text and metadata
- `--fs--l` / `--fs--h6` for section headings
- `--fs--h5` / `--fs--h4` for dashboard panel headings
- `--fs--h3` through `--fs--h1` sparingly for page-level headings

Use:

- `--fw--regular` for normal text
- `--fw--medium` for labels
- `--fw--semi-bold` for section/card headings
- `--fw--bold` only for high-emphasis metrics or page titles

Do not use editorial typography in product UI.

## Spacing

Use:

- `--sp--s0` for standard page shell padding
- `--sp--s100` for compact inline gaps
- `--sp--s150` / `--sp--s200` for field gaps
- `--sp--s200` / `--sp--s300` for card padding
- `--sp--s300` / `--sp--s400` for section gaps
- larger spacing only for dashboard overview pages, not dense operational forms

## Borders, Radius, And Depth

Recommended defaults:

- input/button radius: `6px`
- standard card radius: `8px`
- large panel radius: `12px`
- standard border: `var(--c--neutral-light-active)`
- shadows: sparse and subtle

Prefer neutral borders and spacing over decorative shadows.

## Component Style Guidance

Buttons:

- primary actions use `--c--primary`
- destructive actions use `--c--danger`
- secondary actions use neutral borders/backgrounds

Switches and radios:

- prefer native Yeeflow control behavior
- keep state styling token-aligned
- do not invent one-off option palettes

Status badges:

- approved/active: success
- pending/review: warning
- rejected/error: danger
- draft/inactive/info: neutral or primary depending on context

Dynamic styles:

- preserve existing dynamic class behavior
- use source status semantics to choose token-aligned styles
- do not remove dynamic classes from studied exports unless replacing them with a proven native equivalent

## Export Reality

Yeeflow may store either token references or resolved hex values. Generators should prefer tokens where schema-supported, but validators should warn rather than fail when resolved values are present.

---

<a id="yeeflow-application-ui-ux-standards"></a>

<!-- source: docs/collection-03.md#yeeflow-application-ui-ux-standards -->

# Yeeflow Application UI/UX Standards

This document records the first official generated-app UI/UX standard reference from the mini app export `UI and UX design (1).yap`.

For the consolidated reusable standard, use `docs/collection-03.md#yeeflow-application-design-system` first, then use this document for export-level evidence.

The source export was studied read-only. The raw `.yap` must not be bundled into generated packages or committed as a standards artifact.

Use `docs/collection-04.md#yeeflow-root-style-token-reference` and `docs/collection-03.md#yeeflow-application-style-token-standards` as the design-token references for color, typography, spacing, radius, control, and dynamic style guidance. Do not inject the full root stylesheet into generated apps.

## Studied Export Inventory

Wrapper:

- Title: `UI and UX design`
- Resource format: JSON wrapper with gzip/base64 `Resource`
- `MainListType`: `1024`
- `AppID`: `41`
- `ReplaceIds`: `8`
- `ReportIds`: `0`
- `FormKeys`: `UI-AF`

Application resources:

- Root app/listset: `UI and UX design`
- Dashboard page: `Dashboard`
- Data list: `Data List`
- Data-list custom forms:
  - `Edit Item`
  - `View Item`
- Approval form: `Approval form`

The mini app intentionally contains a minimal control tree. It proves layout scaffolding, naming, page settings, and form-bottom placement rather than rich field or dashboard content.

## Global Generated-App Rule

Generated Yeeflow apps should use this UI shell for every generated page or form surface where the schema supports it:

1. Full-width content area.
2. Zero page padding.
3. One top-level parent container named `Main`.
4. One child container named `Content`.
5. Most visible business content placed inside `Content`.

Designer names are stored as `nv_label`, not as visible page text.

Generated apps should use Yeeflow-native root style tokens where the page/control schema supports style values. Prefer token references such as `var(--c--primary)`, `var(--c--neutral-light-active)`, `var(--fs--base)`, and `var(--sp--s200)` over arbitrary custom colors or spacing. Do not force token usage when a Yeeflow export stores resolved hex values.

Required container naming:

| Name | Stored as | Purpose |
| --- | --- | --- |
| `Main` | `control.nv_label` | Top-level parent container for the generated page/form content |
| `Content` | `control.nv_label` | Main visible content area inside `Main` |
| `Form body` | `control.nv_label` | Approval-form business fields inside `Content` |
| `Form bottom` | `control.nv_label` | Approval-form workflow controls at the end of `Content` |

## Page Settings

Zero padding is stored as:

```json
{
  "attrs": {
    "container": {
      "padding": [
        null,
        {
          "top": "--sp--s0",
          "right": "--sp--s0",
          "bottom": "--sp--s0",
          "left": "--sp--s0"
        }
      ]
    }
  }
}
```

Full-width content area on forms is stored as:

```json
{
  "attrs": {
    "container": {
      "cw": "2"
    }
  }
}
```

Dashboard hidden header is stored as:

```json
{
  "attrs": {
    "hideHeaderAll": true
  }
}
```

## Design Tokens

Default generated-app semantic mapping:

| Use | Preferred token |
| --- | --- |
| page background | `--c--neutral-light` or `--c--background` |
| card/container background | `--c--background` |
| primary action | `--c--primary` |
| success status | `--c--success` / `--c--success-light` |
| warning status | `--c--warning` / `--c--warning-light` |
| danger status | `--c--danger` / `--c--danger-light` |
| neutral border | `--c--neutral-light-active` |
| muted text | `--c--neutral-dark` |
| body text | `--c--text` or `--c--text-normal` |

Typography defaults:

- body and form labels: `--fs--base`
- helper text and metadata: `--fs--s` or `--fs--xs`
- section headings: `--fs--l` or `--fs--h6`
- page headings: `--fs--h3` through `--fs--h1`, used sparingly
- normal text weight: `--fw--regular`
- section/card heading weight: `--fw--semi-bold`

Spacing defaults:

- global shell padding: `--sp--s0`
- field gaps: `--sp--s150` or `--sp--s200`
- card padding: `--sp--s200` or `--sp--s300`
- section gaps: `--sp--s300` or `--sp--s400`

Radius and borders:

- small controls: `6px`
- standard cards/sections: `8px`
- larger panels/upload/settings containers: `12px`
- neutral borders: `var(--c--neutral-light-active)`

## Main And Content Container Shape

The standard `Main` container uses:

```json
{
  "type": "container",
  "label": "Container",
  "nv_label": "Main",
  "attrs": {
    "style": {
      "gap": [null, "--sp--s0"],
      "direction": [null, "column"],
      "justify_content": [null, "flex-start"],
      "align_items": [null, "center"]
    }
  }
}
```

The standard `Content` container uses:

```json
{
  "type": "container",
  "label": "Container",
  "nv_label": "Content",
  "attrs": {
    "style": {
      "gap": [null, "--sp--s0"],
      "direction": [null, "column"],
      "justify_content": [null, "flex-start"]
    }
  }
}
```

In dashboard and custom list form examples, `Content.attrs.style.align_items` may exist as `[null, null]`. Generators may omit that value when not needed.

## Navigation Naming

The studied root navigation contains:

- `Dashboard` for the Type `103` page.
- `Data List` for the data list.
- A Type `105` approval-form navigation entry pointing to `UI-AF`.

Generator rule:

- Use clean business names in navigation.
- Avoid field slot names, internal IDs, and implementation terms.
- For future apps, use specific names such as `Overview`, `Requests`, `Catalog`, and `Submit Request` instead of generic names when the business domain is known.

## Generator Rules

Generated app builders should:

- Apply the global page shell to generated dashboards, custom list forms, and approval form pages.
- Use Yeeflow root style tokens for colors, spacing, and typography where supported by native style attributes.
- Prefer semantic status tokens: primary, success, warning, danger, and neutral.
- Avoid arbitrary hard-coded colors unless copied from a real export or required because Yeeflow stores resolved values.
- Preserve existing hard rules such as native `Title` metadata and `Data.Forms[].ListID = 0`.
- Use `nv_label` for container naming.
- Keep IDs fresh for every generated package.
- Keep workflow logic unchanged when the task is UI/UX improvement only.
- Include Action Panel and Flow History by default in generated approval forms unless the user explicitly asks to omit them.
- Keep raw `.yap`, `.ydl`, `.ywf`, zip, env, secret, and credential artifacts out of commits.

## Validator Recommendations

The validators now add warnings for the UI/UX standard where safe:

- Dashboard hidden header, zero padding, and `Main` / `Content` containers.
- Custom list form full width, zero padding, `Main` / `Content` containers, and Edit/View form mapping.
- Approval form full width, zero padding, `Main` / `Content` / `Form body` / `Form bottom`, and workflow controls in `Form bottom`.
- Excessive arbitrary hard-coded colors where a known root token or semantic token should be used.
- Literal hex colors that exactly match known root token values, where the target schema accepts token references.

These checks are warnings for now. They should become generator/final errors only after a generated package using the standard imports, opens, and exports back cleanly.

## Exceptions

Allowed exceptions:

- Minimal dashboard-only research packages may omit `Content` content while proving shell behavior.
- Some approval forms may omit Action Panel or Flow History only when the user explicitly requests it or when a real export proves the form type should omit them.
- Legacy compatibility validation should not fail solely because older exports do not follow this new UI standard.

## First Generation Test Direction

The first generated test package should be layout-focused:

- One dashboard.
- One data list.
- Two custom list forms: `Edit Item` and `View Item`.
- One approval form with request and task page structures.
- No external connections, AI, document library, custom code, or complex workflow logic.

See `yeeflow-ui-ux-standard-first-generation-test-plan.md` and `yeeflow-ui-ux-standard-first-generation-test-spec.json`.


Dashboard/app page root content-area padding is a hard gate: every generated or upgraded Type 103 dashboard/app page must serialize `Pages[].LayoutInResources[].Resource` with root `attrs.container.cw = "2"` and `attrs.container.padding = [null, { top: "--sp--s0", right: "--sp--s0", bottom: "--sp--s0", left: "--sp--s0" }]`. Scalar padding, object/numeric padding, numeric array padding, `attrs.common.padding`, or `attrs.style.padding` alone are invalid for the root content area. Normalize existing dashboard/app page roots to this exact token-array shape before signing, installing, importing, or upgrading. Inner layout containers may keep intentional spacing.

Data-list custom form root content-area padding uses the same hard gate: every generated or upgraded New, Edit, View, Detail, or custom form under `Data.Childs[].Layouts[].LayoutInResources[].Resource` or `Childs[].Layouts[].LayoutInResources[].Resource` must parse to a root with `attrs.container.cw = "2"` and the same `--sp--s0` token-array padding. Scalar zero, numeric object zero, `attrs.common.padding`, or `attrs.style.padding` alone remain compatibility fallbacks only and do not satisfy generated-final validation. Inner form sections, cards, grids, controls, and content wrappers may keep intentional spacing.

---

<a id="yeeflow-approval-form-design-standards"></a>

<!-- source: docs/collection-03.md#yeeflow-approval-form-design-standards -->

# Yeeflow Approval Form Design Standards

Generated approval forms should be structured for request submission and task review.

## Required Shell

Submission and task pages use:

- `attrs.container.cw = "2"`
- zero padding
- page-level background when a full-page background is required
- `Main`
- `Content`
- `Form body`
- `Form bottom`

`Main` should remain primarily structural. Do not put full-page background color on `Main`; set it on `page.formdef.attrs.background`.
This same rule applies to submission pages and task pages. Section/card/header-specific backgrounds are allowed on their own containers, but `Main` should not be used as the page background layer.

## Form Header

Submission forms with a top request summary should place that area inside a `Form header` container.

`Form header` should carry:

- background
- border
- border radius
- `attrs.style.overflow = [null, "hidden"]`

Use `Request summary panel` and `Request metric row` inside `Form header`. If a gradient is required, use native background first and then `attrs.common.css` on the target container when Yeeflow native background settings cannot express it.

## Form Body

`Form body` contains business content:

- request summary
- applicant/requester fields
- request details
- readonly mirrored fields
- reviewer decision fields
- conditional fields

Recommended child sections:

- `Request summary`
- `Applicant details`
- `Request details`
- `Approval decision`
- `Reviewer notes`

## Form Bottom

`Form bottom` appears at the end of `Content`.

Default controls:

1. `workflowControlPanel`
2. `workflowHistory`

Omit either control only when the user explicitly asks or a real export proves that the form type should omit it.

## Submission Page

The submission page should:

- prioritize editable requester fields
- show conditional fields close to their controlling field
- avoid reviewer-only decision fields
- include Action Panel and Flow History unless explicitly omitted

## Task Page

Task pages should:

- mirror submitted request fields readonly where useful
- include reviewer decision fields
- keep approval comments/decision controls near the decision section
- preserve workflow logic and routing

## Conditional Fields

Conditional fields should:

- have clear business labels
- be placed near the triggering field
- use compatible condition value types
- be mirrored on task pages when reviewers need context

## Style

Use:

- neutral surfaces
- semantic decision/status colors
- `--fs--base` for body text
- `--fs--l` / `--fs--h6` for section headings
- `--sp--s150` / `--sp--s200` field gaps
- neutral borders before shadows

Let native Action Panel and Flow History styling apply unless a studied export proves a safe override.

Text and icon controls should be generated with explicit positioning:

- `heading` / `text-editor`: default to `attrs.common.positioning.widthtype = [null, "2"]`
- `icon`: default to `attrs.common.positioning.widthtype = [null, "2"]`
- section icons should usually be wrapped in square badge containers with centered alignment

Standard field sections should use a `flex_grid` with two desktop/tablet columns and one mobile column. Place long controls such as `textarea`, `richtext`, and `list` outside the grid or make them span the full row after span behavior is export-proven.

Generated forms should detect calculated-looking fields. `Subtotal` should not be a normal editable field when quantity and unit price exist; use a native `calculated` control when safe, or a readonly display with the formula documented as deferred.

Applicant/profile snapshot fields and autofilled fields should be readonly by default. In `Employee Family Implant v1 Core Patch 20260516`, the user repaired `Applicant Name` with the simple export-backed control shape `readonly: true`. Apply the same pattern to generated snapshot controls such as applicant employee ID, department, boarding date, user status, email, line manager, eligibility/status snapshots, lookup-autofilled product type, unit price, row subtotal, and total amount. The requester/applicant picker may be editable only on the new request page; task/reviewer pages should render it readonly.

If the requester/applicant picker is editable for proxy submission, bind its field-change action to rerun applicant snapshot initialization and dependent quota/policy calculations. Field-level Default value = Current User is sufficient for initial defaulting; do not also generate a redundant Set variable step that writes RequesterApplicant from Current User.

For policy and quota sections, keep dependent calculations in ordered form-action steps and use multi-value Set variables only for independent assignments. Submit guard actions should run the relevant policy check first, then use a conditional warning/confirm step with `continue: true` before the native Submit form step when the valid path needs to proceed.

For detailed patterns from the manually improved CAPEX export, see `docs/collection-04.md#yeeflow-form-design-quality-rules` and `docs/collection-02.md#it-hardware-capex-request-runtime-v2-ui-study`.

For native Text controls, use `docs/collection-04.md#yeeflow-text-control-generation-standards`. The focused `Text Style Sample.ywf` export proves that generated Text controls should use `type: "heading"`, inline width at `attrs.common.positioning.widthtype = [null, "2"]`, named typography presets as `attrs.heads.ty = [null, "h5-medium"]`, and plain string colors such as `attrs.heads.color = "var(--c--text)"`.

## Expression Rules

Use `docs/collection-04.md#yeeflow-expression-editor-reference`, `docs/collection-04.md#yeeflow-expression-generation-rules`, and the normalized expression references before generating:

- calculated controls
- dynamic display conditions
- custom validation conditions
- lookup filters
- workflow transition conditions
- default values and request-number expressions

Expressions should be Yeeflow expression-token arrays. Do not generate raw JavaScript formulas. Use only known functions/operators from the normalized references and keep variable tokens in the exact required shape.

## Form Actions

Use `docs/collection-04.md#yeeflow-form-action-generation-rules` and `docs/collection-04.md#yeeflow-temp-variable-generation-rules` before generating front-end form actions.

Phase 1 export-backed patterns:

- form actions live at `page.formdef.actions[]`
- page load trigger lives at `page.formdef.formAction.onLoad`
- button click trigger lives at `action_button.attrs.control_action`
- temp variable declarations live at `variables.tempVars[]`
- temp expression tokens use `id: "__temp_<tempVarId>"`
- Set variable steps use `type: "setvar"`
- Show confirm dialog steps use `type: "confirm"`

Phase 2 export-backed patterns:

- Query data steps use `type: "querydata"`
- Query multiple can map records into a form `list` variable
- Query single can map selected fields into workflow variables
- Query result counts and temp collections use `__temp_` variable targets
- Query result aggregates can use `arraySum`
- Submit form steps use `type: "submit"`
- Save changes is a submit mode with `attrs.submitType = "3"`

Phase 3 export-backed condition-flow pattern:

- A conditional form-action step uses step-level `condition`.
- The designer checkbox `Continue next step when condition is not met` serializes as step-level `continue: true`.
- Conditional warning/confirm/check steps before submit should usually set `continue: true`; otherwise the valid path can skip the warning but stop before the following Submit form step.

Keep form actions distinct from workflow actions. Form actions are front-end form logic; workflow actions are process graph/backend logic. Use form actions for UI state, defaults, confirmation, query-assisted UI, and safe client-side initialization. Do not use temp variables as the only source for persisted business data.

Use `docs/collection-04.md#yeeflow-form-action-query-data-step-rules` and `docs/collection-04.md#yeeflow-form-action-submit-step-rules` before generating Phase 2 form actions.

## Validator Guidance

Warn for missing full width, zero padding, missing page-level background on rich full-page forms, background on `Main`, missing `Main`, missing `Content`, missing `Form body`, missing `Form bottom`, missing `Form header` when a request summary exists, workflow controls outside `Form bottom`, text/icon controls without inline width, field sections without grids, calculated-looking editable fields, applicant/profile snapshot fields that are editable, lookup/autofill target fields that are editable without a documented business reason, runtime-sensitive picker/image attrs, and excessive arbitrary decision/status colors.

## Advanced Control Reference

Use `docs/collection-01.md#ai-training-approval-form-control-study` when a generated approval form needs broader native controls beyond the current proven-safe set. That export shows many control types in one app-level approval form, including tabs, `flex_grid`, rich text, checkbox multi-choice, percent, file/image upload, user/department/location/cost center pickers, metadata, lookup, lookup-list, sublists, data-list display, action buttons, Action Panel, and Flow History.

Treat the AI Training export as a read-only control anatomy reference. It is not a final generated-app baseline because it contains only a submission page and intentionally exercises controls that are still runtime-sensitive for generation. Promote its patterns only through staged generation and import testing.

---

<a id="yeeflow-approval-form-ui-ux-patterns"></a>

<!-- source: docs/collection-03.md#yeeflow-approval-form-ui-ux-patterns -->

# Yeeflow Approval Form UI/UX Patterns

This document defines the approval-form UI/UX standard extracted from `UI and UX design (1).yap`.

Use `docs/collection-03.md#yeeflow-application-design-system`, `docs/collection-03.md#yeeflow-approval-form-design-standards`, and `docs/collection-04.md#yeeflow-root-style-token-reference` for the current reusable approval-form standard. Generated approval forms should use native tokens for form surfaces, status/decision styling, spacing, and typography where schema-supported.

## Studied Approval Form

The mini app contains one approval form:

- Name: `Approval form`
- Key: `UI-AF`
- `Data.Forms[].ListID`: `0`
- `workflowType`: `2`
- Pages: one submission page in this minimal export
- Workflow nodes: `StartNoneEvent`, `SequenceFlow`, `EndNoneEvent`

The export is intentionally small. It proves the submission-page shell and the default placement of Action Panel and Flow History. It does not prove a full reviewer task page, but the same structure should be applied to generated task forms by default.

## Page Settings

The approval page form definition uses:

```json
{
  "attrs": {
    "container": {
      "cw": "2",
      "padding": [
        null,
        {
          "top": "--sp--s0",
          "right": "--sp--s0",
          "bottom": "--sp--s0",
          "left": "--sp--s0"
        }
      ]
    }
  }
}
```

Generator rule:

- Submission pages and task pages should use `attrs.container.cw = "2"`.
- Submission pages and task pages should use zero padding with `--sp--s0` on all sides.

## Container Tree

Studied submission-page tree:

```text
Approval form page
└─ container nv_label="Main"
   └─ container nv_label="Content"
      ├─ container nv_label="Form body"
      └─ container nv_label="Form bottom"
         ├─ workflowControlPanel
         └─ workflowHistory
```

`Main` style:

```json
{
  "gap": [null, "--sp--s0"],
  "direction": [null, "column"],
  "justify_content": [null, "flex-start"],
  "align_items": [null, "center"]
}
```

`Content`, `Form body`, and `Form bottom` use column layout with zero gap and `justify_content = flex-start`.

## Form Body

`Form body` is the container for business fields.

Generator rule:

- Put all normal requester or reviewer fields inside `Form body`.
- Use additional child section containers inside `Form body` for grouped fields when the form is not trivial.
- Do not place Action Panel or Flow History in `Form body`.
- Use `--c--background` for form cards/sections.
- Use `--c--neutral-light-active` for section borders and separators.
- Use `--fs--base` for normal labels and field text.
- Use `--fs--l` or `--fs--h6` for section headings.
- Use `--sp--s150` and `--sp--s200` for field grouping.

## Form Bottom

`Form bottom` is the container for workflow controls at the end of the `Content` container.

The studied export places these controls in order:

1. `workflowControlPanel`
2. `workflowHistory`

Action Panel control:

```json
{
  "type": "workflowControlPanel",
  "attrs": {
    "show-task-panel": true,
    "rejectValidation": true,
    "align": "center"
  }
}
```

Flow History control:

```json
{
  "type": "workflowHistory",
  "attrs": {
    "show-history": true
  }
}
```

Generator rule:

- Include both Action Panel and Flow History by default.
- Place both controls inside `Form bottom`.
- Place `Form bottom` after `Form body`.
- Omit either control only when the user explicitly requests it or a real export proves the form type should omit it.
- Let Yeeflow's native task panel styles control approve/reject/save buttons unless a real export proves a style override.
- Use semantic status tokens for generated decision badges: `--c--primary`, `--c--success`, `--c--warning`, `--c--danger`, and neutral tokens.

## Task Form Pattern

The mini app has only a submission page, but future generated task pages should follow the same shell:

```text
Task form page
└─ Main
   └─ Content
      ├─ Form body
      │  ├─ readonly submitted fields
      │  └─ reviewer decision fields
      └─ Form bottom
         ├─ workflowControlPanel
         └─ workflowHistory
```

Approval/task pages should mirror submitted request fields as readonly unless the field is irrelevant to the reviewer.

## Naming Rules

Use `nv_label` names:

- `Main`
- `Content`
- `Form body`
- `Form bottom`

Recommended section names inside `Form body`:

- `Request summary`
- `Applicant details`
- `Request details`
- `Approval decision`
- `Reviewer notes`

## Generator Rules

Approval-form generators should:

- Keep `Data.Forms[].ListID = 0` for app-level approval forms.
- Keep generated FlowKey/form key fresh for every package.
- Use full-width and zero-padding page settings.
- Wrap content in `Main` and `Content`.
- Put business fields inside `Form body`.
- Put Action Panel and Flow History inside `Form bottom`.
- Place `Form bottom` at the end of `Content`.
- Use native controls before custom code.
- Preserve workflow action validation against the normalized action reference.

## Validator Recommendations

Safe warnings:

- Missing full-width `cw = "2"`.
- Missing zero padding.
- Missing `Main`.
- Missing `Content`.
- Missing `Form body`.
- Missing `Form bottom`.
- `workflowControlPanel` outside `Form bottom`.
- `workflowHistory` outside `Form bottom`.
- Excessive arbitrary hard-coded decision/status colors where Yeeflow semantic root tokens are available.

Current caution:

- Do not make `Form body` and `Form bottom` mandatory errors yet, because existing generated baselines predate this standard.
- Promote to final-mode generator errors only after the first generated UI/UX standard test package passes local validation, runtime import/open, and export-back comparison.

## Future Approval-Form Test

The first generated approval-form UI/UX test should include:

- one submission page
- one reviewer task page
- request fields inside `Form body`
- readonly mirrored fields on the task page
- reviewer decision controls inside task-page `Form body`
- Action Panel and Flow History inside task-page `Form bottom`
- simple workflow graph only

---

<a id="yeeflow-control-configuration-reference"></a>

<!-- source: docs/collection-03.md#yeeflow-control-configuration-reference -->

# Yeeflow Control Configuration Reference

Sources studied read-only:

- `<downloads>/control-configurations.json`

The source contains 93 configuration entries covering 93 control types.

## Support Groups

| Group | Control types |
| --- | --- |
| provenSafe | container, currency, datepicker, heading, input, input_number, list, lookup, radio, section, switch, textarea, workflowControlPanel, workflowHistory |
| schemaSupportedRuntimeUnproven | checkbox, daterange, hyperlink, percent, rate, richtext, tag, time |
| environmentDependent | cost-center-picker, document-embed, document-library, file-upload, icon-upload, identity-picker, location-picker, organization-picker, signer |
| riskyDeferred | barcode, codein, list-qrcode, lookup-list, metadata, mutiple-metadata, print |
| schemaSupportedUnclassified | action_button, ak-tabs-tab, aktabs, alert, applicantInfo, apply-button, bar-chart, check-filter, check-range, collection, colorblock-chart, comments, dashboard-chart, data-list, date-filter, dropbar, dynamic-field, dynamic-file, dynamic-image, dynamic-user, embed, flex_grid, funnel-chart, gallery, gantt, gap, hierarchy-filter, icon, icon_list, kanban, line, line-chart, picture, pie-chart, pivot-table, progress, progress-circle, radio-filter, range-filter, relative-period, remove-filters, search-filter, section-column, select-filter, sorting-filers, sorting-filters, steps-bar, summary, table-v2, text-editor, timeline-h, timeline-v, timer, toggle, wflog |

## Common Properties

Common approval/dashboard control properties include:

- `binding`: variable or field binding for persistence and two-way value updates.
- `readonly`: static readonly flag.
- `label` and `label_var`: static or calculated label.
- `displayLabel`: label visibility.
- `attrs.required`: required input behavior.
- `attrs.placeholder`: placeholder text.
- `attrs.control_display`: dynamic display, readonly/write, or style rules.
- `attrs.control_event_rule`: action triggered after change.
- `attrs.control_validation`: custom control validators.
- `attrs.lablay`, `attrs.labWid`, `attrs.labWidUnit`, `attrs.verAli`: label layout and alignment.

## Control Inventory

| Control type | Support level | Key property paths |
| --- | --- | --- |
| action_button | schema-supported-unclassified | attrs.action-type, attrs.align, attrs.button-size, attrs.button-style, attrs.control_action, attrs.control_display, attrs.cusize.w, attrs.cusize.wu, attrs.data.form.AppID, attrs.data.form.ListSetID, attrs.data.form.ProcKey, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.page.AppID, attrs.data.page.ListSetID, attrs.data.page.PageID, attrs.icon |
| ak-tabs-tab | schema-supported-unclassified | attrs.control_display, attrs.icon, attrs.icon-type, attrs.isDefault, attrs.isDesignDefault, attrs.pic.url, attrs.pic.variable, children, label, label_var |
| aktabs | schema-supported-unclassified | attrs.control_display, attrs.header-stick, attrs.header-zindex, attrs.stick-offset, attrs.stick-offsetu, attrs.tabs-tabposition, children, default-var |
| alert | schema-supported-unclassified | attrs.alert.desc.value, attrs.alert.desc.variable, attrs.alert.dismissbtn, attrs.alert.title.value, attrs.alert.title.variable, attrs.alert.type, attrs.control_display |
| applicantInfo | schema-supported-unclassified | attrs.canDelegate, attrs.canFold, attrs.canFold-default, attrs.col, attrs.collapsed-icon, attrs.control_display, attrs.control_event_rule, attrs.displayItemLabel, attrs.icon, attrs.identity-limits.type, attrs.identity-limits.value, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.user-attribute.layout, attrs.verAli, displayLabel, label |
| apply-button | schema-supported-unclassified | attrs.align, attrs.button-size, attrs.button-style, attrs.control_display, attrs.cusize.w, attrs.cusize.wu, attrs.data.page.AppID, attrs.data.page.ListSetID, attrs.data.page.PageID, attrs.icon, attrs.icon-posi, attrs.icon-space, attrs.icon-spaceu, attrs.icon-type, attrs.label.text, attrs.label.variable, attrs.link.opentype, attrs.link.url |
| bar-chart | schema-supported-unclassified | attrs.chart.autoEllipsis, attrs.chart.autoHide, attrs.chart.autoRotate, attrs.chart.axis-labels, attrs.chart.axis-line, attrs.chart.grid, attrs.chart.height, attrs.control_display, attrs.legend.display, attrs.legend.position, attrs.title.align-t, attrs.title.display, attrs.title.position, attrs.title.title.value, attrs.title.title.variable, attrs.tooltip.display, attrs.tooltip.suffix.value, attrs.tooltip.suffix.variable |
| barcode | risky-deferred | attrs.control_display, attrs.displayValue, attrs.fallback.emp, attrs.fallback.et, attrs.textPosition, attrs.type, attrs.value.value, attrs.value.variable |
| check-filter | schema-supported-unclassified | attrs.apply_btn, attrs.apply_t, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.data.filter, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.sort, attrs.displayStyle, attrs.display_f, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.layout, attrs.less-count, attrs.less-text |
| check-range | schema-supported-unclassified | attrs.apply_btn, attrs.apply_t, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.displayStyle, attrs.displayThousandths, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.layout, attrs.less-count, attrs.less-text, attrs.more-enable, attrs.more-text, attrs.options, attrs.placeholder, attrs.prefix.value |
| checkbox | schema-supported-runtime-unproven | attrs.choices, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.displayStyle, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.layout, attrs.placeholder, attrs.required, attrs.show_control, attrs.verAli, binding, displayLabel, label |
| codein | risky-deferred | attrs.codein-script, attrs.codein-script-param |
| collection | schema-supported-unclassified | attrs.actions, attrs.control_display, attrs.data.cusize.w, attrs.data.cusize.wu, attrs.data.cusl.opentype, attrs.data.cusl.value, attrs.data.cusl.variable, attrs.data.dist, attrs.data.disv, attrs.data.filter, attrs.data.fulltext, attrs.data.limit, attrs.data.link, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.modalsize, attrs.data.more-link.opentype |
| colorblock-chart | schema-supported-unclassified | attrs.chart.autoEllipsis, attrs.chart.autoHide, attrs.chart.autoRotate, attrs.chart.axis-labels, attrs.chart.axis-line, attrs.chart.grid, attrs.chart.height, attrs.control_display, attrs.legend.display, attrs.legend.position, attrs.title.align-t, attrs.title.display, attrs.title.position, attrs.title.title.value, attrs.title.title.variable, attrs.tooltip.display, attrs.tooltip.suffix.value, attrs.tooltip.suffix.variable |
| comments | schema-supported-unclassified | attrs.allowUpload, attrs.control_display, attrs.inputtype, attrs.richtext-choose-toolbar, attrs.richtext-type |
| container | proven-safe | attrs.action-type, attrs.content.htmltag, attrs.content.link.opentype, attrs.content.link.value, attrs.content.link.variable, attrs.control_action, attrs.control_display, attrs.cusize.w, attrs.cusize.wu, attrs.data.form.AppID, attrs.data.form.ListSetID, attrs.data.form.ProcKey, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.page.AppID, attrs.data.page.ListSetID, attrs.data.page.PageID |
| cost-center-picker | environment-dependent | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.identity-maxselection, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.multiple, attrs.placeholder, attrs.required, attrs.verAli, binding, displayLabel, label, label_var, readonly |
| currency | proven-safe | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.currencyCode, attrs.displayFormat, attrs.displayThousandths, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.number_max, attrs.number_min, attrs.number_step, attrs.placeholder, attrs.required, attrs.rounded-to, attrs.verAli |
| dashboard-chart | schema-supported-unclassified | attrs.chart.height, attrs.control_display, exts.AppID, exts.ListID, exts.ListSetID, exts.chartType, exts.label, exts.scaleRange.end, exts.scaleRange.start, exts.settings.Conditions, exts.settings.values.0, label |
| data-list | schema-supported-unclassified | attrs.caption.add, attrs.caption.addtext, attrs.caption.cusize.w, attrs.caption.cusize.wu, attrs.caption.defaultvalue, attrs.caption.display, attrs.caption.layout, attrs.caption.modalsize, attrs.caption.op, attrs.caption.placeholder, attrs.caption.search, attrs.caption.showmore, attrs.caption.title, attrs.control_display, attrs.cusize.w, attrs.cusize.wu, attrs.data.align, attrs.data.dist |
| date-filter | schema-supported-unclassified | attrs.apply_btn, attrs.apply_t, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.end-placeholder, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.layout, attrs.start-placeholder, attrs.verAli, binding, displayLabel, label, readonly |
| datepicker | proven-safe | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.date_picker, attrs.date_type, attrs.dateformat, attrs.disable_now_button, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.minuteStep, attrs.placeholder, attrs.required, attrs.showtime, attrs.verAli, binding |
| daterange | schema-supported-runtime-unproven | attrs.binding-date-range, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.date_begin, attrs.date_end, attrs.date_picker, attrs.date_type, attrs.dateformat, attrs.disable_now_button, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.layout, attrs.minuteStep, attrs.placeholder_range |
| document-embed | environment-dependent | attrs.appearance.height, attrs.appearance.heightu, attrs.control_display, attrs.doc-source, attrs.fallback.emp, attrs.fallback.et |
| document-library | environment-dependent | attrs.caption.add, attrs.caption.addtext, attrs.caption.cusize.w, attrs.caption.cusize.wu, attrs.caption.defaultvalue, attrs.caption.display, attrs.caption.layout, attrs.caption.modalsize, attrs.caption.op, attrs.caption.placeholder, attrs.caption.search, attrs.caption.title, attrs.control_display, attrs.cusize.w, attrs.cusize.wu, attrs.data.align, attrs.data.customPath, attrs.data.dist |
| dropbar | schema-supported-unclassified | attrs.button.after_icon, attrs.button.after_icon_type, attrs.button.align, attrs.button.before_icon, attrs.button.before_icon_type, attrs.button.button_type, attrs.button.img.url, attrs.button.img.variable, attrs.button.size.h, attrs.button.size.w, attrs.button.sizetype, attrs.button.text.value, attrs.button.text.variable, attrs.content.content_type, attrs.content.richtext, attrs.content.simpletext.value, attrs.content.simpletext.variable, attrs.control_display |
| dynamic-field | schema-supported-unclassified | attrs.control_display, attrs.data.cusize.w, attrs.data.cusize.wu, attrs.data.cusl.opentype, attrs.data.cusl.value, attrs.data.cusl.variable, attrs.data.link, attrs.data.modalsize, attrs.data.op, attrs.delimiter, attrs.dis-f, attrs.fallback.et, attrs.i-len, attrs.obj-f, attrs.prefix, attrs.source, attrs.suffix, attrs.t-len |
| dynamic-file | schema-supported-unclassified | attrs.control_display, attrs.dynamic-source, attrs.fallback.et, attrs.i-len, attrs.obj-f, attrs.source, attrs.type_icon_show |
| dynamic-image | schema-supported-unclassified | attrs.control_display, attrs.dynamic-source.url, attrs.dynamic-source.variable, attrs.emp, attrs.et, attrs.i-len, attrs.is, attrs.obj-f, attrs.preview_image, attrs.setting.align, attrs.setting.cg, attrs.setting.cgu, attrs.setting.col, attrs.setting.fit, attrs.setting.fix_width, attrs.setting.fix_widthU, attrs.setting.gap, attrs.setting.gapu |
| dynamic-user | schema-supported-unclassified | attrs.addition_fields, attrs.align, attrs.control_display, attrs.delimiter, attrs.display_name, attrs.fallback.et, attrs.i-len, attrs.image-position, attrs.layout, attrs.obj-f, attrs.show_picture, attrs.source, attrs.users.value, attrs.users.variable |
| embed | schema-supported-unclassified | attrs.code.value, attrs.code.variable, attrs.control_display, attrs.height, attrs.heightu |
| file-upload | environment-dependent | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.file_maxcount, attrs.file_multiple, attrs.file_types.cusValue, attrs.file_types.value, attrs.file_typeslimit, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.maxsize, attrs.required, attrs.type_icon_show, attrs.upload_btn.value, attrs.upload_btn.variable |
| flex_grid | schema-supported-unclassified | attrs.areas, attrs.canFold, attrs.canFold-default, attrs.collapsed-icon, attrs.columns, attrs.control_display, attrs.dense, attrs.direction, attrs.icon, attrs.rows, children, displayLabel, label |
| funnel-chart | schema-supported-unclassified | attrs.chart.axis-labels, attrs.chart.height, attrs.control_display, attrs.legend.display, attrs.legend.position, attrs.title.align-t, attrs.title.display, attrs.title.position, attrs.title.title.value, attrs.title.title.variable, attrs.tooltip.display, attrs.tooltip.suffix.value, attrs.tooltip.suffix.variable, exts.AppID, exts.ListID, exts.ListSetID, exts.chartType, exts.settings.Conditions |
| gallery | schema-supported-unclassified | attrs.control_display, attrs.data.align, attrs.data.cusize.w, attrs.data.cusize.wu, attrs.data.cusl.opentype, attrs.data.cusl.value, attrs.data.cusl.variable, attrs.data.dist, attrs.data.disv, attrs.data.filter, attrs.data.imgf, attrs.data.is, attrs.data.limit, attrs.data.link, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.medty |
| gantt | schema-supported-unclassified | attrs.control_display, attrs.data.filter, attrs.data.gantt.atts.allowadd, attrs.data.gantt.atts.auto_scheduling, attrs.data.gantt.atts.criticalpath, attrs.data.gantt.atts.cusize.w, attrs.data.gantt.atts.cusize.wu, attrs.data.gantt.atts.hight, attrs.data.gantt.atts.hightu, attrs.data.gantt.atts.linkLayout, attrs.data.gantt.atts.linkPopup, attrs.data.gantt.atts.linkcusurl.opentype, attrs.data.gantt.atts.linkcusurl.value, attrs.data.gantt.atts.linkcusurl.variable, attrs.data.gantt.atts.milestone, attrs.data.gantt.atts.modalsize, attrs.data.gantt.atts.passvalues, attrs.data.gantt.atts.readonly |
| gap | schema-supported-unclassified | attrs.control_display, attrs.space, attrs.spaceu |
| heading | proven-safe | attrs.control_display, attrs.headc.align, attrs.headc.fontsize, attrs.headc.link.opentype, attrs.headc.link.url, attrs.headc.link.variable, attrs.headc.title.text-len, attrs.headc.title.value, attrs.headc.title.variable |
| hierarchy-filter | schema-supported-unclassified | attrs.apply_btn, attrs.apply_t, attrs.canFold, attrs.canFold-default, attrs.categoryId, attrs.child_f, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.data.filter, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.sort, attrs.displayStyle, attrs.display_f, attrs.hierarchical-select, attrs.hierarchy |
| hyperlink | schema-supported-runtime-unproven | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.hyperlink_buttonname, attrs.hyperlink_open, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.placeholder, attrs.required, attrs.verAli, binding, displayLabel, label, label_var, readonly |
| icon | schema-supported-unclassified | attrs.control_display, attrs.icon.align, attrs.icon.icon, attrs.icon.shape, attrs.icon.url.opentype, attrs.icon.url.value, attrs.icon.url.variable, attrs.icon.view |
| icon_list | schema-supported-unclassified | attrs.control_display, attrs.data.lay, attrs.data.links |
| icon-upload | environment-dependent | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.controlmultiple, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.maxselection, attrs.picture_size_limit, attrs.required, attrs.verAli, binding, displayLabel, label, label_var, readonly |
| identity-picker | environment-dependent | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.identity-limits.type, attrs.identity-limits.value, attrs.identity-maxselection, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.multiple, attrs.placeholder, attrs.required, attrs.verAli, binding, displayLabel, label |
| input | proven-safe | attrs.allowScan, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.input-maxlength, attrs.input_change_action, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.minnum, attrs.placeholder, attrs.required, attrs.verAli, binding, displayLabel, label |
| input_number | proven-safe | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.displayThousandths, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.number_max, attrs.number_min, attrs.number_step, attrs.placeholder, attrs.required, attrs.rounded-to, attrs.verAli, binding, displayLabel |
| kanban | schema-supported-unclassified | attrs.actions, attrs.cateField, attrs.cateSort.SortByDesc, attrs.cateSort.SortName, attrs.categories.collapse, attrs.categories.summaryNum, attrs.control_display, attrs.data.filter, attrs.data.list.AppID, attrs.data.list.ListID, attrs.data.list.ListSetID, attrs.data.sort, attrs.data.sortingfilter, attrs.footer.show, attrs.includeUncate, attrs.newItem.add, attrs.newItem.addtext, attrs.newItem.cusize.w |
| line | schema-supported-unclassified | attrs.align, attrs.control_display, attrs.eletype, attrs.icon, attrs.line-type, attrs.text.value, attrs.text.variable, attrs.width, attrs.wu |
| line-chart | schema-supported-unclassified | attrs.chart.autoEllipsis, attrs.chart.autoHide, attrs.chart.autoRotate, attrs.chart.axis-labels, attrs.chart.axis-line, attrs.chart.grid, attrs.chart.height, attrs.control_display, attrs.legend.display, attrs.legend.position, attrs.title.align-t, attrs.title.display, attrs.title.position, attrs.title.title.value, attrs.title.title.variable, attrs.tooltip.display, attrs.tooltip.suffix.value, attrs.tooltip.suffix.variable |
| list | proven-safe | attrs.actions, attrs.canFold, attrs.canFold-default, attrs.collapsed-icon, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_validation, attrs.fallback.et, attrs.icon, attrs.list-display-preference, attrs.list-fields, attrs.list-fields-summary, attrs.list_add_btn_text.value, attrs.list_add_btn_text.variable, attrs.list_import_btn_text.value, attrs.list_import_btn_text.variable, attrs.list_row_allow_import |
| list-qrcode | risky-deferred | attrs.align, attrs.control_display, attrs.qr-code-link.customUrl.url, attrs.qr-code-link.customUrl.variable, attrs.qr-code-link.taskStep, attrs.qr-code-link.type, attrs.qr-code-size |
| location-picker | environment-dependent | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.placeholder, attrs.required, attrs.verAli, binding, displayLabel, label, label_var, readonly, value |
| lookup | proven-safe | attrs.addition, attrs.appid, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.cusize.w, attrs.cusize.wu, attrs.cusurl.opentype, attrs.cusurl.value, attrs.cusurl.variable, attrs.displayStyle, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.link, attrs.list_tooltip_field |
| lookup-list | risky-deferred | attrs.addition, attrs.addition-list, attrs.alignment, attrs.appid, attrs.button-size, attrs.button-style, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.display-selected-items, attrs.icon, attrs.icon-posi, attrs.icon-space, attrs.icon-spaceu, attrs.icon-type, attrs.labWid |
| metadata | risky-deferred | attrs.categoryId, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.metadata-treeselect, attrs.parentId, attrs.placeholder, attrs.required, attrs.source, attrs.verAli, binding, displayLabel, label |
| mutiple-metadata | risky-deferred | attrs.categoryId, attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.metadata-treeselect, attrs.parentId, attrs.placeholder, attrs.required, attrs.source, attrs.verAli, binding, displayLabel, label |
| organization-picker | environment-dependent | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.hierarchical-select, attrs.identity-maxselection, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.metadata-treeselect, attrs.multiple, attrs.parentId, attrs.placeholder, attrs.required, attrs.verAli, binding, displayLabel |
| percent | schema-supported-runtime-unproven | attrs.comment-allow, attrs.comment-trigger, attrs.control_display, attrs.control_event_rule, attrs.control_validation, attrs.labWid, attrs.labWidUnit, attrs.lablay, attrs.number_max, attrs.number_min, attrs.placeholder, attrs.required, attrs.rounded-to, attrs.verAli, binding, displayLabel, label, label_var |
| picture | schema-supported-unclassified | attrs.control_display, attrs.picture.align, attrs.picture.caption.type, attrs.picture.caption.url, attrs.picture.caption.variable, attrs.picture.link.lightBox, attrs.picture.link.opentype, attrs.picture.link.type, attrs.picture.link.url, attrs.picture.link.variable, attrs.picture.pic.size.h, attrs.picture.pic.size.w, attrs.picture.pic.sizetype, attrs.picture.pic.url, attrs.picture.pic.variable |
| pie-chart | schema-supported-unclassified | attrs.chart.axis-labels, attrs.chart.axis-line, attrs.chart.height, attrs.control_display, attrs.legend.display, attrs.legend.position, attrs.legend.position2, attrs.title.align-t, attrs.title.display, attrs.title.position, attrs.title.title.value, attrs.title.title.variable, attrs.tooltip.display, attrs.tooltip.suffix.value, attrs.tooltip.suffix.variable, exts.AppID, exts.ListID, exts.ListSetID |

## Generator Guidance

- Generate only proven-safe controls by default.
- Use schema-supported runtime-unproven controls only in isolated packages with explicit runtime proof.
- Treat environment-dependent controls such as identity, organization, location, cost-center, file, icon, and signer controls as deferred unless the target tenant metadata is provided.
- Preserve `attrs.control_display`, `attrs.control_event_rule`, and `attrs.control_validation` shapes from real exports; do not invent runtime logic.
- For lookup controls, require resolved app/listset/list/listfield metadata.

## Validator Guidance

- Unknown control types should warn or fail in generator-final mode depending on context.
- Invalid enum values should be reported when the path is present in the normalized enum index.
- Primitive value-type mismatches should warn unless the type is already proven and required.
- Missing `binding` should warn for generated value-entry controls when persistence is expected.

---

<a id="yeeflow-control-field-generation-rules"></a>

<!-- source: docs/collection-03.md#yeeflow-control-field-generation-rules -->

# Yeeflow Control And Field Generation Rules

This document operationalizes the normalized control and field references for Yeeflow generators.

## Approval Form Controls

- Consult `control-configurations.normalized.json` before using a control type.
- Consult `yeeflow-expression-functions.normalized.json`, `yeeflow-expression-operators.normalized.json`, and `docs/collection-04.md#yeeflow-expression-generation-rules` before generating calculated controls, dynamic display rules, custom validation rules, lookup filters, or workflow transition conditions.
- Default to proven-safe controls: input, textarea, input_number, currency, radio, switch, datepicker, lookup, list, container, section, heading, workflowControlPanel, and workflowHistory.
- Require `binding` for generated value-entry controls when the value must persist.
- Use `attrs.required` for required behavior and keep custom validators in `attrs.control_validation`.
- Use `attrs.control_display` for dynamic show/hide/readonly/style only when the rule shape is copied from a studied export.
- Use `attrs.control_event_rule` only when the action is explicitly modeled and validated.
- Yeeflow page/control JSON often stores style and config values as `[null, value]`; schema validators should unwrap that representation before checking primitive value type or enum membership.
- For generated form design quality, use `docs/collection-04.md#yeeflow-form-design-quality-rules`: page background on `formdef.attrs.background`, `Form header` for request summaries, inline text/icon widths, square icon badges, and two-column `flex_grid` field sections.
- Runtime V2 repaired some environment-sensitive controls by re-adding native controls with minimal attrs. Treat `location-picker`, department/organization picker, `cost-center-picker`, `metadata`, and `icon-upload` as runtime-sensitive unless a focused export proves the exact generated shape.
- File upload controls in the Runtime V2 export include `attrs.ver = 1`; generated file uploads should include this until another export proves otherwise.
- Image/icon upload in Runtime V2 uses `attrs.controlmultiple = true`; generated `icon-upload` should follow that or fall back.
- Detect calculated-looking fields. Use native `calculated` controls for patterns such as `Subtotal = Quantity * Unit Price`; do not generate editable input controls for formulas without an explicit reason.
- Applicant/profile snapshot controls and lookup-autofill target controls should be generated with control-level `readonly: true` unless they are intentional business inputs. This includes applicant employee ID/name/department/boarding date/status/email/line manager and product name/type/unit price/row subtotal/total amount. Keep the applicant/requester picker editable only where the requester is allowed to choose or confirm the applicant.
- Generate expression arrays with exact Yeeflow token shapes. Variables must use `exprType: "variable"`, `type: "expr"`, and `valueType` limited to `number`, `text`, `date`, or `boolean`. Do not invent expression functions or operators.

### AI Training Approval Control Reference

`docs/collection-01.md#ai-training-approval-form-control-study` documents the focused `AI Training.yap` approval form export. Use it as an export-backed anatomy reference for broad approval-form controls, not as a complete generated-app runtime baseline.

Observed control-to-variable rules:

- `input`, `textarea`, `richtext`, `radio`, and `checkbox` bind text variables.
- `input_number`, `percent`, and `currency` bind number variables.
- `switch` binds boolean variables.
- `datepicker`, `daterange`, and `time` bind date variables.
- `file-upload`, `icon-upload`, `identity-picker`, `organization-picker`, `location-picker`, `cost-center-picker`, `metadata`, `mutiple-metadata`, `lookup`, and `list` bind their corresponding advanced variable types.
- `daterange` uses the From variable as `binding` and stores the To variable in `attrs["binding-date-range"]`.
- `list` controls require a matching `variables.basic[]` list variable whose `value` points to a `variables.listref[]` entry; list child controls use `attrs.list_field`, `attrs.list_field_binding`, and `attrs.list_control_id`.

Generation posture:

- The sample improves schema knowledge for `percent`, `checkbox`, `richtext`, file/image upload, pickers, metadata, lookup-list, sublists, tabs, action buttons, and data-list display controls.
- These patterns remain runtime-sensitive for generated packages until each is proven by a focused generated import test.
- For generator final mode, unresolved lookup sources, metadata category IDs, picker dependencies, or listref structures are stop conditions.

### Runtime Coverage Consolidation

Use `fixtures/runtime-test-specs/approval-form-control-runtime-coverage.json` and `docs/collection-01.md#approval-form-control-runtime-coverage-matrix` as the current control safety matrix.

Runtime-proven generated controls now include:

- `input`, `textarea`, `input_number`, `currency`, `checkbox`, `switch`, `datepicker`
- `percent`, `time`, `hyperlink`, `rate`, `calculated`
- `file-upload` and `icon-upload` for workflow-form upload/display, with binary list persistence still deferred
- editable single-select `identity-picker`, with text-summary persistence
- internal packaged single-select `lookup`
- workflow-form `list` / `listref` row capture and review display
- `workflowControlPanel` and `workflowHistory`

Partial or environment-dependent controls:

- `richtext`, `radio`, and `daterange` need focused persistence checks.
- `organization-picker`, `location-picker`, and `cost-center-picker` render/open but depend on tenant metadata and retention behavior.

Deferred controls:

- Stage 5 metadata/tag controls are intentionally skipped for now: `metadata`, `mutiple-metadata`, and `tag`.
- `lookup-list`, embedded `data-list`, `signer`, and tab/layout-only controls need separate isolated packages.

Lookup persistence rule:

- A raw lookup variable mapped into a plain text data-list field persisted the internal local row ID in v6.
- Use `attrs.addition[]` autofill variables or explicit summary variables when generated apps need readable lookup text such as product name, code, category, or price.
- Autofill target controls should normally be readonly; the lookup selection is the editable user input.
- Store the raw lookup row ID only when downstream logic intentionally needs the row ID or the target field is a proven lookup-compatible data-list field.

List/listref persistence rule:

- Generated list/listref controls can render a row table, add/edit child values, submit, show values on reviewer tasks, and complete approval.
- Direct child-row-to-data-list persistence is not proven. Persist a text summary or model a separate child list until row persistence is export-proven.

Sublist calculated field and summary rule:

- Updated `Approval Form Controls Test v6` proves row-level calculated fields inside a list control using `exprType: "variable_ctx"` current-object tokens.
- Add calculated row fields to both `variables.listref[].fields[]` and the submit-page list control `attrs["list-fields"][]`.
- Store row formulas in the child control at `control.attrs.calculated`.
- Use `attrs["list-fields-summary"]` on the parent list control for Sum/Average summaries.
- Bind numeric summaries to top-level number variables with `{ "prefix": "__variables_", "value": "<VariableId>" }`.
- FlowKey/form key text must not be a lowercase substring of reserved JSON property names used by summary binding objects. Known failure: FlowKey `EFI` was replaced inside `prefix`, corrupting it to `pr<runtimeFlowKey>x` and breaking summary-to-variable binding. Avoid keys that collide with `prefix`, `suffix`, `field`, `fields`, `profile`, `definition`, `workflow`, `variable`, `filter`, `ref`, `href`, `control`, `collection`, `condition`, `expression`, `attributes`, `actions`, or `binding`.
- Use summary-bound variables such as `TotalAmount` for display, ContentList persistence, and workflow routing.
- Keep direct child-row-to-data-list persistence deferred; summary values and text summaries are the safe durable persistence path.

## Data List Fields

- Consult `field-configurations.normalized.json` before using a field/control type.
- Preserve native Title metadata in every generated list:
  - `FieldName: "Title"`
  - `Status: 0`
  - `IsSystem: true`
  - `IsIndex: true`
  - `FieldIndex: 0` when present in the package style
- Use `Rules.required`, `Rules.placeholder`, `Rules.choices`, numeric, date, and lookup rules only when type-compatible.
- For internal app-level lookup samples, local target sample IDs may be included in `ReplaceIds`.
- For standalone/external lookup samples, external IDs must be excluded from `ReplaceIds`.

## Fallback And Stop Rules

- Prefer Text fallback for user, organization, location, cost center, tag, metadata, hyperlink, signer, and file/image display values until native runtime is proven.
- For data-list persistence, prefer Text fallback for requester/user values unless a focused data-list identity/user field export proves the native persisted field shape. Approval forms may still use identity-picker for current-user workflow assignment when that pattern is proven.
- Prefer Decimal fallback for percent and rate until native display behavior is proven.
- Defer signer, metadata tree, lookup-list, embedded data-list display, and sublist row persistence unless a focused export/import proves the shape. If explicitly generating native file/image controls, follow the Runtime V2/v3 attrs, use workflow-form usage as proven, and keep binary data-list persistence deferred.
- Do not guess enum values, picker max-selection settings, lookup metadata, or calculated-column formulas.

## Validator Policy

- Unknown control or field type: warning in compatibility, stricter warning/error in generator final where the generator is expected to know the shape.
- Invalid enum value: warning or error depending on proven support and final stage.
- Invalid primitive value type: warning unless it breaks a required proven field/control.
- Missing lookup target metadata: error for generated packages.
- Runtime-unproven types: warning with a stop condition for production-like generation.

## Skill Scope Decision

A standalone `yeeflow-control-field-schema` skill was not created in this learning pass. The reference is cross-cutting, but the operational use belongs inside the existing approval-form, data-list, and application generators. Keeping the normalized references in the project and pointing existing skills at them avoids a duplicated skill surface while still allowing a dedicated schema skill later if this grows into an independent authoring workflow.

---

<a id="yeeflow-control-to-field-mapping"></a>

<!-- source: docs/collection-03.md#yeeflow-control-to-field-mapping -->

# Yeeflow Control To Field Mapping

Use this table when planning app-level persistence from approval form controls to data-list fields.

| Approval/control type | Data-list field type | Data-list control | Support | Fallback / notes |
| --- | --- | --- | --- | --- |
| calculated-column | Calculated | calculated-column | data-list-only |  |
| calculated | Decimal | calculated / readonly display | generation-safe-with-export-backed-expression | Use for approval-form calculated display values such as Subtotal = Quantity * Unit Price. Persist calculated result only when ContentList mapping is type-compatible or store source values and recalculate downstream. |
| checkbox | Text | checkbox | schema-supported-runtime-unproven | radio or text label |
| cost-center-picker | Text | input | fallback |  |
| currency | Decimal | currency | generation-safe-with-rules |  |
| datepicker | Datetime | datepicker | generation-safe-with-rules |  |
| file-upload | File | file-upload | deferred |  |
| hyperlink | Text | hyperlink | fallback |  |
| icon-upload | File | icon-upload | deferred |  |
| identity-picker | Text | input | fallback | Text/input display value unless user/person field metadata is proven |
| input | Text | input | generation-safe | Default text mapping. |
| input_number | Decimal | input_number | generation-safe |  |
| list | Text | list | schema-supported-persistence-deferred |  |
| location-picker | Text | input | fallback |  |
| lookup | Text | lookup | generation-safe-with-local-target | Requires appid/listsetid/listid/listfield and resolved target list. |
| lookup-list | Text | lookup-list | deferred |  |
| metadata | Text | metadata | fallback |  |
| mutiple-metadata | Text | mutiple-metadata | fallback |  |
| organization-picker | Text | input | fallback |  |
| percent | Decimal | percent | schema-supported-runtime-unproven | input_number |
| radio | Text | radio | generation-safe | Use runtime-visible Rules.choices and store selected text; do not rely on Rules.Options alone. |
| rate | Decimal | rate | fallback |  |
| richtext | Text | richtext | schema-supported-runtime-unproven | textarea |
| signer | Text | signer | deferred |  |
| switch | Bit | switch | generation-safe |  |
| tag | Text | tag | fallback |  |
| textarea | Text | textarea | generation-safe | Use for multiline text where the list field stores Text. |
| time | Text | time | fallback | Text/input until time field runtime is proven |

## Rules

- Generation-safe mappings can be used in normal app packages after standard validation.
- Schema-supported mappings need isolated import/runtime proof before becoming defaults.
- Fallback mappings should store a simple display value in Text or Decimal until the native field runtime is proven.
- Deferred mappings must stop generation unless the user explicitly asks for research or provides an export proving the shape.
- `calculated-column` is data-list only; do not model it as an approval variable unless a real export proves the workflow interaction.
- Approval-form `calculated` controls are display/calculation controls, not normal editable inputs. Use them for calculated-looking fields when the expression can be safely built from known workflow variables.
