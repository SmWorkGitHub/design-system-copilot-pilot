---
seo:
  title: HPE GreenLake Developer Standard for GraphQL API Design | HPE GreenLake Platform
toc:
  enable: true
---

# 1. GraphQL API Design Standard

This HPE GreenLake GraphQL API Design standard describes how GraphQL interfaces are to be designed and implemented. The goal is to provide consistent, uniform guidelines for GraphQL APIs spanning multiple HPE services created and owned by different development groups.

See the HPE GreenLake development policy for [GraphQL API Design](../../policies/graphql-policy.md) to determine if your API project must conform to this specification.

This HPE GraphQL standard covers the following aspects of the GraphQL API:

- Schema syntax validation
- Schema versioning
- Schema directives for parameter constraints, etc.
- Schema constructs such as fragments, unions, and interfaces
- Pagination
- Sorting
- Filtering and searching
- Query depth limits
- Request rate-throttling and performance
- Security
- Error handling
- Composite supergraph formation

## 2 Schema

### 2.1 Specification

The official [GraphQL specification](https://spec.graphql.org/October2021/) is language-agnostic, enabling interaction with an API regardless of its programming language. Similarly, the HPE GraphQL standard is agnostic to both languages and tools, allowing any tool or implementation framework that adheres to GraphQL to follow this standard.

### 2.2 Schema Naming Convention

This section describes naming conventions for types, fields, and arguments in the GraphQL schema.

The schema defines the data types that can be queried and mutated. It's important to define a clear and well-structured schema that reflects the domain of your API.

APIs are defined interfaces that mediate interactions with resources. When a resource is accessible through multiple interfaces, such as REST and GraphQL, the properties of that resource should remain consistent across all interfaces.

**GraphQL Components**:

```graphql
"""
Represents a user entity.
"""
type User {
  id: ID!
  name: String!
  email: String!
  age: Int!
  createdAt: DateTime!
  updatedAt: DateTime!
}
"""
Input type for specifying sorting criteria.
"""
input UserSortInput {
  """
  The field to sort by.
  """
  fields: [UserSortFields]!
  """
  The order to sort by.
  """
  order: SortOrder!
}
"""
Enum representing the fields that can be sorted by.
"""
enum UserSortFields {
  NAME
  EMAIL
  AGE
  CREATED_AT
  UPDATED_AT
}
"""
Enum representing the order to sort by.
"""
enum UserSortOrder {
  ASC
  DESC
}
"""
Pagination input type.
"""
input UserPaginationInput {
  """
  Specifies the number of results to be returned
  """
  limit: Int!
  """
  Specifies the next page to be fetched. Used in cursor based pagination.
  """
  next: String!
  """
  Specifies the offset to be used for fetching results. Used in offset based pagination.
  """
  offset: Int!
}
"""
UserResponse with pagination information.
"""
type UserResponse {
  """
  The list of users.
  """
  items: [User]!
  """
  Number of returned items.
  """
  count: Int!
  """
  total number of items.
  """
  total: Int
  """
  next page to be fetched. required in cursor based pagination.
  """
  next: String!
  """
  offset to be used for fetching results. required in offset based pagination.
  """
  offset: Int!
}
"""
Type query for fetching users.
"""
type Query {
  """
  Fetches a list of users, sorted by the specified criteria and paginated by page criteria
  Arguments:
  sort: The sorting criteria.
  pagination: The pagination criteria.
  """
  users(pagination: UserPaginationInput, sort: UserSortInput): UserResponse
}
```

#### 2.2.1 Types

Type names must conform to the following standards:

- Use [PascalCase](https://en.wiktionary.org/wiki/Pascal_case) for the following type names.

- Type names should not be excessively long.

  - [*ObjectTypeDefinition*](https://spec.graphql.org/October2021/#ObjectTypeDefinition)

  - [*InputObjectTypeDefinition*](https://spec.graphql.org/October2021/#InputObjectTypeDefinition)

  - [*EnumTypeDefinition*](https://spec.graphql.org/October2021/#EnumTypeDefinition)

  - [*UnionTypeDefinition*](https://spec.graphql.org/October2021/#UnionTypeDefinition)

```graphql
# ObjectTypeDefinition
type MyType { ... }

# InputObjectTypeDefinition
input MyInput { ... }

# EnumTypeDefinition
enum MyEnum { ... }

# UnionTypeDefinition
union MyUnion = ...
```

- Use [SCREAMING\_SNAKE\_CASE](https://en.wiktionary.org/wiki/screaming_snake_case) for [*EnumTypeDefinition*](https://spec.graphql.org/October2021/#EnumTypeDefinition) values.

```graphql
enum MyEnum {
  VALUE_ONE
  VALUE_TWO
}
```

- Use the suffix `Input` when naming [*InputObjectTypeDefinition*](https://spec.graphql.org/October2021/#InputObjectTypeDefinition) types.

```graphql
input MyInput {
  number: Int!
  size: Int!
}
```

- Use the suffix `Response` when naming output types returned from queries or mutations.

```graphql
type query {
  myObjects(myArgumentName: String): MyResponse
}

type mutation {
  myMutation(input: MyInput!): MyResponse!
}
```

#### 2.2.2 Fields

Field names must conform to the following standards:

- Use [camelCase](https://en.wikipedia.org/wiki/Camel_case) for the field names.

- Field names should not be excessively long.

```graphql
input MyInput {
  myField: String!
}
```

#### 2.2.3 Operations

Operation names must conform to the following standards:

- Use [camelCase](https://en.wikipedia.org/wiki/Camel_case) for the operation names.

- Operation names should not be excessively long.

```graphql
type query {
  myCamelCaseOperationName(myArgumentName: String): String
}
```

- Use verb prefixes like `get` or `list` on the **query** operation.

```graphql
type query {
  # ❌ incorrect
  products: [Product]
  
  # ✅ correct
  getProducts: [Product]
}
```

- Add verb prefixes like `create`, `delete`, or `update` on the **mutation** operation.

```graphql
type mutation {
  # ❌ incorrect
  customerCreate(input: CreateCustomerInput): CreateCustomerResponse!

  # ✅ correct
  createCustomer(input: CreateCustomerInput): CreateCustomerResponse!
}
```

#### 2.2.4 Arguments

Argument names must conform to the following standards:

- Use [camelCase](https://en.wikipedia.org/wiki/Camel_case) for the argument names.

- Argument names should not be excessively long.

- Argument characters must be limited to the following set: A-Z,a-z,0-9.

```graphql
type query {
  myCamelCaseOperationName(myArgumentName: String): String
}
```

For more information, see the [Property Naming](naming.md#property-naming) section in the HPE GreenLake API standard.

#### 2.2.5 Directives

Directive names must conform to the following standards:

- Use [camelCase](https://en.wikipedia.org/wiki/Camel_case) for the directive names.

- Directive names should not be excessively long.

```graphql
type query {
  myCamelCaseOperationName(myArgumentName: String @deprecated): String
}

input MyInput {
  field: String! @deprecated
  newField: String!
}
```

#### 2.2.6 Namespacing

HPE GreenLake APIs are governed by the HPE GreenLake team. For a table of existing API groups, see the [Registry of API Groups](api-groups/api-grp-registry.md) section in the HPE GreenLake API standard.

For more information about adding (registering) an API group, see the [Adding a New API Group](api-groups/add-api-grp.md) section in the HPE GreenLake API standard.

API Namespacing is a pattern for GraphQL APIs that allows you to merge different GraphQL APIs into a single unified GraphQL schema.

The GraphQL operation must be prefixed with the API group in [PascalCase](https://en.wiktionary.org/wiki/Pascal_case).

```graphql
type ConfigDevices { ... }
type MonitoringDevices { ... }
```

### 2.3 Versioning

GraphQL strongly advocates for avoiding versioning by offering tools for continuous schema evolution. The following passage from the [GraphQL Best Practices](https://graphql.org/learn/best-practices/#versioning) guide describes the versioning philosophy of GraphQL:
> *While there's nothing that prevents a GraphQL service from being versioned just like any other REST API, GraphQL takes a strong opinion on avoiding versioning by providing the tools for the continuous evolution of a GraphQL schema.*

Versioning of APIs is mostly done in cases where the response or request of APIs is no longer compatible with the current version and is known as a "breaking change."

In GraphQL, adding new fields or types is not a problem as the API will only return the fields that are explicitly requested. This has led to the common practice of avoiding versioning GraphQL.

In the case where versioning is required then you can do so by following a few best practices.

#### 2.3.1 Backward-Compatible Changes

If possible, try to make changes to your schema in a way that is backward-compatible with existing clients. For example, you could add new fields or types to your schema without removing or modifying existing ones. This can help minimize the impact of changes on existing clients and reduce the need for versioning.

#### 2.3.2 Version Field Names

Follow these steps when introducing a new field to replace an earlier field.

1. Re-implement the field using a different name.
1. Deprecate the field, requesting clients to use the new field instead.
1. Whenever the field is not used anymore by anyone, remove it from the schema.

In the below example, we are deprecating `surname` with `personSurname` as the surname is a mandatory field that works for a person but an organization account will not have a surname.

```graphql
type Account {
  id: Int
  name: String!
  surname: String! @deprecated(reason: "Use `personSurname`")
  personSurname: String
}
```

#### 2.3.3 Unions and Interfaces

Use GraphQL unions and interfaces to allow for flexibility in the response. This can allow you to add new types to the schema without breaking existing clients, as long as the new types conform to the existing interface.

For more information about GraphQL interfaces, see the [interfaces](https://spec.graphql.org/June2018/#sec-Interfaces) section in the GraphQL specification.

For more information about GraphQL unions, see the [unions](https://spec.graphql.org/June2018/#sec-Unions) section in the GraphQL specification.

#### 2.3.4 Version Operation Names

Introduce new operations with the version in cases where backward compatibility cannot be retained.

##### 2.3.4.1 Operation Naming Conventions

Use the format `<operation_name>V<version>[<stage>]`

- `<operation_name>`: The name of the operation (e.g., getUsers).

- `<version>`: The version number of the operation (e.g., 1).

- `<stage>` (Optional): The stage of the operation (e.g., Alpha1, Beta1).

In the example below, a new operation, `getUsersV2`, is introduced without breaking the `getUsers` operation. The `getUsers` operation is marked as deprecated, and a reason is provided in the `reason` argument of the `@deprecated` directive. The new and recommended version of the operation is `getUsersV2`.

```graphql
type query {
    "Operation getUsersV1 has been deprecated on Sun, 11 Nov 2018 23:59:59 GMT and sunset is on Wed, 11 Nov 2020 23:59:59 GMT. Use getUsersV2 instead"
    getUsersV1: [User!]! @deprecated(reason: "This query is being deprecated user getUsersV2 instead")
    getUsersV2: [User!]!
}
```

Deprecation must be indicated by both using 'Deprecation' and 'Sunset' response headers.

For more information about the `Deprecation` response header, see the [Deprecation Header](/docs/greenlake/standards/ratified/api/versioning.md#deprecation-header) section in the HPE GreenLake API standard.

For more information about the `Sunset` response header, see the [Sunset Header](/docs/greenlake/standards/ratified/api/versioning.md#sunset-header) section in the HPE GreenLake API standard.

###### 2.3.4.2 Operation Version Stages

The development of an API proceeds through stages of increasing maturity (alpha, beta, and stable) as described in the [Understanding API Version Stages](/docs/greenlake/standards/ratified/api/versioning.md#understanding-api-version-stages) section in the HPE GreenLake API standard.

Operations should be appropriately named using the patterns specified in the guidelines, such alpha or beta.

In the below example, we have `getUsersV1Alpha1`, `getUsersV1Beta1`, and `getUsersV1` depicting stages of the operation with `getUsersV1Alpha1` and `getUsersV1Beta1` marked with alpha and beta custom directives respectively.

```graphql
type query {
    "This is getUsersV1Alpha1 operation and is not production ready."
    getUsersV1Alpha1: [User!]!
    "This is getUsersV1Beta1 operation and is not production ready."
    getUsersV1Beta1: [User!]!
    "This is getUsersV1 operation and is production ready."
    getUsersV1: [User!]!
}
```

### 2.4 Directives

Directives provide additional information to the client. Several directives are defined in the GraphQL specification. Additional custom directives may be defined.

#### 2.4.1 Built-In Directives

The GraphQL specification includes built-in directives that are recommended.

| **Directive** | **Description** |
|----|----|
| `@deprecated(reason: String)` | Indicate the deprecation of a field or enum value in the schema definition, along with an optional reason. |
| `@skip(if: Boolean!)` | If true, the decorated field in an operation is not resolved by the GraphQL server. |
| `@include(if: Boolean!)` | If false, the decorated field in an operation is not resolved by the GraphQL server. |

Usage:

```graphql
type query {
    getUsersV1: [User!]! @deprecated(reason: "This query is being deprecated user getUsersV2 instead")
    getUsersV2: [User!]!
}

type query ($skipTitle: Boolean!) {
  getPost {
    id
    title @skip(if: $skipTitle)
    text
  }
}

type query ($includeAuthor: Boolean!) {
  getPost {
    id
    title
    text
    author @include(if: $includeAuthor) {
        id
        name
    }
  }
}
```

#### 2.4.2 Validation Directives

Custom executable directives may be used for the validation of GraphQL objects. These directives are recommended:

| **Directive** | **Description** |
|----|----|
| `@range(min: Int, max: Int)` | Checks if the given integer is in the inclusive range |
| `@length(min: Int, max: Int)` | Checks if string length is in the inclusive range |
| `@match(pattern: String)` | Checks if string matches the regex pattern.  |

The directive declarations must added to the schema:

```graphql
directive @range(min: Int, max: Int) on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION
directive @length(min: Int, max: Int) on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION
directive @match(pattern: String) on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION
```

Usage:

```graphql
type Mutation {
  createVlan(vlan: Int! @range(min: 60, max: 4000))
  createRole(name: String! @length(min: 1, max: 256))
}

input VlanParameters {
  trunk_allowed_vlans: String @match(pattern: "(\\d+(-\\d+)?)(,(\\d+(-\\d+)?))*")
}
```

#### 2.4.3 Input Directive

The purpose of the custom `@oneOf` directive is to ensure that only one of the provided input objects is selected in a mutation or query.

| **Directive** | **Description** |
|----|----|
| `@oneOf` | Used to indicate that an input object contains two or more other input objects, one of which may be selected. |

The `@oneOf` directive declaration must added to the schema:

```graphql
directive @oneOf on INPUT_OBJECT
```

Usage:

```graphql
input IpAddressInput @oneOf
{
  ipv4_address: Ipv4AddressInput
  ipv6_address: Ipv6AddressInput
}
input Ipv4AddressInput {ipv4_address: String}
input Ipv6AddressInput {ipv6_address: String}
```

## 3. Pagination

The GraphQL pagination behavior MUST align with the GreenLake API standard pagination.

Query operations that return a list of objects MUST support pagination. Even if the API that returns a list of objects doesn't support pagination, it MUST still return the `count` and is strongly encouraged to return the `total` property in the query response.

GraphQL APIs that support pagination can implement cursor-based, offset-based, or both of the pagination models.

In offset-based pagination, the GraphQL client includes an `offset` argument in the query to fetch a page that starts from the required offset. This model is suitable for executing a "random seek" functionality at the cost of potentially inconsistent results when resource collection is modified between the retrieval of the pages.

With cursor-based pagination, the GraphQL service returns the `next` cursor argument in the GraphQL response that designates the end of the returned page. The GraphQL client provides the cursor in the `next` query operation to retrieve the next page. This model is especially suitable for implementing "infinite" streams of data that the GraphQL client intends to fetch in a continuous, forward-moving fashion.

### 3.1 Input Arguments

| **Name** | **Description** | **Type** | **Required?** |
|----|----|----|----|
| `limit` | Specifies the number of results to be returned | Int | Yes |
| `next` | Specifies the pagination cursor for the next page of resources. | String | Only for cursor-based pagination |
| `offset` | Specifies the zero-based resource offset to start the response from | Int | Only for offset-based pagination |

### 3.2 Response Arguments

| **Name** | **Description** | **Type** | **Required?** |
|----|----|----|----|
| `next` | Specifies the pagination cursor for the next page of resources. | String | Only for cursor-based pagination |
| `count` | Number of returned items | Int | Yes |
| `total` | Total number of items in the result set | Int | No |
| `offset` | Specifies the offset of the returned page | Int | Only required if 'offset' was provided in the query. Encouraged otherwise. |

- For APIs that provide cursor-based pagination, `next` must be `null` if there are no further pages.

- `total` is encouraged for every API that returns a list of resources.

- If the request included filtering such as the `filter` query parameter, then the `total` must be the number of items in the result set with the filter applied.

- APIs that support both types of pagination may return both `next` and `offset` per the above rules.

### 3.3 Sample Schema

This schema serves as a comprehensive guide for implementing both cursor-based pagination and offset-based pagination methods.

The **User** type defines a user entity with attributes such as id, name, email, and age.

The **UserPaginationInput** type serves as the input structure for pagination during user retrieval.

Fields:

- `limit: Int`: Indicates the number of results to retrieve.

- `next: String`: Specifies the subsequent page to retrieve, utilized in cursor-based pagination.

- `offset: Int`: Specifies the starting point for result retrieval, utilized in offset-based pagination.

The **UserResponse** type represents the response generated when retrieving users along with pagination details.

Fields:

- `items: [User]!`: Collection of users (cannot be null).

- `count: Int!`: Number of items returned (cannot be null).

- `total: Int`: Total count of items. If the request included filtering such as the filter then the total must be the number of items in the result set with the filter applied.

- `next: String`: Identifies the next page to retrieve, and is mandatory for cursor-based pagination.

- `offset: Int`: Identifies the starting point for result retrieval, and is mandatory for offset-based pagination.

```graphql
"""
Represents a user entity.
"""
type User {
  id: ID!
  name: String!
  email: String!
  age: Int!
  createdAt: DateTime!
  updatedAt: DateTime!
}

"""
Pagination input type.
"""
input UserPaginationInput {
  """
  Specifies the number of results to be returned.
  """
  limit: Int!
  """
  Specifies the next page to be fetched. Used in cursor based pagination.
  """
  next: String!
  """
  Specifies the offset to be used for fetching results. Used in offset based pagination.
  """
  offset: Int!
}

"""
UserResponse with pagination information.
"""
type UserResponse {
  """
  The list of users.
  """
  items: [User]!
  """
  Number of returned items.
  """
  count: Int!
  """
  Total number of items.
  """
  total: Int
  """
  Next page to be fetched. Required in cursor based pagination.
  """
  next: String!
  """
  Offset to be used for fetching results. Required in offset based pagination.
  """
  offset: Int!
}

"""
Type query for fetching users.
"""
type Query {
  """
  Fetches a list of users.
  """
  users(pagination: UserPaginationInput): UserResponse
}
```

## 4. Sorting

A GraphQL API can facilitate sorting by specifying the fields to sort on and the desired sort orders.

It can offer sorting based on one or multiple properties in either ascending or descending order.

To sort on a specific field, an input must be defined with the SortInput suffix.

This input should consist of two fields:

- **field**: The name of a field to sort on. This field must represent a list of enums for defining available sorting fields.

- **order**: The sorting order can be ascending (ASC) or descending (DESC).

### 4.1 Sample Schema

- The **User** type represents a user entity with fields such as id, name, email, and age.

- The **UserSortInput** input type is introduced to specify the sorting criteria. It includes a field of type UserSortFields to indicate the field to sort on (in this case, name or age) and the order field of type SortOrder to specify the sort order (ASC for ascending or DESC for descending).

- The **UserSortFields** enum defines the available fields for sorting users (NAME and AGE in this example).

- The **UserSortOrder** enum defines the ascending (ASC) and descending (DESC) sort order options.

- The **getUsers** query accepts an optional sort argument of type [UserSortInput!]. It returns a list of users, which can be sorted based on the provided sorting criteria.

```graphql
"""
Represents a user entity.
"""
type User {
  id: ID!
  name: String!
  email: String!
  age: Int!
  createdAt: DateTime!
  updatedAt: DateTime!
}

"""
Input type for specifying sorting criteria.
"""
input UserSortInput {
    """
    The field to sort by.
    """
  field: UserSortFields!
    """
    The order to sort by.
    """
  order: SortOrder!
}

"""
Enum representing the fields that can be sorted by.
"""
enum UserSortFields {
  NAME
  EMAIL
  AGE
  CREATED_AT
  UPDATED_AT
}

"""
Enum representing the order to sort by.
"""
enum UserSortOrder {
  ASC
  DESC
}

"""
Type query for fetching users.
"""
type Query {
    """
    Fetches a list of users, sorted by the specified criteria.

    Arguments:
    sort: The sorting criteria.
    """

  getUsers(sort: [UserSortInput!]): [User!]!
}
```

## 5. Filtering

A GraphQL API must support filtering if it returns a list of objects.

The filter variable is used to filter the set of resources returned in a collection-level query. The returned set of resources must match the criteria in the filter query parameter.

The value of the filter is based on [OData 4.0](https://www.odata.org/documentation/) standards, which consist of conjunctions and operands.

Comparison arguments can be property names or literals. Properties that are nested are referenced as a path delimited by a forward slash "/".

In a GraphQL response, the root contains `data`, `errors`, or both in the event of partial success. Therefore, filter expressions will start from the **operation name.**

The below example contains the property **country**/**name** and **country**/**name/states/code**.

```graphql
{
    "data": {
        "country": {
            "name": "United States",
            "native": "United States",
            "emoji": "🇺🇸",
            "currency": "USD,USN,USS",
            "states": [
                {
                    "code": "AL",
                    "name": "Alabama"
                },
                {
                    "code": "AK",
                    "name": "Alaska"
                },
                {
                    "code": "AZ",
                    "name": "Arizona"
                }
            ]
        }
    }
}
```

The following literals are supported:

| **Type** | **Example** | **Description** |
|----|----|----|
| integer | **1234567** | An integer in base 10 (-1234567 for negative integer). |
| decimal | **1234.567**, **-1234.567** | A decimal in base 10 (-1234.567 for negative decimals). |
| timestamp | **2019-10-12T07:20:50.52934852Z** | A timestamp in RFC 3339 date-time format. |
| string | **'this is a string'** | A string value. Strings must begin and end with single quotes. |
| boolean | **true**, **false** | The boolean false/true value. |
| null value | **null** | A null (or nil) value. |

A comparison term compares a property to a literal to produce a logical value (true or false). The following comparisons are supported:

| **Comparison** | **Example** | **Description** |
|----|----|----|
| equality | name **eq** ‘john' | true if the value of the property is the same as the literal. The property is on the left of the operator. Valid for number, boolean, string, or timestamp properties. |
| inequality | name **ne** 'john' | true if the value of the property is not the same as the literal. The property is on the left of the operator. Valid for number, boolean, string, or timestamp properties. |
| greater than | count **gt** 5 | true if the value of the property is greater than the literal. The property is on the left of the operator. Valid for number or timestamp properties. |
| greater than or equal to | count **ge** 5 | true if the value of the property is greater than or equal to the literal. The property is on the left of the operator. Valid for number or timestamp properties. |
| less than | count **lt** 20 | true if the value of the property is less than the literal. The property is on the left of the operator. Valid for number or timestamp properties. |
| less than or equal to | count **le** 20 | true if the value of the property is less than or equal to the literal. The property is on the left of the operator. Valid for number or timestamp properties. |
| literal in array property | 'blue' **in** colors | true if the literal is contained in the property where the property is an array. The literal is on the left of the operator, the right hand operand of the **in** operator is always a collection. Valid for an array of strings. |
| property in list of literals | color **in** ('red','yellow','blue') | true if the value of the property is contained in the list of literals. The property is on the left of the operator, the right hand operand of the **in** operator is always a collection. Valid for string properties. The list of strings is in parentheses. |

The null value is equivalent to itself in a comparison, but it is not greater than or less than any value. For example, the following comparisons result in the respective truth values:

```graphql
null eq null => true
null ne 5 => true
null gt 5 => false
```

String functions defined in [OData 4.0](https://www.odata.org/documentation/) MAY also be supported as comparison terms. Any string functions MUST be documented.

Logical expressions operate on comparison terms. The operators supported in logical expressions are the following:

| **Expression** | **Example** | **Description** |
|----|----|----|
| conjunction | count eq 5 **and** name eq 'fred' | true if both sides of the operator are true. |
| disjunction | count eq 5 **or** name eq 'fred' | true if either side of the operator is true. |
| complement | **not** color in ('RED', 'GREEN', 'BLUE') | true if the right hand side of the operator is false. |
| grouping | **(** count eq 5 or name eq 'fred' **)** and color eq 'RED' | expressions in parentheses are evaluated first with the result used in the outer expression. |

Properties and literals, including booleans, are not supported as operands in logical expressions because they can evaluate as `null`, and logical expressions only support two-value logic (i.e. true and false). Instead, properties and literals should always be used within comparison terms.

The order of precedence among the logical operators is first `not`, then `and`, then `or`. The evaluation order is modified when grouping using parentheses. The following expressions are equivalent:

```graphql
not count eq 5 and name eq 'fred' or color eq 'RED'
((not count eq 5) and name eq 'fred') or (color eq 'RED')
```

GraphQL is a query language that relies on a predefined schema. Therefore, using a schema-less string in the filter is not recommended.

The derived version of OData standards can offer the same level of filtering options. In GraphQL, the filter input will be a simplified AST of an OData expression in JSON format.

**OData Expression**: `filter=name eq 'Tim' and age gt 25`

**Derived Input:** `{"and":[{"eq":[{"name":"Tim"}]},{"gt":[{"age":25}]}]}`

Examples:

Below is a query.

```graphql
query Country($filter: String) {
  country(filter: $filter) {
    name
    native
    emoji
    currency
    emoji
    emojiU
    code
    states {
      code
      name
    }
  }
}
```

Example 1

**OData Expression**: `filter = country/code eq US`

**Derived expression**:

```graphql
{
    "filter":{"eq":[{"country":{"code":"US"}}]}
}
```

Example 2

**OData Expression**: `filter = country/code in ('US', 'IN', 'AD')`

**Derived expression**:

```graphql
{
    "filter":{"in":[{"country":{"code":["US","IN","AD"]}}]}
}
```

## 6. Security

To protect applications and users, ensure that your GraphQL code adheres to HPE and industry security best practices and to the practices described in this section. The HPE GreenLake development standard for [Secure Coding and Review](../../policies/secure-coding.md) describes general HPE secure coding practices.

### 6.1 Authorization

Once a user is authenticated, the GraphQL API SHOULD enforce authorization policies to determine what data the user can access.

#### 6.1.1 Consistent Authorization Semantics

<!-- Markdown doesn't support outline lists (e.g. 1., a., ...) -->
<ol>
    <li>Any API resource, whether accessed through REST or GraphQL, MUST be protected under the exact same authorization
        semantics.</li>
    <ol type="a">
        <li>The authorization rules and permissions MUST be consistently enforced across all API types to ensure a
            coherent and secure access control system.</li>
    </ol>
    <li>Alignment with Platform Authorization System:</li>
    <ol type="a">
        <li>The authorization mechanism implemented for GraphQL APIs MUST be fully compatible and consistent with the
            platform authorization system so that users have one place to manage access.</li>
        <li>This ensures that the GraphQL APIs seamlessly integrate with the overall security infrastructure and
            maintain a unified approach to access control.</li>
    </ol>
    <li>Regular Security Audits:</li>
    <ol type="a">
        <li>The authorization system for GraphQL APIs MUST undergo regular architectural threat analysis (see <a href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/secure_design_and_architecture_policy/">Secure Architecture Design</a>) and penetration testing to
            identify and address any vulnerabilities or weaknesses.</li>
        <li>Security <a href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/secure-coding/">best
                practices and standards</a> SHOULD be followed to ensure the robustness and effectiveness of the
            authorization mechanism.</li>
    </ol>
</ol>

### 6.2 Limit Query Depth

One of the unique features of GraphQL is that clients can specify the depth of the query they want to execute. However, this can lead to performance and security issues if not properly controlled.

GraphQL APIs SHOULD implement query depth limiting to prevent clients from querying too deeply.

## 7. Error Handling

The GraphQL API error handling must conform to that of the [GraphQL specification Errors](https://spec.graphql.org/October2021/#sec-Errors) section.

### 7.1 Use the Standard GraphQL Response Format

All GraphQL responses MUST conform to the standard GraphQL response format, which includes a data field for successful responses and an errors field for errors.

### 7.2 Include Error-Specific Information

Each error object in the errors field MUST include a message field that describes the error. In addition, consider including additional error-specific information in the extensions field, such as debugId, errorCode, errorDetails, message, and httpStatusCode.

### 7.3 Use Consistent Error Formats

To make it easier for clients to handle errors, consider using a consistent error format across your entire API. This might include using a custom error type or simply including consistent fields in your error objects.

### 7.4 Return Informative Error Messages

The error message returned by the message field should be informative and user-friendly. Avoid returning generic error messages like "An error occurred" or "Invalid input".

### 7.5 Return HTTP Status Codes That Match the Error Type

When returning errors, consider using HTTP status codes that match the type of error that occurred. For example, a validation error might be returned with a 400 Bad Request status code, while an authentication error might be returned with a 401 Unauthorized status code.

### 7.6 Sample Error Format

In the example below, the httpStatusCode is 400 (Bad request), and the GraphQL error block provides details that the email input was wrong with GraphQL query information like location and path.

This example also makes use of extensions to provide more information about the error such as `debugId`, `errorCode`, `errorDetails`, `message`, and `httpStatusCode`.

```graphql
{
  "errors": [
    {
      "message": "Invalid input: email must be a valid email address",
      "locations": [
        {
          "line": 2,
          "column": 5
        }
      ],
      "path": [
        "createUser",
        "email"
      ],
      "extensions": {
        "httpStatusCode": 400,
        "errorCode": "HPE_GL_NETWORKING_INVALID_INPUT",
        "message": "Invalid input: email must be a valid email address"
        "debugId": "00003e2503d1805aaaa0fe534c5c107d",
        "errorDetails": "The provided email is not in a valid format."
      }
    }
  ],
  "data": null
}
```

## 8. References

For additional information, see the [Aruba GraphQL guidelines](https://confluence.arubanetworks.com/display/CNX/GraphQL+Guidelines) (VPN and access permission required).

**Original publication date (yyyy-mm-dd):** 2024-07-30
