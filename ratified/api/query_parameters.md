# Query Parameters

Query parameters are name/value pairs specified after the resource path,
as prescribed in [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986).

## Standard Query Parameters

A number of standard query parameters are defined that support common API features,
such as sorting and filtering. If a request supports any of these features,
it *must* use the specified query parameters for those features.

Non-standard query parameters are permitted, so long as they do not share intent
with the standard ones.

All query parameters *must* follow the [Query Parameter Names](naming.md#query) convention.

| Query parameter | Intent                                  |
|-----------------|-----------------------------------------|
| sort            | [Sorting](#sorting)                     |
| select          | [Select](#select)                       |
| filter          | [Filtering](#filtering)                 |
| filter-tags     | [Filtering by tags](#filtering-by-tags) |
| force           | [Force](#force-delete)                  |
| id              | [Bulk operations](#bulk-operations)     |
| dry-run         | [Dry run](#dry-run)                     |
| limit           | See [Pagination](pagination.md)         |
| next            | See [Pagination](pagination.md)         |
| offset          | See [Pagination](pagination.md)         |

### Sorting

The `sort` query parameter is used to instruct the server to sort the resources
returned by a collection-level GET. They provide sorting by one or more properties
in ascending or descending order.

The value of the `sort` query parameter is a comma separated list of sort expressions.
Each sort expression is a property name optionally followed by a direction indicator
`asc` (ascending) or `desc` (descending).

The first sort expression in the list defines the primary sort order, the second
defines the secondary sort order, and so on. If a direction indicator is omitted
the default direction is ascending.

Examples:

```text
GET /storage/v1/storage-systems?sort=name asc

GET /storage/v1/storage-systems?sort=name,createdAt desc
```

Not all properties are required to support sorting. The API documentation must
specify which properties can be used for sorting and how many properties can be
used in combination.

### Select

The `select` query parameter is used to limit the properties returned for a
resource. This parameter is applicable for both read operations (GET) and write
operations (POST, PUT, PATCH).

The value of the `select` query parameter is a comma separated list of properties.
The server must only return the set of properties requested by the client.
All properties must be returned if the select parameter is omitted.

Examples for read operations:

```text
GET /storage/v1/storage-systems?select=id,name

GET /storage/v1/storage-systems/123e4567-e89b-12d3-a456-426614174000?select=id,name

GET /storage/v1/storage-systems/123e4567-e89b-12d3-a456-426614174000?select=id,name,interfaces/name
```

Examples for write operations:

```text
POST /storage/v1/storage-systems?select=id,name,createdAt

PUT /storage/v1/storage-systems/123e4567-e89b-12d3-a456-426614174000?select=id,updatedAt

PATCH /storage/v1/storage-systems/123e4567-e89b-12d3-a456-426614174000?select=name,status,updatedAt
```

If there are properties that must always be returned, the API documentation must
specify them.

### Filtering

The `filter` query parameter is used to filter the set of resources returned in
a collection-level GET. The returned set of resources must match the criteria in
the filter query parameter.

The value of the `filter` query parameter is a subset of
[OData 4.0](https://www.odata.org/documentation/)
filter expressions consisting of simple comparison operations joined by
logical operators.

Comparison arguments can be property names or literals.
Properties that are nested are referenced as a path delimited by `/`. For
example, the following resource data model contains the properties`name` and
`house/number`:

```json
{
  "name": "mary",
  "house": {
    "number": 1025,
    "street": "1st Avenue"
  }
}
```

The supported literals are the following

| type       | example                           | description                                                    |
|------------|-----------------------------------|----------------------------------------------------------------|
| integer    | **1234567**                       | An integer in base 10 (-1234567 for negative integer).         |
| decimal    | **1234.567**, **-1234.567**       | A decimal in base 10 (-1234.567 for negative decimals).        |
| timestamp  | **2019-10-12T07:20:50.52934852Z** | A timestamp in RFC 3339 date-time format.                      |
| string     | **'this is a string'**            | A string value. Strings must begin and end with single quotes. |
| boolean    | **true**, **false**               | The boolean false/true value.                                  |
| null value | **null**                          | A null (or nil) value.                                         |

A comparison term compares a property to a literal to produce a logical value (true or false).
The comparisons supported are the following:

| comparison                   | example                              | description                                                                                                                                                                                                                                                    |
|------------------------------|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| equality                     | name **eq** 'john'                   | true if the value of the property is the same as the literal. The property is on the left of the operator. Valid for number, boolean, string or timestamp properties.                                                                                          |
| inequality                   | name **ne** 'john'                   | true if the value of the property is not the same as the literal. The property is on the left of the operator. Valid for number, boolean, string or timestamp properties.                                                                                      |
| greater than                 | count **gt** 5                       | true if the value of the property is greater than the literal. The property is on the left of the operator. Valid for number or timestamp properties.                                                                                                          |
| greater than or equal to     | count **ge** 5                       | true if the value of the property is greater than of equal to the literal. The property is on the left of the operator. Valid for number or timestamp properties.                                                                                              |
| less than                    | count **lt** 20                      | true if the value of the property is less than the literal. The property is on the left of the operator. Valid for number or timestamp properties.                                                                                                             |
| less than or equal to        | count **le** 20                      | true if the value of the property is less than or equal to the literal. The property is on the left of the operator. Valid for number or timestamp properties.                                                                                                 |
| literal in array property    | 'blue' **in** colors                 | true if the literal is contained in the property where the property is an array. The literal is on the left of the operator, the right hand operand of the **in** operator is always a collection. Valid for an array of strings.                              |
| property in list of literals | color **in** ('red','yellow','blue') | true if the value of the property is contained in the list of literals. The property is on the left of the operator, the right hand operand of the **in** operator is always a collection. Valid for string properties. The list of strings is in parentheses. |

The null value is equivalent to itself in a comparison, but it is not greater than or less than any value.
For example, the following comparisons result in the respective truth values:

```text
null eq null => true
null ne 5 => true
null gt 5 => false
```

String functions defined in [OData 4.0](https://www.odata.org/documentation/)
*may* also be supported as comparison terms. Any string functions *must* be documented.

Logical expressions operate on comparison terms.
The operators supported in logical expressions are the following:

| expression  | example                                                     | description                                                                                  |
|-------------|-------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| conjunction | count eq 5 **and** name eq 'fred'                           | true if both sides of the operator are true.                                                 |
| disjunction | count eq 5 **or** name eq 'fred'                            | true if either side of the operator is true.                                                 |
| complement  | **not** color in ('RED', 'GREEN', 'BLUE')                   | true if the right hand side of the operator is false.                                        |
| grouping    | **(** count eq 5 or name eq 'fred' **)** and color eq 'RED' | expressions in parentheses are evaluated first with the result used in the outer expression. |

Properties and literals, including booleans, are not supported as operands in logical expressions because they can evaluate
to `null` and logical expressions only support two-value logic (i.e. true and false). Instead, properties
and literals should always be used within comparison terms.

The order of precedence among the logical operators is first `not`, then `and`, then `or`.
The evaluation order is modified when grouping using parentheses.
The follow expressions are equivalent:

```text
not count eq 5 and name eq 'fred' or color eq 'RED'
((not count eq 5) and name eq 'fred') or (color eq 'RED')
```

Examples:

```text
GET /storage/v1/storage-systems?filter=prop1 eq 'foo'

GET /storage/v1/storage-systems?filter=prop1 eq 'foo' and prop3 gt 50

GET /storage/v1/storage-systems?filter=createdAt lt 2021-05-12T07:20:00.00Z

GET /storage/v1/storage-systems?filter=color in ('blue', 'yellow', 'green')

GET /storage/v1/storage-systems?filter='blue' in colors

```

Note, these examples are all in plain text. HTTP clients will typically URL encode
query parameters to convert non-alphanumeric characters to % escaped character codes
or + for spaces. URL encoded filter expressions should be decoded to interpret as above.

### Filtering by tags

The `filter-tags` query parameter is used to filter the set of resources returned in
a collection-level GET, based on the assigned tags and/or their values. The returned set of resources must match the criteria in
the `filter-tags` query parameter.

The value of the `filter-tags` query parameter consists of simple comparison operations joined by
logical operators.

Comparison arguments can be a tag key name or a tag value represented as strings. Strings must begin and end with single quotes (**'this is a string'**).

A comparison term compares comparison arguments to produce a logical value (true or false).
The comparisons supported are the following:

| comparison         | example             | description                                                                                                                                                           |
|--------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tag value equality | 'OS' **eq** 'Linux' | true if the case-insensitive tag key has a specific case-insensitive value. The tag key is on the left of the operator. The tag value is on the right of the operator |
| tag key existence  | 'OS' **in keys**    | true if the given case-insensitive key is part of the resource tags, regardless of the value. The tag key is on the left of the operator.                             |

Logical expressions operate on comparison terms.
The operators supported in logical expressions are the following:

| expression  | example                                                                           | description                                                                                  |
|-------------|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| conjunction | 'OS' eq 'Linux' **and** 'Machine Type' in keys                                    | true if both sides of the operator are true.                                                 |
| disjunction | 'OS' eq 'Linux' **or** 'Machine Type' in keys                                     | true if either side of the operator is true.                                                 |
| complement  | **not** 'Machine Type' in keys                                                    | true if the right hand side of the operator is false.                                        |
| grouping    | **(** 'OS' eq 'Linux' or 'Machine Type' in keys **)** and 'Machine Type' eq 'x86' | expressions in parentheses are evaluated first with the result used in the outer expression. |

The order of precedence among the logical operators is first `not`, then `and`, then `or`.
The evaluation order is modified when grouping using parentheses.
The follow expressions are equivalent:

```text
not 'OS' eq 'Linux' and 'Machine Type' in keys or 'os' eq 'Windows'
((not 'OS' eq 'Linux) and 'Machine Type' in 'keys') or ('os' eq 'Windows')
```

Examples:

```text
GET /storage/v1/storage-systems?filter-tags='OS' eq 'Linux'

GET /storage/v1/storage-systems?filter-tags='OS' eq 'Linux' and 'Machine Type' in keys

```

Note, these examples are all in plain text. HTTP clients will typically URL encode
query parameters to convert non-alphanumeric characters to % escaped character codes
or + for spaces. URL encoded filter expressions should be decoded to interpret as above.

### Force Delete

The `force` query parameter is used to force deletion in situations where the
server would not normally allow a deletion to be processed (due to business
logic reasons or because a deletion is requested to the entire resource collection).
It is optional for a delete operation to support force delete.

The value of the `force` query parameter is `true` or `false` and defaults to
`false` if the query parameter is omitted.

Example:

```text
DELETE /storage/v1/storage-systems/123e4567-e89b-12d3-a456-426614174000?force=true
```

### Bulk operations

> ⚠️ **Deprecated**: Using query parameters for bulk operations is now deprecated. Please use request body for bulk operations instead.

Using the `id` query parameter is supported for bulk operations with the following HTTP verbs: `DELETE`, `PATCH`.
For more information see the [Bulk Operations](./bulk-batch.md#query-parameter) with query parameters guidelines.

### Dry run

The `dry-run` query parameter is used to instruct the server to perform the resource update operation (`POST`, `PUT`, `PATCH`, `DELETE`)
and return a response as if the operation had completed, but without actually creating, updating, or deleting the resource.
This allows the user to test if the request would succeed before making the change.

The value of the `dry-run` query parameter is `true` or `false` and defaults to `false` if the query parameter is omitted.

Examples:

```text
POST /compute-ops-mgmt/v1beta1/groups/93abd6e0-d7b5-471d-9cb9-390d0abe497b/devices?dry-run=true
```

```text
DELETE /storage/v1/storage-systems?id=123e4567-e89b-12d3-a456-426614174000&id=123e4567-e89b-12d3-a456-426614174001&dry-run=true
```

Provide feedback at <https://github.com/glcp/doc-portal>
