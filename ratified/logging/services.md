---
seo:
  title: Cloud Service Debug Logging Standard | HPE GreenLake Cloud Platform
toc:
  enable: true
---

# Cloud Service Debug Logging

## Introduction

Logging in Cloud Services is used to allow software engineers to debug and maintain the system as
well as help support engineers respond to customer escalations. These services are typically
structured as independent microservices with multiple replicas. To avoid those reading these logs
needing to account for idiosyncrasies between each service, it is desirable to have highly
consistent logs to support easy filtering and grouping of records.

The log records discussed below are intended to support the operation and maintenance of a service
only. They are not a substitute for audit logs created for compliance activities.

## Infrastructure

Log records from cloud services MUST be sent to a log aggregation system. This system should be the
primary view for log records which enables queries spanning multiple services to be used during an
investigation. Centralising log records is intended to simplify management, including retention
policies, erasure and access controls.

The mechanism used to send log records to the log aggregation system MUST use encryption in transit,
e.g. HTTPS. Once ingested, log records MUST be encrypted at rest.

### Record Size

Log aggregation systems may split a single log record into multiple if it exceeds a given size.
Furthermore, tooling that support log aggregation may supplement the log record with additional data
about the environment. For example, Fluent Bit can append data about the Kubernetes pod the service
is running within.

In recognition of these constraints, an individual log record output by a service SHOULD NOT exceed
8KiB for error [level] logs. Log records at other levels SHOULD NOT exceed 1KiB to reduce storage
costs.

[level]: #levels

### Retention

As discussed in [Confidentiality], some log records contain personal data. While this should be
avoided where possible, there are exceptions that affect the maximum retention period of a log
record.

To comply with HPE's Privacy Policy and Statement, log records containing personal data MUST NOT be
retained longer than it can be legitimately used. Furthermore, all personal data related to a
customer MUST be deleted following the offboarding of said customer. This deletion applies to log
records kept in a log aggregation system and any local copies collected for investigations.

Log aggregation systems MUST delete all log records 90 days after they were created. Copies of log
records that are used for investigations and contain personal data MUST also be deleted within 90
days, and SHOULD be deleted as soon as the investigation has concluded. Copies or archives likely to
be retained beyond this time MUST remove or anonymize all personal data prior to being stored.

[Confidentiality]: #confidentiality

## Fields

Field keys MUST be formatted in "snake_case", as opposed to "camelCase", "kebab-case", etc. This is
to ensure values in records can be easily grouped together, and for compatibility with Python's
kwargs feature.

The combined size of the fields in each record SHOULD NOT exceed 8KiB. This leaves room for tools
like Fluent Bit to add supplemental data about the environment the service is within.

Below is a formatted example of all fields discussed in following subsections. Production records
are expected to omit superfluous whitespace and may output keys in any order.

```json
{
  "timestamp": "2023-11-28T01:23:45.678Z",
  "source": "github.com/example/example/internal/example/file.go:43",
  "level": "info",
  "msg": "something significant happened",
  "customer_id": "7de078188fa511eeb9d10242ac120002",
  "user_id": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
  "trace_id": "0123456789abcdef0123456789abcdef",
  "rest_method": "GET",
  "rest_path": "/data-services/v1/virtual-machines/fa72854b-61f8-406b-b439-38f929ea4e55",
  "virtual_machine_id": "fa72854b-61f8-406b-b439-38f929ea4e55"
}
```

### Core Fields

The following fields MUST be present in all records unless exempt:

| Key         | Required       | Description |
| ----------- | -------------- | ----------- |
| timestamp   | Yes            | A [RFC 3339 date-time] with at least millisecond precision describing when an event occurred. This MUST use UTC as the time zone with the time-offset set to "Z". |
| source      | Yes            | The file and line number the record was emitted from. |
| level       | Yes            | The level for the record. See [Levels] for the acceptable values. |
| msg         | Yes            | A human readable message summarising the event. This SHOULD NOT be longer than 50 characters and SHOULD be static per the [Style Guide]. |
| customer_id | Yes            | Either the platform or application customer ID, depending on which is used to enforce multi-tenancy. |
| user_id     | No<sup>1</sup> | A SHA-256 hexadecimal digest of the email address of the user that triggered the event. |
| trace_id    | No<sup>2</sup> | The identifier for the current distributed trace. See the [W3C documentation] for more information. |
| error       | No             | A human-readable summary of an error or exception. |
| stacktrace  | No<sup>3</sup> | The stacktrace of an error or exception. |

