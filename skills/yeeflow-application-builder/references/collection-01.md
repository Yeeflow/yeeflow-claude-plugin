# skills/yeeflow-application-builder/references collection 1

This file combines 5 documents from `skills/yeeflow-application-builder/references/` for the Claude directory build. A reference to `skills/yeeflow-application-builder/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [application-design-quality-gates.md](#application-design-quality-gates)
- [application-planning-key-design-decisions.md](#application-planning-key-design-decisions)
- [business-decision-gates.md](#business-decision-gates)
- [business-solution-design-principles.md](#business-solution-design-principles)
- [requirement-to-yap-generation-lifecycle.md](#requirement-to-yap-generation-lifecycle)

---

<a id="application-design-quality-gates"></a>

<!-- source: skills/yeeflow-application-builder/references/collection-01.md#application-design-quality-gates -->

# Application Design Quality Gates

Use this reference before accepting a generated Yeeflow application package.

## Approval Form Layout Gate

Generated approval forms must follow the learned Yeeflow application design standard:

- page-level background on the page/form background only
- structural `Main` container
- `Content` container inside `Main`
- `Form body` container for business fields
- `Form bottom` container for workflow controls
- Action Panel and Flow History inside `Form bottom`
- Form header / request summary section
- two-column grid controls for normal fields
- full-row layout for textarea, upload, list/sublist, rich text, and long helper/guidance content
- learned Text control standard
- Text controls inline width by default
- meaningful `nv_label` for containers and controls

## Business Section Gate

The form should be organized around business operations, not only field order. Common sections:

- Request Summary
- Requester / Applicant Information
- Application Type
- Product / Service Selection
- Policy / Quota / Budget Check
- Attachments
- Remarks / Justification
- Review sections by role
- Form Bottom

## Validation Signals

Warn or fail readiness when:

- normal fields are direct children of section containers instead of grid controls
- Text controls are block-width by default without reason
- Action Panel or Flow History are outside Form bottom
- `Main` carries page background styling
- section containers lack meaningful `nv_label`
- generated form does not mirror submit/review task pages consistently
- applicant/profile snapshot fields are editable without a business reason
- an editable requester/applicant field changes applicant identity but has no field-change action to rerun profile snapshot and dependent quota/policy logic
- multi-product requirements are modeled as one top-level product lookup instead of a sublist/listref
- quota usage records lack a cycle field when the business cycle is employee anniversary or another non-calendar cycle

---

<a id="application-planning-key-design-decisions"></a>

<!-- source: skills/yeeflow-application-builder/references/collection-01.md#application-planning-key-design-decisions -->

# Application Planning Key Design Decisions

Use this reference during app planning and generation-readiness review.

## Requester / Applicant Model

When an app has a requester, applicant, employee, beneficiary, or request-for identity:

- decide whether the applicant is always the current user or can be changed for proxy submission
- if the applicant field is editable, bind field-change actions to reload dependent snapshot/profile/quota logic
- Current User may default `RequesterApplicant` on a new request
- applicant business logic must use `RequesterApplicant` or applicant snapshot variables after defaulting
- task viewers, reviewers, HR, finance, and admins must not overwrite applicant data from their own Current User context
- if the field has Default value = Current User, do not add a redundant Set variable step that also defaults it from Current User

## Profile Snapshot Model

For user-profile-derived business data:

- snapshot required profile fields into workflow/form variables
- make snapshot fields readonly unless HR is intentionally allowed to edit them
- persist snapshots through ContentList when they are needed for reporting/audit
- handle missing profile data with HR verification fallback
- never fall back to the current task viewer for applicant profile values after submission

## Quota / Eligibility Model

When a requirement includes annual quota, benefit eligibility, entitlement, or employee tenure:

- identify the quota cycle: calendar year, employee anniversary year, or custom cycle
- identify quota occupation timing: submission, final approval, or another event
- identify release behavior on rejection/cancel
- identify whether eligibility depends on hire/boarding date
- use numeric cycle fields when comparing usage by non-calendar cycles
- query usage records by applicant identity + cycle + active/occupied status
- use `arraySum` for usage aggregation
- ensure Query data filters compare against the same persisted field values and use the right operand mode: literal values in direct-value mode, workflow variables/calculations as expression-token arrays with `showCus: false`
- use form actions to rerun quota checks on initialization, driver-field change, and submit
- when quota is occupied on submission, create the usage record at workflow start/submission, not after final approval
- include in-progress/occupied records in future quota checks so concurrent in-flight requests consume quota
- update the usage record to approved/confirmed on final approval
- release, reject, delete, or otherwise exclude the usage record on rejection/cancel
- store a correlation key such as form instance id, workflow instance id, request instance key, or application number so the matching usage record can be updated later
- if update/delete is not runtime-proven, keep the create-on-submission behavior as a required v1 proof item and document HR/manual release fallback instead of leaving the usage list idle

Employee-anniversary example:

- `ApplicantBoardingYears = dateDiff(ApplicantBoardingDate, now(), "year", [])`
- if `ApplicantBoardingYears = 0`, effective family annual quota is `0`
- if `ApplicantBoardingYears > 0`, the applicant is eligible for that cycle
- persist numeric `QuotaCycleNumber` on usage records and query usage by the same number

## Multiple Product Selection

When users can request multiple products, services, benefits, or items:

- first choose the persistence model: workflow sublist summary only, direct child-row persistence, or a separate transaction item list
- design selection as a sublist/listref when users need to enter multiple rows in one request
- use a lookup as the first column
- autofill product type, name, price, and other derived fields
- make autofilled and calculated fields readonly
- calculate row subtotal with current-object expressions
- bind row subtotal summary to a top-level total variable
- use the total variable for quota, workflow routing, validation, and persistence
- persist a readable product summary when direct row persistence is not proven
- if row-level reporting is required, model a separate transaction item list and treat row creation/update as a focused runtime proof item before claiming it as complete

## Master / Reference Lists

When a requirement names a catalog, visitor directory, department list, product list, equipment list, cost center list, or other master data:

- decide whether it is generated as a real maintained v1 list, mapped as an external dependency, or deferred
- generate usable fields, views, current-standard Edit Item and View Item custom forms, and sample/reference rows when forms or lookups depend on the list
- do not use free-text fields where the business process requires controlled master-data selection
- do not leave master data as a placeholder concept in the plan while the generated app contains only transaction lists
- confirm which role maintains the list and which form, dashboard, or workflow reads it

## Availability / Stock / Capacity Model

For equipment borrowing, inventory, room booking, visitor capacity, quota, or similar availability-sensitive apps:

- classify availability as manual review only, query-based availability, or inventory/reservation based
- manual review may be valid for v1, but label it as review-only routing and avoid stock-control language
- query-based availability must read a maintained master/reference list or transaction list and show how the query result affects the form or workflow
- reservation/inventory control must define create/update/decrement/release timing and a runtime proof plan before it is treated as complete
- if automatic reservation is not proven, include a reviewer fallback and document the limitation

## Data List Runtime Purpose

Every generated v1 data list must have a declared runtime purpose:

- who maintains it
- who reads it
- which form action, workflow action, ContentList node, dashboard, or report writes to it
- whether it is used for calculations, routing, reporting, audit, or configuration

Do not include a configuration, usage, audit, or reporting list in v1 if nothing reads or writes it. Either use it in the app or mark it v2/deferred and omit it from the v1 package unless the user explicitly wants a placeholder.

For configuration lists such as attachment requirement rules:

- either query or lookup the list at runtime to drive guidance/validation
- or keep a static v1 mapping and remove/defer the list until runtime use is proven
- do not generate dead configuration lists that look maintained but never affect the form/process

## Form Action Planning

For dependent form logic:

- use page-load/init actions for first load
- use field-change actions when a key driver field changes
- run policy-critical recalculations on submit
- keep dependent calculations in ordered Set variable steps
- use multi-value Set variables only for independent assignments where order does not matter
- use `attrs.querydata_filters` for Query data filters
- use expression-token right operands, not HTML expression-button strings, when Query data filters compare list fields to workflow variables
- submit guard actions should call reusable checks, conditionally warn/confirm, then run Submit form
- conditional guard steps before Submit usually require `continue: true`

## Workflow Branch Coverage

For every workflow node with multiple outgoing branches:

- list all outgoing conditions during planning
- confirm that routing variables are required, auto-derived, or protected by fallback branches
- cover yes/no/empty/unexpected values when a branch depends on a choice flag
- route unknown or empty policy-driving values to review/fallback rather than a workflow dead end
- avoid reversing business meaning: custom/high-value/exception paths usually go to the specialist review branch, while normal paths continue to standard persistence

---

<a id="business-decision-gates"></a>

<!-- source: skills/yeeflow-application-builder/references/collection-01.md#business-decision-gates -->

# Business Decision Gates

Use this reference before generating a Yeeflow application from business requirements.

Business decision gates are choices the user or business owner must confirm because they change business behavior, not just implementation details. Do not hide these inside generic generation-readiness notes.

## Required Chat Behavior

When a user provides a requirement and asks Codex to build a Yeeflow application, the builder should:

1. Analyze the requirement.
2. Create the initial app plan/spec.
3. Identify business-critical clarification questions.
4. Present those questions directly in the Codex chat.
5. Stop and wait for the user's answers.
6. Only continue to generation after the user answers or explicitly approves default assumptions.

Use this format:

```text
Business clarification required before generation:

