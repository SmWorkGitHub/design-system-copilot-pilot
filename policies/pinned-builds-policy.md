---
seo:
  title: HPE GreenLake Development Policy for Pinned Builds | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Pinned Builds

All software builds that run in HPE GreenLake platform MUST be pinned in so far as the dependencies of packages and source code from which the software is assembled are known at the time of production to ensure:

* Accurate dependency tracking
* Easier debugging of build process and resulting product
* Confident cherry-picked releases

Compliance with version 1.0 of [SLSA Level 2](https://slsa.dev/spec/v1.0/levels#build-l2) SHOULD be followed.

Inputs to the build processes MUST list static version numbers for all third-party libraries included in the product for a given build of the product.

There MUST be a one-to-one mapping from the complete set of third-party libraries and HPE source code to the final product version number. If any third-party library, the source code, or any combination thereof are modified, then a new version number of the product MUST be created.

Version numbers of third-party libraries SHOULD include build numbers and/or SHAs from a well known source. (SLSA v1.0 L2 requirement)

A dedicated system with a static manifest of source code and/or libraries SHOULD be used to produce the product. (SLSA v1.0 L2 requirement)

This specification does not extend to requiring reproducing two builds of the same product produce the same binary SHA bit-for-bit.

This specification does not require that all dependencies are stored for reproduction of the product at a future point in time.

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Why Does This Policy Matter?

This policy provides the following benefits:

* Allows for knowledge of what source code is executing in the product which affects security in all forms.
* Enforces a known output with known behavior during operation of the product.
* Allows for traceability into what source code the product is composed of.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

Methods to help developers comply with this policy vary across HPE.

### For All HPE Developers

Be aware of the following definitions:

* [SLSA level 2 build security level](https://slsa.dev/spec/v1.0/levels#build-l2)

### Developers in the Aruba Business Unit

All Aruba software is built on dedicated build machines using known CI tools (Jenkins, etc.). In addition, each application uses a pinned version of each third-party library in static files.

### Developers in the Compute Business Unit

All software written by the Compute BU is built by known CI tools on self-hosted machines with all dependencies recorded in static files at build time manifests.

### Developers in the Hybrid Cloud and Office of the CTO Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Private Cloud Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Storage Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

## What HPE Policies or Standards Does This Policy Refer To?

There are none currently.

## What HPE Policies or Standards Are Related to This One?

There are none currently.

**Original publication date (yyyy-mm-dd):** 2024-05-24

**Revised:** 2024-11-04

**What’s Changed** 2024-11-04

* Global wording change from "hermetic" to "pinned" to comply with SLSA usage
* Clarified that version numbers of third-party libraries SHOULD include build numbers and/or SHAs *from a well known source*
