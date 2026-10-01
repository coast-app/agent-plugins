# Dashboards and widgets

A **dashboard** brings operational summaries and routes back to work together within an organization. An **entity widget** is a saved block on a dashboard that names the workspace, workflow template, and [collection view](../workflows/view-templates.md) it uses. It summarizes or links to records from that configured view; the widget is not another record, a copy of the collection, or a separate filter language.

## From a view to a summary

A collection view defines a saved record selection and presentation. A widget can use that view's saved criteria to count matching records and return a person to the source collection. For example, an Open Work widget can show a count and open the corresponding work list. The count's unit and scope come from the selected records and widget behavior, not from a section name or an arbitrary dashboard title.

[View templates](../workflows/view-templates.md#working-with-a-collection) owns which criteria are saved for everyone and which browsing changes are temporary. [Record selection](../workflows/record-selection.md) owns filter, grouping, and summary meanings. A person's temporary filter on a list should not be assumed to rewrite the widget's saved population.

## Personalizing a dashboard

A person can favorite an entity widget. That favorite is an association between the person and the widget; it changes which widgets they want close at hand without changing the widget's query or the underlying records. This association is stored on the server. The dashboard's **All** versus **Favorites** display choice is remembered locally by the client and is different from the widget association itself.

This is also different from a locally stored [workspace favorite](search-and-navigation.md#returning-to-a-destination) and an organization bookmark. A dashboard widget summarizes a configured record population; a workspace favorite is a route back to a workspace; a bookmark names a URL. Use the object that answers the person's task instead of treating all three as one kind of favorite.

## Dashboards and reports

A dashboard supports recurring operational attention: counts and shortcuts connected to live collections. A [report](reports.md) presents an analysis associated with a workflow; its population, measures, and time context depend on that analysis. A report's navigation placement or title is not a substitute for establishing what it analyzes.