1. <Question>
   - Option A:
   - Option B:
   - Recommended default:
   - Why this matters:

2. <Question>
   - Option A:
   - Option B:
   - Recommended default:
   - Why this matters:

Generation is paused until these questions are answered or defaults are explicitly approved.
```

After outputting this block, do not continue to `.yap` generation in the same turn.

Stop before `.yap` generation when unresolved gates materially affect:

- approval roles or responsibility
- workflow route logic
- quota, budget, or entitlement policy
- status lifecycle
- data ownership
- pricing or amount ownership
- required attachments or evidence
- persistence timing
- dashboard inclusion in v1
- compliance/audit handling
- integration responsibility
- role permissions
- rejection, expiry, or resubmission policy
- what happens on approval, rejection, or resubmission

Acceptable ways to proceed:

- the user answers the decision
- the user explicitly approves a default assumption
- the user explicitly says to generate a prototype with documented placeholders

Do not proceed merely because Codex can choose a conservative assumption when the assumption changes business behavior.

## Examples

- Should quota reset by calendar year or employee anniversary year?
- Should quota be occupied on submission or final approval?
- Is manager approval mandatory?
- Is custom pricing fixed or manually entered?
- Which attachments are required by scenario?
- Should v1 include a home dashboard?
- Should rejected records be automatically expired or manually closed?
- Should approval route depend on amount threshold?

## Business Decisions vs Technical Assumptions

Business decisions:

- must be asked to the user
- must block generation if unanswered
- can proceed only if the user approves defaults

Technical assumptions:

- can be tested by Codex during generation/runtime
- should have fallback behavior
- should be documented in the plan/spec
- should not block generation unless they affect business correctness

Examples of technical assumptions:

- whether `getUserAttr(RequesterApplicant, ...)` works directly
- whether a tenant profile field is populated
- whether a dashboard widget binding needs a specific runtime pattern
- whether a form action step needs a reduced runtime proof
- whether Query data, conditional ContentList, route-condition behavior, or dashboard rendering needs a runtime-specific shape

## Spec Shape

App specs should include decision gates like:

```json
"businessDecisionGates": [
  {
    "key": "quotaCycle",
    "question": "Should quota reset by calendar year or employee anniversary year?",
    "options": ["calendarYear", "employeeAnniversaryYear"],
    "recommendedDefault": "calendarYear",
    "requiredBeforeGeneration": true,
    "status": "unanswered"
  }
]
```

---

<a id="business-solution-design-principles"></a>

<!-- source: skills/yeeflow-application-builder/references/collection-01.md#business-solution-design-principles -->

# Business Solution Design Principles

Use these principles before turning requirements into Yeeflow resources.

## Start With The Business Operation

Identify:

- business goal and expected outcome
- actors, roles, reviewers, approvers, and administrators
- trigger event that starts the process
- lifecycle/status model from draft to completion
- approval/review points
- exceptions, rework, rejection, escalation, cancellation, and closure paths
- master/reference data used repeatedly
- line items, sublists, child records, and transactional details
- reporting, dashboards, and operational visibility needs
- automation opportunities that reduce manual work

Design around business operations, not only form fields.

## Decide What Belongs Where

Use data lists for persistent business records, master data, reference tables, and reporting sources.

Use approval forms for request submission, review decisions, task-specific comments, controlled workflow steps, and workflow-visible data.

Use sublists/list controls for line items that belong to a parent request. Use summary binding when totals or counts drive workflow, validation, or persistence.

Use dashboards only when the requirement needs operational overview, queues, status counts, or management insight. Avoid dashboards in v1 when they are not needed for the core process.

Use temp variables for frontend/runtime UI state only. Do not rely on temp variables for backend persistence unless values are copied into workflow/form variables before submit.

Use ContentList to persist readable business values. Avoid persisting raw lookup IDs when users expect display text.

## Scope V1 Carefully

Default v1 should prove:

- core record structure
- main request/approval flow
- required fields and essential validations
- essential calculations
- ContentList persistence
- runtime import/open/submit/approve path

Defer:

- complex dashboards
- reports
- document generation
- integrations
- external APIs
- complex automation
- advanced runtime-sensitive controls
- optional features that do not affect the first working process

Prefer a smaller, high-quality, runtime-proven v1 over a broad package that is hard to validate.

## Consultant-Style Questions To Answer In The Plan

- What business problem does this app solve?
- Who submits, reviews, approves, processes, and audits the work?
- What data must be captured at request time?
- What data is added during review, fulfillment, or closure?
- What must be persisted for reporting and audit?
- What should stay as temporary UI state?
- Which statuses are meaningful to the business?
- Which decisions change the workflow route?
- Which calculations should be automatic and readonly?
- Which reference data should be a packaged list, lookup, or deferred integration?
- What does success look like in the runtime test?

## Key Design Decisions To Make Explicit

Requester/applicant identity:

- Is the requester always the current login user, or can a user submit for another employee?
- If proxy submission is allowed, the applicant picker can remain editable, but changing it must rerun applicant profile snapshot and dependent quota/policy logic.
- Current User may default the applicant on a new request. Applicant business logic after that should use the applicant variable or snapshot values.

Profile snapshots:

- Identify profile fields that must be snapshotted, displayed readonly, and persisted.
- Missing profile data should route to HR verification or another business fallback, not silently use the current task viewer.

Quota and eligibility:

- Decide whether quota cycles are calendar year, employee anniversary year, or custom.
- Decide when quota is occupied and when it is released.
- Use numeric cycle fields when usage records must be matched by employee-anniversary or tenure cycle.
- Query usage by applicant identity, cycle, and active/occupied status.

Multiple product/item selection:

- If the business allows multiple products/items, use a sublist/listref.
- Use lookup/autofill for product data, row subtotal calculation, summary-bound total, and readable summary persistence.

Form actions:

- Use page load, field change, and submit actions for dependent logic.
- Combine independent Set variable assignments with multi-value Set variables.
- Keep dependent calculations in ordered steps.

## Clarification Questions Before Generation

During planning, separate questions into two categories:

Business-critical clarification questions:

- change workflow routes
- change approval responsibility
- change quota/policy calculations
- change status lifecycle
- change data ownership
- change pricing or amount calculations
- change required attachments/documents
- change persistence timing
- change compliance/audit handling
- change dashboard inclusion in v1 scope
- change integration responsibility
- change role permissions
- change what happens on approval, rejection, or resubmission

These must be asked directly in the Codex chat and must pause generation until answered or until the user explicitly approves default assumptions.

Technical assumptions:

- token shape availability
- tenant data completeness
- exact widget binding shape
- form action runtime behavior
- validator/runtime compatibility

These can be documented, tested, and handled with fallback during generation/runtime validation unless they affect business correctness.

---

<a id="requirement-to-yap-generation-lifecycle"></a>

<!-- source: skills/yeeflow-application-builder/references/collection-01.md#requirement-to-yap-generation-lifecycle -->

# Yeeflow Requirement-to-Application Package Lifecycle

Use this lifecycle whenever the user provides Yeeflow application requirements, screenshots, sample forms, SOPs, process documents, sample exports, or app ideas and asks Codex to build, implement, create, generate, test, or output an application package, `.yap`, or `.yapk`.

The builder is the top-level application-building controller. Coordinate the application, data-list, approval-form, form-report, schedule-workflow, dashboard, AI Agent, Copilot, custom-code, package-validator, and expression generator skills as needed. Use proven plugin knowledge first; send genuinely unknown platform behavior to `yeeflow-feature-learning-orchestrator`.

## Trigger Behavior

Run this lifecycle for requests such as:

- "Build this Yeeflow app from the uploaded requirement."
- "Implement this application."
- "Generate the .yapk for this process."
- "Generate an upgrade .yapk for this existing application."
- "Create the Yeeflow application based on this document."
- "Use the current skills to build this app."
- "Here is the requirement. Please generate the final app package."
- "Create the app and test it in Yeeflow."

Do not generate an app when the user explicitly asks only for study, review, planning, or learning.

## Package Target Gate

Before generation, confirm package target and scope:

- New application defaults to `.yapk`.
- `.yap` is used only when explicitly requested or when a documented fallback/debug scope requires YAP.
- Existing application upgrade requires an official baseline `.yapk` from Yeeflow Version management, identity preservation, and ID stability.
- Existing application upgrades must not use fresh-ID clone generation rules unless the user explicitly asks for a cloned app.
- Generated application wrappers must use FontAwesome icon mode only. The top-level `IconUrl` must be a JSON string containing `b`, `i`, and `c`; image URLs, `https://img.yeeflow.com/...`, SVG, emoji, and blank/null icons are forbidden. The App Plan or generation report should include the selected domain-matched icon rationale without generated package IDs.

