---
seo:
  title: HPE GreenLake Development Policy for Secure Architecture Design  | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Secure Architecture Design

Any software component that forms part of the HPE GreenLake platform MUST have an [architectural threat analysis](#what-is-an-architectural-threat-analysis) (ATA) performed on their architectures at least [once a year](#what-is-the-required-annual-architectural-threat-analysis), and changes reviewed in iterative delta reviews.

Further, any HPE employee engaged in designing architecture and writing software that forms part of the HPE GreenLake platform SHOULD adhere to the [Secure Architecture Design](../ratified/security/secure_design_and_architecture.md) standard as noted. Further, any architectural threat assessment conducted SHOULD include an evaluation of the product against these same security guidelines.

## What is an Architectural Threat Analysis?

For the purposes of this policy, an architectural threat analysis is a security design review that examines confidentiality, integrity, and availability of the application layer processes, components, interfaces, and data, to establish if appropriate mitigations, including authentication, authorization, and audit are planned for implementation.

## What is the Required Annual Architectural Threat Analysis?

Development teams are required to have their architectures undergo an architectural threat analysis at least once a year, whether or not there have been any changes, further all significant changes must be reviewed in iterative delta reviews. Developers MUST consult a designated security architect on any collective changes at least quarterly, who will determine if the changes are significant and require delta review. These reviews can be performed using the HPE practice indicated in the [tools and practices](#what-tools-and-practices-can-help-a-developer-comply-with-this-policy) section or another architectural threat analysis tool/review with similar rigor. Examples of other ATA tools include Microsoft [STRIDE](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats) and the Software Engineering Institute's [OCTAVE Framework](https://insights.sei.cmu.edu/library/operationally-critical-threat-asset-and-vulnerability-evaluation-octave-framework-version-10/).

## Why Does This Policy Matter?

A robustly designed security architecture reduces the risk of vulnerabilities, data breaches, and system intrusions.

Such an architecture bolsters resilience against potential coding errors and eases adherence to secure coding standards via built-in security requirements.

## What Tools and Practices Can Help a Developer Comply With This Policy?

Process documents, review meeting documentation, architecture pages, and end-to-end instructions for reviewers are available to help teams provide the artifacts to be used in the architectural threat analysis process. GLS/Office of the CTO has a robust process for conducting reviews. These processes can be taught to organizations who wish to adopt this formal process. Contact Tracy Patman (<patman@hpe.com>) or Nigel Edwards (<nigel.edwards@hpe.com>).

Information regarding the architectural threat analysis process practiced in HPE GreenLake (VPN required):

* [Architecture Page Review](https://rndwiki-pro.its.hpecorp.net/display/HCSS/Architecture+Page+Review)
* [Architecture Page Requirements](https://rndwiki-pro.its.hpecorp.net/display/HCSS/Architecture+Page+Requirements)
* [Architecture Page Template](https://rndwiki-pro.its.hpecorp.net/display/HCSS/Architecture+Page+Template)

## How Can Adherence to This Policy be Verified?

A full architectural threat analysis review with no findings, indicating the integration of security measures, showcases an engineering team's compliance with the policy.

## What Policies or Standards Are Related to This One?

Other HPE software coding standards include:

* [Secure Coding and Review](secure-coding.md)
* [Peer Reviews](peer-reviews.md)
* Coding language standards such as [Go](golang.md) and [Python](pythonlang.md)

## In What Circumstances Should Non-Compliance of This Policy be Escalated to HPE Senior Leadership?

Non-compliance should be escalated if, over 3 months, teams:

* Neglect security consultation
* Forego architectural review for over a year
* Repeatedly identify high and critical defects in iterative reviews

These scenarios indicate potential deviation from design standards.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-12-05
