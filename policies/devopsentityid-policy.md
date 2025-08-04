---
seo:
  title: About the HPE GreenLake Development Policy for DevOps Entity ID | HPE GreenLake Platform
toc:
  enable: true
---

# About the HPE GreenLake Development Policy for DevOps Entity ID

This document describes the DevOps Entity ID policy for HPE GreenLake developers. The policy is:

> Every HPE service, and its dependencies, must have a unique DevOps Entity ID assigned. This applies to all development teams in HPE that are developing services (a) branded as HPE GreenLake and (b) consumed by HPE customers.

## Overview

If a team is creating a new [service](#def-Service) that is (a) branded as HPE GreenLake and (b) consumed by HPE [customers](#def-Customer), OR is a dependency of software that is consumed by HPE customers, that service MUST have an up-to-date [DevOps Entity ID](../other/glcs-sre/devopsentityid.md) (DED) assigned in a DED registry such as: <https://dedmanager.hpelabs.net>

To request a DED for an HPE GreenLake service, complete the [DED Registration](https://forms.office.com/r/ESfPYy8je1) form.
![Relationship of DevOps Entity ID to communications, storage location, persons involved, and ancestor/dependency relationships.](images/devopsentityid-diagram.png)
The only requirement as specified in this policy is that DED creation be a controlled event with an appropriate governance model.

If any of the attributes of a service change, the *product owner* named in the DED is the individual responsible for keeping data in the DED registry up to date. If the named product owner is no longer responsible for a named service (for example, because they leave the company) they MUST inform the lead architect at the contact above with an alternative product owner who has officially accepted this responsibility.

The DED is intended to refer to a reasonably significant chunk of functionality such as a service that is maintained by a scrum team. A DED will often map to the output of a single team. A DED could be applied to a micro-service, but the expectation is that this would be rare. However, organizations are free to decide the level of granularity for themselves.

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

*Service definition*<a name="def-Service"></a>—For the purposes of this document, a *service* is defined as a software system that can be modified in production without explicit coordination with any other software system that depends on it.

*Customer definition*<a name="def-Customer"></a>—For the purposes of this document, a *customer* is any entity external to HPE with whom HPE has a commercial relationship, or intent to create one. This may include end users consuming the platform, or partner organizations who collaborate with HPE to bring products and services to market.

## Why Does This Policy Matter?

The goal of the DED is to provide a central registry of software systems that can facilitate collaboration between teams within HPE, including across organizational boundaries.

While this is useful in many instances (such as to promote discovery and reuse of services across organizational boundaries), it is particularly useful for incident management and resolution—as it allows for core component owners to be quickly identified and, if necessary, contacted.

While not explicitly in scope for this policy, establishing a DED is a foundational requirement for other policies that seek to improve effectiveness across organizational boundaries, such as tracking resiliency requirements, dependency analysis, SLO/SLA definitions, and telemetry correlation.

Implementation can be left to the discretion of individual teams, with the expectation of being able to integrate systems as required via APIs and additional automation outside of the scope of this document.

## What Tools and Practices Can Help a Developer Comply With This Policy?

Implementation of this policy is currently in the beginning stages in many business units, although some have mature processes.

### For All HPE Developers

Some business units are using the DED registry developed by the GLCS team: <https://dedmanager.hpelabs.net> (VPN required). See the following sections for BU-specific DED implementation progress and plans.

### Developers in the Hybrid Cloud and Office of the CTO Business Unit

DEDs are a long-term identifier, and some degree of judgement and context awareness needs to be granted when deciding when and how granular DEDs should be.

We recommend that one or more dedicated architects within Hybrid Cloud and Office of the CTO be deputized with the responsibility of determining when DEDs are assigned and updating the register accordingly. We recommend that these architects be accessible through a dedicated e-mail address as mentioned in the informal process option in [Overview](#overview). The architects would work with a team within GLCS who maintain the DED registry and share knowledge and policy on when DEDs should be assigned.

### Developers in the Aruba Business Unit

This policy is not currently implemented within the Aruba team. We recommend that a similar process be established to that proposed for the Hybrid Cloud and Office of the CTO business unit.

### Developers in the Compute Business Unit

Compute Ops Management has implemented their DED style mechanism in a separate wiki repository, with associated COM team contacts and runbook links. We keep our wiki updated as things change.

### Developers in the Storage Business Unit

This policy is not currently implemented within the Data Services Cloud Console team. We recommend that a similar process be established to that proposed for the Hybrid Cloud and Office of the CTO business unit.

## How Can Adherence to This Policy be Verified?

An auditor from HPE can determine if this policy is being followed by an organizational unit by researching whether the organization has implemented a process for assigning and tracking DEDs and has access to a DED registry service.

If this standard is not followed, SRE, Security, Services/GMS/PointNext, and R&D developers will spend significantly more time tracking down the purpose of a particular service or who maintains it. This in turn will increase the time and impact of any outage or incident involving that service.

## What Policies or Standards Are Related to This One?

The [DevOps Entity ID](../other/glcs-sre/devopsentityid.md) document is the *standard* component of this DevOps Entity ID *policy* document. That detailed specification describes the type of DED metadata required, how to determine the scope of the service to which a should DED apply, and ancestor/descendant relationships. [GLCS SRE Standards](../other/glcs-sre/index.md) includes other related SRE standards.

## In What Circumstances Should Non-Compliance of This Policy be Escalated to HPE Senior Leadership?

All non-compliance situations which are not actively being worked by the delegates or SRE team should be escalated to leadership. DEDs are universally applicable to all software and services within HPE.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2024-04-01

**Revised:** 2025-03-17

**What’s Changed** 2025-03-17

* Added link to HPE GreenLake DED request form in the [Overview](#overview) section.

* In the [Overview](#overview) section, removed theoretical information about how a DED request process could be implemented informally by email or as a GitHub process, as it is now implemented as a Microsoft form.

* In the [Developers in the Hybrid Cloud and Office of the CTO Business Unit](#developers-in-the-hybrid-cloud-and-office-of-the-cto-business-unit) section, removed the following text: "This policy is not currently implemented within the Hybrid Cloud and Office of the CTO business unit."
