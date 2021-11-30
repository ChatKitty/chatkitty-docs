# Introduction

## Simple and convenient Chat API and real-time messaging SDK

```javascript
const kitty = ChatKitty.getInstance(CHATKITTY_API_KEY);

useEffect(() => {
  // start real-time chat session
  let result = kitty.startChatSession({
    channel: channel,
    onReceivedMessage: (message) => {
      showMessage(message); // update your UI as new chat events occur
    },
  });

  return result.session.end;
}, []);
```

ChatKitty is the first complete chat platform; bringing together everything that's required to build real-time chat into Web and mobile apps. ChatKitty provides all the features you need to build modern chat out of the box including:

* **Direct messaging:** Provide secure and encrypted direct messaging to your users.
* **Public and private group chat:** Your users can request to join or be invited to group chats.
* **Message threads:** Keep conversations organized with message threads.
* **Push notifications:** Make sure your users always see their messages.
* **File attachments:** Attach images, videos, or any other type of files.
* **Typing indicators:** Let your users know when others are typing.
* **Reactions:** Users can react to messages with emojis and GIFs.
* **Presence indicators:** Let your users know who's online.
* **Delivery and read receipts:** See when messages get delivered and read.
* **Link preview generation:** Messages with links get rich media previews.

![This example was created with ChatKitty.](<.gitbook/assets/screenshot-chatkitty-demo-app (1).png>)

With ChatKitty you can build real-time chat with or without a back-end. Getting started with ChatKitty is easy and you get:

**Reliability**

Your user chat sessions remain stable even in the presence of proxies, load balancers and personal firewalls. ChatKitty provides auto reconnection support and offline notifications so your users stay in the loop.

**Low Latency**

With response times below 150ms, ChatKitty makes sure your users have a smooth and immersive chat experience.

**Cross-platform support**

You can use ChatKitty across every major browser and device platform. ChatKitty also works great with multi-platform frameworks like React-Native and Ionic.

ChatKitty provides a client JavaScript SDK and Platform API for you to interact with your ChatKitty application.

[The JavaScript client library](getting-started/javascript-real-time-messaging-quick-start.md) provides an asynchronous real-time messaging interface to ChatKitty's user-side functionality. [The Platform API](https://swagger.chatkitty.com) provides a RESTful HTTP interface for you to manage and control your application server-side.

## Client SDK

ChatKitty provides a JavaScript client SDK to integrate chat into your front-end application.

We've provided the following resources to help you implement chat with the ChatKitty JavaScript SDK.

* [Getting Started with the ChatKitty JavaScript SDK](getting-started/javascript-real-time-messaging-quick-start.md)
* [SDK Reference Documentation](https://chatkitty.github.io/chatkitty-js/classes/chatkitty.chatkitty-1.html)
* [Guides And Tutorials](https://www.chatkitty.com/guides/)

## Platform API

The Platform API provides a RESTful interface for administrators and server-side back-ends to manage their ChatKitty applications.

We've provided the following resources to help you manage your application with the ChatKitty Platform API.

* [Platform API Overview](platform-api/overview.md)
* [Open API Reference Documentation And API Explorer](https://swagger.chatkitty.com)
