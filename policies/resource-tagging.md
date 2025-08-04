---
seo:
  title: HPE GreenLake Development Policy for Resource Tagging | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Resource Tagging

If a resource in HPE GreenLake platform supports any form of labeling, then it MUST be in the form of tags (represented as key/value pairs). Tags MUST comply with the [HPE GreenLake Tagging Standard](../ratified/tagging/tagging_standard.md).

If a resource has tags, then any API provided to manage the resource MUST support management of tags. Specifics of how APIs SHOULD support tags SHOULD be specified in the given API (REST, gRPC, etc.) standard.

This policy applies to any HPE GreenLake service developer, regardless of whether a developer codes a core GLP service or codes a BU-specific service which implements a resource that supports tags.

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Why Does This Policy Matter?

This requirement was originally implemented for device inventory (see [GLP PRD](https://hpe.atlassian.net/wiki/spaces/HPEUCP/pages/1741556457/Device+Tags+PRD)).

Custom tagging is a popular mechanism in many infrastructure management tools to help customers organize their inventories effectively, including reconciliation with other registries used by other processes or tools. Consistent implementation of this capability across HPE will significantly minimize the learning curve and integration costs incurred by the customer, and allow for re-use of client libraries, UI, and CLI components.

## What Tools and Practices Can Help a Developer Comply With This Policy?

The GLP team intends to extend the [API governance tool](https://github.com/glcp/api-governance-tool) to verify that tags follow the appropriate semantics. A sample application will exist that demonstrates the specifications.

## How Can Adherence to This Policy be Verified?

The [API governance tool](https://github.com/glcp/api-governance-tool) can be used to check if REST APIs that implement tags are compliant with the tagging standard. In addition, a [code review](peer-reviews.md) should be conducted to check compliance. One more method is to use APIs that implement tags to find out if tags in APIs are compliant with the standard.

## What Policies or Standards Are Related to This One?

The [HPE GreenLake Tagging Standard](../ratified/tagging/tagging_standard.md) is the standard that describes how to implement resource tagging as required by the policy described in this document.

## In What Circumstances Should Non-Compliance of This Policy be Escalated to HPE Senior Leadership?

This standard aims to create a common customer experience around tagging in HPE GreenLake platform. If there is a circumstance when a non-standard customer experience should be escalated to HPE senior leadership, then this policy goes under that same case.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-11-16
