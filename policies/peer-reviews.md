---
seo:
  title: HPE GreenLake Development Policy for Peer Reviews | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Peer Reviews

Code changes must be peer-reviewed and approved. Approval SHOULD be by at least two HPE engineers other than the code committer.

This policy applies to HPE developers coding any software deployed as part the HPE GreenLake platform and services. This includes any code that may have access to customer data and applies to developers in the PCS, GLCS, GLP, COM, DSCC, and other engineering organizations. For the purposes of this policy, “deployed as part of the HPE GreenLake platform” refers to software components that are exclusively leveraged by the HPE GreenLake platform.

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Why Does This Policy Matter?

This policy aims to ensure that code changes undergo proper scrutiny to maintain quality and reduce the risk of introducing errors or bugs into released products.

## What Tools and Practices Can Help a Developer Comply With This Policy?

In some business units, the two reviewer peer review policy is automatically enforced by settings in the source code management software (SCM).

## How Can Adherence to This Policy be Verified?

HPE auditors can verify adherence to this policy by:

1. Examination of reviewers on specific pull requests in the SCM system
2. Examination of the automated peer review settings in the SCM system for the specific organization, repository, or branches

## What Policies or Standards Are Related to This One?

The [HPE GreenLake Development Standard for Secure Coding and Review](https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/secure-coding/) standard contains a [checklist](https://developer.greenlake.hpe.com/docs/greenlake/standards/ratified/secure_coding/securecodingquick/) that may aid peer reviewers in performing their code review.

## In What Circumstances Should Non-Compliance of This Policy be Escalated to HPE Senior Leadership?

If a code review is skipped for any reason, this justifies an escalation.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-10-31
