---
description: Learn how to authenticate requests to the ChatKitty Platform API.
---

# Authentication

Retrieve an **OAuth 2.0 Bearer access token** with the client credentials OAuth flow.

Retrieving a Bearer access token requires a ChatKitty application client ID and client secret.

You can create a ChatKitty application and receive client credentials using [the ChatKitty dashboard](https://dashboard.chatkitty.com/).

ChatKitty expects a valid Bearer access token to be included in all API requests like this:

`Authorization: Bearer {{access_token}}`

 You must replace `{{access_token}}` with an access token gotten using your client credentials.

 To authorize use this replacing `acme` with your client ID and `acmesecret` with your client secret:

```bash
curl --location --request POST 'https://authorization.chatkitty.com/oauth/token' \
--user 'acme:acmesecret' \
--form 'grant_type=client_credentials' \
--form 'username=acme' \
--form 'password=acmesecret'
```

Returns a JSON response with the Bearer access token.

```javascript
{
  "access_token": "d4327889-26a8-49ac-997c-56d8b1bcb09c",
  "token_type": "bearer",
  "expires_in": 42892,
  "scope": "org:1:app"
}
```

 `access_token` is the bearer access token. `expires_in` represents the access token's validity in seconds.

