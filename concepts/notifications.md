---
description: Notifications inform your users when events occur across your application.
---

# Notifications

Notifications inform users of actions that occurred in a context different from their current context. ChatKitty sends notifications to your app through the ChatKitty JavaScript SDK when an action outside of an active chat session occurs. You can observe these notifications and use them to build in-app notification views. ChatKitty also sends notifications to [chat functions](../platform-api/chat-functions.md) that can be used to trigger push notifications.

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| id | number | 64-bit integer identifier associated with this notification | ✔ |
| title | string | Title of this notification briefly informing the user what occurred | ✔ |
| body | string | Detailed message informing the user what occurred triggering this notification | ✔ |
| data | NotificationData | Additional data related to this notification | ✔ |
| muted | boolean | True if this notification is muted and should not actively notify the recipient user of its related event | - |
| createdTime | datetime | ISO 8601 date-time when this notification was created | ✔ |
| readTime | datetime | ISO 8601 date-time when this notification was read | - |

