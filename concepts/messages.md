---
description: Users, bots, and system administrators can send messages
---

# Messages

Messages are the core building blocks of ChatKitty applications. Users send messages using the client SDK, while bots and system administrators can send messages using the Platform API. **Text messages** have a Unicode text body, while **file messages** have a file attached to them. **User messages** are messages sent by end-users of your application through the client SDK. **System messages** are messages sent via the [Platform API](../platform-api/overview.md) on behalf of the application.

## Properties

| Name        | Type             | Description                                                                                      | Required |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------ | -------- |
| id          | number           | 64-bit integer identifier associated with this message                                           | ✔        |
| type        | string           | The type of this message. `TEXT`, or `FILE`                                                      | ✔        |
| body        | string           | The text body of this message. Present if this is a text message                                 | -        |
| file        | ChatKittyFile    | The file attached to this message. Present if this is a file message                             | -        |
| links       | MessageLink \[ ] | **Message links** found in this message. Present if this is a text message                       | -        |
| mentions    | Mention \[ ]     | [Mentions](mentions.md) of channels and users in this message. Present if this is a text message | -        |
| user        | User             | The user who sent this message. Absent if this is a system message                               | -        |
| createdTime | datetime         | ISO 8601 date-time when this message was created                                                 | ✔        |
| properties  | object           | Custom data associated with this message                                                         | ✔        |

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

Using a client SDK as a channel member, send a user text message.

#### Parameters

| Name       | Type    | Description                                    | Required |
| ---------- | ------- | ---------------------------------------------- | -------- |
| channel    | Channel | [Channel](channels.md) this message belongs to | ✔        |
| body       | string  | The Unicode text body of this message          | ✔        |
| properties | object  | Custom data associated with this message       | -        |

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

Using a client SDK, send a user file message.

#### Parameters

| Name             | Type                   | Description                                                           | Required |
| ---------------- | ---------------------- | --------------------------------------------------------------------- | -------- |
| channel          | Channel                | [Channel](channels.md) this message belongs to                        | ✔        |
| file             | File                   | File to upload as an attachment or ChatKitty external file properties | ✔        |
| properties       | object                 | Custom data associated with this message                              | -        |
| progressListener | UploadProgressListener | Listener to be notified as the file upload progresses.                | -        |

```javascript
const result = await kitty.sendMessage({
  channel: channel,
  file: file,
  progressListener: {
    onStarted: () => {
      // Handle file upload started
    },

    onProgress: (progress) => {
      // Handle file upload process
    },

    onCompleted: (result) => {
      // Handle file upload completed
    },
  },
});

if (result.succeeded) {
  const message = result.message; // Handle message
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

{% swagger path="/channels/:id/messages" method="post" summary="Sending a system text message" %}
{% swagger-description %}
Using the Platform API, send a new system text message.
{% endswagger-description %}

{% swagger-parameter name="id" type="number" in="path" %}
The ID for the channel this message belongs to
{% endswagger-parameter %}

{% swagger-parameter name="type" type="string" in="body" %}
Type of this message. Always 

`TEXT`
{% endswagger-parameter %}

{% swagger-parameter name="body" type="string" in="body" %}
The Unicode text body of this message
{% endswagger-parameter %}

{% swagger-parameter name="properties" type="object" in="body" %}
Custom data associated with this message
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "id": 26858,
  "type": "SYSTEM_TEXT",
  "body": "Hello, world!",
  "links": [],
  "properties": {},
  "createdTime": "2021-06-01T17:34:30.276Z",
  "_links": {
    "self": {
      "href": "https://api.chatkitty.com/v1/applications/6902/messages/26858"
    },
    "channel": {
      "href": "https://api.chatkitty.com/v1/applications/6902/channels/35204"
    },
    "application": {
      "href": "https://api.chatkitty.com/v1/applications/6902"
    }
  }
}
```
{% endswagger-response %}
{% endswagger %}

## Getting messages

You can retrieve previous messages and observe new messages.

### Retrieving channel messages

A user can retrieve previous messages in a [channel](channels.md) he or she is a member of.

```javascript
const result = await kitty.getMessages({
  channel: channel,
});

if (result.succeeded) {
  const messages = result.paginator.items; // Handle messages
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Observing channel messages

A user can observe new messages in a [channel](channels.md) he or she is a member of.

```javascript
kitty.startChatSession({
  channel: channel,
  onReceivedMessage: (message) => {
    // Handle recevied message
  },
});
```
