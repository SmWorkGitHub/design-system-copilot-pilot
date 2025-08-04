# Errors

HPE GreenLake Cloud Platform APIs follow a common error pattern intended to offer a consistent experience across APIs, protocols,
and usage paradigms. This allows callers to utilize similar error handling across interactions with HPE GreenLake.

## Error Object

When an API has an error, it MUST provide an error response or object.

The following table describes common error properties and whether
or not the property is a required property when an error is returned.

| Property            |JSON Type | Description                                                           | Required? |
|---------------------|----------|-----------------------------------------------------------------------|-----------|
| `httpStatusCode`    | integer  | The HTTP equivalent status code.                                      | Yes       |
| `errorCode`         | string   | A unique machine-friendly, but human-readable identifier for the error from a global list of enumerated identifier strings. See below for more on error codes. | Yes       |
| `message`           | string   | A user-friendly error message.                                        | Yes       |
| `debugId`           | string   | A unique identifier for the instance of this error. This can be used to help with troubleshooting. May be the same as a trace id (see Trace Headers) for ease of log tracing                                    | Yes       |
| `errorDetails`     | array | Additional detailed information about the error. See below                | No        |

### More on Error Codes

The `errorCode` in the error object is primarily intended to enable API callers to automate error handling. This enables the calling code to recover without human intervention. This also enables improved supportability since the error code SHOULD be available in any logs and can therefore be correlated to troubleshooting information.

Each `errorCode` SHOULD have an API specific meaning that MUST NOT change over time.

The general format for the `errorCode` is:

* `HPE_GL_{APIGROUP}_{SPECIFIC_ERROR}`

Where:

* `{APIGROUP}` SHOULD correlate to api groups in the [API Group Registry](api-groups/api-grp-registry.md). If the api group contains a hyphen (`-`), replace it with an underscore (`_`).
* `{SPECIFIC_ERROR}` MUST be a textual short name for the error

For example: `"errorCode": "HPE_GL_IAM_EXPIRED_TOKEN"`, `"errorCode": "HPE_GL_DATA_SERVICES_EXPIRED_LINK"`

#### Documenting Error Codes

API Reference Documentation MUST include its possible error codes along with potential remediation steps.

#### Basic Example without Error Details

A hypothetical example may be that `httpStatusCode` may simply be a `401` (`Unauthorized`), but the `errorCode` may indicate that in fact the token sent was expired.

```json
{
    "httpStatusCode": 401,
    "errorCode": "HPE_GL_IAM_EXPIRED_TOKEN",
    "message": "Authentication error - the token was expired.",
    "debugId": "12312-123123-123123-1231212"
}
```

#### Common Error Codes

A specific error code SHOULD be supplied. If is not, then there are some common mappings from HTTP error codes to generic error codes.

:::info Intended for Limited Use
APIs SHOULD NOT use generic error codes as shown below. Whenever possible, the API SHOULD provide a an API specific error code that can better enable automation (as shown above).
:::

| Http Code                         | Error Code                            |
| --------------------------------- | ------------------------------------- |
| `400 Bad Request`                 | `HPE_GL_ERROR_BAD_REQUEST`            |
| `401 Unauthorized`                | `HPE_GL_ERROR_UNAUTHORIZED`           |
| `403 Forbidden`                   | `HPE_GL_ERROR_FORBIDDEN`             |
| `404 Not Found`                   | `HPE_GL_ERROR_NOT_FOUND`              |
| `405 Method Not Allowed`          | `HPE_GL_ERROR_METHOD_NOT_ALLOWED`     |
| `406 Not Acceptable`              | `HPE_GL_ERROR_NOT_ACCEPTABLE`         |
| `409 Conflict`                    | `HPE_GL_ERROR_CONFLICT`               |
| `412 Precondition Failed`         | `HPE_GL_ERROR_PRECONDITION_FAILED`    |
| `415 Unsupported Media Type`      | `HPE_GL_ERROR_UNSUPPORTED_MEDIA_TYPE` |
| `429 Too Many Requests`           | `HPE_GL_ERROR_TOO_MANY_REQUESTS`      |
| `500 Internal Server Error`       | `HPE_GL_ERROR_INTERNAL_SERVER_ERROR`  |
| `503 Service Unavailable`         | `HPE_GL_ERROR_SERVICE_UNAVAILABLE`    |

`HPE_GL_ERROR_METHOD_NOT_ALLOWED` is intended for when a requested path is matched by a router, but the requested method is not. This should never appear in an OpenAPI specification, but may be produced by a service when handling this error scenario.

### Error Details

This is a special kind of error data. It provides extensible error information that is machine and human readable. It may be added to over time.

Error details is an array of objects and each object has a declared type. The type will define the payload.

#### Error Details - General Metadata

| Property            |JSON Type | Description                                                                         | Required? |
|---------------------|----------|-------------------------------------------------------------------------------------|-----------|
| `type`              | string   | The type of error details. General examples below (`hpe.greenlake.bad_request`) or API specific errors (`hpe.greenlake.<api-group-here>.<api-specific-error>`).  | Yes       |
| `source`            | string   | The source of the error. Typically a [registered API group](api-groups/api-grp-registry.md). | Yes       |
| `metadata`          | object   | Any additional key value pairs that the service defines.                            | Yes       |

