---
seo:
  title: HPE GreenLake Developer Policy for User Behavior Analytics | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for User Behavior Analytics

Developers SHOULD implement user behavior analytics in all cloud services included in HPE GreenLake by using the [greenlake-analytics library](https://github.com/glcp/greenlake-analytics).

User behavior analytics refers to tracking user interactions with HPE GreenLake cloud services, including user-initiated events, such as clicks or page views, system-initiated events, such as alerts and messages, plus any other events and data required to contextualize user-initiated events.

The greenlake-analytics library is a React library that ensures compliance with privacy, security, and data governance requirements and controls the connection between HPE GreenLake and our analytics vendor, which is currently [Amplitude](https://amplitude.com/).

## Why Does This Policy Matter?

1. HPE customers will benefit from internal (PLM, UX, and engineering) knowledge of how they use HPE GreenLake and our improved ability to identify problem areas.
2. HPE customers will benefit from our ability to evaluate the effectiveness of our UX designs for new features and for remedies we implement to resolve user experience issues.
3. HPE customers will benefit from HPE GLP priorities that better reflect customer impact as indicated by customer usage patterns.

## What HPE Policies or Standards Does This Policy Refer To?

This policy refers to the following HPE policies or standards:

* [Amplitude playbook](https://hpe-my.sharepoint.com/:p:/p/fatima_hassan/ETbVQTOoKHJGkE6oIQwGaXgBOWF7D0S3lX5ZfloMHevcbA?e=HnuDF0) (PowerPoint)

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

* [Privacy Requirements](privacy-policy.md) – End users must consent to user behavior tracking.
* [Accessibility](accessibility.md) – The greenlake-analytics library relies in part on accessibility metadata to generate usable user behavior data.
* [UI Guidelines](ui-policy.md) - User behavior analytics helps evaluate UI quality and design efficacy.

**Original publication date (yyyy-mm-dd):** 2025-05-30
