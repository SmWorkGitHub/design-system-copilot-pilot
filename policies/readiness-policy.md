---
seo:
  title: Production Readiness - Policy | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Production Readiness

The Production Readiness Guide is a set of guidelines and requirements for building and running reliable services on HPE GreenLake cloud.

Please see the sections that make up this guidance and MUST language in each.

* [Overview](../ratified/readiness/readiness-overview.md)
* [Availability](../ratified/readiness/readiness-availability.md)
* [Observability & Incident Management](../ratified/readiness/readiness-observability.md)
* Scalability - not available yet, but coming soon
* Usability - not available yet, planned for future
* Maintainability - not available yet, planned for future
* Secure-ability - not available yet, planned for future

## Why Does This Policy Matter?

Standards for observability ensure that HPE has a consistent way to detect, alert, and then respond to unreliability.

Adopting error budgets would allow for explicit tradeoffs between feature velocity and reliability backed up by objective metrics.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

Methods to help developers comply with this policy vary across HPE.

### Developers in the Hybrid Cloud and Office of the CTO Business Unit

OCTO/HC Observability may be inconsistent within the CTO BU due to the diversity of products.  

Within GLP, observability is provided by integration against NewRelic for APM and distributed tracing.

CCP encompasses a variety of platforms, including legacy OneClick (AWS), the new Northstar platform (AWS), some Azure components, and recent on-premises developments.

## What HPE Policies or Standards Does This Policy Refer To?

There are none currently.

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

* [Audit Logging](audit_logging_policy.md)
* [Logging](logging-policy.md)
* [Service Resiliency](service-resiliency-policy.md)

**Original publication date (yyyy-mm-dd):** 2025-01-27
