---
description: >-
  A user session represents a bi-directional connection between your client
  application and ChatKitty.
---

# User Sessions

## Starting a user session

Before you can use the ChatKitty SDK, a user session must be **started**. Starting a user session creates a **concurrent connection** to ChatKitty belonging to a **user** throughout the duration of the user's usage of your application until the user session is **ended**. A user session is **active** from when it was started until it is ended by the user, or the session's underlying network connection is lost. Only one user session can be active at a time for a ChatKitty client instance.

ChatKitty lets you integrate your application authentication logic to secure user sessions with no hassle. If you are in development mode or don't plan on implementing user authentication, you can also create user sessions without authenticating in **guest mode**. 

You start a user session using the unique **username** of a user and optionally authentication parameters to secure the user session.

{% hint style="info" %}
We recommend you use hashed email addresses or phone numbers for usernames.
{% endhint %}

### Starting a guest user session

By default, your application has the **guest user** feature enabled, allowing you to start a user session with only your users' unique username.

#### Parameters

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| username | string | The unique name of the user starting this guest session. | ✔ |

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

{% hint style="warning" %}
Guest user sessions are **only** appropriate when your application in development, if your application supports anonymous chat, or if you don't need back-end authentication.
{% endhint %}

### Starting an authenticated user session

You can start an authenticated user session by passing a unique **username** and **auth params** to your ChatKitty client start session method. A username is a string that uniquely identifies a user within your application. You define a username when **creating a user** as its name property. Auth params are arbitrary properties passed to an **authentication chat function** used to check if a user is allowed to start a user session with their credentials.

#### Parameters

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| username | string | The unique name of the user starting this authenticated session. | ✔ |
| authParams | object | Application-defined parameters containing user credentials | ✔ |

```javascript
const result = await kitty.startSession({
  username: 'jane.doe@chatkitty.com',
  authParams: {
    // parameters you defined to securely pass to authentication chat function
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

The auth params you pass to ChatKitty are entirely under your control. Typically, these will be relevant authentication credentials your authentication chat function uses to determine if to let a user start a session like a password, access token, session cookie, etc.

## Ending a user session

End a user session to close its concurrent connection and free up resources used by your ChatKitty client.

```javascript
kitty.endSession();
```

