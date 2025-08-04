---
seo:
  title: UI Standards - Policy | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development UI Standards Policy

Developers MUST follow the guidance listed in the sections below for the HPE GreenLake platform and GLP aaS products to ensure a consistent, inclusive, and intuitive user experience across all supported devices and contexts.

It is mandatory that all HPE GreenLake platform and GLP aaS products, regardless of their platform (web, local, mobile), along with embedded user product teams, strictly adhere to the HPE Theme. Conformance ensures a consistent visual language across all HPE products and digital experiences for authenticated and unauthenticated users.

As with all policies, this policy follows the governance of the Developer Standards Program. Any business unit that feels they need an exception should follow the provided guidance and timeline.

This policy applies to all HPE GreenLake product software developers, HPE product designers, HPE product documentation developers, and HPE customer support content and application developers. All new HPE business unit product services intended for integration/distribution with the HPE GreenLake platform are expected to conform to the UI standard guidelines prior to customer release.

This policy does not apply to HPE internal developers or HPE product designers and their products/tools that are not sold or otherwise provided to HPE customers. This policy does not apply to some embedded hardware UIs that HPE has no control over. HPE products that are currently on the market and have no roadmap for integration into the HPE GreenLake platform are exempt from full conformance except for the adoption of the HPE Theme and HPE Brand compliance.

This standard consists of the following sections:

* [Accessibility](accessibility.md)
* Responsive Design - not available yet, but coming soon
* [Consistent Visual Design](../ratified/ui/ui-consistent-visual-design.md)
* [User feedback, usability, interaction, and experience](../ratified/ui/ui-usability.md)
* Performance and loading times - not available yet, planned for future
* Internationalization and Localization - not available yet, planned for future
* Security - not available yet, planned for future

## Why Does This Policy Matter?

This policy ensures that all HPE products and services are brand compliant, accessible, inclusive, and use the latest HPE Theme along with the HPE Design System's visual language and interaction behaviors. This alignment supports HPE’s Journey to One – Win Together initiative, promoting a consistent and unified user experience.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

Methods to help developers comply with this policy vary across HPE.

### Developers in the Compute Business Unit

COM is already leveraging Grommet and the HPE Design System.  We are keeping COM’s theme and design closely aligned with the direction that HPE GreenLake is following.  COM currently uses the HFWS header but is exploring adopting the HPE GreenLake header.  We work with a UX designer to review our designs for consistency with the HPE branding and design system guidance, and our UX designer consults with the HPE Design System team as needed. Our designers reference previous designs as well as a COM specific Figma library. Some work would need to be done to ensure everything is based off the design system Figma.

COM does not have any use case for applying alternative themes from OEM vendors, but this could be supported via the same mechanisms as the HPE theme provided by the HPE Design System team.

### Developers in the Hybrid Cloud and Office of the CTO Business Unit

HPE GreenLake platform is built upon Grommet and the look and feel is tied to the HPE Design System.

### Developers in the Private Cloud Business Unit

The DSCC UI Chapter will socialize this HPE GreenLake Development UI Standard to DSCC stakeholders and provide DSCC platform and application dev teams with a proven overall strategy for verifying and validating DSCC UI applications against them for general use cases. Included in this guidance will be a set of frameworks and templates that can be used to bootstrap new applications that simultaneously adhere to the policy and contain the latest tooling (automation) for detecting violations that can be found programmatically.

## What HPE Policies or Standards Does This Policy Refer To?

This policy refers to the following HPE policies or standards:

* [Accessibility](accessibility.md)
* [Secure Coding and Review](secure-coding.md)
* [HPE Brand Central](https://brandcentral.hpe.com/)
* [HPE Design System](https://design-system.hpe.design/)
* Latest [HPE Grommet Theme](https://github.com/grommet/grommet-theme-hpe) on GitHub
* [Grommet framework v2](https://v2.grommet.io/) - external site

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

There are none currently.

**Original publication date (yyyy-mm-dd):** 2024-12-17
