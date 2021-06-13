---
description: Traverse through ChatKitty collections efficiently with pagination.
---

# Pagination

ChatKitty **paginates** all collection resources. Requesting a resource collection returns the first page of the collection optionally with HAL hypermedia links to subsequent pages if more pages are available.

Traverse the page links to iterate through a collection.

```javascript
{
  "_embedded": {
    "users": [
      {
        "id": 1,
        "name": "jane@chatkitty.com",
        "displayName": "Jane Doe",
        "_links": {
          "self": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/1"
          },
          "channels": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/1/channels"
          },
          "application": {
            "href": "https://api.chatkitty.com/v1/applications/1"
          }
        }
      },
      {
        "id": 2,
        "name": "john@chatkitty.com",
        "displayName": "John Doe",
        "_links": {
          "self": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/2"
          },
          "channels": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/2/channels"
          },
          "application": {
            "href": "https://api.chatkitty.com/v1/applications/1"
          }
        }
      }, // Other items in this slice ...
    ]
  },
  "_links": {
    "first": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=0&size=25"
    },
    "prev": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=0&size=25"
    },
    "self": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=1&size=25"
    },
    "next": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=2&size=25"
    },
    "last": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=2&size=25"
    }
  },
  "page": {
    "size": 25,
    "totalElements": 50,
    "totalPages": 2,
    "number": 0
  }
}
```

## Properties

### Embedded properties <a id="pagination-embedded-properties"></a>

 A page resource embeds a slice of a resource collection in a JSON array property with the same name as the resource collection. For example, with a collection of users resources, the resource collection name would be `users`, and the page would include a JSON array in its `_embedded` property named `users`, as per the [HAL](http://stateless.co/hal_specification.html) specification.

```javascript
{
  "_embedded": {
    "users": [
      {
        "id": 1,
        "name": "jane@chatkitty.com",
        "displayName": "Jane Doe",
        "_links": {
          "self": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/1"
          },
          "channels": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/1/channels"
          },
          "application": {
            "href": "https://api.chatkitty.com/v1/applications/1"
          }
        }
      },
      {
        "id": 2,
        "name": "john@chatkitty.com",
        "displayName": "John Doe",
        "_links": {
          "self": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/2"
          },
          "channels": {
            "href": "https://api.chatkitty.com/v1/applications/1/users/2/channels"
          },
          "application": {
            "href": "https://api.chatkitty.com/v1/applications/1"
          }
        }
      }, // Other items in this slice ...
    ]
  }, // Page links and metadata
}
```

### HAL links

| Link | Methods | Description |
| :--- | :--- | :--- |
| self | GET | Self link to this page. |
| first | GET | **Optional:** Link to the first page of this collection. **Present if** known. |
| prev | GET | **Optional:** Link to the previous page of this collection. **Present if** there are more items before the first item in this page. |
| next | GET | **Optional:** Link to the next page of this collection. **Present if** there are more items after the last item in this page. |
| last | GET | **Optional:** Link to the last page of this collection. **Present if** known. |

```javascript
{
  // ... Embedded properties,
  "_links": {
    "first": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=0&size=25"
    },
    "prev": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=0&size=25"
    },
    "self": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=1&size=25"
    },
    "next": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=2&size=25"
    },
    "last": {
      "href": "https://api.chatkitty.com/v1/applications/1/users?page=2&size=25"
    }
  }, // Page metadata ...
}
```

### Page Metadata <a id="pagination-page-metadata"></a>

Metadata about a page containing its size, total number of elements in the collection, the total number of pages in the collection, and the page number.

| Name | Type | Description |
| :--- | :--- | :--- |
| size | Int | The number of elements in this page. |
| number | Int | **Optional:** The zero-based index of this page. **Present if** known. |
| totalElements | Long | **Optional:** The total number of elements in the collection. **Present if** known. |
| totalPages | Int | **Optional:** The total number of pages in the collection. **Present if** known. |

```javascript
{
  // ... Embedded properties and page links,
  "page": {
    "size": 25,
    "totalElements": 50,
    "totalPages": 2,
    "number": 0
  }
}
```

