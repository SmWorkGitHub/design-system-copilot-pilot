---
seo:
  title: HPE GreenLake Developer Privacy Requirements Standard | HPE GreenLake Platform
toc:
  enable: true
---

# Privacy Requirements for HPE GreenLake Developers

This document describes requirements and guidelines for the use of personal data in HPE GreenLake services and products.
The target audience is development and operations staff. For information about who these privacy requirements apply to, see the [HPE GreenLake Development Policy for Privacy](../../policies/privacy-policy.md).

Personal data consists of unique identifiers associated with an individual, or group of individuals, that can be used to reveal
details of the individual’s life.
This includes personally identifiable information (PII).
A customer expects that propagation of their personal data be minimized and receive appropriate
protection when it is collected. Furthermore, there are privacy protection criteria that are legally
required in many countries, such as the General Data Protection Regulation (GDPR) in the European
Union.

This document provides a level of detail that should be sufficient for most developers and operations staff.
For more comprehensive information, refer to the
[HPE Privacy & Information Governance Office](https://hpe.sharepoint.com/teams/olaa/SitePages/privacy.aspx),
the [Privacy Policies](https://hpe.sharepoint.com/teams/olaa/SitePages/privacypolicies.aspx) document,
and the
[Privacy Rulebook](https://hpe.sharepoint.com/teams/olaa/SitePages/privacy-rulebook.aspx).
Additionally, the [HPE Privacy Statement](https://www.hpe.com/us/en/legal/privacy.html)
describes the company's privacy practices, including personal data policies.

A
[Compliance 360](https://hpe.sharepoint.com/teams/hpe-rcgo/SitePages/Compliance360.aspx)
review is required for all new HPE hardware products, software products, service offerings, mobile apps,
tools, and processes.
A Compliance 360 review is also required for refreshes or updates to these projects.

## Key Requirements and Guidance

The following list describes the key requirements for complying with the HPE GreenLake privacy standard (in bold font) and additional guidelines.

1. **The collection of personal data shall only be performed when there is a demonstrated need.** \
When considering the collection of personal data, the first rule is minimization.
Collect and process only the minimum amount of personal information necessary for the intended purpose.
Information that might be needed in the future should not be collected until there is a demonstrated need.

2. **A link to the
[HPE Privacy Statement](https://www.hpe.com/us/en/legal/privacy.html)
must be prominently displayed to the customer before they use HPE services.**

3. **Consent is required before storing or processing personal data.** \
Explicit consent from individuals is required before collecting or processing their personal information.

4. **The collection and use of personal data must be disclosed to customers.** \
Developers are responsible for documenting the personal information that is being collected so customers can understand the
information being collected and provide consent.
This information will also be required for the HPE internal Compliance 360 review and a new Compliance 360 review must be submitted whenever new personal data is to be collected.

5. **Personal data that is collected must have appropriate confidentiality and integrity protections.** \
When there is a demonstrated need to collect personal data, appropriate technical and organizational measures (e.g., access control lists, encryption)
must be used to protect the information from unauthorized access, disclosure, alteration, and destruction.
Failure to do so can lead to substantial legal penalties for HPE.

6. **The default privacy settings shall provide a high level of protection.** \
Integrate privacy protections into the design and development of HPE GreenLake services to ensure that privacy settings are
configured to most private by default.
Further information can be found in other HPE GreenLake standards such as
the personal data or PII section of the [Secure Architecture Design](secure_design_and_architecture.md/#personal-data-or-pii)
and
the [Secure Coding](../secure_coding/secure_coding_and_reviews.md) standards.

7. **The customer must be made aware of how their personal data is accessed and who has access.**

8. **If personal data must be logged, pseudonymization must be implemented.** \
If personal data must be logged, the information shall undergo pseudonymization.
An example of personal data that may need to be logged is an IP address.
However, there is a distinction between types of IP addresses to be considered.
While client IP addresses may be considered personal data, IP addresses of non-personal devices are generally not considered personal data.
Pseudonymization can be achieved with methods like a one-way hash function applied to the IP address before logging.
This will provide consistent and unique values per IP address without recording personal data.

9. **Personal data must not be retained beyond the stated retention period.** \
Personal data shall only be retained for as long as it is necessary to fulfill the purposes for which it was collected,
unless a longer retention period is required or permitted by law.
See [Hewlett Packard Enterprise Global Records Retention Schedule](https://hpe.sharepoint.com/teams/grm/Shared%20Documents/Compliance/Record%20Series/Simplified%20Records%20Retention%20Schedule%20HP112-01%20Rev%20F.pdf)
for HPE policy.
When possible, deletion of customer data should be automated, based upon defined policy.

10. **Customers have the right to understand what personal data HPE GreenLake retains and the right to have their data erased.**

11. **Personal data must be stored within the geographic areas acceptable to customers and in compliance with their country’s laws.** \
Data transfer / data sovereignty is legally limited in some countries and may be of concern for customers even when not legally required.
HPE must be able to explain to customers where the customers' personal information and customers' data will be geographically located.
This may also include metadata such as logging information.
Compliance with applicable data sovereignty laws and regulations ensures that personal information is stored and processed in
accordance with local requirements.
These geographical limitations are also relevant for log file storage and processing and may have limitations on locations
from which the service is managed.

12. **If a suspected breach or incident involving personal data is identified, the
Regulatory Compliance and Governance Office
[Incident Reporting form](https://entsp1.its.hpecorp.net/teams/bic14/tslegalregulatory/Lists/Incident%20Reporting%20Form/NewForm_copy%283%29.aspx?Source=https://entsp1.its.hpecorp.net/teams/bic14/tslegalregulatory/SitePages/IRFthankyou.aspx)
must be submitted.** \
To access the form, you must be connected to the HPE internal network or VPN. Log on with your HPE email address and password.

**Original publication date (yyyy-mm-dd):** 2024-08-15
