---
seo:
  title: HPE GreenLake Developer Secrets Management Specification | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Developer Secrets Management Specification

This document outlines the mandatory secrets management specifications
that must be followed throughout the development, deployment, and
operation of HPE GreenLake platform (GLP) applications and services. It
ensures that sensitive information such as passwords, API keys, and
access tokens are securely managed at every stage, safeguarding our
systems against unauthorized access and potential security threats.

Refer to [HPE GreenLake Development Policy for Secrets
Management](../../policies/secrets-management-policy.md) for
the accompanying policy to this specification.

## Scope

The baseline set of requirements in this specification enables secure
management of secrets during the HPE GreenLake software development
lifecycle. Secrets here refer to sensitive information, such as database
passwords, API keys, access tokens, or other confidential data, that
must be kept secure during the entire development and operation of HPE
GreenLake software. This specification does not cover requirements
around key management solutions used normally to encrypt secrets and
data. The requirements for managing secrets in this specification
augments other global HPE secret management policies and specifications.

## Secrets Management Requirements

This section covers the mandatory secret management requirements that
development teams must follow during the development, deployment, and
operation of HPE GreenLake software and services.

### Do Not Hardcode Secrets in Images and Repositories

Hardcoded secrets are one of the most common vulnerabilities exploited
in the wild. Attackers use popular tools and automation techniques to
detect hardcoded secrets and use them regularly as an attack vector. It
is particularly important to protect HPE GreenLake deliverables from
this threat. The key requirements for securing against this threat are
as follows:

- Secrets such as passwords, API keys, or private keys must never be
  stored directly in the Git repository. Refer to the [HPE Git
  Security
  Specification](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3021)
  for more details.

- Images, package managers, infrastructure as code, and serverless
  functions stored in repositories and Artifactory must not hardcode
  secrets like passwords, API keys, and access tokens.

#### More Information

- [HPE Git Security
  Specification](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3021)

### Protect Secrets at Rest and in Transit

It is an especially important security practice to protect secrets both
at rest and in transit. The key requirements are as follows:

- Never store secrets in cleartext. All secrets stored in persistent
  media must be encrypted by default using HPE GreenLake approved
  algorithms as specified in [HPE GreenLake TLS Cryptographic
  Requirements](GLS_Crypto_Requirements.md).
  Keys used to secure secrets must be kept separately from the actual
  secret using a FIPS compliant key management service (e.g., FIPS
  compliant key management service (AWS KMS) or FIPS compliant hardware
  security module (HSM)).

- All secrets in transit must follow the [HPE GreenLake TLS
  Cryptographic
  Requirements](GLS_Crypto_Requirements.md).

- Implement principle of least privilege for accessing secrets. Limit
  permissions (e.g., read only vs read-write) and limit access to users
  and applications based on the job role and need to know basis.

#### More Information

- [HPE Cloud Security Secrets Manager
  Specification](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3020)

- [HPE GreenLake TLS Cryptographic
  Requirements](GLS_Crypto_Requirements.md)

### Securing Secrets in CI/CD pipelines

With increased threats from supply chain attacks, it is particularly
important to securely manage secrets used in CI/CD pipelines. The key
requirements are as follows:

- Secret scanning of CI/CD artifacts like images, packages, and
  containers must be implemented in the pipeline. Issues reported by
  secret scanners must be triaged and fixed before releasing to the
  Dev/QA environment.

- CI/CD secrets must never be stored in clear text. CI/CD secrets must
  be protected and stored using an approved secret manager solution like
  Hashicorp vault or cloud native secret manager solutions (e.g., AWS
  Secrets Manager). Please consult your BU security lead for the list of
  approved secret manager solutions.

### Secure Provisioning of Secrets in Kubernetes

Kubernetes secret management must be a key focus area for cloud native
applications. The key requirements are as follows:

- Secrets must never be stored in Kubernetes ConfigMaps. Kubernetes
  ConfigMap is a satisfactory solution for storing cluster
  configurations.

- High-risk environments (e.g., publicly exposed containers, sensitive
  workloads) must use dedicated secrets management tools like Hashicorp
  Vault or cloud native secret manager solutions (e.g., AWS Secret
  manager).

