---
description: Chat sessions let users actively chat inside a channel
---

# Chat Sessions

Before a user can begin sending and receiving real-time messages and use in-app chat features like typing indicators, delivery and read receipts, live reactions, etc, their device needs to **start** a chat session. You can think of starting a chat session like _entering a chat room_. After a user starts a chat session, the chat session is **active** until the user **ends** the chat session. Ending a chat session corresponds to _leaving a chat room_. 

{% hint style="info" %}
A user device can start up to 10 chat sessions at a time.
{% endhint %}

## Chat session event handlers

When starting a chat session, you can define **event handlers** to respond to chat events that occur while the chat session is active, as such when a new message is received, a user starts/stop typing, a user has entered or left the chat room, a user went offline, etc.

{% hint style="info" %}
All chat session event handlers are optional, so you only needed to register handlers for chat events your application cares about.
{% endhint %}

| Name | Parameters | Trigger condition \(when called\) |
| :--- | :--- | :--- |
| onReceivedMessage | Message | A message is sent to this channel |
| onReceivedKeystrokes | Keystrokes | Typing keystrokes are made by users in this channel |
| onTypingStarted | User | A user starts typing in this channel |
| onTypingStopped | User | A user stops typing in this channel |
| onParticipantEnteredChat | User | A user starts a chat session in this channel |
| onParticipantLeftChat | User | A user ends their active chat session in this channel |
| onParticipantPresenceChanged | User | A member of this channel changes their presence status \(goes online or offline\) |
| onMessageUpdated | Message | A message sent in this channel has been updated |
| onChannelUpdated | Channel | The channel associated with this chat session has been updated |
| onMessageRead | Message, ReadReceipt | A message sent in this channel has been read |

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| channel | Channel | The channel this session was started for | ✔ |
| end | function | Ends this chat session when invoked | ✔ |

## Starting a chat session

You start a chat session using a channel object and optionally chat session event handler methods to handle chat events.

#### Parameters

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| channel | Channel | The channel this session is associated with | ✔ |
| onReceivedMessage | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onReceivedKeystrokes | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onTypingStarted | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onTypingStopped | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onParticipantEnteredChat | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onParticipantLeftChat | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onParticipantPresenceChanged | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onMessageUpdated | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onChannelUpdated | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |
| onMessageRead | function | [Chat session event handler](chat-sessions.md#chat-session-event-handlers) function | - |

```javascript
const result = kitty.startChatSession({
  channel: channel, // Optional event handlers below...
  onReceivedMessage: (message) => {
    // handle received messages
  },
  onTypingStarted: (user) => {
    // handle user starts typing
  },
  onTypingStopped: (user) => {
    // handle user stops typing
  }, //... and so on.
});

if (result.succeeded) {
  const session = result.session; // Handle session
}

if (result.failed) {
  const error = result.error; // Handle error
}

```

{% hint style="warning" %}
A [user session](user-sessions.md) must be active for the current user before starting a chat session
{% endhint %}

