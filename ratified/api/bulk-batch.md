# Bulk and Batch Operations

The following section explain Bulk and Batch operations.

## Bulk

Bulk operations is a way to perform same operation with the same request arguments
on multiple resources on a given resource collection in a single request.
Bulk operations should be implemented using the `bulk` path, and may be
implemented using query parameters.

## Bulk Path Format

Using the `bulk` path endpoint is supported for the following HTTP operations: `DELETE`, `PATCH`, `POST`. The general format for the
`bulk` path is as follows:

* `/api-group/version/resource-collection/bulk`
* `/api-group/version/resource-collection/resource-id/resource-collection/bulk`

Examples of valid bulk paths include:

* `/compute-ops-mgmt/v1/groups/bulk`
* `/compute-ops-mgmt/v1/groups/16b78cf2-6d1f-4855-911a-5217d0a45b9f/devices/bulk`
* `/devices/v1/devices/bulk`

`bulk` must be treated as a reserved word in the resource collection identifier space and must not be used as an identifier for
a resource in the collection.

## Bulk Post Operation

A Bulk `POST` operation is used to create multiple resources in a single request. The request body contains a predefined data
structure to be replicated across multiple newly created resources, each receiving a unique identifier. The response will indicate
the number of successfully created resources, as well as any errors encountered.

Following are guidelines for bulk `POST` operation:

* The operation must be non-atomic. If an error occurs while creating a resource, the operation should continue for the remaining resources. The response must indicate which resources were successfully created and which encountered errors.
* The operation must be synchronous. The response must be returned immediately after the operation is complete.

The following response codes are commonly applicable for bulk `POST` operations, though others may also be used depending on specific scenarios.

| HTTP Status Code            | Description                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------|
| `200 OK`                    | The operation succeeded. Individual resources may have succeeded or failed.               |
| `400 Bad Request`           | The request is malformed (incl. more resources in the request than the documented limit). |
| `403 Forbidden`             | The user does not have permission to create the resource.                                 |
| `500 Internal Server Error` | An unexpected error occurred.                                                             |

* The `200 OK` response object will provide details on the resources that were successfully created and those that were not.
  Errors can include individual resource errors, such as permission errors (scope based access control) or resource not found errors.

### Bulk Post - Request Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type ,and behavior.

| Property      | JSON Type | Description                                      | Required? |
| ------------ | --------- | --------------------------------------------------| --------- |
| `count`      | integer   | Number of resources to create                     | Yes       |
| `data`       | object    | Common attributes to be applied to all resources  | Yes       |

* The `count` field specifies the number of new resources to be created.
* The `data` field contains the properties that will be **shared by all newly created resources** except for the `id`, which will be generated uniquely for each resource.

**Example:**

```json
{
  "count": 4,
  "data": {
    "tags": {
      "location": "San Jose"
    },
    "foo": "bar"
  }
}
```

### Bulk Post - Response Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property       | JSON Type | Description                                         | Required? |
| -------------- | --------- | --------------------------------------------------- | --------- |
| `successCount` | integer   | Number of resources successfully created            | Yes       |
| `errorCount`   | integer   | Number of resources not created                     | Yes       |
| `successes`    | array     | List of resources successfully created              | Yes       |
| `errors`       | array     | List of errors for resources not created. See below | Yes       |

* The sum of `successCount` and `errorCount` must equal the number of items in the request.
* Each element in the `successes` array must contain an `id` property that is the identifier of the resource to create.
  as well as an `httpStatusCode`, `errorCode`, `message`, `debugId`, and `errorDetails` property related to the error.