1. The `user_id` field may be omitted only if the event was triggered by an internal process.
2. The `trace_id` field may be omitted only if distributed tracing is not in use.
3. As stacktraces can be very large, it is suggested to optionally include them as needed via
   runtime configuration. This ensures log records are within recommended limit specified in
   [Record Size].

[RFC 3339 date-time]: https://datatracker.ietf.org/doc/html/rfc3339#section-5.6
[Levels]: #levels
[Style Guide]: #static-log-messages
[W3C documentation]: https://www.w3.org/TR/trace-context/#trace-id
[Record Size]: #record-size

### Resource Fields

Fields for resources have been standardised to ensure logs for operations upon a resource can be
easily searched and grouped.

Where fields are properties of resources, the key MUST be a combination of the resource name and the
property. If the resource appears in an OpenAPI specification, the type MUST be used. For example,
the `id` and `createdAt` properties of the `virtual-machine` resource would be logged with the
`virtual_machine_id` and `virtual_machine_created_at` keys respectively.

If a resource contains references to other resources in an OpenAPI specification, then properties of
the referenced resource SHOULD omit the referencing resource. For example,
`virtual-machine.group.id`would be logged as `group_id`.

If a property is deeply nested within a resource, the intermediate object name MAY be omitted if it
provides no additional value. For example, `instance.networkInfo.subnet.id` might be logged with a
key of `instance_subnet_id`.

The resource name MAY be omitted from the field key if all the following criteria are met:

- The resource is owned by the service.
- The resource property is not an identifier of the resource.
- The property is unambiguously associated with the resource.

### Protocol Fields

The following fields MUST be present for services handling a REST request:

| Key          | Description                                  |
| ------------ | -------------------------------------------- |
| rest_method  | The HTTP method of the request in uppercase. |
| rest_path    | The URL path of the request.                 |

The following fields MUST be present for services handling a gRPC request. These fields can be
populated by extracting values from the URL path of the request. The path is in the format
`/{package}.{service}/{method}`.

| Key          | Description                      |
| ------------ | -------------------------------- |
| grpc_service | The gRPC service of the request. |
| grpc_method  | The gRPC method of the request.  |

The following fields MUST be present for services sending or receiving a CloudEvent:

| Key          | Description                            |
| ------------ | -------------------------------------- |
| event_id     | The id property of the CloudEvent.     |
| event_source | The source property of the CloudEvent. |
| event_type   | The type property of the CloudEvent.   |

### Confidentiality

Some data is considered confidential and should be protected to prevent disclosure. Such data MUST
NOT be added to log records. Examples of this include, but are not limited to, credentials, secrets,
authentication and personal device identifiers such as [IMEI].

Personal data SHOULD NOT be added to log records unless it directly supports maintenance of a
service and resolution of customer issues. If personal data is used in fields, any values SHOULD be
anonymized via hashing to prevent unnecessary exposure where practical. For example, user emails are
hashed in [Core Fields] for the user_id field.

This anonymization MAY be omitted only if it would inhibit the utility of the information during
investigation of customer issues. IP and MAC addresses may be examples of this, depending on which
device they identify. The only fields that do not require personal data to be anonymized can be
found in the table below.

| Key         | Description                  |
| ----------- | ---------------------------- |
| ip_address  | The IP address of a device.  |
| mac_address | The MAC address of a device. |

Any additions to this table MUST be approved by [Compliance360] and align with the
[Global Privacy Policy] and [Privacy Statement]. These additions MUST also be added to the table
before use in any service. Where there are multiple instances of a value, the keys should be
prefixed to differentiate between them, e.g. `source_ip_address` and `target_ip_address`.

[IMEI]: https://en.wikipedia.org/wiki/International_Mobile_Equipment_Identity
[Core Fields]: #core-fields
[Compliance360]: https://hpe.sharepoint.com/teams/hpe-rcgo/SitePages/Compliance360.aspx
[Global Privacy Policy]: https://www.hpe.com/us/en/privacy/master-policy.html
[Privacy Statement]: https://www.hpe.com/nl/en/privacy/ww-privacy-statement.html

## Levels

Levels indicate the severity of an event. Their use makes records easier to filter, easier to
understand, and easier to manage. The appropriate use of each is described below.

| Level | Description |
| ----- | ----------- |
| error | An internal error occurred within the service that must be investigated. |
| warn  | An internal error could have occurred within the service and further investigation may be required. |
| info  | An interesting or non-internal error event occurred within the service. |
| debug | Information to support investigations into the details of a service's behaviour. |

