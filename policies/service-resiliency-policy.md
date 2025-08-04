---
seo:
  title: HPE GreenLake Development Policy for Service Resiliency | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Service Resiliency

Teams building services MUST:

* Assign a [DED](devopsentityid-policy.md) for each service, platform or system they support.
* Have a maintained [SLO](sloslitargets.md) published and discoverable.
* Meet the availability and resilience targets defined by their SLO.
* Plan for how to be resilient. This SHOULD include the ability to detect, notify and resolve a problem.
* Have the ability to resolve issues within the downtime window defined by their SLO.
* Define expected throughput (examples: requests/second, megabytes/second).
* Ensure dependencies can provide the reliability and throughput required.
* Monitor service’s interaction with dependencies.
* Plan for both course-of-business failures and disaster-level events.
* Define and maintain runbooks to deal with outages.
* Capture long term trends to enable capacity planning.

When building services:

* Services that require high reliability (> 99.9% uptime) MUST NOT have a single point of failure in their critical path.

  Services themselves SHOULD run multiple instances to allow for service uptime regardless of single instance or node failure. This requirement also applies to dependencies—all dependencies cannot have a single point of failure which causes a failure in the upstream system.

Teams SHOULD:

* Define and maintain runbooks documenting escalation paths with dependencies
* Define and maintain runbooks describing day-to-day operational responsibilities

This policy applies to:

* All current (non-EOL) and new services and hosted products across HPE GreenLake. This applies to systems that cannot be “touched” by the HPE Services Team, but still have an HPE-runtime/HPE-run presence.

This policy does not apply to:

* Exclusively customer-run, non-cloud products purchased from HPE
* EOL products and services run by HPE

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Why Does This Policy Matter?

By increasing the reliability of the platform and setting standards for new offerings we enable two things:

* HPE offerings can be built on existing services that provide a known and maintained SLO in the same manner that they would build on top of any other SaaS offering
* Customers can reliably integrate against HPE GreenLake as they would against any SaaS offering

Not adhering to this policy exposes HPE to the following risks:

* Violation of SLAs or service-specific SLOs across the company
* Expensive outages for systems and services (cost in terms of human time and effort for incident response)
* Reputation damage to HPE as a service-oriented company (e.g., violating the “headline test”)
* Other legal/regulatory compliance violations (e.g., hosting exclusively in one country while requiring systems to be hosted in-country across our service area)

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

There are none currently.

## What HPE Policies or Standards Does This Policy Refer To?

This policy refers to the following policies or standards:

* [DevOps Entity ID (DED) policy](devopsentityid-policy.md)
* [SLO and SLI Targets policy](sloslitargets.md)

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

* [Vulnerability Management and Patching standard](../ratified/security/patching_standard.md)

**Original publication date (yyyy-mm-dd):** 2024-04-16
