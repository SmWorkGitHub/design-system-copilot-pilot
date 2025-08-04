---
seo:
  title: HPE GreenLake Developer Policy for Events | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Developer Policy for Events

This policy establishes the rules for HPE Services regarding the creation and transmission of events within HPE GreenLake platform. The policy aims to ensure consistency, reliability, and a standardized event payload.

An event payload MUST conform to the [HPE GreenLake Developer Events Specification](../ratified/api/events-spec.md) if it meets any of the following criteria:

* The event and its documentation are visible to parties outside of HPE such as customers or partners.

* The event is provided by the HPE GreenLake platform [[1](#def-HPE-GL-CoreService)] for HPE GreenLake Services [[2](#def-HPE-GL-S)] or for other HPE Services [[3](#def-OtherHPEServices)].

* The event is provided by a HPE GreenLake platform Core Service [[1](#def-HPE-GL-CoreService)] and is used by other Core Services [[1](#def-HPE-GL-CoreService)].

* The event is provided by any HPE GreenLake Service [[2](#def-HPE-GL-S)] for the HPE GreenLake platform [[1](#def-HPE-GL-CoreService)] or for another HPE GreenLake Service [[2](#def-HPE-GL-S)] developed by different HPE organizations.

Any other events developed by a single HPE organization within an HPE GreenLake Service that do not meet the aforementioned criteria SHOULD conform to the HPE GreenLake events standard specification.

If the event meets the criteria, but for business reasons is proposed to be exempted from conformance with the standard specification, then an exemption proposal should be sent to the Standards Committee to provide the decision.

## Definitions

For the purposes of this standard, the following terms apply:

* <a name="def-HPE-GL-CoreService"></a> **[1] HPE GreenLake platform Core Service**&mdash;Core services are microservices or bounded contexts that reside within the HPE GreenLake platform such as frontend, app catalog, device management, audit, orgs, service identity, etc.

* <a name="def-HPE-GL-S"></a> **[2] HPE GreenLake Service**&mdash;A service that resides outside of the HPE GreenLake platform such as Aruba Central, Compute Ops Management, Data Ops Manager, Private Cloud Business Edition, etc.

* <a name="def-OtherHPEServices"></a> **[3] Other HPE Services**&mdash;HPE services which are not under the HPE GreenLake platform umbrella such as BRIM, GTS, etc.

## Why Does This Policy Matter?

HPE GreenLake benefits from this policy for the following reasons:

* Establishes a clear event standard, ensuring that all services adhere to a uniform structure, making it easier to integrate, maintain, and troubleshoot.
* Adheres to the CloudEvents specification, ensuring that HPE's events are compatible with various external systems, facilitating seamless integration and interoperability.
* Provides developers with well-documented and predictable event schemas, improving development efficiency.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

The CloudEvents SDK provides libraries in multiple languages (e.g., Go, Java, Python, etc.) that make it easier for developers to generate, send, and validate CloudEvents in a standardized format, ensuring compliance with the event structure.

## What HPE Policies or Standards Does This Policy Refer To?

This policy refers to the following HPE policies or standards:

* [HPE GreenLake API Versioning](../ratified/api/versioning.md)

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

* [HPE GreenLake Development Standard for APIs](api.md)

**Original publication date (yyyy-mm-dd):** 2024-11-07
