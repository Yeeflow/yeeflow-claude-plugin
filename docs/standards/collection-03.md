# docs/standards collection 3

This file combines 5 documents from `docs/standards/` for the Claude directory build. A reference to `docs/standards/<name>.md` is the section that starts at the anchor `#<name>` below.

- [workflow-layout-golden-reference-standard.md](#workflow-layout-golden-reference-standard)
- [workflow-query-data-golden-reference-standard.md](#workflow-query-data-golden-reference-standard)
- [yap-approval-form-workspace-generation-standard.md](#yap-approval-form-workspace-generation-standard)
- [yap-export-shaped-application-generation-standard.md](#yap-export-shaped-application-generation-standard)
- [yap-generation-contract.md](#yap-generation-contract)

---

<a id="workflow-layout-golden-reference-standard"></a>

<!-- source: docs/standards/collection-03.md#workflow-layout-golden-reference-standard -->

# Workflow Layout Golden Reference Standard

Generated Yeeflow workflow diagrams must be readable before signing readiness. This standard applies to Approval form workflows, Data list workflows, and Scheduled workflows.

The workflow layout golden reference is based on export-proven workflow JSON examples and screenshot-backed layout review:

- `workflow_ref_ememo`: a complex horizontal approval workflow with branch gateways, lower-lane cancellation/rejection handling, and vertex-routed long transitions.
- `workflow_ref_expense_reimbursement`: a larger reimbursement workflow with multi-lane branching, optional reviewer paths, finance actions, and rounded/vertex-routed sequence flows.
- `workflow_actions_layout`: a focused action-layout reference with Start, Approval Assignment Tasks, Complete Assignment Tasks, Inclusive Gateway amount branches, shared End with Rejection nodes, and one rejected return line using explicit vertices.

## Scope

This standard validates visual workflow graph layout only:

- node coordinates
- graph spacing
- Assignment Task outcome lines
- Inclusive Gateway branch structure
- End with Rejection placement and grouping
- branch/reject lane separation
- SequenceFlow line style
- Workflow Designer v2 graph attributes
- SequenceFlow vertices for return/backward, overlap, node crossing, and long reroute cases
- graphposition initial viewport visibility

It does not prove business behavior, assignment correctness, workflow execution, publish success, or runtime task routing.

## Generation Rules

1. Every non-`SequenceFlow` workflow node must have a top-level `position` object with numeric `x` and `y`.
2. Every generated workflow action node must have a concise business-specific Action name. Defaults such as `Assignment Task`, `Content List`, `Inclusive Gateway`, `Gateway`, `Task`, or `Sequence flow` are invalid. Use names like `Line manager approval`, `Department head approval`, `Finance manager approval`, `Cashier confirm`, `IT security check`, or `Request clarification`.
3. Action names should stay short enough to fit the node card, normally no more than 48 characters.
4. Every generated `SequenceFlow` must include a non-empty business Description in `properties.documentation`. Use concise labels such as `Submitted`, `Approved`, `Rejected`, `Completed`, `Amount >= 5000`, `Amount < 5000`, or `Sensitive data`. Empty strings and defaults such as `Sequence flow_1` are invalid.
5. SequenceFlow descriptions should stay short enough to avoid label collisions, normally no more than 42 characters. Keep full logic in `conditioninfo`.
6. Nodes must not share identical coordinates.
7. Nodes must not use near-identical coordinates where both `x` and `y` deltas are below the golden-reference near-collision threshold.
8. Complex workflows must not be compressed into a single small cluster. Main-path steps should use a horizontal column gap of about `335px`, with a minimum of `305px` for adjacent task columns.
5. Approval Assignment Tasks must have at least `Approved` and `Rejected` outcome SequenceFlows.
6. Complete Assignment Tasks must have a `Completed` outcome SequenceFlow; `Completed` is the canonical spelling.
7. Business-field branches such as `Amount >= 5000` / `Amount < 5000` must be modeled through an `InclusiveGateway`, not as direct condition fan-out from an Assignment Task.
8. Gateway branch target nodes should share the same x column and use separated y lanes.
9. Rejected/cancel/exception paths must use a separate lane from the main path.
10. A shared `EndRejectEvent` must not collect more than three adjacent Approval Assignment Task rejected flows. Split into additional `EndRejectEvent` nodes after three sources.
11. A shared `EndRejectEvent` may only collect rejected flows from Approval Assignment Tasks on the same horizontal lane. Assignment Tasks on different y lanes must use separate `EndRejectEvent` nodes.
12. `EndRejectEvent` placement must follow the source group by node centers, not top-left coordinates: one source aligns with the task center; two or three sources align near the horizontal center of the source task centers.
13. A shared `EndRejectEvent` must collect only local adjacent sources. If two or three rejected sources are far apart on the same lane, split them into separate local `EndRejectEvent` nodes instead of drawing long rejection lines to one global endpoint.
14. If the source Approval Assignment Tasks are on the first workflow row, their shared `EndRejectEvent` must sit on the row above them. If the source tasks are not on the first workflow row, their shared `EndRejectEvent` must sit on the row below them.
15. Workflow diagrams should use the visible Designer canvas as a roughly `16:9` work area. Large workflows should add vertical rows instead of putting all nodes on one long horizontal line.
16. Generated workflows should not exceed five vertical rows/layers.
17. A complex workflow row must not become a dense strip. Fold later actions or branch targets into upper/lower lanes when one row would contain too many nodes or condition labels.
18. Do not compress the standard column spacing just to satisfy a fixed medium-workflow width target. For 14-25 node workflows, `2600px` is an advisory width where the generator should prefer lane folding, not a reason to reduce label, node, or connector clearance.
19. A medium workflow may exceed the advisory width when it uses readable multi-row folding, row density is acceptable, labels do not collide, and connector lanes remain clear. It should fail only when it remains a single/dense horizontal strip, becomes extremely wide, or violates readability gates.
20. Long connector labels must not overlap workflow nodes or other long connector labels. Use separate lanes, local branch routes, or shorter condition labels instead of relying on Designer auto-placement in crowded areas.
21. Long vertical route segments must not stack into one visual wall. If more than two long vertical segments need the same column gap, offset lanes or split the branch into local routes.
22. Generated SequenceFlow line style should be `rounded` by default for complex graphs.
23. Generated workflow roots must use the current Workflow Designer v2 graph attributes: `lineType = "rounded"` and `graphver = 2`. Preserve/default `graphzoom`; zoom differences are not layout defects.
24. Every generated `SequenceFlow` must include `properties.linetype = "rounded"`, a non-empty string `properties.documentation` field, and `dockers = []` so the designer can auto-route rounded lines.
25. Do not add vertices merely because a line is rejected or cross-lane. Add `vertices[]` for return/backward lines, overlapping lines, lines that cross other nodes, and long reroutes that cannot be made readable by standard spacing.
26. Direct same-row connectors must rely on rounded auto-routing. In particular, `StartNoneEvent -> first task` connectors labeled `Submitted` / `Submit` must have empty or absent `vertices[]`.
27. When a connector needs a horizontal segment between two adjacent rows, route that segment at the midpoint of the net row gap: `routeY = (upperRowY + workflowNodeHeight + lowerRowY) / 2`.
28. When a long return/backward connector crosses more than two workflow rows, do not calculate `routeY` from only the source and target rows. Choose an adjacent open row-gap midpoint along the route and ensure the horizontal segment does not pass through any intermediate row's node bounds.
29. When an explicit connector route includes a vertical segment between workflow columns, route that segment through the midpoint of the net column gap: `routeX = (leftColumnX + workflowNodeWidth + rightColumnX) / 2`.
30. Long connector vertical segments must not pass through intermediate workflow node bounds. If the column gap is too narrow or occupied, increase column spacing, choose an adjacent column-gap midpoint, or use a justified external lane.
31. Use an external return lane only for long backward/return/cross-row reroutes where the row-gap/column-gap midpoint is unsafe, crowded, or too narrow.
32. SequenceFlow `source` and `target` references must resolve to existing workflow node ids.
33. `graphposition` follows the runtime-proven content-boundary contract: `x = minNodeX + 90`, `y = minNodeY + 45`, and `width/height` equal the unscaled node-content span with a `470 x 185` minimum for very small workflows. Do not derive it from browser DOM stage translation. Reject both legacy `minNode - margin` values and the obsolete `safeInset - minNode * graphzoom` formula.
34. Inclusive Gateway branches must be local fan-out motifs. Branch targets should sit in a nearby branch column, normally about `195px` from the gateway and never as a far-right target that requires long labels or detour connectors.
35. Normal End nodes with multiple incoming branches must be local merge points beside the incoming source group. Do not place a final End far from the source group and then wrap long completion connectors into it.
36. Local rejected connectors to a nearby `EndRejectEvent` should not carry explicit vertices. If the rejection endpoint is correctly placed above/below the source group, rounded auto-routing is sufficient.
37. Local forward or merge connectors between nearby upper/lower lanes should also use rounded auto-routing with empty `vertices[]`. Do not add explicit vertices to nearby Completed/Approved merge lines merely because the source and target are on different y lanes. If a short local merge needs several bends to look readable, move the branch/merge nodes into a clearer golden-reference motif instead.
38. Connector display labels must stay concise. Use short visible labels such as `Amount >= 5000`, `Sensitive data`, or `Approved`, and keep the full expression in `conditioninfo`; do not put long business sentences on connector labels.
39. A nearby forward branch, including an outcome labelled `Returned for Rework`, must use empty `vertices[]` when its target is to the right and within the local branch distance. The outcome label does not turn that connector into a long backward return route.
40. Every explicit `vertices[]` polyline must be orthogonal: adjacent vertices must share either x or y. A connector that exits above or below its source card must start at the source card center x (within the configured tolerance), so the first visible leg is vertical.
41. Recalculate every explicit route only after the complete node layout is final. Moving a node without recalculating its connected vertices is a generated-final failure.
42. Same-row forward cards must retain at least `110px` of visible edge-to-edge clearance. Prefer the Golden Reference's `130-160px` clearance; fold into another row rather than compressing cards to make a graph narrower.

## Standard Spacing

The `workflow_actions_layout` reference establishes the following default spacing for generated workflow diagrams. Position values are top-left coordinates.

| Constant | Recommended value | Minimum | Usage |
| --- | ---: | ---: | --- |
| `Y_MAIN` | `40` | fixed | Main approval backbone lane. |
| `H_START_TO_FIRST_TASK` | `305` | `305` | Start to first Assignment Task. |
| `H_TASK` | `335` | `305` | Assignment Task to next same-level task. |
| `H_TASK_TO_GATEWAY` | `320` | `305` | Task to Inclusive Gateway. |
| `H_GATEWAY_TO_BRANCH` | `195` | `180` | Inclusive Gateway to branch task column. |
| `H_TASK_TO_END` | `335` | `305` | Final task to normal End. |
| `V_REJECT_UP` | `125` | `115` | End with Rejection above task lane. |
| `V_REJECT_DOWN` | `135` | `125` | End with Rejection below task lane. |
| `V_BRANCH_NEAR` | `115` | `115` | Near branch vertical separation. |
| `V_LANE` | `155` | `135` | Main lane to lower approval branch lane. |
| `ROW_TOLERANCE` | `60` | fixed | Nodes within this y delta count as one horizontal lane. |
| `MAX_ROWS` | `5` | fixed | Maximum generated workflow vertical rows/layers. |
| `ROW_NODE_DENSITY` | `7` | fixed | Maximum suggested nodes in one complex workflow row before folding into another lane. |
| `MEDIUM_WIDTH_ADVISORY` | `2600` | soft | Suggested width where 14-25 node workflows should prefer folding; do not compress columns just to fit it. |
| `MEDIUM_READABLE_WIDTH_ALLOWANCE` | `3400` | hard-ish | Upper allowance for readable multi-row medium workflows before the width itself becomes a blocker. |
| `LONG_LABEL_COLLISION_MIN` | `16 chars` | fixed | Minimum connector label length for label collision checks. |
| `VERTICAL_ROUTE_LANE_MAX` | `2` | fixed | Maximum long vertical connector segments that may share one lane before lane offsets/local routes are required. |
| `END_REJECT_SOURCE_SPAN` | `760` | fixed | Maximum suggested x-span for approval tasks sharing one rejection endpoint. |
| `WORKFLOW_NODE_SIZE` | `190 x 86` | fixed | Standard task/gateway/end node size used for connector midpoint geometry. |
| `ROW_GAP_MIDPOINT_ROUTE_MIN` | `60` | fixed | Preferred minimum net gap for midpoint connector routing. |
| `ROW_GAP_LABELED_MIN` | `40` | fixed | Below this net gap, do not route labeled long connectors through the row gap. |
| `ROW_GAP_ROUTE_Y_TOLERANCE` | `12` | fixed | Allowed pixel tolerance when validating routeY against the row-gap midpoint. |
| `COLUMN_GAP_MIDPOINT_ROUTE_MIN` | `48` | fixed | Preferred minimum net gap for vertical connector routing between columns. |
| `COLUMN_GAP_ROUTE_X_TOLERANCE` | `12` | fixed | Allowed pixel tolerance when validating routeX against the column-gap midpoint. |
| `EXTERNAL_RETURN_LANE_PADDING` | `80-120` | fixed | Padding outside workflow node bounds for true external return lanes. |
| `GATEWAY_BRANCH_LOCAL_MAX_X` | `650` | fixed | Maximum suggested center-x distance from an Inclusive Gateway to a business branch target. |
| `END_MERGE_LOCAL_MAX_X` | `650` | fixed | Maximum suggested center-x distance from the rightmost incoming source to a multi-source normal End node. |
| `END_MERGE_VERTICAL_TOLERANCE` | `130` | fixed | Allowed y padding around incoming source centers for local End merge placement. |
| `CONNECTOR_LABEL_MAX` | `42 chars` | fixed | Maximum suggested visible business-condition connector label length. |
| `LOCAL_REJECT_VERTEX_DELTA` | `760 x 260` | fixed | Local rejected connectors within this x/y distance should not use explicit vertices. |
| `LOCAL_FORWARD_AUTO_ROUTE_DELTA` | `1250 x 360` | fixed | Local forward or merge connectors within this x/y distance should not use explicit vertices. |
| `FORWARD_NODE_CLEARANCE_MIN` | `110px` | fixed | Minimum visible gap between adjacent same-row forward cards. |
| `ROUTE_SOURCE_EXIT_ALIGNMENT_TOLERANCE` | `12px` | fixed | Permitted horizontal variance between a vertical source exit and the source center line. |
| `CANVAS_RATIO` | `16:9` | advisory | Preferred visible Designer canvas usage. |

## Assignment Task Outcome Rules

Approval task:

- `stencil.id = MultiAssignmentTask`
- `properties.tasktype` is empty or not `complete`
- must have an `Approved` SequenceFlow
- must have a `Rejected` SequenceFlow
- `documentation` should display `Approved`, `Rejected`, or a clear business rejection label such as `Reject and return`
- `conditioninfo` must contain the real task outcome condition

Complete task:

- `stencil.id = MultiAssignmentTask`
- `properties.tasktype = "complete"`
- must have a `Completed` SequenceFlow
- usually has only one outgoing SequenceFlow
- use `Completed`, not misspellings such as `comepleted`

## Inclusive Gateway Rules

When a workflow branches on business data, generate:

```text
Approval task Approved -> InclusiveGateway -> business condition branches
```

Do not generate multiple business condition SequenceFlows directly out of an Assignment Task. Place multiple branch target nodes in the same x column when possible, with separate y lanes. This keeps the branch readable and avoids unnecessary vertices.

Gateway branches should remain local. The `workflow_actions_layout` reference places branch targets close to the gateway, then uses upper/lower lanes to express alternative paths. If a branch target is hundreds of pixels beyond the local branch column, move that node into a nearby lane or restructure the section instead of drawing a long condition connector.

## End With Rejection Rules

For Approval Task rejected outcomes:

- one task to one `EndRejectEvent`: align `EndRejectEvent.x` with the task and place it vertically above or below the task
- two tasks to one `EndRejectEvent`: place it near the horizontal midpoint of the two tasks
- three tasks to one `EndRejectEvent`: place it near the center of the three tasks, usually near the middle task x
- calculate the alignment using node center points, not the top-left `position.x`
- when source Approval Assignment Tasks are on the first workflow row, place the shared `EndRejectEvent` above that row
- when source Approval Assignment Tasks are on a lower workflow row, place the shared `EndRejectEvent` below that row
- shared rejection endpoints are valid only when the source Approval Assignment Tasks are on the same horizontal lane
- source tasks on different vertical lanes must use separate `EndRejectEvent` nodes, even when there are three or fewer rejected sources
- source tasks that are too far apart on the same lane should also use separate local `EndRejectEvent` nodes
- more than three rejected sources: create another `EndRejectEvent`

This prevents many rejected lines from stacking into one crowded endpoint.

## Complex Workflow Lane Strategy

For workflows with many approvals, gateways, and post-approval actions, do not keep extending the main row indefinitely. Use the golden-reference lane pattern:

- main approval backbone on the center row
- condition-specific review branches on upper or lower rows
- request clarification / return paths on a lower row with explicit `vertices[]`
- local rejection endpoints above the first approval row or below lower approval rows
- later procurement, fulfillment, or archive actions folded into another lane when the main row becomes dense

The target visual width for most generated workflows should stay around `1800-2200px`. For medium-complexity workflows, `2600px` is an advisory threshold that should trigger lane folding and readability review. It must not make the generator compress standard column spacing or overlap labels. A wider workflow is acceptable when it uses readable rows, keeps row density under control, avoids connector label collisions, and does not create shared vertical route walls.

The normal End node is also a layout motif. When several branch outcomes complete into the same End, place the End as a local merge beside the incoming source group, similar to the reference `End_1` placement. Avoid a single far-away End that forces multiple long wraparound completion lines.

## Workflow Designer V2 Line Style Rules

The current Yeeflow Workflow Designer expects generated workflow graphs to use rounded auto-routing metadata:

- root `lineType: "rounded"`
- root `graphver: 2`
- each `SequenceFlow.properties.linetype: "rounded"`
- each `SequenceFlow.properties.documentation` present as a non-empty concise business Description string
- each `SequenceFlow.dockers: []`

Do not generate legacy orthogonal/full-docker routes. Keep `vertices[]` only where explicit routing improves readability, such as return/backward paths, overlap avoidance, node crossing, or long reroutes.

`graphzoom` is not a fidelity gate; keep the reference/default value rather than forcing a new zoom.

## Canvas Aspect and Row Utilization Rules

The Workflow Designer visible canvas is approximately `16:9`. Generated diagrams should use that shape rather than creating one long, narrow strip of nodes.

For complex workflows:

- keep the main approval backbone readable from left to right
- when the number of nodes grows, continue into lower rows/layers instead of extending the main lane indefinitely
- use at most five vertical rows/layers
- keep related sibling branches aligned by column when possible
- avoid a single horizontal row with ten or more nodes and an overly wide aspect ratio

The validator treats five rows as the hard maximum and flags large single-row sprawl as a readability failure.

## Vertices Rules

Default to no vertices when standard spacing makes the line readable. The workflow layout standard is node-placement first and connector-routing second.

If a connector needs many vertices to avoid looking chaotic, the source/target nodes are probably in the wrong place. Rebuild the node layout using the golden-reference motifs instead of adding more bends:

- main approval backbone
- upper/lower branch lanes
- local gateway fan-out
- local `End with rejection` endpoints
- lower action/complete lanes

Vertices are a last-mile correction, not the primary layout mechanism. In generated workflows with eight or more nodes, non-return/non-rejection connectors that require vertices should be rare. A graph with many such routed connectors must fail validation and be regenerated with better node placement.

Require vertices when:

- the target node is to the left of the source node, such as `Reject and return`
- the line overlaps another line for a meaningful segment
- the line crosses through or too close to another node
- the route is a long cross-lane reroute that cannot be made readable through spacing alone

Simple direct same-row connectors should not have route vertices:

```text
vertices = []
```

This is mandatory for `Start -> first task` / `Submitted` or `Submit` connectors when the source and target are adjacent and unobstructed.

Local forward/merge connectors should also stay vertex-free when the source and target are nearby. For example, a `Completed` line from an upper branch task to the next local merge task, or an `Approved` line from a nearby lower branch task back into a local process step, should rely on Designer rounded auto-routing. Do not add diagonal or multi-bend `vertices[]` just because the nodes are on different y lanes.

For return lines that can travel in the open space between adjacent rows, route the horizontal segment through the midpoint of the net row gap:

```text
sourceCenterX = source.x + taskCenterOffset
targetCenterX = target.x + taskCenterOffset
upperRowBottom = upperRowY + workflowNodeHeight
lowerRowTop = lowerRowY
routeY = upperRowBottom + ((lowerRowTop - upperRowBottom) / 2)
vertices = [
  { x: sourceCenterX, y: routeY },
  { x: targetCenterX, y: routeY }
]
```

Use an external return lane below or above the graph only when the row gap is too narrow, already crowded, or the connector spans many logical workflow groups. Do not send short local return lines to arbitrary fixed y values.

Do not add vertices to a local outcome branch merely because its label contains `Return`, `Returned`, or `Rework`. For example, an Approval Task that sends `Returned for Rework` to a nearby lower-right `Return stage gate` is a local forward branch and must leave `vertices = []`; Workflow Designer will render its rounded bend. Reserve explicit vertices for the later genuinely leftward/backward return route.

For an explicit lower return route, the first point below the source must align with the source center line. Thus, if the source card center is `(1625, 433)`, the route begins with a point such as `(1625, 530)` before travelling horizontally. Do not reuse a previous source's x coordinate after moving the node.

For long return/backward lines that cross multiple workflow rows, first cluster all rows between source and target. The horizontal segment must use an open adjacent row gap, not the midpoint between the source row and target row when an intermediate row exists. For example, if rows occupy `160..246`, `320..406`, and `480..566`, a return from the third row to the first row should use the first open gap midpoint `283`, not `(160 + 86 + 480) / 2 = 363`, because `363` passes through the middle row.

For long cross-row connectors that also need a vertical segment, first cluster workflow columns along the route. The vertical segment must use an open adjacent column gap, not a task center or a node edge when that x-coordinate overlaps another node. For example, if one column occupies `1260..1450` and the next occupies `1565..1755`, the vertical lane should use `1508`, the midpoint of the net column gap. A vertical segment at an x-coordinate inside any intermediate node's `left..right` bounds is invalid even when the horizontal `routeY` is correct.

Do not trade readability for a hard width number. The medium-width limit is advisory: it should encourage branch folding, not column compression. Standard node spacing, label readability, and low vertex usage are more important than forcing every medium workflow under `2600px`.

## Generator Guidance

When materializing App Plan workflow nodes:

- Place Start on the left.
- Place approval/task/gateway nodes on the main lane.
- Place data-write/action nodes on a lower action lane when they follow an approval outcome.
- Place rejected/cancel endpoints on a lower reject lane.
- Only share an `EndRejectEvent` between Approval Assignment Tasks on the same horizontal lane.
- For large workflows, use additional rows/layers up to the five-row maximum instead of placing every node on one row.
- Keep End on the right of the last main/action step.
- Route return/rework transitions down or up and across with two or more vertices.
- Use vertices for cross-lane approved/complete transitions only when standard spacing would make a diagonal line overlap other lines or cross nodes.
- Do not use vertices for local Completed/Approved merge transitions when the source and target are nearby branch/action lanes. Rounded auto-routing is cleaner; explicit bends in this case usually signal that node placement should be adjusted.
- When generating vertices for a long connector, choose both a safe horizontal `routeY` from row-gap midpoint rules and a safe vertical `routeX` from column-gap midpoint rules. Do not leave the vertical segment on a task/gateway center line when that would run through another node's bounds.
- Before adding vertices, try moving branch/action nodes into the reference motif lanes. If multiple forward or branch connectors need explicit vertices, the layout engine should move nodes, not add bends.

## Hard Gate

Run:

```bash
node scripts/validate-workflow-layout-golden-reference.mjs --package <generated-final.yapk>
```

This gate is also part of `scripts/yapk-first-generation-preflight.mjs`.

The gate must fail generated packages that contain:

- missing node positions
- duplicate or near-duplicate node positions
- compressed complex workflow canvas
- unresolved SequenceFlow endpoints
- rejected/cross-lane flows without readable routing
- invalid `graphposition` dimensions, content-span mismatch, or origin mismatch against the runtime-proven boundary inset
- shared End with Rejection nodes collecting sources from different horizontal lanes
- workflows with more than five vertical rows
- large workflows compressed into one overly wide horizontal row
- medium workflows that exceed the advisory width without readable multi-row folding
- connector labels overlapping nodes or other long connector labels
- too many long vertical connector segments sharing one routing lane
- overuse of vertices on non-return/non-rejection connectors
- connector paths whose detours are far longer than the direct source-target distance

---

<a id="workflow-query-data-golden-reference-standard"></a>

<!-- source: docs/standards/collection-03.md#workflow-query-data-golden-reference-standard -->

# Workflow Query Data Golden Reference Standard

## Scope

This standard covers the `QueryData` workflow node in Approval Form workflows, Data List workflows, and Scheduled Workflows. It is separate from Form Action `querydata` steps.

Focused sources: `Approval form workflow sample-V1.6.yapk`, `Approval form workflow sample-v1.7.yapk`, and `Approval form workflow sample-v1.10.yapk`.

Proof boundary: the node, variable, List/Complex Type, Loop, assignment, Custom Service invocation, and condition structures are export-proven. Scheduled triggering, query execution, loop execution, assignment routing, email delivery, and Custom Service execution remain runtime-proof boundaries.

## Business Selection

Use Workflow Query Data when a server-side workflow needs data that is not already available in its current workflow context.

Preferred modes:

| Business need | Mode | Result contract |
| --- | --- | --- |
| Existence/count decision | `multiple_count_only` | No row target; save `totalCount` to a number workflow variable |
| Read one related record | `single_to_variables` | Map selected source fields to declared workflow variables |
| Process multiple rows | `multiple_to_list_variable` | Map selected source fields into a List variable backed by one ListRef/Complex Type definition |
| Store multiple rows as JSON text | `multiple_to_text_variable` | Use only when a downstream expression, Custom Service, or other proven JSON consumer requires text |

Do not query merely to display rows in a page. That is a form/dashboard decision and should normally use Collection or Data Table directly.

## Shared QueryData Node Contract

- Node stencil is `QueryData`.
- `properties.appid`, `listsetid`, `listid`, and `listtype` must resolve to an included source resource.
- Export-proven source types are Data List (`listtype = 1`), Document Library (`listtype = 16`), and Form Report (`listtype = 32`). Data Report remains `focused-learning-required` until its own training round.
- `filters` and `sorts` are arrays.
- Dynamic filter operands are expression-token arrays and use `showCus: false`.
- A Lookup comparison must compare `ListDataID` to the stored Lookup target identity, not its display title.
- `pageIndex` is a positive integer and defaults to `1`.
- `pageSize` is `1..1000`; use only the amount required by the business operation.
- `sorts[]` contains at most two entries. Each entry uses `SortName` and boolean `SortByDesc`.
- Workflow Query Data writes to normal workflow variables. It does not use page temp variables.

## Count-Only

Count-only is still `result.type = "multiple"`, but it intentionally has no row result target:

- `listName`, `vartype`, and `listParent` are empty strings;
- `fieldMap` and `fields` are `null`;
- `totalCount` names a declared number variable;
- `querycount_prefix = "__variables_"`.

The downstream branch must cover all possible count states. A common complete pair is `count > 0` and `count <= 0`. Yeeflow has no implicit default/else branch.

## Single Result to Workflow Variables

- `result.type = "single"`.
- `fieldMap` maps source field storage names to declared workflow variable IDs.
- Every target variable type must be compatible with the source field type.
- A queried User field mapped to a User workflow variable may feed a later Assignment Task assignee using the canonical workflow-variable expression-button shape.
- Branches after the query should handle both value-present and value-empty states when the returned value is optional.

## Multiple Results to List Variable

- The destination variable has `type = "list"` and `value = <listref-id>`.
- `variables.listref[]` contains that ListRef/Complex Type definition.
- `result.listName` is the List variable ID.
- `result.listParent = "__variables_"`.
- `result.vartype = "list"`.
- `result.fieldMap` maps every selected source field to a real field ID in the linked ListRef.
- `result.fields` is `null`; that array belongs to text/JSON result selection, not List-variable row mapping.

## Loop Modes

### Loop Through List Items

V1.6 proves a workflow List variable source:

- `Loop.properties.loopType = "list"`;
- `loopValue.prefix = "__variables_"`;
- `loopValue.value` resolves to a declared List variable;
- `bodyRef` resolves to the Loop body start node;
- body expressions use `type = "loop_ctx"`, `param.defid = <Loop node id>`, and `prop = "LoopItem.<ListRef field id>"`.

V1.7 additionally proves Data List Workflow Sub List iteration:

- `loopType = "list"`;
- `loopValue.prefix = "__list_"`;
- `loopValue.value` is the Sub List field ID;
- current iteration uses `exprType = "loop_ctx"`, `id = <Loop node id>`, `key = "LoopIndex"`;
- current row fields use `key = "LoopItem.<Sub List field id>"`.

### Loop Through Multiple Values

V1.7 proves:

- `loopType = "values"`;
- `loopValue.type = 2`;
- `loopValue.value` is a non-empty expression-token array that resolves to a multi-value value, such as a multiple User field;
- no `loopValue.prefix` is serialized;
- `bodyRef` resolves to a `LoopBody` node.

### Loop For Fixed Times

V1.7 proves:

- `loopType = "number"`;
- `loopValue.type = 2`;
- `loopValue.value` is an expression-token array; a fixed count uses `{ "type": "num", "value": "3" }`;
- no `loopValue.prefix` is serialized;
- `bodyRef` resolves to a `LoopBody` node.

The sample body sends email and then uses `Delay` with `type = "duration"`, `duration.count = "3"`, and `duration.unit = "Day"`. Repeated email and delay execution remain runtime-sensitive and require explicit safe proof.

For all modes, Loop body actions live under the matching `LoopBody.children[]`. Validators and generators must recursively inspect those nested actions rather than scanning only top-level `childshapes[]`.

## Data List Workflow Count-and-Upsert Pattern

V1.7 export-proves a `WorkflowType = 1` workflow registered through the host Data List `FlowMappings[]`:

- current Campaign `ListDataID` is used as a `list_field` expression in the QueryData filter;
- count-only output writes to a number workflow variable;
- an Inclusive Gateway covers `count > 0` and `count <= 0`;
- the positive branch uses `ContentList type = "edit"` with the same current-record identity filter;
- the zero/non-positive branch uses `ContentList type = "add"`.

This is the canonical exists-then-update/otherwise-add pattern for employee balances, budgets, inventory, and similar aggregate/register scenarios. The target fields and mutations remain business-specific.

V1.7 also proves two valid new-item `FlowMappings.FieldName` shapes: `null`, or a field that resolves to a real host-list `flowstatus` field. A normal text/business field must not be used as a substitute flow-status mapping.

## Document Library and Form Report Sources

V1.10 export-proves both source types in a Data List Workflow and in Data List custom-form Form Actions.

Document Library workflow query:

- source `listtype = 16`;
- Event lookup filter compares the document-library lookup field with the current Event `ListDataID` expression;
- rows map native document fields such as `Title` and `Text4` into a declared List variable backed by a matching ListRef;
- the sample uses `pageIndex = 2`, `pageSize = 3`, and two sorts: `Created desc`, then `Bigint2 asc`.

Form Report workflow query:

- source `listtype = 32`;
- the sample filters the Form ID storage field with the export `is not empty` operator (`op = "7"`, `right = null`);
- Form Report storage fields map into a declared List variable and matching ListRef fields;
- the sample uses `pageIndex = 3`, `pageSize = 1000`, and two sorts: submission date descending, then applicant ascending.

Form Report is a valid Query Data source but cannot host its own independent Form Action. A Form Action on an Approval, Data List, Document Library, or Dashboard surface may query it. Do not confuse source capability with host capability.

Form Action uses the same source type values through `attrs.querydata_list.ListType`. Single-result examples keep the normal single-query contract: omitted `querydata_type`, `querydata_fieldmap`, and `querydata_fields = null`. Targets still follow the host rules: workflow variables on Approval forms, temp/current fields on Data List or Document Library forms, and temp variables only on Dashboard.

## Custom Service and Email Chain

A List variable can be passed to an included Custom Service through an `InvokeCode` parameter with `valueType = "list"`. A text output can be saved to a declared text workflow variable and consumed by a later Send Email node.

The V1.6 sample contains a literal organizer email. Treat that as sample-specific data, never as a reusable golden value. Generated workflows must use an approved workflow variable, list field, configured recipient, or an explicitly approved safe test recipient.

## Host Consistency

Approval, Data List, and Scheduled workflows use the same `QueryData` result contract. Host context differs:

- Approval workflows use `WorkflowType = 2` and may reference Approval workflow variables and Approval fields.
- Data List workflows use `WorkflowType = 1` and may additionally use current-record/list-field context where export-proven.
- Scheduled Workflows use `WorkflowType = 3`, have `ListID = 0`, and have no current approval instance or page temp variables; declare every result, count, List, and output variable explicitly.

The QueryData node contract is shared, but the enclosing workflow resource, trigger, and context expressions are host-specific. Do not copy an Approval workflow envelope into a Data List or Scheduled Workflow.

## Current Generation Boundary

The shared full-app materializer currently emits QueryData nodes through the Approval Workflow (`WorkflowType = 2`) resource builder. V1.6/V1.7 provide export-backed contracts and validators for Scheduled/Data List Workflow QueryData, List variables, ListRef/Complex Types, all three Loop modes, InvokeCode, MailTask, Delay, and ContentList composition, but they do not by themselves complete the missing Scheduled/Data List Workflow resource materializer.

Until those host builders are wired into full-app materialization:

- Scheduled/Data List Workflow QueryData may be planned and validated against this standard;
- generated Scheduled/Data List Workflow payloads require a focused export-shaped builder and proof;
- generation must fail or report `focused-proof-required` rather than silently omit the workflow or claim full-app completion;
- Approval Workflow QueryData may use the shared materializer modes implemented by this training.

## Required Validation

Generated-final validation must reject:

- multiple count-only nodes incorrectly forced to have `listName`;
- multiple List results without a List variable and matching ListRef;
- `fieldMap` targets absent from the linked ListRef;
- single-result mappings to undeclared or type-incompatible workflow variables;
- invalid page size/page index;
- more than two sort entries or malformed sort entries;
- source types other than export-proven `1`, `16`, or `32` without a new focused learning baseline;
- count targets that are missing or not number variables;
- Loop list mode targeting a non-List variable or missing ListRef;
- Loop `bodyRef` that does not resolve;
- Data List Sub List Loop that does not use `__list_` or whose field is unresolved;
- values/number Loop that incorrectly requires or emits `loopValue.prefix`;
- values/number Loop without `loopValue.type = 2` and a non-empty expression-token array;
- nested LoopBody actions skipped by workflow action validation;
- Lookup filters that use display text instead of stored target identity;
- query-derived User assignment that does not use a declared User variable;
- count branches that do not cover the complementary zero/non-zero case.

## Source Discrepancy Preserved

The V1.6 `Query Campaigns` node exports with `filters: []`, despite the accompanying business description saying it filters by `SelectedCampaign`. Do not promote that absent filter as a golden filter example. Use the export-proven `Get Campaign Owner` filter for the canonical Lookup `ListDataID` expression-token shape.

---

<a id="yap-approval-form-workspace-generation-standard"></a>

<!-- source: docs/standards/collection-03.md#yap-approval-form-workspace-generation-standard -->

# YAP Approval Form Workspace Generation Standard

## Purpose

This standard captures the YAP-specific lessons from the successful Service Desk Form Workspace Smoke v8 import. It applies only when a generated `.yap` package intentionally creates an approval/requester form as an operational workspace, such as `multi_column_form_workspace_shell`.

It does not make every YAP form a workspace and does not require real approval routing. A simple Start to End workflow is acceptable when the form is being used as a workspace shell.

This standard is subordinate to the generated-final YAP contract:

- `docs/standards/collection-03.md#yap-generation-contract`
- `docs/standards/collection-03.md#yap-export-shaped-application-generation-standard`

Passing `schemas/yap-schema.json`, Type `105` navigation, `Data.Forms[].ListID = 0`, and `formdef.children` is necessary but not sufficient. Generated-final approval/form-workspace YAP packages must also be export-shaped and import-qualified across ID shape, `Resource.ReplaceIds`, root/list/layout/field metadata, app-level structures, decoded runtime-critical strings, and proof-boundary reporting.

## Materialization Requirements

YAP approval/form workspaces must follow the export-proven materialization shape:

- `Data.Forms[].ListID = 0` for Service Desk Pro-style approval/form workspace entries.
- Root app navigation must include a Type `105` form navigation item.
- The Type `105` navigation `ListID` must point to the form key.
- `Resource.FormKeys[]`, `Data.Forms[].Key`, root Type `105` navigation key, and embedded `DefResource.defkey` / `DefResource.key` must align.
- Use stable export-like form keys, for example `SDP-WS-V4`, instead of weak generated keys that drift between form, nav, and DefResource.
- Use export-observed string IDs for `DefResource.pageurls[].id` and workflow `childshapes[].id` / `resourceid`.
- Keep full export-style DefResource metadata: `name`, `title`, `workflowType`, `AppListSetID`, `ProcModelAppID`, `ProcModelListID`, `ProcModelListSetID`, `ext`, `lineType`, `iconURL`, `flowPage`, `variables`, `graphposition`, `graphzoom`, and `graphver`.

If these rules fail, import can succeed while the approval form is missing or unreachable.

## Designer-Readable Form Tree

Generated YAP form workspace content must use the designer-readable export shape:

- Put form content in `DefResource.pageurls[].formdef.children`.
- Do not use unsupported `formdef.controls`.
- Use lowercase export-style controls.
- For Service Desk-style generated approval/form workspaces, prefer the designer-load-proven control family from the successful v8 smoke: `list`, `container`, `heading`, and `icon`.
- Do not generate `collection` or `dynamic-field` controls in this YAP approval/form workspace pattern unless the exact control shape is copied from an export-proven form workspace and validated as designer-load safe. Generated approximations of those controls caused the form designer to keep loading after import.
- Controls should include export-style `attrs`, `actions`, `exts`, `formAction`, and `ver` where applicable.
- The designer-readable tree must contain expected root children and nested containers/controls.

The failed smoke packages proved that `formdef.controls` and generated PascalCase control names such as `Container`, `Text`, and `Icon` can import as a blank approval form.

The later fresh smoke loop proved a second designer failure mode: a package can import and publish, but the approval form designer can keep loading when the form tree contains generated, non-export-proven `collection` or `dynamic-field` controls. The successful recovery used the user-confirmed v8 designer-loadable form tree and freshened only app/list/form IDs and package identity.

## Designer-Qualified Approval Form Controls

Generated approval forms must be designer-hydration safe, not merely schema-valid or import-accepted.

- Use designer-safe export-proven control families only for generated approval form YAP workspaces unless another control family has been separately studied and validated.
- Generated form content must use `formdef.children`, not `formdef.controls`.
- Every generated form control must have a unique designer-level `id`.
- IDs must be unique across the entire form tree.
- Parent/child and control-reference fields must resolve to those unique IDs.
- Missing, duplicated, or unresolved control IDs must fail generated-final validation.
- Heading/text controls must populate native rendered text metadata, not only label/display metadata.
- For heading controls, populate `attrs.headc.title.value`.
- Do not allow heading controls to fall back to default placeholder text such as `Here is the title`.
- If the generator cannot confidently populate native text metadata, it must fail generated-final validation.
- Control labels and native rendered title values must be synchronized where both exist.

A generated approval form is not designer-qualified until it imports successfully, opens in the designer, renders intended text, and selecting one control selects only that control. API accepted/queued is not designer proof, and import complete is not designer hydration or selection proof.

The Research v0.6.11 smoke cycle proved this gap: one generated package rendered heading controls as the default placeholder because it set labels without `attrs.headc.title.value`, and another selected many controls at once because generated controls lacked unique designer IDs. A manually edited export with unique IDs and native heading title metadata clarified the required generation shape.

## Data List And Sample Row Import Shape

YAP approval/form workspace import is sensitive to child data-list shape even when the form itself is the main test target.

For Service Desk-style YAP workspaces:

- Preserve native `Title` as the first field.
- Start generated business fields at `Text2`, not `Text1`, when following the v8 Service Desk workspace export pattern.
- Do not use generated `Lookup1` for the Ticket Activities ticket reference in this pattern; the successful v8 shape uses `Text2`.
- Store sample data in `ListDatas` as a record-ID-keyed object. Do not wrap rows in `ListDatas.Datas`.
- Every sample row key must resolve to a declared `Defs[].FieldName`.
- Keep field `Rules` in export-style stringified JSON such as `"{}"` or a stringified `choices` object.
- Preserve runtime-safe native `Title` even when the canonical YAP schema reports the known digit-suffix warning.

The failed fresh smoke packages proved these as hard pre-import rules. The async importer accepted packages with API `Status = 0` but failed later when sample rows used keys such as `Datetime8` against `Text8` definitions, when rows were wrapped under `ListDatas.Datas`, or when Service Desk fields drifted to `Text1` / `Lookup1` numbering instead of the v8-proven `Text2...` shape.

## ReplaceIds Remapping Coverage

Generated YAP approval/form workspace packages must rebuild `Resource.ReplaceIds` from the final decoded package after every generation, ID remap, repair, or baseline copy. Do not reuse a successful baseline package's `ReplaceIds`.

Include every generated local ID that Yeeflow must remap during import:

- root app/ListSet ID
- child data-list IDs
- field IDs
- view and custom-form layout IDs
- approval form/process IDs
- workflow shape/resource IDs
- app group IDs when generated
- local sample record `ListDataID` keys

Exclude tenant/user metadata such as `TenantID`, `CreatedBy`, and `ModifiedBy`, and exclude external dependency IDs.

The broad Service Desk form-workspace YAP test proved this as a hard async-import rule: the package passed local shape checks and API queue acceptance, but async import failed until `Resource.ReplaceIds` was rebuilt from the final broad package and covered all generated child list, layout, field, form/process, workflow, and sample record IDs.

## Minimal Workflow Graph

Even no-submit workspace forms need a complete workflow graph for the workflow designer:

```text
StartNoneEvent -> SequenceFlow -> EndNoneEvent
```

Required graph rules:

- Include `StartNoneEvent`.
- Include `SequenceFlow`.
- Include `EndNoneEvent`.
- Start must have outgoing sequence linkage.
- End must have incoming sequence linkage.
- SequenceFlow must include both source and target references.
- Start `taskurl`, `taskUrl`, and `TaskUrl` should point to the requester form page when those fields are present.

Do not add fake approval routing. If there is no real approval process, keep the graph minimal and document the form as a workspace shell.

## Workspace Page Settings

Workspace-style approval/requester forms should set:

- `hideHeader: true`
- `attrs.container.cw = "2"` for Full width
- `attrs.container.padding["1"] = { top: 0, right: 0, bottom: 0, left: 0 }`

The app chrome and workspace shell provide the visible context. The default form header, narrow content width, or default page padding can break multi-column layout.

## Inline Control Width

Generated controls should use inline width behavior for:

- Icon
- Text / heading
- Button
- Dynamic field

For export-style controls this means `attrs.common.positioning.widthtype = [null, "2"]` unless a proven variant uses an equivalent inline setting.

Other controls should still receive explicit width behavior appropriate to the layout. Do not leave control width behavior undefined when the control participates in a multi-column workspace.

## Icon Size And Context

Icon controls must be inline, bounded, and contextually selected:

- Icons in text/menu rows should match nearby text size.
- The v8 smoke package used 16px icons next to 16px menu text.
- The proven v8 shape used `attrs.icon.size = [null, "--sp--s200"]`, `attrs.common.width = [null, 16]`, and `attrs.common.height = [null, 16]`.
- Use meaningful icons such as inbox, at, pencil, users, flag, folder, search, ellipsis, plus, or chevron according to context.
- Do not use generic placeholder icons in generated-final workspaces.

Oversized or full-width icons can distort narrow columns and collapse surrounding text.

## Multi-Column Overflow

For 3-column, 4-column, or other multi-column workspaces, each column needs its own vertical scroll behavior.

Observed overflow options:

1. Default
2. Hidden
3. Auto
4. Scroll

Working Service Desk v7/v8 pattern:

- Shell: Hidden
- Left navigation panel: Scroll
- Ticket collection panel: Scroll
- Ticket collection scroll region: Scroll
- Selected ticket workspace panel: Scroll
- Selected ticket body scroll region: Scroll
- Right attributes panel: Scroll

Generation rules:

- Use Hidden at the outer shell where needed to keep the workspace bounded.
- Use Scroll or Auto for panel columns and body regions.
- Do not rely on Default overflow for multi-column workspace panels.
- Do not let content expand the whole form vertically when each column should scroll independently.

## Application Color Note

The user-updated successful export also included application color setting changes where Neutral color used the Luminance light model rather than the older Lightness setting. Treat this as a product-export-safe theme note until a broader color-setting fixture confirms exact field-level validator rules.

## Scoped Validator Errors

Validators should apply these checks only to generated YAP approval/form workspaces or when a runtime test explicitly asks for this pattern:

- `YAP_FORM_LISTID_MATERIALIZATION_INVALID`
- `YAP_FORM_NAV_TYPE105_MISSING`
- `YAP_FORM_KEY_MISMATCH`
- `YAP_FORM_DEFKEY_MISMATCH`
- `YAP_FORMDEF_CONTROLS_UNSUPPORTED`
- `YAP_FORMDEF_CHILDREN_MISSING`
- `YAP_FORM_CONTROL_EXPORT_SHAPE_INVALID`
- `YAP_FORM_DESIGNER_UNPROVEN_CONTROL`
- `YAP_FORM_DESIGNER_HYDRATION_RISK`
- `YAP_FORM_CONTROL_ID_MISSING`
- `YAP_FORM_CONTROL_ID_DUPLICATE`
- `YAP_FORM_CONTROL_ID_REFERENCE_INVALID`
- `YAP_FORM_DESIGNER_SELECTION_RISK`
- `YAP_HEADING_NATIVE_TITLE_MISSING`
- `YAP_HEADING_NATIVE_TITLE_PLACEHOLDER`
- `YAP_TEXT_NATIVE_VALUE_MISSING`
- `YAP_CONTROL_LABEL_NATIVE_TEXT_MISMATCH`
- `YAP_FORM_DESIGNER_READABLE_TREE_MISSING`
- `YAP_CHILD_LISTTYPE_MISSING`
- `YAP_LIST_EXPORT_METADATA_INCOMPLETE`
- `YAP_LAYOUT_EXPORT_METADATA_INCOMPLETE`
- `YAP_NORULE_INVALID`
- `YAP_FORM_STATUS_NOT_PUBLISHED`
- `YAP_LISTDATAS_KEYED_RECORDS_MISSING`
- `YAP_LISTDATA_FIELD_UNKNOWN`
- `YAP_FIELD_RULES_NOT_STRING`
- `YAP_SERVICE_DESK_FIELD_NUMBERING_DRIFT`
- `YAP_WORKFLOW_START_MISSING`
- `YAP_WORKFLOW_SEQUENCE_MISSING`
- `YAP_WORKFLOW_END_MISSING`
- `YAP_WORKFLOW_START_NOT_LINKED`
- `YAP_WORKFLOW_END_NOT_LINKED`
- `YAP_WORKFLOW_SEQUENCE_SOURCE_TARGET_MISSING`
- `YAP_WORKFLOW_START_TASKURL_MISSING`
- `YAP_FORM_PAGE_HEADER_NOT_HIDDEN`
- `YAP_FORM_PAGE_WIDTH_NOT_FULL`
- `YAP_FORM_PAGE_PADDING_NOT_ZERO`
- `YAP_ICON_WIDTH_NOT_INLINE`
- `YAP_TEXT_WIDTH_NOT_INLINE`
- `YAP_BUTTON_WIDTH_NOT_INLINE`
- `YAP_DYNAMIC_FIELD_WIDTH_NOT_INLINE`
- `YAP_CONTROL_WIDTH_BEHAVIOR_MISSING`
- `YAP_ICON_SIZE_MISMATCH`
- `YAP_ICON_SIZE_LAYOUT_RISK`
- `YAP_ICON_CONTEXT_GENERIC`
- `YAP_WORKSPACE_SHELL_OVERFLOW_INVALID`
- `YAP_WORKSPACE_COLUMN_SCROLL_MISSING`
- `YAP_WORKSPACE_PANEL_OVERFLOW_DEFAULT_RISK`
- `YAP_WORKSPACE_SCROLL_REGION_MISSING`
- `LOCAL_ID_NOT_IN_REPLACEIDS`
- `LOCAL_ID_NOT_IN_REPLACEIDS_TRUNCATED`
- `REPLACEIDS_EMPTY`
- `YAP_EXPORT_SHAPE_TOO_SYNTHETIC`
- `YAP_LOCAL_ID_SHAPE_INVALID`
- `YAP_PAGEURL_ID_SHAPE_INVALID`
- `YAP_CHILD_SHAPE_ID_SHAPE_INVALID`
- `YAP_APPROVAL_DEFRESOURCE_METADATA_INCOMPLETE`
- `YAP_WORKFLOW_GRAPH_METADATA_INCOMPLETE`
- `YAP_REPLACEIDS_APP_GROUP_ID_MISSING`

The native Title schema conflict remains a warning, not a hard failure:

- `YAP_NATIVE_TITLE_SCHEMA_CONFLICT`

Message:

`Canonical YAP schema currently requires digit-suffixed FieldName, but runtime-safe native Title uses FieldName/InternalName Title. Preserve runtime-safe Title and track schema update separately.`

---

<a id="yap-export-shaped-application-generation-standard"></a>

<!-- source: docs/standards/collection-03.md#yap-export-shaped-application-generation-standard -->

# YAP/YAPK Export-Shaped Application Generation Standard

When generating a new `.yap` application package, do not emit a thin synthetic skeleton. Generate an export-shaped, import-qualified YAP package that matches real Yeeflow app exports closely enough for async import/materialization.

For generated-final `.yapk` packages, apply the same principle at AppPackageInfo resource level: resources must be export-shaped enough for signing, install/materialization, designer opening, navigation, and browser runtime proof. Do not satisfy an approved App Plan with count-only resources, synthetic placeholders, dashboard shells, hidden implementation hosts, or incomplete approval/report definitions.

## A. Build From An Export-Like Baseline

- Use a proven exported YAP structure as the structural base.
- Preserve export-style wrapper, resource, and data shapes.
- Do not invent minimal JSON shapes just because validators accept them.
- For new app generation, use synthetic business content but export-like technical structure.
- The plugin must work without user-provided exported YAP samples.
- A bundled sanitized baseline/reference shape must be sufficient for first-time YAP generation.
- If no safe baseline exists, the generator must refuse import-qualified claims and report missing baseline.

## B. Baseline Reference Sanitization Rules

Any bundled or committed reference baseline must be normalized, tenant-neutral, structurally export-like, and free of copied customer/business data, tenant/user/workspace/private IDs, private URLs, raw package payloads from private apps, raw `Resource`, and raw `Sign`.

Replace tenant/user/workspace/private IDs with generated local placeholders. Do not commit raw sample application files.

## C. Use Export-Style IDs

- Use Yeeflow-compatible long numeric-string local IDs.
- Keep ID type/shape consistent with export patterns.
- Avoid short synthetic numeric IDs for app/list/field/layout/form/workflow/sample IDs.
- Preserve real-export type shape per location.
- If real exports use strings for `pageurls` / `childshapes`, use strings there.
- If schema currently says integer but real exports use string, validators should report schema/runtime mismatch rather than force an invalid runtime shape.

## D. Rebuild Resource.ReplaceIds From The Final Decoded Package

`Resource.ReplaceIds` must not be empty for generated YAP packages with local generated resources. Rebuild it after every generation or repair step.

Include every generated local app/list set ID, child list ID, field ID, layout/view ID, form/process ID, workflow node/resource ID, app group ID, sample record ID, and other generated local resource IDs. Exclude tenant/user metadata and external dependency IDs. Never copy `ReplaceIds` from a baseline package. Fail generation if `ReplaceIds` is empty or incomplete.

## E. Preserve Full Root/List/Layout Metadata

Generated root and child list models must include export-style metadata, not only title, list ID, and type. Include import-safe values for `WorkspaceID`, `TenantID`, `AppID`, `TableCode`, `IndexCode`, flags/status fields, `Perm`, `Ext1`, `Ext2`, `Ext3`, audit fields where baseline-safe, and valid `LayoutView`.

## F. Generate Full Field Metadata

Each field should include export-like metadata: `FieldID`, `ListID`, `AppID`, `FieldName`, `InternalName`, `DisplayName`, `FieldType`, `Type`, `Category`, `FieldIndex`, `Status`, `IsSystem`, `IsIndex`, stringified `Rules`, and storage family aligned with `FieldName`.

## G. Generate Full Layout/View Metadata

Each data list must have import-ready default views: Type `0` default view, visible columns resolving to real fields, query fields resolving to real fields/system fields, `Ext1.Url = "default"`, and export-style layout metadata and IDs.

## H. Approval Form DefResource Must Be Export-Shaped

For approval/form workspaces, include `Data.Forms[].ListID = 0`, root Type `105` navigation to the form key, aligned `FormKeys`, `Data.Forms[].Key`, root nav key, and `DefResource.key/defkey`.

Use `DefResource.pageurls[].formdef.children`; do not use `formdef.controls`. Use designer-safe controls only unless export-proven. Include `name`, `title`, `workflowType`, `AppListSetID`, `ProcModelAppID`, `ProcModelListID`, `ProcModelListSetID`, `ext`, `lineType`, `iconURL`, `flowPage`, `variables`, `graphposition`, `graphzoom`, `graphver`, workflow node `id/resourceid`, positions, source/target references, and task URL fields.

For YAPK approval forms, `Data.Forms[].DefResource` must use the canonical base64 encoding of bytes beginning with `::brotli::` followed by Brotli-compressed JSON. Generated-final validation must fail minimal placeholder definitions, missing request/task page metadata, missing `formdef.children`, duplicated designer control IDs, missing workflow graph positions, unresolved task URLs, missing variables, missing `MultiAssignmentTask`, missing task assignee metadata, missing Approved/Rejected paths, or key/defkey mismatch. Do not pass validation by deleting a planned approval form.

Approval form designer hydration is part of import qualification for generated approval workspace packages. Every generated control in `formdef.children` must have a unique designer-level `id`; IDs must be unique across the full form tree and any control references must resolve. Heading/text controls must set native rendered text metadata, including `attrs.headc.title.value` for headings, and must not fall back to placeholder text such as `Here is the title`. Control labels and native rendered values should stay synchronized. Use only designer-safe export-proven approval controls unless another control family has separate designer proof.

## I. Preserve App-Level Export Fields

Include expected app-level structures even when empty: `SimplePortal`, `PortalInfo`, `AppTags`, `AppMetadatas`, `AppComponents`, `AppThemes`, `OtherModules`, report arrays, permissions arrays, and app group structures where needed.

## J. Validate Decoded Runtime-Critical Strings

Generated-final validation must parse and validate decoded `DefResource`, stringified field `Rules`, form `Settings`, layout/view `Ext1`, `Ext2`, `Ext3` JSON where applicable, workflow graph strings where applicable, and `LayoutView` where applicable. Fail generated-final validation when runtime-critical strings are malformed.

## K. Native Title Exception Policy

Allow native Title schema conflict only as warning: `YAP_NATIVE_TITLE_SCHEMA_CONFLICT`. Do not fix native Title into an artificial generated field name if that breaks runtime semantics. Preserve runtime-safe native `Title` with `FieldName: "Title"`, `InternalName: "Title"`, and native/system metadata consistent with real exports.

## L. Validate Import Qualification, Not Only Schema

Before handoff/import, run YAP schema validation, YAP generation contract validation, YAP package validator, graph validator, approval/form workspace inspector, data-list/default-view checks, decoded-string checks, `ReplaceIds` final coverage check, export-shaped metadata completeness check, and safety scan.

For generated-final YAPK handoff, run `scripts/yapk-first-generation-preflight.mjs`; it includes `scripts/validate-generated-yapk-export-shape.mjs` before signing readiness. The export-shape gate rejects count-only `FormNewReports`/`DataReports`, dashboard pages without visible resource-bound business controls, Summary/chart controls without the complete runtime-proven model contract, hidden Summary hosts counted as visible content, and native Title fields missing export-aligned `Status: 0`, `IsSystem: true`, and `IsIndex: true`.

Generated-final YAPK packages must be preflight-clean before `setsign`. Treat `scripts/yapk-first-generation-preflight.mjs --package <file.yapk> --plan <yeeflow-app-plan.md> --json` as the signing-readiness entrypoint when a plan exists. Do not sign a package that fails canonical schema validation, decoded export-shape validation, generated-final resource completeness, ID provenance, app icon, navigation metadata, dashboard materialization, or redacted-output checks. Signing can prove only package wrapper integrity; it cannot repair app content.

Generated-final YAPK packages must not embed business seed rows in `Childs[].ListDatas`, `Childs[].List.ListDatas`, or `Childs[].List.Items`. Sample/demo data belongs in a separate companion seed artifact and may be inserted only through an explicitly approved post-install live seeding step, normally `items.batchCreate` with `TriggerFlow: false` plus before/after count proof. Keeping seed rows outside the package prevents import failures and keeps schema validation focused on application structure.

Dashboard KPI cards must be backed by real Summary controls when dynamic metrics are claimed. A KPI card is not generated-final materialized if it is only static text, a styled number, or a hidden Summary host. The package-side contract must include the Summary control, `Resource.ReportIds[]`, `Resource.exts[]`, `Resource.tempVars[]`, `attrs.save_var`, and a visible text binding to the same saved temp variable.

Dashboard filters must be consumed by at least one Collection/table/KPI consumer. Filter controls with bare scalar operator/value placeholders such as `0`, or filters that expose UI state without a matching consumer query/filter binding, are not generated-final ready. Static package validation proves the linkage contract only; browser/runtime proof is still required before claiming data changes after a filter selection.

Dashboard generated-final quality must also be domain-normalized. Visible Dashboard text must not expose source-template residue such as `All tasks - Multiple select`, `Active Survey Programs`, source-only Event/Survey/Project labels in unrelated apps, or generic control labels such as `Grid`, `Container`, `Text`, `Placeholder`, `Dynamic field`, or `Dynamic user`. Every visible Collection column/card/action must be rewritten to the current App Plan field/action contract or removed before signing readiness.

For approval forms, validate three separate gates: import-qualified means the package is export-shaped and locally import-ready; designer-qualified means the form opens in designer, intended text renders, and selecting one control selects only that control; runtime-qualified means the opened app/form behaves correctly in the target runtime.

## M. Proof Language Rule

Never call a YAP import successful from API status `0` / `Completed=false`.

Use separate proof levels: local validation passed, API accepted/queued, async import completed, app opened, form designer hydrated, controls render with intended text, single-control designer selection verified, and workflow designer hydrated. Generated reports must include a proof boundary field and must fail validation if they label API accepted/queued as import successful.

For YAPK install/import, API `Status: 0` is also only submitted/accepted. Final install success requires async materialization proof and browser/runtime evidence against the decoded package root `ListSetID`. Runtime proof must fail if the app tile or page shows `Install failed`, `Start to build with Components`, `ADD NEW COMPONENT`, model-load errors, empty dashboards after seed, or visible source-domain/control-name residue.

---

<a id="yap-generation-contract"></a>

<!-- source: docs/standards/collection-03.md#yap-generation-contract -->

# YAP Generation Contract

## Purpose

`schemas/yap-schema.json` is the hard structural schema for readable `.yap` packages. Generated-final YAP packages must pass this schema before output, signing-boundary handoff, API import, or any claim that the package is ready. Schema pass is necessary, but it is not the whole import-qualification contract: generated-final YAP packages must also pass this generation contract and the relevant package/workflow validators.

Generated-final YAP packages must use a FontAwesome application icon in the top-level wrapper `IconUrl`. The value must be a JSON string containing `b`, `i`, and `c`; `i` must be a FontAwesome class string such as `fa-solid fa-screwdriver-wrench`; `b` and `c` must be valid colors. Image URLs, `https://img.yeeflow.com/...`, SVG, emoji, blank/null values, or non-FontAwesome icon tokens are invalid. The App Plan or generation report should record the selected icon rationale, but must not include generated package IDs.

Northpeak hardening: generated data-list fields must keep `FieldType` inside the canonical enum `Text`, `Bit`, `Decimal`, `Datetime`; UI/control type belongs in `Type`. Generated approval forms must include request/task pageUrl registration and a real assignment-task workflow graph when the design is an approval workflow.

Product schema refresh v0.6.17: generated custom YAP fields must use storage-code `FieldName` values, not business labels. For custom fields, `FieldName` must exactly equal `FieldType + FieldIndex`, such as `Text1`, `Decimal1`, `Bit1`, or `Datetime1`. `Datetime` is the canonical date/date-time storage spelling; legacy `DateTime` must fail for generated packages. Business labels belong in `DisplayName`. The native system title field must remain `FieldName = "Title"`, `IsSystem = true`, and `FieldIndex = 0`.

Use this contract only for plugin-generated final packages or explicit import-qualified runtime tests. Existing customer/product exports should use compatibility mode: report useful warnings, but do not globally reject valid historical exports just because they do not match current generated-final requirements.

## Required Generated-Final Contract

- Generate an export-shaped package, not a thin synthetic skeleton.
- Use a bundled sanitized export-like baseline/reference shape when the user has not provided a safe exported sample.
- Use Yeeflow-compatible long numeric-string local IDs for app, list, field, layout, form/process, app group, and sample record IDs.
- Preserve export-observed string ID shape for `DefResource.pageurls[].id` and workflow `childshapes[].id` / `resourceid`.
- Rebuild `Resource.ReplaceIds` from the final decoded package after every generation or repair step.
- Fail generated-final validation when `Resource.ReplaceIds` is empty or misses local generated IDs.
- Include full export-style root, child list, field, layout/view, approval/process, workflow graph, and app-level metadata.
- Custom data-list fields use `IsSystem = false`, `FieldIndex >= 1`, and storage-code `FieldName = FieldType + FieldIndex`; native `Title` uses `IsSystem = true` and `FieldIndex = 0`.
- Include app-level structures even when empty or null: `Resource.SimplePortal`, `Data.PortalInfo`, `AppTags`, `AppMetadatas`, `AppComponents`, `AppThemes`, `OtherModules`, report arrays, permission/group arrays, and app group structures where needed.
- Parse decoded/stringified runtime-critical fields before output: `DefResource`, field `Rules`, form `Settings`, layout/view `Ext1` / `Ext2` / `Ext3`, `LayoutView`, and workflow graph strings where applicable.
- Preserve native `Title` semantics. `YAP_NATIVE_TITLE_SCHEMA_CONFLICT` is warning-only; do not replace native `Title` with an artificial generated field name when that breaks runtime semantics.
- Generated reports must include proof boundaries. API accepted/queued is not async import success.
- Approval form workspaces must be designer-qualified: every `formdef.children` control has a unique designer-level `id`, all control references resolve, and selecting one control must not select many controls.
- Generated approval form workspaces must use designer-safe export-proven control families unless another family has separate designer proof.
- Heading/text controls must populate native rendered text metadata. Heading controls must set `attrs.headc.title.value` and must not emit default placeholders such as `Here is the title`.
- Generated child lists must include `ListType`, full list/layout export metadata, valid approval `NoRule`, published approval form status, and export-shaped approval DefResource metadata.
- `Resource.ReplaceIds` must include generated app group IDs.

## Dedicated Error Codes

Generated-final validation should hard-fail these conditions:

- `YAP_EXPORT_SHAPE_TOO_SYNTHETIC`
- `YAP_GENERATION_CONTRACT_MISSING`
- `YAP_LOCAL_ID_SHAPE_INVALID`
- `YAP_ID_TYPE_RUNTIME_MISMATCH`
- `YAP_REPLACEIDS_EMPTY`
- `YAP_REPLACEIDS_FINAL_COVERAGE_INCOMPLETE`
- `YAP_ROOT_METADATA_INCOMPLETE`
- `YAP_CHILD_LIST_METADATA_INCOMPLETE`
- `YAP_FIELD_METADATA_INCOMPLETE`
- `YAP_LAYOUT_METADATA_INCOMPLETE`
- `YAP_DEFAULT_VIEW_EXPORT_SHAPE_INCOMPLETE`
- `YAP_APP_LEVEL_EXPORT_FIELDS_MISSING`
- `YAP_APPROVAL_DEFRESOURCE_METADATA_INCOMPLETE`
- `YAP_WORKFLOW_GRAPH_METADATA_INCOMPLETE`
- `YAP_PAGEURL_ID_SHAPE_INVALID`
- `YAP_CHILD_SHAPE_ID_SHAPE_INVALID`
- `YAP_LISTDATAS_KEYED_RECORDS_MISSING`
- `YAP_FIELD_RULES_NOT_STRING`
- `YAP_FIELD_RULES_JSON_INVALID`
- `YAP_DEFRESOURCE_DECODE_INVALID`
- `YAP_FORM_SETTINGS_JSON_INVALID`
- `YAP_LAYOUT_EXT_JSON_INVALID`
- `YAP_PROOF_BOUNDARY_MISSING`
- `YAP_REPORT_API_QUEUED_MARKED_SUCCESS`
- `YAP_FORM_CONTROL_ID_MISSING`
- `YAP_FORM_CONTROL_ID_DUPLICATE`
- `YAP_FORM_CONTROL_ID_REFERENCE_INVALID`
- `YAP_FORM_DESIGNER_SELECTION_RISK`
- `YAP_HEADING_NATIVE_TITLE_MISSING`
- `YAP_HEADING_NATIVE_TITLE_PLACEHOLDER`
- `YAP_TEXT_NATIVE_VALUE_MISSING`
- `YAP_CONTROL_LABEL_NATIVE_TEXT_MISMATCH`
- `YAP_FORM_DESIGNER_UNPROVEN_CONTROL`
- `YAP_FORM_DESIGNER_HYDRATION_RISK`
- `YAP_CHILD_LISTTYPE_MISSING`
- `YAP_LIST_EXPORT_METADATA_INCOMPLETE`
- `YAP_LAYOUT_EXPORT_METADATA_INCOMPLETE`
- `YAP_NORULE_INVALID`
- `YAP_FORM_STATUS_NOT_PUBLISHED`
- `YAP_REPLACEIDS_APP_GROUP_ID_MISSING`

## Proof Boundary

Never call API status `0` with `Completed=false` an import success. Use separate proof levels:

- local validation passed
- API accepted/queued
- async import completed
- app opened
- form designer hydrated
- workflow designer hydrated
- controls render with intended text
- single-control designer selection verified