These levels are ordered, such that error is greater than warn, warn is greater than info, etc.
Additional levels such as trace, or critical MUST NOT be used. Similarly, use of a level must not
have any side-effects. For example, using fatal in Go typically causes a process to exit.

Levels SHOULD be output in lowercase. Services MAY deviate if all services with a system already use
uppercase for levels.

By default, services MUST filter out records at "debug" level. It is therefore expected that debug
records do not contribute towards day-to-day maintenance. These records may be enabled during
time-bound investigations, and filtered out again once the investigation has concluded. Enabling
debug level records MUST affect no more than a single process and its replicas. In Kubernetes terms
this is usually a single Deployment, minus any sidecars.

If debug level records are enabled for third-party packages, this MUST NOT result in secrets or
other confidential information being leaked in production environments. See [Confidentiality] for
more details on what cannot be included in records.

## Libraries

Migrating an existing Cloud Service to this standard is expected to involve a non-trivial amount of
work as logging libraries tend to be foundational in any code written. Therefore, the below
libraries exist as recommendations for new services or for services that do not currently use
structured logging.

| Language      | Library                                           |
| ------------- | ------------------------------------------------- |
| Go (1.21+)    | [`log/slog`]                                      |
| Python (3.8+) | [`structlog`] with the [`JSONRenderer`] processor |

[`log/slog`]: https://pkg.go.dev/log/slog
[`structlog`]: https://www.structlog.org/en/stable/
[`JSONRenderer`]: https://www.structlog.org/en/stable/api.html#structlog.processors.JSONRenderer

## Recommended Practices

The below recommendations are intended to guide services onto known good behaviour and SHOULD be
followed whenever possible and practical.

### Static Log Messages

Dynamic log record messages are created by substituting variable data into a pre-defined format,
e.g. `printf`. However, these unstructured strings are often difficult to parse consistently in log
aggregation systems and can vary between services. Furthermore, they also can cause a small
performance loss from manipulating strings that are later filtered out depending on the current log
level.

To maximise the potential of JSON, any variable data should be moved to a dedicated field. This
allows the field values to be analysed and grouped more easily, identifying service-agnostic
commonality and differences as needed. As a result, the log record message becomes static and easy
to find in the source codebase.

#### Examples

<table>
<thead><tr><th>Library</th><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

[`log/slog`] (Go)

</td><td>

```go
logger.Error(fmt.Sprintf(
  "values were found: %s %d",
  val1,
  val2,
))
```

</td><td>

```go
logger.Error(
  "values were found",
  slog.String("value_1", val1),
  slog.Int("value_2", val2),
)
```

</td></tr>
<tr><td>

[`zerolog`] (Go)

</td><td>

```go
logger.
  Error().
  Msgf("values were found: %s %d", val1, val2)
```

</td><td>

```go
logger.
  Error().
  Str("value_1", val1).
  Int("value_2", val2).
  Msg("values were found")
```

</td></tr>
<tr><td>

[`zap`] (Go)

</td><td>

```go
logger.Error(fmt.Sprintf(
  "values were found: %s %d",
  val1,
  val2,
))
```

</td><td>

```go
logger.Error(
  "values were found",
  zap.String("value_1", val1),
  zap.Int("value_2", val1),
)
```

</td></tr>
<tr><td>

[`logrus`] (Go)

</td><td>

```go
logger.Errorf(
  "values were found: %s %d",
  val1,
  val2,
)
```

</td><td>

```go
logger.
  WithFields(logrus.Fields{
    "value_1", val1,
    "value_2", val2,
  }).
  Error("a value was found")
```

</td></tr>
<tr><td>

[`structlog`] (Python)

</td><td>

```python
logger.error(
  "values were found: %s %d",
  val1,
  val2,
)

logger.error(
  f"values were found: {val1} {val2}",
)
```

</td><td>

```python
logger.error(
  "values were found",
  value_1=val1,
  value_2=val2,
)
```

</td></tr>
</tbody></table>

[`zerolog`]: https://pkg.go.dev/github.com/rs/zerolog
[`zap`]: https://pkg.go.dev/github.com/uber-go/zap
[`logrus`]: https://pkg.go.dev/github.com/sirupsen/logrus

### Record Significant Events

It is desirable to reduce unnecessary noise in log records, allowing those viewing log records to
identify interesting or anomalous behaviour. As a result, log records should focus on significant
events.

In general, a simple read operation that worked flawlessly is not significant and so does not
deserve a log record. Conversely, a successful write operation reflects a change to the service and
so is usually considered significant. However, this may not be true in write-heavy services. A
failure occurring during an operation is always considered significant.

