---
seo:
  title: About the HPE GreenLake Development Standard for APIs | HPE GreenLake Cloud Platform
toc:
  enable: true
redirectFrom:
  - /docs/greenlake/api/internal/api_standards/platform/
  - /docs/greenlake/api/internal/api_standards/platform/versioning/
---

# About the HPE GreenLake Development Standard for APIs

## Overview

The [HPE GreenLake API Standard](../ratified/api/index.md) describes how HPE GreenLake APIs should be designed and implemented.

An API endpoint MUST conform to this standard if it meets any of the following criteria:

* The API is branded as an HPE GreenLake API
* The API and its documentation are visible to parties outside of HPE such as customers or partners
* The API is provided by the HPE GreenLake platform for HPE GreenLake Services [[2](#def-HPE-GL-S)] or for Other HPE
Services [[3](#def-OtherHPEServices)]
* The API is provided by a HPE GreenLake Platform Core Service [[1](#def-HPE-GL-CoreService)] and is used by other Core Services.
* The API is provided by any HPE GreenLake Service [[2](#def-HPE-GL-S)] for the HPE GreenLake platform, or for another HPE
GreenLake Service [[2](#def-HPE-GL-S)] developed by different HPE organizations.

Any other APIs developed by a single HPE organization within an HPE GreenLake Service that do not meet the aforementioned criteria SHOULD conform to the HPE GreenLake API standard specification.

If the API meets the criteria, but for business reasons is proposed to be exempted from conformance with the standard specification, then an exemption proposal should be sent to the Standards Committee to provide the decision.

This standard applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Definitions

For the purposes of this standard, the following terms apply:

* <a name="def-HPE-GL-CoreService"></a> **[1] HPE GreenLake Platform Core Service** - Core services are microservices or bounded contexts that reside within the HPE GreenLake Platform such as frontend, app catalog, device mgmt., audit, orgs, service identity, etc.

* <a name="def-HPE-GL-S"></a> **[2] HPE GreenLake Service** - A service that reside outside of the HPE GreenLake Platform such as Aruba Central, Compute Ops, DSCC, Bisbee, etc.

* <a name="def-OtherHPEServices"></a> **[3] Other HPE Services** - HPE services which are not under the HPE GreenLake platform umbrella such as BRIM, GTS, etc.

## Why does this standard matter?

Adherence to this standard benefits HPE GreenLake by:

* Creating a consistent syntax, resource definition, and versioning strategy across all HPE GreenLake APIs.
* Removing the need for developers to adopt multiple API parsing tools when consuming multiple
independent services.
* Ensuring the appropriate controls are implemented to minimize the cost of breaking API changes and
allowing for consistent platform-wide management of change control risks and costs.
* Simplifying the process of making HPE GreenLake developed APIs available to third parties.

## What tools and practices can help a developer comply with this standard?

### For all HPE developers

Developers can use the following tool to check if an API is compliant with this standard: <https://github.com/glcp/api-governance-tool/>

### Developers in the Compute Business Unit

Compute Ops Management uses a purpose-built API for its UI which is not compliant with the API standard. COM intends to deprecate and discontinue using this API once a suitable replacement solution (ex. GraphQL) is supported and available within the platform.

All API changes within Compute Ops Management require the team to demonstrate that this standard has been followed during code reviews of API documentation changes to compute-cloud/public-api-docs. Our CI system runs the API Governance Tool to ensure that the documentation changes comply with the standard.

In addition, the CI system runs API Contract Testing to ensure that the API implementation matches the documentation changes.

### Developers in the Storage Business Unit

The Storage BU uses an API-first approach. API specifications are reviewed for consistency with the GLCP standard and automatically checked during CI using the API governance tool. CI tests also check that implementations conform to their API specifications.

### Developers in the Aruba Business Unit

It is recommended that the appropriate API interfaces are defined during a design review phase.

### Developers in the GreenLake Cloud Services Business Unit

The new unified PCE API (which subsumes VMaaS, BMaaS and CaaS) is being designed with the specific purpose of being compliant to this standard.

Assume that any new service offerings to be added under PCE (e.g. current recommendation for Cloud synthesis) will fall under the same standard.

### Developers in the Office of the CTO

It has been widely announced across the organization that all new APIs should conform with the standard. APIs will be reviewed during the Design Document Review sessions by architects.

## Standing Exceptions

### Industry Standard APIs

Highly adopted industry standard APIs that provide equivalent functionality should be exposed instead of or in addition to vendor-proprietary APIs whenever possible. Industry standards conformance can enable a fast time to value for customers and partners to integrate their software with HPE GreenLake.

Examples: [OAuth2](https://oauth.net/2/), [Redfish](https://www.dmtf.org/standards/redfish)

HPE GreenLake APIs that implement an industry standard interface should:

* Document and reference the appropriate industry standard.
* Conform to the industry standard specification within the context of the HPE GreenLake API surface.
* Supplement the industry standard where needed with HPE GreenLake API standards in a way that will not cause compatibility issues with the industry standard. For example, provide OpenAPI documentation, follow the security practices, be exposed in an appropriate API group.

HPE GreenLake might leverage third party software in some situations. This could be to provide functionality under the HPE GreenLake brand
or to expose the third party endpoints as available managed services.

### Third Party Branded APIs

Any third party API that is directly exposed in its native form should not be branded as an HPE GreenLake API. The third party API should be advertised and documented as a third party API with appropriate reference to the third party API documentation.

## Requesting Exceptions

To request an exception to this standard, refer to the Exceptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-06-27
