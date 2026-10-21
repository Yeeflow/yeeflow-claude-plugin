# docs/standards collection 2

This file combines 21 documents from `docs/standards/` for the Claude directory build. A reference to `docs/standards/<name>.md` is the section that starts at the anchor `#<name>` below.

- [dashboard-summary-card-generation-standard.md](#dashboard-summary-card-generation-standard)
- [data-list-custom-form-layoutview-runtime-source-standard.md](#data-list-custom-form-layoutview-runtime-source-standard)
- [data-list-generation-standard.md](#data-list-generation-standard)
- [data-list-view-filter-standard.md](#data-list-view-filter-standard)
- [form-action-open-resource-golden-reference-standard.md](#form-action-open-resource-golden-reference-standard)
- [form-action-print-page-barcode-scan-golden-reference-standard.md](#form-action-print-page-barcode-scan-golden-reference-standard)
- [form-action-query-data-golden-reference-standard.md](#form-action-query-data-golden-reference-standard)
- [form-action-set-data-list-golden-reference-standard.md](#form-action-set-data-list-golden-reference-standard)
- [form-control-type-schema-authority-standard.md](#form-control-type-schema-authority-standard)
- [functional-specification-standard-template.md](#functional-specification-standard-template)
- [multi-column-form-workspace-standard.md](#multi-column-form-workspace-standard)
- [native-collection-density-standard.md](#native-collection-density-standard)
- [native-collection-rich-content-training.md](#native-collection-rich-content-training)
- [public-form-form-action-golden-reference-standard.md](#public-form-form-action-golden-reference-standard)
- [service-portal-generation-standard.md](#service-portal-generation-standard)
- [set-variable-golden-reference-standard.md](#set-variable-golden-reference-standard)
- [standalone-export-shared-generation-standard.md](#standalone-export-shared-generation-standard)
- [tab-gantt-control-golden-reference-standard.md](#tab-gantt-control-golden-reference-standard)
- [three-column-workspace-layout-standard.md](#three-column-workspace-layout-standard)
- [ui-summary-kpi-runtime-hard-gates.md](#ui-summary-kpi-runtime-hard-gates)
- [workflow-assignee-expression-serialization-standard.md](#workflow-assignee-expression-serialization-standard)

---

<a id="dashboard-summary-card-generation-standard"></a>

<!-- source: docs/standards/collection-02.md#dashboard-summary-card-generation-standard -->

# Dashboard Summary Card Generation Standard

Official v0.6.19 adds a stricter runtime-binding companion standard in `docs/standards/collection-01.md#dashboard-runtime-binding-standard`. Use this file for the Summary card host/control scope, and use the runtime-binding standard to verify `attrs.data.list`, aggregate field binding, matching layout-resource `Resource.exts[]`, `settings.values[]`, filter consumption, lookup filter record-id behavior, and layout-resource `Resource.ReportIds[]` registration.

The UI/Summary/KPI hard gates in `docs/standards/collection-02.md#ui-summary-kpi-runtime-hard-gates` are now required for generated-final dashboard claims. Summary/KPI generation has three separate proof states: designer-configured Summary control, validator-valid Summary contract, and runtime-proven visible dynamic KPI rendering. Do not collapse these into one claim.

## Purpose

This standard defines how Yeeflow application generators must create KPI and summary cards on Yeeflow surfaces that support the Summary control. It converts the golden reference pattern from the Service Desk Pro Executive Dashboard into a required generation rule.

The goal is to avoid static sample KPI values such as `128`, `24`, or `7` in generated applications. Visible KPI cards must display actual calculated data from the source data list while preserving full visual-design control over typography, layout, colors, and spacing.

## Scope

The Summary control is available only on these generated surfaces:

- Dashboard pages.
- Data List custom forms, including custom view/new/edit/print layouts when the export-proven host supports Summary.

This standard does not apply to, and must not generate Summary controls on:

- Approval forms.
- Data List public forms.
- Other form/page hosts where Summary controls are not export-proven or product-supported.

For unsupported surfaces, generators must not attempt to satisfy this standard by inserting Summary controls. Use a supported alternative display pattern, mark the metric as deferred if it cannot be calculated safely, and do not claim Summary-card pattern compliance for that surface.

## Data Analytics Control Inventory

Summary is one Yeeflow Data Analytics control. The broader Data Analytics inventory also includes:

- Pie chart
- Column chart
- Line chart
- Gauge
- Funnel chart
- Color block heatmap
- Pivot table

All generated Data Analytics controls require runtime-safe IDs. Use UUID-based control IDs unless an export-proven Yeeflow sample proves another ID shape is valid for that exact non-Summary control type. Generated Summary controls must always use UUID IDs; semantic/layout-derived Summary IDs are not runtime-proven for KPI temp-variable writeback. Preserve existing analytics control IDs during upgrades; only newly added analytics controls get newly generated UUID/API-issued IDs. Summary has a proven UUID runtime shape through KPI Runtime Binding Proof v1.0.1. Other analytics controls require export-proven control shapes before a generator can claim runtime correctness, and runtime screenshot/evidence is required before claiming analytics dashboard quality.

## Golden Reference

Use the Service Desk Pro `Executive Dashboard` dashboard as the best-practice reference for summary cards:

- Summary controls calculate the real values.
- Summary controls save their values into dashboard temporary variables.
- A hidden container keeps the Summary controls out of the visual UI.
- Styled Text controls display the temporary variable values inside KPI cards.
- The visible KPI card design remains independent from the Summary control's default rendering.

## Required Pattern

For every dashboard or Data List custom-form KPI or summary card that represents a count, sum, average, min, max, rate, or status total:

1. Create one Summary control for each metric.
2. Bind each Summary control to the correct data list and field/filter.
3. Configure each Summary control to save its calculated value into a page/form temporary variable supported by the host surface.
4. Place all Summary controls inside a dedicated hidden container.
5. Hide that container with a dynamic display rule that is always false, such as `1 == 0`.
6. Create a visible KPI card using Container, Grid, and Text controls.
7. Bind the visible numeric Text control to the saved temporary variable.
8. Style the visible card manually for spacing, typography, color, borders, and responsive layout.
9. Do not use the Summary control's default visual output as the final KPI card UI.
10. Do not use static sample numbers for final generated KPI values.

## Required Variables

Each generated dashboard or supported Data List custom form with summary cards must define named temporary variables for the metric values.

Use clear names based on the business meaning:

```text
totalVendors
pendingOnboardingCount
highRiskVendorCount
expiringDocumentCount
openTicketCount
monthlyRevenueTotal
averageResolutionHours
```

Avoid generic variable names:

```text
value1
summary1
count
temp
number
```

## Hidden Summary Container Rules

Every supported surface that uses KPI summary cards must include a hidden Summary source container.

Required container naming:

```text
Hidden Summary Data Sources
```

Required behavior:

- The container must contain only Summary controls and small helper controls needed for calculation.
- The container must not appear in the user-facing dashboard.
- The container must use `attrs.common.hide = [null, true, true, true]` so it is hidden on desktop, tablet, and mobile.
- The container must use `attrs.style.direction = [null, "row"]`.
- The display rule must be intentionally impossible, for example `1 == 0`.
- The hidden state must not rely on styling tricks alone when a dynamic display rule is available.

Required Summary control naming:

```text
Summary - Total Vendors
Summary - Pending Onboarding
Summary - High Risk Vendors
Summary - Expiring Documents
```

## Visible KPI Card Rules

The visible cards must be designed as normal dashboard UI, not as raw Summary controls.

Required structure:

```text
Lifecycle KPI Section
  KPI Card Row
    KPI Card - Total Vendors
      KPI Label - Total Vendors
      KPI Value - Total Vendors
      KPI Description - Total Vendors
    KPI Card - Pending Onboarding
      KPI Label - Pending Onboarding
      KPI Value - Pending Onboarding
      KPI Description - Pending Onboarding
```

Required visible controls:

- Container or Grid for the card row.
- One card-like Container per KPI.
- Text control for KPI label.
- Text control for KPI value.
- Text control for KPI description or trend note.

Required style rules:

- Card row must use a grid or flex row layout.
- Cards must have meaningful padding.
- Cards must have a border, background, or visual grouping.
- Card gaps must be configured intentionally.
- KPI value must have stronger hierarchy than label and description.
- KPI cards must not be plain text blocks directly on the dashboard root.

## Data Binding Rules

Visible KPI value Text controls must be bound to temporary variables populated by Summary controls.

Required binding flow:

```text
Data List -> Summary control -> save value to temp variable -> visible Text control
```

Forbidden binding flow:

```text
Static Text "128"
Static Text "24"
Static Text "7"
Static Text "13"
```

Static placeholder values may only be used in early internal drafts. They must be replaced before a package is marked ready, successful, full UI, or quality-proofed.

## Summary Control Configuration Rules

Each Summary control must define:

- UUID control ID, never a semantic/layout-derived ID such as `<LayoutID>_summary`.
- Source data list.
- Metric type, such as count, sum, average, min, or max.
- Target field when the metric requires a field.
- Designer-shaped field metadata in `attrs.data.field`, `attrs.field`, `fieldObject`, and `fieldInfo`.
- Count field shape with `ListDataID` where required.
- Filter condition when the metric represents a subset.
- Target dashboard/form temporary variable.
- `save_var` as a designer-exported expression object, not a plain string.
- Clear control name in Navigator.
- Layout-resource `Resource.ReportIds` coverage for the Summary control ID. Top-level `Pages[].ReportIds` is optional compatibility metadata only and is not the primary generated dashboard Summary registration path.
- Matching layout-resource `Resource.exts[]` entry where both `i` and `id` equal the Summary UUID, `category = "___Pivot___"`, `key = "summary"`, and `attr` includes `AppID`, `ListSetID`, `ListID`, `list`, `source`, and export-shaped `settings.values[]`.
- No `runtimeModelProven: true` claim unless the UUID Summary shape and runtime metadata above are present.

Examples:

```text
Summary - Total Vendors
Source: Vendors
Metric: Count all records
Filter: none
Save value to: totalVendors
```

```text
Summary - High Risk Vendors
Source: Vendors
Metric: Count records
Filter: Risk Level is High or Critical
Save value to: highRiskVendorCount
```

```text
Summary - Expiring Documents
Source: Vendor Documents
Metric: Count records
Filter: Expiry Date within next 30 days
Save value to: expiringDocumentCount
```

## Formatting Rules

Visible Text controls may format the variable value for presentation.

Allowed formatting:

- Number with thousand separators.
- Currency symbol and decimal precision.
- Percent sign.
- Prefix or suffix text.
- Empty-state fallback such as `0`.

Examples:

```text
{{totalVendors}}
${{monthlyRevenueTotal}}
{{onboardingCompletionRate}}%
{{averageResolutionHours}} hrs
```

If the platform does not support inline formatting in the Text control, the generator must document the limitation and use the safest supported field/variable display pattern.

Dynamic visible KPI binding is proven only for the exact UUID Summary v1.0.1 shape described below, unless another focused runtime proof proves a different shape. If the dynamic binding shape is not runtime-proven, generation must stop or use an explicitly labeled fallback value and record the gap. A fallback value is a visual fallback, not runtime proof of dynamic KPI rendering.

### UUID Summary v1.0.1 Runtime-Proven Binding Shape

KPI Runtime Binding Proof v1.0.1 proved dynamic visible KPI binding for one exact dashboard shape:

- Summary control IDs are UUIDs.
- Each Summary UUID appears in `Resource.ReportIds[]`.
- Each Summary UUID has a matching `Resource.exts[]` entry with `i` equal to the Summary ID, `category: "___Pivot___"`, and `key: "summary"`.
- Each Summary saves to a dashboard temp variable with a designer-shaped `attrs.save_var` expression object.
- `Resource.tempVars[]` declares the same temp variable ids/names.
- Visible KPI Heading/Text controls bind through `attrs.headc.title.variable[]`.
- The proof does not use static or formatted fallback values.
- Summary field metadata is complete in `attrs.data.field`, `attrs.field`, `fieldObject`, and `fieldInfo`.

### Temp variable identity and KPI binding

Use two distinct identifiers for the same Dashboard temp variable:

- Declaration: `tempVars[].id = tempVars[].name = "var_<page>_<metric>"`. Never begin either declaration value with `__temp_`.
- Summary write: `attrs.save_var = { exprType: "variable", type: "expr", id: "__temp_var_<page>_<metric>", name: "var_<page>_<metric>" }`.
- Visible KPI Heading/Text read: `attrs.headc.title.variable[]` uses the same pair: `id = "__temp_" + declared id`, `name = declared id`.

`__temp_` is a runtime expression prefix, not part of the stored variable name. Do not write `__temp_` in `tempVars[].id` or `tempVars[].name`; do not double-prefix a reference as `__temp___temp_*`; and do not use the `{ prefix, value }` serialization for a KPI Summary/Text binding unless that specific control property is separately export-proven. For custom-code or other property families that do use `{ prefix: "__temp_", value: "<declared-id>" }`, keep `value` as the unprefixed declared ID.

Before save, verify each Summary write and each visible KPI Text/Heading reference resolves to exactly one same-page temp declaration. After `component_save`, reread the persisted Resource and report this structural closure separately from Designer-open and browser value-refresh evidence.

Runtime proof must include before/after source data mutation evidence, expected-value notes, inspector output, and refreshed/recalculated after-evidence. The v1.0.1 proof changed the KPI values from `3 / 600 / 2 / 300` to `4 / 1000 / 3 / 700`. Because Summary recalculation can be asynchronous or cache-delayed, stale after-evidence that still shows before values is not proof.

When generated apps use seed/demo rows for runtime verification, KPI proof must also include seed-derived expected values and compare them with the visible runtime KPI text. A KPI that is merely bound to a Summary control is package-valid, but not business-semantically proven until the visible value matches the expected count/sum/rate derived from the seeded records.

Do not generalize this proof to semantic/non-UUID Summary IDs, approval forms, public forms, unsupported surfaces, or other visible binding shapes. If a generated package does not use the exact proven UUID Summary shape and does not provide before/after mutation proof, visible KPI binding remains unproven and fallback KPI values must be explicitly labeled fallback. Marketing Event dashboards may use this shape, but must still run their own before/after mutation proof before claiming runtime dynamic KPI success.

## Sample Data Rule

Generated applications may include sample data to make summary cards visible during testing.

However:

- The KPI card must still be data-bound.
- The sample number must come from sample records through Summary controls.
- The visible KPI value must not be typed manually.

Sample records are acceptable. Static sample KPI text is not acceptable.

## Control Naming Rules

Every control used in a summary-card section must be renamed in Navigator.

Required names:

- `Lifecycle KPI Section`
- `Hidden Summary Data Sources`
- `KPI Card Row`
- `KPI Card - <Metric Name>`
- `KPI Label - <Metric Name>`
- `KPI Value - <Metric Name>`
- `KPI Description - <Metric Name>`
- `Summary - <Metric Name>`

Forbidden default names:

- `Container`
- `Grid`
- `Text`
- `Summary`
- `Button`

Default names are allowed only during temporary local experimentation and must be fixed before handoff.

## Validator Requirements

Application quality validation must fail a generated dashboard or supported Data List custom form when:

- KPI values are static Text controls.
- A summary card has no Summary control source.
- A Summary control does not save to a temp variable.
- A visible KPI value is not bound to a temp variable or summary result.
- Hidden Summary controls are visible in the UI.
- KPI card controls still use default Navigator names.
- KPI cards are plain text blocks without card/grid/container structure.
- A generated application claims full UI quality while KPI cards show hardcoded sample values.
- A Summary control is generated on an unsupported surface such as an approval form or Data List public form.

Suggested hard errors:

```text
SUMMARY_CARD_STANDARD_SCOPE_INVALID
SUMMARY_CONTROL_UNSUPPORTED_SURFACE
SUMMARY_CARD_STATIC_VALUE
SUMMARY_CONTROL_MISSING
SUMMARY_CONTROL_SAVE_VAR_MISSING
SUMMARY_TEMP_VAR_MISSING
SUMMARY_VALUE_BINDING_MISSING
SUMMARY_SOURCE_CONTAINER_VISIBLE
KPI_CARD_STRUCTURE_MISSING
KPI_CONTROL_DEFAULT_NAME
```

## Generator Checklist

Before returning a generated package, check every dashboard or supported Data List custom-form summary section:

- [ ] The summary section is on a Dashboard page or supported Data List custom form.
- [ ] No Summary control is present on approval forms, Data List public forms, or unsupported hosts.
- [ ] Each KPI metric has a matching Summary control.
- [ ] Each Summary control has a real source data list.
- [ ] Each Summary control has the correct metric and filter.
- [ ] Each Summary control saves to a meaningful temp variable.
- [ ] Hidden Summary source container is present and hidden with an impossible display rule.
- [ ] Visible KPI cards are separate styled containers.
- [ ] Visible KPI values bind to temp variables.
- [ ] No final KPI value is static sample text.
- [ ] All controls have meaningful Navigator names.
- [ ] Sample records, if included, drive the KPI values through real summaries.

## Proof Boundary

This standard is based on the Service Desk Pro Executive Dashboard golden reference, the Vendor Onboarding v4.1 hard checks, and the KPI Runtime Binding Proof v1.0.1 UUID Summary runtime proof. The UUID Summary proof narrows dynamic visible KPI success to that exact package/runtime shape; it does not prove every Summary/KPI configuration. This standard should be treated as the default generation rule for dashboard and supported Data List custom-form summary cards in Yeeflow App Builder and Yeeflow App Builder.

If a generator cannot safely implement the Summary-control-to-temp-variable pattern for a specific supported surface, it must:

1. Do not silently fall back to static KPI text.
2. Document the unsupported part.
3. Mark the summary card as deferred or use a clearly labeled non-final fallback.
4. Do not call the package complete or full UI quality-proofed until the issue is resolved.

---

<a id="data-list-custom-form-layoutview-runtime-source-standard"></a>

<!-- source: docs/standards/collection-02.md#data-list-custom-form-layoutview-runtime-source-standard -->

# Data List Custom Form LayoutView Runtime Source Standard

## Purpose

Generated Type `1` custom Data List forms must load in Yeeflow runtime and Designer without a manual repair. Runtime and Designer read the custom form JSON from `Layouts[].LayoutView`, while export/package surfaces also carry the same JSON in `Layouts[].LayoutInResources[0].Resource`.

## Required Contract

For every generated Type `1` custom form layout on a Data List or Document Library:

- `Layout.LayoutView` must be a JSON string containing the complete rendered form resource.
- `Layout.LayoutInResources[0].Resource` must be a JSON string containing the same complete rendered form resource.
- `Layout.LayoutView` and `LayoutInResources[0].Resource` must be semantically equivalent after stable JSON normalization.
- The rendered form resource must contain the actual form tree, including `children`, the selected Data List Form Layouts v1.1 template provenance, and business field controls.
- New/Edit forms must use `data_list_form_layout_new_edit_v1_1`.
- Standard View forms must use `data_list_form_layout_view_item_v1_1`. Full-page Workbench View forms must use `data_list_form_layout_workbench` and `ListModel.LayoutView.opentype.view` must open them as Full page.

## Forbidden Patterns

The following are signing-readiness blockers:

- `Layout.LayoutView` is `null`, empty, omitted, malformed, or a placeholder.
- `Layout.LayoutView` contains a minimal handoff object such as `source: "minimal-resource-graph"`.
- `Layout.LayoutView` and `LayoutInResources[0].Resource` drift.
- `LayoutInResources[0].Resource` is complete but `Layout.LayoutView` is only a shell.
- The generated package relies on Designer publish to repair an incomplete `LayoutView`.

## Proof Boundary

Package generation, signing, API upgrade acceptance, and Version Management submission do not prove custom form runtime loading. Generated-final validation must catch incomplete custom form runtime sources before signing. Browser proof remains a separate stage after install or upgrade.

---

<a id="data-list-generation-standard"></a>

<!-- source: docs/standards/collection-02.md#data-list-generation-standard -->

# Data List Generation Standard

## Purpose

This standard defines the minimum quality rules for generated Yeeflow data lists in full application packages. It converts repeated runtime findings from generated applications, especially the Vendor Onboarding application, into hard generation and validation requirements.

Generated data lists must not be technically present but unusable. A generated data list is not complete until its fields, default view, choice options, lookup relationships, lookup display fields, and sample data are usable in the runtime UI.

## Golden Reference Cases

Use these prior proven patterns as references:

- Vendor Onboarding v4.1: `Vendors` is the master list. `Vendor Documents`, `Compliance Reviews`, and `Vendor Activity / History` depend on `Vendors` through lookup fields.
- Department Access Management: `Departments` is created before dependent request/employee lists, and dependent sample rows reference the target list sample record IDs.
- Related data-list staged lookup pattern: reference lists must exist before dependent lookup sample data is generated.

## Required Data List Quality Rules

Every generated data list must include:

1. Valid list schema.
2. Native `Title` field preserved.
3. Unique `DisplayName`, `FieldName`, and `InternalName` values.
4. Correct `FieldName` suffix and `FieldIndex` alignment.
5. Default data view with meaningful display fields.
6. Populated options for every choice field.
7. Complete lookup field rules for every cross-list relationship.
8. Sample data when the application is intended for runtime testing.
9. Custom New/Edit/View forms assigned through list display settings; generated business lists must not use default form routing.
10. Export-shaped default view metadata that materializes columns in the runtime UI.

When the App Plan supplies an explicit schema-safe internal field key, the generated field must preserve it. Do not infer or renumber over planned keys such as `Text5`. A planned `Status | Text5` field must materialize as `FieldName = Text5`. Current generated-final YAPK schema stores identity-picker controls on Text-backed storage fields, so planned user/person fields such as `Requester` or `Assigned Agent` must preserve identity-picker control behavior with schema-safe `Text*` field keys rather than regressing to ordinary text inputs. Placeholder filtering for resource names must not be applied to field display names; `Status`, `State`, `Stage`, `Priority`, `Requester`, `Assigned Agent`, and similar domain labels are real fields unless the App Plan explicitly marks them as not planned.

App Plans may define all Data List fields in one consolidated schema table instead of one heading per list. The generator must treat tables with columns such as `List`, `Field label`, `Internal field`, `Field type`, and `Purpose` as authoritative data-list schemas. Each row must be grouped by the `List` value and materialized into that list's `Childs[].Fields`; headings such as `Data List Schema Table` are not resource names. The generator must preserve explicit `Internal field` values, infer Yeeflow storage from `Field type`, and infer select options from the `Purpose` column when it lists business values such as `Low, Medium, High, Critical` or `Open, In Progress, Resolved, Closed`. Lookup fields described by purpose text such as `Lookup to Tickets` must emit complete lookup runtime rules with the target list ID and a valid display field. Text identifiers such as `Ticket Number` must remain Text/input fields unless the App Plan explicitly marks the field as Decimal/Number.

Field-table parsing must also tolerate legacy planning headings that appear in real generated plans. `Display Name` is an alias for `Field label`, `Storage Name` is an alias for `Internal field` / `Field name`, and `Control` may be used when `Field type` is absent. In that case, infer schema-safe storage from the control shape: `input_number`/number/currency/amount controls map to Decimal, date/datetime controls map to Datetime, switch/checkbox/boolean controls map to Bit, and text/select/identity/lookup/file/image controls remain Text-backed unless an explicit field type says otherwise. Recognizable field tables must never silently collapse a business data list to the native `Title` field only; unsupported headings should fail planning/materialization validation before package handoff. When a planned Data View references non-Title columns, query/search fields, or fixed filters, the host Data List must have a parseable field table; if not, stop with `DATA_LIST_FIELD_TABLE_REQUIRED_FOR_PLANNED_VIEW` rather than generating a Title-only default list.

## Custom Data List Form Layout Templates

Every generated business Data List or Document Library must have custom Data List forms assigned for New Item, Edit Item, and View Item. Default layouts are not signing-ready for generated business lists.

Every generated custom Data List form must explicitly select and materialize a Data List Form Layouts v1.1 page template. This is a hard plan-to-resource contract: New, Edit, View, Detail, Print, and other custom form rows must identify their page role and selected template in the App Plan, then serialize the matching `dataListFormLayoutTemplateId` and `derivedFromDataListFormLayoutTemplate` marker on the cloned resource. The generator must not infer a template from the form title, fabricate a generic Container root, silently add required forms, or fall back to a default page layout.

Approved page-level templates:

- `data_list_form_layout_new_edit_v1_1` for New Item and Edit Item forms
- `data_list_form_layout_view_item_v1_1` for standard View Item forms
- `data_list_form_layout_workbench` for full-page Workbench View Item forms

The source registry is `docs/reference/data-list-form-layout-templates.json`.

Generators must clone the selected export-shaped template first and then place business-specific fields, actions, analytics, or related data only inside approved business-content slots. They must not generate title-only, flat, or ad hoc custom Data List form layouts.

`ListModel.LayoutView.add`, `ListModel.LayoutView.edit`, and `ListModel.LayoutView.view` must each point to a Type `1` custom form layout owned by the same list. The literal value `default`, missing display settings, unresolved layout IDs, or Type `0` data views used as form routes must fail generated-final validation. System/support lists may skip this requirement only with an explicit App Plan exemption and generated package policy marker.

For the standard routes, New and Edit must select `data_list_form_layout_new_edit_v1_1`; standard View must select `data_list_form_layout_view_item_v1_1`; a full-page View workbench must select `data_list_form_layout_workbench` and declare `Open in: Full page`. A custom Detail or Print page must explicitly use the compatible View/Workbench family; no unclassified custom form can be materialized.

Every generated Data List-bound control in a custom form or Dashboard must retain the complete source identity in `attrs.data.list`: `AppID`, `ListSetID`, and `ListID`. Do not rely on a persisted field mapping or `ListID` alone. Runtime may render with an incomplete source object, but the Designer cannot reliably rediscover compatible fields when `ListSetID` is absent. The final materialization pass must preserve explicit source identities and complete missing same-app identities before the resource is serialized.

Current-record Data List fields inside generated New/Edit/View custom forms must use the field-layout golden reference `data_list_form_fields_grid_v1_1` from `docs/reference/data-list-form-field-layout-templates.json`. The generator must clone `form_grid_fields_wrapper` into the page-level template's `section_content_area`, place field controls inside that Grid, set every field control margin to zero, assign each field control a business-specific `nv_label`/`nav_label`, keep Multiple line/Rich text/Sub list controls as full-row controls, and keep responsive column spans bounded by the parent Grid's columns. Sub list field controls must clone the control-level `data_list_form_control_sublist_v1_1` template from `docs/reference/data-list-form-control-sublist.template.json` and preserve its locked style/table/header/card settings while mapping business field metadata.

New/Edit forms must focus on the current list item and must not include related Collection/Data Analytics/KPI regions. View Item forms may show current-record information plus related business data, approved Collection templates, Data Analytics templates, and KPI regions.

Generated-final packages must pass:

```bash
node scripts/validate-data-list-form-layout-template.mjs --package <app.yapk> --plan <yeeflow-app-plan.md>
node scripts/validate-data-list-form-fields-template.mjs --package <app.yapk> --plan <yeeflow-app-plan.md>
```

Signing readiness, install/import, upgrade, and runtime proof must remain blocked when custom Data List form layouts fail this gate.

## Default Data View Rules

Every generated data list must configure the default data view with visible display fields.

### Data View Contract Before Creation

Fields alone are not a Data List deliverable. Before any MCP create or update call, every planned Data List — including a hidden support or lookup list — must have an explicit Data View contract containing:

- the default Type `0` View name and its `IsDefault: true` designation;
- visible field names in display order, with native `Title` first;
- query/search fields that cover every visible column;
- the default sort, or an explicit `No default sort` decision;
- executable fixed filters in `LayoutView.filter[]`, or an explicit all-record/no-filter decision;
- the New, Edit, and View Type `1` form targets that the list's `ListModel.LayoutView.add/edit/view` will resolve to; and
- the business consumer or operating purpose of the View (navigation, Dashboard source, lookup maintenance, or support administration).

`Hidden in navigation` means the list has no primary menu item. It never means `Layouts: []`, no Type `0` Data View, or missing form routing. Do not submit a list-only/fields-only MCP create request and plan to add views later.

Immediately after the MCP save, re-read the persisted list and run `validate-data-list-completion-contract.mjs`. The readback must contain exactly one default Type `0` View with a meaningful title, `Ext1.Url = "default"`, parseable `LayoutView`, resolved visible columns, query coverage for those columns, and resolved Type `1` New/Edit/View form routes. `apiAccepted` and a fields-only readback are not completion evidence.

Required:

- Include `Title` or the primary name field.
- Include 3 to 8 useful business fields when available.
- Put status, owner, type, date, amount, risk, or priority fields near the front.
- Include lookup fields that help users understand relationships.
- Do not leave the default view display field list empty.
- Do not rely on a simplified `Layouts[].LayoutView.layout` shape only. API install acceptance does not prove that Yeeflow runtime will materialize data-view columns.
- Use the export-proven default view metadata shape: `Layouts[].Ext1 = "{\"Url\":\"default\"}"`.
- Use full column objects in `Layouts[].LayoutView.layout`, not field-name-only shortcuts.

Example for `Vendors`:

```text
Vendor Name
Vendor Type
Country / Region
Risk Level
Onboarding Status
Compliance Status
Owner
Renewal Date
```

Example for `Vendor Documents`:

```text
Document Name
Vendor
Document Type
Review Status
Expiry Date
Reviewer
```

Generated-final validation must fail when a default data view has no display fields.

Use the following evidence labels in the generation report:

- `apiAccepted`: the create/update request was accepted;
- `persistedReadback`: the list, view, and form IDs were returned and passed the completion contract;
- `designerOpen`: the Data View opens with the expected columns, filter, sort, and form routes in Designer; and
- `browserActionRuntime`: the rendered View opens and its New/Edit/View actions work.

Do not call `apiAccepted` or `persistedReadback` a usable Data View.

## Data View Fixed Filters

Every generated data view must follow `docs/standards/collection-02.md#data-list-view-filter-standard`.

Planning and materialization rules:

- Default/all-record views may use `LayoutView.filter: []`.
- Business-specific views must materialize their fixed dataset constraints into `LayoutView.filter[]`.
- User-facing filter/search fields belong in `LayoutView.query[]`; they do not satisfy a fixed filter requirement.
- Section 13 fixed-filter column aliases such as `Filters` and `Data Filters` must be parsed the same as `Filter Conditions`.
- Non-empty fixed-filter cells must be executable field-level conditions. Vague phrases such as `All active meetings` must be converted to concrete filters, for example `Meeting Status = Active` or `Meeting Date is not empty`, before generation.
- If the fixed-filter text is non-empty and not an explicit all-record/no-filter marker, generated `LayoutView.filter[]` must contain at least one condition or materialization must fail.
- `op: "7"` with `right: null` is the export-proven non-empty check.
- `pre: "and"` and `pre: "or"` are both valid and must be preserved from the App Plan.
- Business wording such as `Date >= Today` must be converted to the export-proven `now` expression for Data View filters.

Example:

```json
{
  "pre": "and",
  "left": "Datetime1",
  "op": "3",
  "right": [
    {
      "type": "func",
      "func": "now",
      "params": []
    }
  ],
  "showCus": false
}
```

## Default View Runtime Materialization Shape

The default data view must use the export-proven shape that Yeeflow recognizes at runtime. A package can install successfully and still render an empty data view when the view metadata is too simplified.

Required default view metadata:

```text
Layouts[].Title = "All Items" or a business-specific view title
Layouts[].Type = 0
Layouts[].IsDefault = true
Layouts[].Ext1 = "{\"Url\":\"default\"}"
Layouts[].LayoutView = JSON string with layout/query/sort/filter metadata
```

Each visible column object inside `LayoutView.layout` must include:

```text
FieldID
FieldName
DisplayName
Type
Order
Mobile
Show
```

Recommended column object shape:

```json
{
  "FieldID": "<field id>",
  "FieldName": "Text2",
  "DisplayName": "Account Owner",
  "Type": "input",
  "Order": 1,
  "Mobile": 2,
  "Show": true
}
```

`LayoutView.query` should include the visible field names plus common system fields used by runtime views:

```text
ListDataID
CreatedBy
ModifiedBy
Created
Modified
```

Do not treat these as sufficient:

```text
layout: ["Title", "Text1", "Text2"]
layout: [{ "FieldName": "Text1" }]
layout: [{ "field": "Text1" }]
```

These simplified shapes can pass local structural checks but still fail runtime materialization, leaving views such as `All Accounts` or `All Health Signals` with no visible columns.

Suggested hard error:

```text
DEFAULT_VIEW_DISPLAY_FIELDS_MISSING
DEFAULT_VIEW_EXT1_URL_MISSING
DEFAULT_VIEW_COLUMN_SHAPE_INCOMPLETE
DEFAULT_VIEW_RUNTIME_MATERIALIZATION_RISK
```

## Choice Field Option Rules

Every single-select, multiple-select, multi-select, radio, checkbox, select, or tag-style field must include non-empty business-specific options.

Required:

- Use the runtime-visible, export-proven option shape in `Rules.choices`.
- When color metadata is useful, also include aligned `Rules.color_choices`.
- Do not use `Rules.Options`, `Rules.options`, `Rules.Items`, `Rules.Choices`, top-level `Options`, or top-level `Choices` as the only option source. Those paths can pass loose structural checks but are ignored by the runtime dropdown UI.
- Do not create blank option rows.
- Do not create generic options such as `Option 1`, `Option 2`, unless the user explicitly requested them.
- Options must match the business domain.
- Status and lifecycle fields must include all expected statuses used by dashboards, filters, forms, and sample data.
- Apply this rule equally to single-select and multiple-select fields. Multiple-select fields must not be treated as text-only fields without options.
- Sample data values for choice fields must match one or more configured `Rules.choices[].value` entries.

Recommended option shape:

```json
{
  "choices": [
    { "key": "1", "value": "Low", "color": "#22c55e" },
    { "key": "2", "value": "Medium", "color": "#f59e0b" },
    { "key": "3", "value": "High", "color": "#ef4444" }
  ],
  "color_choices": [
    { "key": "1", "value": "Low", "color": "#22c55e" },
    { "key": "2", "value": "Medium", "color": "#f59e0b" },
    { "key": "3", "value": "High", "color": "#ef4444" }
  ]
}
```

Example `Risk Level` options:

```text
Low
Medium
High
Critical
```

Example `Document Type` options:

```text
Insurance Certificate
Tax Form
Security Assessment
Contract
Compliance Evidence
```

Example `Review Status` options:

```text
Not Started
In Review
Needs Information
Approved
Rejected
Expired
```

Generated-final validation must fail when a choice field has missing or blank options.

Suggested hard errors:

```text
CHOICE_OPTIONS_MISSING
CHOICE_OPTION_BLANK
CHOICE_OPTION_GENERIC
CHOICE_OPTION_RUNTIME_SHAPE_MISSING
CHOICE_SAMPLE_VALUE_NOT_IN_OPTIONS
MULTI_SELECT_OPTIONS_MISSING
MULTI_SELECT_SAMPLE_VALUE_NOT_IN_OPTIONS
```

## Lookup Relationship Rules

When data lists are related, the generator must model the relationship as Yeeflow lookup fields instead of duplicating the related record name as plain text.

Required:

- Identify master/reference lists before generating dependent lists.
- Create master/reference lists first.
- Create lookup fields in dependent lists.
- Configure lookup `Rules` with target AppID, ListSetID, ListID, and display field metadata.
- Select a valid display field from the target list, usually `Title` or the target list primary name field.
- Do not point lookup display fields to nonexistent fields such as `Text0` when the target list uses `Title`.
- Include lookup fields in default views and forms where users need the relationship.

For parent/detail relationships, also plan reverse-related View Item sections. When list B contains a lookup field to list A and users need to inspect B records while viewing A, the parent list A View Item form should include a reverse-related Collection section:

- Host form: parent Data List View Item or Workbench View Item form.
- Child source: the dependent Data List that owns the lookup field.
- Read filter: child lookup storage field equals current host item `ListDataID`.
- Search: `search-filter.binding` must be consumed by `collection.attrs.data.fulltext[].value`, with `fulltext.fields[]` resolving on the child list.
- Add: if child creation is allowed, the Add `action_button` must target the child list and include `attrs.passvalues[]` mapping the child lookup field `FieldName` to the current host item `ListDataID` expression.
- Never write the parent title, display name, label, hardcoded ID, or sample row ID as the child lookup value.

This pattern is View-only by default. New/Edit forms must not include reverse-related Collections because a saved current `ListDataID` may not exist yet.

Vendor Onboarding required relationship example:

```text
Vendors
  <- Vendor Documents.Vendor lookup
  <- Compliance Reviews.Vendor lookup
  <- Vendor Activity / History.Vendor lookup
```

In this structure:

- `Vendors` must be created first.
- `Vendor Documents`, `Compliance Reviews`, and `Vendor Activity / History` must each include a `Vendor` lookup field.
- The lookup display field must resolve on `Vendors`, normally `Title` / `Vendor Name`.
- Dependent sample rows must use valid `Vendors` sample record IDs.

Generated-final validation must fail when lookup targets or display fields are unresolved.

Suggested hard errors:

```text
LOOKUP_TARGET_LIST_MISSING
LOOKUP_DISPLAY_FIELD_MISSING
LOOKUP_DISPLAY_FIELD_NOT_FOUND
LOOKUP_RULES_INCOMPLETE
LOOKUP_RELATIONSHIP_MODELED_AS_TEXT
```

## Data List Creation Order

When multiple lists are generated in one application, the creation order must follow dependency order.

Required order:

1. Master/reference lists.
2. Lists that depend on master/reference lists.
3. Transaction/detail/activity/history lists.
4. Dashboards and forms that bind to those lists.
5. Workflows/actions that depend on the lists.

Example:

```text
1. Vendors
2. Vendor Documents
3. Compliance Reviews
4. Vendor Activity / History
5. Dashboards and custom forms
```

If a dependency graph has multiple master lists, topologically sort the lists so every lookup target exists before dependent lookup fields and sample data are generated.

Generated-final validation must fail when a dependent list is generated before its lookup target is resolvable.

Suggested hard error:

```text
DATA_LIST_DEPENDENCY_ORDER_INVALID
```

## Sample Data Rules

When sample data is requested or needed for runtime testing, generate sample data in dependency order.

Required:

1. Generate sample data for master/reference lists first.
2. Use generated master record IDs as lookup values in dependent sample rows.
3. Generate dependent list sample data after the target lookup rows exist.
4. Keep sample data synthetic and tenant-neutral.
5. Do not use private tenant records, private URLs, or real customer data.

For generated-final app-level `.yapk` packages:

- Do not embed seed/sample rows in `Childs[].List.Items`, `Childs[].ListDatas`, or `Childs[].List.ListDatas`.
- Emit synthetic seed rows as a separate companion artifact.
- Seed artifacts for `identity-picker`, `lookup`, and `file-upload` fields must be structured live-write instructions, not plain strings. Identity-picker sample values must declare `seedValueType: "identity-picker"` and `requiresLiveUserResolution: true` so the runtime seed step resolves an existing tenant user before writing. Lookup sample values must declare `seedValueType: "lookup"`, `requiresLookupListDataIDResolution: true`, `storedValueField: "ListDataID"`, the target list, and the display field; the live seed writer must create/read the target rows first and write the target record `ListDataID`, never the lookup display title, code, or other display text. File-upload sample values must declare `seedValueType: "file-upload"` and `requiresFileUploadReference: true` so the runtime seed step waits for an uploaded file reference or leaves the field empty. The generated YAPK field shape remains schema-safe `FieldType: "Text"` plus `Type: "identity-picker"`, `Type: "lookup"`, or `Type: "file-upload"`; do not invent unsupported `FieldType: "User"` storage.
- Run live seed writes only after explicit approval, after install/materialization proof, and with before/after count reporting.
- Use `TriggerFlow: false` by default for demo/test seed writes unless the user explicitly asks to exercise workflows.

### Direct MCP Lookup Write Closure

The structured seed artifact is not permission to substitute display text during a later live MCP create or update. For every direct record write to a Lookup field, the operator must:

1. Read the host Lookup field metadata and identify its configured target `listid` and display field.
2. Resolve exactly one target row in that target list and capture its returned `ListDataID`.
3. Build a structured write intent with `seedValueType: "lookup"`, `requiresLookupListDataIDResolution: true`, `storedValueField: "ListDataID"`, `mustNotUseDisplayTextAsStoredValue: true`, the configured `targetListId`, and the resolved `ListDataID`.
4. Write that resolved ID, not a title, number, code, label, or other display value.
5. Re-read the host record and verify the persisted field equals the resolved target ID and resolves in the configured target list.

Capture redacted target resolution, intended writes, and persisted readback in a closure-evidence JSON file and run `node scripts/validate-live-lookup-write-closure.mjs --input <evidence.json>`. It fails closed for an unstructured title write, a target-list mismatch, an unresolved target record, missing readback, or a persisted value different from the resolved ID. When the available MCP toolset cannot safely perform the target-row readback, stop and report the missing capability; never guess an ID or downgrade to display text.

Report `apiAccepted`, `persistedReadback`, `lookupReferenceResolved`, and `browserActionRuntime` separately. The validator proves only supplied resolution/readback evidence; it does not prove form save, Designer rendering, or browser interaction. Only a focused browser runtime test establishes those user-facing behaviors.

For app-level `.yap` packages or non-final local-only experiments where the target list and dependent list are packaged together:

- Local sample record IDs may be generated in the package.
- Yeeflow should remap target sample record IDs and dependent lookup values consistently during import/install.
- Export-back validation should confirm dependent lookup values resolve after import when runtime testing is available.

For standalone `.ydl` packages that reference an already-existing external lookup target:

- Use real imported target `ListDataID` values only when provided safely.
- Exclude external target record IDs from `ReplaceIds`.
- Do not invent external lookup target record IDs.

Generated-final validation must fail when sample lookup values reference missing target rows.

Suggested hard errors:

```text
LOOKUP_SAMPLE_TARGET_ROW_MISSING
LOOKUP_SAMPLE_VALUE_UNRESOLVED
MASTER_SAMPLE_DATA_MISSING
DEPENDENT_SAMPLE_DATA_ORDER_INVALID
```

## Form and View Relationship Rules

Default views and forms must expose relationships clearly.

Required:

- Lookup fields must be included in dependent list default views.
- Lookup fields must appear in New/Edit/View forms when the user needs to select or understand the related record.
- Lookup display fields must already be selected.
- Do not leave lookup fields with empty display-field settings.
- Known product issues, such as a lookup picker not returning records in a specific tenant, must be documented separately and must not be fixed by removing lookup display-field validation.

## Validator Checklist

Before a generated package is returned:

- [ ] Every data list has a non-empty default display field set.
- [ ] Every default view includes `Title` or primary name.
- [ ] Every default view has `Ext1.Url = default`.
- [ ] Every default view column uses the full export-shaped column object with `FieldID`, `FieldName`, `DisplayName`, `Order`, `Mobile`, and `Show`.
- [ ] Every choice field has non-empty, business-specific options.
- [ ] Single-select and multiple-select fields both have populated options.
- [ ] No choice field has blank option rows.
- [ ] Every cross-list relationship is modeled as a lookup field.
- [ ] Every lookup field has complete target metadata.
- [ ] Every lookup display field exists on the target list.
- [ ] Master/reference lists are generated before dependent lists.
- [ ] Master/reference sample rows are generated before dependent sample rows.
- [ ] Dependent sample lookup values point to valid target sample record IDs.
- [ ] Lookup fields are included in relevant views and forms.
- [ ] Custom Add/Edit/View forms load correctly or have safe default routing.

## Generator Checklist

When designing a generated application:

1. Identify master/reference lists.
2. Identify dependent lists and lookup relationships.
3. Draw or write the list dependency graph.
4. Generate fields with correct schema and options.
5. Generate master lists first.
6. Generate dependent lookup fields after targets are known.
7. Generate default data views with export-shaped metadata and meaningful display fields.
8. Generate sample data in dependency order.
9. Validate lookup display fields and sample lookup values.
10. Validate runtime-facing forms and dashboards against the data model.

## Failure Examples

These are quality failures:

- A data list opens with no display fields in its default view.
- A data list has fields in `LayoutView.layout` but the runtime view is empty because `Ext1.Url = default` or full column metadata is missing.
- A single-select field opens with blank option rows.
- A multiple-select field opens with blank option rows.
- A dependent list stores a related vendor name as plain text instead of a lookup to `Vendors`.
- A lookup field points to a nonexistent display field.
- A dependent sample row references a lookup target row that was never created.
- A dashboard expects related records but the underlying data model has no lookup relationships.

## Proof Boundary

This standard captures known generation lessons from Vendor Onboarding v4.1 and prior related-list lookup studies. It does not claim that every tenant's lookup picker runtime behavior is fully solved. If a lookup field is structurally correct but a product runtime API returns no records, record that as a product-team follow-up while keeping the generation requirements for lookup target metadata, display fields, and data dependency order.

---

<a id="data-list-view-filter-standard"></a>

<!-- source: docs/standards/collection-02.md#data-list-view-filter-standard -->

# Data List View Filter Standard

This standard governs fixed Data View filters for Yeeflow Data Lists and Document Libraries. It applies to full application `.yapk` generation and standalone `.ydl` generation.

## Contract

Data View fixed filters live in the parsed `Layouts[].LayoutView.filter[]` array. They are different from user filter/search fields, which live in `Layouts[].LayoutView.query[]`.

Each generated App Plan data view must decide both:

- `Filter Conditions`: fixed business constraints that every user sees.
- `Query/Search Fields`: interactive filter fields that users can apply at runtime.

Do not satisfy a fixed filter requirement by adding only `query[]` fields.
Do not treat App Plan Section 13 as navigation metadata only. Every planned Data View must materialize as a real `Layouts[]` record with `Type: 0`, `Title`, `Ext1.Url`, and a complete `LayoutView` object.

The App Plan table may use `Filter Conditions`, `Fixed Filters`, `Filter`, `Filters`, `Data Filter`, or `Data Filters` as the fixed-filter column header. The generator must treat all of these as fixed `LayoutView.filter[]` input, not as display copy.

Fixed filters must be concrete field-level conditions. Phrases such as `All active meetings`, `Meeting lifecycle`, `active records`, or `pending work` are not executable by themselves. If a business view narrows records, the plan must write explicit filters such as `Meeting Status = Active`, `Meeting Date is not empty`, or `Pack Status != Completed`. If a non-empty fixed-filter cell does not materialize at least one `LayoutView.filter[]` condition, generated-final materialization must fail instead of silently emitting `filter: []`.

If a planned Data View references business columns, query/search fields, or fixed filters, the host Data List must have a parseable field table. A generated business view must not downgrade to the native `Title` field only because the field table was missing or written with unsupported headings. In that case generated-final materialization must fail with `DATA_LIST_FIELD_TABLE_REQUIRED_FOR_PLANNED_VIEW`.

## Export-Proven Basic Shape

```json
{
  "key": "<uuid-or-stable-condition-id>",
  "pre": "and",
  "left": "Datetime1",
  "op": "7",
  "right": null
}
```

Rules:

- `left` must be a real field storage name from the host list, such as `Datetime1`, `Text3`, `Decimal1`, or a known system field.
- `pre` may be `and` or `or`.
- `op: "7"` means the field is not empty and must use `right: null`.
- `right` may be a literal value, `null`, or an expression token array when the export proves that shape.
- Nested groups may use a `conditions[]` child array, up to two levels, when a view needs grouped `and` / `or` logic.

## Event Planning Golden Reference

The `Event Planning (1).ydl` focused sample proves these view-filter patterns:

| View | Fixed `LayoutView.filter[]` | User `LayoutView.query[]` |
| --- | --- | --- |
| `All Events` | `[]` | `Name`, `Date`, `RSVP Status` |
| `Schedule Overview` | `Datetime1 op "7" right null` AND `Datetime1 op "3" right [{ "type": "func", "func": "now", "params": [] }]` | `Name`, `Date` |
| `RSVP Tracker` | `Text3 op "7" right null` AND `Text4 op "7" right null` | `Name`, `RSVP Status`, `Guest List` |
| `Budget and Vendors` | `Text7 op "7" right null` OR `Decimal1 op "7" right null` | `Name`, `Vendors` |

Field mapping:

- `Date` = `Datetime1`
- `Guest List` = `Text3`
- `RSVP Status` = `Text4`
- `Budget` = `Decimal1`
- `Vendors` = `Text7`

## Today To Now Conversion

Yeeflow's current Data View expression editor does not expose a `Today` function in this export path. When the business requirement says `Date >= Today`, generate the export-proven `now` function:

```json
{
  "left": "Datetime1",
  "op": "3",
  "right": [
    {
      "type": "func",
      "func": "now",
      "params": []
    }
  ],
  "showCus": false
}
```

Never emit `Today`, `today()`, or `{ "func": "Today" }` in generated Data View fixed filters.

## Validation

Generated-final validation must fail when:

- `LayoutView.filter` is not an array.
- a non-empty planned fixed-filter cell is present but the generated `LayoutView.filter[]` is empty.
- A filter field does not resolve to the host list fields or a known system field.
- A leaf condition omits `left` or `op`.
- `pre` is not `and` or `or`.
- `op: "7"` does not use `right: null`.
- a filter uses an unsupported `Today` token/function instead of `now`.
- a planned business view references non-Title columns or fixed filters while the host list has no parseable field table.

Validation must not confuse `LayoutView.query[]` user filters with `LayoutView.filter[]` fixed filters.
The regression gate must also materialize an Event Planning fixture from App Plan to generated-final `.yapk` and assert that the four planned data views appear as decoded `Type: 0` layouts with the expected filters.

---

<a id="form-action-open-resource-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#form-action-open-resource-golden-reference-standard -->

# Form Action Open Resource Golden Reference Standard

Source: normalized export evidence from `Leave Management-v1.7.yapk`, plus safe render-only proof on an existing installed reference baseline for Add Item, New Approval Submission, and Dashboard targets. Record creation, form submission, selected Edit/View, and Submitted Form loading remain outside that proof.

## Exact step types

| Business step | Exact `type` | Operations |
| --- | --- | --- |
| Open item form | `listitem` | add (omit `op_type`), `edit`, `view` |
| Open approval form | `openform` | `new`, `submitted` |
| Open dashboard | `opendashboard` | open Type 103 page |

`listitem` targets only a Data List or Document Library. Selected Edit/View requires a parseable non-empty Yeeflow expression-token array in `listdataid`. Optional `layout` must resolve to a Type 1 custom form on that exact target and its declared purpose must match the operation: Add uses New/New-Edit, Edit uses Edit/New-Edit, and View uses View. Current item mode is allowed only from a Data List or Document Library custom form; Approval Forms and Dashboards must select a data item.

`openform/new` may carry `setVars` and `queryParams`. `setVars.defKey` must equal the target `ProcKey`; every rule requires a target variable ID, the declared target type, and expression tokens; and the target/type/value must be compatible with the target Approval variable schema. `openform/submitted` requires a parseable non-empty expression-token array in `formid` and must not carry `setVars` or `queryParams`.

`opendashboard` requires a `PageID` resolving to a Type 103 Dashboard and may carry `queryParams`. Dashboard-host expressions may reference only declared page temp variables.

Every generated Query Parameter requires a non-empty unique name and either a direct non-null value or a parseable expression-token value. Generated artifacts use the strict contract; compatibility validation may accept older export omissions only where runtime evidence supports them.

## Presentation

| UI choice | `attrs.op` |
| --- | --- |
| Slide in | `slide` |
| Pop-up window | `modal` |
| Full page | `target` |
| New window | `new` |

Slide/Pop-up sizes use `modalsize`: `0`, `1`, `2`, `3`, or custom `9`. Custom requires positive `cusize.w`. Full page and New window must not carry modal sizing.

## Host rules

- Approval Submission and Task Forms use Approval workflow variables and may only select a target item.
- Data List and Document Library custom forms may use current item or a selected item. Current item omits `data.list`, `source_type`, and selected-record `listdataid`.
- Dashboard uses declared temp variables only and may only select a target item.
- Public Forms cannot contain any of these three step types.
- Trigger bindings use `formAction.onLoad`, `attrs.control_event_rule`, or `attrs.control_action` through the shared Form Action trigger binder.

## Required gates

- Target resource and optional custom layout resolve locally.
- Selected custom form purpose matches Add/Edit/View.
- Operation and target mode are host-compatible.
- Selected Edit/View or Submitted Form has a parseable non-empty ID expression-token array.
- Approval Set Variables resolve to the target schema and preserve compatible target and expression value types.
- Query Parameter names are present and unique, and each value is direct or expression-backed.
- Submitted Form contains no defaults or query parameters.
- Open mode and size are structurally compatible.
- Planned steps materialize exactly once before signing.

---

<a id="form-action-print-page-barcode-scan-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#form-action-print-page-barcode-scan-golden-reference-standard -->

# Form Action Print Page and Barcode Scan Golden Reference Standard

## Scope

`Online Library-v3.1.yapk` export-proves Dashboard-hosted Print page and Barcode scan Form Action steps, a multi-record print Dashboard built with Collection + Table + QR Code, and a Text/input Data List field with barcode/QR scanning enabled.

This round proves Dashboard as the Print page target. Data List item forms and Approval submission/print pages require their own focused target exports before generated-final support is expanded.

## Print Page

- Exact step type: `print`.
- Dashboard target: `attrs.printtype = "select"`, `attrs.data.Type = "page"`, and `attrs.data.SourceID` resolves to a Type 103 page.
- Print title uses a non-empty Yeeflow expression-token array in `attrs.ptit`.
- A4 is `settings.Size = "6"`.
- Layout is `portrait` or `landscape`.
- Scale is a decimal string, for example 80% is `"0.8"`.
- Minimum margins are `settings.Margins = "3"`.
- The print target must select `dashboard-print-multi-record-table-v1` in the App Plan.

The print Dashboard requires a one-column Collection, a `table-v2` inside each Collection item, cell content containers, Text/heading labels, Dynamic controls for business values, and at least one valid `attrs.table-merges` row/column merge. Per-item QR uses `list-qrcode` with `attrs.qr-code-link.type = "2"`, which means Current Collection item.

## Barcode-enabled Text field

Scanning is a capability of a normal Text/input field, not a separate barcode field type:

```json
{
  "FieldType": "Text",
  "Type": "input",
  "Rules": "{\"allowScan\":true}"
}
```

The App Plan must explicitly set `Allow Barcode / QR Scan = Yes`. Do not infer it from labels such as ISBN, serial number, or asset tag.

## Barcode Scan Form Action

- Exact step type: `barcode`.
- Modes: `multiple`, `select`, `auto`.
- Auto barcode type omits `attrs.type`; ISBN/EAN-13 uses `attrs.type = "ean-13"`.
- `attrs.response` and `attrs.message` both use `{ "prefix": "__temp_", "value": "<variable name>" }`.
- Result and error variables must exist in the host page `tempVars`.
- The export proves Stop action on read error. Continue-on-error remains focused-export-proof-required.
- Scanned values must have a consumer. The standard consumer is a Collection fixed filter with operator `9`, whose right operand references `__temp_<result variable>`.

## Hard gates

Generated-final validation fails unresolved Print Dashboard targets, missing title tokens, unsupported print setting encodings, print targets without Collection/Table/merge/current-item QR, unsupported scan modes or barcode types, undeclared result/error temp variables, orphan scan results, and `allowScan=true` on non-Text/non-input fields.

## Generated-final integration contract

The App Plan, generated-final materializer, decoded package, and canonical preflight are one contract:

- `Allow Barcode / QR Scan = Yes` must survive field normalization and produce stringified Text field `Rules.allowScan = true`; parsing the plan without emitting the rule is a hard failure.
- A Dashboard selected as `dashboard-print-multi-record-table-v1` must still clone and preserve the canonical `dashboard-page-layouts-v1.1` page shell. Store the print template in the dedicated `printDashboardTemplateId` provenance field instead of replacing the page-shell marker.
- Canonical `content_card_wrapper` modules preserve both `section_title_area` and `section_content_area`. Remove source-template text, then map a business title; do not delete the title slot or reconstruct a structurally different substitute.
- Dynamic values inside the printable `table-v2` expose the field on the canonical root and `attrs.obj-f`, `attrs.field`, and `attrs.data.field` binding surfaces.
- The printable Collection carries a real Data List source, deterministic sort, no-open behavior, one-column repeated layout, `table-v2` merge metadata, and `qr-code-link.type = "2"` for the current Collection item.
- Planned Print and Barcode actions must have reachable bound controls. Use the shared trigger-insertion helper when the selected page template does not already contain a suitable control; do not hand-patch runtime candidates.
- Focused validation is necessary but not sufficient. Every generated-final regression fixture must also pass `yapk-first-generation-preflight.mjs`, including Dashboard layout, dataset presentation, page-scope dependencies, field rules, and form-action gates.

## Proof boundary

The schema and package shapes are export-proven and validator-backed. Actual browser print output, physical scanner/camera behavior, permission prompts, device compatibility, scanned QR destinations, and page-break quality still require focused runtime proof.

---

<a id="form-action-query-data-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#form-action-query-data-golden-reference-standard -->

# Form Action Query Data Golden Reference Standard

## Scope

This standard governs `type: "querydata"` steps inside front-end Yeeflow Form Actions. Focused references include Approval Submission v1.1, Custom Data List Form v1.2, Dashboard v1.3, pagination/temp JSON/Document Library/Approval Task Form v1.4, and Document Library/Form Report sources v1.10.

The focused exports prove Approval Submission, Custom Data List Form, and Dashboard placement with Data List sources. Product guidance also allows Form Action Query Data on Approval task/print pages; those unstudied wrappers remain focused-proof-required. Public Forms must never contain Query Data steps.

Workflow Action Query Data is a different workflow-node surface and is outside this focused training.

## Golden Modes

Every planned Form Action Query Data step must select one explicit mode.

| Mode | Query type | Record output | Count output | Intended lifetime |
| --- | --- | --- | --- | --- |
| `single_to_variables` | Single, exported default may omit `querydata_type` | `querydata_fieldmap` to workflow variables | Optional | Persisted with Approval instance and usable by workflow |
| `single_to_temp_variables` | Single, exported default may omit `querydata_type` | `querydata_fieldmap` to `__temp_<id>` targets | Optional | Current page session only |
| `single_to_list_fields` | Single, exported default may omit `querydata_type` | current Data List fields through `____customListFields_<FieldName>` | Optional | Current record, persisted when the New/Edit form is saved |
| `single_to_list_fields_and_temp_variables` | Single, exported default may omit `querydata_type` | current Data List fields plus `__temp_<id>` display targets | Optional | Mixed persisted form state and page-session display state |
| `multiple_to_sublist` | `querydata_type: "multiple"` | Sub list variable through `querydata_listname`, `querydata_vartype: "list"`, `querydata_listname_parent: "__variables_"`, and field map | Optional | Persisted form Sub list / workflow context |
| `multiple_to_list_sublist` | `querydata_type: "multiple"` | current record Sub list through `querydata_listname_parent: "__list_"` | Optional temp count | Editable current-record working copy |
| `multiple_count_only` | `querydata_type: "multiple"` | None | Required through `querydata_totalcount` and `querydata_totalparent` | Workflow or page-session count only |

The previously proven `multiple_to_temp_collection` mode remains supported for transient aggregates/display, with explicit `querydata_fields[]`; it is not one of the four new v1.1 focused templates.

## Business Capability Selection Before Query Mode

Do not start by selecting a Query Data serialization mode. First choose the smallest native capability that satisfies the business requirement:

- read-only multi-row browsing, recent records, and reverse-related rows: bind an approved Collection or Data Table directly to the Data List source;
- one related record needed for page-only context: `single_to_temp_variables`;
- one related record whose values must persist with an Approval instance: `single_to_variables`;
- dependent Data List New/Edit enrichment: `single_to_list_fields` or `single_to_list_fields_and_temp_variables`;
- queried rows that become a persisted editable working copy: `multiple_to_list_sublist`;
- count/KPI/visibility logic without row consumption: `multiple_count_only`;
- client-side aggregate, JSON text, or explicitly justified custom transformation: `multiple_to_temp_collection`.

Do not generate `Query Data -> temp JSON/temp collection -> Collection/Data Table`. Dataset controls already own a normal Data List source contract and cannot consume Query Data temp JSON. If the required result genuinely needs custom transformation, name the Custom Code consumer and runtime-proof boundary in the App Plan.

## Required Source Contract

`attrs.querydata_list` must contain confirmed source metadata:

```json
{
  "AppID": 41,
  "ListSetID": "<confirmed-list-set-id>",
  "ListID": "<confirmed-source-id>",
  "ListType": 1
}
```

Keep 19-digit IDs as strings. Do not copy source-template IDs into generated applications. Full-app generation must remap the source to the current API-issued resource IDs.

Focused exports prove `ListType: 1` Data List, `ListType: 16` Document Library, and `ListType: 32` Form Report queries. Data Report remains focused-learning-required until its dedicated training round.

## Filters And Sorts

- Use `attrs.querydata_filters` plural. The singular `querydata_filter` path is ignored by runtime.
- Literal operands use primitive `right` values and `showCus: true`.
- Dynamic/calculated operands use expression-token arrays and `showCus: false`.
- Never serialize expression-button HTML into `right`.
- Use explicit sorts for deterministic single-record selection. Multiple sort rows preserve their listed priority, with a product maximum of two sort fields.
- A single-record query without filters or deterministic sort is a business-quality warning even when structurally valid.
- The Dashboard v1.3 export proves current-user membership filtering as `op: "11"` with `right: null`. Validators must not misclassify this exact nullary operator shape as an incomplete filter.

## Shared Pagination Contract

Approval Form, Custom Data List Form, and Dashboard Query Data use the same pagination semantics:

- omitted `querydata_pagesize` means Page Size `100`;
- explicit `querydata_pagesize` must be an integer from `1` through `1000`;
- omitted Page Number means `1`;
- business planning may request any positive Page Number.

The v1.4 export proves Page Size `5` and Page Number `2`. Serialize non-default Page Number as `querydata_pageindex`; omit it for default Page Number 1. Do not use guessed aliases such as `querydata_page` or `querydata_pagenumber`.

## Output Contracts

### Single To Workflow Variables

Every `querydata_fieldmap` source key must be a real source field. Every target must be a declared workflow variable with a compatible business type. Use this mode when the value must be submitted, persisted, used by workflow routing, or shown on task forms.

### Single To Temp Variables

Every target is serialized as `__temp_<temp-variable-id>`, while the declared temp variable id remains unprefixed under `variables.tempVars[]`. Use this mode only for client-side display and calculations during the current page session.

### Single To Current Data List Fields

- Only Custom Data List New/Edit forms should normally write current-record fields.
- Encode each target with `____customListFields_<FieldName>`; never emit a plain field name.
- `single_to_list_fields_and_temp_variables` may combine current-field targets with declared `__temp_` targets in one field map.
- A field-change trigger is stored on the source field control as `attrs.control_event_rule = <action id>`.
- Use this pattern for dependent-field enrichment after a Lookup selection, such as copying Campaign owner into an Event field while displaying the Campaign description through a temp variable.

### Multiple To Sub List

- The target basic variable must have `type: "list"` and reference a valid `variables.listref[]` definition.
- Every source field must map to an existing row field in that list reference.
- Count output is optional. If it uses `__variables_`, the target must be a number variable.

### Multiple To Current Record Sub List

- Set `querydata_listname_parent: "__list_"`, `querydata_vartype: "list"`, and `querydata_listname` to the host Data List Sub list field name.
- Every `querydata_fieldmap` target must exist in that Sub list control's `list-variables`/`list-fields` schema.
- A result count may use a declared temp variable through `querydata_totalparent: "__temp_"`.
- Prefer New/Edit forms because queried rows are intended to become an editable working copy that is persisted with the host record.
- The v1.2 `View Campaign` sample proves that a View Item custom form can carry this structure, but generation requires an explicit save/edit workflow and business rationale.
- Do not use this pattern merely to show reverse-related rows. Use Collection or Data Table for read-only display. Query into Sub list is appropriate for quotation/product-line scenarios where users edit quantity, description, price, or other loaded row values.

### Multiple Count Only

Count-only output must not retain hidden/stale record-output settings. Omit all of:

- `querydata_fieldmap`
- `querydata_fields`
- `querydata_listname`
- `querydata_vartype`
- `querydata_listname_parent`

The v1.1 source export's fourth step retained Sub list settings while its UI/business intent was count-only. Treat those fields as designer residue, not as the normalized golden template.

### Dashboard Temp Outputs And Chained Queries

- Dashboard pages declare and use temp variables only. Single-query field maps use `__temp_<id>`; result counts use `querydata_totalparent: "__temp_"`; transient collections also use `__temp_`.
- Workflow-variable, current-record field, and current-record Sub list targets are forbidden on Dashboard Query Data.
- Preserve Query Data step order. A later query may filter by a temp ID produced by an earlier query in the same action.
- A chained filter uses expression tokens and `showCus: false`.
- Guard a chained query with `isNullOrEmpty(__temp_<id>) == false` so an absent prior result does not start an invalid dependent query.
- Generated steps require concise business names. Do not preserve an empty step name merely because a manually configured export omitted it.
- A count-only result may drive Text and Dynamic Display. The v1.3 reference shows details only when the count is not empty and greater than zero.
- A multiple query may save selected `querydata_fields[]` to a declared temp JSON value with `querydata_vartype: "text"` and `querydata_listname_parent: "__temp_"`, while also returning a count.
- The temp JSON value is not a Collection/Data Table data source. Use exact function `JSONStringfy` for text output. Use a planned Custom Code control only when a business requirement needs custom tabular processing/display.

### Document Library Custom Forms

- A native `Type: 16` Document Library custom form uses the same Form Action wrapper and Query Data step contracts as a Data List custom form.
- Current-field, current-Sub-list, temp, trigger, pagination, and target validation rules are shared.
- A Document Library lookup may be used as a `list_field` expression operand with `showCus: false` to query its related Data List record.
- V1.10 proves Document Library as both a host and a Query Data source. It also proves Form Report as a source.
- Form Report and Data Report do not host independent Form Actions. Form Report details use the Approval Submission form; Data Report has no custom form surface.

### Approval Task Forms

- Approval Task Form pages (`pageurls[].type: 2`) use the same `formdef.formAction`, `formdef.actions[]`, trigger, step, and pagination serialization as Submission Form pages.
- Validate each Task Form's declared variables/temp variables and action binding in its own page context.

## Host Rules

- Allowed by product guidance: Approval Submission, Approval task, Approval print, custom Data List form, Dashboard.
- Forbidden: Public Form.
- Temp variables are page scoped and must not be represented as persisted Approval/Data List values.
- Dashboard Query Data targets temp variables only.
- Custom Data List page-load actions use `formAction.onLoad`; field-change actions use the field control's `control_event_rule`.
- Every Query Data action must be bound to at least one proven trigger. An action present only in `actions[]` is not executable.
- Do not transfer Approval Submit/Save steps to Dashboard actions.

## App Plan Contract

Every Query Data row must state:

- action and step name;
- host surface/page and trigger;
- one exact golden mode;
- source resource type/name;
- filters and sorts;
- result target type/name;
- source-to-target field mapping;
- count target type/name;
- persistence/page-lifetime intent;
- proof boundary.
- Page Size and Page Number, including whether each uses its shared default.
- exact result consumer/use, especially for temp collections and JSON values.

Planning labels are resolved to current source fields and declared workflow/temp/listref variables during generation. App Plans must not contain copied runtime IDs.

Generation-ready planning must not retain alternative implementation wording such as `temp collection / import buffer`, `Query Data or lookup-bound display`, or `preferably`. Select one exact mode and target; put unselected alternatives in an explicitly deferred row.

Lookup filters and chained-query mappings must distinguish the display value from the stored target identity. Use the target record `ListDataID`/stored target identifier; never pass a display title as the next query's record ID.

Run `node scripts/validate-form-action-query-data-plan.mjs --plan <app-plan.md>` before materialization whenever the Form Actions table includes Query Data.

## Hard Gates

Final generation fails when:

- source metadata is missing/incomplete;
- query type is unknown;
- multiple query has neither record output nor count output;
- count-only mode retains result mappings;
- single query lacks field mappings;
- mapped workflow/temp/Sub list row targets are undeclared;
- workflow count target is not numeric;
- temp collection omits selected fields;
- singular filters, HTML operands, or expression mode mismatches are present;
- Query Data is placed on a Public Form.
- a Custom Data List Form action is unbound;
- a temp target is undeclared;
- a current-record target omits `____customListFields_` or references a missing field;
- a `__list_` target is not a real Sub list field or maps to missing row fields;
- a Data List Form incorrectly uses Approval `__variables_` result targets.
- Page Size is outside `1..1000`;
- Page Number is not a positive integer or uses an alias instead of `querydata_pageindex`;
- a Dashboard result/count target is not a declared temp variable;
- a chained Dashboard query reads a temp before an earlier step produces it;
- a chained Dashboard query omits its not-empty guard.
- Collection/Data Table directly binds a Query Data temp JSON value.
- Query Data is hosted directly by a Form Report or Data Report.
- a temp collection/JSON result omits its supported consumer or is planned as a Collection/Data Table source;
- an exact result target remains ambiguous or uses an import-buffer alternative;
- a Lookup display value or ambiguous Lookup value is used where a target record identifier is required;
- Generation Readiness detects Form Action Query Data intent but no standard planning table exists.

`validate-generation-readiness-review.mjs` must invoke the Query Data plan validator automatically whenever Form Action Query Data intent exists. Passing only resource-order validation is not sufficient to call an App Plan generation-ready.

Schema/package validation does not prove live query execution, returned values, count accuracy, or host-page trigger execution. Keep runtime proof separate.

---

<a id="form-action-set-data-list-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#form-action-set-data-list-golden-reference-standard -->

# Form Action Set Data List Golden Reference Standard

## Scope

This standard covers the `setdatalist` step inside page/form actions on:

- Approval Form Submission and Task pages;
- Data List and Document Library custom forms;
- Dashboard pages.

Public Forms are excluded. Workflow `ContentList` nodes use the separate Workflow Set Data List standard.

## Export-backed baseline

`Leave Management-v1.5.yapk` export-proves ten Form Action steps:

- Approval Submission: conditional Add, conditional Update, and Delete;
- Data List View Item: update current record, Add/Update another Data List, and Delete;
- Dashboard: Add, Update, and Delete.

`Leave Management-v1.6.yapk` adds direct export evidence for:

- Approval Task Form Add/Edit/Remove using the same serialization as Submission Form;
- Document Library View Form current-record Edit;
- one-file Document Library Add from a Data List View Form, including `_Path` and Upload File.

The export is evidence only and is not committed. Normalized references under `docs/reference/form-action-set-data-list-*.template.json` contain no tenant IDs or business data.

## Canonical serialization

Every step uses `type: "setdatalist"`. Its operation is represented by `attrs.type` (`add`, `edit`, `remove`); an export may omit `attrs.type` for Add or current-record Edit, but generated output should be explicit.

- selected target: `attrs.listtype = "select"` plus `attrs.list.AppID/ListSetID/ListID`;
- current record: `attrs.listtype = "current"`, no target list;
- Add/Edit mappings: `attrs.listdatas[]` with `Columns`, `Per`, and expression-token `Data[]`;
- selected Edit/Remove scope: non-empty `attrs.wheres[]`;
- optional status result: `code` plus `codeparent`;
- optional item result: `itemid/itemidparent` or export-backed `totalcount/totalparent`;
- optional execution condition: step-level `condition[]`;
- continue after unmet condition: step-level `continue: true`.

## Operation decisions

| Need | Form Action mode |
| --- | --- |
| Create one target record | `select + add` |
| Update bounded target records | `select + edit` with filters |
| Delete bounded target records | `select + remove` with filters |
| Immediately update the viewed Data List/Document Library record | `current + edit` |
| Change values on a New/Edit form before submit | Prefer Set Variable + Submit Form |
| Expand every Sub List row into a target row | Not supported in Form Action; use Workflow Set Data List |
| Add one file to a Document Library | `select + add`, map `_Path` when needed and map Upload File `Text4` |
| Add multiple selected files | Not supported in Form Action; use Workflow Set Data List with Loop or another workflow pattern |

## Numeric mapping operations

`Per` is part of each mapped target field:

- `0`: Value;
- `1`: Increase;
- `2`: Decrease;
- `3`: Multiply;
- `4`: Divide.

Arithmetic operations require a numeric target field and numeric-compatible right-side expression.

## Host differences

Approval Form Actions can read workflow variables and page temp variables. v1.6 directly proves that Task pages use the same action wrapper and Set Data List serialization as Submission pages. Print Page Set Data List remains focused-export-proof-required in this training round.

Data List/Document Library custom forms can read current list fields. View Item uses Set Data List for persistent current-record commands because Submit Form is unavailable. New/Edit should normally use Set Variable followed by Submit Form.

Data List and Document Library New/Edit/View forms share one custom-form serializer. A selected Document Library Add must map the native Upload File field `Text4`; `_Path` is a valid Document Library pseudo-field and may contain `folder/subfolder` expression tokens. Form Action has no Loop and accepts one file value per Add step. Array, multi-file, List, and Sub List sources are invalid here.

Dashboard Form Actions can mutate selected Data Lists/Document Libraries only. Execute-result outputs must use declared Dashboard temp variables. Click actions may bind to Buttons or Containers through `attrs.control_action`.

## Hard gates

Signing must fail for:

- Public Form placement;
- unknown operation or target mode;
- selected target without a resolvable Data List/Document Library;
- selected Edit/Remove without filters;
- Add/Edit without mappings;
- unresolved target mapping/filter fields;
- undeclared execute-result temp variables;
- partially configured execute-result target pairs or an invalid item/count result attribute;
- Dashboard result targets that are not temp variables;
- Sub List bulk-row sources in a Form Action Set Data List step;
- Document Library Add without the native `Text4` Upload File mapping;
- multi-file, array, List, or Sub List sources mapped to Document Library Upload File;
- generated steps without concise business names.
- click/change/Collection triggers without an exact bound control identity in the App Plan.

Run:

```bash
node scripts/validate-form-action-set-data-list-plan.mjs --plan <app-plan.md>
node scripts/validate-form-action-set-data-list.mjs --package <app.yapk> --plan <app-plan.md> --strict-generated
node scripts/inspect-form-action-set-data-list-yapk.mjs --package <reference.yapk>
```

## Proof boundary

This training provides export-backed serialization, shared generation, and validator-backed pre-signing gates. It does not claim live mutation execution until a separate safe runtime proof creates, updates, and deletes disposable records in an authorized environment.

---

<a id="form-control-type-schema-authority-standard"></a>

<!-- source: docs/standards/collection-02.md#form-control-type-schema-authority-standard -->

# Form Control Type Schema Authority Standard

## Required rule

Generated form control type is determined in this order:

1. explicit App Plan control type;
2. exact field or variable schema type;
3. a schema-safe default such as `input`.

Display labels, business names, and variable IDs must not participate in control-type inference. Words such as `Status`, `Category`, `Type`, `Priority`, `Owner`, `Amount`, and `Date` are not schema declarations.

## Required examples

| Display label | Variable ID | Schema type | Planned control | Required result |
| --- | --- | --- | --- | --- |
| Workflow Proof Status | WorkflowProofStatus | Text | input | input |
| Status Notes | StatusNotes | Multiple line | textarea | textarea |
| Approval Status | ApprovalStatus | Choice | radio | radio with business choices |
| Effective Date | EffectiveDate | Date | datepicker | datepicker |

## Choice controls

`radio` and `select` are allowed only when the schema is choice-like or the App Plan explicitly selects a choice control supported by real business options. A generated choice control must contain non-placeholder options from the App Plan or schema.

## Shared generation

Standalone `.ywf` generation and full-application `.yapk` materialization must use `scripts/lib/form-control-type-authority.mjs` through the shared Approval Form builder. A separate name-based mapper is prohibited.

## Signing gates

The following findings are generated-final blockers:

- `TEXT_VARIABLE_MATERIALIZED_AS_CHOICE_CONTROL`
- `CHOICE_CONTROL_OPTIONS_MISSING`
- `CONTROL_TYPE_VARIABLE_TYPE_MISMATCH`

Run:

```bash
node scripts/test-approval-form-fields-template-gates.mjs
node scripts/validate-approval-form-fields-template.mjs --package <package.yapk> --plan <yeeflow-app-plan.md>
```

Runtime proof may confirm visible behavior, but it must not replace these schema and materialization gates.

---

<a id="functional-specification-standard-template"></a>

<!-- source: docs/standards/collection-02.md#functional-specification-standard-template -->

# <Application Name> - Functional Specification

Assign each core business requirement a stable marker using the `REQ-<AREA>-NN` or `FS-<AREA>-NN` pattern. Preserve each concrete marker in the App Plan so traceability is checked requirement by requirement instead of inferred from generic section keywords.

Use this template as Stage 1 / Step 1 for every Yeeflow application build. The purpose of this document is to convert the user's request, documents, screenshots, examples, and clarifications into a complete business requirement specification before creating the Yeeflow App Plan.

This document focuses on business requirements and expected application behavior. It should read like a business-analysis document that explains the process, rules, data lifecycle, user roles, decision points, exception cases, dashboard information needs, and operational reporting needs before the Yeeflow App Plan exists. It should not be a package-generation blueprint. Yeeflow resource selection, generation order, field internal names, control implementation shapes, package validation, signing, and runtime proof belong in the App Plan and later steps.

Do not include low-level Yeeflow implementation details in the Functional Specification. Exclude Yeeflow control types, exact resource IDs, ListID/PageID/FormID/LayoutID/ProcKey values, actionTypeCode values, JSON property paths, package JSON fragments, and exact generated IDs. When those details are needed, defer them to the App Plan, Page Function Plan, Blueprint, or resource generation stage.

Primary artifact rule: generate this document as the primary human-readable Markdown file named `functional-specification.md`. Machine-readable JSON may exist only as a companion/projection artifact for validation, traceability, or tests, such as `functional-specification.trace.json`; JSON must not replace this Markdown document. The Markdown file must contain the full rich business logic and dashboard business requirements, not just a link to JSON.

Canonical section rule: emit every numbered section in this template, in this exact order and with these section names. Do not merge, rename, skip, or compress later sections even when a requirement is not applicable; write a concrete not-applicable rationale, assumption, or deferred decision in the required section instead.

## 1. Specification Status

Document metadata:

- Application name:
- Request source:
- Source materials:
- Requirement detail level: brief / moderate / detailed / document-backed / screenshot-backed
- Prepared by:
- Prepared with plugin:
- Current status: draft / pending clarification / ready for review / approved
- Review status:
- Known blockers:

## 2. Source Input Summary

Summarize what the user provided.

- Direct user request:
- Business documents:
- Screenshots or mockups:
- Existing app/package/export references:
- Sample data:
- User corrections or constraints:
- Explicit exclusions:
- Dashboard custom implementation preference: preserve an explicit request for primarily Custom Code delivery, supplied reference images, functional regions, interactions and device expectations as user constraints; retain their requirement markers in the App Plan. Record business intent here; defer template IDs and input/output/filter/action wiring to planning.

## 3. Requirement Interpretation Method

State how the requirements were interpreted.

- If the user request is brief, list the inferred business scope and assumptions.
- If the user provided detailed requirements, reorganize them into this standard format without losing details.
- If the user provided documents or screenshots, treat them as reference sources and restate the understood requirements here.
- If anything is unclear, list it in Business Clarification Questions.

## 4. Business Context

Goals and non-goals:

- Business problem:
- Target users:
- Operational scope:
- Expected outcome:
- Business goal:
- Business goals:
- Non-goals:
- What the application manages:
- Primary business value:
- Success criteria:
- Out of scope:

## 5. User Roles and Responsibilities

| Role | What They Need To Do | Records They Can See | Actions They Can Perform | Decisions They Own | Dashboards/Pages They Need | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Role> | <Responsibilities> | <Record scope> | <Read/create/edit/approve/assign/close/admin> | <Decisions> | <Dashboard/page needs> | <Notes> |

Rules:

- Describe business roles, not tenant-specific user IDs.
- Do not assume real users, user groups, departments, positions, or emails unless provided and approved.
- Mark unknown role ownership as a clarification question when it affects workflow routing or permissions.

## 6. Business Objects and Data Requirements

List all business objects that the application needs to manage, track, approve, report on, or reference.

| Business Object | Business Purpose | Required Fields | Field Meaning | Field Type Expectation | Lookup/Reference Relationships | Lifecycle/Status Fields | Audit Fields | Reporting/Dashboard Fields |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Object> | <Purpose> | <Fields> | <Meaning> | <Business-level type expectation> | <Relationships> | <Status/lifecycle fields> | <Created/updated/approved/history fields> | <Metrics/filter/display fields> |

Examples:

- Project and Task
- Employee and Travel Request
- Asset, Asset Request, Assignment, Maintenance Ticket
- Vendor, Onboarding Request, Risk Review

## 7. Business Relationships and Dependency Rules

Describe how business objects relate to each other.

| Parent Object | Child/Related Object | Relationship Type | Business Rule | Required for Creation? | Notes |
| --- | --- | --- | --- | --- | --- |
| <Parent> | <Child> | one-to-many / lookup / reference / line items | <Rule> | Yes/No | <Notes> |

Rules:

- Identify parent objects before child objects.
- Identify lookup-like relationships early.
- Identify line-item/sublist needs early.
- Identify whether relationship data must persist as child rows, form sublist data, or separate transaction records.

## 8. Core Business Process

Describe the main business process from start to finish.

1. <Step 1>
2. <Step 2>
3. <Step 3>

For each process, include:

- Start trigger:
- Submission/intake:
- Review/approval:
- Assignment/fulfillment:
- Status tracking:
- Completion/closure:
- Exception handling:
- Audit/history needs:
- Actor:
- Data created or updated:
- Decision points:
- Status changes:
- Notifications or handoffs:
- Completion outcome:

## 9. Business Rules and Status Lifecycles

| Lifecycle Name | Applies To | Status Values | Initial Status | Final Statuses | Transition Rules |
| --- | --- | --- | --- | --- | --- |
| <Lifecycle> | <Business object/process> | <Statuses> | <Status> | <Statuses> | <Rules> |

| Rule Area | Business Rule | Applies To | Condition | Required Data/Fields | Responsible Role | Exception/Rework Behavior | Reporting Impact |
| --- | --- | --- | --- | --- | --- | --- | --- |
| status lifecycle / approval / assignment / SLA / validation / document / notification / escalation / completion / cancellation / permission | <Rule> | <Object/process> | <Condition> | <Fields> | <Role> | <Exception behavior> | <Dashboard/reporting impact> |

Rules:

- Status, priority, condition, approval state, fulfillment state, and lifecycle state should be explicit.
- If status values affect routing, reporting, or permissions, mark them as business-critical.
- Include approval rules, assignment rules, SLA/overdue rules, validation rules, required document/rich data rules, notification rules, escalation rules, completion rules, cancellation/rejection/rework rules, and permission rules when applicable.

## 10. Approval and Review Requirements

| Approval/Review Process | Trigger | Submitter | Reviewers/Approvers | Decisions | Required Task Work | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Process> | <Trigger> | <Role> | <Roles> | approve/reject/rework/etc. | <Tasks> | <Notes> |

Include:

- Whether formal approval is required.
- Who approves or reviews.
- Whether assignment/task work is needed.
- Whether multiple task forms are needed.
- Whether approval routing depends on amount, category, department, location, priority, requester, or other fields.
- What happens on approval, rejection, cancellation, return, resubmission, or exception.
- Required attachments/documents for submission, review, or completion.
- Completion rules, including what data or evidence is required before closure.

## 11. Data Entry and Form Requirements

Describe each user-facing form from a business perspective.

| Form Purpose | Used By | When Used | Information Captured | Read-only Context | Validation Needs | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Form> | <Role> | <Process step> | <Fields/concepts> | <Context> | <Rules> | <Notes> |

For each form, consider:

- Required fields.
- Read-only fields.
- Default values.
- Dynamic display rules.
- Lookup/reference data.
- Repeating line items.
- Attachments.
- Calculations or summaries.
- Business validation.
- Submission guards.

## 12. Workflow and Notification Requirements

| Workflow/Notification | Trigger Condition | Business Condition | Actor/Recipient | Action/Result | Timing/SLA | Notification Content Intent | Data Read | Data Updated | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Requirement> | <Trigger> | <Condition> | <Actor/recipient> | <Action/result> | <Timing/SLA> | <Intent> | <Data> | <Data> | <Notes> |

Include:

- Page load behavior.
- Form submission behavior.
- Field change behavior.
- Approval workflow behavior.
- Data-list workflow behavior.
- Scheduled workflow behavior.
- Notifications.
- AI-assisted behavior.
- External integration behavior.

## 13. Dashboard Page Requirements

Functional Specification must define dashboard requirements at the business level before the App Plan. Do not select Yeeflow control types, IDs, actionTypeCode values, ListID/PageID/FormID values, JSON property paths, layout JSON, or exact generated resources here.

For each Dashboard page:

| Dashboard Name | Primary Users | Business Purpose | Business Questions It Must Answer | Source Business Objects/Data Lists | Mobile Support Requirement | Business Exceptions/Alerts To Highlight |
| --- | --- | --- | --- | --- | --- | --- |
| <Dashboard> | <Roles> | <Purpose> | <Questions> | <Objects/lists by business name> | <Requirement> | <Alerts/exceptions> |

Required summary metrics:

| Dashboard | Metric | Source Object/List | Source Fields | Calculation Logic | Default Scope | Alert/Threshold Logic |
| --- | --- | --- | --- | --- | --- | --- |
| <Dashboard> | <Metric> | <Object/list> | <Fields> | <Business calculation> | <Scope> | <Threshold/alert> |

Required data regions:

| Dashboard | Data Region | Business Purpose | Source Object/List | Display Fields | User Actions Needed | Sorting/Grouping Requirements |
| --- | --- | --- | --- | --- | --- | --- |
| <Dashboard> | <Region> | <Purpose> | <Object/list> | <Fields> | <Actions> | <Sort/group rules> |

Filter requirements:

| Dashboard | Filter | Source Object/List | Source Field | Default Scope | Applies To Regions | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Dashboard> | <Filter> | <Object/list> | <Field> | <Default> | <Regions> | <Notes> |

## 14. Reporting and Audit Requirements

| Report/Page/KPI | Business Question | Users | Data Needed | Filters | Actions | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Report/Page/KPI> | <Question> | <Roles> | <Data> | <Filters> | <Actions> | <Notes> |

Include:

- Reports and dashboards.
- KPIs.
- Queues.
- Workbenches.
- Operational tables.
- Status boards.
- Audit/history views.
- Print or export requirements.
- Chart requirements.
- What needs to be auditable.
- What history should be visible.
- What compliance or operational evidence is needed.

## 15. Document and Attachment Requirements

| Document/Attachment | Used By | Required When | Storage Need | Preview/Print Need | Notes |
| --- | --- | --- | --- | --- | --- |
| <Document> | <Role> | <Condition> | <Need> | <Need> | <Notes> |

Include:

- Attachments on forms.
- Document libraries.
- Generated documents.
- Print pages.
- QR/barcode output.
- File retention or audit expectations.

## 16. AI, Copilot, and Intelligent Assistance Requirements

| Capability | Purpose | Users | Inputs | Outputs | Human Review | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <AI Agent/Copilot/AI action> | <Purpose> | <Roles> | <Inputs> | <Outputs> | Required/Not required | <Notes> |

If no AI is required, state: No AI Agent or Copilot requirement identified.

## 17. Integration Requirements

| Integration | Purpose | Direction | Data Exchanged | Credential Sensitivity | Required Now? | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| <Integration> | <Purpose> | inbound/outbound/both | <Data> | low/medium/high | Yes/No | <Notes> |

Rules:

- Never include secrets in this document.
- Tenant URLs, API keys, webhook URLs, and private endpoints must be treated as post-import configuration unless explicitly approved for a safe test tenant.

## 18. Permissions and Access Requirements

Permissions and visibility rules:

| Role | Resource/Process | View | Create | Edit | Delete/Archive | Approve | Admin | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <Role> | <Resource> | Yes/No | Yes/No | Yes/No | Yes/No | Yes/No | Yes/No | <Notes> |

## 19. UI and Experience Requirements

Describe the expected user experience without selecting exact Yeeflow controls yet.

- Navigation style:
- Main pages:
- Workbench or dashboard needs:
- Form layout expectations:
- Mobile/tablet expectations:
- Visual style requirements:
- Provided mockups/design references:
- Accessibility/readability concerns:

## 20. Business Clarification Gates

List decisions that materially affect workflow, data persistence, validation, dashboards, routing, approvals, or permissions.

| Decision Key | Question | Options | Recommended Default | Why It Matters | Required Before App Plan? | Approval Status |
| --- | --- | --- | --- | --- | --- | --- |
| <key> | <Question> | <Options> | <Default> | <Business/routing/data/dashboard/generation impact> | Yes/No | unanswered/answered/default-applied-for-planning/user-default-approved-for-generation/deferred/not applicable |

If unresolved business-critical decisions exist, stop before final App Plan unless the user approves defaults.

Executable gate:

```bash
node scripts/validate-business-clarification-gate.mjs --spec <functional-spec.md>
```

The Business Clarification Gate has two modes. Planning mode allows `default-applied-for-planning` so the Functional Specification and App Plan can be created from explicit assumptions. Generation mode blocks `default-applied-for-planning` and requires `answered`, `not applicable`, `user-default-approved-for-generation`, or `deferred` with reason, fallback, and proof/generation impact. It fails when decision gates remain unanswered, pending, TBD, open, require clarification, or when generation is paused until answers are provided. It proves clarification-gate document readiness only, not that the business answer is correct.

Final planning-output rule:

- When any business decision gate remains unresolved, the assistant's final planning response must list every unresolved gate by `Key` and `Question`.
- The response must include the recommended default for each unresolved gate.
- If the response offers an "approve all recommended defaults" option, it must name every gate covered by that option, for example: `Approve all recommended defaults for: approvalRoute, financeThreshold, reminderOffsets, requiredDocuments, permissionModel`.
- The response must include: `No package generation will proceed until the business gates are answered or explicitly user-default-approved-for-generation.`
- Do not shorten the visible clarification list in a way that hides unresolved gates.

## 21. Assumptions

Assumptions, defaults, and deferred decisions:

- Business assumptions:
- Data assumptions:
- Role assumptions:
- Workflow assumptions:
- Reporting assumptions:
- UI assumptions:
- Integration assumptions:
- Defaults applied for planning:
- Deferred business decisions:

## 22. Risks, Constraints, and Unknowns

| Area | Risk/Unknown | Impact | Proposed Handling | Requires User Confirmation |
| --- | --- | --- | --- | --- |
| <Area> | <Risk> | <Impact> | <Handling> | Yes/No |

## 23. Functional Specification Completeness Review

Functional Specification validation checklist:

| Review Item | Status | Notes |
| --- | --- | --- |
| Business context is clear | pass/fail | |
| User roles and responsibilities are identified | pass/fail | |
| Business objects and data requirements are identified | pass/fail | |
| Relationships are identified | pass/fail | |
| Main process is documented | pass/fail | |
| Business process steps, decision points, exceptions, and audit needs are documented | pass/fail | |
| Business rules and status lifecycles are documented | pass/fail | |
| Approval/review needs are documented or explicitly not required | pass/fail | |
| Form/data-entry needs are documented | pass/fail | |
| Workflow/notification needs are documented | pass/fail | |
| Dashboard page content requirements include metrics, calculations, data regions, display fields, filters, sorting/grouping, actions, mobile needs, and alerts | pass/fail | |
| Reporting and audit needs are documented | pass/fail | |
| Document/attachment needs are documented or explicitly not required | pass/fail | |
| AI/integration needs are documented or explicitly not required | pass/fail | |
| Permissions are documented | pass/fail | |
| Business clarification gates are resolved or listed | pass/fail | |
| Assumptions are explicit | pass/fail | |

## 24. Readiness for App Plan

- Ready for App Plan: Yes/No
- Blocking clarification questions:
- Defaults approved by user:
- Requirement document path:
- App Plan should be created after:

Before moving to the Yeeflow App Plan, `validate-functional-specification.mjs` and `validate-business-clarification-gate.mjs` should pass or the unresolved business decisions must be explicitly treated as blockers.

---

<a id="multi-column-form-workspace-standard"></a>

<!-- source: docs/standards/collection-02.md#multi-column-form-workspace-standard -->

# Multi-Column Form Workspace Standard

## Purpose

The `multi_column_form_workspace_shell` pattern is an optional Yeeflow application pattern for operational workspaces that need form-state, action-driven filtering, selected-record detail binding, comments, and dense multi-panel navigation. The Service Desk Pro export proves this pattern on an approval/form surface rather than a dashboard.

Use it for service desk, help desk, CRM workbench, support console, case management, renewal review, triage, review queues, and operational inbox apps where the user works through records without leaving the workspace.

## When To Use A Form Workspace Instead Of A Dashboard

Use a dashboard when the page is primarily reporting, monitoring, KPI review, charting, or table browsing.

Use an approval/form workspace when the page needs one or more of these capabilities:

- form actions that set state variables and refresh controls
- selected-record state shared across panels
- comment or update input controls
- dynamic fields bound to the selected record
- collection item click actions
- form variables that drive query/filter conditions
- field/edit controls that belong on a form surface

Do not add fake workflow logic just to use this pattern. If the workflow exists only as Start to End, document that the approval form is being used as a workspace shell, not as a true approval workflow.

## Layout Anatomy

The Service Desk Pro workspace is closer to a 4-column service desk console than the earlier 3-column dashboard reference:

```text
approval_form_workspace_root
├─ helper/list controls
└─ multi_column_form_workspace_shell
   ├─ left_help_desk_navigation_panel
   ├─ ticket_collection_panel
   └─ selected_ticket_workspace_area
      ├─ selected_ticket_conversation_panel
      └─ right_ticket_details_panel
```

The exported form uses an absolute row container as the visible shell. The first two visible columns are fixed-width containers. The remaining workspace area fills the rest of the page and contains the selected ticket conversation/detail surface and the right attributes panel.

Recommended semantic names:

- `multi_column_form_workspace_shell`
- `left_help_desk_navigation_panel`
- `ticket_collection_panel`
- `selected_record_workspace_panel`
- `right_attributes_panel`
- `workspace_filter_state`
- `selected_record_state`
- `comment_update_area`
- `collapse_expand_action_area`

## Reference Implementation Notes

The Service Desk Pro export shows these reusable mechanics:

- The form page contains a helper `list` control and then an absolute positioned shell container.
- The shell uses row direction with no gap between columns.
- The left navigation panel uses fixed width behavior around `400px`, full-height behavior, and scroll overflow.
- The ticket collection panel uses fixed width behavior around `500px`, full-height behavior, and a scrollable collection body.
- The selected workspace area uses full-height behavior and contains conversation/detail content and the right detail attributes region.
- The ticket collection body uses a CSS flex fill rule equivalent to `flex: 1 1 0`.
- Header/search/filter areas use row alignment, fixed min height, and stable top placement.
- Panel bodies scroll inside their container rather than expanding the full form vertically.
- Navigation rows are containers made from icon plus text controls.
- Collection item rows have click/action behavior and selected visual state.
- The right panel uses dynamic field controls bound to the selected ticket.

## Required Runtime Layout Properties

The Service Desk Pro runtime proves that `multi_column_form_workspace_shell` is not satisfied by four semantic containers. The form workspace shell must reproduce the layout mechanics, or the designer/runtime can squeeze text into narrow vertical columns.

For YAP generation, also apply the YAP approval form workspace standard:

- `docs/standards/collection-03.md#yap-approval-form-workspace-generation-standard`
- `docs/standards/collection-03.md#yap-generation-contract`
- `docs/standards/collection-03.md#yap-export-shaped-application-generation-standard`

The multi-column workspace layout can be visually correct while the package is still too synthetic to materialize. Generated-final YAP workspaces must therefore pass both the form-workspace layout checks and the export-shaped generation contract.

Required shell rules:

- The form workspace shell must not rely on default container layout.
- The form page should hide the standard header when the app chrome already provides context.
- The form container should use full-width behavior and zero page padding for the workspace surface.
- The visible shell is the root workspace container after any hidden helper/list control.
- The shell must define explicit placement and row layout. In the reference this is `common.pos = absolute`, `common.hor = center`, `common.ver = center`, `style.direction = row`, and `style.gap = 0`.
- The shell must define side-by-side columns; it must not let panels flow as generic sequential content.

Required column rules:

- Left navigation panel: fixed width, about `400px` in the reference; full-height behavior; scroll overflow; grey background; stable header row.
- Ticket collection panel: fixed width, about `500px` in the reference; full-height behavior; right border; stable header/filter area; scrollable collection body.
- Selected ticket workspace region: fills the remaining workspace; full-height behavior; column structure with selected-ticket content and a bottom comment/update bar.
- Selected ticket content row: row layout with the conversation/detail workspace filling available width and the right attributes panel fixed around `420px`.
- Right attributes panel: fixed width, full-height behavior, left border, z-index when needed, stable header, and scrollable body.
- Panel bodies should scroll inside their parent. Do not let collection/detail content expand the whole form vertically.

Required control rules:

- Navigation menu items must be horizontal icon/text containers with `direction = row`, `align_items = center`, a small gap, padding, and click/form actions.
- Ticket collection item templates must be stable row/card containers with padding, background/hover/selected-state styling, and a click action that sets selected/current-ticket state.
- Header rows must specify horizontal direction, alignment, spacing, and justification.
- Detail field rows should use horizontal label/value layout with `justify_content = space-between`; long text fields may switch to column layout with enough width.
- Text, heading, dynamic field, and user controls must sit inside containers with enough width; generated pages must avoid any one-character-per-line vertical text risk.
- Icon, Text/heading, Button, and Dynamic field controls should default to inline width behavior in generated form workspaces. For YAP export-style controls, use `attrs.common.positioning.widthtype = [null, "2"]` unless a proven equivalent is used.
- Icon controls should be bounded and context-aware. The successful v8 Service Desk YAP smoke used 16px icons beside 16px menu text with `attrs.icon.size = [null, "--sp--s200"]`, `attrs.common.width = [null, 16]`, and `attrs.common.height = [null, 16]`.
- Generated icons should be specific to the action or menu item, such as inbox, at, pencil, users, flag, folder, search, ellipsis, plus, or chevron. Generic placeholder icons are a generated-final failure.
- Comment/update controls belong in a bottom bar with row direction, `margin-top: auto` behavior, a top border, and safe selected-record binding.

Required YAP materialization rules:

- Use `Data.Forms[].ListID = 0` for Service Desk Pro-style approval/form workspace entries.
- Add a root Type `105` navigation item pointing to the form key.
- Align `Resource.FormKeys[]`, `Data.Forms[].Key`, Type `105` navigation `ListID`, and embedded `DefResource.defkey` / `DefResource.key`.
- Put the requester workspace form in `DefResource.pageurls[].formdef.children`; do not use unsupported `formdef.controls`.
- Use export-style lowercase controls.
- For YAP Service Desk-style generated workspaces, use only designer-load-proven controls unless an exact export-proven control shape is available. The successful v8 smoke loaded with `list`, `container`, `heading`, and `icon`; generated `collection` and `dynamic-field` approximations later caused the approval form designer to keep loading.
- Keep a minimal complete workflow graph: `StartNoneEvent -> SequenceFlow -> EndNoneEvent`.
- Set workspace page settings: hide default form header, content width Full width, and padding `0`.
- Preserve the v8-proven child-list import shape: native `Title`, business fields starting at `Text2`, record-ID-keyed `ListDatas`, sample row keys that exactly match `Defs[].FieldName`, and stringified field `Rules`.
- Rebuild `Resource.ReplaceIds` from the final decoded package after all generation and repair steps. Include generated root/list/field/layout/form/process/workflow/sample IDs; stale baseline `ReplaceIds` can pass API queue acceptance and still fail async import.
- Generate from a bundled sanitized export-like baseline/reference shape when no user-provided safe export is available. If no safe baseline exists, do not claim import-qualified output.
- Parse runtime-critical strings before output: `DefResource`, field `Rules`, layout/view `Ext1` / `Ext2` / `Ext3`, `LayoutView`, and form `Settings` where present.

Required overflow rules:

- The outer multi-column shell should use Hidden overflow where it owns the full workspace bounds.
- Each column should have independent Scroll or Auto overflow.
- Inner body regions such as ticket collection, selected ticket body, and attributes should scroll independently when content exceeds the panel height.
- Do not rely on Default overflow for multi-column workspace columns.

Property checklist for generated packages claiming this pattern:

- root shell width/height or root placement strategy
- shell direction
- shell align-items when used by direct child rows
- shell justify-content when used by direct child rows
- shell gap
- shell overflow or bounded child overflow
- panel width
- panel height
- panel overflow
- panel padding
- panel margin
- control width
- control shrink/wrap behavior
- icon inline width
- icon bounded size
- Text/heading, Button, and Dynamic field inline width
- collection item layout
- selected record binding
- form action binding

If these properties are missing, the generator must not claim conformance to `multi_column_form_workspace_shell`, and validators should fail the package before signing or install.

## Action-Driven Filtering

Navigation and filters are not just visual labels. The reference uses form actions to set variables and refresh controls:

| UI element | Trigger | Form action | Variables affected | Target control/query | Runtime behavior |
|---|---|---|---|---|---|
| Your inbox | menu container click | nav action | current filter state and current user values | ticket collection query | show tickets relevant to current user |
| Mentioned you | menu container click | nav action | mentioned-user filter values | ticket collection query | show tickets where current user is mentioned |
| Created by you | menu container click | nav action | created-by filter values | ticket collection query | show user-created tickets |
| All tickets | menu container click | nav action | filter mode reset or all-ticket filter | ticket collection query | show all tickets |
| Unassigned | menu container click | nav action | assigned/team filter values | ticket collection query | show unassigned tickets |
| Flagged | menu container click | nav action plus flagged-ticket lookup state | ticket collection query | show flagged tickets |
| Search icon | icon/container click | show or hide search action | keyword filter variable | search filter and ticket collection | expose keyword search and refresh results |
| Priority/Status filters | filter control change | filter variable update | priority/status filter variables | ticket collection query | narrow visible ticket collection |
| Ticket item | collection item click | selected record action | selected/current ticket variable | middle and right panels | refresh selected ticket details |
| Comment submit | action button click | set data list plus refresh action | comment fields and selected ticket reference | ticket activities list | create an activity/comment for the selected ticket |
| Collapse/expand icon | icon/container click | show/hide panel action | panel visibility variables | navigation/details panels | collapse or reopen workspace panels |

Use readable aliases in docs and generated plans. Do not expose tenant, workspace, package, upload, or record IDs.

## Generation Rules

- Build the workspace on an approval/form surface when the app needs form actions, variables, comments, selected-record state, or dynamic field controls.
- Do not represent a no-submit workspace as a normal request form.
- Do not add a fake Submit Request button when no request submission is intended.
- If the workflow is only Start to End, keep it minimal and document that the form is used as a workspace shell.
- Navigation menu items should be containers with icon and text controls, and active items should have click/form actions.
- Filter actions should set variables that are consumed by collection query/filter conditions.
- Collection items should set selected/current record state.
- Details and attributes panels should bind to the selected/current record.
- Comment or update controls should write to a related activity/update list only when the data mutation is safe and validated.
- Expand/collapse icons should be action-bearing containers or icon controls with bounded inline sizing.
- Avoid placeholder-only final panels and designer drop-zone text.
- For YAP smoke packages, prefer static designer-safe ticket cards and detail headings until collection/current-record controls are copied from a matching export-proven approval workspace. Do not claim dynamic selected-record behavior when the package uses static designer-safe placeholders.

## Validation Checklist

Apply these checks only when the app/page declares `multi_column_form_workspace_shell`, `four_column_service_desk_workspace`, `service_desk_inbox_workspace`, or an equivalent operational inbox pattern:

- A form workspace surface exists.
- The shell has side-by-side panel mechanics and does not stack panels vertically.
- The shell has explicit runtime layout properties: placement, row direction, gap, full-height column behavior, and bounded overflow.
- The implementation is not default-container-only.
- Left navigation, ticket/list collection, selected workspace, and right attributes/detail regions are identifiable.
- Left navigation, ticket collection, selected workspace, and right attributes columns have explicit width behavior.
- Panel body regions have explicit scroll/overflow behavior.
- Navigation menu items use horizontal icon/text layout.
- Ticket collection item templates use stable row/card layout.
- Navigation menu containers have action bindings.
- Filter variables or set-variable actions exist.
- Collection/query filtering is detectable.
- Collection item click actions set selected/current record state.
- Details panels contain dynamic selected-record bindings.
- Icon controls used for collapse/action behavior are inline, bounded, and action-bearing.
- Icon, Text/heading, Button, and Dynamic field controls use inline width behavior.
- Icons match nearby text size and use contextual symbols rather than generic placeholders.
- Text and field controls do not have obvious vertical-text or 0px/narrow-width risk.
- Each multi-column workspace column has its own Scroll or Auto overflow; the shell has Hidden overflow when it bounds the page.
- The form does not contain a fake request-submit button.
- The workflow is not misrepresented as real approval routing when it is only Start to End.
- No final panel is placeholder-only.
- YAP child-list sample rows are keyed by row ID and all row fields resolve to `Defs[].FieldName`.
- Generated YAP workspace packages have complete final-package `Resource.ReplaceIds` coverage for generated local app/list/field/layout/form/process/workflow/sample IDs.
- Service Desk YAP field numbering follows the v8-proven `Title` plus `Text2...` pattern rather than generated `Text1` or `Lookup1` drift.
- YAP approval form controls avoid generated `collection` / `dynamic-field` shapes unless the exact shape is export-proven for this host.

## Relationship To The Three-Column Dashboard Standard

The three-column dashboard standard is layout-focused and uses dashboard Style 2 when `three_column_workspace_shell` is the root dashboard body container. The multi-column form workspace standard is behavior-focused and uses an approval/form surface for stateful operational consoles.

Common reusable mechanics:

- multi-column shell
- fixed/fill/fixed panel behavior
- scrollable panel bodies
- stable header/action areas
- icon plus text navigation rows
- list or collection panel
- selected record workspace
- right detail or attribute panel

Different responsibilities:

- Dashboards are best for reporting and monitoring.
- Form workspaces are best for action-driven state, comments, selected-record editing, field controls, and form actions.

## Runtime Risks

- API import success is not proof that stateful panel interactions work.
- API `Status = 0` with an action-log response is not proof that the async import completed; import can still fail later if child-list sample rows, field numbering, or designer form controls drift from export-proven shape.
- A visually correct shell can still fail if nav actions do not update filter variables.
- Selected-record panels can appear static if collection item actions do not set the expected variables.
- A package can import and publish but leave the approval form designer loading if generated form controls are not designer-load safe.
- Comment/update controls can mutate the wrong list if selected-ticket binding is not validated.
- Fake workflow routing can mislead users and should be avoided.
- Icon controls that render full-width or oversized can distort narrow panels.

---

<a id="native-collection-density-standard"></a>

<!-- source: docs/standards/collection-02.md#native-collection-density-standard -->

# Native Collection column width and text density

## Evidence and scope

Native Dashboard table columns support `attrs.tablecols[i].attrs.cw` (responsive numeric value) and `cwu` (responsive unit). User-authored Designer samples plus persisted readback established `[null,160]` for 160px with the default unit omitted and `[null,15]` / `[null,"%"]` for 15%. Native `dynamic-field` Text length is `attrs["t-len"] = [null,180]`. This is NOT the Text/heading `attrs.headc.title["text-len"]` property. `attrs.width` or `minWidth` alone is not the proven Column width mapping.

Source-list bound Dashboard updates were saved, read back and observed on desktop. Percentage columns resolved to container-relative pixel col widths. Native text length reduced excessive narrative row height. These observations do not prove alternate hosts, mobile cards, every breakpoint, multilingual truncation, or form submissions. Existing functions, paging and data remained unchanged.

## Generation contract

Use `scripts/lib/collection-native-density.cjs` through the actual Dashboard materializer. Apply density AFTER the Collection is placed in its page slot, with resolved source fields. A full page does not imply that each Collection has full page width. Row siblings and Grid ancestry constrain the slot. Unknown or constrained slots use deliberate pixel widths and retain native horizontal scrolling. Full-width slots with up to eight columns use percentages totaling 100 unless explicit native widths take precedence. More columns use readable pixel widths instead of squeezing all content into one screen.

Widths reflect field roles and choices: Title and narrative receive more room, people/dates moderate room, choice/numeric fields compact room, operation/selection columns narrow room. Choice option lengths come from schema, not only the first page of records. General text and lookup labels must not be assumed short. The helper uses conservative starting widths; the runtime reviewer must refine them from actual content and container measurements. Do not turn sample 160px/15% values into universal defaults. Do not globally force a fixed row height.

New sample-derived business columns discard reference widths and text limits before binding; these values belonged to sample fields. Preserve native column IDs, selection-column shape, sorting, item context and actions. New generation defaults ordinary record Collections to 10 per page. Do not rewrite card, Kanban, Flex Grid or Data Table layouts with the native table policy. Preserve the separate mobile card subtree; desktop width settings are not mobile proof.

The automatic pass covers newly materialized standard responsive record tables. Pre-embedded master-detail selection/detail collections and print-specific layouts retain their separate policies and are not automatically replanned by this pass.

For existing applications, the helper is a no-op unless `mode:"existing", authorized:true` expresses the user's scope. Existing device overrides and text limits stay in place unless specifically changed. Never replace unrelated CSS, filters, actions, data, paging, or source schemas to adjust density.

## Native text length and complete information

Only narrative fields such as Next Action, Blocker, Failure Reason, description/notes, or schema-confirmed textareas qualify for default display limits. Default Next Action is 180 and other narratives 160 when a complete-value route is established. Keep Title, identity/reference/receipt/URL, people, dates, numbers and ordinary unknown text complete. `Text lengths` may use `"full"` to explicitly keep full display.

Text length is NOT a line clamp and cannot promise exactly three lines or a fixed row height. It changes displayed text, not stored data. The native tooltip may contain the truncated value too; do not claim hovering reveals the full text. Keep the source list or a verified record-detail route accessible. No verified route means no automatic truncation. Never fabricate a detail action from a button's label. User-approved clipping must preserve an independent full-value path.

## Plan and overrides

The Collection selection table in App Plan section 14, also used by standalone Dashboard generation, accepts these optional columns:

| Dataset region | Source list | Selected template | Display fields | Column widths | Text lengths | Full value access |
|---|---|---|---|---|---|---|
| Work queue | Work Items | collection_control_responsive | Title, State, Next Action, Due Date | {"Title":"30%"} | {"Next Action":180} | source-list |

`Column widths` and `Text lengths` are JSON objects keyed by exact resolved field name or display name. Use `"120px"` / `"15%"` width strings and positive integer lengths or `"full"`. A partial percentage plan allocates the remaining percentage by field role; invalid budgets fail. Unknown fields, conflicting aliases, invalid values and ambiguous partially specified mixed units fail before generation. Complete intentional mixed-unit plans preserve native intent and require runtime checking.

`Full value access` is `source-list`, `record-detail`, or `none`. For new generated Data Lists with a resolved list ID and detail layout, the materializer defaults to the source-list route; this is not a claim that the table Title is clickable. `record-detail` must be confirmed by the functional plan. `none` disables automatic truncation and rejects explicit clipping. Missing route evidence appears as a density warning rather than silently hiding content.

## Validation and training acceptance

New materialized native Collections carry `collectionDensityPolicy.version = 1`. The public Collection gate requires native widths for that policy, checks responsive shapes, numeric positive widths, supported units, percentage budget and text lengths. Historical missing widths remain advisory unless `requireNativeCollectionDensity` is explicitly requested; old specimens must not be retroactively declared product-invalid.

Run `node scripts/test-collection-native-density.mjs` and the native-rich and full UI suites in source and distribution. Tests must exercise Markdown plan parsing through the real builder, full and constrained placement, role-aware allocation, explicit overrides, narrow operation columns, mixed-width rejection, wrong schema keys, unbounded identifier/title values, missing full-value route, existing-instance no-op, unrelated configuration and device preservation, and corrupted generated artifacts through the public validator. Helper-only tests are insufficient.

For live acceptance: snapshot first; save; allow bounded readback reconciliation for asynchronous persistence without immediately repeating writes; verify exact scope and persisted content; wait for each Collection's query to settle; then measure actual col widths, table/container overflow and row heights. A transient zero-row DOM is not proof of no data. Re-test search/clear, sort, paging and record routes. A successful input fill does not prove the controlled input changed. Record desktop and mobile proof separately.

Native Column width and Text length are first choice. Custom CSS is a scoped fallback only after an observed native limitation, never the default width solution. Source/generator tests do not constitute a new live application acceptance or plugin publication.

---

<a id="native-collection-rich-content-training"></a>

<!-- source: docs/standards/collection-02.md#native-collection-rich-content-training -->

# Native Collection rich content training

## Scope and proof

This learning round uses a previously saved/read-back Dashboard and desktop browser observations. It is a narrow live-update baseline, not a fresh-import or all-host baseline. Reusable files contain no tenant payloads or business rows. Local fixtures are synthetic.

Preserve native `attrs.tablecols` for desktop/tablet and the separate mobile `children` item tree. Never replace a native Collection with a fabricated paired Flex Grid merely to style it. Preserve list binding, fulltext, sort, pagination, local actions, current-item context, and unrelated page regions.

## Generation recipe

Choice-field composition and new Dashboard 10-record pagination follow `docs/standards/collection-01.md#dashboard-collection-choice-pagination-standard`. Existing single-choice badge helpers are reusable; multi-choice membership and visual composition have separate proof requirements. The materializer applies pagination only to eligible new datasets and respects explicit plan values.

1. Inventory actual fields and existing functions before interpreting an image. A pictured field, upload, progress calculation, or selected-detail pane is not evidence that it exists. Compare the returned image against the approved toolbar order too.
2. Keep the required content_card_wrapper under section_content_area. A Collection with its own caption does not need a repeated section_title_area. Preserve unique business descriptions if they carry information.
3. For single-select badges, use `buildCollectionNativeBadge` from `scripts/lib/collection-native-badge.mjs`. Supply resolved field names, actual values, reviewed palettes, and a page-wide unique ID allocator. The helper is opt-in; do not convert ordinary text fields into badges automatically.
4. The generated Container owns control_display rules targeting its own ID. Each rule compares a variable_ctx expression in __ctx_coll against an exact value and applies style_class/action_style. The Text binds the real field and inherits color. Unknown values remain visible using the neutral base style; never relabel or rewrite records to match a mockup. Empty values do not imply a business status.
5. Give newly materialized native columns stable identities, preserving existing IDs. Review missing/conflicting identities with the dashboard collection inspector. Historical omissions are advisory, not a hard claim of invalid product schema.
6. Include both tablecols descendants and ordinary children in control ID/binding/action audits. Desktop success is not mobile success.

## Runtime-sensitive styling

For toolbar height/alignment and row-menu clipping/contrast, follow `docs/standards/collection-01.md#collection-toolbar-style-standard` in this directory and the opt-in `scripts/lib/collection-toolbar-style.mjs` helpers. Apply custom CSS to the owning Button or Select, not a broad card-level button selector. The native-rich training suite includes the focused toolbar regression.

The earlier 200px equal-column failure came from using incorrect `attrs.width` / `attrs.minWidth` mappings. Designer samples and later persisted desktop verification resolved the native mapping: `tablecols[i].attrs.cw/cwu`. Dynamic field Text length is `attrs["t-len"]`. Follow `docs/standards/collection-02.md#native-collection-density-standard` and the integrated generator; prefer native properties before CSS. Keep sample values separate from content-aware design choices.

An observed fallback is `attrs.common.css` scoped with `selector`, using `.ant-table-thead>tr>th`, `.ant-table-tbody>tr>td`, and `table.ant-table-fixed` / col selectors. This can style header density, horizontal separators, hover, and widths. Keep it scoped to the target card, derive widths from its real columns, preserve narrow-screen scrolling, and verify DOM measurements after reload. Do not ship tenant-specific CSS classes or promote these implementation selectors as a stable platform API. Native style support and this fallback are distinct claims.

## Required acceptance matrix and open learning

| Area | Established observation | Still required before broader claims |
|---|---|---|
| Badge | Saved/readback and desktop colored labels | All palettes, unknown/empty values, alternate hosts and mobile |
| Filters | Search, displayed Status/Owner/Channel selections, combinations and individual clears worked | Exhaustive options, values occurring only after first source page, duplicates, empty owner, remote search and no-match recovery |
| Owner | Visible identity label and combined equality worked | DOM title was [object Object]; accessibility and value serialization need explicit coverage |
| Add | Existing New/Edit form opened and was cancelled | Submission/persistence not tested by this design pass |
| Edit | Existing Edit item route opened a View layout | Inspect actual action mode, target-owned layout and editable form controls; a label is not execution proof |
| Selection | Hover tint | Persistent selection and linked-detail state are separate capabilities |
| Responsive | Native mobile subtree preserved | Desktop rich fields/styles and mobile controls must be compared and tested independently |

Do not infer complete filter candidates from a successful selection: observed candidate lists omitted a status present elsewhere in the page summary. Cause (pagination, configuration, or other runtime behavior) is unresolved. Do not change source records or invent a pagination property to hide this gap.

Run `node scripts/test-collection-native-rich-training.mjs`, the Collection generation tests, responsive routing tests, and dashboard grid-table tests. Run equivalent distribution tests. Retain separate labels for local validation, persisted readback, rendering, and business action execution. Training this document does not publish a plugin or prove the open cases.

## Training validation result

Release 1.15.5 resolves the earlier preflight failure below: two synthetic Dashboard Search controls inherited attrs.binding from the responsive template, while the canonical exported path is control.binding. The generator now normalizes legacy placement and rejects conflicting values. Regression tests verify declarations and fulltext consumers. Source and distribution full-app tests pass; both complete UI suites pass 29/29. The earlier failure paragraph is retained as historical diagnosis, not the current release status.

The new badge/identity/traversal regression and the existing Collection generation, responsive routing, and dashboard grid-table suites passed in source and plugin distribution. The aggregate UI suite passed 14/15 source suites and 13/14 distribution suites. Both failed the full-app materialization entrypoint preflight at `dashboard-select-filter-runtime-contract` with `FILTER_VARIABLE_MISSING`. Causality is not established; do not report aggregate readiness or release readiness until this failure is investigated. Source and distribution had a pre-existing difference in aggregate suite membership; this round preserves that difference.

This round changes source and distribution learning assets only. It does not merge, tag, publish, install a new version, or run new live tenant mutations. The open capability matrix remains an explicit continuation backlog.

---

<a id="public-form-form-action-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#public-form-form-action-golden-reference-standard -->

# Public Form Form Action Golden Reference Standard

## Scope

Anonymous Data List Public Forms have a smaller Form Action surface than Custom Data List forms. They cannot read or mutate other application resources. Their action expressions are limited to fixed values, fields on the current host Data List that are allowed on Public Forms, and temp variables declared on the same Public Form.

## Allowed Steps

The product UI allows exactly these eight capabilities: Set variables, Execute custom code, Show confirm dialog, Redirect page to, Submit form, Start another action, Barcode scan, and NFC reader.

`setvar` and `redirect` are export-proven by `Customer Satisfaction (2).ydl` and have normalized templates in `docs/reference/public-form-form-action-set-variables.template.json` and `docs/reference/public-form-form-action-redirect.template.json`. `confirm`, `submit`, and `otheraction` reuse established Form Action step shapes but still require Public Form runtime proof. Execute custom code, Barcode scan, and NFC reader are product-UI-backed only in this training; their exact Public Form package serialization must not be guessed. Generated-final materialization must stop with `PUBLIC_FORM_ACTION_STEP_SERIALIZATION_UNPROVEN` until a decoded export proves each shape.

All other Form Action step types are forbidden, including Query data, Set data list, Open item form, Open approval form, Open dashboard, Invoke custom service, and application-resource navigation or mutation steps.

## Set Variables

- Targets: current host Data List fields allowed on Public Forms, or same-form temp variables.
- Values: fixed expression tokens, current host Data List field tokens, and same-form temp-variable tokens.
- Single assignment uses `attrs.setvar_var` plus `attrs.setvar_val`.
- Multiple assignments use `attrs.setvar_multi = true` plus `attrs.setvar_array[]`.
- Application, instance, current-user, workflow-variable, Collection-item context, lookup traversal, or any other resource scope is forbidden.

## Redirect Page To

`attrs.link` may contain a fixed `url`, or an expression-token array in `variable`. Expression URLs may combine only fixed values, current-list fields, and same-form temp variables. Redirect must not resolve an application resource by ID or consume external-resource data.

## Local Action References

`formAction` hooks, control `attrs.control_action`, and `otheraction.attrs.control_action` must resolve to an action ID on the same Public Form. Cross-form and cross-page action invocation is forbidden.

## Custom Data List Form Difference

Custom Data List forms may use broader application-aware actions such as Query data, Set data list, and Open resource steps. Public Forms are anonymous and isolated: sharing the same generic page/action JSON container does not grant those capabilities. Generation and validation must route by host surface first.

## Required Gates

Before signing or standalone `.ydl` handoff, validate the eight-step allowlist, action references, current-list field resolution, temp-variable declarations, Set variables targets and values, Redirect URL/value scope, and the export-proof boundary for screenshot-only steps.

---

<a id="service-portal-generation-standard"></a>

<!-- source: docs/standards/collection-02.md#service-portal-generation-standard -->

# Service Portal Generation Standard

This standard is based on `Supplier Management-v1.2.yapk`, a current Yeeflow export that includes a configured Service Portal.

Proof boundary: the structure below is export-proven and validator-backed. New generated Service Portal packages still require focused install, Version Management, portal-login, navigation, resource-access, and permission proof before being called runtime-proven.

## Planning Rules

Service Portal is an advanced app-level capability and must be explicitly planned before package generation.

The App Plan must include a `Service Portal Planning` section with:

- whether a Service Portal is planned;
- portal name, domain/subdomain intent, icon/logo intent, and appearance/navigation colors;
- login/authentication assumptions and any login-page copy or legal links;
- portal user groups to create;
- exposed application resources;
- portal navigation menu;
- per-resource access for each portal user group or special audience;
- list/document-library views exposed through portal permissions;
- list form routing for add/edit/view when the portal should use non-default forms;
- proof boundary and runtime test plan.

If Service Portal is not planned, `PortalInfo` must be `null` in YAPK output. Do not emit `{}` or `[]`.

## Resource Eligibility

Service Portal can expose only resources from the same application:

- Data List resources: child `List.Type = 1`;
- Document Library resources: child `List.Type = 16`;
- Dashboard pages: `Pages[].Type = 103`, referenced by `LayoutID`.

Do not expose Approval forms, Form reports, Data reports, workflow-only resources, Custom Services, AI Agents, or hidden support resources unless a focused product export proves the shape and the App Plan explicitly requests it.

## YAPK Shape

The Service Portal lives in top-level decoded `AppPackageInfo.PortalInfo`.

Observed required object shape:

- `ID`
- `Type`
- `Name`
- `Description`
- `IconUrl`
- `LogoUrl`
- `Settings`
- `Flag`
- `Status`
- `DefaultGroupId`
- `Domain`
- `Groups`
- `Resources`
- `Perms`

The root `ListSet.Ext3` should include:

```json
{
  "externalPortal": {
    "id": "<PortalInfo.ID>"
  }
}
```

`PortalInfo.Settings` is a JSON string. The sample proves these keys:

```json
{
  "navigator-menu": {
    "position": "left",
    "hover": { "color": "var(--c--background)" },
    "bgc": "var(--c--primary-dark-hover)",
    "color": "var(--c--background)",
    "active": { "color": "var(--c--background)" }
  },
  "appearance": {
    "bgc": "var(--c--primary-darker)",
    "color": "var(--c--background)"
  }
}
```

The screenshot also shows login page customization fields, privacy/terms links, and authentication method options. Those values were not found in the decoded sample `PortalInfo.Settings`; treat their export location as unknown until another focused export proves it.

## Portal User List

The sample includes a Type `128` child list for external portal users:

- `List.ListID` equals `PortalInfo.ID` in the sample.
- `List.Title` follows `__external_portal_user-<suffix>__`.
- `List.TableCode = "userinfo"`.
- `List.Type = 128`.
- `List.Flags = 2`.
- `List.LayoutView.add/edit/view` point to the `__userinfo__` Type `1` layout.
- Exported user items were empty; actual portal user credentials/passwords are not proven to migrate through this package.

Observed portal user fields:

- `AppcenterID`
- `Flag`
- `Name_CN`
- `SPAccount`
- `SPAccount_Short`
- `DepartmentID`
- `Email`
- `Photo`
- `Deleted`
- `DelTime`
- `LastLoginTime`

Generated apps should not fabricate real external users, passwords, login sessions, or portal user membership records unless the user provides an explicit safe migration scope and a focused export proves how those rows should be serialized.

## Portal Groups

Portal user groups live in `PortalInfo.Groups[]`.

Observed shape:

```json
{
  "ID": "<long id>",
  "Code": "<uuid>",
  "Name": "Standard users",
  "Description": null
}
```

Use API-issued/package-local IDs consistently and preserve group IDs for portal permissions. Use UUID-like `Code` values for generated groups.

## Portal Resource Registry

`PortalInfo.Resources[]` contains several typed configuration objects.

### Portal Source

`Category = "portal"`, `Type = "resource"`, `Title = "portal source"`.

`Content` is a JSON string:

```json
{
  "lists": [
    { "id": "<child ListID>", "type": 1, "title": "Supplier List", "isHidden": false },
    { "id": "<child ListID>", "type": 16, "title": "DUCS", "isHidden": false }
  ],
  "dashboards": [
    { "id": "<dashboard LayoutID>", "type": 103, "title": "Home page", "isHidden": false }
  ]
}
```

Every `lists[]` entry must point to an existing child list/document library and every `dashboards[]` entry must point to an existing dashboard page.

### Portal Menus

`Category = "portal"`, `Type = "menus"`, `Title = "portal menus"`.

`Content` is a JSON string:

```json
{
  "menus": [
    { "id": "<LayoutID>", "type": 103, "title": "Home page", "isHidden": false, "icon": "fa-regular fa-house" },
    {
      "id": "<uuid>",
      "type": "classes",
      "title": "Data management",
      "icon": "fa-regular fa-database",
      "childs": [
        { "id": "<ListID>", "type": 1, "title": "Supplier List", "isHidden": false, "icon": "fa-regular fa-grid-4" }
      ]
    }
  ]
}
```

Visible menu items and groups should carry suitable FontAwesome icons. Hidden resource entries may omit icons.

### Portal List Forms

Portal list form settings use one `PortalInfo.Resources[]` object per configured list:

- `Category = "41_<target child ListID>"`;
- `Type = "listforms"`;
- `Content.forms[]` maps `add`, `edit`, and `view` to `default` or a same-list Type `1` custom form layout ID;
- `opentype.open` and `opentype.size` must be preserved from the planned portal UX.

Example:

```json
{
  "forms": [
    { "type": "add", "id": "default", "opentype": { "open": "current", "size": -1 } },
    { "type": "edit", "id": "<same-list custom form LayoutID>", "opentype": { "open": "current", "size": -1 } },
    { "type": "view", "id": "<same-list custom form LayoutID>", "opentype": { "open": "current", "size": -1 } }
  ]
}
```

## Portal Permissions

Portal permissions live in `PortalInfo.Perms[]`.

Observed shape:

```json
{
  "ID": "<permission id>",
  "ItemType": 1,
  "ItemID": "<child ListID>",
  "PermType": 3,
  "PermObjID": "<PortalInfo.Groups[].ID or 999999>",
  "Perm": 25,
  "Ext": "{\"views\":[\"<LayoutID>\"]}"
}
```

Rules:

- `ItemType = 1` references a Data List or Document Library child ListID.
- `ItemType = 2` references a Dashboard page LayoutID.
- `PermObjID` must reference `PortalInfo.Groups[].ID` or the observed special portal audience `999999`.
- List/document-library permission rows should include `Ext.views[]` with allowed view LayoutIDs.
- Dashboard permission rows may have empty `Ext`.
- Observed `Perm` values are `1`, `25`, and `249`; the exact bitmask semantics are not fully proven from this single export, so generation must use explicit mapped permission presets only when product meaning is confirmed or user supplies an export-backed mapping.

## Portal-Aware Dashboard/List Binding

The sample includes one dashboard Collection data source with:

```json
{
  "AppID": 41,
  "ListID": "<portal-exposed child ListID>",
  "Type": 1,
  "Title": "<resource title>",
  "ListSetID": "<root app ListSetID>",
  "ExtPortal": {
    "id": "<PortalInfo.ID>"
  }
}
```

When a dashboard/page inside a portal needs portal-aware resource access, preserve `attrs.data.list.ExtPortal.id`.

## Validation Rules

Before signing readiness for any generated package with Service Portal:

- `PortalInfo` is object when planned, `null` when not planned.
- `ListSet.Ext3.externalPortal.id` matches `PortalInfo.ID`.
- Type `128` external portal user list is present or explicitly deferred with proof boundary.
- `PortalInfo.Resources` includes source registry and menus.
- Exposed resources are only Type `1`, Type `16`, or Type `103`.
- Every source/menu/permission ID resolves to a current generated resource or portal group.
- `PortalInfo.Perms[].Ext.views[]` resolves to same-resource views.
- Portal listforms target same-list Type `1` layouts or `default`.
- Visible portal menu entries include FontAwesome icons.
- No Approval forms or Form reports are included in portal source, menus, or permissions.

Use:

```bash
node scripts/inspect-service-portal-yapk.mjs --package <package.yapk> --json
```

## Unknowns / Needs Focused Runtime Proof

- Exact export location for login page customization text, legal links, and login authentication options was not found in this sample's decoded `PortalInfo.Settings`.
- The semantic meaning of `Perm` values `1`, `25`, and `249` needs product confirmation or a matrix export.
- Whether generated packages should create portal user rows is unproven; current safe default is to create the portal user system list and portal groups, not real user credentials.
- Runtime behavior of newly generated Service Portal packages still needs focused install/open/login/resource-access/export-back proof.

---

<a id="set-variable-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#set-variable-golden-reference-standard -->

# Set Variable Golden Reference Standard

Proof boundary: export-proven from the manually configured `Business Travel Request-v1.1.yapk`; generator and validator backed after the focused regression suite. Runtime mutation remains a separate proof.

The Approval Form Form Action, Approval workflow, Data List workflow, field-change action, Start another action, and current-list update additions are export-proven from `Business Travel Request-v1.2.yapk`.

## Capability Decision

Use Set variable when page or workflow logic must assign a known value or expression result to an existing variable/field. Do not use it to query records, persist unrelated resources, or create undeclared variables.

- Dashboard Form Actions may target declared page `tempVars` only.
- Data List Form Actions may target declared page `tempVars` or fields on the current Data List record.
- Approval Form Actions may target declared approval/page variables supported by that form context; they must not use Data List `list_field` targets.
- Approval, Data List, and Scheduled workflows use `SetVariableTask.properties.variablesetting[]`. Workflow nodes have no single/multiple toggle; one versus many is represented only by array length.

## Form Action Serialization

Single assignment:

```json
{
  "type": "setvar",
  "attrs": {
    "setvar_var": { "exprType": "variable", "id": "__temp_var_Status", "type": "expr" },
    "setvar_val": [{ "type": "str", "value": "Active" }]
  }
}
```

Multiple independent assignments use `setvar_multi: true` plus non-empty `setvar_array[]`, each with `var` and expression-token-array `value`. Do not use one multi step when assignment B depends on the newly assigned value from assignment A; use ordered steps.

Page-load actions bind through `formAction.onLoad`. Button/container clicks and Collection actions bind through a resolvable `attrs.control_action`. Every target temp variable must be declared on the same page/form.

Approval Form field-change actions bind through the source field control's `attrs.control_event_rule`. A Form Action `otheraction` step starts another action through `attrs.control_action`; that target must resolve to another action on the same page. The v1.2 Requester example proves this chain: Page Load sets `Requester = currentUser()`, assigns page temp variables, starts the department action, and the Requester field invokes the same department action after user changes.

Approval Form Set Variable may target workflow/form variables such as User, Department, Text, and Number, or declared page temp variables. User organization attributes use nested export-proven expressions such as `getUserAttr(Requester, DepartmentID)` and `getOrgAttr(getUserAttr(Requester, DepartmentID), Name)`. Derived Department and Department Name controls should be read-only.

Data List field targets use `exprType: "list_field"` and a real current-list field ID/prop. Conditional steps use an expression-token-array `condition`. `continue: true` means continue to the next step when the condition is not met; serialize it only as a boolean. The Domestic/International allowance example requires the first conditional step to continue so the second condition can be evaluated.

Dynamic Display may consume a temp variable assigned by Set variable. The producer target ID, page `tempVars` declaration, and consumer expression ID must resolve to the same variable. This proves structural linkage, not runtime behavior.

Editable/read-only Dynamic Display uses `attrs.control_display[].actions.attrs.style_regulation_action = "style_regulation_action_editable"` for the true condition. Every rule's `controlId` must equal the containing control's `id`; do not copy another control's ID when cloning a rule.

For plan-driven Approval field generation, `Read Only = Yes` is materialized directly. A generated Dynamic Display rule requires an exact JSON rule array in the field plan; the shared builder replaces each planned `controlId` with the newly generated containing control ID. Prose-only Dynamic Display requirements are not silently converted into guessed expressions.

## Workflow Serialization

Every `SetVariableTask` assignment must include `idx`, `id`, `name`, `type`, `editable`, and expression-token-array `value`. Supported RHS patterns in the focused export include:

- fixed string/number/boolean/date values;
- `now`, `dateAdd`, `iif`, `currentUser`, `getUserAttr`, and `isInGroup` functions when valid for the host;
- another declared workflow variable;
- instance Applicant expression.

The target must be a declared workflow variable with matching ID/type metadata. Scheduled Workflow uses the same `SetVariableTask` contract as Approval and Data List workflows.

## Data List Workflow Boundary

In a Data List Workflow, `SetVariableTask` can assign only declared workflow variables. It may read the current Data List field on the RHS using an export-shaped `exprType: "list_field"` token, but it cannot use that field as the assignment target.

To update a current Data List item field, use `ContentList` / Set Data List with `properties.listtype = "current"` and `properties.listdatas[]` mappings. Each mapping target must resolve to a field on the workflow host list. To update another list, use the selected-list shape with exact application/list metadata and filters. Never reinterpret a planned field mutation as Set Variable.

## App Plan Contract

Form Action rows must use `Form Action Set Variable Planning` and name action, ordered step, host, trigger, target kind, target ID, exact JSON RHS expression tokens, condition, continue behavior, bound control, action-chain target, and consumer. Rows with the same action and step order become one multi-variable step. The shared materializer applies this contract to Approval submission/task pages, custom Data List forms, and Dashboard pages; standalone and full-app generation must call the same helper. Workflow node rows use `Set Variable Assignments` with this machine-readable grammar:

```text
VariableId :: variableType :: Business Name :: JSON expression token array ;; next assignment...
```

Never materialize an empty placeholder assignment when the plan omits exact assignments. Emit an empty `variablesetting[]` and fail validation so the plan can be corrected.

Form Action Host Type is a shared planning/materialization contract, not free text. Normalize these aliases before matching records:

- `Approval`, `Approval Form`, `Approval Submission`, `Approval Submission Form`, `Approval Task Form`, and `Task Form` -> `approval`.
- `Data List Form`, `Custom Data List Form`, and `Document Library Form` -> `data-list-form`.
- `Dashboard` and `Dashboard Page` -> `dashboard`.

Unknown Host Type values fail with `FORM_ACTION_SETVAR_HOST_TYPE_UNSUPPORTED`. When the same Host Resource and Host Page resolve to a different canonical host than the shared builder call, fail with `FORM_ACTION_SETVAR_HOST_TYPE_MISMATCH`; never silently skip a planned Set Variable action.

`SetVariableTask` is a system workflow action. It must carry valid `variablesetting[]`, but it must not be given fake assignee or task-page metadata. Approval assignee and `taskurl` gates apply only to human task stencils such as `MultiAssignmentTask` and `CandidateTask`.

Full-app Data List Workflow and Scheduled Workflow resource envelopes remain explicitly blocked by their existing materializer-not-implemented gates. Their Set Variable schema is export-proven and validator-backed, but the generator must not claim those workflow resources were materialized until the host workflow envelope/registration implementation is complete.

## Generated-Final Hard Gates

- `FORM_ACTION_SETVAR_TARGET_MISSING`
- `FORM_ACTION_SETVAR_TARGET_UNDECLARED`
- `FORM_ACTION_SETVAR_MULTI_ARRAY_EMPTY`
- `FORM_ACTION_SETVAR_VALUE_NOT_EXPRESSION_ARRAY`
- `FORM_ACTION_SETVAR_LIST_FIELD_TARGET_UNSUPPORTED_HOST`
- `FORM_ACTION_SETVAR_LIST_FIELD_TARGET_UNRESOLVED`
- `FORM_ACTION_SETVAR_CONDITION_NOT_EXPRESSION_ARRAY`
- `FORM_ACTION_SETVAR_CONTINUE_NOT_BOOLEAN`
- `APPROVAL_FORM_START_ANOTHER_ACTION_UNRESOLVED`
- `APPROVAL_FORM_FIELD_CHANGE_ACTION_UNRESOLVED`
- `APPROVAL_DYNAMIC_DISPLAY_CONTROL_ID_MISMATCH`
- `APPROVAL_DYNAMIC_DISPLAY_VARIABLE_UNDECLARED`
- `SETVARIABLE_TARGET_TYPE_MISMATCH`
- `DATALIST_WORKFLOW_SETVARIABLE_RHS_FIELD_UNRESOLVED`
- `DATALIST_WORKFLOW_CURRENT_LIST_FIELD_UNRESOLVED`
- `SET_VARIABLE_PLAN_RHS_EXPRESSION_INVALID`
- `SET_VARIABLE_PLAN_LIST_FIELD_TARGET_UNSUPPORTED_HOST`
- `SET_VARIABLE_PLAN_CHAIN_TARGET_UNRESOLVED`
- `FORM_ACTION_SETVAR_HOST_TYPE_UNSUPPORTED`
- `FORM_ACTION_SETVAR_HOST_TYPE_MISMATCH`
- existing Workflow Set Variable assignment, expression-array, declaration, and type gates

---

<a id="standalone-export-shared-generation-standard"></a>

<!-- source: docs/standards/collection-02.md#standalone-export-shared-generation-standard -->

# Standalone Export Shared Generation Standard

Standalone Yeeflow resource exports must not use a separate hand-built generation path when the plugin already knows how to materialize the same resource inside a full application package.

This standard applies to:

- Approval Form `.ywf` exports.
- Data List `.ydl` exports.
- Dashboard page `.ydp` exports.
- Full application `.yapk` materialization that contains the same resources.

## Core Rule

Standalone file generation and full-application `.yapk` materialization must share the same schema planners, golden-reference template builders, field/control mappers, dependency remappers, and validators for the same Yeeflow surface.

Do not hand-build a simplified standalone `.ydl`, `.ydp`, or `.ywf` body just because the output format is narrower than `.yapk`. If a standalone format cannot carry an app-level artifact, explicitly document that proof boundary, but keep the resource body itself generated by the same standard path used for full-app materialization.

## Mandatory Standalone Flow

Every generated standalone Yeeflow artifact must follow this flow:

```text
Standalone Artifact Plan
-> Trace JSON
-> Shared Builder
-> Standalone Export
-> Plan-vs-Actual Validator
```

The plan is the generation contract. The trace is the machine-readable projection of that contract. The shared builder is the same builder family used by full `.yapk` materialization for that surface. The standalone exporter may only wrap the shared-builder resource body; it must not rebuild or simplify the body. The final validator must compare the generated artifact back to the plan and trace.

Standalone generation must stop before handoff when the plan or trace is missing, when the trace does not declare the shared builder, or when generated output cannot prove plan-to-resource conformance. Run:

```bash
node scripts/validate-standalone-artifact-plan-trace.mjs --plan <artifact-plan.md> --trace <artifact-plan.trace.json>
```

The trace must include:

- `artifactType`: `approval_form`, `data_list`, or `dashboard`.
- `name`: the generated artifact name.
- `plan.path` or `plan.source`; include `plan.sha256` when the plan file is stable on disk.
- `sharedBuilder.required = true`.
- `sharedBuilder.builderFamily`: the exact builder family used by full-app materialization.
- `validators[]`: the gates required for this artifact type.
- `conformance.planToActualRequired = true`.
- `conformance.requiredChecks[]`: the plan-vs-actual checks the generated artifact must satisfy.

## Approval Form `.ywf`

Standalone `.ywf` generation and full-application `.yapk` Approval Form materialization must share the same Approval Form layout, workflow, field-control, condition, assignee, and validation path.

Required standalone plan sections:

- Artifact Type and approval form name.
- Submission page template, sections, fields, required/read-only/default behavior, and control mappings.
- Task form templates and reviewer-context field coverage.
- Workflow nodes, business action names, assignees, conditions, branch coverage, connector descriptions, and layout strategy.
- Shared Builder declaration for the Approval Form Layouts v1.1 and workflow builder path.
- Generation Contract and proof boundary.

Required trace surfaces:

- `approvalForm.pages[]`
- `approvalForm.layoutTemplates[]`
- `approvalForm.workflow`
- `sharedBuilder.builderFamily`
- validators including `validate-ywf-def.js`, `test-approval-ywf-form-structure-gates.mjs`, and workflow gates when workflow is present.

Generating a `.ywf` by directly assembling a simplified form body or workflow graph without this plan/trace contract is a drift defect.

## Data List `.ydl`

Standalone `.ydl` generation and full-application `.yapk` Data List materialization must share the same Data List schema and custom form generation path.

Required standalone plan sections:

- Artifact Type and Data List name.
- Fields, storage names, field types, controls, required/default/choice/lookup rules.
- Data views, display columns, sort rules, user filters, and fixed `LayoutView.filter[]` rules.
- Custom New/Edit/View/Workbench form template selections and field-grid selections.
- Reverse-related Collection, Data table, Data Analytics, Sub list, workflow, and sample-data plans when relevant.
- Shared Builder declaration for the Data List schema/form/view builders.
- Generation Contract and proof boundary.

Required trace surfaces:

- `dataList.fields[]`
- `dataList.views[]`
- `dataList.forms[]`
- `sharedBuilder.builderFamily`
- validators including `validate-ydl-list.js`, custom form layout/field-grid gates, and data-view filter gates when views are present.

Required shared contracts:

- Native `Title` field identity and uniqueness.
- FieldName / FieldIndex / InternalName synchronization.
- Lookup target rules and lookup seed value lifecycle.
- `identity-picker` fields as Text-backed identity controls unless a current export proves otherwise.
- Default view and list metadata shape.
- Custom New/Edit/View/Workbench form selection from Data List Form Layouts v1.1.
- Current-record field grids from Data List Form Fields Grid v1.1.
- Reverse-related Collection sections from the official designer-open `.ydl` shape.
- Data table, Data Analytics, Dynamic control, and Sub list template rules where those controls are generated on list forms.
- Data-list workflow and task-form generation rules where list workflows are included.
- Public Forms from `public-form-page-layout-standard`, including generated optional-region cleanup: no unconfigured CTA area, no empty copied column sections, no placeholder section title copy, and no Operations without real actions.

Standalone `.ydl` output must pass the same list and custom-form gates that full-app generation relies on:

- `validate-ydl-list.js --mode generator --stage final`
- `validate-ydl-list.js --mode generator --stage final --strict-import-ready` before import handoff
- `validate-data-list-form-layout-template.mjs` when custom forms are present
- `validate-data-list-form-fields-template.mjs` when current-record fields are materialized
- `inspect-app-creation-rules.mjs` or equivalent field creation/schema gates when package context is available
- shared Public Form generated-output validation when `PublicForms[]` is present

The packaged helper entrypoints are part of this contract. `scripts/validate-ydl-list.js` copies in the plugin and installed skills must delegate to the shared root validator, and `scripts/build-ydl-wrapper.js` must run the same strict import-ready validation before writing and after round-trip decode. A wrapper builder that can write a `.ydl` which `--strict-import-ready` would reject is considered a release-blocking entrypoint drift.

Generating a `.ydl` by directly assembling fields and a minimal `Main > Content` form shell is a drift defect if the corresponding full-app path would use the approved template builders.

Standalone `.ydl` import-safe output has a stricter wrapper contract than internal full-app resource materialization:

- `Defs[].Rules` must remain stringified JSON. Do not emit object-shaped `Rules` into standalone import-ready `.ydl` files.
- Demo `ListDatas` records must not write Yeeflow system audit fields such as `Created`, `Modified`, `CreatedBy`, or `ModifiedBy`; let import/runtime populate them.
- Type `1` custom form layouts must use `LayoutView: null` and carry the form body in `LayoutInResources[0].Resource`.
- The default Type `0` data view must keep the export-safe `Ext1.Url = "default"` key. Business slugs may be used only for non-default views.

Treat `YDL_IMPORT_FIELD_RULES_NOT_STRINGIFIED`, `YDL_IMPORT_SAMPLE_AUDIT_FIELD_PRESENT`, `YDL_IMPORT_CUSTOM_FORM_LAYOUTVIEW_NOT_NULL`, `YDL_IMPORT_CUSTOM_FORM_RESOURCE_MISSING`, and `YDL_IMPORT_DEFAULT_VIEW_URL_NOT_DEFAULT` as standalone `.ydl` import-readiness blockers.

Generator-final standalone `.ydl` validation must hard-fail simplified custom form output. A generated business Data List must not:

- reuse one generic Type `1` custom form for `ListModel.LayoutView.add`, `edit`, and `view`;
- omit a concrete New/Edit form titled `Edit Item`;
- omit a concrete View form titled `View Item`;
- place current-record field controls directly in a generic container or `flex_grid` instead of `form_grid_fields_wrapper`;
- omit the Data List Form Layouts v1.1 root contract, including `Main`, `Content`, full-width content area, zero page padding, approved content card wrappers, and approved business slots.

If any of these conditions are found, report `STANDALONE_YDL_SHARED_GENERATION_BYPASSED` and stop before handoff.

## Dashboard `.ydp`

Standalone `.ydp` generation and full-application `.yapk` Dashboard materialization must share the same Dashboard page-layout and component-template generation path.

Required standalone plan sections:

- Artifact Type and Dashboard page name.
- Dashboard page-layout template selection.
- Section plan, data source plan, Collection/Data table/Data Analytics/KPI/Summary/filter/action plan.
- Dataset presentation template IDs and business rationale.
- Page-scoped filter/temp/action namespace plan.
- Shared Builder declaration for the Dashboard page layout and component-template builders.
- Generation Contract and proof boundary.

Required trace surfaces:

- `dashboard.pageLayoutTemplateId`
- `dashboard.sections[]`
- component-template selections for Collections, Data Analytics, Data tables, Summary/KPI, filters, and actions when present.
- `sharedBuilder.builderFamily`
- validators including dashboard page layout, hard gates, dataset presentation, page-scope dependency, Data Analytics, Data table, and plan-conformance gates as relevant.

Required shared contracts:

- Dashboard page layout selection from `docs/reference/dashboard-page-layout-templates.json`.
- Exact selected template identity for `dashboard-page-layouts-v1.1`, `dashboard-page-layouts-workbench`, `dashboard-page-layouts-two-panel-workspace`, and `dashboard-page-layouts-three-panel-workspace`.
- Allowed-slot placement and removal of empty or unused template modules.
- Collection template cloning from approved golden references, including schema-bound item templates and page-scoped variables/actions.
- Data Analytics / Summary / KPI runtime model generation, including `ReportIds`, `exts`, `tempVars`, `save_var`, and visible binding.
- Data table golden reference cloning.
- Search/Data Filter variable contract and consumer binding.
- Page-level `filterVars`, `tempVars`, `actions`, and `formAction` namespace uniqueness.
- Recursive remapping of package-local source-template IDs, not adding old source IDs to provenance manifests.

Standalone `.ydp` output must pass the same dashboard gates that full-app generation relies on:

- `validate-dashboard-page-layout-template.mjs`
- `validate-dashboard-generation-hard-gates.mjs`
- `validate-dashboard-page-layout-plan-conformance.mjs` or equivalent plan-vs-actual conformance when an App Plan is present
- `validate-dashboard-dataset-presentation-golden-references.mjs` when Collections are present
- `validate-data-analytics-golden-references.mjs` and runtime-model checks when charts, Summary, KPI, or Pivot controls are present
- `validate-data-table-golden-references.mjs` when native Data table controls are present
- `validate-page-scope-template-dependencies.mjs` when multiple component templates share a page

Generating a `.ydp` by composing a visually similar dashboard body without the registered page layout and component template builders is a drift defect.

## Failure Classification

Use these classifications in validation reports:

- `STANDALONE_YDL_SHARED_GENERATION_BYPASSED`: standalone `.ydl` generation did not use the same Data List schema/form/template path as full-app materialization.
- `STANDALONE_YDP_SHARED_GENERATION_BYPASSED`: standalone `.ydp` generation did not use the same Dashboard layout/component path as full-app materialization.
- `STANDALONE_YWF_SHARED_GENERATION_BYPASSED`: standalone `.ywf` generation did not use the same Approval Form layout/workflow/template path as full-app materialization.
- `STANDALONE_ARTIFACT_PLAN_MISSING`: standalone export generation did not produce or receive an artifact plan.
- `STANDALONE_ARTIFACT_TRACE_MISSING`: standalone export generation did not produce or receive trace JSON.
- `STANDALONE_ARTIFACT_SHARED_BUILDER_MISSING`: trace JSON did not identify the shared builder.
- `STANDALONE_ARTIFACT_PLAN_TO_ACTUAL_NOT_REQUIRED`: trace JSON did not require plan-vs-actual conformance.
- `STANDALONE_EXPORT_TEMPLATE_GATE_MISSING`: standalone export validation skipped a gate required for the same surface in full-app generation.
- `STANDALONE_EXPORT_PROOF_BOUNDARY_UNDECLARED`: standalone export omitted app-level context without documenting the boundary.

## Proof Boundary

Passing standalone export gates proves only that the exported resource body follows the current generation standard. It does not prove signing, install, Version Management success, live data seed, browser runtime, Designer open behavior, or cross-resource application navigation unless those stages are executed separately.

## Custom Code Dashboard template selection

`dashboard-page-layouts-custom-code` is an explicit alternate Dashboard template. Preserve the Functional Specification requirement marker and use the Composition, Module Plan, and Communication and Native Integration tables from `docs/standards/collection-01.md#dashboard-page-layouts-custom-code-standard` for both App Plan and standalone Dashboard Plan. Materialize through the shared Custom Code page builder with populated control artifacts; no generic dataset/header fallback. Keep the existing Core issued-ID, source-binding and standalone wrapper gates.


## Dataset caption card composition (1.15.4)

Use `docs/standards/collection-01.md#dashboard-dataset-composition-standard` and the shared `scripts/lib/dashboard-dataset-composition.mjs` normalizer/validator for Dashboard generation and incremental repairs. The registered `dataset-caption-v1` content-card variant omits `section_title_area` only for a single caption-bearing Collection/Kanban with no independent outer description or action; preserve `content_card_wrapper > section_content_area` and the complete dataset template. This conditional rule overrides generic mandatory outer-title guidance. Keep grouping titles for multiple datasets and outer titles for captionless controls. Remove unused empty `kpi_cards_kpi_row` and empty ancestor modules, never a bound dataset merely because it has no records. Preserve Search before Add and all responsive template slots, filter/action dependencies and record bindings. Run `scripts/test-dashboard-dataset-composition.mjs` and final Dashboard/standalone gates. Updating a plugin is not a live application repair or runtime visual proof.

---

<a id="tab-gantt-control-golden-reference-standard"></a>

<!-- source: docs/standards/collection-02.md#tab-gantt-control-golden-reference-standard -->

# Tab and Gantt Control Golden Reference Standard

## Status and scope

This standard records privacy-safe configuration learning from the `Projects` Data List `Project Overview` custom form in Real Estate Project Control Hub. It is a generation and validation contract for the following Data List custom-form controls:

- `tab_control_workspace` (`aktabs`)
- `gantt_control_linked_activity` (`gantt`)

It does not copy tenant, workspace, application, list, layout, record, or user identifiers. Replace every `{{...}}` placeholder with a resolving ID or field name for the target application.

## Selection

Use `tab_control_workspace` when a form needs several peer sections, each with its own meaningful content. Use `gantt_control_linked_activity` only when the selected Activity source list contains the required fields and the parent form can provide a current `ListDataID` for the relationship filter and Add defaults.

Do not use either template as a full-page clone. The source form contains source-specific Custom Code, document, and business sections that are not part of these reusable control templates.

## Tab structure

Clone `docs/reference/tab-control-workspace.template.json`.

- Root type is `aktabs`.
- Every direct tab is `ak-tabs-tab` with a unique stable ID, non-empty label, and at least one content container/control.
- Exactly one child sets both `attrs.isDefault = true` and `attrs.isDesignDefault = true`; all other tabs set both to false.
- Preserve the observed root top margin token `--sp--s100` unless the host page layout needs a documented variation.
- `attrs.tabs-tabposition` is optional. When supplied, only use `top`, `bottom`, `left`, or `right`.
- Map labels and tab contents to the target domain; do not retain source-specific labels or Custom Code controls by default.

## Gantt source and relationship structure

Clone `docs/reference/gantt-control-linked-activity.template.json` only after the Activity Data List and its Type 1 detail/View layout are resolved.

1. `attrs.data.list` must include the resolved source `AppID`, `ListSetID`, and `ListID`. All three are required for Designer field discovery as well as runtime data binding.
2. The standard related-record filter is `Activity.ParentBusinessLookup == current parent ListDataID`. The filter left field and Add `passvalues[].Name` must be the same Activity lookup field.
3. `attrs.data.gantt.atts.allowadd = true` requires `passvalues` to set that lookup field from the current parent `ListDataID` expression.
4. `linkLayout` must resolve to a Type 1 detail/View layout on the Activity source list. Keep the observed `modalsize: 2`, month scale, toolbar, milestones, automatic scheduling, and right skin unless the App Plan explicitly changes them.
5. The selected source list and every Gantt field mapping must resolve before materialization. Never bind by display label alone.

## Gantt field compatibility contract

Inspect both the Yeeflow storage metadata and the form control metadata. `FieldType` alone is insufficient for Lookup and Percent validation.

| Gantt mapping | Required field metadata |
| --- | --- |
| `start_date` | `FieldType = Datetime` |
| `end_date` | `FieldType = Datetime` |
| `dependency` | `Type = lookup`; parsed `Rules.multiple = true`; parsed `Rules.listid` equals `attrs.data.list.ListID` |
| `progress` | `FieldType = Decimal`; `Type = percent`; normalized Rules range is `number_min = 0`, `number_max = 1` |
| `parent` | Optional. If mapped: `Type = lookup`; parsed `Rules.multiple = false`; parsed `Rules.listid` equals `attrs.data.list.ListID` |

The observed Activity reference uses an Activity title for text, two Datetime fields for actual start/end, a multiple self-Lookup for predecessors, a Decimal/percent completion value, and an optional single self-Lookup for hierarchy. A business may use different internal field names, but it must preserve these type relationships.

## Validation and stop conditions

Run:

```bash
node scripts/validate-tab-gantt-golden-references.mjs <decoded-app-or-fixture.json> --strict
node scripts/test-tab-gantt-golden-references.mjs
```

Stop before packaging or live save if source `AppID`, `ListSetID`, or `ListID` is missing; if any required Gantt source field is missing; if either date field is not `Datetime`; if Dependency is not a same-source multiple Lookup; if Progress is not Decimal/percent; if a mapped Parent is not a same-source single Lookup; if the parent filter/`passvalues` relationship does not resolve; if a Tab has no content; or if the default-tab count is not exactly one.

## Proof boundary

The source configuration was read back from the live Designer data model. The plugin also has a separate focused Dashboard proof that Tab switching works. This does not prove this exact Gantt configuration renders, schedules, opens Activity View, creates prefilled child activities, or executes dependencies in a generated Data List form. Those behaviors require a focused generated-package runtime test before a runtime-proven release claim.

---

<a id="three-column-workspace-layout-standard"></a>

<!-- source: docs/standards/collection-02.md#three-column-workspace-layout-standard -->

# Three-Column Workspace Layout Standard

## Purpose

The `three_column_workspace_layout` pattern is an optional Yeeflow page design for dense business workspaces that need an inbox-style left context panel, central work area, and right detail/action panel. It is based on the `Dashboard Layout Golden Reference` export study and is intended for planning and generation guidance, not as a mandatory global layout.

Use this standard when an app needs a Gmail/Outlook-style work management surface: triage queues, service desk inboxes, CRM account workspaces, renewal reviews, approval review centers, task centers, or list-detail-detail experiences.

## When To Use

- The user needs to scan a queue or list and inspect details without leaving the page.
- Left navigation, filters, or contextual summary should remain visible while work happens in the middle.
- A right-side detail panel should show selected item context, actions, SLA/risk information, or notes.
- The page is operational and repeated-use, not a simple single-submit form.
- The generated app can fill each panel with meaningful data-bound controls or business content.

## When Not To Use

- Simple forms, single-purpose dashboards, lightweight request pages, or mobile-first minimal pages.
- Pages where all three panels would be empty placeholders.
- Executive overview dashboards that are better served by KPI rows, charts, and table sections.
- Approval forms where a single-column requester or reviewer experience is clearer.

## Layout Anatomy

Use these semantic names in app plans, composition checklists, and generated Navigator labels where possible:

- `three_column_workspace_layout`
- `left_context_panel`
- `main_content_panel`
- `right_detail_panel`
- `left_panel_bottom`
- `main_panel_bottom`
- `right_panel_bottom`
- `page_header_action_area`
- `detail_information_panel`

The reference app uses a root page/form wrapper with one top-level `container`. That main container contains three sibling panel containers. Each panel contains three regions:

- header/action area
- scrollable body/content area
- bottom/supporting region

The reference body regions contain designer drop-zone text, but generated final apps must replace those placeholders with useful controls, fields, tables, cards, or guidance.

## Runtime Implementation Rules

The golden reference is a container-layout pattern, not just a semantic grouping pattern. A generated page must not claim `three_column_workspace_shell` conformance merely because it contains three labeled sections. The shell must be configured so Yeeflow renders left, main, and right panels side by side at runtime.

The reference package and exported `Dashboard-Home.html` show this implementation model:

- The visible shell is a top-level `container` under the page/form content root.
- The shell uses `attrs.common.pos = [null, "absolute"]` with centered horizontal/vertical placement in the reference.
- The shell uses `attrs.style.direction = [null, "row"]` and a gap token such as `--sp--s100`; the runtime HTML maps this to a flex row with a gap.
- The shell has exactly three direct panel container children for the desktop workspace.
- The left panel uses fixed width behavior: `attrs.style.widthtype = [null, "3", null, "1"]`, `attrs.style.width = [null, 360]`.
- The main panel uses fill/remaining width behavior: `attrs.style.widthtype = [null, "0"]`. The observed `width = 300` is not the layout driver; `widthtype = 0` is what prevents the main panel from becoming a fixed narrow column or stacking below the left panel.
- The right panel uses fixed width behavior: `attrs.style.widthtype = [null, "3", null, "1"]`, `attrs.style.width = [null, 480]`.
- Every panel uses full-height bounded behavior: `attrs.style.height = [null, "2"]`, `attrs.style.cushei = [null, 100]`, `attrs.style.cusheiu = [null, "%"]`, `attrs.style.justify_content = [null, "flex-start"]`, `attrs.style.overflow = [null, "hidden"]`.
- Each panel body uses `attrs.style.overflow = [null, "scroll"]`, full-height behavior, and inner padding, so body content scrolls inside the panel instead of expanding the shell vertically.
- Header containers use sticky/top behavior: `attrs.common.pos = [null, "sticky"]`, `attrs.common.sticky.to = [null, "1"]`, `attrs.common.zidx = [null, 2]`, row direction, center alignment, `justify_content = "space-between"`, and a bottom border separator.
- Bottom/support containers use row direction, center alignment, padding, background, and a top border separator.
- Panel backgrounds, radius, borders, and shadows are part of the bounded workspace feel, but the layout-critical properties are row direction, direct sibling panel hierarchy, fixed/fill/fixed widths, full height, and overflow.

Generated-final requirements:

- Do not emit a claimed three-column shell if the shell lacks row/flex direction or equivalent position-based side-by-side mechanics.
- Do not rely on visual order alone. Left, main, and right panels must be siblings under the same shell container.
- Do not let the main panel flow below the left panel. Use the reference fill-width behavior or another validated side-by-side mechanism.
- Do not let the right panel flow below the main panel. Use explicit fixed width or validated side-by-side positioning.
- Do not leave panel bodies as default overflow. Use scroll/auto body overflow and hidden outer panel overflow when generating an inbox-style workspace.
- Do not use unsupported or invented width styles that create 0px runtime risk.
- Do not show `Drag to add controls here` in generated-final packages.
- If the generator cannot reproduce these mechanics, it must not claim `three_column_workspace_shell`; choose a simpler dashboard section or two-column list/detail pattern instead.

## Dashboard Layout Styles

Yeeflow App Builder now treats dashboard pages as having two valid root layout styles.

### Style 1: Standard Dashboard Content Layout

Use Style 1 for normal dashboard pages: KPI dashboards, overview dashboards, report dashboards, mixed card/table dashboards, and most generated dashboard pages.

Structure:

```text
dashboard_page_wrapper
└─ Main
   └─ Content
      └─ sections/cards/tables
```

Rules:

- `Main > Content` is required.
- Page content width should be Full Width when the dashboard needs full-width content.
- Page padding should be `0` for full-width dashboards; visible spacing belongs inside `Content` sections.
- Existing Type `103`, `LayoutView = null`, `Ext2 = "{\"src\":true}"`, and dashboard wrapper/materialization rules still apply.

### Style 2: Three-Column Workspace Shell Layout

Use Style 2 for inbox-style, Gmail-style, Outlook-style, list-detail-detail workspaces where the shell itself owns the full-page layout.

Structure:

```text
dashboard_page_wrapper
└─ three_column_workspace_shell
   ├─ left_context_panel
   ├─ main_content_panel
   └─ right_detail_panel
```

Rules:

- Do not nest `three_column_workspace_shell` inside default `Main > Content` wrappers.
- `three_column_workspace_shell` must be the root page body layout container.
- The dashboard page must set content width to Full Width.
- The dashboard page must set page padding to `0`.
- The shell controls full available width, full available height, row layout, panel sizing, panel overflow, scrollable body regions, and sticky/stable headers.
- If `three_column_workspace_shell` is nested inside `Main > Content`, the runtime may render blank or incorrectly because the parent containers use default height/positioning and prevent the shell from occupying the page.

This distinction does not invalidate the normal `Main > Content` rule. `Main > Content` remains required for Style 1 dashboards. Style 2 is a separate root-layout rule for three-column workspace pages only.

## Property Checklist

When generating or validating `three_column_workspace_shell`, check these properties on the actual control tree:

| Area | Required properties |
|---|---|
| Shell | `position`/`common.pos`, top/right/bottom/left or equivalent full-page offsets, row direction, gap, background, padding |
| Shell children | exactly three direct panel containers for desktop layout |
| Left panel | explicit fixed width, full height, `overflow = hidden`, bounded background/border/radius/shadow |
| Main panel | fill/remaining width behavior, full height, `overflow = hidden`, bounded background/border/radius/shadow |
| Right panel | explicit fixed width, full height, `overflow = hidden`, bounded background/border/radius/shadow |
| Headers | sticky/top behavior, z-index, row direction, center alignment, `justify_content = space-between`, min height, separator border |
| Bodies | full-height/fill behavior, `overflow = scroll` or `auto`, inner padding, meaningful business controls |
| Bottom regions | row direction, padding, top border separator, meaningful supporting content when present |
| Actions/icons | icon/action containers have explicit small widths/heights and center alignment; active actions have valid bindings |
| Responsive behavior | reference contains mobile/tablet display conditions; generated apps must document mobile behavior as runtime-sensitive unless separately tested |

## Dashboard Implementation Notes

For dashboard pages:

- Keep the dashboard host as a Type `103` page/layout.
- Preserve current dashboard wrapper rules: `LayoutView = null`, `Ext2 = "{\"src\":true}"`, and `LayoutInResources[]` that points to the embedded page resource when inline content exists.
- The embedded page resource should keep the export-like wrapper keys: `children`, `attrs`, `title`, `ver`, `filterVars`, `tempVars`, and `exts`.
- For normal Style 1 dashboards, keep the standard `Main > Content` composition.
- For `three_column_workspace_shell`, use Style 2: place the shell as the root page body layout container, not under `Main > Content`.
- Set dashboard content width to Full Width and page padding to `0` for Style 2 pages.
- Use native Heading/Text, Data table, Collection, Kanban, Timeline, Dynamic field, Button, Icon, and container controls. Do not use ad hoc `type: "text"` controls.
- Icon controls inside the shell should use inline width behavior, like Text and Button controls. Choose icon size intentionally for the panel/header context; large/default icon sizing can distort narrow panel layout.
- Text/heading, Button, and Dynamic field controls should also default to inline width behavior in generated workspace shells. Other controls still need explicit width behavior appropriate to their container.
- For 3-column, 4-column, or other multi-column workspace layouts, each visible column should have its own vertical scroll behavior. The outer shell should be bounded with hidden overflow when it owns the full-page layout; panel bodies should use Scroll or Auto instead of relying on Default overflow.

## Approval Form/Page Implementation Notes

For approval forms:

- The reference approval form uses the same container tree as the dashboard but is hosted inside `DefResource.pageurls[].formdef`.
- The studied reference includes a requester/submit page: outer `pageurls[].type = 1`, outer `pageurls[].pagetype = 1`, embedded `formdef.pagetype = 1`.
- If this pattern is adapted to a workflow task/reviewer page, keep the task URL shape correct:
  - outer `pageurls[].type = 2`
  - outer `pageurls[].pagetype = 1`
  - embedded `pageurls[].formdef.pagetype = 2`
- Do not reintroduce the invalid task page shape where the outer page URL uses `pagetype = 2`.
- Avoid unsupported controls on approval forms. For example, Pivot Table belongs on dashboards, not approval forms.

## Recommended Container Hierarchy

```text
dashboard_page_wrapper_or_form_content_root
└─ three_column_workspace_shell
   ├─ left_context_panel
   │  ├─ left_header_action_area
   │  ├─ left_panel_body
   │  └─ left_panel_bottom
   ├─ main_content_panel
   │  ├─ page_header_action_area
   │  ├─ main_work_area
   │  └─ main_panel_bottom
   └─ right_detail_panel
      ├─ detail_header_action_area
      ├─ detail_information_panel
      └─ right_panel_bottom
```

Recommended generated content:

- Left panel: queue groups, filters, navigation shortcuts, app context, counts, or ownership summaries.
- Main panel: primary Data table, Collection, Kanban, request fields, selected queue work area, or review checklist.
- Right panel: selected item detail, status/SLA/risk, actions, notes, activity history, or decision context.
- Bottom regions: supporting notes, totals, recent activity, selected queue metadata, secondary actions, or help content. Omit a bottom region when it would be empty.

## Spacing And Style Guidance

The reference uses:

- root/page padding plus an inner main container gap
- fixed-width side panel intent with a wider central work area in generated designs
- panel containers with neutral borders, soft shadow/radius, white background, and hidden outer overflow
- header/footer regions with horizontal padding and a bottom/top border separator
- body regions with larger padding, vertical gap, and scroll overflow

For generated apps:

- Use safe desktop padding, typically 24px to 32px where supported.
- Use a clear gap between panels.
- Make the main panel the dominant work area. If width settings are explicit, the middle panel should be visually largest or should flex to fill remaining space.
- Avoid invented unsupported width/style fields. Prefer export-proven style attributes and validate dashboard wrapper rules.
- Ensure panel text and controls do not overlap at smaller widths. If mobile behavior is not proven, document it as runtime-sensitive and prefer a simpler responsive stack or a different pattern.

## Header And Action Guidance

- The main panel header should include the page/workspace title and primary actions.
- The left panel header should identify the app/context and may include a compact navigation/action icon.
- The right panel header should identify the selected detail context and may include overflow or detail actions.
- Active buttons must have valid action bindings. Placeholder buttons are generated-final failures.
- Icon-only actions must have meaningful Navigator labels and business purpose.
- Icon controls should use inline width behavior unless a full-width icon block is explicitly intended and validated.
- Text/heading, Button, and Dynamic field controls should use inline width behavior by default in generated workspace pages.
- In narrow panels, icon size must not force wrapping, overflow, or header distortion.
- Icon size should match the nearby text or control rhythm. For menu/action rows with 16px text, use a 16px bounded icon instead of a large default icon.

## Runtime Risks

- Empty drop-zone headings from a designer reference are not acceptable generated-final content.
- Dashboard pages can render blank if the embedded page resource wrapper is incomplete or placed outside the expected dashboard shell.
- Three-column dashboards can render blank or incorrectly if `three_column_workspace_shell` is nested under default `Main > Content` containers with default height/positioning.
- Data tables can fail at runtime if display columns omit source `Field` bindings.
- Approval task pages can fail publish/import if task URL page shapes are incorrect.
- Mobile behavior is not runtime-proven by this reference and should be tested separately.
- A YAPK wrapper can validate while runtime rendering still needs manual proof.

## Validation Checklist

When a generated page declares `three_column_workspace_shell` or `three_column_workspace_layout`:

- The page/form contains one main workspace container with three sibling panel containers.
- For dashboard Style 2, `three_column_workspace_shell` is the root page body layout container and is not nested under `Main > Content`.
- The dashboard page sets content width to Full Width and page padding to `0`.
- The panels can be identified as left context, main content, and right detail/action regions.
- Each used panel contains header and body regions; bottom regions are present only when meaningful.
- The main panel is the primary work area and is not visually subordinate.
- No final panel is placeholder-only.
- Dashboard wrappers still satisfy current Type `103` page rules.
- Approval task pages keep outer task page URL `pagetype = 1` and embedded task formdef `pagetype = 2`.
- Native Text/Heading controls are used instead of ad hoc text shapes.
- Icon controls inside the workspace use inline width and intentionally bounded sizes.
- Data-bound controls resolve their lists, fields, filter variables, actions, and display columns.

## Relationship To Existing Dashboard Standards

This standard extends the existing dashboard standards with a page-level workspace composition. It does not replace KPI row, summary card, alert, table, Kanban, Collection, timeline, filter, or dashboard shell rules. Use those section templates inside the three-column panels when they fit the business process.

## Relationship To Approval Form/Page Rules

This standard can guide requester and reviewer form layout, but it does not change approval workflow generation rules. Workflow task URL hard gates, publish-readiness checks, variable reference checks, form action bindings, and supported-control host rules remain authoritative.

## Relationship To Multi-Column Form Workspaces

Service Desk Pro proves a related but distinct pattern: `multi_column_form_workspace_shell`. That real-case app is an approval/form workspace, not a dashboard. It uses the form surface for variables, form actions, comments, selected-record behavior, dynamic field controls, and action-driven filtering. Its visible layout is closer to a four-column service desk console: left help desk navigation, ticket collection/list, selected ticket conversation/detail workspace, and right ticket attributes panel.

Use this three-column standard for dashboard Style 2 workspaces where the main goal is a side-by-side dashboard/list-detail-detail layout. Use `docs/standards/collection-02.md#multi-column-form-workspace-standard` when the app needs form actions, variable-driven filters, collection item click actions, selected-record state, comment/update controls, or dynamic form fields. Do not force `Main > Content` dashboard rules onto the form workspace pattern, and do not add fake workflow routing just to get a stateful form surface.

---

<a id="ui-summary-kpi-runtime-hard-gates"></a>

<!-- source: docs/standards/collection-02.md#ui-summary-kpi-runtime-hard-gates -->

# UI, Summary/KPI, And Runtime Evidence Hard Gates

## Purpose

Generated Yeeflow UI quality must be treated as a product contract, not decoration after app structure. A package can pass schema validation, signing, install, or upgrade acceptance and still be visually wrong, scaffold-like, or dynamically unproven.

## Page-By-Page UI Contract

Before modifying dashboards or UI-heavy pages, create a page-by-page UI implementation contract. The contract must name each target page, page purpose, design/mockup reference when requested, visual sections, Yeeflow control mapping, data/list bindings, KPI/Summary plan, filter/action plan, grid/table plan, status/badge plan, runtime evidence requirement, and proof boundary.

Phase 1 closed-loop helper:

```sh
node scripts/generate-ui-contract-from-design.mjs --design <design-image> --plan <app-plan.md-or-json> --output <ui-contract.md>
```

If no local vision parser is available, this helper emits a review-required draft from the app plan and design image reference. It must not be treated as complete visual understanding until a human reviewer resolves the visual extraction items.

Phase 2 closed-loop helper:

```sh
node scripts/compare-design-to-runtime-structure.mjs --contract <ui-contract.md-or-json> --runtime-evidence <redacted-runtime-evidence.json> [--design-image <design-image>] [--strict]
```

Phase 2 adds structural design-to-runtime comparison. It is not pixel-perfect visual diffing and does not claim full automatic image understanding. It compares UI contract expectations against redacted runtime evidence, including Phase 1 evidence from `capture-runtime-ui-evidence.mjs`. When a design image is supplied without a reliable parser, the comparison must stay in contract-runtime mode, mark human review required, and emit a review warning. Runtime evidence is required before UI quality claims. Screenshots are helpful but not always mandatory unless high-quality visual proof is claimed. Dynamic KPI proof remains governed by before/after mutation evidence and the existing runtime/KPI validators.

Phase 1 and Phase 2 are now stable closed-loop capabilities. Phase 3A makes the workflow stricter in hard-gate guidance and regression tests: design/mockup work must start with a generated and validated UI implementation contract; UI upgrades must define and validate a page/scope manifest; runtime evidence plus structural comparison is required before design fidelity claims; and agents must iterate the exact failing controls reported by the hard gates. Package validation/signing/install/upgrade success is not visual proof, and neither upgrade-check nor upgrade-apply success can be used as a substitute for redacted runtime evidence. Dynamic KPI proof remains separate and requires before/after mutation evidence. Real Marketing Event private artifacts are not committed; regression fixtures are synthetic/inspired.

Phase 3B is the workflow enforcement layer. Phase 3B closes the planned UI-quality track; after Phase 3B, future work is normal incremental improvement unless a new runtime issue class appears. The workflow enforcement helper purpose is to validate workflow/report metadata and final artifact paths before high-quality UI, runtime UI quality, design fidelity, or dynamic KPI proof is claimed:

```sh
node scripts/inspect-ui-closed-loop-workflow-enforcement.mjs --workflow <workflow-report.json-or-md> [--strict]
```

High-quality UI claims require evidence chain, not just install success. Final reports must include the UI contract, contract validation result, scope manifest and scope validation result for upgrade work, runtime evidence, design/runtime structure findings, workflow enforcement findings, unresolved findings or warning waivers, and before/after KPI mutation evidence when dynamic KPI proof is claimed. Package validation/signing/install/upgrade success is not visual proof. Dynamic KPI proof remains separate. Structure comparison alone is not dynamic KPI proof. The workflow enforcement helper is required before high-quality UI/design-fidelity claim.

Default closed-loop artifact/report paths are:

- UI contract Markdown: `docs/generated-ui-contracts/<app-or-package>/<page>.ui-contract.md`
- UI contract JSON: `docs/generated-ui-contracts/<app-or-package>/<page>.ui-contract.json`
- UI upgrade scope manifest: `docs/ui-upgrade-scopes/<app-or-package>/<page>.scope.json`
- Runtime evidence: `dist/runtime-evidence/<app-or-package>/<page>.runtime-evidence.redacted.json`
- Design/runtime structure findings: `dist/runtime-evidence/<app-or-package>/<page>.design-runtime-structure.findings.json`
- Workflow enforcement findings: `dist/runtime-evidence/<app-or-package>/<page>.closed-loop-workflow.findings.json`

## Application Layout Design Images

Generated design images for Yeeflow apps must use one of the four official Yeeflow application layouts before they become UI implementation references: `application-layout-1-vertical-nav`, `application-layout-2-horizontal-nav`, `application-layout-3-header-nav`, or `application-layout-4-no-nav`. PNG/JPEG layout screenshots are the primary visual references for header/navigation/content safe-area geometry and dropdown or expanded menu behavior; YAPK exports are supporting structural references. All page images in the same app must use the same selected layout and the same canonical application chrome style. Design-image prompts must preserve that Yeeflow header/nav/content safe-area structure, header mode, nav mode, nav background mode, selected state style, app icon placement, and app name placement. For Layout 1 vertical nav, do not add a header hamburger icon or bottom Collapse control, and do not mix dark and light nav panels across pages unless a separately named canonical style is explicitly reviewed and used everywhere. Header/navigation chrome must follow the selected layout, and page-specific content must stay inside the content safe area.

Run `scripts/inspect-application-layout-design-rules.mjs` for generated design image specs or UI contracts. The validator checks declared layout compliance and application chrome fidelity; blocks Layout 1 header hamburger and bottom Collapse controls; detects cross-page chrome drift such as dark/light nav background mismatches; and blocks unsupported arbitrary app shells/navigation chrome, custom sidebars, custom top bars, floating navigation, and content that overlaps header/nav safe areas. It does not claim automated screenshot understanding unless a future reliable parser exists; screenshot-derived layout rules must remain human-reviewed or review-required and are not pixel-perfect or automated screenshot proof.

Run:

```sh
node scripts/inspect-yeeflow-ui-design-contract.mjs --contract <ui-contract.md> --claim-high-quality-ui
```

Broad full-app restyling without page-level contracts is a hard stop. Placeholder wording such as `Here is the title`, `Here is the description`, `placeholder`, or `scaffold` is a generated-final failure.

## Export-Proven Style Shapes

Use export-proven Yeeflow style shapes for dashboard cards and chips:

- `attrs.common.background.normal.classic.color`
- `attrs.common.border.normal.type`
- `attrs.common.border.normal.width`
- `attrs.common.border.normal.color`
- `attrs.common.border.normal.radius`
- `attrs.common.padding`
- `attrs.style.gap`
- control-level typography style objects
- Collection grid-table patterns when grid-table layout is intended

Weak top-level style guesses can look plausible in JSON and flatten at runtime. For uncertain styling or new controls, create a sandbox page first, verify a runtime screenshot, then apply the pattern to the real page.

Run:

```sh
node scripts/inspect-dashboard-style-shapes.mjs --package <decoded.json-or-package.yapk>
```

## Summary/KPI Contract

Summary/KPI dashboard generation has three proof states that must stay separate:

1. Designer-configured Summary control
2. Validator-valid Summary contract
3. Runtime-proven visible dynamic KPI rendering

Hidden Summary controls must live in a dedicated hidden container with:

```json
{
  "attrs": {
    "common": { "hide": [null, true, true, true] },
    "style": { "direction": [null, "row"] },
    "display": { "rule": "1 == 0" }
  }
}
```

Each generated Summary must use a UUID control ID, resolve to the current app, a real list, and real field metadata. Semantic/layout-derived Summary IDs such as `<LayoutID>_summary` are not runtime-proven and must fail signing readiness even if `ReportIds`, `exts`, and `tempVars` exist. `attrs.data.field`, `attrs.field`, `fieldObject`, and `fieldInfo` must be populated consistently. Count summaries require valid count/ListDataID shape where required. Sum and average summaries must use numeric fields. Filters must use designer-compatible condition/filter shapes, unless intentionally documented as an all-record KPI.

Every Summary must save to a unique temp variable with the designer-exported expression-object `save_var` shape, not a plain string. Summary control IDs must be registered in `Pages[].LayoutInResources[].Resource.ReportIds`. The matching `Resource.exts[]` entry must set both `i` and `id` to the same Summary UUID and include complete runtime source metadata: `AppID`, `ListSetID`, `ListID`, `list`, `source`, and export-shaped `settings.values[]`. Top-level `Pages[].ReportIds` is optional compatibility metadata and must not be required unless proven by official exports. `runtimeModelProven: true` cannot be used to override these package-shape requirements.

Run:

```sh
node scripts/inspect-dashboard-summary-control-contract.mjs --package <decoded.json-or-package.yapk>
```

## Data Analytics Control Identity

Data Analytics controls must use runtime-safe control identities. This inventory is currently in scope:

- Pie chart
- Column chart
- Line chart
- Gauge
- Funnel chart
- Color block heatmap
- Summary
- Pivot table

For every generated Data Analytics control, the control ID must be UUID-based unless an export-proven Yeeflow sample proves another ID shape is valid for that exact non-Summary control type. Generated Summary controls are stricter: use UUID IDs only. Do not assume the Summary control shape applies exactly to Pie chart, Column chart, Line chart, Gauge, Funnel chart, Color block heatmap, or Pivot table.

Every analytics control must satisfy these generated-final rules:

- Control ID shape is runtime-safe.
- Control ID is stable across upgrades.
- Control ID appears in the correct page/report registration metadata when required.
- Matching `Resource.exts[]` entries exist when required.
- `Resource.exts[].i` matches the control ID when the control uses `exts`.
- Analytics settings, data source, fields, filters, grouping, aggregation, and display metadata are designer-shaped.
- Placeholder fields and invented field IDs are forbidden.
- Runtime proof is not claimed unless runtime screenshot/evidence confirms the control renders correctly.
- Upgrade/new-version workflows preserve existing analytics control IDs.
- Only newly added analytics controls get newly generated UUID/API-issued IDs.

Summary has a proven UUID runtime shape through KPI Runtime Binding Proof v1.0.1. Pivot table has export-proven dashboard `exts[]` registration where `category = "___Pivot___"`, `key = "PivotTable"`, and `i` equals the Pivot Table control ID, with focused runtime proof for a representative dashboard package. Pie chart, Column chart, and Line chart have prior generated dashboard runtime proof for visible rendering, but their detailed generated-final identity/registration rules must still be validated from export-proven shapes before broader runtime correctness claims. Gauge, Funnel chart, and Color block heatmap remain unproven for generated-final runtime claims until sandbox/export study captures exact designer shapes.

Run:

```sh
node scripts/inspect-data-analytics-control-identity.mjs --package <decoded.json-or-package.yapk>
```

## Visible KPI Runtime Boundary

Visible Text/Heading controls must not show raw temp variable names and must not render blank. Dynamic visible KPI binding is proven only for the exact UUID Summary v1.0.1 shape documented below, and only when runtime evidence proves nonblank visible dynamic values without raw variable names.

If a package does not use that exact shape, visible KPI dynamic binding is not considered solved unless runtime-proven for its own shape. Generation must stop or use an explicitly labeled fallback and record the gap. Fallback values are visual fallback only; they are not proof of dynamic KPI rendering.

### Exact Proven UUID Summary Shape

KPI Runtime Binding Proof v1.0.1 proved dynamic visible KPI binding only for this dashboard Summary shape:

- Summary control IDs are UUIDs.
- Each Summary UUID appears in `Resource.ReportIds[]`.
- Each Summary UUID has a matching `Resource.exts[]` entry where both `i` and `id` equal the Summary control ID, `category` is `___Pivot___`, and `key` is `summary`.
- Each Summary `Resource.exts[].attr` includes `AppID`, `ListSetID`, `ListID`, `list`, `source`, and export-shaped `settings.values[]` metadata for the current app/list.
- Each Summary control saves to a dashboard temp variable through a designer-shaped `attrs.save_var` expression object.
- `Resource.tempVars[]` declares the same temp variable ids/names.
- Visible KPI Heading/Text controls bind through `attrs.headc.title.variable[]`.
- Proof KPI values do not use static or formatted fallback values.
- Summary field metadata includes `attrs.data.field`, `attrs.field`, `fieldObject`, and `fieldInfo`.

Runtime proof for this shape must include before/after source data mutation evidence and refreshed/recalculated page state. The v1.0.1 proof changed expected KPI values from `3 / 600 / 2 / 300` to `4 / 1000 / 3 / 700` after adding a synthetic source row with amount `400`, status `Open`, and region `APAC`.

Summary recalculation may be asynchronous or cache-delayed. Stale after-evidence that still shows the before values must not be used as the final verdict; capture final evidence only after the refreshed/recalculated runtime state is visible.

This proof explicitly does not prove semantic/non-UUID Summary IDs, approval forms, public forms, unsupported surfaces, or other visible KPI binding shapes. Semantic/non-UUID Summary IDs must remain signing-readiness failures for generated packages, even when the package includes matching `ReportIds`, `exts`, `tempVars`, and `runtimeModelProven: true`. Marketing Event dashboards may use this shape, but each generated package must still run its own before/after mutation proof before claiming runtime dynamic KPI success.

Run:

```sh
node scripts/inspect-visible-kpi-runtime-bindings.mjs --evidence <redacted-runtime-evidence.json>
```

Use `docs/examples/runtime-evidence.redacted.example.json` as the safe starting point for runtime proof reports. It is synthetic, includes the exact UUID Summary v1.0.1 proof-shape fields, preserves a fallback example for unsupported shapes, and avoids private screenshots, tenant URLs, workspace IDs, raw responses, and package payloads.

Phase 1 closed-loop helper:

```sh
node scripts/capture-runtime-ui-evidence.mjs --pages-json <redacted-pages.json> --output <redacted-runtime-evidence.json>
```

The capture output is redacted metadata shaped for `inspect-runtime-evidence.mjs` and `inspect-visible-kpi-runtime-bindings.mjs`. Do not store private tenant screenshots or raw runtime responses in fixtures.

Use Phase 2 comparison after capture to check structural fidelity:

```sh
node scripts/compare-design-to-runtime-structure.mjs --contract <ui-contract.md-or-json> --runtime-evidence <redacted-runtime-evidence.json>
```

The comparison produces structured findings for page presence, sections, KPI cards, tables/grids, filters/actions, badges/status cells, spacing hierarchy, placeholder/filler text, weak evidence, screenshot availability, and dynamic KPI proof boundaries. It must never treat install, signing, import, upgrade, API acceptance, or package validation as visual success.

## Runtime Screenshot Evidence

Do not claim high-quality UI from schema validation, package validation, signing, install, upgrade-check, upgrade-apply, or API acceptance. Runtime screenshot/evidence must confirm KPI values are visible, hidden Summary controls are not visible, dashboard cards are card-like, filters/actions are visible, tables/grids are not empty scaffolds, badges/chips are distinct, and the page does not look like a plain scaffold.

Runtime proof that depends on browser screenshots must explicitly refresh Chrome before screenshot capture. Runtime navigation evidence must be nav-scoped or exact-line based; broad body-text scans are not reliable navigation proof. Runtime proof must separately report app chrome fidelity, primary navigation fidelity, content structure fidelity, and dynamic KPI proof boundary. Signing, verifysign, upgrade-check, and upgrade-apply are not visual proof.

For design-driven apps, the plan and UI contract must define exact primary navigation labels and order plus hidden support resources. Visible primary navigation must be generated from the approved UI contract, not inferred from all resources in the package. Support data lists, forms, approval pages, and implementation-only resources must not automatically appear in primary navigation when the approved contract excludes them. Hiding support resources should use schema-compatible metadata such as `ListSet.LayoutView.sort`.

P0 runtime navigation proof is enforced by `scripts/inspect-runtime-navigation-proof.mjs`. The validator accepts structured JSON contract and runtime-evidence files only; it does not inspect Chrome, parse real screenshots, parse raw package payloads, or call Yeeflow APIs. It fails when the approved primary navigation labels/order are missing or mismatched, hidden support-resource expectations are missing, support resources are visible in primary navigation, screenshot metadata lacks explicit browser refresh before capture, runtime navigation evidence is broad body-text-only rather than nav-scoped or exact-line based, or signing/verifysign/upgrade-check/upgrade-apply success is used as visual proof. Navigation fidelity passing does not mean content fidelity passed.

Control-property fidelity is enforced by `scripts/inspect-ui-control-property-fidelity.mjs` when high-quality dashboard/content design fidelity is claimed from declared or redacted control specs. Section presence is not enough. Container rows/cards must use Yeeflow attrs such as `widthtype`, `direction`, `align_items`, `justify_content`, `gap`, and `wrap`; filters must be real Data Filter controls with filter variables; target Summary/Collection/List controls must consume those variables; and KPI cards must use a consistent icon tile and text-stack pattern. Grid used for visual composition, static Text used as a filter, missing filter-variable consumption, weak KPI card structure, and dynamic KPI proof without before/after mutation evidence are hard-gate findings.

Content-fidelity review must cover KPI card visual richness, table badge/progress/avatar treatment, spacing, and hierarchy. If design KPI values are mock visual placeholders, runtime value mismatch is a warning only. If dynamic KPI proof is claimed, before/after mutation evidence is required.

Run:

```sh
node scripts/inspect-runtime-evidence.mjs --evidence <redacted-runtime-evidence.json> --claim-high-quality-ui
node scripts/test-ui-hard-gates-all.mjs
```

When runtime evidence is unavailable, generated reports must say:

- UI runtime proof not completed
- dynamic KPI visible binding not proven
- install/signing is not visual runtime proof

## Grid/Table Quality

When a plan calls for Collection grid-table layout, generated dashboards must include Collection grid-table templates with meaningful columns, headers, item templates, empty states, and planned row/action links. Do not use dashboard `data-list` when Collection grid-table was intended.

Run:

```sh
node scripts/inspect-grid-table-quality.mjs --package <decoded.json-or-package.yapk> --require-grid-table
```

## Upgrade Lineage

UI upgrades must preserve the installed app lineage. ListSetID and existing resource IDs must not drift when the user requests an update. Package classification must remain upgrade, existing app identity must stay stable, existing pages/resources must not change outside declared scope, and `Resource.ReplaceIds` must be rebuilt from final package contents.

Run:

```sh
node scripts/inspect-yapk-upgrade-app-identity.mjs --package <package.yapk> --lineage <lineage.json>
node scripts/validate-yapk-upgrade-id-stability.mjs --previous-package <previous.yapk> --previous-manifest <previous-id-lineage.json> --new-package <package.yapk> --new-manifest <new-id-lineage.json>
```

Phase 1 closed-loop helper:

```sh
node scripts/validate-ui-upgrade-scope.mjs --previous-package <previous.json-or-yapk> --new-package <new.json-or-yapk> --scope <scope-manifest.json>
```

Use this before focused UI upgrades to fail closed on ListSetID drift, app identity drift, unrelated page/resource changes, data list/field changes, approval form changes, workflow changes, navigation drift, numeric ID lineage changes, and new generated IDs without explicit allowance.

## Tolerant Decode Helper

Official designer exports can use Brotli/base64 variants that strict helpers may reject. Use `decode-yapk-tolerant-brotli.mjs` only for safe structural diagnostics. It must fail safely and never print raw package payloads, raw `Resource`, raw `Sign`, tenant URLs, tokens, or private values.

```sh
node scripts/decode-yapk-tolerant-brotli.mjs --package <official-export.yapk>
```

---

<a id="workflow-assignee-expression-serialization-standard"></a>

<!-- source: docs/standards/collection-02.md#workflow-assignee-expression-serialization-standard -->

# Workflow Assignee Expression Serialization Standard

## Scope

This standard applies to expression-based `usertaskassignment[]` entries on Approval, Data List, and Scheduled workflows, including Assignment Task and Claim Task receiver expressions.

## Canonical Syntax

Workflow assignee Expression Buttons use Yeeflow variable JSON syntax. They are not ordinary JSON objects wrapped in a template string.

Applicant line-manager outer expression:

```text
${ "type":"user", "param":{"id":"${\"type\":\"application\",\"prop\":\"ApplicantUserID\"}"},"prop":"LineManager"}
```

Required properties:

- The outer expression starts with `${` followed by object members. It must not start with `${{`.
- `param.id` for manager and organization-property expressions contains a nested `${...}` expression.
- `ApplicantUserID` uses nested `type=application` context.
- Workflow user variables use nested `type=variable` context when a property such as `LineManager` is selected.
- `usertaskassignment[].title` and `.value` preserve the same Expression Button data.
- Multiple assignees remain separate array entries.

## Prohibited Shapes

These are generated-final blockers:

```text
${{"type":"user",...}}
```

```json
{
  "param": {
    "id": "{\"type\":\"application\",\"prop\":\"ApplicantUserID\"}"
  }
}
```

The first shape incorrectly wraps ordinary JSON as `${{...}}`. The second stores plain JSON text where nested Yeeflow `${...}` variable JSON is required.

## Generation Contract

- Use `scripts/lib/workflow-assignee-expression-utils.cjs` for serialization.
- Do not call generic `JSON.stringify()` and interpolate the result directly as `data="${...}"`.
- Standalone `.ywf` and full-application `.yapk` generation must share this serializer.
- Approval, Data List, and Scheduled workflow materializers must not implement separate assignee Expression Button builders.

## Validation Contract

Validators must HTML-decode the Expression Button `data` attribute, parse the outer Yeeflow variable JSON, recursively parse nested `param.id` expressions, and then validate the semantic expression tree. Manager labels alone are not proof that the expression is valid.

Hard gates:

- `WORKFLOW_ASSIGNEE_EXPRESSION_OUTER_SHAPE_INVALID`
- `WORKFLOW_ASSIGNEE_EXPRESSION_PARSE_FAILED`
- `WORKFLOW_ASSIGNEE_NESTED_EXPRESSION_MISSING`
- `WORKFLOW_ASSIGNEE_NESTED_EXPRESSION_INVALID`
- `WORKFLOW_ASSIGNEE_EXPRESSION_TITLE_VALUE_MISMATCH`
- `ASSIGNMENT_TASK_VARIABLE_UNRESOLVED`

Generator output should match the canonical golden value. Validators should tolerate insignificant whitespace while enforcing the same parsed expression tree.

## Proof Boundary

Serializer and validator success prove export-compatible package structure. They do not prove live task routing. Runtime proof still requires submission, task creation, and observed assignee ownership in the target tenant.
