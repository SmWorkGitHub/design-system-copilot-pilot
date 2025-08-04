---
seo:
  title: HPE GreenLake Developer Policy for GraphQL API Design | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for GraphQL API Design

The HPE GreenLake [GraphQL API design](../ratified/api/graphql-spec.md) standard describes how GraphQL interfaces are to be designed and implemented. The goal is to provide consistent, uniform guidelines for GraphQL APIs spanning multiple HPE services created and owned by different development groups.

A GraphQL API endpoint MUST conform to this standard if it meets any of the following criteria:

* The API is branded as an HPE GreenLake API
* The API and its documentation are visible to parties outside of HPE, such as customers or partners
* The API is provided by the HPE GreenLake platform for HPE GreenLake services or other HPE services
* The API is provided by an HPE GreenLake platform core service and is used by other core services
* Any HPE GreenLake service that provides the API for the HPE GreenLake platform or another HPE GreenLake service developed by different HPE organizations

Any other GraphQL APIs developed by an HPE organization within an HPE GreenLake service that do not meet these criteria SHOULD conform to the HPE GreenLake GraphQL standard specification. If the API meets the criteria but does not conform to this standard for business reasons, the standard exemption process must be followed.

This standard applies in addition to and does not replace, any other relevant policies applicable to HPE developers.

The HPE GraphQL standard covers the following aspects of the official [GraphQL specification](https://spec.graphql.org/October2021/):

* Schema syntax validation
* Schema versioning
* Schema directives for parameter constraints, etc.
* Schema constructs such as fragments, unions, and interfaces
* Pagination
* Sorting
* Filtering and searching
* Query depth limits
* Request rate-throttling and performance
* Security
* Error handling
* Composite supergraph formation

A future version of this policy will specify tooling for generating, validating (linting), and testing the GraphQL schemas. This tooling must be integrated into the CI/CD toolchain.

## Why Does This Policy Matter?

HPE GreenLake benefits from this policy for the following reasons:

* GraphQL is designed to provide a consistent and understandable interface for clients to interact with APIs. It achieves this through its well-defined query, mutation, and subscription syntax, semantics, and behavior.
* Easy formation of a composite supergraph from individual GraphQL schemas.
* Provides guidelines, methods, and practices for developers to use GraphQL features such as error handling, versioning, pagination, filtering, and constraint directives.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

Methods to help developers comply with this policy vary across HPE. We hope to provide additional information in future versions of this policy.

### For All HPE Developers

There is no information about official tools or practices to help comply with this policy.

### Developers in the Aruba Business Unit

The Aruba business unit has implemented GraphQL interfaces for device and service configuration (Liberty) and telemetry (Gravity). Aruba has used Apollo Federation for creating a supergraph.

### Developers in the Compute Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the GreenLake Cloud Services Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Hybrid Cloud and Office of the CTO Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Private Cloud Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Storage Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

## What HPE Policies or Standards Does This Policy Refer To?

This policy refers to the following HPE policies or standards:

* [REST API Design](api.md)
* [Aruba GraphQL guidelines](https://confluence.arubanetworks.com/display/CNX/GraphQL+Guidelines) (VPN and access permission required)

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

There are none currently.

**Original publication date (yyyy-mm-dd):** 2024-07-30
