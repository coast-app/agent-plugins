# Attention and activity

Several Coast experiences help a person notice work, but they answer different questions. A [message](../organization/conversations.md) says something in a conversation. An unread indicator concerns a person's reading state. A notification calls attention to an event. An activity item records that something happened. None is interchangeable with a record's saved field values or detailed history.

## Read and unread state

A conversation has per-person read state: when its messages were viewed, whether newer content is unread, and where seen-by information can be shown. A workspace can also track unread records separately from unread messages. Viewing a record and reading every message in its thread are not the same act, and a notification does not prove either occurred.

When a screen combines counts, identify which population each number measures. Do not add a record-unread count to a message-unread count as though both were distinct work items without establishing the screen's counting rule. The originating [workspace or record](../organization/workspaces.md) remains the context for the attention signal.

## Notifications and preferences

Notification events, recipient selection, preferences, and delivery are separate stages. A person may choose broad notifications, mentions only, or none in a relevant context; workspace muting or snoozing and personal settings can further affect attention. A mention in message content or a person assigned to a record does not by itself prove that a push, email, or in-app notification reached them. Establish the current event and delivery contract before promising a channel or timing.

Configured [automations](../workflows/automations.md) can also send notifications. The automation rule and its recipient source explain why one was requested; delivery and the recipient's preferences still need separate interpretation.

## Activity and mentions

The **activity feed** gives a person a chronological view of events such as record changes and messages, with the actor and originating work context where available. It is broader than one conversation, while a record's [history](../workflows/records.md) gives detail about changes to that record. A system message in a thread, an activity item, and a notification can relate to one event without becoming one object.

The **mentions feed** helps find messages addressed to a person. It is a route back to message content, distinct from the mention token in that content, notification delivery, and read state. Use [Search and navigation](search-and-navigation.md) when the question is how to find a destination from a term rather than which events call for attention.