Generated-final YAPK packages must be export-shaped, not structurally approximate placeholders. Approval forms, reports, dashboards, navigation, native Title metadata, and runtime URL reporting are validated after resource generation and before signing readiness. Dashboard pages use Dashboard Page Layouts v1.1 as the page shell and Event Portfolio Golden Reference as the component/region reference; generation must copy and normalize the v1.1 shell before mapping business-specific sections, KPI cards, filters, grid-table Collections, and actions into approved v1.1 slots. Business mapping is constrained to registered allowed business-content containers, and added dashboard layout modules must be copies of registered repeatable/removable modules with template structure preserved. Event Portfolio must not be copied as a competing page-root shell, and Collections/filters/actions must not be placed directly under root `Content`. Dashboard filters must be copied from an approved v1.1/Event Portfolio filter module and then mapped to app-specific `attrs.data.list`, `attrs.data.field`, `display_f`, `value_f`, label, placeholder, typography, tuple `attrs.lablay = [null, "top"]`, placeholder color, supported radius, fixed-width positioning, and Collection/KPI consumer linkage; ad hoc filter bars and invented filter modules are generated-final blockers. Dashboard Golden Reference component regions must preserve child-control property fidelity: page title `h2-bold`, page subtitle `body-medium`, KPI labels `s-medium`, KPI values `h2-bold`, KPI trend/note and section subtitle `caption-regular`, and grid-table column headers `caption-medium`. Preserve the canonical v1.1 `Content` padding instead of reusing obsolete zero-padding normalization. These are generator/package hard gates and must not be exposed as Functional Specification or App Plan requirements for business users.

