# Tagging Standard

## Background

Tagging is a well-known concept which is widely utilized across the industry. Its main purpose is to allow various policies or presentation filters to be applied across services and resource types. All existing cloud providers support tags, although the way those were introduced has sometimes been painful. The main pain point was lack of unification. Various services/resources, implemented by the same cloud provider, could expose different format, different constraints and different operational semantics, causing inability to create tags-based policies across different services. This standard is meant to introduce uniform tagging to GreenLake Cloud Platform as early as possible, learning from the mistakes of our predecessors.

## Applicability

**This standard is applicable to EVERY service which is integrated with GreenLake Cloud Platform, regardless of whether it's a core platform service or a BU specific service. While a specific resource might not support tagging at all, those that support MUST comply with this specification.**

## Tag Structure

* Tag is a key-value pair of character strings that can be attached to the resource by either a user or a service.
* Users MUST not be able to edit or remove service-originated tag.
* Resources that support tagging MUST support up to 50 user-originated tags per resource. Number of service-originated tags is not restricted.
* Tags MUST be stored by preserving input case sensitivity. However, tag keys and values are considered case insensitive. This means if a given resource has the tag "location" : "San Jose" then same resource MUST NOT have "Location" : "New York" tag as it is considered the same key.
* Search on the tags MUST be case insensitive. For REST API implementation for tags please refer to query parameters specific for tags in [API Standard](../../ratified/api/query_parameters.md#filtering-by-tags).
* For a given resource, there can only be one value for a particular tag key.
* Tag key MUST have 1-128 characters.
* Tag value can have a maximum of 256 characters.
* Allowed characters for both keys and values are letters, numbers, spaces representable in UTF-8, and the following characters: _ . : = + - @
* Null is not an allowed value. Instead, empty string ("") MUST be supported to enable 'label' semantics.
* Tags MUST NOT be used to store sensitive data, such as personally identifiable information.
* Service-originated tags MUST start with a prefix "hpe:". User-defined tags MUST NOT start with the "hpe:" prefix.
  * A [registry](service-namespace-registry.md) of prefix namespaces is managed alongside this standard for services to reserve and use for their corresponding service-originated tags. For example: "hpe:sdwan:", "hpe:backups:", etc.

## Tagging Implementation Responsibilities

* Tags MUST be stored and presented in the context of the specific resource and MUST NOT have an independent lifecycle i.e. tags cannot be created, updated or deleted as standalone entities.
* Services own the APIs for tagging (& untagging) resources and for querying the resources based on tags. Adhere to the [API Standards](../../policies/api.md) while implementing this.
* Events MUST be published by the services whenever any tag is modified on any resource. Refer to [event standards](../../draft/event/index.md) for this.

**Original publication date (yyyy-mm-dd):** 2023-11-16
