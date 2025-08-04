---
seo:
  title: HPE GreenLake Development Policy for Device Authentication | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Development Policy for Device Authentication

All physical devices manufactured by HPE (e.g., compute, storage, networking, etc.) that will interoperate with HPE GreenLake (including GLP as well as applications like COM, DSCC, Aruba, etc.) must follow the [Device Authentication](../ratified/security/devid.md) standard which uses secure device identities based on the DevID (IEEE 802.1AR) industry standard for authentication and zero-touch onboarding. These identities consist of a unique per device public/private key pair and X.509v3 certificate.

The HPE Device Authentication standard applies the [IEEE 802.1AR](https://1.ieee802.org/security/802-1ar/) and [TCG TPM 2.0 Keys for Device Identity(https://trustedcomputinggroup.org/wp-content/uploads/TPM-2p0-Keys-for-Device-Identity-and-Attestation_v1_r12_pub10082021.pdf)] standards and specifies requirements and permitted values for HPE-issued DevIDs in the factory and in the field. This enables all services that perform device authentication as part of their operations to operate on a common and well-specified mechanism and data format. Virtual devices (i.e., virtual machines) are outside the scope of this specification.

This policy applies to all HPE employees working on hardware and software built by HPE that authenticate to the HPE GreenLake platform.

## Why Does This Policy Matter?

This policy benefits HPE in several ways:

* HPE will be able to provably verify the devices connected to the platform, including that they are genuine HPE devices, and not counterfeits, protecting our revenue.
* HPE customers will be less likely to suffer a data breach incurred through a malicious attacker being able to able to exfiltrate device credentials from a customer’s device and impersonating that customer’s device when authenticating to the platform.
* The cost of ownership of the platform will be reduced through the ability to standardize on a single device authentication credential at the time of manufacture. This will allow credentials to be embedded at the time of component manufacture (e.g., a DL380 motherboard) and re-used without the need to build in a new credential issuing step at the time of final product assembly.
* The cost of ownership of the platform will be reduced as fewer mechanisms for device authentication will need to be supported, and more centralized services can be developed that leverage a common authentication framework (this cost will be mainly realized once legacy systems and devices are eventually deprecated).
* User experience will improve through the removal of manual processes for authenticating devices and the move to a completely zero-touch experience.

## What Tools and Practices Can Help a Developer Comply With This Policy?

The RDA (Remote Device Access) service issues LDevID certificates that follow this specification with version 3 of their certificate format onwards. Project Pebble (LDevID provisioning framework for HPE GreenLake) will heavily leverage this specification. Contact members of either team for implementation assistance or access to their code. Also, feel free to reach out to PKI Policy Committee members Tom Laffey, Nigel Edwards, or Matthew Yang.

## How Can Adherence to This Policy be Verified?

Certificates are issued by a central HPE Certificate Authority which has been reviewed to comply with this specification.

The existence of the DevID and attestation key credentials on devices can be readily confirmed by customers and engineering teams using open standard commands or APIs (e.g., Redfish on servers).

Conformance of the credentials to this specification is confirmed by inspecting the corresponding certificates using standard tools like OpenSSL.

## What Policies or Standards Are Related to This One?

No internal standards are related. However, this standard depends on policies implemented by the HPE public key infrastructure (PKI) team.

## In What Circumstances Should Non-Compliance of This Policy be Escalated to HPE Senior Leadership?

Non-compliance should be escalated if an HPE device does not have a compliant identity leading to new code having to be introduced for onboarding and management tasks.

## Requesting Exceptions

To request an exception to this policy, refer to the exemptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-12-05