## 1. Requirement Intake

Read all uploaded requirement files, screenshots, sample forms, sample exports, Markdown files, Word documents, PDFs, JSON files, and user notes.

Extract:

- business purpose
- business problem, target users, operational scope, and expected outcome
- source materials
- user roles
- role responsibilities, record visibility, actions, owned decisions, and dashboard/page needs
- app modules
- business objects
- data concepts
- business object field meanings, business-level field type expectations, lifecycle/status fields, audit fields, reporting/dashboard fields, and lookup/reference relationships
- data lists and document libraries
- approval/review needs
- business process start triggers, intake, review/approval, assignment/fulfillment, status tracking, completion/closure, exceptions, and audit/history needs
- business rules for status lifecycle, approvals, assignments, SLA/overdue behavior, validation, documents, notifications, escalations, completion, cancellation/rejection/rework, and permissions
- form report needs
- data list custom forms
- dashboards/pages
- selected application icon rationale at business/control-category level; do not include generated package IDs
- dashboard business questions, metrics, source fields, calculation logic, filters, data regions, display fields, sorting/grouping, user actions, mobile support, and alerts
- data list views
- workflows and automations
- notifications
- AI Agent or Copilot needs
- documents/files
- lookups/reference data
- sublists/line items
- calculations/expressions
- form actions
- query data needs
- integrations, if any
- risky, unsupported, or deferred features

Classify the input detail level as brief, moderate, detailed, document-backed, screenshot-backed, export-backed, or mixed. Preserve source files as references; do not copy them directly into the final requirements.

## 2. Functional Specification

Create a Functional Specification using:

`docs/standards/collection-02.md#functional-specification-standard-template`

