---
description: >-
  A user session represents a bi-directional connection between your client
  application and ChatKitty.
---

# User Sessions

## Starting a user session

Before you can use the ChatKitty SDK, a user session must be **started**. Starting a user session creates an open connection to ChatKitty belonging to a **user** throughout the duration of the user's usage of your application until the user session is **ended**. A user session is **active** from when it was started until it is ended by the user, or the session's underlying network connection is lost. Only one user session can be active at a time for a ChatKitty client instance.

ChatKitty lets you integrate your application authentication logic to secure user sessions with no hassle. If you are in development mode or don't plan on implementing user authentication, you can also create user sessions without authenticating in **guest mode**. 

You start a user session using the unique **username** of a user and optionally authentication parameters to secure the user session.

{% tabs %}
{% tab title="Guest session" %}
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
{% endtab %}

{% tab title="Authenticated session" %}
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
{% endtab %}
{% endtabs %}

{% hint style="info" %}
We recommend you use hashed email addresses or phone numbers for usernames.
{% endhint %}

### Starting a guest user session

By default, your application has the **guest user** feature enabled, allowing you to start a user session with only your users' unique username.

### Starting an authenticated user session

You can start an authenticated user session by passing a unique **username** and **auth params** to your ChatKitty client start session method. A username is a string that uniquely identifies a user within your application. You define a username when **creating a user** as its name property.

The auth params you pass to ChatKitty are entirely under your control. Typically, these will be relevant authentication credentials your **authentication chat function** uses to determine if to let a user start a session like a password, access token, session cookie, etc.

