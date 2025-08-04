---
title: HPE GreenLake Development Policy for Java Language Coding | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Java Language Coding

All new Java code MUST follow this approved version of the [Google Java Style Guide](https://github.com/google/styleguide/blob/4d9a47834bfbadeb00a3dcf3d9808ffe49e43aeb/javaguide.html). As of March 2024, the rendered version of the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) is in sync with the approved version and is easier to view. If you use the rendered version, ensure that it still matches the approved version.

This policy applies only to Java, not other JVM languages.

This policy applies to HPE developers coding any software deployed as part the HPE GreenLake platform and services. This includes any code that may have access to customer data and applies to developers in the PCS, GLCS, GLP, COM, DSCC, and other engineering organizations. For the purposes of this policy, “deployed as part of the HPE GreenLake platform” refers to software components that are exclusively leveraged by the HPE GreenLake platform.

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Definitions

[1] <a name="def-DeployedAsPartOfPlatform"></a>For the purposes of this standard, “deployed as part of the HPE GreenLake platform” refers to software components that are exclusively leveraged by the HPE GreenLake platform.

For example:

* Software components that form part of a globally hosted HPE GreenLake solution, including applications such as Aruba Central, or shared hosted services such as GLP Common Cloud Services, are considered in scope.

* Software components running on a device that provides HPE GreenLake functionality (such as the HPE GreenLake Control Plane) are considered in scope.

## Why Does This Policy Matter?

Standardized formatting drastically increases code maintainability, as developers' experiences align to a common flow, structure, look, and feel. As other developers refactor or add features to the code, knowledge of its structure will decrease the cost of ownership.

The Java Language Coding Policy is designed to ensure Java code written by HPE developers is (a) written according to established industry best practices, and (b) easily maintained and transferable, within and across teams in HPE. This also facilitates investment in common tooling and training across the company that can benefit all Java developers. Divergent practices increase risk against reliability, bug production, and security.

## What Tools and Practices Can Help a Developer Comply With This Policy?

Methods to help developers comply with this Java standard vary across HPE.

### For All HPE Developers

Consider the following verification methods:

* The [Checkstyle](https://checkstyle.sourceforge.io/) Java coding standard checker could be integrated in a continuous integration pipeline using a pre-commit hook.
* If no continuous integration is present, adherence to the standard can be validated via a [peer review](peer-reviews.md) process.

### Developers in the Compute Business Unit

Each repository with Java code MUST run formatting validation adhering to the Java standard outlined in this document. Validation failure prevents merging the PR. This will be implemented on all repositories.

### Developers in the Aruba Business Unit

The Aruba BU will integrate a Java standard-compatible formatter (work is still in progress starting Gravity) with the "super linter" which is to be applied to all CI systems deployed in the Aruba BU. Use of the formatter is mandatory for all systems and will be automatically included via GitHub Actions or other CI manager.

## How Can Adherence to This Policy be Verified?

As business units add checks to their continuous integration tools, adherence to this standard can be verified by reviewing the configuration and logs of such tools.

## What Policies or Standards Are Related to This One?

[Standards for Engineering Best Practices](../index_engineering.md) describes other coding policies and standards, including for other programming languages.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2024-03-25
