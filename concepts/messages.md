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
| links | MessageLink \[ \] | **Message links** found in this message. Present if this is a text message | - |
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

Using a client SDK as a channel member, send a user text message.

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

Using a client SDK, send a user file message.

#### Parameters

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| channel | Channel | Channel this message belongs to | ✔ |
| file | File | File to upload as an attachment or ChatKitty external file properties | ✔ |
| progressListener | UploadProgressListener | Listener to be notified as the file upload progresses. | - |

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

{% api-method method="post" host="https://api.chatkitty.com/v1/applications/:appId" path="/channels/:id/messages" %}
{% api-method-summary %}
Sending a system text message
{% endapi-method-summary %}

{% api-method-description %}
Using the Platform API, send a new system text message.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="number" required=true %}
The ID for the channel this message belongs to
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="type" type="string" required=true %}
Type of this message. Always `TEXT`
{% endapi-method-parameter %}

{% api-method-parameter name="body" type="string" required=true %}
The Unicode text body of this message
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a new message resource
{% endapi-method-response-example-description %}

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
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Getting messages

You can retrieve previous messages and observe new messages.

###  Retrieving channel messages 

A user can retrieve previous messages in a [channel](channels.md) he or she is a member of. 