The Functional Specification is the business requirement document. It must describe what the business needs before selecting exact Yeeflow resources or package shapes. It must be rich enough for reliable App Plan and Page Function Plan generation: business context, user roles, process steps, decision points, exception cases, status lifecycle, explicit business rules, data lifecycle, approval/form needs, workflows, notifications, dashboard content requirements, operational reporting, audit evidence, assumptions, and business clarification gates.

Keep low-level Yeeflow implementation details out of the Functional Specification. Do not include Yeeflow control types, ListID/PageID/FormID/LayoutID/ProcKey values, actionTypeCode values, JSON property paths, exact generated IDs, package JSON, or resource implementation shapes. Those belong in the App Plan, Page Function Plan, Blueprint, resource generation, and validation stages.

The primary artifact must be a human-readable Markdown file named `functional-specification.md`. JSON is allowed only as a companion/projection artifact for validators, traceability, or tests, such as `functional-specification.trace.json`; JSON must not replace `functional-specification.md`. Any companion JSON projection must be derived from the Markdown Functional Specification and remain consistent with it.

The generator must emit every numbered section from `docs/standards/collection-02.md#functional-specification-standard-template`, in order and with the canonical names. Do not merge, rename, skip, or compress required sections; when something is not applicable, keep the section and write a concrete not-applicable rationale, assumption, or deferred decision.

For brief requirements:

- infer and supplement a complete business requirement document with explicit assumptions
- keep business-critical gaps as clarification questions
- avoid blocking on minor gaps that can be safely assumed

For detailed requirements:

- analyze and reorganize the material into the standard Functional Specification template
- preserve important details without copying whole source sections verbatim

For uploaded documents, screenshots, reference materials, or existing samples:

- treat them as supporting references
- produce a new standardized Functional Specification based on the understood requirements
- list original materials in Source Input Summary

Business-critical uncertainty must be recorded in Business Decision Gates and Risks, Constraints, and Unknowns.

## 3. Functional Specification Review Gate

Review the Functional Specification before creating the App Plan. The review must confirm:

- required 24 sections are present
- requirement interpretation method is explicit
- business context covers the business problem, target users, operational scope, and expected outcome
- user roles identify responsibilities, record visibility, actions, owned decisions, and dashboards/pages needed
- business objects and data requirements are present or explicitly not applicable, including required fields, field meanings, business-level field type expectations, relationships, lifecycle/status fields, audit fields, and reporting/dashboard fields
- relationships and dependency rules are present or explicitly not applicable
- core process steps cover start trigger, submission/intake, review/approval, assignment/fulfillment, status tracking, completion/closure, exception handling, and audit/history needs
- business rules cover status lifecycle, approvals, assignments, SLA/overdue behavior, validation, required documents/rich data, notifications, escalations, completion, cancellation/rejection/rework, and permissions when applicable
- approval/review, forms, workflow/notification, dashboard, reporting/audit, document/attachment, AI, integration, permissions, UI, assumptions, and risks are covered
- dashboard page requirements include business questions, source business objects/data lists, summary metrics, metric source fields and calculation logic, data regions, display fields, filters with source fields/default scope/applies-to regions, sorting/grouping, user actions, mobile support, and alerts
- business clarification gates are answered, `user-default-approved-for-generation`, not applicable, deferred with reason/fallback/proof impact, or listed as blockers
- Readiness for App Plan is marked Yes only when the specification is consistent and complete enough
- no vague standalone language such as "show dashboard", "track status", "manage requests", or "send notifications" appears without business rules, fields, roles, or conditions
- no low-level Yeeflow IDs, control types, actionTypeCode values, JSON property paths, or generated resource IDs leak into the Functional Specification

If inconsistent, incomplete, or incorrect content is found, revise and validate again. A failed Functional Specification review gate blocks the Yeeflow App Plan stage.

## 4. Yeeflow App Plan

Create the Yeeflow App Plan only from the reviewed Functional Specification, using:

`docs/standards/collection-01.md#app-plan-standard-template`

The reviewed Functional Specification is the business source of truth for the App Plan. The App Plan is not a free-form plan, generic project plan, or ad hoc script plan. It is the Yeeflow resource generation contract and must convert business requirements into Yeeflow-supported resources in this standard order:

The primary artifact must be a human-readable Markdown file named `yeeflow-app-plan.md`. JSON is allowed only as a companion/projection artifact for validators, traceability, or tests, such as `app-plan.trace.json`; JSON must not replace `yeeflow-app-plan.md`. Any companion JSON projection must be derived from the Markdown App Plan and remain consistent with it.

The canonical primary App Plan title/schema is `# <Application Name> - Yeeflow App Plan` using the resource-order sections from `docs/standards/collection-01.md#app-plan-standard-template`. Do not generate or require a conflicting primary `Yeeflow Application Plan` schema. If a compatibility validator or companion artifact is used, it must validate or derive from the same `yeeflow-app-plan.md` Markdown contract.

Generation must be based on the reviewed Markdown Functional Specification and Markdown Yeeflow App Plan first. Companion JSON may support validators, traceability, or tests, but it must not replace, override, or silently narrow the Markdown planning contracts.

1. Data lists and Document libraries
2. Approval forms
3. Form reports
4. Schedule workflows
5. AI Agents
6. Copilots
7. Custom Data List forms
8. Data List workflows
9. Notifications
10. Data List views
11. Dashboard pages
12. Application navigation
13. Target users, roles, groups, and permissions

