---
description: >-
  Integrate real-time chat into your React Native or Web application with the
  ChatKitty JavaScript SDK.
---

# JavaScript Real-Time Messaging Quick Start

## Installing the ChatKitty JS SDK

To use the ChatKitty JavaScript Chat SDK, you'll need to add the [ChatKitty JavaScript SDK](https://www.npmjs.com/package/chatkitty) NPM package to your JavaScript front-end project:

{% tabs %}
{% tab title="NPM" %}
```bash
npm install chatkitty
```
{% endtab %}

{% tab title="Yarn" %}
```
yarn add chatkitty
```
{% endtab %}
{% endtabs %}

## Starting a guest user session

You must create a [**user session**](../concepts/user-sessions.md) ****before a user can begin chatting with other users using ChatKitty. A user session represents a secure bi-directional connection with ChatKitty servers allowing users to send and receive messages in real-time. 

Before you learn about **authenticating users** with ChatKitty, you can create a guest user session. You can start a user session by passing a unique **username** to your [`ChatKitty`](https://chatkitty.github.io/chatkitty-js/classes/chatkitty.chatkitty-1.html) client [`startSession()`](https://chatkitty.github.io/chatkitty-js/classes/chatkitty.chatkitty-1.html#startsession) method. A username is a string that uniquely identifies a user within your application.

```javascript
const result = await kitty.startSession({
  username: 'jane.doe@chatkitty.com'
});

if (result.succeeded) {
  const session = result.session; // Handle session
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

{% hint style="info" %}
We recommend you use uniquely hashed email addresses or phone numbers as usernames.
{% endhint %}

## Creating a direct channel

With a ChatKitty connection established, you can create a **direct channel** to direct message \(DM\) another user.

```javascript
const result = await kitty.createChannel({
  type: 'DIRECT',
  members: [{ username: 'john.doe@chatkitty.com' }],
});

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

## Starting a chat session

Next, start a **chat session** using [`startChatSession()`](https://chatkitty.github.io/chatkitty-js/classes/chatkitty.chatkitty-1.html#startchatsession) to listen to channel events.

```javascript
const result = kitty.startChatSession({
  channel: channel,
  onReceivedMessage: (message) => {
    // handle received messages
  },
  onReceivedKeystrokes: (keystrokes) => {
    // handle received typing keystrokes
  },
  onTypingStarted: (user) => {
    // handle user starts typing
  },
  onTypingStopped: (user) => {
    // handle user stops typing
  },
  onParticipantEnteredChat: (user) => {
    // handle user who just entered the chat
  },
  onParticipantLeftChat: (user) => {
    // handle user who just left the chat
  },
  onParticipantPresenceChanged: (user) => {
    // handle user who became online, offline, do not disturb, invisible
  },
  onMessageRead: (message, receipt) => {
    // handle read receipt for message
  },
  onMessageUpdated: (message) => {
    // handle message changes
  },
  onChannelUpdated: (channel) => {
    // handle channel changes
  },
});

if (result.succeeded) {
  const session = result.session; // Handle session
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Receiving channel messages

ChatKitty lets you register a number of handler methods when starting a chat session. Perhaps most importantly, the [`onReceivedMessage()`](https://chatkitty.github.io/chatkitty-js/classes/chat_session.startchatsessionrequest.html#onreceivedmessage) handler method. This method is called in real-time whenever a new message is sent by a channel member. Typically, this is where you included code to update your UI with the new message.

{% hint style="info" %}
All handler methods are optional. You only needed to register handlers for chat events your application cares about.
{% endhint %}

## Sending messages

To send a message to a channel with an **active chat session**, use [`sendMessage()`](https://chatkitty.github.io/chatkitty-js/classes/chatkitty.chatkitty-1.html#sendmessage).

### Sending a text message

To send a **text message** pass a string `body` to `sendMessage()`.

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

### Sending a file message

To send a **file message** pass a [JavaScript file](https://developer.mozilla.org/en-US/docs/Web/API/File) or [`CreateChatKittyFileProperties`](https://chatkitty.github.io/chatkitty-js/modules/file.html#createchatkittyfileproperties) as `file` to `sendMessage()`. Optionally, you can pass a [`ChatKittyUploadProgressListener`](https://chatkitty.github.io/chatkitty-js/interfaces/file.chatkittyuploadprogresslistener.html) to track file upload progress.

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

## Getting channel message history

To fetch previous messages sent in a channel, use the [`getMessages()`](https://chatkitty.github.io/chatkitty-js/classes/chatkitty.chatkitty-1.html#getmessages) method.

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