A function starting and ending is never considered a significant event. Similarly, producing a log
record exclusively for capturing the execution time of a function is not significant. If this data
is required, it is recommended to invest in metrics or distributed tracing which can provide
superior views for this information.

### One Record Per Event

It can be tempting to produce log records in multiple places as an event is passed through a
service, particularly if the service is structured as a series of layers. However, excessive logs
can combine to produce confusing or misleading views as one layer's error might be another layer's
success and vice versa.

To maximise the utility of log records, it is recommended to produce a single record at the point
where the result of handling the event is known or decided. This may be within business logic, or a
higher layer if an error occurs prior to business logic being invoked.

This guidance does not apply to debug level log records which are filtered out by default. These are
intended to add additional colour and context to the event handling, and so a more granular set of
views are often useful.

### Use Debug Sparingly

While debug-level log records are filtered out by default, that does not mean they shouldn't be
curated carefully. Enabling the debug level should not cause a massive increase in the volume of
produced log records that would otherwise inhibit those useful to an investigation.

Instead, it is recommended to add debug-level log records as part of an investigation and then
review their utility once the investigation has concluded. If they were found to add no value, it
may be prudent to remove them. If they were extremely useful and the issue is likely to resurface,
consider moving them to the info level so they are not filtered out going forward. By following
this approach, speculative debugging that adds no value can be avoided, allowing investigations to
be concluded more quickly by removing unnecessary noise.

### Complex Objects

It is sometimes useful to add an object to a log record, allowing the fields within the object to be
used for filtering and grouping log records. This can be achieved by converting the object to JSON
which is then used as the value for a field within the log record. This conversion can performed
recursively, although this may be of diminishing value at some depths.

The exact implementation of the conversion varies significantly between libraries. The generalised
input and output are detailed below, with examples for recommended libraries in the next section.
Regardless of implementation, care must be taken to omit any confidential values as defined in
[Confidentiality].

<table>
<thead><tr><th>Input</th><th>Output</th></tr></thead>
<tbody>
<tr><td>

```go
struct Example {
  foo int
  bar string
  baz bool
}
```

</td><td>

```json
{
    "example": {
        "foo": 5,
        "bar": "hello",
        "baz": false
    }
}
```

</td></tr>
</tbody>
</table>

#### Examples

When using Go's [`log/slog`] with [`log/slog.JSONHandler`], objects are automatically converted to
JSON. The exact format of the output can be controlled using the standard `json` struct field tag as
described in the documentation of [`encoding/json.Marshal`].

```go
type Foo struct {
    Bar int    `json:"bar"`
    Baz string `json:"baz"`
}

func NewFoo(bar int, baz string) Foo {
    return Foo{
        Bar: bar,
        Baz: baz,
    }
}

// {"msg":"something happened","foo":{"bar":5,"baz":"example"}}
logger.Info("something happened", slog.Any("foo", NewFoo(5, "example")))
```

For python, objects are logged using their [`__str__`] implementation by default which in turn
defaults to the [`__repr__`] implementation. The default `__repr__` implementation is often
impossible to use for debugging via logs. One option is to implement `__repr__` to produce a
human readable way to reconstruct the object. An alternative for those using [`structlog`] is to
implement the [`__structlog__`] method as demonstrated below using the [`__dict__`] attribute.

```python
class Foo:
    def __init__(self, bar: int, baz: str):
        self.bar = bar
        self.baz = baz

    def __structlog__(self):
        return self.__dict__

# {"msg":"something happened","foo":{"bar":5,"bar":"example"}}
logger.error("something happened", foo=Foo(5, "example"))
```

[`log/slog.JSONHandler`]: https://pkg.go.dev/log/slog#JSONHandler
[`encoding/json.Marshal`]: https://pkg.go.dev/encoding/json#Marshal
[`__str__`]: https://docs.python.org/3/reference/datamodel.html#object.__str__
[`__repr__`]: https://docs.python.org/3/reference/datamodel.html#object.__repr__
[`__structlog__`]: https://www.structlog.org/en/stable/api.html#structlog.processors.JSONRenderer
[`__dict__`]: https://docs.python.org/3/library/stdtypes.html#object.__dict__

### No Log-based Dashboards or Alerts

Dashboards and alerts built upon log records can create unnecessary load upon log aggregation
systems. Instead, it is recommended to leverage data from metrics such as those from Prometheus
which can be used to define alerts and build Grafana dashboards as needed.

**Original publication date (yyyy-mm-dd):** 2024-05-24