Form Report is an independent Yeeflow resource based on a specific Approval Form. It is not a Dashboard page, not a Data List view, and must not be mixed into Dashboard planning. Normally, one Approval Form should have one corresponding Form Report unless the plan states why not.

All fields, controls, Dynamic controls, variables, workflow nodes, form actions, Collection/Kanban actions, Sub List actions, schedule workflow properties, page controls, property paths, bindings, and configuration shapes must come from active plugin-known skills, standards, validators, template library entries, control/property knowledge base entries, extension registry entries, or export-proven references. Unknown capabilities must be marked `export-learning-required`, `runtime-proof-required`, or `deferred`. Temporary scripts and ad hoc generation logic must not bypass plugin knowledge.

## 5. App Plan Review Gate

Review the App Plan before any full-page design images, page implementation blueprints, resource generation, decoded resource-vs-blueprint parity, package/sign/upgrade, or runtime proof.

The review must confirm:

- required 23 sections are present
- Resource Generation Order uses the standard Yeeflow order
- Data Lists and Document Libraries include field names, field types, dependencies, validation, relationships, sample rows, and Placeholder planning
- Approval Forms include submission fields with Placeholder, task form fields with Placeholder, workflow nodes, assignments, form actions, temp variables, Query data, and Set data list targets
- Form Reports are standalone and based on Approval Forms
- Schedule workflows, AI Agents, Copilots, Custom Data List forms, Data List workflows, Notifications, Views, Dashboards, Navigation, and Permissions are planned or explicitly not applicable
- Dashboard planning is separate from Form Report planning
- Dashboard planning must trace to the Functional Specification's dashboard business questions, metric source fields, calculation logic, data regions, display fields, filters, sorting/grouping, user actions, mobile support, and alerts
- Dashboard planning must convert Functional Specification dashboard business requirements into Yeeflow-supported resource/control-type planning at App Plan level
- Each Dashboard page plan must include page identity, source Functional Specification dashboard requirement reference, source data lists/business objects, navigation placement, and Page Function Plan reference if applicable
- Each dashboard section must state business purpose, source data list or business object, required fields or metrics, selected Yeeflow control type category, why the control type is appropriate, user actions, and proof boundary or deferred note
- Dashboard controls come from plugin-known controls, template library, validators, or export-proven references, including Summary/KPI card, Data Filter, Collection, Data table, Kanban, Vertical Timeline, Horizontal Timeline, Text/Heading, Button/action button, Container, Grid/flex grid, and Chart/Data analytics only when supported or marked proof-required/deferred
- Record-display sections must choose Collection, Data table, Kanban, Vertical Timeline, or Horizontal Timeline with a reason; prefer Collection for portfolio/card/grid-table regions, Data table for dense tabular records, Kanban for lane/status workflows, Vertical Timeline for audit/activity/history, and Horizontal Timeline for lifecycle/phase/milestone views
- Dashboard filters, summary metrics, actions, and item-template dynamic display needs must be planned at business/control-type level
- Dashboard App Plan entries must not include concrete generated IDs, ListID/PageID/FormID/LayoutID/ProcKey values, actionTypeCode values, Yeeflow JSON property paths, exact Container nesting, exact style values, runtime binding payloads, implementation-level layout JSON, or fake placeholder IDs such as LIST-* or LAYOUT-*
- Dashboard/Page sections that display Data List records state selected display control and reason: Data table, Collection, Kanban, Vertical Timeline, or Horizontal Timeline
- Collection, Kanban, Vertical Timeline, and Horizontal Timeline controls include item-template Dynamic control planning with bound fields
- Collection and Kanban controls include item actions or explicitly state `No Collection/Kanban item actions required`
- Approval Forms and Custom Data List forms with Sub List controls include Sub List list actions or explicitly state `No custom Sub List actions required`
- field/control/Dynamic control/workflow/action/schedule/resource/property types are plugin-supported or marked for learning/proof/deferment
- Generation Contract and Hard Gates, Validation Plan, Proof Boundary, Assumptions, Deferred or Runtime-Proof Items, and Recommended Next Prompt are present

If the App Plan review fails, revise and validate again. A failed App Plan review gate blocks business clarification closure, generation-readiness review, design image generation, page blueprinting, resource/package generation, signing, install/import/upgrade, and runtime proof.

After App Plan/Page Function Plan approval and before dashboard blueprint/resource generation, create a Dashboard Golden Reference Selection artifact for each dashboard page. The default selection for standard operational dashboards is `event_portfolio_dashboard_golden_reference`; map the page's business questions, source lists, KPI metrics, filter fields, grid/table fields, and actions into the export-shaped `_ak_c` / `_ak_c_opt` reference shell and sub-regions. Blueprint/resource generation must clone and normalize the selected export tree before domain mapping. Use an alternative reference only with an explicit reason and expected validation impact.

## 6. Business Clarification Gate

After the Functional Specification and App Plan are drafted, identify business-critical decisions that are still unanswered.

Business-critical questions include anything that changes:

- workflow route
- approval responsibility
- quota/policy calculation
- status lifecycle
- data ownership
- pricing or amount calculation
- required attachments/documents
- persistence timing
- compliance/audit handling
- dashboard inclusion if it affects app scope
- integration responsibility
- role permissions
- what happens on approval/rejection/resubmission

When unanswered business decisions exist, output a clear question block in the Codex chat:

