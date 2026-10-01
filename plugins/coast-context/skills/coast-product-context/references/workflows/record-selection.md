# Record selection

Record selection answers which [records](records.md) are in a working population, how they are ordered or grouped, and what can be summarized about them. A [collection view](view-templates.md#working-with-a-collection) can save these choices as a shared work queue. A Related Card picker can use filters to offer relevant targets. A dashboard widget can count the population defined by its selected collection view. These uses share criteria concepts while keeping their own configuration and permissions.

Start with the scope: a workflow template identifies the kind of records, and a workspace or consumer supplies the relevant place or context. A filter such as “Status is Open” selects from that scoped population; it does not change the template, move the matching records, or grant access to someone who could not otherwise see them. The same words may produce different populations when the workspace, template, current person, or related record changes.

## Filters and their inputs

A field filter identifies a component and evaluates one of its values with an operator appropriate to that type. Text can be compared with a phrase, a Number with a numeric threshold, a Date with a date window, and a Tag with a configured option. Filters can also ask whether a value is empty. The available operators depend on the field and the surface configuring the filter; a Date criterion and a Person criterion are not interchangeable just because both appear in a filter menu. [Components](components.md) owns the meaning of each field type.

An operand can be **fixed**, such as the Tag option “Open” or a specified date, or **context-dependent**, such as the current person or a date relative to today. “Assigned to me” is evaluated for the person using the view, so different people can see different records from the same saved criterion. “Due this week” changes as the date moves. A saved criterion can therefore be stable even though its matching records change. Where a picker derives a comparison value from the record being edited, the same configured picker can offer different targets for different source records.

In the web Related Card picker, a dynamic criterion whose source field is empty or absent is omitted. Other criteria still apply, but the missing source value does not force an empty picker. This matters when a form's earlier selection is intended to narrow a later one.

Relationship filters can select records using a [Related Card](components.md#related-card) connection, the [Referenced In](components.md#referenced-in) reverse of a connection, or a field reached through a supported relationship path. For example, a Work Order view can select work orders related to an Asset, while an Asset view can focus on assets referenced by matching Work Orders. Where the chosen consumer supports it, a criterion can address a nested value through a configured path rather than a top-level field. A relationship filter follows configured record identities; comparing an asset's typed name as plain text is a different, weaker criterion. The builder or tool in use determines which paths and operators it offers; an arbitrary chain of fields is not guaranteed.

Filters are conditions over a population, and their combination matters. A queue of work that is Open **and** assigned to the current person differs from one that is Open **or** assigned to that person. Configure the intended combination explicitly in the chosen view or tool rather than inferring it from a title such as “My open work.” A search term entered while browsing is another working control; it is not automatically a saved field criterion. [Search and navigation](../across-workspaces/search-and-navigation.md) explains global card and message search, whose scope and matching differ from field filters.

## Ordering and grouping

Sorting orders matching records by a supported component and direction, such as earliest due Date first. A collection can also use custom record order for a deliberate manual sequence. Neither order changes the record's field values or transfers it to another workspace. A board uses groups to arrange a matching population into useful buckets, commonly by a configured Status Tag. A saved group can also carry criteria for which records appear in it. The [view template](view-templates.md#filters-groups-and-sorting) owns the saved group and sort configuration; browsing adjustments can be temporary.

One record can appear in more than one group when the grouping dimension has multiple values. In that case, adding visible group counts can exceed the number of distinct records. For a staffing or cost question, identify whether the desired unit is distinct records, group memberships, or values before using the number.

## Counts and numeric summaries

A count answers how many records satisfy the selected scope and criteria. A numeric summary answers a different question about a selected Number value, such as total estimated cost or average hours. Supported aggregate queries include count and numeric operations such as sum, average, minimum, and maximum; the specific field, grouping, and available operations must be chosen in the consumer that runs the summary. Grouping can produce one result per configured dimension, while a count without grouping describes the population as a whole. A metric is a computed answer over records, not another record kind or a value automatically stored back on each card.

A dashboard entity widget uses a referenced collection view's saved filters and groups for its count and navigation target. [Dashboards](../across-workspaces/dashboards.md) owns widget identity, placement, and display. [Reports](../across-workspaces/reports.md) owns report-specific analysis; a report should state its population, measure, and time range rather than inherit an assumed view query.

## Where selection is used

| Consumer | Selection's job | Other owner |
| --- | --- | --- |
| Collection view | Define a shared work queue and its groups or order. | [View templates](view-templates.md) owns saved versus temporary state and layout. |
| Related Card picker | Narrow the records a person can select as a relationship, sometimes using the current record as context. | [Components](components.md#related-card) owns the relationship field and its target template. |
| Dashboard widget | Count and reopen the population in a selected collection view. | [Dashboards](../across-workspaces/dashboards.md) owns widget configuration. |
| Recurring-series automatic extension | Select records in the series and compare the matching count with the configured minimum. | [Recurrence](recurrence.md) owns generation and extension behavior. |

This shared language does not make the consumers interchangeable. A picker criterion is evaluated while selecting a relationship; a collection view presents an operational queue; a recurrence rule uses matching records to decide whether to generate more. In each case, check the actual source population and context before treating an empty result or count as evidence that no record exists.
