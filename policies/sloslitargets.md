---
seo:
  title: HPE GreenLake Development Standard for SLO and SLI Targets | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Standard for SLO and SLI Targets

All HPE GreenLake Platform (GLP) Services [1] that are Consumed [2] by another entity, or otherwise
interact with GLP, MUST define and publish:

* One or more Service Level Objectives (SLOs) that collectively can demonstrate conclusively if a
Service is meeting its customer [3] expected goals. A single SLO must be defined in terms of a specific
quantifiable measure (called a Service Level Indicator, or SLI) along with thresholds that determine
if the SLI is within acceptable boundaries.

* Service-Level Objectives (SLOs) should be clearly defined at the API level to ensure that APIs fulfill the service expectations of consumers.
By establishing SLOs, organizations can effectively demonstrate whether a service is meeting the goals anticipated by customers.

* Associating SLOs directly with API specifications provides clear expectations regarding the performance and reliability of the API.

* Up to date current and historical data for all SLIs available for the Service[1] for no less than 30
days is required to determine if that Service is meeting its defined SLOs.

## Service Level Objectives

SLOs are defined and adjusted by the service team and defined in terms of SLIs. (e.g. "99.5% of APIs should
return in 20ms or less.") SLOs are effectively an "internal-to-the-team" performance goal for each
system.

For services which consume other services, the primary SLO should be constructed based on the SLOs of
its dependencies&mdash;without dependent-service SLOs published and defined, the consuming service cannot
use the dependent service to construct its own SLO.

To ensure that SLOs are widely known and used within an organization, it's important to make them discoverable by centralizing SLO definitions in a catalog or store and using consistent language and models to define SLOs.

## Service Level Indicators

While the exact SLIs to specify are at the discretion of the affected team developing the Service team,
useful SLIs should provide measures of the four [Google SRE Golden Signals](https://sre.google/sre-book/monitoring-distributed-systems/),
specifically:

* **Latency** – Examples of latency measurements MAY be:
  * "/api/create responds within a threshold"
  * "All UI operations MUST individually complete within within a threshold (1000ms)"
  * For asynchronous services the proportion of work-queue tasks completed faster than a threshold.
* **Traffic / Throughput** – Examples of traffic measurements are:
  * "x per unit-time" such as transactions- or requests-per second (TPS or RPS, respectively).
  * Throughput for asynchronous services are defined in terms of the rate at which the messages produced by the consumer are being  processed.
* **Errors** – Examples of errors and error rates are "X per unit-time", similar to traffic, or a percentile of
overall rates ("0.1% of total requests").
* **Saturation** – Examples of saturation are often related to infrastructure or resources used. Saturation
can be represented as percentiles ("percent CPU consumed", "percent disk utilized") or specific units of
comparison to operations (like "5kb of cache memory and 1kb of disk per request").

Availability (of a Service) is defined as the amount of time a service is operating correctly, not excluding
maintenance windows. This is due to the provided service being decoupled from the underlying
resources. In the case where we are vending resources to a consumer, maintenance windows do
apply.

This standard applies to all those designing and operating services, and their supporting SRE engineers.

## Definitions

[1] For the purposes of this document, a Service is defined as a software system that can be modified in
production without explicit coordination with any other software system that depends on it.

[2] For the purposes of this document, an internal Consumer is any other internal Service owned by HPE which depends on
the Service for proper function. When a Service is being used by a Consumer or a Customer[3] it is said to be
Consumed.

[3] For the purposes of this document, a Customer is any entity external to HPE with whom HPE has a
commercial relationship, or intent to create one. This may include end users consuming the platform, or partner
organizations who collaborate with HPE to bring products and services to market.

For example:

* Software components that form part of a globally hosted HPE GreenLake solution, including applications such as
Aruba Central, or shared hosted services such as GLP Common Cloud Services, are considered in scope.

* Software components running on a device that provides HPE GreenLake functionality (such as the HPE GreenLake
Control Plane) are considered in scope.

## Why does this standard matter?

A published set of SLIs and SLOs is essential for determining the health of your service and
facilitating cross-BU communication of reliability expectations.

* SLI – Identify how to track the interaction with the service. All consumers need to know how
to track their instantiation of your service.
* SLO – Identify if the service is reacting in an acceptable fashion. All consumers need to know
how their instantiation of your service is behaving versus the usual norms.
Other services MAY use the SLI/SLO information to assess if they can expect to reliably take a dependency
on the target service.

## What tools and practices can help a developer comply with this standard?

### Developers in the Compute Business Unit

Compute Ops Manager (COM) has Service Level Objectives (SLOs) and SLIs for features and Services that are
defined and will be augmented as additional features are added over time. COM will track SLIs and SLOs via
Grafana dashboards for internal assessment and remediation of service reliability and availability. Currently
these Services are internal-only to the Compute team, and thus do not need to be made available to a broader
audience.

## Requesting Exceptions

To request an exception to this standard, refer to the Exceptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-09-14

**Revised:** 2024-09-11

**What's Changed** 2024-09-11

* Clarified that SLO measurements apply to APIs
* Added additional benefits of SLOs
* Clarified how SLOs should be stored and published
