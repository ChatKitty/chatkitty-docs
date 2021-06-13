---
description: Manage your ChatKitty application using the Platform API.
---

# Overview

The Platform API provides a RESTful Application Programming Interface for administrators and server-side back-ends to configure and manage their ChatKitty applications. This API lets you create [users](../concepts/users.md) and [channels](../concepts/channels.md), send [system messages](../concepts/messages.md) and even delete your application. Everything you see on your ChatKitty dashboard can be controlled with the Platform API.

There are a couple of things you need to start using the Platform API:

1. Your application's OAuth 2.0 client credentials from your dashboard to authenticate your requests
2. An HTTP client of your choice to make API calls

## Headers

In addition to the `Authorization` header for **authentication**, you should also include the following headers in all your requests:

* `Accept: application/hal+json`
* `Content-Type: application/json`

## Errors <a id="errors"></a>

The Platform API may return any of these HTTP error codes:

| Error Code | Meaning |
| :--- | :--- |
| 400 | Bad Request -- Your request is invalid. |
| 401 | Unauthorized -- Your API key is wrong. |
| 403 | Forbidden -- The resource requested is hidden for administrators only. |
| 404 | Not Found -- The specified resource could not be found. |
| 405 | Method Not Allowed -- You tried to access a resource with an invalid method. |
| 406 | Not Acceptable -- You requested a format that isn't json. |
| 410 | Gone -- The resource requested has been removed from our servers. |
| 429 | Too Many Requests -- You're making too many requests. |
| 500 | Internal Server Error -- We had a problem with our server. Try again later. |
| 503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later. |

