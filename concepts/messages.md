---
description: 'Users, bots, and system administrators can send messages'
---

# Messages

Messages are the core building blocks of ChatKitty applications. Users send messages using the client SDK, while bots and system administrators can send messages using the Platform API. 

## Properties

| Name | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| id | number | 64-bit integer identifier associated with this message | ✔ |
| type | string | The type of this message. `TEXT`, or `FILE` | ✔ |
| body | string | The text body of this message. Present if this is a text message | - |
| file | ChatKittyFile | The file attached to this message. Present if this is a file message | - |
| links | MessageLink \[ \] | **Message links** found in this message. Present if this is a text message  | - |
| user | User | The user who sent this message. Absent if this is a system message | - |
| createdTime | datetime | ISO 8601 datetime when this message was created | ✔ |
| properties | object | Custom data associated with this message | ✔ |

