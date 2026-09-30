# Reports

A **report** presents analytical content associated with a workflow definition. It helps people examine a question about work, such as how a backlog or cost changed over time. It is distinct from a [collection view](../workflows/view-templates.md), which presents records for operational work, and from a [dashboard widget](dashboards.md), which keeps a count and route to a configured collection in sight.

## What a report names

Coast stores a report's name, association with a workflow template, and reference to an analytical source. The analysis behind that source determines the actual population, measure, and time window. Those analytical choices should be established for the particular report; they are not universal fields or guaranteed properties of every Coast report object. A report's title alone does not establish its calculation, refresh policy, or coverage.

For example, a Preventive Maintenance report may analyze work orders over a selected period. The section where a person finds the report can help them navigate to it, while the linked analysis determines what records and measure it uses. Reorganizing the workspace rail does not change the report's input population.

## Viewing, authoring, and exporting

People find reports through the workspaces and workflow templates available to them. Report listings are limited to templates associated with their workspace memberships, so a report can exist without appearing for every organization member. Opening a report also requires access to its linked analytical content. A viewing route does not establish an editor in every client. When a request is to author or edit an analysis, establish the supported management path and the customer's configuration.

An export from a report is an output of that particular analysis. It should not be confused with exporting a [selected record set](../exchange/export.md) from a workflow collection. Identify the analysis scope, result format, and current export route before promising a file or schedule.

## Choosing the right surface

Use a collection view to act on individual records in a repeatable queue; a dashboard widget when the count and return route should stay visible; a report when the person needs an analytical answer whose population and measure are explicit. [Search](search-and-navigation.md#search-scopes) finds a particular record or message from a clue, while [activity](attention-and-activity.md#activity-and-mentions) shows recent events. These surfaces can refer to the same work without sharing one saved object or one query definition.
