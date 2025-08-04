# Naming conventions

## URIs

### Component Names

URIs follow [RFC 3986](https://tools.ietf.org/html/rfc3986) specification. This specification simplifies REST API service development and consumption. The guidelines in this section govern your URI structure and semantics following the [RFC 3986](https://tools.ietf.org/html/rfc3986) constraints.

### URI Components

According to [RFC 3986](https://tools.ietf.org/html/rfc3986), the generic URI syntax consists of a hierarchical sequence of components referred to as the scheme, authority, path, query, and fragment as shown in example below.

```text
   https://example.com:8042/over/there?name=ferret#nose
   \___/   \______________/\_________/ \_________/ \__/
     |             |            |           |       |
  scheme       authority       path       query  fragment
```

### URI Naming Conventions

Examples of the valid syntax for URI paths are as follows:

- `/api-group/version/resource-collection`
- `/api-group/version/resource-collection/resource-id`
- `/api-group/version/resource-collection/resource-id/resource-collection`
- `/api-group/version/resource-collection/resource-id/resource-collection[/resource-id...]`
- `/api-group/version/singleton`
- `/api-group/version/resource-collection/resource-id/singleton`

This general format can be summarised as:

`/api-group/version/[/singleton|/resource-collection[/resource-id[/singleton|/resource-collection[/resource-id...]]]]`

where `api-group` must be from a centrally governed [API groups registry](api-groups/api-grp-registry.md).

- Sample URI paths:
  - Resource collection: `/iam/v1/users`
  - Instance of a resource within a collection: `/iam/v1/users/123e4567-e89b-12d3-a456-426614174000`
  - Singleton resource: `/iam/v1/settings`
- An individual resource may exist directly beneath a resource collection:
  - `/users/<user_id>`
- A sub-resource collection may exist under individual resources of another resource collection:
  - `/users/<user_id>/assignments`
- A sub-resource collection may *not* exist under a singleton resource:
  - `/status/components` is not allowed.
- A sub-resource collection may *not* exist directly under another resource collection without specifying an instance of the top-level resource collection:
  - `/users/admins/<user-id>` is not allowed because admins is directly beneath users (no resource-id).
- The level of nesting in a URI should be limited to a maximum of 2 levels (unless it is absolutely necessary):
  - `/users/<user_id>/assignments/<assignment_id>/roles/<role_id>` has 3 levels of nesting and is generally not recommended

### Resource Names

When modeling a service as a set of resources, developers must follow these principles:

- Nouns must be used, not verbs.
- Names must be lower-case and use only alphanumeric characters and hyphens.
- Resource collections' names must be plural.
- Use the resource ID to identify an instance of a resource in a resource collection.

### Query Parameter Names

Query parameter names must conform to the following standards

- Query parameters must be expressed in kebab-case (i.e. dry-run)
- Query parameters should not be excessively long
- Parameter characters must be limited to the following sets: a-z, 0-9, and - (dash)

There are a set of [standard query parameters](query_parameters.md#standard-query-parameters)
that a service *may* support. If a service supports any one of the standard query
parameters then the implementation for that query parameter must match the stated intent.

## Resources

### Property Naming

Property names must conform to the following standards

- Property names must be expressed as lower camel case, with acronyms and abbreviations treated as words (i.e. resourceUri, not resourceURI)
- Property names should not be excessively long
- Property characters must be limited to the following set: A-Z,a-z,0-9

There are certain properties that may express measurements and without referencing
the schema it may be impossible for the user to know the measure. As a convenience
to API users, the unit of measure (or an abbreviated form) should be present
in the property name as a suffix. Several examples follow:

- requestsPerSecond
- responsesBytesPerMin
- averageTransferSpeedInMbps
- durationInMins

There are certain properties that may contain specially encoded values. These properties
should include a suffix to indicate the type. The following suffixes apply:

| Suffix | Type                                                       | Example    |
|--------|------------------------------------------------------------|------------|
| At     | The property value contains a RFC 3339 timestamp in UTC+0. | createdAt  |
| Uri    | The property value contains a URI.                         | invoiceUri |

Where a property is an object and this object is used to represent a dynamic data value that is not part of the documented resource naming structure, the property names of the object are treated as data and are exempt from the above naming convention. As an example, the `tags` property below is an object that represents arbitrary key-value pairs. The `tags` keys do not have to comply with the resource property naming convention.

### Common Properties

The following table describes standard resource properties. Required/mandated = Yes means the property must be defined for the resource. Required/mandated = No means the property may be defined for the resource. If used, the properties must have the defined property name, type and behavior.

| Property     |JSON Type  | Read/Write | Description                                              | Required/Mandated  |
|--------------|-----------|------------|----------------------------------------------------------|--------------------|
| id           | string    | Read only  | Primary identifier for the resource given by the system. | Yes                |
| type         | string    | Read only  | The type of the resource.                                | Yes                |
| generation   | number    | Read only  | Monotonically increasing update counter.                 | Yes unless exempt. |
| createdAt    | string    | Read only  | Time of resource creation                                | Yes unless exempt. |
| updatedAt    | string    | Read only  | Time of the last resource update                         | Yes unless exempt. |
| resourceUri  | string    | Read only  | URI to the resource itself (i.e. a self link).           | No                 |
| name         | string    | Read/Write | Name for the resource given by the user.                 | No                 |
| tags         | object    | Read/Write | User configured or system assigned tags of the resource. | No                 |

The `id` property is the primary identifier of the resource:

- The value in the `id` property MUST match the id in the URI used for operating on the resource and must be unique within a resource collection.
- The use of a UUID conforming to RFC 4122 is strongly preferred and SHOULD be used if at all possible.
Otherwise, other formats may be used, but all ids for a given resource type must
be in a common format.
- Resource IDs SHOULD try to use either Resource Identifier Characters or ASCII characters. There SHOULD NOT be any ID using UTF-8 characters.
- APIs MUST NOT use the database sequence number as the resource identifier.
- APIs MUST NOT use the following reserved words as identifiers: `bulk`, `batch`. See the path formats in [Bulk and Batch Operations](./bulk-batch.md).

The `type` property identifies the type of the resource. This value *must* be unique across the API group and prefixed by API group name.
All resources in a collection must present the same type value. Example: "storage/storage-array" or "networking/switch".

The `generation` property of a resource is a monotonically increasing integer that represents a
revision of the resource with respect to a history of updates. The generation is incremented
whenever the resource is modified and so provides an order to the updates. This is
useful as a means for a client to determine if a resource has changed or which of two copies
of a resource is more up to date. The generation also provides a strong validator for [conditional requests](./http.md#conditional-requests). Resources that are fully dynamic with the data generated during GET requests, not backed by persistent storage,
are exempt from having `generation` property.

`createdAt` and `updatedAt` are timestamps that designate the time when the resource was created/updated. Resources that are
fully dynamic with the data generated during GET requests, not backed by persistent storage, are exempt from having `createdAt`
and `updatedAt` properties.

The `resourceUri` property identifies the resource's URI. The resourceUri value is a URI encoded
string starting with the version of the API group (e.g. "/storage/v1/storage-systems/123e4567-e89b-12d3-a456-426614173999").

The `name` property is a user defined name for the resource.

The `tags` property is an object of tag names and associated values. The tag names are represented by the
keys of the object and their values by a string value. The following example has a single tag named `location`
with the value `San Jose`:

```json
"tags": {
  "location": "San Jose"
}
```

Resources that support tags but have none defined must have a tags property with an empty object as its value: `tags: {}`.
Refer to the [Tagging Standard](../../ratified/tagging/tagging_standard.md) for the details on the tags structure and their constraints.

### Property Values

The following table describes the standards that certain property values must conform to.

| Property Value | Standard                                                                                   | Example                                               |
| -------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------- |
| Enumeration    | All upper case letters with words separated by underscores                                 | `RUNNING_AWAY`                                        |
| Timestamp      | [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) (and ISO 8601) in UTC+0 timezone | `2019-10-12T07:20:50.52Z`                             |
| URI            | Relative to the API group root                                                             | `/api/v1/houses/123e4567-e89b-12d3-a456-426614173999` |
| UUID           | [RFC 4122](https://datatracker.ietf.org/doc/html/rfc4122) (see *Note*)                     | `123e4567-e89b-12d3-a456-426614174000`                |

**Note**: Identifiers that are UUIDs in a non-RFC 4122 representation can be used, but they must not be defined or described as
UUIDs. Any property that is defined to be a UUID must be in the RFC 4122 format.

For representing a range of values in the response, half-open intervals must be used with the naming convention
**[start\_xxx, end\_xxx)** (inclusive start, exclusive end).

- e.g. To represent numbers ranging from 1 to 9, use the following convention: [start\_num, end\_num) where start\_num = 1 and end\_num = 10.

Values that represent a time duration should use a standard unit of measure (e.g. seconds, minutes) and adhere to the suffix
naming described in a previous section.

- e.g. "durationInMins" = 120
