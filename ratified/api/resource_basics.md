# Resource Oriented Design

*“The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service (e.g. "today's weather in Los Angeles"), a collection of other resources, a non-virtual object (e.g. a person), and so on."* - Roy Fielding

Resources are sometimes referred to as the nouns that the HTTP verbs act upon. This is known as REST and is often represented as a JSON document communicated over HTTPS. Other representations can be binary serialization over network protocols such as gRPC (gRPC Remote Procedure Calls) when performance and scalability are a primary concern. Regardless of the protocol, the same resource-oriented design principles should be followed.

HPE GreenLake APIs MUST have a REST API unless technically infeasible and MUST follow the HPE GreenLake API design standards. gRPC design specifications will be published in the future, and all APIs will be expected to comply with that specification.

## REST

REST, which stands for REpresentational State Transfer, was introduced by Roy Fielding in his 2000 dissertation and is an architectural design pattern with many design conventions. REST itself is not a standard, but HPE GreenLake APIs follow a common set of standards.

A REST API is modeled as collections of individually-addressable resources (the nouns of the API) using a set of [standard methods](http.md) according to a [versioning](versioning.md) scheme.

The following sections specifically describe resource representation using HTTPS/JSON methodologies, but the concepts apply to gRPC and other representations.

### Single Resource Response

A GET to a URI that references a single resource will return only that resource. The resource must contain all properties of that resource (e.g. no hiding of properties with default values) except if filtered by a [query parameter](query_parameters.md). This includes properties with values equal to "null".

### Collection of Resources

A collection contains a list of resources. For example, an apartment has a collection of units.

A GET to a URI that references a collection will return an `items` property which is an array of resources where each item is a resource in the collection. Each resource in the array must contain all properties of that resource except if modified by a [query parameter](query_parameters.md). This includes properties with values equal to `null`.

[Paging properties](pagination.md) must also be returned at the top-level of the document.

For example:

```json
{
    "items": [
        {
            "petId": "40ef5c1d-c4ab-430c-a6aa-1cb2d1c3ef2f",
            "name": "simba",
        }, ...
    ],
    ... additional top level metadata
}
```

If a collection is empty, such as when no items match the query, the `items` and paging properties are still returned, but the items array will be empty and the total will be 0.

These query parameters are covered further in the [Query Parameters](query_parameters.md) and [Pagination](pagination.md) standards.

The following table summarizes the required common collection properties:

|Property   |JSON Type   | Description                                               |
|-----------|------------|-----------------------------------------------------------|
|items      |Array       | Array of objects that are the resource in the collection. |

### Singleton Resources

A singleton represents a resource that exists in singular form, such as the overall status for a service:

```json
GET /storage/v1/status
{
  "status": "OK",
  "version": "1.0.2",
  "id": "97a24ade-3753-48da-aaf2-929d9749df46",
  "type": "storage/status",
  "generation": 3,
  "createdAt": "2020-04-14T18:54:01.661Z",
  "updatedAt": "2022-01-14T10:55:34.453Z",
}
```

As with resource collections, singletons may be nested underneath a resource collection, e.g.:

```json
GET /iam/v1alpha1/workspaces/{workspaceId}/preferences
{
    "name": "Workspace preferences",
    "type": "iam/workspace/preferences",
    "id": "97a24ade-3753-48da-aaf2-929d9749df46",
    "generation": 1,
    "createdAt": "2020-04-14T18:54:01.661Z",
    "updatedAt": "2022-01-14T10:55:34.453Z",
    "preference1":"preference1value",
    "preference2":"preference2value"
}
```

A singleton resource MUST contain all the mandatory common properties required for an individual item in the resource collection, as defined
[here](./naming.md#common-properties), including the `id` property.

A singleton resource MUST exist, i.e. a `GET` operation on a singleton resource cannot return 404. Therefore, a singleton resource cannot support
`POST` or `DELETE` method. Other methods, such as `PUT` and `PATCH` may be supported for a singleton resource.

### Empty Bodies

There may be special cases where an empty body response may be returned. An empty body must only be
returned with an HTTP status code of `204` or `304`. Otherwise, a valid JSON document must be returned, even if the document is only an empty object (e.g. `{}`)

### Referencing Resources

If one resource has to reference another resource it *should* be done using a reference object. The reference object *must*
include a `resourceUri`. The value of `resourceUri` is a relative URI based on the API group root, or a URL with a schema and
hostname. References to resources outside of HPE GreenLake Cloud, or with a different hostname, *should* use a URL for the
`resourceUri` value.

Other properties from the referenced resource *may* be included in the reference object. All referenced properties *must*
reflect current values from the referenced resource. They *must not* contain stale information. If the values are maintained via
an asynchronous eventual consistency pattern, then the API documentation *must* clearly set user expectations regarding the
currency of the data.

If a reference object can be used to reference more than one type of resource,  the reference object *should* include
the `type` property to disambiguate the resource type. Only properties that are common to all types of resources that might
be referenced can be included in the reference object.

Examples:

In this example both the network and switch resources are available on the same hostname and therefore the `resourceUri` is
provided relative to the API group. The `name` parameter is included to facilitate a common user use-case and *must* be kept
current.

```json
{
    "name": "dev-network-123",
    "type": "networking/network",
    "id": "4c2d1463-f8f9-449a-b592-c41adac199a4",
    "resourceUri": "/networking/v1beta1/network/4c2d1463-f8f9-449a-b592-c41adac199a4",
    "associatedSwitch": {
        "resourceUri": "/networking/v1beta2/switches/fc4fbacd-7260-4086-a15c-620b43bb2de1",
        "name": "switch-row10-rackA"
    }
}
```

In this example the `compute-ops-mgmt/server` resource has a reference to an associated storage volume in the
`associatedStorageVolume` property. The value includes the schema and hostname because the block-storage API group is on a
different hostname than servers.

```json
{
    "name": "dev-server-exchange",
    "type": "compute-ops-mgmt/server",
    "id": "CN12349876+MX12AB34C",
    "resourceUri": "/compute-ops-mgmt/v1beta2/servers/CN12349876+MX12AB34C",
    "associatedStorageVolume": {
        "resourceUri": "https://us-west-2-api.common.cloud.hpe.com/block-storage/v1beta2/volumes/988f2672-284c-4942-a3b4-e3b61bd89f42"
    }
}
```

In this example the `storage-fleet/async-operation` resource has a list of references to resources involved in
the operation: one is a volume and the other is a storage system.

```json
{
    "name": "volume deletion",
    "type": "storage-fleet/async-operation",
    "id": "d1f2b3c4-5e6f-7a8b-9c0d-e1f2g3h4i5j6",
    "resourceUri": "/storage-fleet/v1beta1/async-operations/d1f2b3c4-5e6f-7a8b-9c0d-e1f2g3h4i5j6",
    "associatedResources": [
        {
            "resourceUri": "/storage-fleet/v1beta1/volumes/0de2aeab-2da2-48d9-bdc3-8cb2a331c0e9",
            "type": "storage-fleet/volume"
        },
        {
            "resourceUri": "/storage-fleet/v1beta1/storage-systems/0de2aeab-2da2-48d9-bdc3-8cb2a331c0e9",
            "type": "storage-fleet/storage-system"
        }
    ]
}
```
