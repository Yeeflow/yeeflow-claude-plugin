# skills/yeeflow-application-generator/references collection 1

This file combines 8 documents from `skills/yeeflow-application-generator/references/` for the Claude directory build. A reference to `skills/yeeflow-application-generator/references/<name>.md` is the section that starts at the anchor `#<name>` below.

- [baseline-department-access-management-v5.md](#baseline-department-access-management-v5)
- [baseline-visitor-access-management-v11.md](#baseline-visitor-access-management-v11)
- [build-yap-wrapper.md](#build-yap-wrapper)
- [examples-summary.md](#examples-summary)
- [knowledge-base-app-pattern.md](#knowledge-base-app-pattern)
- [validate-yap-graph.md](#validate-yap-graph)
- [validate-yap-package.md](#validate-yap-package)
- [yap-structure-study.md](#yap-structure-study)

---

<a id="baseline-department-access-management-v5"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#baseline-department-access-management-v5 -->

# Generated `.yap` Baseline: Department Access Management v5

This document records the first successful generated Yeeflow application-level `.yap` baseline.

## 1. What v5 Proves

`Department Access Management_v5` proves that Codex can generate a minimal app-level `.yap` package that Yeeflow can import and open.

Confirmed sandbox result:

- The `.yap` imports successfully.
- The import popup shows logo, name, and description.
- The app appears on the workspace home page.
- The app opens from the workspace card.
- The app header and navigation render.
- The `Departments` list opens.
- The `Departments` list shows 3 sample department rows.
- The `Department Access Requests` list opens.
- The `Department Access Requests` list shows sample request rows.
- The `Department Access Request` approval form opens.
- The approval request form renders with Department lookup, Department Code, Needed By Date, and Access Reason fields.

This is the first app-level generated package baseline that combines:

- root app/listset shell
- two related data lists
- lookup relationship between lists
- sample records
- custom list forms
- app navigation
- approval form/workflow
- approval form lookup control
- approval workflow `ContentList` persistence target

## 2. App Shell Rules Learned

The working v5 baseline requires a complete app shell, not only valid child resources.

Confirmed rules:

- Top-level wrapper `IconUrl` must be non-null.
- Top-level wrapper `Title` and `Description` should be populated.
- Root `Data.Item.ListModel.Title` should match the app name.
- Root `Data.Item.ListModel.Description` should be populated when a description is intended.
- Root `Data.Item.ListModel.IconUrl` should match the wrapper icon.
- Root `Data.Item.ListModel.Type = 1024`.
- Root `Data.Item.ListModel.CustomType = ""`.
- Root `Data.Item.ListModel.Perm = 0`.
- Root `Data.Item.ListModel.WorkspaceID` must be present.
- Root `Data.Item.ListModel.LayoutView` must contain populated navigation.
- Root `Data.Item.Layouts[]` should include at least one Type `103` app page layout.
- `Data.AppTags` must exist as an array.
- `Data.AppMetadatas` must exist as an array.
- `Data.AppComponents` must exist as an array.
- `Data.AppThemes` must exist and contain an application style entry.
- Root `CreatedBy` and `ModifiedBy` should be populated.

The generated app's first successful open route was:

```text
/#/list-set/<workspace-id>/<listset-id>/<list-id>
```

This confirms Yeeflow remapped the generated root app/listset ID and Type `103` page layout ID during import.

## 3. Root App Page Rule

Root Type `103` app pages have a different resource-registration pattern from data-list custom forms.

Confirmed rule:

- Type `103` app page `LayoutID` is a local generated ID and must be included in `Resource.ReplaceIds`.
- `LayoutInResources[0].ID` and `LayoutInResources[0].RefId` are separate resource IDs.
- Type `103` `LayoutInResources` resource IDs must not equal `LayoutID`.
- Type `103` `LayoutInResources` resource IDs should be excluded from `Resource.ReplaceIds`.
- `LayoutInResources[0].Resource` must contain valid app page JSON.

This differs from the data-list custom form pattern, where `LayoutInResources[0].ID` and `RefId` match the custom form `LayoutID`.

## 4. Failed v1-v4 Lessons

Earlier generated packages helped isolate the required app-shell metadata.

| Version | Result | Lesson |
| --- | --- | --- |
| v1 | Package validated locally but imported poorly. | Component-level validity is not enough for app-level runtime behavior. |
| v2 | Import popup still lacked proper name/description behavior. | Root and wrapper metadata must both be complete. |
| v3 | Import popup showed logo/name/description, app card appeared, but open behavior was not yet proven. | Non-null wrapper `IconUrl` and root app page layout are required. |
| v4 | Still uncertain in browser testing. | Root Type `103` page resource IDs needed the real export pattern. |
| v5 | App imports, opens, renders navigation, lists, and approval form. | Complete app shell, Type `103` page registration, theme arrays, and audit metadata form the first working baseline. |

## 5. Validator Updates Already Made

`validate-yap-package.js` now checks app-shell readiness in generator/final mode:

- wrapper title/description presence
- non-empty wrapper icon
- root `CustomType = ""`
- root `Perm = 0`
- root `WorkspaceID` present
- parseable root `LayoutView`
- populated root navigation
- root navigation references resolve
- root Type `103` app page exists
- Type `103` app page has `LayoutInResources`
- Type `103` app page resource ID/RefId are separate from `LayoutID`
- Type `103` app page resource IDs are excluded from `ReplaceIds`
- root `CreatedBy` and `ModifiedBy` are populated
- `AppTags`, `AppMetadatas`, and `AppComponents` arrays exist
- `AppThemes` is present and non-empty

`validate-yap-graph.js` now records root navigation edges so app shell navigation can be checked as part of the app graph.

`build-yap-wrapper.js` now supports `--icon-url` and defaults the top-level wrapper icon from the root `ListModel.IconUrl` when possible.

## 6. Import Test Result

Observed v5 sandbox result:

- Import: passed
- Workspace card: created
- App open: passed
- App header: rendered
- App navigation: rendered
- Departments list: opened
- Departments sample data: rendered
- Department Access Requests list: opened
- Department Access Requests sample data: rendered
- Department Access Request approval form: opened and rendered

The working imported IDs observed in Chrome include:

- Imported root app/listset ID: `2053789849616134145`
- Imported Departments list ID: `2053789859448766465`
- Imported Department Access Requests list ID: `2053789859452960776`
- Imported Type `103` overview page ID: `2053789859457155085`

## 7. Internal Lookup Sample Data Export-Back Evidence

The exported-back `Department Access Management_v5.yap` confirms that app-level internal lookup sample references are structurally valid.

Department sample record remapping:

| Department | Generated `ListDataID` | Exported-back `ListDataID` | Department Code |
| --- | --- | --- | --- |
| Information Technology | `2060010000000011001` | `2053789859452960773` | `IT` |
| Human Resources | `2060010000000011002` | `2053789859452960774` | `HR` |
| Finance | `2060010000000011003` | `2053789859452960775` | `FIN` |

Request sample lookup remapping:

| Request | Generated Department raw value | Exported-back Department raw value | Matches exported Department record |
| --- | --- | --- | --- |
| DAR-1001 | `2060010000000011001` | `2053789859452960773` | yes |
| DAR-1002 | `2060010000000011002` | `2053789859452960774` | yes |
| DAR-1003 | `2060010000000011003` | `2053789859452960775` | yes |

This disproves the stale-ID hypothesis for app-level packaged sample data. Yeeflow remapped both the target Department sample record IDs and the dependent request lookup values consistently.

Confirmed app-level rule:

- For an internal lookup where both source and target lists are packaged in the same `.yap`, target sample record IDs are local package IDs.
- Local target sample record IDs should be included in `Resource.ReplaceIds`.
- Dependent sample lookup values may reference those local target sample record IDs as plain strings.
- During app import, Yeeflow remaps the target sample records and the dependent lookup values together.

This differs from standalone `.ydl` packages where the lookup target already exists outside the dependent package. In that external-lookup case, target record IDs must be excluded from the dependent package `ReplaceIds`.

## 8. Remaining Runtime Display Note

The remaining issue is sample lookup display in the `Department Access Requests` list:

- `Departments` sample rows display correctly.
- `Department Access Requests` sample rows display Department Code values.
- The `Department` lookup display column appeared blank for generated sample request rows in the grid.

In the generated v5 source, `Department Access Requests.Text2` stores plain generated Department sample `ListDataID` strings, for example:

```json
{
  "Title": "DAR-1001",
  "Text2": "2060010000000011001",
  "Text3": "IT"
}
```

The target Department sample records exist in the same package and those sample record IDs are included in `Resource.ReplaceIds`. Export-back proves the raw `Text2` lookup values point to valid exported-back Department `ListDataID` values.

Therefore, if the grid display is still blank after refresh/reopen, classify it as a runtime lookup display/index/cache issue unless the item form lookup value is also broken.

Recommended manual checks before creating v6:

1. Refresh or reopen the app.
2. Open a sample request row form.
3. Verify whether the Department lookup selection is present inside the form.
4. Create or edit one row and select a Department.
5. Confirm whether manually selected lookup values display in the grid.

Do not create a new package only to address blank grid display without stronger evidence that the package data or lookup field is wrong.

---

<a id="baseline-visitor-access-management-v11"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#baseline-visitor-access-management-v11 -->

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

<a id="build-yap-wrapper"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#build-yap-wrapper -->

# build-yap-wrapper.js

`build-yap-wrapper.js` wraps decoded Yeeflow application package JSON into a `.yap` file using the confirmed Yeeflow application export format:

```text
[______gizp______] + base64(gzip(Resource JSON))
```

The tool is read-only with respect to source exports. It does not import packages, operate Yeeflow UI, or modify original `.yap`, `.ydl`, or `.ywf` files.

## Usage

```bash
node build-yap-wrapper.js \
  ./some-decoded-yap-data.json \
  ./rebuilt-test.yap \
  --title "Test App" \
  --description "Round-trip rebuilt application package"
```

For rebuilding a decoded Resource JSON from a real export during compatibility testing:

```bash
node build-yap-wrapper.js \
  ./real-export-resource.json \
  ./rebuilt-test.yap \
  --title "Test App" \
  --description "Round-trip rebuilt application package" \
  --validation-mode compatibility
```

Options:

- `--title`: required top-level wrapper title.
- `--description`: optional top-level wrapper description.
- `--icon-url`: optional top-level wrapper icon; if omitted, the builder uses the root `ListModel.IconUrl` when present.
- `--validation-mode generator`: strict mode for generated app packages.
- `--validation-mode compatibility`: tolerant mode for round-tripping real historical exports.

## Supported Input Shapes

The input may be:

1. Decoded `Resource.Data` JSON:

```json
{
  "Item": {},
  "Childs": [],
  "Forms": []
}
```

2. Decoded Resource-level JSON:

```json
{
  "MainListType": 1024,
  "AppID": 41,
  "ReplaceIds": [],
  "ReportIds": [],
  "FormKeys": [],
  "Data": "{...}",
  "SimplePortal": null
}
```

When Resource-level JSON is provided, safe Resource keys are preserved and `Data` is replaced with the normalized JSON string for the decoded app data.

## Output Wrapper Shape

The generated `.yap` wrapper uses:

```json
{
  "Title": "Test App",
  "Description": "Round-trip rebuilt application package",
  "IconUrl": "<icon-url-or-root-listmodel-icon>",
  "IsListSet": true,
  "Resource": "[______gizp______]<base64-gzip-resource-json>"
}
```

The Resource JSON preserves or constructs:

- `MainListType`
- `AppID`
- `ReplaceIds`
- `ReportIds`
- `FormKeys`
- `Data` as a JSON string
- `SimplePortal`
- other Resource-level keys present in the source Resource JSON

## Pre-Write Validation

Before writing the wrapper, the builder checks:

- input JSON parses
- decoded app data contains `Data.Item`
- decoded app data contains `Data.Childs`
- `Data.Forms` is an array when present
- `AppID` exists at Resource or root ListModel level
- `ReplaceIds` exists or can be collected from the decoded app data
- no unresolved placeholders matching `^__.*REQUIRED.*__$`
- `validate-yap-package.js` passes in the selected validation mode
- `validate-yap-graph.js` passes in the selected validation mode

In generated/final mode, validator failures stop the build.

## Round-Trip Validation

After writing, the builder immediately:

1. Reads the generated `.yap`.
2. Confirms top-level wrapper JSON is valid.
3. Confirms the Resource prefix.
4. Base64-decodes Resource.
5. Gunzips Resource.
6. Parses Resource JSON.
7. Parses Resource.Data JSON.
8. Confirms decoded Resource.Data equals the source decoded app data.
9. Runs `validate-yap-package.js`.
10. Runs `validate-yap-graph.js`.
11. Confirms unresolved placeholders remaining equals `0`.

## Report Shape

The builder prints JSON:

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
    "packageValidationPassed": true,
    "graphValidationPassed": true,
    "placeholdersRemaining": 0
  }
}
```

## Safety Rules

- Preserve large numeric IDs as strings.
- Do not print raw token, secret, password, credential, client secret, API key, access key, or authorization values.
- Do not import generated packages automatically.
- Do not use compatibility-mode success as proof that a generated app is ready.
- Use generator/final validation for generated app packages.
- Do not build if placeholders remain.
- Do not build if package or graph validation fails.

## Current Limitations

This is the first wrapper builder. It packages an already-decoded app structure; it does not generate app resources from a normalized spec.

It does not yet:

- generate a new app from business requirements
- compare exported-back `.yap` packages
- infer missing root app/ListModel metadata
- repair graph references
- validate every report/dashboard/AI schema deeply

For the first Department Access Management `.yap` generation test, this builder should be used only after decoded child lists, approval form DefResource, `ReplaceIds`, package structure, and app graph all validate successfully.

## v5 Baseline Note

The `Department Access Management_v5` test showed that wrapper `IconUrl` cannot be treated as optional for generated app packages. Earlier generated packages with missing or null top-level icon metadata produced incomplete import-dialog behavior. The builder should preserve or set a non-null icon for generator/final packages.

---

<a id="examples-summary"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#examples-summary -->

# Examples Summary

## Proven Baseline: Department Access Management v5

Use `references/collection-01.md#baseline-department-access-management-v5` for the full evidence.

The app contains:

- `Departments` data list
- `Department Access Requests` data list
- `Department Access Request` approval form

Proven sandbox result:

- app imports
- import dialog shows name, description, and icon
- app opens from workspace
- app header/navigation renders
- both lists open
- Departments sample rows display
- request sample rows display
- approval form opens and renders

Critical learned rules:

- root app shell must be complete
- wrapper `IconUrl` must be non-null
- root `CustomType = ""`
- root `Perm = 0`
- root `WorkspaceID` present
- root `LayoutView` navigation populated
- root Type `103` app page exists
- Type `103` page `LayoutID` is included in `ReplaceIds`
- Type `103` page `LayoutInResources` IDs are separate from `LayoutID` and excluded from `ReplaceIds`
- `AppTags`, `AppMetadatas`, and `AppComponents` arrays exist
- `AppThemes` is non-empty
- root `CreatedBy` and `ModifiedBy` populated

## Related Lookup Samples

Standalone `.ydl` external lookup:

- target list is imported separately
- target record IDs are external
- exclude target record IDs from the dependent package `ReplaceIds`

App-level `.yap` internal lookup:

- target and dependent lists are packaged together
- target sample record IDs are local package IDs
- include target sample record IDs in `ReplaceIds`
- dependent sample lookup values may reference local target sample IDs as plain strings
- export-back proved Yeeflow remaps both target records and dependent lookup values consistently

## First Future App Pattern

For a new small app, start with:

- one root app/listset
- one reference list
- one transactional list
- one lookup from transactional list to reference list
- one approval form
- one `ContentList` action writing to the transactional list

Avoid reports, dashboards, AI resources, connections, document libraries, and external actions until the base app imports and export-back comparison passes.

## Current Advanced Baseline: Visitor Access Management v11

Use `references/collection-01.md#baseline-visitor-access-management-v11` for the full evidence chain.

Proved:

- fresh `216...` ID family
- fresh FlowKey/form key `VBB`
- app-level approval form `Data.Forms[].ListID = 0`
- multi-field expansion after field types were proven
- text/input, number/input_number, radio/dropdown, switch/boolean, and conditional display
- app package validation, graph validation, approval form validation, wrapper round-trip, and runtime import test

Proven storage and `ContentList` mappings:

- `VisitorEmail -> Text13`
- `VisitorPhone -> Text14`
- `NumberofVisitors -> Decimal1`
- `AccessType -> Text15`
- `RequiresEscort -> Bit1`

Sample value shapes:

- Decimal: numeric
- choice/dropdown: selected option text
- data-list Bit/switch: `"1"` / `"0"`
- approval switch: boolean `true` / `false`

Use this baseline before generating richer app-level packages with multiple field/control types.

---

<a id="knowledge-base-app-pattern"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#knowledge-base-app-pattern -->

# Knowledge Base App Pattern

Source studied: `/Users/Renger/Downloads/Knowledge Base_1.yap`

Use this when generating a small Knowledge Base-style `.yap` app that combines local lists and dashboard pages.

## Safe Runtime-Proven Scope

Generate:

- one root app/listset
- one Type `103` Home dashboard page
- local `Categories` list
- local `Articles` list
- plain text article category labels
- local category/article sample rows
- root navigation for Home, Categories, and Articles

After the v4 and v11 baselines are accepted, the Phase 1 completion package may also include:

- a plain local `Sections` list
- an `Article Library` Type `103` dashboard
- flat article/category/section dashboard Collections
- `Categories.Decimal1` Order
- `Articles.Text3` Category Lookup sorted by `Categories.Decimal1`
- `Articles.Text4` kept as a plain input slot

Defer:

- `Sections`
- article-to-category lookup metadata until the v4 plain text baseline is already proven; use the v11 lookup-isolation shape only when the test explicitly includes lookup
- richtext article bodies
- icon-upload images
- article detail pages
- nested category-to-article dashboard Collections
- Search query-param flow
- Admin application-URL cards
- forms, workflows, reports, AI modules, connections, document libraries

## App Shell Rules

- root app/listset keeps `CustomType: ""`, `Perm: 0`, non-null `IconUrl`, `WorkspaceID`, `CreatedBy`, and `ModifiedBy`
- root `LayoutView.sort[]` includes duplicate Type `103` entries for the Home dashboard when following the proven dashboard shell pattern
- local list navigation entries point to included child list IDs
- `AppTags`, `AppMetadatas`, `AppComponents`, `AppThemes`, and `OtherModules` are present as arrays
- child lists keep app-level metadata in the proven generated shape: populated `TenantID`, `CreatedBy`, and `ModifiedBy`, with child `WorkspaceID: null`
- do not add an ad hoc `ListSetID` property directly to the root `ListModel`

## ReplaceIds Rules

Include:

- root ListSetID
- dashboard LayoutID
- local list IDs
- generated field IDs
- generated list view/layout IDs
- local sample row IDs

Exclude:

- tenant/user IDs
- AppID
- any external dependency IDs

## Runtime Checklist

- import metadata parses app name and icon
- app appears in Shared Workspace
- Home dashboard opens and renders
- local list navigation renders
- Categories and Articles list queries do not return `400`

## Runtime Lessons

- v1 imported and opened but showed Yeeflow's empty component shell, so import alone is not enough.
- v2/v3 rendered Home/Categories, but Articles stayed on the loading spinner.
- v4 passed after correcting child-list metadata, keeping list `LayoutView: null`, removing the first-stage lookup, and preserving native `Title.FieldIndex: 0`.
- v5/v6/v7 article-to-category lookup isolations imported and rendered Home/Categories, but Articles stayed on the loading spinner. The failed variants used `Articles.Text3` lookup with local sample IDs, blank values, and hidden list-view visibility.
- The source template uses `Articles.Text4` for Category lookup; v8 validates and round-trips with that source-like slot, imports, and renders Home, but Categories and Articles stay on loading spinners. Lookup generation remains blocked.
- v9 removes the generated `Text3` placeholder and adds only blank `Articles.Text4` lookup metadata. It imports, renders Home, and opens Categories, but Articles still stays on the loading spinner. This narrows the blocker to Articles lookup metadata.
- v10 adds `Articles.Text4` as a plain input field and imports successfully. Home renders and Categories opens, but Articles stays on a loading spinner with Chrome console `Uncaught RangeError: Wrong length!`. Do not treat the plain non-contiguous `Text4` field slot as proven.
- v11 follows the user's exported v10 update and is the first generated Knowledge Base lookup isolation to pass runtime: `Categories.Decimal1` Order, `Articles.Text4` plain input slot, and `Articles.Text3` Category Lookup sorted by `Decimal1`. Articles loaded after one refresh, and the new-item lookup dropdown resolved Categories in Order sequence. Keep v4 as the safest initial app baseline.
- `knowledge-base-phase1-full.generated.yap` is the first broader Phase 1 generated app baseline. It imported, opened, rendered Home and Article Library Collections, and opened Categories/Sections/Articles with sample rows after refresh stabilization where needed. It keeps the v11 first-open refresh caveat and should not yet be treated as production-ready for lookup sample values, nested Collections, or source-style section lookup chains.

---

<a id="validate-yap-graph"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#validate-yap-graph -->

# validate-yap-graph.js

`validate-yap-graph.js` is a read-only Yeeflow `.yap` application graph validator. It decodes a wrapped `.yap` export or reads a decoded `Resource.Data` JSON file, builds an internal relationship graph, and reports whether resources resolve to each other correctly.

It does not import packages, operate Yeeflow UI, modify exports, or create `.yap` packages.

## Usage

```bash
node validate-yap-graph.js "./Procurement Management.yap" --mode compatibility

node validate-yap-graph.js "./generated-app.decoded.json" \
  --mode generator \
  --stage draft \
  --json ./generated-app.graph.json \
  --md ./generated-app.graph.md

node validate-yap-graph.js "./generated-app.decoded.json" \
  --mode generator \
  --stage final
```

Options:

- `--mode compatibility`: tolerant mode for real historical exports.
- `--mode generator`: strict mode for generated app packages.
- `--stage draft`: unresolved placeholders and planned dependencies are warnings/dependencies.
- `--stage final`: unresolved placeholders and missing references are errors.
- `--json <path>`: writes the full machine-readable report.
- `--md <path>`: writes a Markdown summary.

## What It Validates

The validator builds graph nodes for:

- root app/listset resources
- data lists
- fields
- approval forms
- list workflows and scheduled workflows
- form pages
- reports
- dashboards
- document libraries
- AI agents, copilots, knowledges, connections, and other modules when detectable

It builds edges for:

- root app navigation entries to app pages, child lists, approval forms, and process links
- data-list lookup fields to target lists and display/search fields
- approval-form lookup controls to source lists and additional-field mappings
- `ContentList` workflow actions to target lists and mapped fields
- `QueryData` actions to target lists
- form page/task URL references
- report/dashboard/module references to lists when parseable
- AI/document/external workflow dependencies as dependency warnings

## Relationship Checks

The tool checks:

- lookup target lists resolve inside the package or are reported as dependencies
- lookup display and search fields resolve
- internal lookup sample values point to target sample records in the same package where sample data is available
- approval-form lookup source lists and additional mappings resolve where parseable
- `ContentList` target lists and target fields resolve
- `QueryData` target lists resolve
- workflow sequence-flow references resolve
- workflow graphs are connected enough to inspect
- reports and dashboards reference known lists when parseable
- AI, connection, knowledge, document, and external API resources are inventoried as sensitive dependencies
- local generated graph IDs are present in `Resource.ReplaceIds`
- unresolved external dependency IDs are not placed in `ReplaceIds`
- unresolved placeholders follow the requested stage policy
- list-level dependency cycles are reported
- root navigation entries resolve to known app pages, child lists, forms, or supported process links

## Output Shape

The JSON report follows this shape:

```json
{
  "status": "pass | pass_with_warnings | fail",
  "mode": "compatibility | generator",
  "stage": "draft | final",
  "errors": [],
  "warnings": [],
  "dependencies": [],
  "graph": {
    "nodes": [],
    "edges": []
  },
  "summary": {
    "nodes": 0,
    "edges": 0,
    "lists": 0,
    "fields": 0,
    "approvalForms": 0,
    "listWorkflows": 0,
    "reports": 0,
    "dashboards": 0,
    "agents": 0,
    "connections": 0,
    "lookups": 0,
    "contentListEdges": 0,
    "queryDataEdges": 0,
    "unresolvedEdges": 0,
    "cycles": 0
  }
}
```

## Severity Policy

Compatibility mode is intentionally tolerant. Real `.yap` exports can contain references to external lists, documents, AI resources, or historical workflow shapes. These are reported as warnings or dependencies.

Generator final mode is strict. A generated `.yap` package should not have unresolved lookup targets, unresolved `ContentList` targets, unresolved target fields, unresolved placeholders, or unresolved AI/resource references unless they are explicitly modeled as external dependencies and excluded from `ReplaceIds`.

## Safety Rules

- Preserve large numeric IDs as strings.
- Redact secret-like fields in reports.
- Do not print raw token, credential, client secret, API key, or authorization values.
- Do not include external dependency IDs in `ReplaceIds`.
- Do not treat a graph report as production proof until a sandbox import/export round trip confirms Yeeflow accepts and preserves the package.
- For app-level packages with internal sample lookup values, local target sample record IDs should be included in `ReplaceIds`; export-back comparison should confirm Yeeflow remapped dependent lookup values to the remapped target records.

## Known Limits

Some Yeeflow report, dashboard, AI Agent, Copilot, and external action schemas are still only partially decoded. The validator inventories and scans these resources conservatively, but deeper schema-specific validation should be added as more exports are studied.

## Recommended Future Check: ID Namespace Reuse

The Visitor Access Management v5 fresh-compatible baseline showed that graph correctness also depends on avoiding accidental reuse of a generated app namespace.

Recommended additions:

- Summarize generated ID families in the graph report.
- Summarize approval form keys/FlowKeys in the graph report.
- Warn when app, list, field, layout, form, workflow, or sample record IDs appear to come from an ID family already used by a prior generated/imported app test.
- Warn or fail when an approval form key is reused across generated app tests.
- Treat known ID-family reuse as a sandbox-readiness risk even when graph edges resolve.

Reason:

- Reused IDs and form keys can leave graph edges locally valid while Yeeflow attaches or resolves imported resources incorrectly at runtime.
- The visible failure can be a blank app or detached components after an otherwise successful import.

## Visitor Access v6.1 Graph Lesson

The Visitor Access Management v6.1 test confirms the graph-level expansion rule:

- Keep the resource graph shape from the known-good baseline.
- Add one field/change at a time.
- Use a fresh ID family and fresh form key for every import-test package.
- Preserve proven field slots unless explicitly testing a new slot.

The v6 failure also exposed a package-level registration rule that graph resolution alone may not catch:

- App-level approval forms must use `Data.Forms[].ListID = 0`.
- The approval process ID belongs in `ProcModelID`.
- If `ListID` is set to `ProcModelID`, the graph can still look internally consistent, but Yeeflow may import the app with the approval form missing.

Current validator coordination:

- `validate-yap-package.js` owns the `APPROVAL_FORM_LISTID_NOT_ZERO` structural check.
- `validate-yap-graph.js` confirms the approval form navigation, pages, lookup controls, and `ContentList` edges resolve after that structural rule passes.

## Visitor Access v10 Graph Baseline

The Visitor Access Management v10 package confirms the graph-validation pattern for incremental generated app expansion through `Text12`.

Successful chain:

| Version | Field | Slot | ID family | FlowKey |
| --- | --- | --- | --- | --- |
| v6.1 | Visitor Name | `Text8` | `211...` | `VAW` |
| v7 | Visitor Company | `Text9` | `212...` | `VAX` |
| v8 | Access Area | `Text10` | `213...` | `VAY` |
| v9 | Host Employee | `Text11` | `214...` | `VAZ` |
| v10 | Visitor Contact | `Text12` | `215...` | `VBA` |

The v10 graph validation passed with:

- two data lists
- one approval form
- root navigation edges to Overview, Departments, Visitor Access Requests, and Visitor Access Request
- Department lookup edges resolving to the internal Departments list and its `Title`/`Text1` fields
- approval-form lookup controls resolving to Departments
- one `ContentList` edge resolving to Visitor Access Requests
- no unresolved edges
- no cycles

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
| Visit Date | `Datetime2` |

Graph validator expectations for future expansions:

- The new field should be represented as a local field node.
- Root navigation should remain unchanged except for deliberate app-shell work.
- The approval form should still resolve from root navigation Type `105`.
- `ContentList` must continue to target the generated request list.
- Lookup edges to Departments must remain intact.
- No reports, dashboards beyond the Type `103` shell page, AI resources, connections, document libraries, or list workflows should appear during a one-field expansion.

## Multi-Type Field Graph Recommendations

The manual Visitor Access Management v10 export confirms graph relationships that should be represented for richer approval controls. See `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`.

Recommended graph checks:

- Create graph nodes for approval form variables, not only controls/pages.
- Create edges from form controls to their bound variables.
- Create edges from dynamic display target controls to source variables referenced in `attrs.control_display[]`.
- Create edges from `ContentList` source variables to target data-list fields.
- Annotate `ContentList` edges with type compatibility:
  - `number -> Decimal/input_number`
  - `boolean -> Bit/switch`
  - `text -> Text/input/radio`
- Warn when a data-list field exists for an approval variable but no `ContentList` mapping persists it.
- Warn when a new request-page business control is not mirrored on the approval page as readonly, unless the control is explicitly request-only.

## Proven v11 Multi-Type Graph Baseline

`visitor-access-management.v11-five-fields-multitype.yap` passed graph validation and runtime testing with a multi-field, multi-type expansion.

Graph validation summary:

- Fresh ID family: `216...`
- Fresh FlowKey/form key: `VBB`
- App-level approval form `Data.Forms[].ListID = 0`
- Nodes: `39`
- Edges: `21`
- Lists: `2`
- Fields: `27`
- Approval forms: `1`
- Lookup edges: `3`
- ContentList edges: `1`
- Unresolved edges: `0`
- Cycles: `0`
- ReplaceIds: `46`

Confirmed target fields and mappings:

| Source variable | Target field | Compatibility |
| --- | --- | --- |
| `VisitorEmail` | `Text13` | text -> Text/input |
| `VisitorPhone` | `Text14` | text -> Text/input |
| `NumberofVisitors` | `Decimal1` | number -> Decimal/input_number |
| `AccessType` | `Text15` | text/radio dropdown -> Text/radio |
| `RequiresEscort` | `Bit1` | boolean/switch -> Bit/switch |

Conditional display graph rule:

- `EscortUser` is a form-only variable/control.
- Its target control contains `attrs.control_display[]`.
- The display condition references source variable `RequiresEscort`.
- The condition value is boolean `true`.
- It is not a graph persistence edge in v11 because there is no target storage field.

Graph validator implication:

- Multi-field expansion should still produce one coherent `ContentList` target edge to the generated request list.
- Richer control types should not alter root navigation, Department lookup edges, approval-form lookup edges, or app shell edges.
- The graph should flag a new persisted business variable if no compatible `ContentList` target field exists.
- Form-only conditional fields may be allowed when explicitly documented as not persisted.

---

<a id="validate-yap-package"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#validate-yap-package -->

# validate-yap-package.js

`validate-yap-package.js` is a read-only structural validator for Yeeflow `.yap` application packages.

It accepts either:

- a wrapped `.yap` export, or
- a decoded `Resource.Data` JSON file.

It does not import packages, operate the Yeeflow UI, or modify source exports.

## Usage

```bash
node validate-yap-package.js "./NHIC Innovation Ecosystem Platform.yap" --mode compatibility
node validate-yap-package.js "./generated-app.decoded.json" --mode generator --stage draft
node validate-yap-package.js "./generated-app.decoded.json" --mode generator --stage final
```

## Modes

| Mode | Purpose |
| --- | --- |
| `compatibility` | Tolerant validation for historical real exports. |
| `generator` | Stricter validation for generated packages. |

## Stages

| Stage | Placeholder behavior |
| --- | --- |
| `draft` | unresolved `__...REQUIRED...__` placeholders are warnings |
| `final` | unresolved placeholders are errors in generator mode |

## Checks

The validator checks:

- `.yap` wrapper JSON validity
- `Resource` prefix, base64, and gzip decoding
- decoded Resource JSON validity
- decoded `Resource.Data` JSON validity
- required `Data` sections such as `Item`, `Childs`, `Forms`, reports, modules, groups, and themes
- root app/ListSet metadata
- generated app-shell readiness, including wrapper icon, root `CustomType`, root `Perm`, root `WorkspaceID`, root audit users, app theme arrays, and root navigation
- root Type `103` app page registration, including the rule that `LayoutInResources` resource IDs are separate from the page `LayoutID`
- child resource `ListModel`, `Defs`, `Layouts`, custom forms, and sample data
- workflow/form `DefResource` parsing
- approval form page URLs and task URLs
- workflow graph references
- `ContentList` target list/field references
- `QueryData` target references where parseable
- lookup field target list/display field resolution
- report and dashboard list references where parseable
- `ReplaceIds` presence, duplicates, and local ID coverage
- app-level internal lookup sample values, where same-package target sample records should exist and target sample record IDs should be included in `ReplaceIds`
- sensitive resource warnings for token/credential-like lists/fields, connections, AI agents, knowledges, document libraries, HTTP nodes, and document generation

## Output

The script prints JSON:

```json
{
  "status": "pass | pass_with_warnings | fail",
  "mode": "compatibility | generator",
  "stage": "draft | final",
  "errors": [],
  "warnings": [],
  "dependencies": [],
  "summary": {
    "childResources": 0,
    "dataLists": 0,
    "forms": 0,
    "approvalForms": 0,
    "listWorkflows": 0,
    "reports": 0,
    "dashboards": 0,
    "agents": 0,
    "copilots": 0,
    "connections": 0,
    "knowledges": 0,
    "replaceIds": 0,
    "lookupRelationships": 0,
    "contentListReferences": 0
  }
}
```

`fail` exits with code `1`.

## Safety Rules

- Preserve large numeric IDs as strings.
- Do not print raw token, secret, password, credential, API key, or client secret values.
- Treat AI Agents, Knowledges/Copilots, Connections, document libraries, HTTP nodes, and document-generation nodes as dependencies or risks.
- Do not treat compatibility warnings from historical exports as generation approval.
- Generated final packages still need app-graph validation and sandbox import/export comparison before production use.
- A generator/final pass means the package matches known structural rules; it does not prove runtime behavior until sandbox import/open/export-back testing passes.
- For internal lookup sample data, export-back should confirm dependent lookup values were remapped to the exported target sample record IDs.

## Current Scope

This first validator is structural. It now includes the app-shell checks learned from the successful `Department Access Management_v5` baseline, but it is still not a complete runtime validator.

Future companion tools should include:

- `validate-yap-graph.js`
- `build-yap-wrapper.js`
- `compare-yap-export-back.js`
- `generate-yap-dependency-map.js`

## Recommended Future Check: Fresh ID Families

The Visitor Access Management v5 fresh-compatible baseline showed that app-level generated packages need a fresh local ID family and fresh approval form key for each sandbox test.

Recommended additions:

- Report the apparent generated ID family for root app/listset, child lists, fields, layouts, forms, workflow nodes, and sample records.
- Report approval form keys/FlowKeys found in `Data.Forms[]`.
- In generator mode, warn when a package appears to reuse a known generated/imported ID family.
- In generator mode, warn or fail when a package reuses a known generated/imported approval form key.
- In generator/final mode, recommend a fresh ID family unless the package explicitly declares that it is testing update behavior.

Reason:

- A package can pass structural validation and import, but still open as a blank app if generated IDs or form keys collide with an already-imported generated app.

## Implemented Check: App-Level Approval Form `ListID`

The Visitor Access Management v6 expansion showed that app-level approval form rows must keep `Data.Forms[].ListID = 0`.

The validator now fails generator/final mode when an approval form has a non-zero `Data.Forms[].ListID`.

Failure caught:

```json
{
  "code": "APPROVAL_FORM_LISTID_NOT_ZERO",
  "message": "App-level approval form registration should keep Data.Forms[].ListID as 0; using the ProcModelID here can import without registering the form in navigation."
}
```

Correct pattern:

```json
{
  "Key": "VAW",
  "ListID": 0,
  "ProcModelID": "2110030000000000001"
}
```

Generator implication:

- Keep app-level approval form `ListID` as numeric `0`.
- Store the generated workflow/form process ID in `ProcModelID`.
- Keep the form key in `Key` and `Resource.FormKeys`.
- Keep root navigation Type `105` pointed at the form key.

## Proven Visitor Expansion Validation Pattern

The Visitor Access Management v6.1 through v10 packages confirm the package-validation profile for incremental generated app expansion.

Successful chain:

| Version | Field | Slot | ID family | FlowKey |
| --- | --- | --- | --- | --- |
| v6.1 | Visitor Name | `Text8` | `211...` | `VAW` |
| v7 | Visitor Company | `Text9` | `212...` | `VAX` |
| v8 | Access Area | `Text10` | `213...` | `VAY` |
| v9 | Host Employee | `Text11` | `214...` | `VAZ` |
| v10 | Visitor Contact | `Text12` | `215...` | `VBA` |

The v10 generated package passed package validation with:

- app-level approval form `Data.Forms[].ListID = 0`
- one approval form
- two child data lists
- 16 fields in `Visitor Access Requests`
- `Visitor Contact` added as `Text12`
- `ReplaceIds` including the new local field ID
- no placeholders, errors, warnings, AI resources, connections, or external dependencies

Package validator expectations for future expansions:

- Fail generator/final mode if app-level approval form `Data.Forms[].ListID` is not `0`.
- Report or warn on apparent reuse of a prior generated ID family or FlowKey when detectable.
- Confirm new local field IDs are included in `ReplaceIds`.
- Confirm no unrelated modules are accidentally introduced during a one-field expansion.
- Pair package validation with graph validation and extracted approval-form validation before wrapper build.

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
| Visit Date | `Datetime2` |

## Multi-Type Field Validation Recommendations

The manual Visitor Access Management v10 export confirmed additional app-level field/control patterns. See `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`.

Recommended package checks:

- Data-list number fields:
  - `FieldType = "Decimal"`
  - `Type = "input_number"`
  - `Rules` parses when present
  - known numeric attrs such as `displayThousandths`, `rounded-to`, `number_min`, and future `number_max` have valid shapes
- Data-list switch fields:
  - `FieldType = "Bit"`
  - `Type = "switch"`
  - sample values should be `"1"`, `"0"`, empty, or absent
- Approval number controls:
  - control `type = "input_number"`
  - bound variable exists and has `type = "number"`
  - default control `value`, if present, is numeric
- Approval choice controls:
  - control `type = "radio"`
  - bound variable exists and has `type = "text"`
  - `attrs.choices[]` is a non-empty string array
  - dropdown controls use `attrs.displayStyle = "dropdown"`
  - `attrs.color_choices[]`, if present, aligns with choices
- Approval switch controls:
  - control `type = "switch"`
  - bound variable exists and has `type = "boolean"`
  - control `value`, if present, is boolean
- Dynamic display:
  - target control `attrs.control_display[]` references an existing source variable
  - `controlId` matches the target control ID
  - condition value type is compatible with the source variable type
- Persistence:
  - `ContentList` mappings from number variables should target `Decimal/input_number`
  - boolean variables should target `Bit/switch`
  - text/radio variables should target text/radio-compatible fields

## Proven v11 Multi-Type Package Validation Baseline

`visitor-access-management.v11-five-fields-multitype.yap` is the first generated app-level package that passed validation and runtime testing with multiple new field/control types in one expansion.

Successful validation profile:

- Fresh ID family: `216...`
- Fresh FlowKey/form key: `VBB`
- App-level approval form `Data.Forms[].ListID = 0`
- Package validation: `pass`
- Graph validation: `pass`
- Extracted approval-form validation: `pass`
- Wrapper round-trip validation: `pass`
- `ReplaceIds`: `46`
- Child resources: `2`
- Approval forms: `1`
- Dashboards: `1` root Type `103` shell page only
- No agents, copilots, connections, knowledges, document libraries, list workflows, or external actions

v11 confirms these generated package structures:

| Field | Slot | Expected package validation |
| --- | --- | --- |
| Visitor Email | `Text13` | `Text` / `input` field with local field ID in `ReplaceIds` |
| Visitor Phone | `Text14` | `Text` / `input` field with local field ID in `ReplaceIds` |
| Number of Visitors | `Decimal1` | `Decimal` / `input_number` field with parseable numeric rules |
| Access Type | `Text15` | `Text` / `radio` field for dropdown-compatible text choice |
| Requires Escort | `Bit1` | `Bit` / `switch` field with switch-compatible sample values |

Confirmed sample value shapes:

- Decimal samples are numeric values.
- Choice/dropdown samples are selected option text.
- Bit/switch list samples are `"1"` or `"0"`.
- Approval switch controls use boolean `true` or `false`.

Validation rule upgrade:

- Multi-field expansion is acceptable in generator/final mode after the underlying field/control types are proven.
- The validator should continue to fail unresolved placeholders, non-zero approval-form `ListID`, missing local IDs in `ReplaceIds`, unresolved target fields, and incompatible `ContentList` mappings.
- Conditional display should validate that `attrs.control_display[]` references an existing source variable and that the target control ID matches the owning control.

---

<a id="yap-structure-study"></a>

<!-- source: skills/yeeflow-application-generator/references/collection-01.md#yap-structure-study -->

# Yeeflow `.yap` Application Package Study

This report studies two full Yeeflow application exports:

- `NHIC Innovation Ecosystem Platform.yap`
- `Procurement Management.yap`

The goal is to understand `.yap` as the application-level package format that can combine data lists, approval forms, list workflows, reports, dashboards/pages, document libraries, and AI-related resources.

This study is read-only. No package was imported, no Yeeflow UI was operated, and the source exports were not modified.

Generated study outputs:

- `nhic-yap-study-fresh-inspection.json`
- `nhic-yap-study-fresh-inspection.md`
- `nhic-yap-study-fresh-metadata.json`
- `nhic-yap-study-fresh-metadata.md`
- `procurement-yap-study-fresh-inspection.json`
- `procurement-yap-study-fresh-inspection.md`
- `procurement-yap-study-fresh-metadata.json`
- `procurement-yap-study-fresh-metadata.md`

## 1. `.yap` File Format Anatomy

Both `.yap` files are plain JSON wrappers with the same top-level shape:

```json
{
  "Title": "...",
  "Description": "...",
  "IconUrl": "...",
  "IsListSet": true,
  "Resource": "[______gizp______]..."
}
```

The `Resource` field uses the same prefix and compression pattern learned from `.ydl`:

1. Prefix: `[______gizp______]`
2. Base64 payload
3. Gzip-compressed Resource JSON
4. Resource JSON contains a `Data` property
5. `Data` is a JSON string containing the decoded application package

Decoded Resource keys:

```json
{
  "MainListType": 1024,
  "AppID": 41,
  "ReplaceIds": [],
  "ReportIds": [],
  "FormKeys": [],
  "Data": "{...}",
  "SimplePortal": null
}
```

Decoded `Resource.Data` keys:

```json
{
  "Item": {},
  "Childs": [],
  "Forms": [],
  "FormReports": [],
  "DataReports": [],
  "FormNewReports": [],
  "AppGroups": [],
  "AppTags": null,
  "AppMetadatas": null,
  "AppThemes": [],
  "AppComponents": null,
  "PortalInfo": null,
  "OtherModules": []
}
```

Confirmed differences:

| Format | Package role | Main payload | Resource wrapper |
| --- | --- | --- | --- |
| `.ydl` | Single data list package | `Data.Item` is the data list | gzip/base64 `Resource` |
| `.ywf` | Single approval form/workflow | top-level wrapper with base64 `Def` | no `.ydl/.yap` `Resource.Data` structure |
| `.yap` | Full app/listset package | `Data.Item` is root app/listset; `Data.Childs` are resources | gzip/base64 `Resource` |

## 2. Application Resource Inventory

### NHIC Innovation Ecosystem Platform

Package summary:

- Title: `NHIC Innovation Ecosystem Platform`
- AppID: `41`
- Main ListSetID: `2033878389230415872`
- MainListType: `1024`
- ReplaceIds: `175`
- ReportIds: `14`
- FormKeys: `3`

| Resource type | Count | Examples | Where stored | Notes |
| --- | ---: | --- | --- | --- |
| root app/listset | 1 | NHIC Innovation Ecosystem Platform | `Data.Item` | Main application resource. |
| data lists | 5 | Portfolio Management, Partner Management, Communication Records, NHIC Grants, Contact Database | `Data.Childs[].Item` | Child lists share the root ListSetID. |
| workflow/forms | 3 | Auto-link Partner Organization, Update partner info, Sync update Partner and Portfolio | `Data.Forms[]` | WorkflowType `1`; no approval request/approval pages. |
| list workflows | 3 | Same as workflow/forms above | `Data.Forms[]` | Use `ContentList`, `QueryData`, `AI`. |
| dashboards/pages | root has 1 layout | root app page/layout | `Data.Item.Layouts[]` | Type/layout metadata present. |
| data reports | 0 | none extracted | `Data.DataReports[]` | None in this export. |
| form reports | 0 | none extracted | `Data.FormReports[]` / `Data.FormNewReports[]` | None in this export. |
| document libraries | 0 | none extracted | `Data.Childs[]` | None found. |
| AI Agents | 6 module entries | sensitive dependency | `Data.OtherModules[]` Type `Agents` | Do not expose contents blindly. |
| Copilots/knowledge | 2 knowledge entries | sensitive dependency | `Data.OtherModules[]` Type `Knowledges` | Treat as app-level dependencies. |
| connections | 2 | sensitive dependency | `Data.OtherModules[]` Type `Connections` | Credential-like risk; do not copy blindly. |
| themes/settings | 1 theme | app theme | `Data.AppThemes[]` | App-level visual/theme resource. |

### Procurement Management

Package summary:

- Title: `Procurement Management`
- AppID: `41`
- Main ListSetID: `1906896262421815296`
- MainListType: `1024`
- ReplaceIds: `421`
- ReportIds: `31`
- FormKeys: `18`

| Resource type | Count | Examples | Where stored | Notes |
| --- | ---: | --- | --- | --- |
| root app/listset | 1 | Procurement Management | `Data.Item` | Main application resource. |
| data lists | 16 | PR Records, PR Details List, PO Records, PA Records, Supplier List, Purchase Category | `Data.Childs[].Item` | Includes transactional and master/reference lists. |
| report/data resources | 1 | Supplier and PO Details | `Data.Childs[].Item` plus `DataReports[]` | Report-like list resource. |
| document libraries | 2 | Template Files, PO Documents | `Data.Childs[].Item` | Used by document generation / storage. |
| form report/list resources | 1 | PR | `Data.Childs[].Item` plus `FormNewReports[]` | Derived from approval form/report data. |
| approval forms | 4 observed | Purchase Request, Purchase Order Request, Payment Request, New Supplier Form | `Data.Forms[]` | WorkflowType `2`; typically includes two `pageurls`. |
| list/process workflows | many | status update workflows, supplier sync workflows | `Data.Forms[]` | WorkflowType `1`; often no pages. |
| scheduled jobs | 3 | SharePoint sync jobs | `Data.Forms[]` | WorkflowType `3`; includes HTTP/QueryData/Loop nodes. |
| dashboards/pages | 5 root layouts | Home Page, Procurement Overview, Supplier Onboarding | `Data.Item.Layouts[]` | Type `103` pages with embedded resources. |
| data reports | 1 | Supplier and PO Details | `Data.DataReports[]` | Joins/combines source lists. |
| form new reports | 1 | PR | `Data.FormNewReports[]` | Form-derived reporting resource. |
| AI Agents | 1 module entry | sensitive dependency | `Data.OtherModules[]` Type `Agents` | AI references also appear inside forms. |
| knowledge resources | 1 | sensitive dependency | `Data.OtherModules[]` Type `Knowledges` | Treat as dependency. |
| app groups/themes | 1 group, 1 theme | app nav/theme | `Data.AppGroups[]`, `Data.AppThemes[]` | Root app UI/permissions/navigation. |
| token/credential-like list | 1 warning | Token List | child data list | Do not expose values; treat as sensitive. |

## 3. Root App / ListSet Structure

The root application is stored in `Data.Item`.

Root `Item.ListModel` fields include:

- `TenantID`
- `AppID`
- `ListID`
- `Title`
- `Description`
- `Status`
- `IconUrl`
- `TableCode`
- `IndexCode`
- `Created`
- `Modified`
- `CreatedBy`
- `ModifiedBy`
- `Perm`
- `Type`
- `Flags`
- `CustomType`
- `WorkspaceID`
- `LayoutView`
- `IsBreakInherit`
- `IsDataSeparate`
- `AdvanceList`

Important root patterns:

- `ListModel.Type = 1024` for the application/listset root.
- Root `ListID` equals the app/listset ID.
- `Resource.MainListType = 1024`.
- Root `LayoutView` can store navigation/menu structure, app page ordering, process links, resource groups, hidden flags, and theme/menu attributes.
- Root `Layouts[]` can store app pages/dashboards. Procurement includes Type `103` page layouts such as Home Page and Procurement Overview.
- Child resources link to the root app through shared `ListSetID` and `CustomType` values such as `ListSite_<ListSetID>`.

## 4. Child Data List Structure Inside `.yap`

Child lists are stored under `Data.Childs[]`, with each child using the same `Item` shape learned from standalone `.ydl`.

Typical child list shape:

```json
{
  "Item": {
    "ListModel": {},
    "Defs": [],
    "Layouts": [],
    "PublicForms": [],
    "RemindRules": [],
    "FlowMappings": [],
    "ListDatas": {}
  }
}
```

Comparison with standalone `.ydl`:

| Topic | Standalone `.ydl` | Inside `.yap` | Generator implication |
| --- | --- | --- | --- |
| Root payload | `Data.Item` is the list | `Data.Item` is the app; list is in `Data.Childs[]` | App generator must wrap lists as children. |
| ListSetID | May be generated or imported standalone | Shared root ListSetID | Related lists can be packaged together. |
| Lookup target availability | Often external or staged | Target list can be in same package | `.yap` can solve staged lookup remapping. |
| ReplaceIds | Local list IDs/field/layout/sample IDs | App, child list, field, layout, report/page IDs together | Local app graph IDs should be remapped together. |
| Forms/workflows | List workflows may appear inside list package | App workflows are centralized in `Data.Forms[]` | Generate app-level workflow graph. |
| Views/custom forms | `Item.Layouts[]` | Same inside child `Item.Layouts[]` | Reuse `.ydl` custom form rules. |
| Sample data | Optional `Item.ListDatas` | Optional; samples may be present | Include only safe sample data. |

The app-level package can include both lookup source and dependent lists. That means their generated IDs can be local to the same package and remapped consistently by Yeeflow during import.

## 5. Approval Form / Workflow Structure Inside `.yap`

Application workflows/forms are stored in `Data.Forms[]`.

Each form row includes keys such as:

- `Name`
- `Key`
- `IsItemPerm`
- `AppID`
- `ListID`
- `ProcModelID`
- `Description`
- `Ext`
- `DefResource`
- `ImgResource`
- `Deployed`
- `NoRule`
- `WorkflowType`
- `Settings`

`DefResource` contains the workflow/form Def payload. It can be decoded to the same kind of Def structure used by standalone `.ywf`:

- `defkey`
- `workflowType`
- `variables`
- `pageurls`
- `childshapes`
- workflow graph metadata
- `formdef` pages
- `ContentList` nodes
- lookup controls
- AI nodes/actions where present

Comparison with standalone `.ywf`:

| Topic | Standalone `.ywf` | Inside `.yap` | Generator implication |
| --- | --- | --- | --- |
| Wrapper | `.ywf` wrapper with base64 `Def` | form row in `Data.Forms[]` with `DefResource` | App builder must embed forms rather than emit separate `.ywf`. |
| FlowKey | top-level wrapper `FlowKey` plus Def `defkey` | `Forms[].Key` plus Def `defkey` | Keep keys aligned. |
| WorkflowType | wrapper and Def | `Forms[].WorkflowType` and Def | Keep both aligned. |
| Page URLs | inside Def | inside decoded `DefResource` | Same page registration rules apply. |
| Graph metadata | inside Def | inside decoded `DefResource` | Same graph layout rules apply. |
| ContentList targets | external if standalone | can target child lists in same app package | App graph validation can resolve local targets. |
| Lookups | external or staged | can target child lists in same app package | App-level generation can avoid staged `.ydl` handoff. |

WorkflowType observations:

| WorkflowType | Observed meaning | Examples |
| ---: | --- | --- |
| `1` | List/process workflow without approval pages, or special workflow with pages in some cases | status updates, sync jobs, supplier workflows |
| `2` | Approval form/workflow with request/approval pages | PR, PO, PA, New Supplier Form |
| `3` | Scheduled/job-like workflow | SharePoint sync jobs |

## 6. Relationship / Dependency Model

Relationship graph nodes should include:

- app/listset root
- data lists
- fields
- approval forms
- list workflows
- reports
- dashboards/pages
- document libraries
- AI Agents
- knowledge/copilot-like resources
- connections

Relationship graph edges should include:

- lookup field references
- approval form lookup controls
- lookup additional-field mappings
- `ContentList` create/update/remove targets
- `QueryData` targets
- report source list references
- dashboard/page report/list references
- approval page task URL/page references
- document generation template/library references
- AI Agent/Copilot/action references
- workflow to workflow/list trigger references

### NHIC relationship summary

Observed relationships:

- `ContentList`: 3
- approval-form lookup controls: 0 found by current extractor
- document libraries: 0
- AI references: 1 workflow AI node/action
- reports: 0 extracted

Representative ContentList edges:

- Auto-link Partner Organization -> Communication Records
- Update partner info -> Portfolio Management
- Sync update Partner and Portfolio -> Communication Records

### Procurement relationship summary

Observed relationships:

- `ContentList`: 45
- lookup controls: 12
- document library dependencies: 2
- AI references: 7
- reports: 2

Representative ContentList edges:

- Purchase Request -> PR Records
- Purchase Request -> PR Details List
- Purchase Order Request -> PO Records
- Purchase Order Request -> PO-Payment Terms
- Purchase Order Request -> PR Records / PR Details List updates
- Payment Request -> PA Records
- Payment Request -> PO-Payment Terms / PO Records updates
- Purchase Order Request -> PO Documents

Representative lookup edges:

- Purchase Request category lookup -> Purchase Category
- Purchase Order request lookup -> PR Records
- Purchase Order item lookup-list -> PR Details List
- Purchase Order supplier lookup -> Supplier List
- Payment supplier lookup -> Supplier List
- Payment records lookup-list -> PO-Payment Terms

Report edges:

- Supplier and PO Details report -> PO Details List + supplier/pre-supplier list inputs
- PR form new report -> PR form/report data

## 7. ReplaceIds and Import Remapping

`Resource.ReplaceIds` is central to app import/remapping.

Observed counts:

| Package | ReplaceIds | ReportIds | FormKeys |
| --- | ---: | ---: | ---: |
| NHIC Innovation Ecosystem Platform | 175 | 14 | 3 |
| Procurement Management | 421 | 31 | 18 |

Observed ReplaceIds contents:

| ID type | NHIC in ReplaceIds | Procurement in ReplaceIds | Notes |
| --- | ---: | ---: | --- |
| root app/listset ID | yes | yes | App root ID is local and remappable. |
| child list IDs | yes | yes | All extracted child list IDs are included. |
| field IDs | yes | yes | All extracted field IDs are included. |
| layout/page IDs | yes | yes | All extracted layout IDs are included. |
| sample record IDs | none observed | none observed | These exports did not include list sample records in ReplaceIds. |
| form keys | stored separately | stored separately | `FormKeys[]` tracks workflow/form keys. |
| report IDs | stored separately and/or in ReplaceIds-related structures | stored separately and/or in ReplaceIds-related structures | Reports need their own validation. |
| generated app graph IDs | yes | yes | Additional internal IDs appear beyond list/field/layout IDs. |

Rules:

- IDs local to the generated `.yap` should be in `ReplaceIds`.
- External dependency IDs should not be in `ReplaceIds`.
- Local related lists can be packaged together so lookup target and dependent lookup fields are remapped consistently.
- App-level packages can solve the staged standalone lookup issue by including both sides of a relationship in one local ID graph.
- Sensitive external resources such as live connections, credentials, AI agent IDs, external document templates, or production library IDs should not be guessed or blindly copied.

This differs from standalone `.ydl`: in `.ydl`, external lookup record/list IDs must be excluded from `ReplaceIds` or the imported dependent list can point to remapped IDs that do not exist. In `.yap`, if both lists are local to the package, both IDs can be remapped together.

Export-back from the generated `Department Access Management_v5` package confirms the internal sample lookup rule:

- Department target sample records were remapped.
- Department Access Request sample lookup values were also remapped.
- Exported-back request rows point to the exported-back Department `ListDataID` values.
- Therefore app-level internal lookup sample references are structurally valid when the target sample records exist in the same package and those local sample IDs are included in `ReplaceIds`.

## 8. App-Level Generation Implications

A future generated app such as **Department Access Management** can include:

- Departments data list
- Department Access Requests data list
- Department Access Request approval form
- Department lookup relationship
- Department Code additional-field mapping
- workflow `ContentList` persistence into Department Access Requests
- basic views/custom forms

IDs that can be generated together and remapped together:

- root app/listset ID
- child list IDs
- field IDs
- custom form layout IDs
- view layout IDs
- workflow/process model IDs
- workflow graph resource IDs
- report/page IDs if generated

IDs that should remain external unless bundled:

- user/group/position IDs
- external app/list IDs
- external document library IDs
- external AI agent/copilot IDs
- connection/credential IDs
- external file/template IDs

If Department Access Management packages Departments and Department Access Requests in the same `.yap`, the request list lookup to Departments can target the generated local Departments ListID. Both list IDs and fields can be included in `ReplaceIds`, and Yeeflow should remap the whole graph consistently on import.

## 9. App-Level Generation Pipeline

Recommended pipeline:

```text
Business requirement
-> app decomposition
-> resource model
-> data lists
-> relationships/lookups
-> approval forms
-> workflows
-> reports/dashboards if needed
-> AI resources if needed
-> dependency graph
-> decoded .yap draft
-> validate components
-> validate app graph
-> build .yap wrapper
-> sandbox import
-> export-back comparison
```

Component validators should still run:

- data lists through `validate-ydl-list.js`-equivalent checks
- approval forms through `validate-ywf-def.js`-equivalent checks
- app relationships through a new app graph validator

## 10. Proposed `.yap` Normalized Spec

```json
{
  "app": {
    "title": "",
    "description": "",
    "appId": "41",
    "listSetId": "",
    "mainListType": 1024,
    "icon": null,
    "navigation": [],
    "theme": {}
  },
  "resources": {
    "dataLists": [],
    "approvalForms": [],
    "reports": [],
    "dashboards": [],
    "agents": [],
    "copilots": [],
    "documentLibraries": []
  },
  "relationships": [],
  "replaceIds": [],
  "dependencies": [],
  "importAssumptions": []
}
```

Section definitions:

| Section | Meaning | `.yap` mapping |
| --- | --- | --- |
| `app` | Root app/listset identity, nav, theme | `Resource`, `Data.Item.ListModel`, root `Layouts`, `AppThemes` |
| `resources.dataLists` | Data list specs, fields, views, custom forms, optional samples | `Data.Childs[].Item` |
| `resources.approvalForms` | Approval form Defs and workflow metadata | `Data.Forms[]` with `WorkflowType: 2` |
| `resources.reports` | Data reports / form reports | `Data.DataReports[]`, `Data.FormReports[]`, `Data.FormNewReports[]`, report resources in `Childs` |
| `resources.dashboards` | App pages/dashboard layouts | root `Data.Item.Layouts[]` Type `103` |
| `resources.agents` | AI Agent resources | `Data.OtherModules[]` Type `Agents` |
| `resources.copilots` | Knowledge/copilot-like resources | `Data.OtherModules[]` Type `Knowledges` or future confirmed modules |
| `resources.documentLibraries` | Document storage/template libraries | child items with document-library resource type |
| `relationships` | Graph edges between resources | lookup rules, ContentList nodes, QueryData nodes, reports, dashboard refs |
| `replaceIds` | Local IDs remapped on import | `Resource.ReplaceIds` |
| `dependencies` | External IDs/resources not bundled | dependency map / validator input |
| `importAssumptions` | Sandbox/prod import expectations | report/handoff metadata |

## 11. Required Validators / Tools

| Tool | Purpose | Inputs | Outputs | Key checks |
| --- | --- | --- | --- | --- |
| `inspect-yap-package.js` improvements | Continue deep package inventory | `.yap` | JSON/Markdown | Resource modules, app pages, dashboards, reports, AI resources, secrets redaction |
| `extract-yap-metadata.js` improvements | Clean app graph metadata | `.yap` | metadata JSON/Markdown | lists, fields, forms, reports, document libraries, AI refs, relationships |
| `validate-yap-package.js` | Structural validation of decoded `.yap` | decoded app JSON or `.yap` | validation report | wrapper, Resource, Data keys, root app, child resources, forms, reports |
| `validate-yap-graph.js` | Cross-resource graph validation | decoded app JSON + optional dependency map | graph report | lookup targets, ContentList targets, reports, dashboard refs, AI/docs refs |
| `build-yap-wrapper.js` | Build gzip/base64 `.yap` wrapper | decoded app JSON | `.yap` + round-trip report | no placeholders, ReplaceIds policy, wrapper round-trip equality |
| `compare-yap-export-back.js` | Import/export learning | source decoded app + exported-back app | diff report | ID remaps, normalized fields, dropped resources, relationship integrity |
| `generate-yap-dependency-map.js` | Dependency planning | normalized spec + metadata | dependency JSON | unresolved external users/groups/positions/docs/AI/connections |

## 12. Stop Conditions

Stop before final `.yap` generation when:

- resource graph has unresolved IDs
- lookup targets are missing
- `ContentList` targets are missing
- `QueryData` targets are missing
- report source lists/fields are missing
- dashboards reference missing reports/lists/fields
- approval forms have invalid page registration or workflow graph metadata
- AI Agent/Copilot references are unresolved
- credential/connection resources are present or required
- token/secret-like resources would be copied or exposed
- document templates/libraries are unresolved
- user/group/position IDs are unresolved
- validators fail
- production import is requested without sandbox proof

## 13. First Future Generation Test Recommendation

Recommended first app-level generation test:

**Department Access Management**

Resources:

- Departments data list
- Department Access Requests data list
- Department Access Request approval form

Relationships:

- Department Access Requests.Department lookup -> Departments.Title
- Department Access approval form Department lookup -> Departments.Title
- lookup additional field Department Code -> DepartmentCode variable
- approval workflow `ContentList` -> Department Access Requests

Intentionally exclude at first:

- AI Agent/Copilot resources
- external connections
- document generation
- dashboards beyond simple app/list navigation
- advanced reports
- production user/position dependencies beyond already proven line-manager pattern

Why this is the right first test:

- It is small enough to validate completely.
- It exercises the key reason `.yap` matters: packaging related lists and approval form persistence together.
- It removes the staged `.ydl` import/export handoff for local lookup/list IDs.
- It reuses proven data-list custom form patterns and approval form patterns.
- It avoids sensitive modules and external connections.

## 14. Final Summary

Confidently understood:

- `.yap` uses the same gzip/base64 `Resource` wrapper as `.ydl`.
- `Data.Item` is the root app/listset.
- `Data.Childs[]` contains child resources such as data lists, document libraries, and report/list resources.
- `Data.Forms[]` contains approval forms, process/list workflows, and scheduled/job workflows.
- Approval forms inside `.yap` contain the same decoded Def patterns learned from `.ywf`.
- Data lists inside `.yap` contain the same list model/fields/layout patterns learned from `.ydl`.
- `ReplaceIds` includes local app/list/field/layout/internal IDs and enables import-time remapping.
- `.yap` is the right packaging level for multi-component generated apps because related resources can be remapped together.

Needs more export examples:

- clean app packages with multiple generated lists and no legacy modules
- dashboard-only apps
- apps with data reports generated from scratch
- apps with form reports generated from scratch
- apps with document libraries but no external templates
- AI Agent/Copilot packaging where sensitive resources are safe to inspect
- apps with permissions/groups intentionally configured

Should not be guessed:

- production ListSetID/ListID/FieldID values
- user/group/position IDs
- external document template/library IDs
- AI agent/copilot IDs
- connection/credential IDs
- report internals not validated by export-back comparison
- dashboard component source references

Recommended next tool to build first:

`validate-yap-package.js`, followed by `validate-yap-graph.js`.

The first tool should confirm the structural package shape. The second should confirm cross-resource relationships. Together they create the safety gate needed before attempting `build-yap-wrapper.js`.

## 15. Generated v5 App Baseline Addendum

The first successful generated app-level `.yap` baseline is `Department Access Management_v5`.

It proves that a generated package can import and open when the root app shell is complete:

- top-level wrapper title, description, and non-null `IconUrl`
- root `Data.Item.ListModel.Type = 1024`
- root `CustomType = ""`
- root `Perm = 0`
- root `WorkspaceID` present
- root `LayoutView` navigation populated
- root Type `103` app page layout present
- `AppTags`, `AppMetadatas`, and `AppComponents` arrays present
- `AppThemes` present and non-empty
- root `CreatedBy` and `ModifiedBy` populated

The Type `103` app page rule is now confirmed:

- the page `LayoutID` is local and included in `ReplaceIds`
- the embedded page resource `LayoutInResources[0].ID` and `RefId` are separate IDs
- those embedded resource IDs are excluded from `ReplaceIds`

The v5 app imported, opened, rendered navigation, opened both child lists, and opened the approval request form. Export-back then confirmed generated request sample rows store plain target Department `ListDataID` strings and Yeeflow remapped those values to the exported-back Department records. If a lookup grid cell appears blank despite exported-back values matching target records, classify it as a runtime display/index/cache issue unless item-form lookup values or manually edited rows also fail.

## 16. Fresh ID Family Policy For Generated App Tests

The successful Visitor Access Management v5 fresh-compatible test added one more app-level generation rule.

Observed behavior:

- A v5-compatible Visitor package that reused the old `206...` generated ID family and `DAR` approval form key imported but opened as a blank app.
- A fresh-compatible Visitor package with a new local ID family and fresh approval form key `VAV` imported and opened successfully.

Generation rule:

- Treat each generated `.yap` app test as a new local ID namespace.
- Generate a fresh root ListSetID/app ID family.
- Generate fresh child list IDs, field IDs, layout IDs, sample record IDs, approval form/process IDs, page IDs, and workflow node IDs.
- Generate a fresh FlowKey/form key for each approval form in the package.
- Include local generated IDs in `ReplaceIds` according to the normal app-level rules.
- Do not reuse generated IDs or approval form keys from an app that has already been imported into the sandbox, unless the explicit goal is to study update/merge behavior.

Runtime implication:

- ID/key reuse can pass local structural and graph validation.
- ID/key reuse can also import successfully.
- The failure may appear only after opening the app, as a blank app or detached components.

Validator recommendation:

- When detectable, validators should warn or fail if a generated app reuses an ID family or form key observed in prior generated/imported test artifacts.
- Validation output should summarize generated ID families and form keys to make accidental reuse visible.

## 17. App-Level Approval Form Registration Rule

The Visitor Access Management v6 one-field expansion exposed a required approval form registration rule inside app-level `.yap` packages.

Working app-level approval form rows use:

```json
{
  "Key": "VAW",
  "ListID": 0,
  "ProcModelID": "2110030000000000001"
}
```

Rule:

- `Data.Forms[].ListID` must be `0` for app-level approval forms.
- The approval form/process identity belongs in `Data.Forms[].ProcModelID`.
- Root navigation Type `105` should point to the form key, not to `ProcModelID`.
- `ProcModelID` should remain a local generated ID and should be included in `ReplaceIds`.

Failure mode:

- If `Data.Forms[].ListID` is set to `ProcModelID`, Yeeflow can import/open the app and show data lists, but the approval form may be missing from navigation.

The corrected v6.1 package used:

- fresh ID family `211...`
- fresh form key `VAW`
- `Data.Forms[].ListID = 0`
- `Visitor Name` added as `Visitor Access Requests.Text8`

It imported, opened, rendered navigation, displayed the approval form, and showed `Visitor Name` on the request page.

## 18. Visitor Access v7 Expansion Evidence

The Visitor Access Management v7 package confirmed that incremental one-field expansion is safe when all app/list/form/workflow references are updated together.

v7 added:

- `Visitor Company`
- data-list field slot `Visitor Access Requests.Text9`
- workflow variable `VisitorCompany`
- custom list form control
- approval request page control
- approval page readonly control
- sample data values
- `ContentList` mapping `VisitorCompany -> Text9`

Confirmed successful package settings:

- fresh ID family `212...`
- fresh form key `VAX`
- app-level approval form `Data.Forms[].ListID = 0`
- approval form `ProcModelID = 2120030000000000001`
- request list `ListID = 2120020000000001000`
- `Visitor Company` field ID `2120020000000001013`

Confirmed runtime result:

- app imported and opened
- app navigation rendered
- approval form appeared
- `Visitor Name` and `Visitor Company` rendered
- `ContentList` persisted `VisitorCompany` to `Text9`

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

- Start from the latest known-good baseline.
- Add one text field/change at a time.
- Use a fresh ID family and fresh FlowKey for every import-test package.
- Preserve proven field slots.
- Update data-list field definitions, custom forms, approval pages, workflow variables, `ContentList` mappings, views, and sample rows consistently.

## 19. Visitor Access v8-v10 Expansion Evidence

The Visitor Access Management v8, v9, and v10 packages extended the v7 baseline one field at a time and confirmed the generated app expansion pattern through `Text12`.

Successful expansion chain:

| Version | Added field | Slot | ID family | FlowKey | Confirmed result |
| --- | --- | --- | --- | --- | --- |
| v6.1 | Visitor Name | `Text8` | `211...` | `VAW` | Approval form rendered the field. |
| v7 | Visitor Company | `Text9` | `212...` | `VAX` | Field rendered and persisted. |
| v8 | Access Area | `Text10` | `213...` | `VAY` | Field rendered and persisted. |
| v9 | Host Employee | `Text11` | `214...` | `VAZ` | Field rendered and persisted. |
| v10 | Visitor Contact | `Text12` | `215...` | `VBA` | Field rendered and persisted. |

v10 specifically proved:

- `Visitor Contact` can be added as a simple single-line text field in `Visitor Access Requests.Text12`.
- A fresh `215...` ID family avoids collisions with prior imported generated apps.
- A fresh FlowKey `VBA` avoids approval-form registration/key reuse issues.
- `Data.Forms[].ListID = 0` remains required for app-level approval forms.
- The new field must be added consistently to the data-list `Defs`, list views, custom list form, approval request page, approval readonly page, workflow variable list, `ContentList` mapping, and sample rows.

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

- Expand from a known-good `.yap` baseline one change at a time.
- Use a fresh ID family and fresh FlowKey/form key for every import-test package.
- Keep app-level approval form `Data.Forms[].ListID = 0`.
- Preserve proven slots unless intentionally testing a new field type or slot.
- Validate package, graph, and approval form before wrapper build.

Recommended next tests:

- Convert `Access Area` from text to choice/dropdown.
- Test `Host Employee` as a user picker or lookup.
- Add dashboard/report resources only after the core app remains stable.

## 20. Manual v10 Multi-Type Field Export Evidence

The manually updated export `/Users/rengerhu/Downloads/Visitor Access Management v10.yap` confirms additional approval-form and data-list field/control patterns inside an app-level `.yap`.

Detailed study:

- `docs/collection-01.md#approval-form-and-yap-field-type-pattern-study`

Confirmed structures:

| Pattern | Approval form | Data list |
| --- | --- | --- |
| Number | variable `type = "number"`; control `type = "input_number"` | `FieldType = "Decimal"`; `Type = "input_number"` |
| Radio single select | variable `type = "text"`; control `type = "radio"` | text/radio-compatible field if persistence is needed |
| Dropdown single select | `type = "radio"` plus `attrs.displayStyle = "dropdown"` | text/radio-compatible field if persistence is needed |
| Switch | variable `type = "boolean"`; control `type = "switch"` | `FieldType = "Bit"`; `Type = "switch"` |
| Dynamic display | target control `attrs.control_display[]` | not applicable |

Important observations:

- Approval number defaults are stored as control `value`, while formatting/min settings are in `attrs`.
- Dropdown choices can include `attrs.color_choices[]` with UUID keys matching `attrs.choices[]` values.
- Switch default on the approval form is boolean `false`; list sample values use string `"0"`/`"1"`.
- The dynamic display condition for `Escort User` used `RequiresEscort == true` and a show action.
- The manual export did not map the new fields in `ContentList`; generated packages must add mappings explicitly when persistence is required.

Generator rule:

- For a multi-type generated app expansion, update data-list field definitions, approval variables, request controls, readonly approval controls, custom list form controls where needed, sample raw values, and `ContentList` mappings as one consistent type-compatible set.
- Stop before build if any field type has no confirmed pattern or no compatible target field.

## 21. Generated v11 Multi-Field, Multi-Type Evidence

`visitor-access-management.v11-five-fields-multitype.yap` confirmed that the manual v10 field-type patterns can be generated successfully inside an app-level `.yap`.

Successful package settings:

- Fresh ID family: `216...`
- Fresh FlowKey/form key: `VBB`
- App-level approval form `Data.Forms[].ListID = 0`
- Two child data lists
- One approval form
- `ReplaceIds` count: `46`
- Package validation: pass
- Graph validation: pass
- Extracted approval form validation: pass
- Wrapper round-trip validation: pass
- Runtime import/open/form test: pass

v11 added five persisted fields:

| Business field | Data-list slot | Data-list type | Approval variable/control |
| --- | --- | --- | --- |
| Visitor Email | `Text13` | `Text` / `input` | `text` / `input` |
| Visitor Phone | `Text14` | `Text` / `input` | `text` / `input` |
| Number of Visitors | `Decimal1` | `Decimal` / `input_number` | `number` / `input_number` |
| Access Type | `Text15` | `Text` / `radio` | `text` / `radio` with `displayStyle = "dropdown"` |
| Requires Escort | `Bit1` | `Bit` / `switch` | `boolean` / `switch` |

Proven `ContentList` mappings:

| Source variable | Target field |
| --- | --- |
| `VisitorEmail` | `Text13` |
| `VisitorPhone` | `Text14` |
| `NumberofVisitors` | `Decimal1` |
| `AccessType` | `Text15` |
| `RequiresEscort` | `Bit1` |

Sample value shapes confirmed for generated packages:

- Decimal: numeric values.
- Choice/dropdown: selected option text.
- Bit/switch in list sample data: string `"1"` or `"0"`.
- Boolean approval control: boolean `true` or `false`.

Conditional display:

- `Escort User` was generated as an approval-form-only text field.
- It is shown when `RequiresEscort == true`.
- The rule is stored on the `Escort User` target control in `attrs.control_display[]`.
- It was intentionally not persisted in v11 because no storage target field was added.

Generator implication:

- Multi-field expansion is now allowed after underlying field/control types are proven.
- A fresh ID family and fresh FlowKey/form key remain mandatory for sandbox import tests.
- `Data.Forms[].ListID = 0` remains mandatory for app-level approval forms.
- Each persisted field must be added consistently to the data list, custom list form, approval request page, approval readonly page, workflow variables, sample rows, and `ContentList` mapping.

Remaining gaps:

- Persisted conditional text field, such as `EscortUser -> Text16`.
- Multi-select choice.
- User picker.
- Lookup inside a line-item table.
- Dashboards and reports beyond the Type `103` root page.
