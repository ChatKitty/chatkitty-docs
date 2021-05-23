# User Sessions

ChatKitty lets you integrate your application authentication logic to secure user sessions with no hassle. If you are in development mode or don't plan on implementing user authentication, you can also create user sessions without authenticating in **guest mode**.

 You can start a user session using the unique **username** of a user and optional authentication parameters to secure the user session.

## Authenticating a user session

You can start an authenticated user session by passing a unique **username** and optionally **auth params** to your `ChatKitty` client `startSession()` method. A username is a string that uniquely identifies a user within your application. You define a username when **creating a user** as its `name` property.

The auth params you pass to ChatKitty are entirely under your control. Typically, these will be relevant authentication credentials your **authentication chat function** uses to determine if to let a user start a session like a password, access token, session cookie, etc.

