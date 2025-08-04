---
seo:
  title: Logging Standard | HPE GreenLake Cloud Platform
toc:
  enable: true
---

# Logging

## Introduction

Logs are used to communicate the occurrence of a significant event. Each event has metadata
associated with it, including when it occurred, who triggered it, the operation performed and what
the operation was performed upon.

Recording these events enables others to understand what happened and when. This rarely includes why
the event occurred, but whether it should have occurred can be determined and is leveraged in
compliance and debugging efforts. As these are critical to both those using and supporting a system,
producing accurate, effective, and useful logs is a non-negotiable requirement.

## Audience

This document applies only to the writing of log-related standards. For standards that apply to the
production of logs, see the [Implementations] section.

[Implementations]: #implementations

## Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT",
"RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in
[RFC 2119].

| Term   | Description                                                  |
| ------ | ------------------------------------------------------------ |
| Event  | An occurrence that carries significance or meaning.          |
| Target | The objects or resources affected by the event.              |
| Record | Information logged about an event.                           |
| Field  | A key-value pair that appears in a Record.                   |
| Tenant | A logically isolated instance within a larger shared system. |

[RFC 2119]: https://datatracker.ietf.org/doc/html/rfc2119

## Records

Records are structured as key-value pairs and MUST be formatted as JSON unless otherwise specified.
The following values MUST be present in all records:

- A [RFC 3339 date-time] with at least millisecond precision describing when an event occurred. This
  MUST use UTC as the timezone with the time-offset set to "Z".
- An immutable identifier and type that identifies the event's target(s). The identifier MUST be
  unique for the target type.
- An immutable, unique identifier for the entity that triggered the event, unless the event was
  triggered by an internal workflow.
- An immutable, unique identifier for the current tenant when multi-tenancy is supported.
- The operation the event was triggered for.
- The result of the operation. If the operation failed, the reason for the failure MUST also be
  included.

These core requirements are expected to be extended and supplemented by implementations of the
record defined here. This includes definition of the keys for the required values listed above.

Implementations MUST NOT relax any of the core requirements.

[RFC 3339 date-time]: https://datatracker.ietf.org/doc/html/rfc3339#section-5.6

## Implementations

Implementations are intended to support specific logging use cases by extending the requirements
defined above. Where decisions were deferred, implementations are required to define the behaviour
that makes sense in the targeted domain. Implementations can support additional values as needed,
assuming they do not conflict with or replace the required values.

### Audit Logging

See the Audit Logging [policy](../../policies/audit_logging_policy.md) and [standard](audit_logging_guidelines.md).

### Cloud Service Debug Logging

See [Cloud Service Debug Logging](./services.md).

**Original publication date (yyyy-mm-dd):** 2024-05-24
