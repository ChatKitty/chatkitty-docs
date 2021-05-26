---
description: Users in ChatKitty are your end-users.
---

# Users

A user represents an end-user using your application. Users can join channels, chat with other users, receive notifications and perform other actions. Users are identified by a unique **name**.

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| id | number | 64-bit integer identifier associated with this user. | ✔ |
| name | string | The unique name of the user. | ✔ |
| displayName | string | Human readable name of this user. Shown to other users. | ✔ |
| displayPictureUrl | string | URI for this user's display picture | ✔ |
|  |  |  |  |

## Current User

 After starting a ChatKitty [user session](user-sessions.md), you can request the current user anytime.

```javascript
const result = await kitty.getCurrentUser();

const user = result.user; // Handle user
```

### Observing the current user <a id="current-user-observing-the-current-user"></a>

Get updates when the current user changes by registering an observer function.

```javascript
const unsubscribe = kitty.onCurrentUserChanged((user) => {
  // handle new current user or current user changes
});

// call unsubscribe function when you're no longer interested in updates
unsubscribe();
```

{% hint style="info" %}
The observer function passed to onCurrentUserChanged is called with the current user value when first registered.
{% endhint %}

### Updating the current user

Update the current user by passing a function taking the current user and returning a user with the changes to be applied to your ChatKitty client instance.

```javascript
await kitty.updateCurrentUser((user) => {
  // Perform updates
  user.properties = {
    ...user.properties,
    'new-property': newPropertyValue,
  };

  return user; // Return updated user
});
```

### See also

* [JavaScript SDK Reference Documentation](https://chatkitty.github.io/chatkitty-js/modules/current_user.html)

## See also

* [JavaScript SDK Reference Documentation](https://chatkitty.github.io/chatkitty-js/modules/user.html)
* [Platform API OpenAPI Specification](https://swagger.chatkitty.com/#/user)

