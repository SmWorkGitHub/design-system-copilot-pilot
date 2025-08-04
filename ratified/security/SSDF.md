# HPE GreenLake Secure Development Lifecycle Specification

## Introduction

This document describes the Secure Development Lifecycle (SDL)
specification that must be used for all HPE GreenLake software development including
platform, applications, and services.

## Scope

The requirements in this specification are focused on creating
standardized Secure Development Lifecycle practices across
HPE GreenLake. The baseline set of requirements and practices in this specification are derived
from [NIST Secure Software Development Framework (800-218 SP)](https://csrc.nist.gov/pubs/sp/800/218/final) (SSDF). SSDF is
increasingly being accepted as a set of de facto security practices for software development
across many industry segments. The requirements captured
in this document are aligned to comply with several other HPE Secure Development
Lifecycle policies.

## Secure Development Lifecycle Specification

This section covers the mandatory set of security practices and tasks
for all teams contributing to HPE GreenLake software. The
security practices are grouped across four categories
covering key requirements from HPE policies and NIST SSDF security
practices:

- **[Security Governance](#security-governance):** This category covers secure development
    lifecycle practices and requirements that are normally driven at
    the organizational level as policies, standards, and procedures.
    NIST SSDF 1.1 category: Prepare the Organization (PO).

- **[Secure Artifacts](#secure-artifacts):** This category covers security practices and
    requirements that are aimed at protecting different types of
    software artifacts from both malicious and
    inadvertent attacks. This category also covers key
    practices for securing the supply chain and the key artifacts that
    are generated as part of the software development process.
    NIST SSDF 1.1 category: Protect the Software (PS).

- **[Secure Design](#secure-design):** This category covers security practices and
    requirements with the aim of "building security in" to all the
    development activities and a defense in depth approach.
    NIST SSDF 1.1 category: Produce Well-Secured Software (PW).

- **[Secure Deployment](#secure-deployment):** This category covers security practices and
    requirements related to software release and post-deployment
    phases. The requirements help enforce continuous
    security monitoring and compliance across the entire software lifecycle.
    NIST SSDF 1.1 category: Response to Vulnerabilities (RV).

Increasing threats to software supply chains and critical
infrastructure has resulted in US federal agencies mandating the NIST SSDF-based
self-attestation requirement for all software purchased by the federal government. The
self-attestation form captures the minimum secure software development
requirements a software producer must meet and self-attest for the
organization's software deliverables. Since compliance to key
SSDF practices via self-attestation is mandated for all critical
software, every team working in HPE and HPE GreenLake should adhere to the
security practices listed in [SSDF Self-Attestation form](https://www.cisa.gov/sites/default/files/2023-04/secure-software-self-attestation_common-form_508.pdf).
This specification also covers the key SSDF self-attestation requirements
and maps it to the relevant categories as identified by the bold text
"**SSDF self-attestation requirement**".

The following sections cover the key security practices that must be
followed across the software development lifecycle.

## Security Governance

This category covers secure development
lifecycle practices and requirements that are normally driven at
the organizational level as policies, standards, and procedures. The
requirements in this group map to the Prepare the Organization (PO)
category of practices in NIST SSDF 1.1.

### Security training

For the security governance objective,
it is important to provide role-based security training to
all staff working in HPE GreenLake development, testing, and operations.
Providing regular security training to the
engineering community is also a key customer requirement and helps to
comply with many industry standards and regulations. The key
requirements for security training are as follows:

- Provide annual role-based security training for all personnel that contribute
    to secure development and operations of HPE GreenLake.

- Annually review and update security training for effectiveness and
   relevancy.

- Ensure all new engineers are trained in basic secure development
    lifecycle practices as part of the onboarding process.

*Benefits*: A security-aware engineering community is key for building
secure products and services. Annual security training also helps comply
with multiple industry standards and regulatory requirements.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Roles and Responsibilities

Organizations and product teams must
have well-defined roles and responsibilities for staff involved in
the development and deployment of HPE GreenLake software. The
key requirements for this practice are as follows:

- All (inside and outside the development organization) involved in the Secure
    Development Lifecycle must have defined roles and
    responsibilities covering the permissions and actions that can be
    performed in the secure development lifecycle activities. The key
    roles for HPE GreenLake include Developer Engineer, QA Engineer, Product
    Manager, Security Engineer, DevOps Engineer, Cloud Engineer,
    Platform Development Engineer, SRE Engineer, Program Manager, Cloud
    Architect, Security Architect, Product Manager, Executives, and
    Project Manager.

- Access to production environment and customer data must be
    restricted to authorized personnel who are responsible for the
    security and operations of the infrastructure.

- Access using cloud root accounts for production environments or environments with customer data must be protected by multi-factor
    authentication and restricted to authorized and approved personnel
    based on business and disaster recovery requirements. HPE GreenLake
    software teams must also comply with the [Cloud Privileged Access Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3013).

- All the defined SDL roles and responsibilities must be reviewed and
    updated every year.

*Benefits*: Well-defined roles and responsibilities implemented with the
principle of least privilege help organizations minimize attack surface
due to user errors and misconfigurations.

*Applicable products/services categories*: All

*More information:*

- [Cloud Privileged Access Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3013)

*Priority*: Mandatory

*References*:

- [Cloud Information Security Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3011)

### Security Tool Chain Management

Organizations and product teams
must have a process to validate and approve security tooling to ensure the security posture and governance of applications.
The security tools must be deployed using industry best practices. The key
requirements for this practice are as follows:

- Document the approved security tools that can be used within an
    organization. If there are approved tools from multiple vendors,
    ensure that the tools integrate with each other for meeting the
    organization's security objectives.

- Follow recommended practices to deploy, operate, and maintain
    security tool chains. The infrastructure that hosts the approved
    security tools must comply with HPE GreenLake and HPE-approved secure
    configuration and vulnerability management policies. Examples of this
    effort includes patching defects within organization-approved SLAs
    and configuration and hardening of the infrastructure based on
    organization-approved security baselines.

- Security tools must be configured to generate artifacts to meet
    defined security objectives. This means that the tools must be
    configured and tuned properly to detect security issues while also
    minimizing false positives. The tool configuration must
    be documented and enforced across all teams in the organization.

*Benefits*: Selecting, configuring, and operating security tools in a
standard way helps detect security issues in a consistent manner, and
implement shift-left security practices.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [Vulnerability Management and Patching Standard](patching_standard.md)

*Priority*: Mandatory

*References*:
The following references provide pre-vetted security tools:

- [HPE-approved security tools delivered as a service](https://hpe.sharepoint.com/sites/Product-Security-Office)

- [Cloud approved security tools](https://hpe.sharepoint.com/sites/WW/cybersecurity/SitePages/approved-cloud-security-tools.aspx)

### Security KPI for Key Security Activities

Key Performance Indicators (KPI) must be defined in order to
measure the effectiveness of the Secure Development Lifecycle
practices and tasks. To make the measurement effective, organization-approved
security tools must be used to collect metrics to measure
compliance to KPIs. The security metrics must be
treated as confidential data of the organization and protected as per
organization-specific cybersecurity policies. For different types of
security assurance practices like threat modeling, static code
analysis / static application security testing (SAST), dynamic
application security testing (DAST), or software composition analysis
(SCA), the generic KPI that must be used as criteria for completion
are as follows:

- All critical/high/medium/low issues found in security assurance
    activities during the development phase should be fixed and tested before release of the
    product/service to production.

- All critical/high/medium/low issues found in security assurance
    activities during the development phase which cannot be fixed before release must go through the
    organization-approved exception approval process.

- All critical/high/medium/low issues found in security assurance
    practices post-deployment shall be fixed and tested to comply with the
    remediation schedule in the HPE GreenLake [Vulnerability Management and Patching Standard](patching_standard.md).

*Benefits*: Well-defined KPIs provide directions to software
development teams on the required level of security rigor. Well-defined
KPIs help measure the effectiveness of an organization's security
program.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [Vulnerability Management and Patching Standard](patching_standard.md)

*Priority*: Mandatory

*References*:

- [HPE Cloud Security Vulnerability Scoring & Prioritization Specification](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3015)

### Secure Development Environment

While there are differing definitions for
the developer environment across the industry, from the HPE perspective,
the developer environment includes the collection of processes and tools
that are used to develop the source code and convert
this into a software product or service. This involves the entire
environment that supports the development process from end to end, including
developer workstations/VMs, build infrastructure, development
infrastructure/clusters, staging infrastructure/clusters, and production
infrastructure/clusters. The key requirements for this practice are as
follows:

- All machines used for development, including the build
    infrastructure and machines hosting code and HPE Intellectual
    property, must undergo periodic malware scanning using the latest
    scanning tools approved by HPE or the HPE GreenLake organization.

- All machines used for development must undergo periodic
  patching to fix security vulnerabilities, per the HPE GreenLake
  vulnerability management and patching standard.

- **SSDF self-attestation requirement**: It is mandatory to separate
    production infrastructure from the rest of the environment, such as
    staging and developer systems/environments. Separation must be
    implemented via network isolation techniques like subnets and access to these
    environments must be limited to satisfy the principle of least privilege.

- **SSDF self-attestation requirement**: It is mandatory to separate
    and protect developer systems/environments and build infrastructure.
    Separation must be implemented via network isolation techniques like subnets
    and access to these environments must be limited using the principle
    of least privilege.

- **SSDF self-attestation requirement**: Ensure all critical secrets
    within the developer environment are managed securely (e.g., using
    well configured vaults). Secrets must be encrypted at rest and in
    transit.

- **SSDF self-attestation requirement**: Enforce multi-factor
    authentication and conditional access for all high risk development
    environment infrastructure for privileged user access (e.g., admin access for staging and production clusters).
    Conditional access must be enforced by allowing access to the high risk development environment infrastructure only from HPE-managed
    systems and laptops. HPE Code repositories must never be accessed from non-HPE owned personal systems, laptops or Bring Your Own Devices.
    HPE Code must not be stored or developed on non-HPE owned personal systems, laptops or Bring Your Own Devices.
    Shared user account based access control must not be allowed for operations on the development environment.

- **SSDF self-attestation requirement**: Regularly log, monitor, and
    audit the access and authorization controls across the development environment
    and build systems.

- Harden and secure all endpoints (developer VMs, testing
    infrastructure, build environment) using organization-approved
    policies, processes, and tools.

- **SSDF self-attestation requirement**: Implement continuous
    monitoring and alerting of the development environment and integrate
    with the organization's cybersecurity incident response management
    systems and procedures.

- **SSDF self-attestation requirement**: Key build and security tools must be
    approved by the organization or by designated security teams before
    using them inside the developer environment and build infrastructure. The list includes key products and tools
    used for storing and accessing code (e.g., Git, GitHub), CI/CD tools like Jenkins and GitHub Actions,
    and tools used for documentation and implementation of agile methodology (e.g., Confluence, Jira). The
    list of security tools used for static analysis (e.g., SonarQube), dynamic analysis (e.g., HPE Armor, Qualys), and
    continuous monitoring (e.g., Wiz) also needs to be approved by the organization or designated security teams.

*Benefits*: Since security is as strong as the weakest link, a holistic
approach of securing the entire development environment is critical for
protecting from modern supply chain attacks.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Security Requirements Management

It is important to have a baseline set of security
requirements for HPE GreenLake software that are based on emerging risks
and compliance requirements. The requirements should have enough details
to enable testing teams to verify the design and implementation in a
clear and unambiguous way. Some of the key security tasks to comply with
this practice are as follows:

- Ensure there are clearly defined security requirements for all HPE GreenLake
    products and services. The requirements must not only cover
    product/service security controls, but must also include
    requirements covering development environments, [secure architecture
    design](../../ratified/security/secure_design_and_architecture.md), and secure development lifecycle/process.

- The security requirements must be either documented as security
    policies and standards or as mandatory security requirements that
    are included as part of the HPE GreenLake requirements management process.

- The requirements must be maintained regularly via periodic reviews
    to ensure the emerging threat landscape and risks are addressed
    effectively. All the documented requirements must be reviewed once
    a year for completeness and accuracy.

- The documented requirements must be enforced within the organization's
    teams and suppliers.

*Benefits*: Documented security requirements capturing software,
process, architecture, and design controls is a key security governance
practice that helps standardize security capabilities across the
organization and supply chain ecosystem.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*:

- HPE GreenLake [Secure Architecture Design Standard](secure_design_and_architecture.md)

- [Cloud Information Security Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3011)

- [HPE STAR tool](https://hpe.sharepoint.com/sites/Product-Security-Office/SitePages/STAR.aspx) capturing key security requirements for products and
services

## Secure Artifacts

This category covers security practices and
requirements that are aimed at protecting different types of
software artifacts from both malicious and
inadvertent attacks. This category also covers key
practices for securing the supply chain and the key artifacts that
are generated as part of the software development process. The
requirements in this group map to the Protect the Software (PS)
category of practices in NIST SSDF 1.1.

### Secure All Code and Software Release Artifacts

It is important to secure all key artifacts that
are generated as part of the software development process. This includes
artifacts like code, binaries, scripts, and Infrastructure as Code (IaC)
modules. Some of the key security tasks to comply with this practice are
as follows:

- Prevent unauthorized changes to code (both inadvertent and
    intentional) for all forms of code using the principle of least
    privilege. This means that people, tools, and services must have
    the minimum level of access to the artifacts needed to perform the
    intended operation.

- After every release, securely archive all the release artifacts
    including provenance data like a Software Bill of Material (SBOM) with
    read-only access to authorized personnel.

*Benefits*: The security of all artifacts is a key practice for mitigating
emerging supply chain threats and risks.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Produce and Secure Provenance Artifacts

Emerging compliance and regulatory landscapes are
mandating that organizations share provenance data using artifacts like
an SBOM. Some of the key security tasks to
comply with this practice are as follows:

- Generate SBOMs for all produced software and for every software
    release by complying with HPE SBOM policies and best practices.

- If a security tool is used to generate SBOM, it is mandatory to
    verify the SBOM accuracy and correctness before publishing to
    customers. At a minimum, the SBOM generated from approved tool needs to be verified
    for the accuracy of key software components (e.g., OS version, container runtime, web server components)
    and cryptographic modules. The SBOM must also be verified for anomalies like an abnormally low number of
    detected components and packages. For more details on generating and managing software inventory and SBOMs, please refer to the best practice
    for software inventory scanning in the more information section.

- **SSDF self-attestation requirement**: Secure and maintain SBOM
    data for both the organization-developed software and third party
    code by using the principle of least privilege. SBOMs must also be
    protected by complying with organizational information security
    policies for SBOM security and maintenance.

*Benefits*: Generation and maintenance of SBOMs is a key requirement for
many federal and high security markets. Not having SBOMs for HPE
products and services can result in loss of business in key markets and
federal deals.

*Applicable products/services categories*: All

*More information:*

- [HPE SBOM Specification](https://hpe.sharepoint.com/teams/CorporateStdDMT/Shared%20Documents/HPE-00030-01.pdf)

- [Best Practices for Software Inventory Scanning](https://rndwiki-pro.its.hpecorp.net/display/ospotools/BEST+PRACTICES+FOR+SOFTWARE+INVENTORY+SCANNING)

*Priority*: Mandatory

*References*:

- [Open source software and SBOM review specification](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE027-09.pdf)

### Standard Integrity Verification of Release Artifacts

Software integrity
verification information must be made available to software acquirers and end
customers. Software products must have release notes with the following
information:

- Release date

- Version number

- Security fixes and security enhancements

- SHA-256 or equivalent hash/checksum and instructions on how to
    verify the checksum.

*Benefits*: Providing clear instructions for verifying the authenticity and
integrity of software products released to internal and external
customers helps detect malicious attacks or random transport errors
before software is deployed in production.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory for products released to internal or external customers.
Optional for cloud services, cloud platform, and cloud applications.

*References*: None

## Secure Design

This category covers security practices and
requirements with the aim of "building security in" to all the
development activities and a defense in depth approach. The
requirements in this group map to the Produce Well-Secured Software
(PW) category of practices in NIST SSDF 1.1.

### Architectural Threat Analysis and Threat Modeling

Architectural threat analysis and threat modeling is
an important secure practice that must be followed by all
product/service teams. During the design phase, all products/services must undergo a proactive
architecture and design review to identify flaws and weaknesses. The
identified flaws/weakness must be addressed through the defined defect reporting process and application of appropriate
mitigations. The key security task to comply with this practice
is as follows:

- During the design process, HPE GreenLake software deliverables must comply with the architectural threat analysis (ATA)
   process to proactively identify threats and associated mitigations very early in the development lifecycle.

*Benefits*: Architectural threat analysis and threat modeling is a key
security practice that helps to proactively identify issues early in the
design cycle. Threat modeling is also a key requirement in
many industry standards.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [Secure Architecture Design Standard](secure_design_and_architecture.md)

*Priority*: Mandatory

*References*: None

### Secure by Design

Software architecture and software itself must be designed with built in security features to satisfactorily mitigate modern risks and threats. Some of the key security tasks to comply with this practice are as follows:

- All HPE GreenLake teams must adhere to the approved HPE GreenLake Secure Architecture Design standard (see below).

- Review the software design to verify compliance with standard HPE GreenLake security requirements mandated by the compliance and product management teams.

*Benefits*: Presence of a comprehensive secure design specification and requirements will
help address common design vulnerabilities and produce secure software.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [API Standard](../../policies/api.md)

- HPE GreenLake [Secure Architecture Design Standard](secure_design_and_architecture.md)

*Priority*: Mandatory

*References*:

- [HPE Common Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE-00030-02.pdf)

### Secure OSS and Vendor Management

With proliferation of open source software (OSS) components in HPE GreenLake software, it is important to enforce secure onboarding and management of open source and vendor-delivered components. Some of the key security tasks to comply with this practice are as follows:

- Teams must maintain an accurate inventory of all OSS components and the associated license used in HPE GreenLake software. This inventory must be reviewed by the HPE Open Source Review Board (OSRB) for compliance with HPE licensing and legal requirements. Key Practices that must be followed during OSS onboarding process are as follows:

  - **SSDF self-attestation requirement**: All OSS components must undergo a security and quality assurance analysis during the onboarding process to ensure only trusted components are used in HPE GreenLake deliverables. See the [Open source software and SBOM review specification](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE027-09.pdf).

  - **SSDF self-attestation requirement**: All third party/vendor products and services used in the organization-developed software must undergo an assessment for security and compliance based on organization-approved security policies and security requirements. Refer to the Cloud Procurement requirements in the [Cloud Information Security Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3011).

  - Open Source and vendor deliverables must be obtained from trusted locations (e.g., source must be verified with certificates and shall be a trusted by HPE and the industry).

  - Secure (encrypted) connections must be used when obtaining open source and vendor deliverables.

  - Open source and vendor deliverables must be verified using integrity checks before use (e.g., SHA-256 hash verified for downloaded components). The integrity checks must be performed using secure algorithms (SHA-256 and above only; MD5 is not an approved algorithm).

  - All open source and vendor binaries must be scanned for malware before integrating them into build infrastructure or code repositories.

*Benefits*: Secure OSS management is a key emerging customer and
compliance requirement. A secure onboarding process also helps in
implementing shift-left security methodology and is a key mitigation for
emerging supply chain attacks.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*:

- [HPE Open Source Review Board](https://hpe.sharepoint.com/sites/OSPO/SitePages/OSRP.aspx)

- [Open Source Policy](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE027-04.pdf)

### Secure Coding Standard

Development teams must have a secure coding standard covering the key programming languages used in HPE GreenLake software development. The coding standard must cover mechanisms to detect and handle [SANS/CWE Top 25 software errors](https://www.sans.org/top25-software-errors/) and dangerous coding practices or [OWASP Top Ten](https://owasp.org/www-project-top-ten/) vulnerabilities. Some of the key security tasks to comply with this practice are as follows:

- All products/teams must comply with the approved HPE GreenLake coding standard covering the key programming languages and security best practices.

- The criteria in the secure coding standards must be used to tune/configure static code analysis tools and in the manual peer review process.

*Benefits*: A secure coding
specification and compliance to the specification using automated tools and manual reviews will help
address common sources of coding vulnerabilities and produce secure software.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [Coding Standard](../secure_coding/secure_coding_and_reviews.md)

*Priority*: Mandatory

*References*:

- [HPE Common Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE-00030-02.pdf)

### Secure Compiler and Interpreter Configuration

It is important to detect security defects and
weaknesses early in the development lifecycle. Compilers and
interpreters support various options to detect potential coding
flaws during the coding phase. It is important to enforce these
settings during the development process. Some of the key security
tasks to comply with this practice are as follows:

- Use compiler, interpreter, and build tools that offer features
        to improve executable security.

- Identify, enforce, and monitor the compiler/interpreter security
        configurations.

*Benefits*: This practice helps decrease the number of security
vulnerabilities in the software with the shift-left methodology and
lowers cost by reducing vulnerabilities before the testing phase.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Static Code Analysis

Static code analysis is defined as analysis of
    source code to identify potential security vulnerabilities without
    the need for code deployment. Some of the key
    security tasks to comply with this practice are as follows:

- Static analysis must be performed using an organization-approved
        tool chain and configuration. Some of the approved tools within
        HPE and HPE GreenLake are Fortify, Coverity, and SonarQube.

- Static analysis must be performed for all kinds of code (e.g.,
        source code in different languages and Infrastructure as
        Code)

- The static analysis assurance activity must comply with
        organization-defined KPIs. For example, the general requirement is to
        fix all "Critical" and "High" issues before release. Any
        residual security issues should be documented and approved for
        exception in the project bug tracking system.

*Benefits*: Static code analysis tools perform analysis on source code
to identify potential security vulnerabilities, such as buffer overflows
and out-of-bounds memory access.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Code Signing

Code signing is the process of digitally
    signing binaries and scripts to confirm the authenticity of software
    authorship and integrity. Code signing also guarantees that the code
    has not been altered or corrupted. Some of the key security tasks to
    comply with this practice are as follows:

- All HPE and HPE GreenLake code/binaries delivered to customers must be digitally
        signed using the HPE or HPE GreenLake-approved code signing service.

- The signature must be verified by HPE and HPE GreenLake products/services during
        the update process.

*Benefits*: Code signing is a key security practice that helps detect
attacks on supply chain and integrity issues of downloaded software.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*:

- [Code Signing of HPE Software and Firmware](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE027-02.pdf)

### Malware Scanning

It is important to protect the HPE brand and
    deliverables from various malware attacks. It is mandatory to scan
    all deliverables before publishing them to external or internal
    customers. Some of the key security tasks to comply with this
    practice are as follows:

- Malware scanning of all deliverables must be performed before
        publishing them to external or internal customers.

- Malware scanning tools and versions must be updated to the
        latest releases as per HPE policy.

- The output of each malware scan comprises an audit artifact that must be maintained for at least six
        years for products released to customers.

*Benefits*: Proactive malware scanning helps in detecting
infections and potential attacks on HPE deliverables before shipping them
to our customers. Delivering malware-infected binaries can result in large
penalties and can damage the reputation of a company like HPE.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Security Testing

It is important to have a robust security testing
    strategy based on the risk profile of the HPE GreenLake software deliverables. Test
    cases should be designed to proactively detect vulnerabilities using
    the identified trust boundaries and security risks. Some of the key security tasks to comply with this practice
    are as follows:

- All products and services must have a security test strategy
        covering the high-level approach for validating the functional
        security requirements.

- All threats and mitigations identified during the architectural
        threat analysis and threat modeling review must be validated.

- **SSDF self-attestation requirement**: Vulnerability assessment
        must be performed for all products and services before release
        using HPE-approved, or HPE GreenLake designated tools (e.g.,
        Armor, Tenable, WebInspect, Qualys, Trivy, etc.). Automated
        tools/scanners must be used to identify all well-known
        vulnerabilities.

- **SSDF self-attestation requirement:** All vulnerabilities
        discovered as part of the pre-release security assurance
        activities must be mitigated before release
        to meet the security KPIs and minimize risks.

- Penetration testing must be performed on

  - All new cloud services (customer exposed) or after any major
    update to the service that is exposed to the
    customer (mandatory).

  - All new products that are released to customers
    or after any major update to these types of
    products (mandatory).

  - All new internal services that expose an API or GUI or after
    any major update to these products/interfaces (highly
    recommended).

- Security defects must be categorized and managed separately
        from functional defects.

*Benefits*: Comprehensive security testing is another important HPE GreenLake secure development
requirement. Detecting and fixing security issues before releasing to
our end customers is key for implementing a shift-left security strategy.
Robust security testing best practices also help improve the security
posture of our products and services.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*: None

### Secure By Default

Another important principle to follow during
the development process is the secure by default design practice.
When we ship a product or a service, it is important to ensure that
the configurations are secure by default. This includes the security
configurations of all underlying vendor and open source software.
A secure by default assessment before the release phase helps
ensure that less widely used insecure features are turned off by
default and the default configuration is secure while balancing
performance and business requirements. For example, if a product
supports both HTTP and HTTPS communication, HTTP should be disabled
by default and customers should be allowed to turn on this feature
based on their requirements. Additionally, this approach requires
specific action on the part of the customer, and therefore acceptance,
to use less secure configurations. Some of the key security tasks to
comply with this practice are as follows:

- Ensure compliance to the security requirements from the HPE GreenLake [Secure Architecture Design Standard](secure_design_and_architecture.md).

- Define, review, approve default configurations and baselines for all HPE GreenLake software deliverables.

  - The default configuration should be secure (default deny/disable versus allow all).

  - Rarely used and legacy features that can impact security
    should be disabled by default.

- Implement and verify default configurations and baselines
        before release to production/customers. The verification must be independently performed by
        the QA team or security team.

*Benefits*: Practicing secure by default design principles helps reduce
risks caused by misconfigurations and weak security default settings.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [Secure Architecture Design Standard](secure_design_and_architecture.md)

*Priority*: Mandatory

*References*: None

## Secure Deployment

This category covers security practices and
requirements related to software release and post-deployment
phases. The requirements help enforce continuous
security monitoring and compliance across the entire software lifecycle.
The requirements in this category map to the Response to
Vulnerabilities (RV) category of practices in NIST SSDF 1.1.

### Proactive Vulnerability Monitoring and Management

Proactive and ongoing vulnerability monitoring and management
is an important security practice mandated in every
industry standard and regulation. Every day, there are new
vulnerabilities discovered in open source and vendor components and
it is important to proactively monitor newly discovered
vulnerabilities and identify appropriate mitigations. Some of the
key security tasks to comply with this practice are as follows:

- Have an organization policy/standard for vulnerability
        management covering key KPIs for fixing
        vulnerabilities before the release of the product or service.

- **SSDF self-attestation requirement**: Implement proactive
        vulnerability monitoring of security vulnerabilities for HPE GreenLake software including all open source and vendor-delivered
        modules. The monitoring of vulnerability notifications may include notifications
        from vulnerability databases (e.g., NVD) and vendor/OSS
        community feeds, or using relevant security defect information
        from bulletins, bug reports, and release notes.

- Analyze each vulnerability to gather sufficient information
        about the risk to plan its remediation or other risk response.
        Note: for teams using a continuous pipeline (e.g., CI/CD) it may be
        more cost effective to skip the analysis and pick up the latest open source fix.

- Implement risk responses for vulnerabilities by complying with
        defined SLAs. Refer to HPE GreenLake [Vulnerability Management and Patching Standard](patching_standard.md) for additional
     details.

- Ensure continuous validation (review, analyze, test) to identify
        new vulnerabilities using industry-approved scanners for
        existing releases that are in active support or in production.

*Benefits*: Proactive vulnerability management is a key requirement for
ensuring the security of HPE GreenLake products and services. It is also a key
compliance requirement for all standards and regulations.

*Applicable products/services categories*: All

*More information:*

- HPE GreenLake [Vulnerability Management and Patching Standard](patching_standard.md)

*Priority*: Mandatory

*References*:

- [Cloud Information Security Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3011)

### Incident Management

With so much focus on security, many times, we find that security issues get reported after release by ethical hackers, customers, or our partners. With a continuous cycle of security testing within HPE GreenLake, there is also a possibility of new vulnerabilities being discovered after a product or service is released to our customers. When any new security incident is reported on HPE GreenLake products or services, it is important to manage the incident per the HPE and HPE GreenLake Incident Response Policy. Some of the key security tasks to comply with this practice are as follows:

- **SSDF self-attestation requirement**: All software and services must adhere to the organization-approved and published vulnerability disclosure program. All disclosed security defects must be fixed in a timely manner based on the urgency and impact.

- Security incidents must be handled as per the organization-approved incident management process and procedures.

- Security defect confidentiality must be maintained. This means that security defects must only be shared on a need-to-know basis and the information should be secured using encryption during internal communication.

- Analyze identified vulnerabilities to determine their root causes. Determine the class problems/patterns (root cause) for identified vulnerabilities. Take actions to proactively review and fix software for the identified root causes. Review process/policies including training needs proactively after root cause identification.

- Security bulletins must be published in a timely manner (as per HPE or HPE GreenLake policy and standards)

- There must be an approved process to address customer queries on Common Vulnerabilities and Exposures
 (CVEs).

*Benefits*: Effective incident management process is critical to meet
key industry standards and regulations.

*Applicable products/services categories*: All

*More information:* N/A

*Priority*: Mandatory

*References*:

- [Cloud Security Incident Management Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3012)

## References

- [NIST Secure Software Development Framework (800-218 SP)](https://csrc.nist.gov/pubs/sp/800/218/final)

- [HPE Product Security
Office (PSO) SharePoint](https://hpe.sharepoint.com/sites/Product-Security-Office/SitePages/Product-Security-Policies.aspx) - Policies and standards applicable to product and service development

- [HPE Cybersecurity Policies for Systems and Networks](https://hpe.sharepoint.com/sites/WW/globalsecurity/Pages/global-security-policy.aspx?ID=2894)

- [HPE Supply Chain policies and standards](https://hpe.sharepoint.com/sites/B1/EG/product_protection_info_sharing_center/Pages/Supplier_Standards_and_Policies.aspx)

- [HPE STAR Tool](https://hpe.sharepoint.com/sites/Product-Security-Office/SitePages/STAR.aspx) - Mandatory security
requirements for HPE products

- [HPE Privacy policy details and the HPE Global Master Privacy Policy](https://hpe.sharepoint.com/teams/CorporateStdDMT/Approved/HPE002-01.pdf)

- [Cloud Information Security Policy](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3011)

**Original publication date (yyyy-mm-dd):** 2024-06-03
