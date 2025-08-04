---
seo:
  title: HPE GreenLake Development Standard for Secure Coding Overview | HPE GreenLake Cloud Platform
toc:
  enable: true
  maxDepth: 5
---

# HPE GreenLake Development Standard for Secure Coding and Review

**Any code developed by HPE that is software that is compiled or interpreted from human readable source code and deployed as part the GreenLake Cloud Platform [[1](#def-DeployedAsPartOfPlatform)] MUST conform to all of the secure coding requirements published in the [HPE GreenLake Development Standard for Secure Coding](../ratified/secure_coding/secure_coding_and_reviews.md).**

This standard applies to HPE employees (including contractors or agencies that are developing software at the request of HPE) that are developing software that forms part of the GreenLake Cloud Platform, by writing software that is compiled or interpreted from human readable source code.

This standard applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Definitions

[1] <a name="def-DeployedAsPartOfPlatform"></a>For the purposes of this standard, “deployed as part of the GreenLake Cloud Platform” refers to software components that are exclusively leveraged by the GreenLake Cloud Platform.

For example:

* Software components that form part of a globally hosted GreenLake solution, including applications such as Aruba Central, or shared hosted services such as GLCP Common Cloud Services, are considered in scope.

* Software components running on a device that provides GreenLake functionality (such as the GreenLake Control Plane) are considered in scope.

## Why does this standard matter?

The HPE GreenLake Development Standard for Secure Coding has been developed as specific targeted responses to real-world security breaches that occurred because of software flaws in customer-facing code and represent the collective wisdom of many security engineers at HPE. By requiring engineering teams to follow these guidelines we will prevent a wide range of common attacks on our platform.

If a developer is building software that has access to customer data or could become a vector for an attacker to gain access to customer data fails to follow the specification there is an increased risk of security compromise leading to direct or indirect loss of customer data, intellectual property or loss of service. This will result in significant financial loss to HPE as a result of fines imposed by regulatory bodies and devaluation of the brand.

## What tools and practices can help a developer comply with this standard?

### For all HPE developers

To ensure conformity to the standard, all source code subject to the standard must be reviewed by an expert trained to check for violations before that code is run in production. We recommend that reviewers [use this checklist](../ratified/secure_coding/securecodingquick.md).

Additionally, automated source code scanning before (pre-submit) or during (submit) the process of committing code to a source code repository may help catch some of these errors more quickly than a human reviewer. These include:

* Static analysis
* Secret scanning
* Log file analysis (e.g. with MACIE)

One or more of the following recommended analysis tools for HPE GreenLake code MUST be used to comply with this standard.

* Armor Fuzzer (for Web APIs)
* Armor OWASP Top 10 Scan&mdash;for web applications and APIs only
* Bandit&mdash;Python-specific security analysis tool
* Coverity
* Fortify
* Nexpose
* Qualys WAS
* RESTLer&mdash;OpenAPI fuzzer
* SonarQube
* Tenable (Nessus)
* WebInspect

Note however that no known automated tool is currently able to reliably detect all known security vulnerabilities defined in this document.

### Developers in the Compute Business Unit

The Compute BU today has a pool of security reviewers who assess code when invited to by development teams on a best effort basis, according to the [guidelines here](https://rndwiki-pro.its.hpecorp.net/display/ComputeCentral/Secure+Coding+Practices+and+Secure+Code+Reviews).

### Developers in the Storage Business Unit

Architectural Threat Analysis reviews are conducted by a security review team. Development teams are trained to review their code according to secure development practice, but also invite review by specialists for security related features. Review of container and VM configuration and security settings are included in penetration testing.

### Developers in the Aruba Business Unit

Developers should deploy automated code scanning tools which will check for common mistakes and alert developers so they can be fixed.

## Standing Exceptions

In principle this standard does not apply to HPE employees writing software that is never accessible to outside parties, AND does not handle customer data, configuration, or assets, AND could not be used by an attacker to gain unauthorized access to HPE systems. However, since very little software developed at HPE strictly meets these criteria, it is recommended that the standard apply universally unless an explicit exception is granted.

On-device firmware (such as UEFI boot firmware or iLO firmware) used in GreenLake products and services is not required to follow this standard, however other HPE corporate cybersecurity and product development policies still apply to this software.

## Requesting Exceptions

To request an exception to this standard, refer to the Exceptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-06-27

**Revised:** 2024-08-23

**What's Changed** 2024-08-23

* Added a list of required code scanners
