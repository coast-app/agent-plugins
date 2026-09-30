# Recurring work

A **recurring schedule** generates or extends a series of [workflow records](records.md). The schedule belongs to one workflow template and is anchored to a Date component. Each occurrence is a separate record with its own values and lifecycle; the schedule connects those records as a series. Recurrence is record generation, not an [automation](automations.md) trigger or a calendar view.

## Schedule, seed, and occurrences

A schedule can use a calendar recurrence rule or a configured interval offset in days, weeks, months, or years. An offset can include a starting date and time zone. The source or seed record supplies business content for generation, subject to configured field values and overrides. The generated records share the schedule, but they are not repeated renderings of the seed record. Their date values place them in time; a [collection view](view-templates.md#working-with-a-collection) can then present upcoming or overdue occurrences without controlling generation.

The initial occurrence and later occurrences can receive different configured defaults. For example, a recurring Tag field can have an initial selection and a subsequent selection. Those are values on individual records. Editing one occurrence changes that record; changing the schedule or series affects a different scope.

## Extending and changing a series

Automatic extension can maintain a configured minimum number of matching records in a series. Its series filter selects which existing occurrences count toward that minimum, using the shared field-filter concepts in [Record selection](record-selection.md). If too few series records match, extension can generate more. This filter controls the extension count; it is not merely a view filter and does not remove nonmatching occurrences from the series.

Series operations can extend the series or update or delete occurrences. A bulk change may apply to all occurrences or only those from a chosen date onward. The series update path also changes the field values used for future extension; deleting a series selection stops automatic extension and starts a deletion operation for the selected occurrences. These effects are distinct from editing a single generated record. Before changing a series, identify both the existing-record date scope and what should happen to future generation. The operation's completion matters because a bulk update or deletion can continue after the request is accepted.

A monthly inspection illustrates the separation. The schedule creates dated inspection records; a collection view shows the approaching work; a Scheduled Automation field can invoke a reminder relative to each record's date. Those are three configurations with different effects. [Components](components.md#date) explains the Date + Repeat field and managed series association; [Automations](automations.md#date-relative-invocation) owns the reminder behavior.
