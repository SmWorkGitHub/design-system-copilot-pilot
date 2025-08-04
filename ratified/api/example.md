# Examples

This section provides high-level examples of how to apply the API standards effectively. These examples are meant to serve as guidelines for various elements of API design.

## Tags

Refer to the [Tagging Standard](../../ratified/tagging/tagging_standard.md) for the details on the tags structure and their constraints.

### Tag Case Sensitivity and Operations

Operations on tags, such as searching, updating, and deleting, should treat tag keys as case-insensitive. This means that when searching for or updating tags, keys such as "Location" and "location" should be considered equivalent. Values, however, will remain case-sensitive and stored as entered.

When performing update operations using different methods, such as `PUT`, `PATCH`, or `JSON Merge Patch`, case-insensitive handling must be respected to avoid duplicate tags with the same meaning.

According to the [API standards](../../ratified/api/http.md#http-methods), if an API supports PATCH, it must follow [RFC 7396 (JSON Merge Patch)](https://datatracker.ietf.org/doc/html/rfc7396) and be idempotent. Developers can also support [RFC 6902 (JSON Patch)](https://datatracker.ietf.org/doc/html/rfc6902) in addition to JSON Merge Patch.

Below are examples for clarity:

#### JSON Merge Patch Example

Assume the resource initially has the following tags:

```json
{
  "tags": { "location": "San Jose" }
}
```

A user tries to update the tag using PATCH:

```json
{
  "tags": { "Location": "Santa Clara" }
}
```

The result will be the updated version:

```json
{
  "tags": { "Location": "Santa Clara" }
}
```

In this example, the tag key was matched while ignoring the case (location ~= Location), and both the key and value were updated to reflect the case provided in the PATCH request.

However, if the user tries to update the same resource with:

```json
{
  "tags": { "location": "San Jose", "Location": "Santa Clara" }
}
```

The result will return an error, indicating that the same resource cannot have tags with equivalent keys differing only in case.

#### JSON Patch Example

Assume the resource initially has the following tags:

```json
{
  "tags": { "location": "San Jose" }
}
```

A user attempts to add a new tag with the following JSON Patch request:

```json
[
    {
        "op": "add",
        "path": "/tags/Location",
        "value": "san jose"
    }
]
```

This operation will return an error.

```json
{
  "tags": { "location": "San Jose", "Location": "san jose" }
}
```

Both location and Location are provided as tag keys which is not allowed.

Instead, the user should remove the original tag before adding the new one:

```json
[
    {
        "op": "remove",
        "path": "/tags/location"
    },
    {
        "op": "add",
        "path": "/tags/Location",
        "value": "san jose"
    }
]
```

The result will be:

```json
{
  "tags": { "Location": "san jose" }
}
```

Tag key is Location instead of location. This will match and result in the tag being updated, including the new case for the value.
