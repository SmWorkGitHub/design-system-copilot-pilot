---
seo:
  title: HPE GreenLake Energy and Power Reporting Policy | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Energy and Power Reporting Policy

Energy and power reporting related to sustainability on HPE GreenLake platform MUST adhere to the [Energy and Power Reporting for Sustainability](../ratified/energy-spec.md) spec.

This allows the spec and measurement values within to change as needed as new data populates the Sustainability Dashboard and we learn more about how to accurately measure energy and power. (The customer-facing name for the Sustainability Dashboard is HPE Sustainability Insight Center but Sustainability Dashboard is used in this document.)

This policy applies to any devices that are visible on or managed through the HPE GreenLake platform. Therefore, engineers and developers that deal with on-device power measurement for such devices need to follow this policy. In addition, since these measurements are to be reported into the Platform, developers associated with sending or receiving the data need to follow the policy.

Note that the policy need not apply to in-market devices. Updating in-market devices may not be technically feasible or make business sense. However, all devices in development should follow the policy, so engineers and developers working on those devices should follow the policy.

## Why Does This Policy Matter?

It is critical that HPE business units and developers follow the proposed policy to ensure data quality and consistency for sustainability-related products on HPE GreenLake platform. Energy and power consumption measurements from devices must be of sufficient accuracy and frequency to produce reasonably accurate estimates of total IT energy consumption and greenhouse gas (GHG) emissions. The frequency of measurements must be sufficient to enable the value-added features around recommendations and advanced analytics that customers want.

Additionally, GLP device-specific services such as COM, Aruba Central, DSCC, and OpsRamp reach out to the devices that they manage to collect this data. Other, non-device specific applications and services on GLP, such as the Sustainability Dashboard, communicate with COM / Aruba Central / DSCC / OpsRamp to obtain the power or energy data for the devices managed by each.

Without consistency in device energy or power measurements, we run the risk of presenting incorrect, inconsistent, or valueless information to customers. Without consistency in data reporting, it becomes much more difficult for platform developers to accurately collect data from different devices, increasing the possibility for bugs and errors. Furthermore, different HPE offerings may then surface different power use information to the customer for the same device. This will lead to confusion and cause customers to mistrust the data provided by HPE. All offerings should report the same information for a given device and the data reported should be consistent across device types.

Inconsistent, confusing, or incorrect data presented to customers reflects, at best, just a poor-quality product, and in the worst case could open HPE to accusations of greenwashing.

## What Tools and Practices Can Help a Developer Comply With This Policy?

OpsRamp may be useful in implementing this policy as it can help collect energy or power data from devices that cannot easily communicate with HPE GreenLake platform.

## How Can Adherence to This Policy be Verified?

For a third party to determine whether a device complies with this policy, the party would need to access a log of energy or power measurements that are being supplied to HPE GreenLake platform and inspect the measurements to determine compliance. This could likely be achieved programmatically.

## When Should Non-Compliance of This Policy be Escalated?

There are two foreseeable cases in which non-compliance with this policy should be escalated to senior leadership:

1. If a HPE GreenLake platform offering (e.g. DSCC, COM, Aruba Central, or devices accessed through OpsRamp) is surfacing sustainability-related information that is not based upon measurements complying with this policy and the non-compliance has been brought to the attention of the HPE GreenLake platform offering team, but that team has not provided a plan to bring their offering into compliance.

2. Either the Sustainability Dashboard team or a device team wants a class of devices to be included in the Dashboard, but the devices cannot feasibly be brought into compliance and the teams determine that non-compliance poses a risk of material inaccuracy in reporting of sustainability-related information in the Sustainability Dashboard.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-11-14