**Example:**

```json
{
    "httpStatusCode": 401,
    "errorCode": "HPE_GL_IAM_EXPIRED_TOKEN",
    "message": "Authentication error - token expired",
    "debugId": "12312-123123-123123-1231212",
    "errorDetails": [{
        "type": "hpe.greenlake.metadata",
        "source": "hpe.greenlake.iam",
        "metadata": {
            "access_token": "expired"
        }
    }]
}
```

#### Error Details - Bad Request

Additional data on why the request is bad.

| Property            |JSON Type | Description                                                          | Required? |
|---------------------|----------|----------------------------------------------------------------------|-----------|
| `type`              | string   | The type of error details.  `hpe.greenlake.bad_request`              | Yes       |
| `issues`            | array    | Array of bad request issues.                                         | Yes       |

Issues Array

| Property            |JSON Type | Description                                                                                                          | Required? |
|---------------------|----------|----------------------------------------------------------------------------------------------------------------------|-----------|
| `source`            | string   | The part of the request with an issue. See table below.                                                              | Yes       |
| `subject`           | string   | The specific issue key.                                                                                              | Yes       |
| `description`       | string   | An elaborate description of issue. This can be used by developers to understand how the failure can be addressed.    | No        |

Issue Sources

| Source            | Description                                                                                                       |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
| `field`           | The path and name of the field with an issue. Use dot.separated notation if needed (e.g. user.name).              |
| `header`          | Name of the header with an issue.                                                                                 |
| `query.parameter` | A query parameter that is missing or has an invalid value.                                                        |

**Example:**

```json
{
    "httpStatusCode": 400,
    "errorCode": "HPE_GL_ERROR_BAD_REQUEST",
    "message": "BAD REQUEST",
    "debugId": "12312-123123-123123-1231212",
    "errorDetails": [{
    "type": "hpe.greenlake.bad_request",
        "issues": [{
                "source": "field",
                "subject": "user.phone",
                "description": "Invalid format."
            },
            {
                "source": "query.parameter",
                "subject": "limit",
                "description": "Must be a numeric value which is 0 or greater."
            }
        ]
    }]
}
```

#### Error Details - Precondition Not Met

Additional data on why the request is bad.

| Property            |JSON Type | Description                                                          | Required? |
|---------------------|----------|----------------------------------------------------------------------|-----------|
| `type`              | string   | The type of error details.  `hpe.greenlake.precondition_failed`             | Yes       |
| `issues`            | array    | Array of precondition issues.                                        | Yes       |

Issues Array

| Property            |JSON Type | Description                                                          | Required? |
|---------------------|----------|----------------------------------------------------------------------|-----------|
| `source`            | string   | A service specific type (hpe.greenlake.organizations.verification)   | Yes       |
| `subject`           | string   | The specific issue. E.g. DNS entry.                                  | Yes       |
| `description`       | string   | An elaborate description of how the precondition failed. This can be used by developers to understand how the failure can be fixed.                                                                           | No        |

**Example:**

```json
{
    "httpStatusCode": 412,
    "errorCode": "HPE_GL_ORGANIZATIONS_DOMAIN_UNVERIFIED",
    "message": "Precondition Failed",
    "debugId": "12312-123123-123123-1231212",
    "errorDetails": [
        {
            "type": "hpe.greenlake.precondition_failed",
            "issues" : [
                {
                    "source": "hpe.greenlake.organizations.verification",
                    "subject" : "TXT Record with key _hpe-greenlake-validation",
                    "description" : "The TXT record with the domain ownership verification did not exist."
                }
            ]
       }
    ]
}
```

#### Error Details - Retry Data

Informs the caller information on how to retry a request. This may be due to rate limits or due to service load / availability.

| Property            |JSON Type | Description                                                          | Required? |
|---------------------|----------|----------------------------------------------------------------------|-----------|
| `type`              | string   | The type of error details.  `hpe.greenlake.retry_info`               | Yes       |
| `retryAfterSeconds` | integer  | Seconds to wait before retrying.                                     | Yes       |

**Example:**

```json
{
    "httpStatusCode": 500,
    "errorCode": "HPE_GL_ERROR_INTERNAL_SERVER_ERROR",
    "message": "Current request can not be processed due to unknown issue.",
    "debugId": "12312-123123-123123-1231212",
    "errorDetails": [
        {
            "type": "hpe.greenlake.retry_info",
            "retryAfterSeconds" : 30
        }
    ]
}
```

## Error Propagation

When a service has dependencies on other services, it should not directly propagate errors. It should translate errors
using the following concepts:

* Hide implementation details - these may change and your API may change its dependencies in the future
* Hide private information which may expose user personally identifiable information
* Hide information which may present a security risk if intercepted. For example, an error message like "Client IP address is not on allowlist 18.0.0.0/24" exposes information about the server-side policy and can be used to adjust an attack
* Properly map errors. For example, if a user's input to the original API is valid input, but the API makes a call to another service which receives a `BAD REQUEST` response, do not return `BAD REQUEST` to the caller. In this case `INTERNAL ERROR` may be an appropriate response because it will not cause the caller to think they have created a bad request.
