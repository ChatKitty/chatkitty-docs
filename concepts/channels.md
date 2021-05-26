---
description: >-
  Channels are the backbone of the ChatKitty chat experience. Users join
  channels to receive or send messages.
---

# Channels

ChatKitty organizes conversations into dedicated contexts called channels. Channels structure and order your application's chat experience. You can create a channel for any topic, project, or team.

After a user **joins** a channel, the user becomes a **channel member**. ChatKitty broadcasts messages created in channels to channel members with active **chat sessions** and sends **push notifications** to offline members.  ChatKitty persists messages sent in a channel by default but this behaviour can be configured.

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| id | number | 64-bit integer identifier associated with this channel | ✔ |
| type | string | The type of this channel. `PUBLIC`, `PRIVATE`, or `DIRECT` | ✔ |
| name | string | The unique name of this channel | ✔ |
| creator | User | The user who created this channel. Absent if the channel was created with the Platform API | - |
| properties | object | Custom data associated with this channel | ✔ |

## Channel types

There are three types of channels:

### Public Channels

Public channels let users discuss topics openly. By default, any user can view and join a public channel. Users can join public channels by themselves or via invites from an existing channel member.

### Private Channels

Private channels are for topics that should not be open to all members. Users must be **added** to a private channel by someone who's already a member of the channel.

### Direct Channels

 Direct channels let users have private conversations with **up to 9** other users. New users cannot be added to a direct channel and there can only exist one direct channel between a set of users.



