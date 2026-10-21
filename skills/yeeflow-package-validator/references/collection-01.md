# skills/yeeflow-package-validator/references collection 1

This file combines 3 documents from `skills/yeeflow-package-validator/references/` for the Claude directory build. A reference to `skills/yeeflow-package-validator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [package-validation-lifecycle.md](#package-validation-lifecycle)
- [validation-error-severity.md](#validation-error-severity)
- [yap-materialization-rules.md](#yap-materialization-rules)

---

<a id="package-validation-lifecycle"></a>

<!-- source: skills/yeeflow-package-validator/references/collection-01.md#package-validation-lifecycle -->

# Package Validation Lifecycle

Run this lifecycle before Yeeflow import or runtime testing. Use available repo scripts and validators rather than inventing ad hoc checks.

## 1. Identify Package Scope

- Confirm whether the artifact is a generated `.yap`, decoded app folder, `.ydl`, `.ywf`, or `.yapk`.
- Treat `.yapk` as read-only/server-generated for app-content changes unless Yeeflow Resource generation behavior is proven. Wrapper signing/verification for an already-valid opaque Resource is not app-content generation proof.
- Do not modify raw generated packages unless the task explicitly asks for regeneration.

## 2. Run Structural Validators

Use the repo's actual script names and paths when available. Common validators include:

- `validate-yap-package`
- `validate-yap-graph`
- `validate-ywf-def`
- `validate-ydl-list`
- wrapper round-trip checks

If a named validator is unavailable, report it as unavailable and use the closest repo-local inspection without pretending the validator ran.

## 3. Validate Lists And Fields

- Confirm every list has stable identity, type, and field collection.
- Confirm app-wide `FieldID` uniqueness.
- Confirm every field's `ListID` equals its parent data list `ListID`.
- Confirm `FieldName`, `InternalName`, and `DisplayName` are unique within each list unless Yeeflow export evidence proves an exception.
- Confirm lookups point to existing lists and fields.
- Do not remap `TenantID`, `CreatedBy`, or `ModifiedBy` into generated ID families.

## 4. Validate Workflows

- Run `validate-ywf-def` or equivalent.
- Inspect task actions, approval/rejection branches, set-variable actions, update-item actions, notifications, conditions, expressions, and task URLs.
- Confirm `FlowKey` values are stable, unique where required, and not accidentally copied across unrelated workflows.

## 5. Validate Forms, Dashboards, And Custom Code

- Confirm form controls reference existing lists, fields, actions, and expressions.
- Confirm dashboards reference real data sources, not static placeholder KPI values.
- Inspect custom code controls for expected runtime context, SDK usage, missing imports, hard-coded tenant data, and unsafe claims.

## 6. Validate Materialization

- Inspect package wrapper and decoded resources for app, list, form, workflow, dashboard, and file/resource materialization.
- Check for prefix or `pr<id>x` corruption where encoded IDs, field names, or resource references were accidentally rewritten.
- Confirm compressed or embedded resources can be decoded or round-tripped when the repo has tooling for it.
- When an approved App Plan exists, run `scripts/validate-generated-final-resource-completeness.mjs --plan <yeeflow-app-plan.md> --package <app.yapk|decoded.json>` before signing readiness. Fail generated-final packages that omit non-deferred planned resources, including approval forms with `Forms: []`, planned Form Reports with empty `FormNewReports[]`, planned Data Reports with empty `DataReports[]`, planned dashboards with empty `Content.children`, and planned navigation groups/items reduced to a generic/default group. Also fail App Plans that reference unplanned generated resources: Data List navigation targets must be planned Data Lists or valid planned Data Views with a planned host, and Data List lookup targets must be planned/generated Data Lists. The materializer must fail with `FULL_APP_MATERIALIZATION_LOOKUP_TARGET_DATA_LIST_NOT_PLANNED` instead of emitting lookup fields with empty `Rules`.

## 7. Decide Import Safety

Do not import when blocking issues remain. If only warnings remain, document them and explain why runtime testing is still safe.

---

<a id="validation-error-severity"></a>

<!-- source: skills/yeeflow-package-validator/references/collection-01.md#validation-error-severity -->

# Validation Error Severity

Classify findings conservatively. When the consequence is unclear, block import until Yeeflow runtime evidence proves it safe.

## Blocking

Block import or runtime testing for:

- invalid package JSON or wrapper structure
- failed package graph validation
- failed workflow definition validation
- missing required app, list, form, dashboard, workflow, or resource definitions
- unresolved lookup targets
- duplicated app-wide `FieldID`
- field `ListID` mismatch with parent data list
- duplicate `FieldName`, `InternalName`, or `DisplayName` within one list when not proven safe
- corrupted prefixes or `pr<id>x` resource/name corruption
- unsafe `.yapk` mutation
- missing or invalid `FlowKey`
- workflow actions that reference missing variables, fields, lists, tasks, or expressions
- dashboard controls that only contain static KPI/chart mockups when a data-bound dashboard is required

## Warning

Warnings can allow runtime testing when documented:

- optional display metadata differences
- non-critical layout differences
- unused but harmless fields
- missing sample data
- conservative fallback controls documented as such
- custom code support limited to proven contexts

## Informational

Informational notes do not block:

- validator unavailable but replaced by a manual inspection
- package size observations
- documentation gaps
- runtime tests still pending

## Report Format

Use this structure:

```text
Validation result: passed / passed with warnings / blocked
Blocking:
- ...
Warnings:
- ...
Runtime import decision:
- safe / unsafe / pending
Evidence:
- command or inspection result
```

---

<a id="yap-materialization-rules"></a>

<!-- source: skills/yeeflow-package-validator/references/collection-01.md#yap-materialization-rules -->

# YAP Materialization Rules

Materialization checks answer whether Yeeflow can create the expected runtime objects from a package.

## Required Objects

For an application-level `.yap`, confirm the package contains the expected:

- app metadata
- data lists
- approval forms
- dashboards or app pages
- workflows
- resources or embedded definitions needed by those objects

## Identity And Relationship Rules

- Keep generated app IDs, list IDs, field IDs, form IDs, workflow IDs, and page IDs internally consistent.
- Ensure fields belong to their parent list through matching `ListID`.
- Ensure lookups target existing lists and target fields.
- Ensure approval forms bind to the correct main list.
- Keep `Data.Forms[].ListID = 0` when the package format requires app-level packaging with wrapper-owned binding.
- Avoid reusing stale ID families from unrelated app generation runs.

## Corruption Checks

Search decoded package content for:

- malformed prefixes
- accidental `pr<id>x` insertion into field names or resource keys
- duplicated internal names
- unresolved lookup targets
- missing workflow variables
- broken task URLs
- stale resource references

## `.yap` Versus `.yapk`

- `.yap` can be generated for new application creation.
- `.yapk` is treated as read-only/server-generated until Yeeflow signing and Resource mechanisms are proven.
- Do not mutate `.yapk` contents for an upgrade claim. Export, inspect, and document findings instead.
- Product signing utilities can sign and verify wrappers only when `Resource` is already valid `.yapk` opaque payload. Wrapper-sign success does not prove app-content mutation, and unchanged `Resource` means unchanged app content.

## Import Decision

Materialization failures are usually blocking. Do not import if required lists, forms, workflows, pages, or resources cannot be created from the package or if identity corruption is detected.
