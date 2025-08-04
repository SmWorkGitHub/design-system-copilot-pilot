---
seo:
  title: HPE GreenLake Developer Policy for Auditing Logging | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Audit Logging

All HPE-authored code MUST create audit log entries for security relevant events.

The [HPE GreenLake Audit Logging Standard](../ratified/logging/audit_logging_guidelines.md) provides a definition of an audit log, including the types of events that need to be audited. It also defines the events and attributes that MUST be included in audit logs (e.g., timestamp, user ID).

The contents of the audit log are used for the following purposes:

* To identify potential security breaches
* To assert which human being authorized changes to be made to a system, by tracking privileged operations that a user performed
* To demonstrate compliance with industry regulations
* To record activity for determining a customer’s obligations

This policy applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Why Does This Policy Matter?

Adherence to the audit logging policy is required for meeting regulatory requirements (e.g., SOC 2) and for helping to detect and mitigate cybersecurity attacks.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

The following sections describe assistance available to HPE developers to help comply with this policy.

### For All HPE Developers

Teams use tools such as logz.io and Humio to collect audit log information.

### Developers in the Aruba Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Compute Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the GreenLake Cloud Services Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Hybrid Cloud and Office of the CTO Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Private Cloud Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

### Developers in the Storage Business Unit

There is no information about official tools or practices to help comply with this policy in this business unit.

## What HPE Policies or Standards Does This Policy Refer To?

This policy refers to the following HPE policies or standards:

* Logging [policy](logging-policy.md) and [standard](../ratified/logging/index.md)
* [Cloud Service Debug Logging Standard](../ratified/logging/services.md)

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

There are none currently.

**Original publication date (yyyy-mm-dd):** 2024-08-28
