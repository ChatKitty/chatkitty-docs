---
description: 'Users, bots, and system administrators can send messages'
---

# Messages

Messages are the core building blocks of ChatKitty applications. Users send messages using the client SDK, while bots and system administrators can send messages using the Platform API. **Text messages** have a Unicode text body, while **file messages** have a file attached to them. **User messages** are messages sent by end-users of your application through the client SDK. **System messages** are messages sent via the **Platform API** on behalf of the application.

{% hint style="info" %}
Before sending or receiving messages in real-time, a [chat session](chat-sessions.md) must be [started](chat-sessions.md#starting-a-chat-session) for the channel the messages belong to.
{% endhint %}

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| id | number | 64-bit integer identifier associated with this message | ✔ |
| type | string | The type of this message. `TEXT`, or `FILE` | ✔ |
| body | string | The text body of this message. Present if this is a text message | - |
| file | ChatKittyFile | The file attached to this message. Present if this is a file message | - |
| links | MessageLink \[ \] | **Message links** found in this message. Present if this is a text message  | - |
| user | User | The user who sent this message. Absent if this is a system message | - |
| createdTime | datetime | ISO 8601 datetime when this message was created | ✔ |
| properties | object | Custom data associated with this message | ✔ |

## Message types

There are four types of messages:

### User Text Message

Users can send text messages containing a Unicode text body. These messages can contain emojis and other Unicode characters.

### User File Message

Users can send files messages with a file attachment.

### System Text Message

You can send text messages containing a Unicode text body on behalf of your application.

### System File Message

You can send file messages with a file attachment on behalf of your application.

## Sending a message

You can send messages in a [channel](channels.md).

### Sending a user text message

Using a client SDK, send a user text message.

#### Parameters

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| channel | Channel | Channel this message belongs to | ✔ |
| body | string | The Unicode text body of this message | ✔ |

```javascript
const result = await kitty.sendMessage({
  channel: channel,
  body: 'Hello, world!',
});

if (result.succeeded) {
  const message = result.message; // Handle message
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Sending a user file message

Using a client SDK, send a user file message