### Bulk Post - Response Error Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property         | JSON Type | Description                                                                                        | Required? |
| ---------------- | --------- | -------------------------------------------------------------------------------------------------- | --------- |
| `httpStatusCode` | integer   | Equivalent HTTP status code for the error                                                          | Yes       |
| `errorCode`      | string    | A unique machine-friendly, but human-readable identifier for the error                             | Yes       |
| `message`        | string    | A user-friendly error message                                                                      | Yes       |
| `debugId`        | string    | A unique identifier for the instance of this error                                                 | Yes       |
| `errorDetails`   | array     | Any general metadata [error details](./errors.md#error-details---general-metadata) about the error | No        |

* The `errorCode` must follow the [error code format](./errors.md#more-on-error-codes).
* While errorDetails is not required, it’s recommended to include it when possible, as it can provide valuable context for debugging and support.

**Example:**

```json
{
  "successCount": 2,
  "errorCount": 2,
  "successes": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3",
      ...
      "tags": {
        "location": "San Jose"
      },
      "foo" : "bar"
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c",
      ...
       "tags": {
        "location": "Santa Clara"
      },
      "foo" : "test"
    }
  ],
  "errors": [
    {
      "httpStatusCode": 400,
      "errorCode": "HPE_GL_ERROR_BAD_REQUEST",
      "message": "BAD REQUEST",
      "debugId": "12312-123123-123123-1231212",
      "errorDetails":
      [
        {
          "type": "hpe.greenlake.bad_request",
          "issues":
          [
              {
                  "source": "field",
                  "subject": "foo",
                  "description": "Invalid format."
              },
              {
                  "source": "field",
                  "subject": "foo",
                  "description": "Invalid size."
              }
          ]
        }
      ]
    },
    {
      "httpStatusCode": 500,
      "errorCode": "HPE_GL_ERROR_INTERNAL_SERVER_ERROR",
      "message": "Internal Server Error.",
      "debugId": "",
      "errorDetails": []
    }
  ]
}
```

#### Bulk Post Error Handling

* The errors list does not need to include an individual error entry for every failed item.
* If multiple items fail with the same error code, they may be combined into a single error entry.
* The errorDetails field should provide additional information, such as the number of items that failed with the same error.

## Bulk Patch Operation

A Bulk `PATCH` operation is used to update multiple resources in a single request. The request body contains a list of
resource identifiers along with optional headers for conditional updates. The response will indicate the number of
successfully updated resources as well as any errors encountered.

Following are guidelines for bulk `PATCH` operation:

* The operation must be non-atomic. If an error occurs while updating a resource, the operation should continue for
  the remaining resources. The response must indicate which resources were successfully updated and which encountered errors.
* The operation must be synchronous. The response must be returned immediately after the operation is complete.
* The request body must include a list of resource identifiers (`items`) along with a `data` field that defines the
  changes to be applied to all specified resources.

The following response codes are commonly applicable for bulk `PATCH` operations, though others may also be used depending on specific scenarios.

| HTTP Status Code            | Description                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------|
| `200 OK`                    | The operation succeeded. Individual resources may have succeeded or failed.               |
| `400 Bad Request`           | The request is malformed (incl. more resources in the request than the documented limit). |
| `403 Forbidden`             | The user does not have permission to update the resource.                                 |
| `500 Internal Server Error` | An unexpected error occurred.                                                             |

* The `200 OK` response object will provide details on the resources that were successfully updated and those that were not.
  Errors can include individual resource errors, such as permission errors (scope based access control) or resource not found errors.

### Bulk Patch - Request Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property   | JSON Type | Description                                                                   | Required? |
| ---------- | --------- | ----------------------------------------------------------------------------- | --------- |
| `items`    | array     | A list of resources to update.                                                | Yes       |
| `data`     | object    | Common object to apply for all resources. Format requirements descried below. | Yes       |

* Each element in the `items` array must contain an `id` property that is the identifier of the resource to update.
* Each element in the `items` array should contain a `headers` property, which is optional and may include conditional request headers such as `"If-Match"`.
* `data` object must follow request JSON format described in RFC 7396 (JSON Merge Patch)
and additionally may support request JSON format described  RFC 6902 (JSON Patch).

**Example:**

```json
{
  "items": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3",
      "headers": {
        "If-Match": "2"
      }
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c",
      "headers": {
        "If-Match": "3"
      }
    },
    {
      "id": "313ea04b-fda3-496b-b142-3577dd897c4b"
    },
    {
      "id": "30d6bbb5-8359-48bd-8675-399dba01a859"
    }
  ],
  "data": {
    "tags": {
      "location": "San Jose"
    },
    "foo": "bar"
  }
}
```

### Bulk Patch - Response Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property       | JSON Type | Description                                         | Required? |
| -------------- | --------- | --------------------------------------------------- | --------- |
| `successCount` | integer   | Number of resources successfully updated            | Yes       |
| `errorCount`   | integer   | Number of resources not updated                     | Yes       |
| `successes`    | array     | List of resources successfully updated              | Yes       |
| `errors`       | array     | List of errors for resources not updated. See below | Yes       |

* The sum of `successCount` and `errorCount` must equal the number of items in the request.
* Each element in the `successes` array must contain an `id` property that is the identifier of the resource to update.
* Each element in the `errors` array must contain an `id` property that is the identifier of the resource that was not updated,
  as well as an `httpStatusCode`, `errorCode`, `message`, `debugId`, and `errorDetails` property related to the error.

* Recommendation is to return full representation of the resource in the succeed items list. However, to provide flexibility to implementers, the response may return only the resource `id`. Whichever option is implemented items should be either full representation or just id.

* Returning just id allows server side to not read all properties of specified items.

### Bulk Patch - Response Error Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property         | JSON Type | Description                                                                                        | Required? |
| ---------------- | --------- | -------------------------------------------------------------------------------------------------- | --------- |
| `id`             | string    | Identifier of the resource not updated                                                             | Yes       |
| `httpStatusCode` | integer   | Equivalent HTTP status code for the error                                                          | Yes       |
| `errorCode`      | string    | A unique machine-friendly, but human-readable identifier for the error                             | Yes       |
| `message`        | string    | A user-friendly error message                                                                      | Yes       |
| `debugId`        | string    | A unique identifier for the instance of this error                                                 | Yes       |
| `errorDetails`   | array     | Any general metadata [error details](./errors.md#error-details---general-metadata) about the error | No        |

* The `errorCode` must follow the [error code format](./errors.md#more-on-error-codes).
* While errorDetails is not required, it’s recommended to include it when possible, as it can provide valuable context for debugging and support.

**Example:**

```json
{
  "successCount": 2,
  "errorCount": 2,
  "successes": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3"
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c"
    }
  ],
  "errors": [
    {
      "id": "51e8fe83-b0d6-4622-93f6-b89b4adfe1ca",
      "httpStatusCode": 400,
      "errorCode": "HPE_GL_ERROR_BAD_REQUEST",
      "message": "BAD REQUEST",
      "debugId": "",
      "errorDetails":
      [
        {
          "type": "hpe.greenlake.bad_request",
          "issues":
          [
              {
                  "source": "field",
                  "subject": "foo",
                  "description": "Invalid format."
              },
              {
                  "source": "field",
                  "subject": "foo",
                  "description": "Invalid size."
              }
          ]
        }
      ]
    },
    {
      "id": "30d6bbb5-8359-48bd-8675-399dba01a859",
      "httpStatusCode": 404,
      "errorCode": "HPE_GL_ERROR_NOT_FOUND",
      "message": "Resource not found.",
      "debugId": "",
      "errorDetails": []
    }
  ]
}
```

#### Bulk Patch Error Handling

* The errors list does not need to include an individual error entry for every failed item.
* If multiple items fail with the same error code, they may be combined into a single error entry.
* The errorDetails field should provide additional information, such as the number of items that failed with the same error.

## Bulk Delete Operation

A Bulk `DELETE` operation is used to delete multiple resources in a single request. The request body will contain a list of
resource identifiers to delete. The response will contain a list of resource identifiers that were successfully deleted, as well
as a list of errors.

Following are guidelines for bulk `DELETE` operation:

* The operation should be non-atomic. If an error occurs while deleting a resource, the operation should continue to delete other resources.
  The response must indicate which resources were successfully deleted and which were not.
* The operation must be synchronous. The response must be returned immediately after the operation is complete.

The following response codes are commonly applicable for bulk `DELETE` operations, though others may also be used depending on specific scenarios.

| HTTP Status Code            | Description                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------|
| `200 OK`                    | The operation succeeded. Individual resources may have succeeded or failed.               |
| `400 Bad Request`           | The request is malformed (incl. more resources in the request than the documented limit). |
| `403 Forbidden`             | The user does not have permission to delete the resource.                                 |
| `500 Internal Server Error` | An unexpected error occurred.                                                             |

* The `200 OK` response object will provide details on the resources that were successfully deleted and those that were not.
  Errors can include individual resource errors, such as permission errors (scope based access control) or resource not found errors.

### Bulk Delete - Request Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property | JSON Type | Description                   | Required? |
| -------- | --------- | ----------------------------- | --------- |
| `items`  | array     | A list of resources to delete | Yes       |

* Each element in the `items` array must contain an `id` property that is the identifier of the resource to delete.

**Example:**

```json
{
  "items": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3"
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c"
    },
    {
      "id": "313ea04b-fda3-496b-b142-3577dd897c4b"
    },
    {
      "id": "30d6bbb5-8359-48bd-8675-399dba01a859"
    }
  ]
}
```

### Bulk Delete - Response Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property       | JSON Type | Description                                         | Required? |
| -------------- | --------- | --------------------------------------------------- | --------- |
| `successCount` | integer   | Number of resources successfully deleted            | Yes       |
| `errorCount`   | integer   | Number of resources not deleted                     | Yes       |
| `successes`    | array     | List of resources successfully deleted              | Yes       |
| `errors`       | array     | List of errors for resources not deleted. See below | Yes       |

* The sum of `successCount` and `errorCount` must equal the number of items in the request.
* Each element in the `successes` array must contain an `id` property that is the identifier of the resource to delete.
* Each element in the `errors` array must contain an `id` property that is the identifier of the resource that was not deleted,
  as well as an `httpStatusCode`, `errorCode`, `message`, `debugId`, and `errorDetails` property related to the error.

### Bulk Delete - Response Error Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property         | JSON Type | Description                                                                                        | Required? |
| ---------------- | --------- | -------------------------------------------------------------------------------------------------- | --------- |
| `id`             | string    | Identifier of the resource not deleted                                                             | Yes       |
| `httpStatusCode` | integer   | Equivalent HTTP status code for the error                                                          | Yes       |
| `errorCode`      | string    | A unique machine-friendly, but human-readable identifier for the error                             | Yes       |
| `message`        | string    | A user-friendly error message                                                                      | Yes       |
| `debugId`        | string    | A unique identifier for the instance of this error                                                 | Yes       |
| `errorDetails`   | array     | Any general metadata [error details](./errors.md#error-details---general-metadata) about the error | No        |

* The `errorCode` must follow the [error code format](./errors.md#more-on-error-codes).
* While errorDetails is not required, it’s recommended to include it when possible, as it can provide valuable context for debugging and support.

**Example:**

```json
{
  "successCount": 2,
  "errorCount": 2,
  "successes": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3"
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c"
    }
  ],
  "errors": [
    {
      "id": "313ea04b-fda3-496b-b142-3577dd897c4b",
      "httpStatusCode": 403,
      "errorCode": "HPE_GL_ERROR_FORBIDDEN",
      "message": "Access denied.",
      "debugId": "",
      "errorDetails": []
    },
    {
      "id": "30d6bbb5-8359-48bd-8675-399dba01a859",
      "httpStatusCode": 404,
      "errorCode": "HPE_GL_ERROR_NOT_FOUND",
      "message": "Resource not found.",
      "debugId": "",
      "errorDetails": []
    }
  ]
}
```

#### Bulk Delete Error Handling

* The errors list does not need to include an individual error entry for every failed item.
* If multiple items fail with the same error code, they may be combined into a single error entry.
* The errorDetails field should provide additional information, such as the number of items that failed with the same error.

## Query Parameter

> ⚠️ **Deprecated**: Using id query parameters for bulk operations is now deprecated. Please use request body for bulk operations instead.

Using the `id` query parameter is supported for bulk operations with the following HTTP verbs: `DELETE`, `PATCH`.

### Query Parameter - Bulk Operations Guidelines

Following are guidelines for bulk operations:

* The API documentation *must* state whether the operation is atomic (either fully successful or failing without modifying any resources)
  or whether it might result in a partial update or deletion.
  * A service should limit the number of resources that can be modified in a single atomic request to avoid implementation challenges
    around rollback (ex. undoing a delete).
* The API documentation *must* state whether the operation is synchronous or asynchronous.
  * For asynchronous operations, see the [asynchronous response](./http.md#asynchronous-responses) guidelines.
* The maximum number of resources that can be modified in a single request *should* be documented and will be based on the maximum URL length supported by the implementation.
* In the case of a partial update or deletion, an appropriate error response *must* be provided using a corresponding synchronous or asynchronous method.
  * The [error response](./errors.md#error-object) *must* include all resources that were not updated or deleted.
* The `id` parameter is used to specify a subset of collection items and *must* be documented with the
  [OpenAPI parameter](https://spec.openapis.org/oas/v3.1.0#parameterStyle) specified as `style: form` and `explode: true`.

### Query Parameter - Bulk Delete

> ⚠️ **Deprecated**: Using id query parameters for bulk operations is now deprecated. Please use request body for bulk operations instead as described in [Bulk Operations with Request Body](#bulk-delete---request-object). This deprecation does not apply to the [force query parameter](./query_parameters.md#force-delete),  which remains valid.

To delete a subset of the collection items, `DELETE` should accept one or more `id` query parameters.

Example:

```text
DELETE /storage/v1/storage-systems?id=123e4567-e89b-12d3-a456-426614174000&id=123e4567-e89b-12d3-a456-426614174001
```

### Query Parameter - Bulk Patch

> ⚠️ **Deprecated**: Using id query parameters for bulk operations is now deprecated. Please use request body for bulk operations instead as described in [Bulk Operations with Request Body](#bulk-patch---request-object).

To patch a subset of the collection items, `PATCH` should accept one or more `id` query parameters.

Example:

```text
PATCH /storage/v1/storage-systems?id=123e4567-e89b-12d3-a456-426614174000&id=123e4567-e89b-12d3-a456-426614174001
{
  tags: {
    org: "finance",
  }
}
```

## Batch

Batch operations is a way to perform same operation with different request arguments
on multiple resources on a given resource collection in a single request.
Batch operations should be implemented using the `batch` path.

## Batch Path Format

Using the `batch` path endpoint is supported for the following HTTP operations: `POST`, `PATCH`. The general format for the
`batch` path is as follows:

* `/api-group/version/resource-collection/batch`
* `/api-group/version/resource-collection/resource-id/resource-collection/batch`

Examples of valid batch paths include:

* `/devices/v1/devices/batch`
* `/compute-ops-mgmt/v1/groups/batch`
* `/compute-ops-mgmt/v1/groups/16b78cf2-6d1f-4855-911a-5217d0a45b9f/devices/batch`

`batch` must be treated as a reserved word in the resource collection identifier space and must not be used as an identifier for
a resource in the collection.

## Batch Post Operation

A Batch `POST` operation is used to create multiple resources in a single request. The request body contains items array with properties for individual resource. The response will indicate the number of successfully created resources, as well as any errors encountered.

Following are guidelines for batch `POST` operation:

* The operation must be non-atomic. If an error occurs while creating a resource, the operation should continue for the remaining resources. The response must indicate which resources were successfully created and which encountered errors.
* The operation must be synchronous. The response must be returned immediately after the operation is complete.

The following response codes are commonly applicable for batch `POST` operations, though others may also be used depending on specific scenarios.

| HTTP Status Code            | Description                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------|
| `200 OK`                    | The operation succeeded. Individual resources may have succeeded or failed.               |
| `400 Bad Request`           | The request is malformed (incl. more resources in the request than the documented limit). |
| `403 Forbidden`             | The user does not have permission to create the resource.                                 |
| `500 Internal Server Error` | An unexpected error occurred.                                                             |

* The `200 OK` response object will provide details on the resources that were successfully created and those that were not.
  Errors can include individual resource errors.

### Batch Post - Request Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type ,and behavior.

| Property     | JSON Type | Description                                                     | Required? |
| ------------ | --------- | ----------------------------------------------------------------| --------- |
| `items`      | array     | Array of individual resources with their properties and values. | Yes       |

**Example:**

Creation of multiple devices in the single request.

```json
{
  "items": [
      {
        "foo" : "bar",
        "deviceType": "switch",
        "tags": {
          "location": "San Jose"
        },
      },
      {
        "foo" : "bar",
        "deviceType": "access point",
      },
      {
        "foo" : "111",
        "deviceType": "server",
      },
      {
        "foo" : "bar",
        "deviceType": "storage",
      },
  ],
}
```

### Batch Post - Response Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property       | JSON Type | Description                                         | Required? |
| -------------- | --------- | --------------------------------------------------- | --------- |
| `successCount` | integer   | Number of resources successfully created            | Yes       |
| `errorCount`   | integer   | Number of resources not created                     | Yes       |
| `successes`    | array     | List of resources successfully created              | Yes       |
| `errors`       | array     | List of errors for resources not created. See below | Yes       |

* The sum of `successCount` and `errorCount` must equal the number of items in the request.
* Each item in the `successes` array must contain an `id` property that is the identifier of the resource to create.
* Each item in `errors` should follow format defined in [error object](./errors.md#error-object).

### Batch Post - Response Error Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property         | JSON Type | Description                                                                                        | Required? |
| ---------------- | --------- | -------------------------------------------------------------------------------------------------- | --------- |
| `httpStatusCode` | integer   | Equivalent HTTP status code for the error                                                          | Yes       |
| `errorCode`      | string    | A unique machine-friendly, but human-readable identifier for the error                             | Yes       |
| `message`        | string    | A user-friendly error message                                                                      | Yes       |
| `debugId`        | string    | A unique identifier for the instance of this error                                                 | Yes       |
| `errorDetails`   | array     | Any general metadata [error details](./errors.md#error-details---general-metadata) about the error | No        |

* The `errorCode` must follow the [error code format](./errors.md#more-on-error-codes).
* While `errorDetails` is not required, it’s recommended to include it when possible, as it can provide valuable context for debugging and support.

**Example:**

```json
{
  "successCount": 2,
  "errorCount": 2,
  "successes": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3",
      ...
      "tags": {
        "location": "San Jose"
      },
      "foo" : "bar",
      "deviceType" : "switch",
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c",
      ...
      "foo" : "bar",
      "deviceType": "access point"
    }
  ],
  "errors": [
    {
      "httpStatusCode": 400,
      "errorCode": "HPE_GL_ERROR_BAD_REQUEST",
      "message": "BAD REQUEST",
      "debugId": "12312-123123-123123-1231212",
      "errorDetails":
      [
        {
          "type": "hpe.greenlake.bad_request",
          "issues":
          [
              {
                  "source": "field",
                  "subject": "foo",
                  "description": "Invalid format. Numerical values are not supported."
              },
          ]
        }
      ]
    },
    {
      "httpStatusCode": 500,
      "errorCode": "HPE_GL_ERROR_INTERNAL_SERVER_ERROR",
      "message": "Internal Server Error.",
      "debugId": "",
      "errorDetails":
      [
        {
          "type": "hpe.greenlake.devices.internal_server_error",
          "issues":
          [
              {
                  "source": "field",
                  "subject": "deviceType",
                  "description": "Internal Server Error. Unable to create resource."
              },
          ]
        }
      ]
    }
  ]
}
```

#### Batch Post Error Handling

* The errors list does not need to include an individual error entry for every failed item.
* If multiple items fail with the same error code, they may be combined into a single error entry.
* The errorDetails field should provide additional information, such as the number of items that failed with the same error.
* As there are no ids in the request body the recommendation is to use properties values from the request to describe errors.

## Batch Patch Operation

A Batch `PATCH` operation is used to update multiple resources in a single request.
The request body contains items array with properties for individual resources and with optional headers for conditional updates.
The response will indicate the number of successfully updated resources, as well as any errors encountered.

Following are guidelines for batch `PATCH` operation:

* The operation must be non-atomic. If an error occurs while updating a resource, the operation should continue for the remaining resources. The response must indicate which resources were successfully updated and which encountered errors.
* The operation must be synchronous. The response must be returned immediately after the operation is complete.
* The request body must include a array of resources `items` with an `data` field for individual item in the array which defines the changes to be applied to the specific resource.

The following response codes are commonly applicable for batch `PATCH` operations, though others may also be used depending on specific scenarios.

| HTTP Status Code            | Description                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------|
| `200 OK`                    | The operation succeeded. Individual resources may have succeeded or failed.               |
| `400 Bad Request`           | The request is malformed (incl. more resources in the request than the documented limit). |
| `403 Forbidden`             | The user does not have permission to create the resource.                                 |
| `500 Internal Server Error` | An unexpected error occurred.                                                             |

* The `200 OK` response object will provide details on the resources that were successfully updated and those that were not.
Errors can include individual resource errors.

### Batch Patch - Request Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type ,and behavior.

| Property     | JSON Type | Description                                                                           | Required? |
| ------------ | --------- | ------------------------------------------------------------------------------------- | --------- |
| `items`      | array     | Array of individual resources with their properties and values.                       | Yes       |
| `items/data` | object    | Changes for each individual resource to apply. Format requirements descried below.    | Yes       |

* `items/data` object must follow request JSON format described in RFC 7396 (JSON Merge Patch)
and additionally may support request JSON format described  RFC 6902 (JSON Patch).
* Each element in the `items` array may contain a `headers` property, which is optional and may include conditional request headers such as `"If-Match"`.

**Example:**

Update of multiple devices in the single request.
In the example, the first and the last item in the array have `headers` property with `If-Match` header where value is `generation` number from common properties to allow
[conditional request](http.md#conditional-requests)).

```json
{
  "items": [
      {
        "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3",
        "headers" : {
          "If-Match": "3"
        },
        "data": {
          "tags": {
            "location": "San Jose"
          },
        }
      },
      {
        "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c",
        "data" : {
          "foo" : "bar",
        }
      },
      {
        "id": "313ea04b-fda3-496b-b142-3577dd897c4b",
        "data" : {
          "foo" : "111",
        }
      },
      {
        "id": "30d6bbb5-8359-48bd-8675-399dba01a859",
        "headers" : {
          "If-Match": "4"
        },
        "data" : {
          "tags": {
            "location": "New York"
          },
        }
      },
  ],
}
```

### Batch Patch - Response Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property       | JSON Type | Description                                         | Required? |
| -------------- | --------- | --------------------------------------------------- | --------- |
| `successCount` | integer   | Number of resources successfully created            | Yes       |
| `errorCount`   | integer   | Number of resources not created                     | Yes       |
| `successes`    | array     | List of resources successfully created              | Yes       |
| `errors`       | array     | List of errors for resources not created. See below | Yes       |

* The sum of `successCount` and `errorCount` must equal the number of items in the request.
* Each item in the `successes` array must contain an `id` property that is the identifier of the resource to create.
* Each item in `errors` should follow format defined in [error object](./errors.md#error-object).

### Batch Patch - Response Error Object

Required = Yes means the property must be defined for the resource. Required = No means the property may be defined for the resource.
If used, the properties must have the defined property name, type, and behavior.

| Property         | JSON Type | Description                                                                                        | Required? |
| ---------------- | --------- | -------------------------------------------------------------------------------------------------- | --------- |
| `httpStatusCode` | integer   | Equivalent HTTP status code for the error                                                          | Yes       |
| `errorCode`      | string    | A unique machine-friendly, but human-readable identifier for the error                             | Yes       |
| `message`        | string    | A user-friendly error message                                                                      | Yes       |
| `debugId`        | string    | A unique identifier for the instance of this error                                                 | Yes       |
| `errorDetails`   | array     | Any general metadata [error details](./errors.md#error-details---general-metadata) about the error | No        |

* The `errorCode` must follow the [error code format](./errors.md#more-on-error-codes).
* While `errorDetails` is not required, it’s recommended to include it when possible, as it can provide valuable context for debugging and support.

**Example:**

```json
{
  "successCount": 2,
  "errorCount": 2,
  "successes": [
    {
      "id": "a998cbe4-3bfb-43bd-9e29-6c8a7ec6cfb3",
      ...
      "tags": {
        "location": "San Jose"
      },
    },
    {
      "id": "ef4ffcae-d62f-4ce8-b218-8397661d247c",
      ...
      "foo" : "bar",
    }
  ],
  "errors": [
    {
      "id" : "313ea04b-fda3-496b-b142-3577dd897c4b",
      "httpStatusCode": 400,
      "errorCode": "HPE_GL_ERROR_BAD_REQUEST",
      "message": "Invalid user input.",
      "debugId": "12312-123123-123123-1231212",
      "errorDetails":
      [
        {
          "type": "hpe.greenlake.bad_request",
          "issues":
          [
              {
                  "source": "field",
                  "subject": "foo",
                  "description": "Invalid format. Numerical values are not supported."
              },
          ]
        }
      ]
    },
    {
      "id" : "30d6bbb5-8359-48bd-8675-399dba01a859",
      "httpStatusCode": 412,
      "errorCode": "HPE_GL_ERROR_PRECONDITION_FAILED",
      "message": "Attempt to update resource in outdated state. Generation number does not match.",
      "debugId": "",
      "errorDetails":
      [
        {
          "type": "hpe.greenlake.devices.precondition_failed",
          "metadata": {
              "if_match_header": "Generation number does not match with current value.",
          }
        }
      ]
    }
  ]
}
```

#### Batch Patch Error Handling

* The errors list does not need to include an individual error entry for every failed item.
* If multiple items fail with the same error code, they may be combined into a single error entry.
* The errorDetails field should provide additional information, such as the number of items that
failed with the same error and list of ids.
