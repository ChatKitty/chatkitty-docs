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
| type | string | The type of this channel. `DIRECT`, `PUBLIC`, or `PRIVATE` | ✔ |
| name | string | The unique name of this channel | ✔ |
| creator | User | The user who created this channel. Absent if the channel was created with the Platform API | - |
| lastReceivedMessage | Message | The message that was last received in this channel | - |
| properties | object | Custom data associated with this channel | ✔ |

## Channel types

There are three types of channels:

### Direct Channels

Direct channels let users have private conversations with **up to 9** other users. New users cannot be added to a direct channel and there can only exist one direct channel between a set of users.

### Public Channels

Public channels let users discuss topics openly. By default, any user can view and join a public channel. Users can join public channels by themselves or via invites from an existing channel member.

### Private Channels

Private channels are for topics that should not be open to all members. Users must be **added** to a private channel by someone who's already a member of the channel.

#### Additional Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| members | User \[ \] | An array of member users of this channel | ✔ |

## Creating a channel

Create a new channel of channel type \(`DIRECT`, `PUBLIC`, or `PRIVATE`\) with a unique name for public and private channels and member references for direct channels. A user is automatically a member of a channel they created.

### Creating a direct channel

```javascript
const result = await kitty.createChannel({
  type: 'DIRECT',
  members: [
    { username: 'jane@chatkitty.com' },
    { username: 'john@chatkitty.com' },
  ],
});

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Creating a public channel

```javascript
const result = await kitty.createChannel({
  type: 'PUBLIC',
  name: 'my-first-public-channel',
});

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Creating a private channel

```javascript
const result = await kitty.createChannel({
  type: 'PRIVATE',
  name: 'my-first-private-channel',
});

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

## Getting channels

Get all channels accessible to the current user regardless of current membership status.

```javascript
const result = await kitty.getChannels();

if (result.succeeded) {
  const channels = result.paginator.items; // Handle channels
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Getting joined channels

Get channels the current user has joined \(is already a member\) and can start **chat sessions** in.

```javascript
const result = await kitty.getChannels({ filter: { joined: true } });

if (result.succeeded) {
  const channels = result.paginator.items; // Handle channels
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Getting joinable channels

Get channels the current user can join, becoming a member.

```javascript
const result = await kitty.getChannels({ filter: { joined: false } });

if (result.succeeded) {
  const channels = result.paginator.items; // Handle channels
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Getting a channel

Get a channel by searchable properties like channel ID

```javascript
const result = await kitty.getChannel(channelId);

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

## Joining a public channel

```javascript
const result = await kitty.joinChannel({ channel });

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Observing user joined channel

When the current user joins a channel or is added to a channel by another user or through the Platform API, registered channel observers are notified.

```javascript
const unsubscribe = kitty.onJoinedChannel((channel) => {
  // handle channel
});

// call when you're no longer interested in updates
unsubscribe();
```

## Reading a channel

Read a channel to automatically **read** all channel messages.

```javascript
const result = await kitty.readChannel({ channel });

if (result.succeeded) {
  const channel = result.channel; // Handle channel
}

if (result.failed) {
  const error = result.error; // Handle error
}
```

### Checking if a channel is unread

A channel is unread if it has any unread messages by the current user.

```javascript
const result = await kitty.getChannelUnread({
  channel: channel,
});

if (result.succeeded) {
  const unread = result.unread; // Handle if unread 
}
```

