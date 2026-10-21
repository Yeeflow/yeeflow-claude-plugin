# skills/yeeflow-runtime-test-orchestrator/references collection 1

This file combines 3 documents from `skills/yeeflow-runtime-test-orchestrator/references/` for the Claude directory build. A reference to `skills/yeeflow-runtime-test-orchestrator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [runtime-result-classification.md](#runtime-result-classification)
- [runtime-test-checklist.md](#runtime-test-checklist)
- [runtime-test-lifecycle.md](#runtime-test-lifecycle)

---

<a id="runtime-result-classification"></a>

<!-- source: skills/yeeflow-runtime-test-orchestrator/references/collection-01.md#runtime-result-classification -->

# Runtime Result Classification

Use one of these labels for each tested area. Classify narrowly: one app can have a runtime-proven list, a render-only dashboard, and a blocked workflow at the same time.

## runtime-proven

Use when the feature was tested in Yeeflow with real runtime actions and evidence. For example, a form was opened, submitted, reviewed, and the resulting list/workflow state was verified.

## partially proven

Use when some required runtime behavior passed but important paths remain untested or failed. Name the proven paths and missing paths.

## render-only proven

Use when the page, form, dashboard, or custom control renders but data persistence, action behavior, workflow side effects, or data binding were not tested.

## blocked by configuration

Use when testing is blocked by missing tenant settings, permissions, users, roles, connectors, feature flags, or environment setup that is outside the package itself.

## blocked by package/materialization

Use when import, app creation, list/form/workflow materialization, corrupted IDs, missing resources, graph validation, or package structure prevents runtime testing.

## blocked by Yeeflow runtime context

Use when Yeeflow runtime behavior or access prevents proof even though local package validation did not find a structural blocker. Examples include unavailable runtime modules, inaccessible public form context, or runtime-only SDK differences.

## not tested

Use when no runtime action was performed for the feature. Do not infer proof from similar controls or local validation.

## Reporting Format

For each row include:

- area: list, form, dashboard, workflow branch, expression, custom code, or lifecycle
- result: one label above
- evidence: test action and observed result
- blocker or gap
- next action

---

<a id="runtime-test-checklist"></a>

<!-- source: skills/yeeflow-runtime-test-orchestrator/references/collection-01.md#runtime-test-checklist -->

# Runtime Test Checklist

Use this checklist as a minimum runtime pass. Add app-specific checks from requirements, screenshots, SOPs, and known risk areas.

## Import And Navigation

- Import/open app package.
- Confirm app name, menus, pages, and permissions.
- Confirm dashboards, data lists, approval forms, and workflows appear.

## Data Lists

- Confirm every expected list materializes.
- Confirm field labels, types, required flags, choices, lookups, defaults, and calculated values.
- Create and edit sample records.
- Filter and search representative fields.
- Confirm child-list or `ContentList` values persist after save/reopen.

## Approval Forms

- Open requester form.
- Validate required fields and conditional sections.
- Submit happy-path request.
- Open reviewer task page.
- Approve and reject representative requests.
- Confirm status, comments, audit/history, and task URL behavior.

## Workflow Branches

- Approve path.
- Reject path.
- Revise/resubmit path when present.
- Conditional routing path.
- Notification and list-update side effects.
- Error or incomplete-input behavior.

## Dashboards

- Confirm dashboard renders.
- Confirm every KPI, chart, table, and filter is data-bound.
- Add or change sample records and verify values update.
- Confirm no static mockup KPI is accepted as proof.

## Expressions And Query Data

- Test calculated fields.
- Test filter expressions with matching and non-matching records.
- Test lookup and query data controls.
- Record expression errors or empty-result behavior.

## Custom Code Controls

- Confirm runtime context: dashboard, application page, approval form, or public form.
- Test SDK calls and returned data.
- Test user interaction, save/submit behavior, loading state, and error state.
- Claim support only for contexts actually tested.

## Lifecycle Scenarios

- Family Quota Usage or entitlement calculation.
- Audit lifecycle creation and review.
- Status transitions across request, approval, rejection, cancellation, and renewal.
- Reopen completed record and confirm persisted state.

---

<a id="runtime-test-lifecycle"></a>

<!-- source: skills/yeeflow-runtime-test-orchestrator/references/collection-01.md#runtime-test-lifecycle -->

# Runtime Test Lifecycle

Use this lifecycle after local validation says a Yeeflow package is safe to import. Runtime testing proves behavior in Yeeflow; it is not a substitute for package validation.

## 1. Prepare

- Record package path, version, branch, commit, and validator results.
- Define the test tenant/environment and note any missing permissions, connectors, or sample data constraints.
- Prepare minimal sample records that exercise required states without exposing customer or tenant-specific runtime data.
- Decide which behavior must be proven before baseline acceptance.

## 2. Import And Open Smoke

- Import or open the generated app package in Yeeflow.
- Confirm the app shell, navigation, pages, lists, forms, and workflow entries are visible.
- If import fails, classify as `blocked by package/materialization` unless the failure is clearly environment configuration.
- If open succeeds but functional tests are not run, do not classify beyond render-level proof.

## 3. Data Lists And Materialization

- Confirm each expected list exists after import.
- Confirm fields materialize with correct labels, types, lookups, choices, required flags, and parent relationships.
- Create, edit, view, filter, and delete or archive sample records when the scenario permits.
- Check `ContentList` and child-table persistence by saving, reopening, and reviewing submitted values.

## 4. Forms And Actions

- Open each approval form and data-entry form.
- Test required fields, conditional visibility, calculated/default values, form actions, query data controls, filter expressions, and task URLs.
- Submit at least one happy-path request and one representative validation/error path.
- Review task/review pages as the assigned reviewer.

## 5. Workflows

- Exercise every required workflow branch: approve, reject, revise, cancel, conditional routing, escalation, notification, and update actions where applicable.
- Verify workflow variables, set-variable actions, list-item updates, task assignment, and status transitions.
- Record branches not executed as `not tested` or `partially proven`, not runtime-proven.

## 6. Dashboards

- Confirm dashboards render without broken controls.
- Confirm KPIs, charts, tables, and filters are data-bound to Yeeflow lists or workflow data.
- Change or add sample data and verify dashboard values respond.
- Reject static KPI mockups or placeholder charts as baseline proof.

## 7. Custom Code

- Test each custom code control in the exact claimed runtime context, such as dashboard, application page, approval form, or public form.
- Verify SDK calls, query results, callback refs, event handlers, save behavior, and error states.
- Do not claim public form support unless public form runtime testing was performed.

## 8. Domain Lifecycle Tests

For quota, audit, family, or entitlement scenarios, include lifecycle-style checks:

- initial request creation
- quota/usage calculation
- approval/rejection side effects
- audit log or history persistence
- renewal, cancellation, or adjustment where required
- reopen/review after workflow completion

## 9. Baseline Decision

Accept a baseline only when the required user journeys are runtime-proven and documented. If any critical journey is blocked or not tested, record a partial baseline or a rejected baseline with the exact blocker.
