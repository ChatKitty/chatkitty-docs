---
description: Notify users of messages that need their attention directly.
---

# Mentions

Mentions are a direct way to notify users of things that need their attention in a [message](messages.md). Mentions can trigger [notifications](notifications.md) and are embedded inside a message.

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| type | string | The type of this message mention. `CHANNEL`, or `USER` | ✔ |
| tag | string | The literal text referencing the mentioned entity inside the message | ✔ |
| startPosition | number | The starting position of this mention reference inside the message | ✔ |
| endPosition | number | The ending position of this mention reference inside the message | ✔ |

## Mention types

There are two types of message mentions:

### Channel mention

This [notifies all members](notifications.md#user-mentioned-channel) of a [public](channels.md#public-channels) or [private](channels.md#private-channels) channel.

#### Additional Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| channel | Channel | The channel that was mentioned | ✔ |

### User mention

This [notifies a user](notifications.md#user-mentioned-user).

#### Additional Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| user | User | The user that was mentioned | ✔ |

## Mentioning entities

Users can mention channels and users inside their messages using ChatKitty's mention syntax.

### Mentioning a channel

You can mention a channel using the ChatKitty channel mention syntax inside a text message: `<#channelName>` where `channelName` is the name of the channel being mentioned.

```javascript
const result = await kitty.sendMessage({
  channel: channel,
  body: 'Hello, <#my-public-channel>!',
});

if (result.succeeded) {
  const message = result.message; // Handle message
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Mentioning a user

You can mention a user using the ChatKitty user mention syntax inside a text message: `<@username>` where `username` is the username of the user being mentioned.

```javascript
const result = await kitty.sendMessage({
  channel: channel,
  body: 'Hello, <@jane@chatkitty.com>!',
});

if (result.succeeded) {
  const message = result.message; // Handle message
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

