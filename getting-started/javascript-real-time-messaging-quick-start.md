---
description: >-
  Create a bi-directional connection with ChatKitty to start sending and
  receiving real-time messages.
---

# JavaScript Real-Time Messaging Quick Start

## Starting a guest user session

You must create a [**user session**](../concepts/user-sessions.md) ****before a user can begin chatting with other users using ChatKitty. A user session represents a secure bi-directional connection with ChatKitty servers allowing your users to send and receiving messages in real-time. 

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

Next, start a **chat session** using this channel to listen to channel events.

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
});

if (result.succeeded) {
  const session = result.session; // Handle session
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

