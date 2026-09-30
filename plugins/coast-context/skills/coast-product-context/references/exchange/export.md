# Exporting work

An **export** produces an external representation of Coast data. Name the object and scope before choosing a format: one record, a selected population of records, a form representation, and a QR or link artifact answer different requests. Export is a snapshot or artifact; it does not establish an ongoing connection to its source records.

## A selected record population

The collection export experience can send a CSV of records from a workflow template to specified email recipients. Its request includes the selected record filters and search term, rather than assuming the entire workspace or only the rows currently loaded in the browser. The applicable [record-selection](../workflows/record-selection.md) rules determine which records match. The request can also carry ordering and a time zone for date conversion. A recipient list controls delivery of the artifact; it does not grant access to the live records in Coast.

When explaining an exported count or an absent record, identify the template, selection criteria, search term, and operation time. The web collection export uses the current selection, including temporary browsing adjustments, rather than only the saved view defaults. A one-time request does not become a scheduled report because it was made from a view.

## One record and other artifacts

A single-card export can produce a PDF representation of one existing record. This differs from the CSV record-set export in both scope and format. A QR code or link can encode a destination for a card or form, but the code or link itself is not a file snapshot of the underlying record data. A [shared-card link](../workflows/view-templates.md#public-forms-and-shared-cards) gives a live view of a record and has its own access boundary.

Choose the actual export operation for the requested object and audience. The existence of CSV, PDF, QR, and other format names in an API does not mean every format is available for every export surface. For recurring delivery or ongoing transfer, establish a separately configured [automation](../workflows/automations.md) or [integration](integrations.md) with that behavior; an export request alone provides neither.
