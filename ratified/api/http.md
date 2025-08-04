# HTTP Protocol Handling

This page describes the HPE GreenLake Semantics for HTTP protocol handling.

## HTTP methods

| Method | Description                                  |
| ------ | -------------------------------------------- |
| GET    | Retrieve resource(s)                         |
| POST   | Create resource or execute complex operation |
| PUT    | Update a resource                            |
| PATCH  | Partial update on a resource                 |
| DELETE | Delete a resource                            |

- `GET` must be idempotent and should not include a request body.
- `POST` should be used to create a new resource in a collection.
- `POST` may also be used to execute complex, actionable operations.
- `PUT` must be idempotent and 'must not' be used to create a resource.
- If an API supports `PATCH` it must follow [RFC 7396](https://datatracker.ietf.org/doc/html/rfc7396) (JSON Merge Patch) and be idempotent.<br> **_NOTE_:** Developers can support
   [RFC 6902](https://datatracker.ietf.org/doc/html/rfc6902) (JSON Patch) in addition to JSON
   Merge Patch.
- `DELETE` must be idempotent.
- Any methods not supported for an endpoint should return a `405` status code.

Idempotent means that the same request can be invoked many times
without the different outcome on the server side, although HTTP
response may be different. For example, the first `DELETE` request may
return 204, but subsequent `DELETE` requests may return 404. Essentially
it means that the result of a successfully performed request is
independent of the number of times it is executed.

### Custom actions

- Custom actions are used to define complex functionality that does not directly map to CRUD operations.
- The action is represented by a verb as the last term in the URI: `/api-group/version/resource-collection/id/action-verb`
- The verb term must be lower-case and use only alphanumeric characters and hyphens.
- `POST` is the preferred method for all custom actions.

Example: `/iam/v1/users/<id>/move`

### Common custom actions

| Action  | Description                                   | Custom Verb |
| ------- | --------------------------------------------- | ----------- |
| Cancel  | To cancel an outstanding operation.           | /cancel     |
| Move    | To move a resource from one parent to another | /move       |
| Suspend | To suspend an operation                       | /suspend    |
| Resume  | To resume an operation                        | /resume     |

## HTTP headers

The purpose of HTTP headers is to provide metadata information about the body or the sender of the message in a uniform, standardized, and isolated way.

- HTTP headers should only be used for the purpose of handling cross-cutting concerns.
- Headers must not include API or domain specific values.
- If available, HTTP standard headers must be used instead of creating a custom header.

When services receive request headers, they must pass on relevant custom headers in addition to the HTTP standard headers in requests/messages dispatched to downstream applications.

### Standard headers

| HTTP Header Name | Description                                                                                                       |
| ---------------- | ------------------------------------------------------------------------------------------------------------------|
| `Authorization`  | The HTTP **`Authorization`** request header contains the credentials to authenticate a user agent with a service |
| `Content-Type`   | This request/response header indicates the media type of the request or response body. <br/>API client must include it with request if the request contains a body, e.g. it is a `POST`, `PUT`, or `PATCH` request. <br/>API developer must include it with response if a response body is included (not used with `204` responses). |
| `Location`       | This response-header field is used to redirect the recipient to a location other than the Request-URI for completion of the request. <br/>It is mandatory for a new resource creation in the `POST` response that returns `201` or in a `202` response of any method.                                                                |

Specific Error Conditions may include additional headers. See [Error Headers](#errors)

### Rate limit headers

The following is based on [draft IETF Rate Limit standard](https://datatracker.ietf.org/doc/draft-ietf-httpapi-ratelimit-headers/). The only difference is usage of the  `X-` prefix since the proposal is not ratified.

X-RateLimit-* fields convey hints from the server to the clients in order to avoid being throttled out.

| Property                | JSON Type | Description                                                                 | Required? |
|------------------------ |-----------|-----------------------------------------------------------------------------|-----------|
| `X-RateLimit-Limit`     | Number    | The service-limit associated to the client in the current limit time-window | No        |
| `X-RateLimit-Remaining` | Number    | The remaining quota-units associated to the client                          | No        |
| `X-RateLimit-Reset`     | Number    | The number of seconds until the quota resets.                               | No        |

Example:

```text
   HTTP/1.1 200 Ok
   Content-Type: application/json
   X-RateLimit-Limit: 100
   X-Ratelimit-Remaining: 0
   X-Ratelimit-Reset: 48
```

### Tracing headers

Services that receive the following headers must forward them to the subsequent services:

- `X-Request-ID`
- `X-B3-TraceId`
- `X-B3-SpanId`
- `X-B3-Sampled`

Services that receive a request that does not contain any of these headers must generate them for the subsequent requests with the exception of `X-B3-Sampled`. The value of `X-B3-SpanId` forwarded to subsequent services must match the ID of the current span within the service and so may be different to the value received.

## HTTP status codes

### Status code ranges

| Range     | Meaning                                                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`2xx`** | Successful execution. It is possible for a method execution to succeed in several ways. This status code specifies which way it succeeded.                                                 |
| **`4xx`** | Usually these are problems with the request, the data in the request, invalid authentication or authorization, etc. In most cases the client can modify their request and resubmit.        |
| **`5xx`** | Server error: The server was not able to execute the method due to site outage or software defect. 5xx range status codes should not be utilized for validation or logical error handling. |

### Status reporting guidelines

Success and failure apply to the whole operation.

Following are the guidelines for status codes and reason phrases.

- Success must be reported with a status code in the `2xx` range.
- HTTP status codes in the `2xx` range must be returned only if the complete code execution path is successful.
- Failures must be reported in the `4xx` or `5xx` range. This is true for both system errors and application errors.
- A service returning a status code in the `4xx` or `5xx` range must return a consistent, JSON-formatted error response in the body.
- A service returning a status code in the `2xx` range must not return any kind of error code as part of the response body.
- For client errors in the `4xx` code range, the reason phrase should provide enough information for the client to be able to determine what caused the error and how to fix it.
- For service errors in the `5xx` code range, the reason phrase and an error response should limit the amount of information to avoid exposing internal service implementation details to clients. This is true for both external facing and internal APIs. Service developers should use logging and tracking utilities to provide additional information.

### HTTP response codes

The following table describes the HTTP response codes used per HTTP verb. HTTP status codes listed to return error properties must return the required error properties. Optional error properties may also be returned.

| Code                         | GET | POST | PUT | PATCH | DELETE | Return Error Properties? | Notes                                                                                                                                                                                                                                                                                         |
| ---------------------------- | --- | ---- | --- | ----- | ------ | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `200` `OK`                   | Y   | Y    | Y   | Y     | Y      |                          | The request has succeeded.                                                                                                                                                                                                                                                                    |
| `201 Created`                |     | Y    |     |       |        |                          | The request has been fulfilled and resulted in a new resource. The response body must include the new resource.                                                                                                                                                                               |
| `202 Accepted`               |     | Y    | Y   | Y     | Y      |                          | The request has been accepted, but processing has not completed. The 'Location' header must provide an async operation uri to allow the client to monitor progress.                                                                                                                           |
| `204 No Content`             |     |      |     |       | Y      |                          | The request has been completed; the response must not contain a body.                                                                                                                                                                                                                         |
| `400 Bad Request`            | Y   | Y    | Y   | Y     | Y      | Y                        | The request could not be understood by the server due to malformed syntax, for example, missing data or incorrect format.                                                                                                                                                                     |
| `401 Unauthorized`           | Y   | Y    | Y   | Y     | Y      | Y                        | The client is not authenticated. Note: Despite the designation "Unauthorized", 401 is not about authorization, it means the user has not logged in, has not presented an auth token, or the auth token is invalid.                                                                            |
| `403 Forbidden`              | Y   | Y    | Y   | Y     | Y      | Y                        | The client is not authorized for this request. Authentication comes first. If the user is not authenticated, 401 is the correct response. If the user is authenticated but not authorized, then 403 is appropriate.                                                                           |
| `404 Not Found`              | Y   | Y    | Y   | Y     | Y      | Y                        | The resource is not found.                                                                                                                                                                                                                                                                    |
| `406 Not Acceptable`         |     |      |     |       |        |                          | The server must return this status code when it cannot return the payload of the response using the media type requested by the client. For example, if the client sends an `Accept: application/xml` header, and the API can only generate `application/json`, the server must return `406`. |
| `409 Conflict`               |     | Y    | Y   | Y     | Y      | Y                        | The request cannot be performed due to a conflict with the current state of the system, for instance, an attempt to delete a resource that is in use or an attribute (name) that must be unique is already in use.                                                                            |
| `412 Precondition Failed`    |     | Y    | Y   | Y     | Y      | Y                        | The request had an If-Match header that did not match the generation of the resource.                                                                                                                                                                                                         |
| `415 Unsupported Media Type` |     | Y    | Y   | Y     |        | Y                        | The server must return this status code when the media type of the request's payload cannot be processed. For example, if the client sends a `Content-Type: application/xml` header, but the API can only accept `application/json`, the server must return `415`.                            |
| `429 Too Many Requests`      | Y   | Y    | Y   | Y     | Y      | Y                        | The server must return this status code if the rate limit for the user, the application, or the token has exceeded a predefined value. Defined in Additional HTTP Status Codes [RFC 6585](https://tools.ietf.org/html/rfc6585).                                                               |
| `500 Internal Server Error`  | Y   | Y    | Y   | Y     | Y      | Y                        | These are essentially bugs within the system that have not been handled gracefully. Usually the response is generated by an exception handler.                                                                                                                                                |
| `503 Service Unavailable`    | Y   | Y    | Y   | Y     | Y      | Y                        | The service is unavailable due to a temporary condition that is not considered an internal error. This error is typically generated by a load balancer or API gateway when it cannot forward a request. It can also be generated by a server if a service it depends on is unavailable.       |

## Asynchronous responses

Operations that require an extended period of time to complete SHOULD do the following:

- Create an async-operation resource to represent the operation
- Return a response with HTTP code `202 Accepted`
- Provide the URI of the async-operation resource in the `Location` header

Example:

```text
Request: POST /block-storage/v1/volumes

Response: 202 Accepted
Location: /block-storage/v1/async-operations/123e4567-e89b-12d3-a456-426614174000
```

Asynchronous operations MUST use a resource collection called `async-operations`. The client will use the `async-operations`
resource to monitor progress of the operation. The `async-operations` resource collection MUST support the following operations:

- `GET /{api-group}/{version}/async-operations` - List all async-operation resources
- `GET /{api-group}/{version}/async-operations/{id}` - Get an async-operation resource by id

A client cannot create an `async-operation` resource directly. The `async-operation` resource is defined below.

### Async-operation resource

In addition to the [Common Properties](./naming.md#common-properties), the following properties are defined for the `async-operations` resource.

Required (Yes) means that the property MUST be defined in the response. Required (No) means that the property MAY be defined in the response.
If the property is used, then it MUST have the defined property name, type, and behavior.

| Property                          | JSON Type | Description                                                                                                                    | Required? |
| --------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------ | --------- |
| `sourceResourceUri`               | string    | URI reference to the resource or resource collection that initiated the operation                                              | Yes       |
| `state`                           | string    | State of the operation. See the table below.                                                                                   | Yes       |
| `startedAt`                       | string    | Time at which the operation entered the `RUNNING` state.                                                                       | Yes       |
| `endedAt`                         | string    | Time at which the operation completed in the `SUCCEEDED`, `FAILED`, or `CANCELLED`.                                            | Yes       |
| `logMessages`                     | array     | List of progress update objects, which may be empty                                                                            | Yes       |
| `progressPercent`                 | number    | Percent progress of the operation as an integer value of 0 to 100.                                                             | Yes       |
| `error`                           | object    | [Error Object](./errors.md#error-object) which MUST be populated if the operation reaches the `FAILED` state.                  | Yes       |
| `recommendations`                 | array     | List of recommendations for resolving the error(s).                                                                            | No        |
| `results`                         | array     | List of references to resources (other than the source resource) which were created, updated, or deleted during the operation. | No        |
| `suggestedPollingIntervalSeconds` | number    | Number of seconds recommended for clients to poll for updates (default: 5).                                                    | No        |
| `timeoutMinutes`                  | number    | Number of minutes after the last update before the operation moves into the `TIMEDOUT` state (default 60).                     | No        |

`sourceResourceUri` is the URI of the resource or resource collection that initiated the operation. For creating a resource with `POST`, it
MUST be the resource collection URI. For other calls it MUST be the URI of the source resource that initiated the operation.

- ex. `"sourceResourceUri": "/block-storage/v1/volumes"`

`logMessages` is a list of progress update items. Each item MUST include the `message` property and MAY include the `timestamp` property.

```json
{
   "logMessages": [
      {
         "message": "Volume Creation initiated",
         "timestamp": "2021-01-01T12:00:00Z"
      },
      {
         "message": "Volume Creation in progress",
         "timestamp": "2021-01-01T12:05:00Z"
      }
   ]
}
```

`results` can be a list of resource references that were created, updated, or deleted during the operation. The resource reference MUST include the `resourceUri` and MAY include
other properties. See [Referencing Resources](./resource_basics.md#referencing-resources) for details.

   ```json
   {
      "results": [
         {
            "resourceUri": "/block-storage/v1beta1/volumes/123",
            "name": "dev-volume"
         },
         {
            "resourceUri": "/block-storage/v1beta1/volumes/456",
            "name": "prod-volume"
         }
      ]
   }
   ```

Async-operation state

| State         | Description                                                          | Active/Inactive | Terminal? |
| ------------- | -------------------------------------------------------------------- | --------------- | --------- |
| `INITIALIZED` | The operation has been created and is waiting to be started          | Active          | No        |
| `RUNNING`     | The operation is running                                             | Active          | No        |
| `PAUSED`      | The operation has been paused by the system and requires user input  | Active          | No        |
| `TIMEDOUT`    | The operation did not receive an update in the last `timeoutMinutes` | Inactive        | No        |
| `SUCCEEDED`   | The operation is complete and succeeded                              | Inactive        | Yes       |
| `FAILED`      | The operation is complete and failed                                 | Inactive        | Yes       |
| `CANCELLED`   | The operation was cancelled                                          | Inactive        | Yes       |

The `TIMEDOUT` state is used to indicate that the operation did not receive an update before `updatedAt` plus `timeoutMinutes`
and should be considered out of date. If the operation receives a later update it will reflect the new state.

### Asynchronous operations and resource creation

A scenario worth special consideration is a POST operation that may create a resource,
but also initiates an asynchronous process in relation to creation of the resource. An example
could be the creation of a storage volume, which may be represented immediately,
but requires asynchronous operations in on-prem devices to complete.

This may be positioned as either a synchronous operation or an asynchronous operation.

In the synchronous case, the resource should be returned in a `201 Created`
response in an "initial" state and completion of further processing should be exposed
as state changes in the properties of the resource (e.g. details become populated, maybe
a state property transitions to "READY", etc.). async-operation should not be used in this
case.

In the asynchronous case, an async-operation should be returned in a `202 Accepted`
response. The resource can be created at any time during the asynchronous processing
and identified in the async-operation properties.

## Conditional requests

A conditional request is one that includes an If-Match header that must match
the "generation" property of a resource in order for the request to be performed.

Resources are _not required_ to support conditional requests.
Resources MUST document the fact that if-match header _is_ supported.

Conditional requests are supported for resources as follows:

- POST, PUT, PATCH or DELETE requests may include an If-Match header with the generation as the value
- POST, PUT, PATCH or DELETE requests must return 412 "Pre-condition Failed" if the If-Match header does not match the generation

ETags are not required in the response to a GET request. This is because the generation
property is already available in a response.

## Errors

All failures of REST APIs must respond with an HTTP error code. See the table above.

### Error response body

- See [Error Handling](errors.md) for details on response body.

## Error response headers

### 429 Too Many Requests

`429 Too Many Requests` errors may be due to rate limits. It is a best practice to return information
about the rate limits to allow the client to throttle requests and handle retries.