```text
Business clarification required before generation:

Artifacts:
- Functional Specification: <path>
- Yeeflow App Plan: <path>
- Validation report: <path or not generated>

Validation summary:
- Functional Specification structure: pass/fail
- App Plan resource order: pass/fail
- Functional Spec to App Plan traceability: pass/fail
- Generation Readiness structural check: pass/fail
- Business Clarification Gate for planning: pass/fail
- Business Clarification Gate for generation: blocked until user approves defaults or answers gates
- Overall generation readiness: blocked by Business Clarification Gate
- No package generation was performed

Unresolved business decision gates:

1. <gateKey>: <Question>
   - Option A:
   - Option B:
   - Recommended default:
   - Why this matters:

Approve all recommended defaults for: <gate1>, <gate2>, ...

No package generation will proceed until the business gates are answered or explicitly user-default-approved-for-generation.
```

The final planning response must enumerate every unresolved business decision gate by key and question. If it offers "approve all recommended defaults", it must explicitly name each gate covered by that approval option.

Stop after outputting the clarification block. Do not continue to Yeeflow resource/package generation in the same turn.

Generation may continue only after the user answers the questions or explicitly approves default assumptions.

When Functional Specification and/or App Plan Markdown artifacts exist, run:

```bash
node scripts/validate-business-clarification-gate.mjs --spec <functional-spec.md> --plan <app-plan.md>
```

The gate fails on unanswered, pending, TBD, open, requires-clarification, or generation-paused decision gates, and on decision tables that omit status/answer/default approval evidence. This validator proves clarification-gate document readiness only; it does not prove the business answer is correct.

## 7. Generation Readiness Review

Before generation, confirm:

- Functional Specification review gate passed
- App Plan review gate passed
- all required business decision gates are answered or defaults are explicitly approved
- mandatory business capabilities are not misclassified as future enhancements
- package target is `.yapk` by default for new applications, `.yap` only when explicitly requested/fallback scoped, or versioned `.yapk` for a safe existing-app upgrade
- technical assumptions have validation/fallback plans
- approval form design-quality gates are represented
- Form Report requirements are represented as standalone resources
- generated resource inventory matches required scope
- runtime-unproven features are marked as focused proof items or documented limitations
- plugin capability and standards compliance has no invented unsupported shapes

Stop if any required business decision gate remains unanswered or either review gate is failed.

When an App Plan Markdown artifact exists, run:

```bash
node scripts/validate-app-plan-template.mjs <app-plan.md>
node scripts/validate-app-plan-resource-order.mjs <app-plan.md>
node scripts/validate-generation-readiness-review.mjs --plan <app-plan.md>
```

`validate-app-plan-template.mjs` is a compatibility entrypoint for the same canonical resource-order `yeeflow-app-plan.md` contract enforced by `validate-app-plan-resource-order.mjs`; both must accept the same primary Markdown artifact. The generation-readiness review fails when any of the 13 Yeeflow resource areas is missing, empty, placeholder-only, or lacks concrete planned resources or explicit not-applicable/deferred/runtime-proof status. It also checks that Functional Specification review, App Plan review, business decision closure, and unsupported-shape gates are documented as passed. These validators prove planning readiness only; they do not prove package generation, schema validity, signing/API acceptance, install/upgrade success, or runtime behavior.

Validation reports must distinguish structural planning checks from overall generation readiness:

- Functional Specification structure: pass/fail
- App Plan resource order: pass/fail
- Functional Spec to App Plan traceability: pass/fail
- Generation Readiness structural check: pass/fail
- Business Clarification Gate for planning: pass/fail
- Business Clarification Gate for generation: pass/fail
- Overall generation readiness: pass only if all required planning gates pass for generation

If Generation Readiness structural check passes but generation-mode Business Clarification Gate fails, write `Overall generation readiness: blocked by Business Clarification Gate`. Do not write only `Validation passed` or `all passed` when business clarification is intentionally unresolved or only `default-applied-for-planning`.

When the same unresolved business decision gate appears in both the Functional Specification and App Plan, use the Business Clarification validator's deduplicated summary for user-facing reporting. Show raw finding count, unique unresolved gate count, unique gate keys, and occurrence locations when helpful. Do not make users infer five decisions from ten duplicated raw findings.

Before design images, page implementation blueprints, resource/package generation, decoded resource-vs-blueprint parity, signing, install/import/upgrade, or runtime proof, also run:

```bash
node scripts/validate-functional-spec-to-app-plan-traceability.mjs --spec <functional-spec.md> --plan <app-plan.md>
```

Traceability fails when business objects, relationships, approvals, forms, workflows, reporting, documents, AI/Copilot, integrations, permissions, or UI/experience requirements in the Functional Specification are not mapped to Yeeflow resources, planning sections, or explicit deferred/not-applicable coverage in the App Plan. Deferred items must include a reason, fallback, proof impact, or follow-up. This validator proves planning traceability only; it does not prove generated package conformance or runtime behavior.

When planning artifacts are saved in an output directory, also run:

```bash
node scripts/validate-planning-artifact-formats.mjs --dir <artifact-dir>
```

This gate fails if `functional-specification.md` or `yeeflow-app-plan.md` is missing, if either primary Markdown document is empty, skeletal, or only links to JSON, if Functional Specification or App Plan is generated only as JSON, or if companion JSON projections such as `functional-specification.trace.json`, `app-plan.trace.json`, or `page-function-plan.trace.json` do not reference the Markdown source they project from. It proves artifact format discipline only; it does not prove business correctness, package validity, or runtime behavior.

