---
description: >-
  The basics. Create a ChatKitty project, then install and configure the
  ChatKitty client.
---

# Setting up a ChatKitty project

## Creating a ChatKitty project

You'll need a **ChatKitty account** before you can begin building chat with ChatKitty.

You can [create a free ChatKitty account here](https://dashboard.chatkitty.com/authorization/register).

Once you've created a ChatKitty account, create an application for your project:

![Create an application for your project](<../.gitbook/assets/screenshot-chatkitty-create-application (1).png>)

## Installing the ChatKitty JS SDK

To use the ChatKitty JavaScript Chat SDK, you'll need to add the [ChatKitty JavaScript SDK](https://www.npmjs.com/package/chatkitty) NPM package to your JavaScript front-end project:

{% tabs %}
{% tab title="NPM" %}
```bash
npm install chatkitty
```
{% endtab %}

{% tab title="Yarn" %}
```bash
yarn add chatkitty
```
{% endtab %}
{% endtabs %}

Next, you'll need to configure the ChatKitty SDK with your ChatKitty API key. You can find your API key on the **ChatKitty dashboard**, in the "**Settings**" page.

![From the ChatKitty dashboard](<../.gitbook/assets/screenshot-chatkitty-side-menu-settings (1).png>)

Copy the string value under "**API Key**", you'll need it to initialize a ChatKitty client instance:

![Unique string used to identify your project](<../.gitbook/assets/screenshot-chatkitty-settings-api-key (1).png>)

## Initializing the ChatKitty SDK with your API key

With your API key from the ChatKitty dashboard, you can initialize a new instance of the **ChatKitty client:**

```javascript
import ChatKitty from 'chatkitty';

export const kitty = ChatKitty.getInstance('YOUR CHATKITTY API KEY HERE');
```

With that, you can now use the ChatKitty SDK to begin building real-time chat features into your application.