- Kubernetes Secrets can be used in low-risk environments (e.g.,
  non-publicly exposed containers processing public data). Kubernetes
  secrets are unencrypted by default in many supported versions.
  Encryption must be enabled before using Kubernetes secrets for storing
  sensitive information.

#### More Information

- [Kubernetes
  Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

### Secure Provisioning of Secrets in Containers

With the proliferation of containers in cloud native applications,
securely managing secrets is extremely important to mitigate container
security threats and risks. The key requirements are as follows:

- Secrets must never be hardcoded in any container image. Container
  secrets must be provisioned at runtime using a dedicated secrets
  management tools like HashiCorp Vault or cloud native specific secret
  manager solutions (e.g., AWS Secret manager). Using an external secret
  management solution for managing secrets must be the preferred design
  choice for HPE GreenLake software deliverables.

- Container secrets can also be provisioned using environment variables
  or storage volumes. Using environment variables for passing secrets
  increases the inherent risks of information disclosure by compromised
  containers in the same system and by other containers running in a
  privileged mode. Using environment variables for passing secrets to
  containers should be considered as a design choice for containers
  running in private subnets or in low-risk environments. If secrets are
  passed using storage volumes, the access to secrets must be restricted
  using the principle of least privilege.

### Secure Provisioning of Secrets in Serverless Deliverables

Serverless design is exceedingly popular in cloud native application
development and hardcoded secrets in serverless code can be a major
source of vulnerabilities. The key requirements are as follows:

- Secrets must never be hardcoded in serverless functions like AWS
  Lambda.

- Use cloud native and IAM (identity and access management) features to
  configure roles with least privileges to allow serverless code access
  to secrets stored in a secret manager solution like HashiCorp Vault or
  cloud native secret manager solutions. For example, database passwords
  must be stored in a secret manager solution like HashiCorp Vault and
  associated serverless code must be configured with the least
  privileged role to allow access to the Vault during its operation.

- Secrets can also be provisioned at runtime using environment
  variables. This method is only recommended for Lambda functions running
  in a low-risk environment.

### Secrets Access Control and Logging

It is important to secure access to secrets and have sufficient audit
logs for auditing and forensic operations. The key requirements are as
follows:

- Role-based access control (RBAC) using IAM-style roles or policies
  must be used for accessing secrets.

- Enforce the principle of least privilege by granting only necessary
  access to secrets based on the need-to-know principle. Service
  accounts must only be given the minimum privileges to perform the
  intended operation.

- Detailed logs of every secret access attempt (success or failure),
  including user, timestamp, IP addresses, and actions taken, must be
  generated and stored according to the [HPE GreenLake Audit Logging
  Standard](../logging/audit_logging_guidelines.md).

#### More Information

- [HPE GreenLake Audit Logging
  Standard](../logging/audit_logging_guidelines.md)

### Secret Rotation and Lifecycle Management

Secret rotation is a process that involves regularly updating passwords
and keys to reduce the risk of compromised credentials and sensitive
data. The key requirements are as follows:

- All secrets must be rotated at a minimum of once a year.

- Automation must be used to rotate the secrets wherever possible.

- HPE GreenLake services, applications, and platform must back up the
  secrets for meeting high availability and disaster recovery
  requirements.

- The disposal phase of the secret lifecycle must include irretrievable
  removal using crypto-shredding or industry accepted data sanitization
  techniques to ensure deleted secrets cannot be accessed or recovered.

### Emergency Rotation Procedures

Upon detection of a secret leak, it is imperative to act swiftly to
mitigate risk. The secret should be rotated as soon as possible, ideally
within 48 hours, to prevent unauthorized access. This rapid response
aligns with best practices, such as those recommended by NIST, for
managing the lifecycle of cryptographic keys and secrets. The specific
steps should include:

- Identification of the affected secret: Quickly ascertain which secret
  has been compromised.

- Generation and implementation of the new secret: Replace the
  compromised secret across all systems and services without delay.

- Verification and communication: Ensure the new secret is operational
  and inform relevant stakeholders of the change.

## References

- [HPE Git Security
  Specification](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3021)
  
- [HPE Cloud Security Secret Manager
  Specification](https://hpe.sharepoint.com/sites/WW/cybersecurity/Pages/global-security-policy.aspx?ID=3020)

- [NIST Guidelines for Media
  Sanitization](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r1.pdf)

**Original publication date (yyyy-mm-dd):** 2024-10-28
