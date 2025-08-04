---
seo:
  title: HPE GreenLake Developer Events Specification | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Developer Events Specification

This event standard describes the format, protocol, and
handling of asynchronous events in HPE GreenLake.

At its core, HPE GreenLake is a large distributed system composed of
many services, processes, and data sources. In typical client-server
applications, a request/response paradigm for APIs is employed using
synchronous communications where the client waits for a response from
the server. This works well for many uses cases and situations where an
action or request for information can be completed in a matter of
milliseconds. However, synchronous architectures can be hard to scale, run the
risk of performance bottlenecks, and high risk of failure due to
long-running request/response sessions.

Asynchronous architectures can provide better perceived performance,
better error handling, better scaling, and better flexibility. Why?
Because depending on the implementation:

- Clients don't have to wait for full completion to continue

- I/O connections don't have to be held open

- Actions can be queued for completion when resources allow

- Services can perform independent error handling and retries

- Additional logic and workflows can be added without changing the base
  logic

Asynchronous architectures do have tradeoffs. They can be more
challenging to comprehend and don't fit all scenarios. However, the
right mix of synchronous and asynchronous workflows enable all HPE
GreenLake consumers and providers to build a larger ecosystem which is
more fault tolerant and performant than can be achieved with just
synchronous paradigms.

The event standard focuses on the event payload, independent of the
protocols or data formats.  
No matter the delivery mechanism, whether it's webhooks, WebSockets,
etc., the payload structure must adhere to the standardized format
defined in this document.

