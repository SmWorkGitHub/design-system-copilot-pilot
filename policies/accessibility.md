---
seo:
  title: UI Standards - Accessibility | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Accessibility

Any software deployed as part the HPE GreenLake platform and services MUST be designed and proven to fully support all Level A and Level AA accessibility criteria documented in the [Web Content Accessibility Guidelines (WCAG)](https://www.w3.org/WAI/standards-guidelines/) (currently v2.2), [EN 301 549](https://www.etsi.org/deliver/etsi_en/301500_301599/301549/03.02.01_60/en_301549v030201p.pdf), and [U.S. Section 508](https://www.access-board.gov/ict/#about-the-ict-accessibility-standards) in order to comply with U.S. federal and international regulations. Failure to comply will result in a significant loss of business to HPE, legal action, fines, and/or other penalties (e.g., imprisonment).

The European Accessibility Act (EAA) mandates that all code deployed once the directive goes into effect on June 28, 2025 is fully accessible and conformant with EN 301 549 standards. HPE will have until June 28, 2030 to remediate accessibility issues released prior to the effective date.

This policy applies to HPE employees (including contractors or agencies) who are designing, developing or integrating software that (a) forms part of the GreenLake Platform and Services and (b) i*s responsible for generating any part of a customer-facing user interface.*

This policy also applies to HPE employees (including contractors or agencies) who are developing the associated supporting products and websites that provide operating instructions and support information for that product.

For the purposes of this policy, “deployed as part of the HPE GreenLake platform” refers to any software product or components that are leveraged by the HPE GreenLake platform, whether that software is developed and sold by HPE, or developed by a third party and provided as an integral component of an HPE GreenLake customer solution (for example, to manage virtual machines).

Further, for the purposes of this policy, “components” includes any associated materials and websites that provide operating instructions and support information for the software product.

## Why Does This Policy Matter?

Ensuring products and services are accessible to all users, including those with disabilities and/or those who use assistive technologies, is a growing basic requirement for enterprise products and services. Key benefits to complying with this policy include:

- Meeting the demands of legislation in US Federal and European Economic Area (EEA), which account for approximately \$11.6B of HPE’s annual revenue.

- Ensuring HPE can continue selling products in the EEA. Failure to comply with the European Accessibility Act (EAA) risks HPE losing its CE marking, which is required to sell in this market.

- Reducing technical debt and increasing efficiency by minimizing accessibility issues earlier in the product development lifecycle.

- Eliminates risk of legal action, fines, and/or other penalties such as imprisonment.

- Enables all users to use and complete tasks in HPE GreenLake both independently and efficiently.

- Showcases HPE’s core value of being a force for good and being unconditionally inclusive to our customers, partners, and HPE team members.

## What Tools and Practices Can Help a Developer or Development Team Comply With This Policy?

Defining how to implement the policy will require BU input that considers their development, test, and release processes. Below are six key action items to help build accessible products and services.

All HPE teams **MUST** implement the following into their processes:

1. **Consume HPE brand-approved styling via grommet-theme-hpe or hpe-design-tokens and adhere to HPE Design System component behaviors.**

    - The HPE Design System and HPE Theme are designed to conform to WCAG 2.2 Level A and AA. Correctly implementing these components will help ensure conformance to WCAG. Recent testing has shown that implementing the HPE Design System reduces accessibility issues by ~90%. Refer to the [Accessibility](https://design-system.hpe.design/foundation/accessibility) section of the HPE Design System.

2. **Perform an Accessibility Assessment.**

    - Prior to formal release, all initial and subsequent major release versions of HPE GreenLake platform and services products MUST be assessed for conformance to the applicable Level A and AA guidelines in the currently published [Web Content Accessibility Guidelines (WCAG).](https://www.w3.org/TR/WCAG22/) Refer to [How to Meet WCAG (Quick Reference)](https://www.w3.org/WAI/WCAG22/quickref/?versions=2.2&currentsidebar=%23col_overview#top) for information on success criteria for each WCAG guideline.

    - HPE GreenLake platform and services product teams are responsible for performing this assessment. Conformance assessments MUST be performed by skilled evaluators with a properly equipped testbench that includes common assistive technology products to validate compatibility with WCAG. It is recommended to outsource this assessment for conformance to qualified (HPE) vendors.

    - All accessibility issues identified in the assessment MUST be addressed within 90 days according to pending accessibility regulations.

    - The results of this assessment MUST be provided to the Product Accessibility Office (<hpeaccessibilty@hpe.com>) for the purpose of creating a customer-facing product accessibility conformance report (VPAT).

3. **Create an International Accessibility Conformance Report (ACR, or VPAT) for each major release or update, or once per year, whichever comes first.**

    - [Accessibility Conformance Reports (ACRs, or VPATs)](https://www.section508.gov/sell/vpat/) MUST be completed at the conclusion of an accessibility assessment. This is a growing requirement from customers as part of the RFP and bid process. Failure to complete VPATs can prevent sales of the product(s). This reporting process is facilitated through the HPE Product Accessibility Office (<hpeaccessibility@hpe.com>) using data provided directly by the responsible BU.

    - Product and Service teams MUST complete VPATs for US Section 508, WCAG 2.2, and EN 301 549, also called an International VPAT. Creating an International VPAT at the conclusion of the assessment saves significant time and money, and ensures business continuity.

The following processes **SHOULD** be considered to help enhance accessibility:

1. **Provide accessibility training to all designers, UI developers, and QA testers.**

    - The [HPE Product Accessibility Office](https://hpe.sharepoint.com/sites/F5/CTO/Office/Accessibility/Pages/index.aspx) provides free accessibility guides and trainings for HPE team members to complete and/or reference.

    - There are a number of [free or low-cost trainings](https://www.section508.gov/training/) available online to provide baseline knowledge about digital accessibility.

    - Deque University provides live and/or online, self-paced, role-based training to both technical and design teams. Please contact <adam.york@deque.com> for more information.

2. **Incorporate WCAG 2.2 Level A and AA conformance into the [Definition of Done](definition-of-done-policy.md) (DoD).**

    - Add tasks for checking and testing accessibility to the DoD per the Non-Functional Requirements listed in the [HPE GreenLake Development Policy for Definition of Done](definition-of-done-policy.md).

    - Remediate all issues before release into production, or within 90 days.

3. **Integrate Automated and Manual Accessibility Testing into the Development and QA Processes.**

    - Perform automated and [manual accessibility and usability evaluations](https://www.w3.org/WAI/test-evaluate/involving-users/) throughout the development and QA process to resolve issues before the product goes into production, as this can save up to 99.5% of remediation costs. [Deque’s Axe DevTools](https://www.deque.com/axe/?branded=&utm_term=deque%20axe&utm_campaign=Search+-+axe+Pro+-+Branded&utm_source=adwords&utm_medium=ppc&hsa_src=g&hsa_ad=431265982275&hsa_tgt=aud-1249912087819:kwd-942967158653&hsa_mt=e&hsa_ver=3&hsa_acc=7854167720&hsa_kw=deque%20axe&hsa_grp=76354481701&hsa_cam=6769485255&hsa_net=adwords&gad_source=1&gclid=Cj0KCQjwiOy1BhDCARIsADGvQnBUjBGTZfWc4sTuaohph8BelVOew4icocL1LAw2BMfjpQbsnyeB5rcaAug6EALw_wcB) and [Evinced](https://www.evinced.com/) are recommended for automated testing.

    - Use the free [Accessible Software Design educational Resources – Developer Guides and Instructional Videos](https://hpe.sharepoint.com/sites/f5/cto/office/Accessibility/Pages/accessible-web-software-and-hardware-design.aspx)  to help you create accessible and usable software and services. One form of manual accessibility evaluation is described in the [Guided JAWS Evaluations Guide](https://hpe.sharepoint.com/:b:/r/sites/F5/CTO/Office/Accessibility/Documents/Developer%20Guides/Developer%20Guide%20-%20Guided%20JAWS%20Evaluations%20Aug_%202023%20v1.pdf?csf=1&web=1&e=PYGrO5).

    - Incorporate Lighthouse testing into QA reviews to catch and remediate issues before performing an accessibility assessment.

Please reach out to the [HPE Product Accessibility Office](mailto:hpeaccessibility@hpe.com) with any questions.

## What HPE Policies or Standards Does This Policy Refer To?

The policy does not reference any standards that are developed and maintained by HPE, but does refer to the following externally developed standards:

- The [**Web Content Accessibility Guidelines (WCAG) 2.2**](https://www.w3.org/TR/WCAG22/) is developed and maintained by The World Wide Web Consortium (W3C).

- [US Section 508/255 Technical Standard](https://www.access-board.gov/ict/#about-the-ict-accessibility-standards) is maintained by the General Services Administration (GSA).

- [EN 301 549 Accessible ICT Technical Standard (v3.2.1)](https://www.etsi.org/deliver/etsi_en/301500_301599/301549/03.02.01_60/en_301549v030201p.pdf) is maintained by the European Telecommunications Standards Institute.

## What HPE Policies or Standards Are Related to This One?

This policy is related to the following HPE policies or standards:

- [Consistent Visual Design](../ratified/ui/ui-consistent-visual-design.md)
- [Definition of Done](definition-of-done-policy.md)

**Original publication date (yyyy-mm-dd):** 2023-09-19

**Revised:** 2024-12-18

**What's changed** 2024-12-18

Major overhaul of this document

- Overview section:  WCAG 2.1 -> 2.2, added discussion of European Accessibility Act
- "Why Does This Policy Matter?" section: replaced all existing text with new text, emphasizing the strict new EAA rules
- "What Tools and Practices Can Help..." section: replaced all existing text with new text containing three MUST and three SHOULD processes for implementing accessibility
- Added directly referenced and related standards sections
- In the sidebar and indexes, moved this policy into a new multi-section UI standard
