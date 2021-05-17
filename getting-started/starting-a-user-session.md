# Starting a user session

## 

Before a user can begin chatting with other users using ChatKitty, you must create a **user session**. A user session represents a secure bi-directional connection with ChatKitty servers allowing your users to send and receiving messages in real-time. 

ChatKitty lets you integrate your application authentication logic to secure user sessions with no hassle. If you are in development mode or don't plan on implementing user authentication, you can also create user sessions without authenticating in **guest mode**.

 You can start a user session using the unique **username** of a user and optional authentication parameters to secure the user session.

## Authenticating a user session

You can start an authenticated user session by passing a unique **username** and **auth params** to your `ChatKitty` client `startSession()` method. A username is a string that uniquely identifies a user within your application. You define a username when **creating a user** by its `name` property.

{% hint style="info" %}
We recommend you use uniquely hashed email addresses or phone numbers as usernames.
{% endhint %}

```javascript
const result = await kitty.startSession({
  username: email,
  authParams: { // parameters to pass to authentication chat function
    password: password, 
  },
});

if (result.succeeded) {
  const session = result.session; // Handle session
}

if (result.failed) {
  const error = result.error; // Handle error
}
```



