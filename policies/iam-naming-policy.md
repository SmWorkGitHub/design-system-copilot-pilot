---
seo:
  title: HPE GreenLake Developer Policy for Role and Permission Naming | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Developer Policy for Role and Permission Naming

Developers integrating a service with HPE GreenLake platform MUST follow the [HPE GreenLake Developer Role and Permission Naming Specification](../ratified/iam-naming-spec.md).

## Why Does This Policy Matter?

Predefined roles and permissions are customer visible constructs central to administration of authorization.

Consistency in naming, syntax, and semantics of roles and permissions across the platform is crucial for enhancing the customer experience. It not only fosters a professional and cohesive appearance but also facilitates customers' understanding of their security configuration.

The standard also includes guidelines about permission granularity, allowing security administrators to apply the security best practice of least privilege.

By minimizing unexpected variations in privileges granted by IAM administrators through different sets of roles or permissions, this standardization enables customers to reason about their security setup more effectively, improving customer security posture.

The standard is also critical to help reduce internal friction in this difficult, often contentious area of “naming things”. By establishing clear guidelines, we increase the likelihood that teams will make correct choices from the beginning, requiring less intervention during role and permission naming reviews by product management, platform UX, and authorization technical leads.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

For HPE GreenLake platform core services (CCS services), all changes and additions to authorization registration proposed during the [Design Review Process](https://hpe.atlassian.net/wiki/spaces/GEDD/pages/1914044949/Design+Review+Process+Overview) must be compliant with the role and permission naming standard.

For all other platform integrators, each business unit MUST appoint one standards champion who is responsible for reviewing all authorization configuration for their business unit jointly with the GLP IAM Roles & Permissions Standard Group (contact: <glp-iam-naming-standards@groups.int.hpe.com>).

The GreenLake Platform Authorization v2 service enforces the syntax requirements at registration time.

## What HPE Policies or Standards Does This Policy Refer To?

There are none currently.

## What HPE Policies or Standards Are Related to This One?

The working group developing this standard is also working on resource naming standards. That standard will be adjacent to this standard.

**Original publication date (yyyy-mm-dd):** 2024-11-14
