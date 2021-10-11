---
description: Notifications inform your users when events occur across your application.
---

# Notifications

Notifications inform users of actions that occurred in a context different from their current context. ChatKitty sends notifications to your app through the ChatKitty JavaScript SDK when an action outside of an active chat session occurs. You can observe these notifications and use them to build in-app notification views. ChatKitty also sends notifications to [chat functions](../chat-functions/overview.md) that can be used to trigger push notifications.

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| id | number | 64-bit integer identifier associated with this notification | ✔ |
| title | string | Title of this notification briefly informing the user what occurred | ✔ |
| body | string | Detailed message informing the user what occurred triggering this notification | ✔ |
| data | NotificationData | Additional data related to this notification, including its type | ✔ |
| muted | boolean | True if this notification is muted and should not actively notify the recipient user of its related event | - |
| createdTime | datetime | ISO 8601 date-time when this notification was created | ✔ |
| readTime | datetime | ISO 8601 date-time when this notification was read | - |

## Notification types

### User sent message

Sent when another user [sends a message](messages.md#sending-a-user-text-message) in a channel the recipient is a member of but currently has no active chat sessions.

#### Notification data

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| type | string | Always `USER:SENT:MESSAGE` | ✔ |
| message | Message | The message that was sent | ✔ |
| channelId | number | 64-bit integer identifier of the channel this notification is related to | ✔ |

### System sent message

Sent when a [system message is sent](messages.md#sending-a-system-text-message) using the Platform API in a channel the recipient is a member of but currently has no active chat sessions.

#### Notification data

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| type | string | Always `SYSTEM:SENT:MESSAGE` | ✔ |
| message | Message | The message that was sent | ✔ |
| channelId | number | 64-bit integer identifier of the channel this notification is related to | ✔ |

### User mentioned channel

Sent when another user [mentions a channel](mentions.md#mentioning-a-channel) the recipient is a member of, in a channel accessible to the recipient.

#### Notification data

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| type | string | Always `USER:MENTIONED:CHANNEL` | ✔ |
| message | Message | The mentioning message | ✔ |
| channelId | number | 64-bit integer identifier of the channel with the mentioning message | ✔ |
| mentionedChannel | Channel | The channel that was mentioned | ✔ |

### User mentioned user

Sent when another user [mentions the recipient](mentions.md#mentioning-a-user) in a channel accessible to the recipient.

#### Notification data

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| type | string | Always `USER:MENTIONED:USER` | ✔ |
| message | Message | The mentioning message | ✔ |
| channelId | number | 64-bit integer identifier of the channel with the mentioning message | ✔ |
| mentionedUser | Channel | The user that was mentioned | ✔ |