After this gate passes, downstream Page Function Plan, Blueprint, resource generation, and package validation must use the Markdown planning contracts as the business and resource source of truth. JSON projections can be read as validator-friendly indexes, not as replacement specifications.

## 8. Decide Safe Build Scope

Choose the safest build scope that satisfies the reviewed Functional Specification and App Plan.

Default scope is the complete functional application described by the approved App Plan, not a simple MVP, unless the user requests staged generation or approves deferral. Staged generation is allowed when:

- the app is too large for one safe package
- critical information is missing and the user approves assumptions
- a focused runtime proof package is explicitly requested
- an advanced capability must be learned or runtime-proven before full generation

Do not defer core business capabilities silently.

## 9. Resource/Package Generation

Generate resources in the App Plan order:

1. Data lists and Document libraries
2. Approval forms
3. Form reports
4. Schedule workflows
5. AI Agents
6. Copilots
7. Custom Data List forms
8. Data List workflows
9. Notifications
10. Data List views
11. Dashboard pages
12. Application navigation
13. Target users, roles, groups, and permissions

Use only current proven generation rules:

- field types from plugin Data List / Document Library standards
- Approval Form field and variable types from plugin Approval Form standards
- workflow nodes and form actions from plugin workflow/action knowledge
- schedule workflow configs from plugin Schedule Workflow knowledge
- dashboard/page controls from plugin-known controls, template library, validators, or export-proven references
- runtime navigation metadata from current navigation hard gates
- API-issued IDs for generated-final `.yapk` output
- ID stability for upgrades from official baseline `.yapk`

Do not generate with temporary custom scripts that invent resource/control/action shapes outside the plugin standards. If a generator helper is needed, it must consume the reviewed Functional Specification, reviewed App Plan, plugin templates, validators, and export-proven references.

## 10. Local Validation

Run the relevant safe local checks for the generated artifact and proof boundaries. At minimum, validate schema, graph/resource structure, Functional Specification/App Plan conformance, generated-final resource completeness against the approved App Plan, ID provenance for generated-final `.yapk`, navigation runtime metadata, approval forms, Form Reports, schedule workflows, AI/Copilot resources, custom data list forms, data-list workflows, notifications, views, dashboard controls, dashboard generation hard gates, grid-table Collection patterns, root padding, plan-to-package conformance, and source/dist consistency as applicable.

When a package is generated from an approved App Plan, run `scripts/validate-generated-final-resource-completeness.mjs --plan <yeeflow-app-plan.md> --package <app.yapk|decoded.json>` before signing readiness. The generated-final package must materialize every non-deferred planned data list, approval form, custom data list/document library form, dashboard page, FormNewReport, DataReport, workflow, notification, navigation group/item, and permission/role resource where planned. A shell package with `Forms: []`, empty `FormNewReports[]`/`DataReports[]` for planned reports, Type 103 dashboards whose `Content.children` is empty, or generic-only navigation is blocked unless the App Plan explicitly defers the item with reason, fallback/user impact, and follow-up proof.

Dashboard generation hard gates are generator/package/reporting rules only. Do not add them to the Functional Specification or App Plan, and do not require business users to specify control-property details. Before signing readiness, signing, install/import, upgrade, or final success reporting, run `scripts/validate-dashboard-generation-hard-gates.mjs` for generated dashboard packages, include `--plan <yeeflow-app-plan.md>` when an approved plan exists, and include the final/install/upgrade report when available. Canonical runtime URLs must use decoded package `$.ListSet.ListID`; install/import API returned IDs are operation evidence unless separately proven by product/API docs.

Dashboard Golden Reference conformance is also a generator/package hard gate. Run `scripts/validate-dashboard-golden-reference-conformance.mjs` on the registry/reference quality, selection, blueprint, generated package, and runtime filter proof whenever dashboard pages are generated and filter linkage is claimed. The gate verifies the selected golden reference, export-shape parity, sub-region provenance, non-empty business regions, app-specific field mappings, static filter contracts, module-level typography and filter-property fidelity, runtime filter proof separation, `dynamic-user` user-field rendering, and rejection of copied Marketing Event fields in unrelated apps.

Do not sign, install, import, upgrade, or run live Yeeflow writes unless explicitly authorized.

## 11. Runtime Import/Testing Only When Requested Or Authorized

Runtime proof is a separate layer from local validation, package schema validation, signing, API acceptance, and browser/design proof. Run runtime import/testing only when the user requests or authorizes it.

## 12. Runtime Issue Fixing

If authorized runtime testing finds issues, fix the smallest resource/package surface that addresses the issue, rerun local validation, and keep runtime proof boundaries explicit.

## 13. Documentation

Document generated artifacts, validation results, proof boundaries, deferred items, and known follow-up items.

## 14. Skill Updates Only If Reusable Knowledge Is Learned

Update skills, standards, validators, or references only when the run produces reusable Yeeflow knowledge. Keep tenant-specific details, raw API responses, raw package `Resource`, raw `Sign`, tenant URLs, full workspace IDs, raw user IDs, and private screenshots out of docs.

## 15. Git Commit/Push And Final Package Output

Commit and push only the intended safe source changes when requested. Final package output must separately report:

- Functional Specification status
- App Plan status
- local validation status
- signing status
- API acceptance status
- runtime/browser proof status
- deferred or runtime-proof-required items
- safety boundaries