Throughout the standard, we use JSON in our examples for illustration
purposes. However, for specific formats, please refer to
[CloudEvents formats](https://github.com/cloudevents/spec/tree/v1.0.2/cloudevents/formats).
If you need to bind the event to a specific protocol, you can follow the
approach defined in [CloudEvents protocol bindings](https://github.com/cloudevents/spec/tree/v1.0.2/cloudevents/bindings).

## Events

Events are notifications that something has happened within a system.
They are typically used in an event-driven architecture to signal state
changes. They are immutable and their primary purpose is to broadcast
information about a change in state so that other parts of the system
can react accordingly.

The lack of a common way of describing events means developers must
constantly relearn how to consume events. This also limits the
potential for libraries, tooling, and infrastructure to aid the delivery
of event data across environments, such as SDKs, event routers, or tracing
systems. The portability and productivity we can achieve from event data
is hindered overall.

## CloudEvents

[CloudEvents](https://github.com/cloudevents/spec/blob/v1.0.2/cloudevents/primer.md)
is a specification for describing event data in common
formats to provide interoperability across services, platforms, and
systems, making it easier to develop observable, loosely coupled
systems.

The adoption of CloudEvents by major cloud providers and its integration into
numerous open-source projects underscores its importance in the industry.
Reflecting its robust development and community support, CloudEvents
graduated from the Cloud Native Computing Foundation (CNCF) in January
2024.

While providing a structured event format, CloudEvents also allows for
custom extensions. Service can include additional metadata as needed
without breaking compatibility.

The CloudEvents standard adopted is [CloudEvents
v1.0.2](https://github.com/cloudevents/spec/blob/v1.0.2/cloudevents/spec.md).

### Event Properties

The CloudEvents standard specifies common event
properties and how to bind these events to different transport
protocols, including HTTP, Kafka, NATs, and HTTP webhooks. These
properties are described in the following table.

#### Attributes

| **Property** | **Type** | **Description** | **Required** | **Example** | **CloudEvents Extension** |
|----|----|----|----|----|----|
| specversion | string | The version of the CloudEvents spec. MUST be "1.0". | Yes | "1.0" | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| id | string | The identifier for the event. `source`+`id` must be unique and specified in UUID v4 format. | Yes | "e8fc75c1-9ad5-432d-a858-499d9f279647" | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| source | string | The URI that identifies the source and context which the event occurred. `//<region>.api.greenlake.hpe.com/<api-group>` | Yes | "//us-west.api.greenlake.hpe.com/compute-ops", “//global.api.greenlake.hpe.com/audit-log“ | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| type | string | The type of event. Should be in reverse-DNS notation. The value is in the following form: `com.hpe.greenlake.<api-group>.<version>.<resource-collection>.\|subresouce-group\|<event>` | Yes | "com.hpe.greenlake.compute-ops-mgmt.v1.jobs.created" | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| time | string | The timestamp of when the event occurred in [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) format. | No | "2019-10-12T07:20:50.52Z" | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| datacontenttype | string | The encoding of the data in the `data` property as an [RFC 2046](https://datatracker.ietf.org/doc/html/rfc2046) type. | No | "application/json" | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| dataschema | string | The schema of the `data` property. | No | "https&colon;//schemas.example.com/userCreated.json" | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| data | object | The event payload. The contents are marshaled according to the media type in `datacontenttype`. | No | {“key“:”value”} | [core](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md) |
| traceparent | string | The [W3C Trace Context](https://w3c.github.io/trace-context/) contains a globally unique identifier for the trace. | No | "00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-00" | [distributed-tracing](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/distributed-tracing.md) |
| tracestate | string | The [W3C Trace Context](https://w3c.github.io/trace-context/) provides additional context about the trace as an event passes through various services. | No | "service1=abc123, nats=xyz456, service2=def789" | [distributed-tracing](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/distributed-tracing.md) |
| recordedtime | string | A timestamp indicating when the occurrence was recorded in this CloudEvent in [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) format, i.e., when the CloudEvent was created by a producer. | No | "2019-10-12T07:20:50.52Z" | [recordedtime](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/recordedtime.md) |
| sequence | string | This event's order in the stream of events. | No | “00001“ | [sequence](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/sequence.md) |
| deprecated | boolean | Indicates whether the resource is deprecated. | No | true | [deprecation](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/deprecation.md) |
| deprecationfrom | string | The date and time in [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) format when the resource was officially marked as deprecated. | No | "2024-10-11T00:00:00Z" | [deprecation](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/deprecation.md) |
| deprecationsunset | string | The future date and time in [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) format when the resource will become unsupported. | No | "2024-11-12T00:00:00Z” | [deprecation](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/deprecation.md) |
| deprecationmigration | string | The URI SHOULD point to a valid and accessible resource that helps consumers understand what SHOULD replace the deprecated event. | No | "https&colon;//example.com/migrate-to-new-evt" | [deprecation](https://github.com/cloudevents/spec/blob/main/cloudevents/extensions/deprecation.md) |

#### Required Attributes

- **specversion**: The version of the CloudEvents
  specification that the event adheres to.

- **id**:  The identifier of the event. Use a UUID conforming to
  [RFC 4122](https://datatracker.ietf.org/doc/html/rfc4122) whenever possible.
  Other formats may be used, but all ids for a
  given source must be in a common format.

- **source**: The context of the event represented in
  reverse DNS notation. It is prefixed
  with `//<region>.api.greenlake.hpe.com` and identifies the region and
  api group that produced the event.  
  *The `source`+`id` must be unique for each event.*

- **type**: The type of the event related to the `source`. The
  value is in reverse DNS notation prefixed with `com.hpe.greenlake`, the
  api group, and the event version
  (`com.hpe.greenlake.<api-group>.<version>.<resource-collection>.<event>`).
  The event may be one of three kinds:

  Resource created, updated, or deleted events, indicated by the
    resource type and operation:

  - `com.hpe.greenlake.data-services.v1.volumes.created` &ndash; type: `volumes`,
    event: `created`

  - `com.hpe.greenlake.data-services.v1.volumes.snapshots.updated` &ndash; type: `volumes.snapshot` (subresource), event: `updated`

  - `com.hpe.greenlake.iam.v2.users.deleted` &ndash; type: `users`,
    event: `deleted`

  Resource custom event, indicated by the resource type and operation:

  - `com.hpe.greenlake.compute-ops.v1.servers.rebooted` &ndash; type: `servers`,
    event: `rebooted`

  Non-resource event, indicated by the event name:

  - `com.hpe.greenlake.data-services.v1.service-offline` &ndash; service
    status

#### Optional Attributes

- **time**: The time stamp at which the event occurred in [RFC
  3339](https://datatracker.ietf.org/doc/html/rfc3339) format. For
  resource create, update, or delete events, this must be the same as
  the `updatedAt` property of the event.

- **datacontenttype**: The encoding of the `data` property in [RFC
  2046](https://datatracker.ietf.org/doc/html/rfc2046) format. The
  property may be omitted when the cloud event headers are marshaled
  using same format as the `data` property.

- **dataschema**: A URI pointing to a schema that defines the
  structure of the `data` payload.

- **subject**: This attribute is not used.

- **data**: The payload of the event if it has one. The value
  of the `data` property is marshaled in the format specified by
  the `datacontenttype` property. The data structure (or schema) of the
  data marshaled in the `data` property is defined in
  the `dataschema` property.

#### Extensions

CloudEvents extensions provide a flexible way to include additional
metadata in events beyond the standard required fields. These extensions
allow for customization to meet specific use cases and can adapt to
various requirements while maintaining a consistent and interoperable
event format.

##### Distributed Tracing

The traceability extension in CloudEvents provides a way to track the
journey of an event as it passes through different systems and services.
This extension helps in debugging and monitoring by allowing systems to
attach trace information to events.

- **traceparent**: This field is used to track the event as it moves
  through different systems, from the initial producer, through other
  services, until it reaches the customer’s destination. It follows the
  [W3C Trace Context](https://w3c.github.io/trace-context/)
  specification and has the following format:
  `<version>-<trace-id>-<parent-id>-<trace-flags>`.

  Example: `00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-00`

  - version: `00` &ndash; 2-character hex representing the version.

  - trace-id: `4bf92f3577b34da6a3ce929d0e0e4736` &ndash; 32-character hex
    string representing the globally unique trace identifier.

  - parent-id: `00f067aa0ba902b7` &ndash; 16-character hex string
    representing the identifier of the immediate parent span.

  - trace-flags: `00`  &ndash; 2-character hex representing trace options,
    such as sampling.

- **tracestate**: This field carries additional tracing information
  specific to vendors and services involved in the event’s lifecycle.

  Example: `service1=abc123, nats=xyz456, service2=def789`

  - `service1=abc123`: `service1` is the name of the first microservice
    producing the event and `abc123` is the trace information or ID
    specific to that service.

  - `nats=xyz456`: `nats` represents the NATS messaging system and
    `xyz456` is the trace information or ID for the event as it passes
    through NATS.

  - `service2=def789`: `service2` is the name of another microservice
    handling the event after NATS and `def789` is the trace
    information or ID for that service.

##### Recorded Time

The recorded time extension in CloudEvents captures the precise
timestamp at which an event was recorded by the system. This allows for
accurate tracking and ordering of events based on when they were logged.

- **recordedtime**: The exact timestamp when an event was
  recorded. The recorded time is represented in [RFC
  3339](https://datatracker.ietf.org/doc/html/rfc3339) format.

  Example: `2019-10-12T07:20:50.52Z`

##### Sequence

The sequence extension in CloudEvents allows for tracking the order of
events in a stream. This extension is useful for ensuring events are
processed in the correct sequence.

- **sequence**: Track the order of events within a stream or series,
  which is important in distributed systems where events might arrive
  out of sequence.

##### Deprecation

The deprecation extension in CloudEvents defines attributes that can be
included in CloudEvents to indicate to consumers or intermediaries the deprecation of events. These
attributes inform CloudEvents consumers about upcoming changes or
removals, facilitating smoother transitions and proactive adjustments.

See the table under [Attributes](#attributes) for descriptions of the following deprecation attributes:

- **deprecated**

- **deprecationfrom**

- **deprecationsunset**

- **deprecationmigration**

### Event Data

There are three kinds of HPE GreenLake cloud events:

- Resource(s) create, update and delete events, indicated by the resource type and operation:

  - `com.hpe.greenlake.data-services.v1.volumes.created` &ndash; type: `volumes`,
    event: `created`

  - `com.hpe.greenlake.data-services.v1.volumes.snapshots.updated`
    &ndash; type: `volumes.snapshots`, event: `updated`

  - `com.hpe.greenlake.workspaces.v1beta1.tenants.deleted` &ndash; type:
    `tenants`, event: `deleted`

- Resource custom events, indicated by the resource type and operation:

  - `com.hpe.greenlake.compute-ops.v1.servers.rebooted` &ndash; type: `servers`,
    event: `rebooted`

- Non-resource events, indicated by the event name:

  - `com.hpe.greenlake.data-services.v1.service-offline` &ndash; service
    status

#### Resource(s) Create, Update, and Delete Events

These events represent state changes to individual resources. The data
is the new state of a resource as it would be returned in a GET request
to the REST API and has the same format. A full representation of the
resource is included in each event.

The resource create event is published when a resource is created and
has:

- `type`: A string in the following format: `com.hpe.greenlake.<api-group>.<version>.<resource-collection>.created`

- `time`: Same as the `createdAt` and `updatedAt` property of the resource.

- `data`: Full representation of the created resource.

The resource update event is published when a resource is updated and
has:

- `type`: A string in the following format: `com.hpe.greenlake.<api-group>.<version>.<resource-collection>.updated`

- `time`: Same as the `updatedAt` property of the resource.

- `data`: Full representation of the updated resource.

The resource delete event is published when a resource is deleted and
has:

- `type`:  A string in the following format: `com.hpe.greenlake.<api-group>.<version>.<resource-collection>.deleted`

- `time`: The time the resource was deleted.

- `data`: A partial representation of the deleted resource, containing the
  common properties defined in the [API
  standard](naming.md/#common-properties)
  with `type` and `id` being required.

#### Resource Custom Events

These events represent a transition related to an individual resource
that is not a state change. An example might be the completion of a
system reboot.

The resource custom event has:

- `type`:  A string in the following format: `com.hpe.greenlake.<api-group>.<version>.<resource-collection>.<event>` (past
  tense event verb).

- `time`: The time the custom event occurred (e.g., the time a server was
  rebooted).

- `data`: Custom data.

#### Non-Resource Event

These events represent a transition within the system that are not
related to an individual resource.

The non-resource event has:

- `type`: `com.hpe.greenlake.<api-group>.<version>.<event>` (past
  tense event verb)

- `time`: The time the custom event occurred.

- `data`: Custom data.

### GLP CLoudEvent Example

The following is a full example of a CloudEvent from HPE GreenLake platform.

#### User

```json
{
  "specversion": "1.0",
  "id": "c4bc75c1-9ad5-432d-a858-499d9f279746",
  "source": "//global.api.greenlake.hpe.com/identity",
  "type": "com.hpe.greenlake.identity.v1.users.created",
  "time": "2024-09-04T01:30:20.52Z",
  "datacontenttype": "application/json",
  "data": {
    "id": "a63bdb62-37d5-4b4d-8758-9363f17f343e",
    "type": "user",
    "generation": 1,
    "createdAt": "2023-10-11T18:16:10.459463",
    "updatedAt": "2024-09-10T18:23:21.949007",
    "resourceUri": "global.api.qa.hpedevops.net/identity/v1/users/a63bdb62-37d5-4b4d-8758-9363f17f343e",
    "username": "user@hpe.com",
    "userStatus": "VERIFIED",
    "lastLogin": "2024-09-10T18:23:21.949005"
  }
}
```

#### Audit Log

```json
{
  "specversion": "1.0",
  "id": "c4bc75c1-9ad5-432d-a858-499d9f279746",
  "source": "//global.api.greenlake.hpe.com/audit-log",
  "type": "com.hpe.greenlake.audit-log.v1beta1.logs.created",
  "time": "2024-09-04T01:30:20.52Z",
  "datacontenttype": "application/json",
  "data": {
    "id": "Pa-punEBGT8LtJNjb9cu",
    "type": "/audit-log/logs",
    "user": {
      "username": "user@hpe.com"
    },
    "workspace": {
      "id": "e6e7e552184511ef88e9cacb36fa0723",
      "workspaceName": "Test Workspace"
    },
    "category": "Unified Eventing",
    "application": {
      "id": "00000000-0000-0000-0000-000000000000",
      "applicationName": "HPE GreenLake Platform"
    },
    "description": "Creation of Subscription: 4c9c6a57077 by User: user@hpe.com is successful.",
    "generation": 1,
    "createdAt": "2024-09-04T01:30:20.000000Z",
    "updatedAt": "2024-09-04T01:30:20.000000Z",
    "additionalInfo": {
      "requestId": "1dd8e958-7ea0-44e8-bae7-6be05861a8d9"
    },
    "hasDetails": true
  }
}
```

## Versioning

Versioning is an important aspect in the development and maintenance of
event-driven architectures. It ensures that systems remain adaptable
over time, facilitating updates without disrupting existing
functionalities. It allows services to introduce new features, changes,
or deprecations without breaking compatibility with older versions of
the software.

Each event type must include a version string to clearly indicate the
version of the event being published. This version string should align
with the versioning strategy used in your REST API or other relevant
services.

Examples:

- `com.hpe.greenlake.data-services.`**v1.**`volumes.created`

- `com.hpe.greenlake.workspaces.`**v1beta1**`.tenants.deleted`

- `com.hpe.greenlake.devices.`**v2alpha2**`.devices.updated`

For more detailed information on these guidelines, refer to [HPE GreenLake API Versioning](versioning.md).

### Deprecation Policy and Sunsetting

During the deprecation period, the event is still supported but is no
longer recommended for use. Consumers are encouraged to migrate to the
newer version.

After deprecation, in the sunset phase, the event version is no longer
supported and may be completely removed from the system.

In aligning with our REST API deprecation strategy, our events will
also implement similar versioning attributes to ensure clear
communication regarding the lifecycle of each event. Specifically, we
will use the [CloudEvents deprecation extension](#deprecation)
to mirror the ‘Deprecation’ and ‘Sunset’ headers used in our REST APIs.

**Original publication date (yyyy-mm-dd):** 2024-11-07
