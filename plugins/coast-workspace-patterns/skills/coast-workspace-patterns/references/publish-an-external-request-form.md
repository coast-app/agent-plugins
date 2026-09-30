# Publish an external request form

View templates shape both card forms and saved collections. This recipe covers a public Card form that accepts submissions from people outside Coast; [choose collection and card views](choose-record-views.md) covers general view selection. For the product meaning and public-link scope, use `coast-context:coast-product-context` and its views and forms guidance.

A tailored external form uses a public Card view to control fields, layout, copy, and defaults. The current `create_shared_card_link` tool also accepts `EXTERNAL_FORM` without `viewTemplateId`, using a default form. Choose a view explicitly when the audience needs a tailored experience; inspect the generated link and resulting form before sharing it.

## Build the public form and link

1. For a tailored form, create a CARD view template with `isPublic: true` (`isPublic` is valid on CARD views, not COLLECTION views).
2. Use `componentsViewOptions` to choose which fields are visible and editable on the form. Configure intentional hidden defaults for any internal classification or triage fields.
3. `create_shared_card_link` with `type: "EXTERNAL_FORM"` and the `workflowTemplateId` generates the public form URL. Pass the chosen public CARD `viewTemplateId` when using a tailored form; an optional `fields` map pre-fills values.
4. Confirm the link's disclosure and submission scope before sending it to people outside the organization.

### Example: external work request

A public Card view can show a title, problem description, optional file, requester contact fields, and location while hiding internal triage and assignment fields. A configured origin Tag and requester email can route a confirmation or status update through automations. See the [maintenance workflow](examples/maintenance-workflow.md#external-requests) for the selected fields, overrides, and notification loop.

## Verify the submission

- An email loop needs its configured origin condition and recipient Email field populated. In the [maintenance example](examples/maintenance-workflow.md#external-requests), the origin Tag value is `["external"]`.
- Verify how the selected form's overrides and link prefills combine before relying on a hidden default such as Pending Review. The submitted record should be inspected in the intended workspace.

## Share an existing record read-only

When someone needs to view an existing card rather than submit a new one, use a public read-only Card view. Call `create_shared_card_link` with `type: "VIEW_CARD"` and that entity's `cardId`; select the intended public Card view if the connected tool contract supports it. Open the generated link to verify exactly which details it discloses. A `VIEW_CARD` link does not accept new submissions.
