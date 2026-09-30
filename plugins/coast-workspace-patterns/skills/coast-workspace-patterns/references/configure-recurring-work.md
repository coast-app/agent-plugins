# Configure recurring work

Use this recipe to configure repeated records and their first/upcoming defaults. A recurring series generates separate records; a Scheduled Automation invokes a rule relative to a record's Date field. Use `coast-context:coast-product-context` for those product meanings. The optional queue policy below combines them to reveal upcoming work near its due date.

## Configure the series

1. Inspect the active template and choose the Date component that anchors the work. Enable its `allowEntityBatchCreation` setting when the connected component tool supports recurring creation.
2. Choose the repeat schedule through the available recurrence operation or client. Inspect the connected tool's contract for schedule inputs; enabling the component is not itself the act of creating a series.
3. Decide which values should differ between ordinary records, the first occurrence, and upcoming occurrences. Configure the relevant components' `default`, `firstRecurringEntityDefault`, and `upcomingRecurringEntityDefault` where supported.
4. If the series should extend automatically, inspect the Date component's `autoExtendDefault` settings. Choose an extension target that fits the scheduling horizon and workload; the queue example below counts occurrences through a configured field.
5. Inspect the first and later records. Verify their dates, configured defaults, independent identities, and the chosen series-extension behavior. When records are created through another path, verify how that path applies recurring defaults before adding explicit values.

### Classify recurring records

A shared template can use a discriminator Tag to identify a recurring branch. For example, the maintenance configuration assigns its preventive-maintenance Category option to both the first and upcoming occurrences:

```json
{
  "firstRecurringEntityDefault": { "value": ["<pm-category-option-value>"] },
  "upcomingRecurringEntityDefault": { "value": ["<pm-category-option-value>"] }
}
```

Use the customer's actual option value and verify both occurrences. [Author automations](author-automations.md) explains how that discriminator can branch actions. A Category default classifies records; it does not decide whether a queue shows them.

## Optional: reveal near the due date

Use this policy when a team wants future recurring records to stay out of an operational queue until a chosen time before they are due. The records still exist and remain accessible through other views and authorized paths. A planning view can show upcoming records immediately. The policy is a collection-selection choice, not access control or a requirement for recurrence.

### Configure the fields and rule

1. Add a hidden **Visible Tag** with options Visible (`option-1`) and Hidden (`option-2`). Let ordinary records and the first occurrence default to Visible; set upcoming occurrences to Hidden:

   ```json
   {
     "default": { "value": ["option-1"] },
     "firstRecurringEntityDefault": { "value": ["option-1"] },
     "upcomingRecurringEntityDefault": { "value": ["option-2"] }
   }
   ```

2. On the recurring Date component, configure automatic extension if needed. This example uses a 100-record extension target and identifies the field used to count matching series records:

   ```json
   {
     "autoExtendDefault": {
       "impliedFilterComponentId": "<visible-tag-component-id>",
       "minimumInSeries": 100
     }
   }
   ```

   The target is an example, not a Coast-wide batch size or a guarantee that 100 future records always exist. Confirm the active tool's recurrence settings and the records counted by the chosen filter.

3. Create and enable an `ADHOC` automation, **Make Recurring Card Visible**, that updates the current record's Visible field to `option-1`.

4. Add a `SCHEDULED_AUTOMATION` component pointing to that rule and the chosen Date component. This example invokes it one day before the due date:

   ```json
   {
     "automationId": "<make-visible-automation-id>",
     "dateComponentId": "<due-date-component-id>",
     "default": { "automationTriggerDurations": ["-P1D"] }
   }
   ```

   Durations use ISO 8601: `-P1D` means one day before, `-PT1H` one hour before, and `PT0S` the date's exact time. People can override the duration per record. Read the connected tool description for the field's full value shape.

5. Apply the Visible Tag criterion to each saved operational collection that should omit future work. Leave it off planning or administrator views intended to include future occurrences. [Choose record views](choose-record-views.md) covers collection configuration; widgets inherit their backing collection's selection.

### Verify the resulting queue

For a monthly series, the first occurrence is visible immediately and upcoming records start hidden. As each scheduled invocation sets its Visible field, that occurrence enters the filtered queue. Series extension can add more occurrences as the matching count falls below the configured minimum.

Check ordinary creation, the first occurrence, an upcoming occurrence, and a scheduled reveal. Ordinary records should receive the configured Visible default without a separate write. Verify the target automation is enabled and inspect both the record value and the queue; a saved rule alone does not prove the invocation happened.

The [maintenance example](examples/maintenance-workflow.md#recurring-work-in-operational-queues) combines this policy with work queues and dashboard widgets.
