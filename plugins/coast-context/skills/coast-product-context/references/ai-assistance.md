# AI assistance

Coast AI works in the context of an organization, workspace, record, or form. **Ask** is conversational assistance; **Capture** assists with values in a create or update form. The context supplied to an AI interaction does not by itself grant access to every record or authorize every action. An AI conversation is distinct from the human messages and comments described in [Conversations](organization/conversations.md).

## Ask and its work context

Ask helps a person answer questions about work in the selected organization, workspace, or record context. It can look up accessible workspaces, people, records, messages, activity, and counts or summaries, then present an answer with links to the work it found. For example, a person can ask about open work orders or a card's discussion. The selected scope, the person's authorization, and the available tools shape the answer.

Ask can also offer a reviewable record proposal when a suitable form view is available. The person reviews the draft and saves accepted changes separately. A request to change Coast data needs the specific supported action and a confirmed result: an Ask reply or an in-progress tool indicator alone is not evidence of a saved record, posted message, or completed external effect. Neither a read-only restriction nor a separate confirmation gate should be assumed for every tool-assisted action. Tools available through other programmatic interfaces are not automatically part of Ask.

An **agent thread** keeps an AI conversation and its context. A **run** is a turn of agent work in that thread; history and feedback can refer to a run. Those identities do not replace a workspace's human chat thread or a record's comments.

## Capture and the current form

Capture works inside a selected create or update [view](workflows/view-templates.md#captures-form-context). The view identifies the fields and applicable form rules that the person is using. The **baseline** is the current draft at the time assistance is requested. On an update form, that draft may already differ from the last saved record; on a create form, there may be no saved record yet. Capture can use text and supported attachments to propose changes to that form rather than importing a file as a batch of new records.

## Proposals, review, and saving

A proposal presents field values for review. An omitted field means leave its current value alone; an explicit clear means remove its value. A proposed person or related record must resolve to a real object rather than an invented identifier. Unresolved references need review before they become form selections. The proposed fields are constrained by the selected view and the current task; a proposal does not change the underlying workflow template.

Receiving a proposal is not a saved record write. The person can review and refine proposed changes in the draft, then use the form's save operation to persist the accepted values. If assistance is requested again after an unsaved edit, the current draft is the relevant baseline. A tool that only proposes values does not execute that save. [Records](workflows/records.md) owns persistence and identity; [Components](workflows/components.md) owns field values and relationships.
