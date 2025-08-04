---
seo:
  title: HPE Device Identity and Attestation Key Certificate Specification | HPE GreenLake Cloud Platform
toc:
  enable: true
  maxDepth: 4
---

# HPE Device Identity and Attestation Key Certificate Specification - Version 1.2

*Theo Koulouris*, *Tom Laffey*, *Gareth Richards*, *Steve Roscio*

To enable a Zero Trust, as a Service model all HPE products must be provisioned with strong
cryptographic identities. For physical platforms (e.g., servers, storage arrays, access
points, etc.) HPE cryptographic identities are manifested as
a combination of a Public/Private cryptographic key pair
and an X.509 public key certificate.

This document specifies the **certificate format** for HPE Device Identities
(DevID) and Attestation Keys (AK). Both the factory-provisioned Initial
DevID (IDevID) and Initial AK (IAK), and the factory-provisioned and
in-field provisioned Local DevID (LDevID) and Local AK (LAK) are covered in this
specification. While there is a TPM-related semantic difference between
a DevID and AK, the associated certificates are almost identical. The
generic term DevID is used in the remainder of this document to refer to
any of IDevID, LDevID, IAK or LAK; the exact name (e.g., IAK) is only
used when required.

Refer to [Policy for Device Authentication](../../policies/devid-policy.md) for the accompanying policy to this standard.

## Introduction

HPE Device Identities (DevID) and their certificates must comply with the *IEEE 802.1AR*
specification [[1]](#references). Additionally, Trusted Platform Module (TPM) version
2.0-based DevID must comply with the Trusted Computing Group’s (TCG)
*TPM 2.0 Keys for Device Identity and Attestation* specification [[2]](#references).
Both specifications leverage the Internet Engineering Task Force (IETF)
*RFC 5280 Internet X.509 Public Key Infrastructure Certificate and
Certificate Revocation List (CRL) Profile* standard [[3]](#references).

This document only covers the information related to the identification
of the entity (e.g., a server) identified in the certificate. Fields
such the Issuer that depend on the implementation of HPE-wide PKI policies are
not specified in this specification.

In HPE, the Registration Authority (RA) and Certification Authority (CA)
are responsible for determining which identification fields are included
in a DevID certificate, and the location of the identification fields in
the certificate (e.g., in the Subject field or the Subject Alternative
Name). The presence of an identification field in a Certificate Signing
Request (CSR) does not mean it will be included in the certificate, or
at the location requested, unless specified by this document. The CA may
also insert additional items in order to comply with this specification.

### References

[1] 802.1AR: Secure Device Identity, IEEE. Version IEEE Std
    802.1AR-2018. <https://1.ieee802.org/security/802-1ar/>

[2] TPM 2.0 Keys for
    Device Identity and Attestation, TCG. Version 1.00, Revision 12.
    <https://trustedcomputinggroup.org/resource/tpm-2-0-keys-for-device-identity-and-attestation/>

[3] RFC 5280 Internet X.509 Public Key Infrastructure Certificate and
    Certificate Revocation List (CRL) Profile, IETF.
    <https://datatracker.ietf.org/doc/html/rfc5280>

[4] TCG Platform Certificate Profile Specification Version 1.1,
    Revision 19.
    <https://trustedcomputinggroup.org/resource/tcg-platform-certificate-profile/>

## Identifier selection

DevIDs are required to identify devices uniquely even when built identically.
To accomplish this, a DevID certificate will typically use a combination of
device attributes that results in an identifier value that is globally unique.
A device serial number is such an identifier, often combined with a device
model or part number.

This specification covers a variety of device types, from standalone
platforms (e.g., rack-mounted servers, network switches, etc.), to appliance-
type devices (e.g., storage appliances), to systems composed from multiple
constituent devices (e.g., an HPC system composed of a number of blades).

This creates ambiguity in selecting the appropiate set of identifiers
(e.g., serial numbers, part numbers, etc.) to uniquely identify a particular device type.
For example, a server may have a serial number and part number assigned to the
main board hosting its main components and another serial number and SKU
assigned to the assembled chassis. In another example, a storage array may
consist of a number of blades, each with its own serial number and SKU, but
may carry a product-specific serial number and SKU covering the entire assembly.

In cases like these, it is not clear which set of identifiers should be used in
a particular DevID certificate. Compounding the problem, it may be necessary to switch
from one set of identifiers to another for the same device when issuing different
types of DevIDs (i.e., IDevID, factory-issued LDevID, field-issued LDevID).

To address this problem, this specification outlines the top-level requirements
for selecting the appropriate identifiers for each DevID type in the relevant sections
covering each DevID type (i.e., IDevID, factory-issued LDevID, field-issued LDevID)
and provides concrete guidance on how these requirements translate to specific
identifier selection for different types of devices and DevID types in Annex A.

## Factory-provisioned IDevID and IAK certificates

This section details the certificate format for IDevID and IAK, which
are issued by the CAs that are part of the “HPE Manufacturing PKI”,
rooted in the “HPE Common Devices Root” CA. Table 1 below summarizes the
values that a product team needs to decide when provisioning IDevID and
IAK certificates.

**Table 1: Summary of Subject fields assigned by the product teams.**

| **Field**            | **Value**                                                     |
|----------------------|---------------------------------------------------------------|
| Subject’s OU         | Name of the BU responsible for the product, or “HPE”          |
| Subject’s CN         | SKU or Product ID of the product (should be customer visible) |
| Subject’s SN         | Serial Number of the product (should be customer visible)     |
| Certificate Policies | Type of platform (e.g., server, storage), etc.                |

**Table 2: Summary of Subject fields whose values are fixed.**

| **Field**    | **Value**                              |
|--------------|----------------------------------------|
| Subject’s C  | US                                     |
| Subject’s ST | Texas                                  |
| Subject’s L  | Houston                                |
| Subject’s O  | Hewlett Packard Enterprise Development |

### Subject field

The Subject field must contain the identity information of the entity
(usually a platform, device, or component) in the form of a
Distinguished Name (DN). The DN is composed of the following Relative
Distinguished Names (RDNs), in no particular order:

#### Organization Name (O)

The organizationName attribute (abbreviated “O” and using the OID
“2.5.4.10”) MUST contain the name of the company: “Hewlett Packard
Enterprise Development”. It MUST NOT be changed.

#### Organization Unit Name (OU)

The organizationUnitName attribute (abbreviated “OU” and using the OID
“2.5.4.11”) contains the name of the business unit, for example:
“Storage”. It can be defined by the business unit responsible for the
product. To ensure that the value is meaningful to relying parties, the
business unit SHOULD coordinate with the *HPE Devices PKI Policy
committee*[^1]. The maximum string size MUST NOT exceed 16 characters,
to minimize Subject field length. If the business unit has not defined a
specific value, the default value of “HPE” MUST be used.

#### Country Name (C)

The countryName attribute (abbreviated “C” and using the OID “2.5.4.6”)
MUST contain the ISO 3166 alpha-2 code of the country hosting the parent
company “O”: “US”. It MUST NOT be changed.

#### State or Province Name (ST)

The stateOrProvinceName attribute (abbreviated “ST” and using the OID
“2.5.4.8”) MUST contain the name (not the abbreviation) of the state or
province hosting the parent company “O”: “Texas”. It MUST NOT be
changed.

#### Common Name (CN)

The commonName attribute (abbreviated “CN” and using the OID “2.5.4.3”)
MUST contain the SKU or Product/Model/Part number (PN) of the entity identified
by the certificate; for example, it can be a customer-orderable SKU:
“P39886-B21”. The following requirements apply:

- The pair commonName and serialNumber (described below) MUST uniquely
  identify the entity associated with the certificate.

- The commonName value SHOULD be a customer visible value; for example,
  it can be the SKU number assigned to the entity.

Each product group MUST determine which value to use using the information in Annex A.

#### Serial Number

The Subject serialNumber attribute (using the OID “2.5.4.5”) contains
the serial number of the entity identified by the certificate; for
example, it can be a customer-visible serial number: “MXQ1490VZK”.
The following requirements apply:

- The pair serialNumber and commonName (described above) MUST uniquely
  identify the entity associated with the certificate.

- The serialNumber value SHOULD be a customer visible value; for
  example, it can be the serial number listed on the label of a
  platform.

Each product group MUST determine which value to use using the information in Annex A.

##### MAC address

If the product has a fixed MAC address, it MUST be included in the
serialNumber attribute of the certificate. In this case, the
serialNumber string selected in 2.1.6 is concatenated with “, BaseMAC “
and the fixed MAC address using a 6-dash-6 format. For example, the
serialNumber attribute could contain the string “MXQ1490VZK, BaseMAC
AAAAAA-BBBBBB”. Note that for the MAC address to be included in the
serialNumber, the associated network interface MUST be integrated in the
entity identified by the serialNumber, i.e. not on a field-replaceable
unit (e.g. a NIC).

### Validity

An IDevID/IAK is expected to exist for as long as the host IDevID/IAK
module remains operational. Therefore, these certificates MUST have a
validity set as follows:

- notBefore: Date the certificate was issued

- notAfter: The GeneralizedTime value “99991231235959Z” which is used
  for “does not expire.”

### Certificate extensions

This section details the certificate extensions for IDevID and IAK
certificates. No other extensions are permitted without an update of
this specification. All IDevID and IAK certificate extensions are
non-critical, apart from the keyUsage extension which MUST be marked
critical.

#### Subject Key Identifier and Authority Key Identifier extensions

The subjectKeyIdentifier and authorityKeyIdentifier extensions (using
the OIDs “2.5.29.14” and “2.5.29.35” respectively) MUST be present in
the IDevID and IAK certificates. They are used to easily identify the
public key of the entity certificate or the issuing CA. The content of
those extensions is generated by the CA.

#### Basic Constraints extension

The basicConstraints extension MUST be absent. The IDevID and IAK
certificate are end-entity certificates, which means that they are not
allowed to sign a X.509 certificate.

#### Key Usage extension

The IDevID and IAK are signing keys, which can be used in TLS (IDevID),
or for signing a TPM Quote (IAK) for example. The keyUsage extension
(using the OID “2.5.29.15”) MUST have the digitalSignature bit set in
both IDevID and IAK and MAY have keyAgreement bit set in the IDevID
certificate only. Other keyUsage bits MUST NOT be set.

The keyUsage extension MUST be marked critical.

#### Subject Alternative Name extension

The subjectAltName (abbreviated SAN and using the OID “2.5.29.17”)
extension contains the Hardware Module Name and Permanent Identifier
otherNames, and may contain other values to express logical identifiers
associated with the DevID.

##### Hardware Module Name

The hardwareModuleName (“id-on-hardwareModuleName” using the OID
“1.3.6.1.5.5.7.8.4”) identifies the Root of Trust component that
protects the private key of the IDevID or IAK.

For a TPM 2.0-based IDevID or IAK, it is defined in [[2]](#references); the content
is generated by the issuing CA based on the Endorsement Key (EK)
certificate. Specifically, hwType: “2.23.133.1.2” (TPM 2.0) and
hwSerialNum: Concatenation of {TCG TPM Manufacturer code, EK Authority
Key Identifier and EK CertificateSerialNumber} separated by colon (“:”)
in this order. When the TPM is provisioned with both RSA and ECC
endorsement key certificates, the RSA EK Certificate serialNumber MUST
be used as the EK CertificateSerialNumber. Implementers MUST refer to
[[2]](#references) for the full definition of this field, including the
ASN.1 encoding.

For iLO 5-based IDevID or IAK, use hwType: “1.3.6.1.4.1.47196.6.3.2.1”
(iLO 5) and hwSerialNum: the serial number value used in the Subject
serialNumber RDN.

For iLO 6-based IDevID or IAK, use hwType: “1.3.6.1.4.1.47196.6.3.2.2”
(iLO 6) and hwSerialNum: the serial number value of the board the iLO
chip is attached to: for example, the serial number of the PCA in a
monolithic system, or the DC-SCM serial number in a modular system.
Depending on the system topology this may or may not be the same as the value
used in the Subject serialNumber RDN.

For iLO 7-based IDevID or IAK, use hwType: “1.3.6.1.4.1.47196.6.3.2.3”
(iLO 7) and hwSerialNum: the serial number value of the board the iLO
chip is attached to: for example, the serial number of the PCA in a
monolithic system, or the DC-SCM serial number in a modular system.
Depending on the system topology this may or may not be the same as the value
used in the Subject serialNumber RDN.

Future iLO versions may be assigned new OIDs[^2].

##### Permanent Identifier

The permanentIdentifier (“id-on-permanentIdentifier” with OID
“1.3.6.1.5.5.7.8.3”) identifies the TPM with the following two values:

- identifierValue: Digest of DER-encoded EK certificate in hexadecimal
  using the UTF8 character set.

- assigner: “2.23.133.12.1” (tcg-on-ekPermIdSha256).

Implementers MUST refer to [[2]](#references) for the full definition of this field,
including the ASN.1 encoding.

This only applies to TPM-resident credentials. For iLO-based IDevID and
IAK, permanent identifier MUST NOT be present.

##### directoryName attributes

For backwards compatibility with iLO firmware and use-cases, the
subjectAltName extension MAY contain directoryName elements with the
following contents:

1. OID “2.23.133.5.1.1” (“tcg-at-platformManufacturerStr”) with value
    “HPE”.

2. OID “2.23.133.5.1.4” (“tcg-at-platformModel”) with a UTF-8 encoded
    string representing the platform SKU or model name.

3. OID “2.23.133.5.1.6” (“tcg-at-platformSerial”) with a UTF-8 encoded
    string representing the platform (i.e. assembly/chassis) serial number.

The values assigned SHOULD match exactly those present in a Platform
Certificate issued for the same platform.

#### Certificate Policies extension

The certificatePolicies extension (using the OID “2.5.29.31”) contains a
set of OIDs, which indicate the type of platform and Root of Trust used.
This extension MUST be present, but MUST NOT be marked critical.

TCG specifies in [[2]](#references) industry standard OIDs for TPM2.0 IDevID and IAK.

In addition, HPE has defined a number of Policy OIDs to describe
particular device and provisioning attributes (see [HPE-defined OID values](#hpe-defined)). HPE
Policy OIDs have no associated value; their presence is a statement that
the device has a particular attribute or it was provisioned in a
particular way:

- **Type of platform**: HPE manages OIDs to identify the kind of entity
  the certificate represents; for example, “1.3.6.1.4.1.47196.6.2.1” for
  a generic server (compute), “1.3.6.1.4.1.47196.6.2.2” for a storage
  product. A list of assigned OIDs is available in [HPE-defined OID values](#hpe-defined). New
  platform types could need a new OID to be issued. The product team MUST
  work with the HPE Devices PKI steering committee to determine which
  OID to use.

- **Root of Trust**: HPE manages OIDs to identify the Root of Trust that
  protects the IDevID or IAK private key; for example,
  “1.3.6.1.4.1.47196.6.1.22” for iLO, “1.3.6.1.4.1.47196.6.1.21” for TPM
  2.0.

- **Type of certificate**: HPE manages OIDs to identify the type of
  certificate, such as IDevID/IAK (“1.3.6.1.4.1.47196.6.1.1”),
  factory-issued LDevID/LAK (“1.3.6.1.4.1.47196.6.1.3”) or field-issued
  LDevID/LAK (“1.3.6.1.4.1.47196.6.1.2”).

See tables below for details.

### Summary of certificate values

#### Table 3: TPM-Based IDevID

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 9%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 14%" />
<col style="width: 7%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="9"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="8">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="4"><strong>Use</strong></td>
<td colspan="3"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="4">Country</td>
<td colspan="3">“US”</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="4">State or Province</td>
<td colspan="3">“Texas”</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="4">Location</td>
<td colspan="3">“Houston”</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="4">Organization</td>
<td colspan="3">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="4">BU</td>
<td colspan="3">BU-defined, or “HPE” (see <a href="#organization-unit-name-ou">OU</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="4">Entity Part Number</td>
<td colspan="3">The permanent PN (see <a href="#common-name-cn">CN</a>)</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="4">Entity Serial Number</td>
<td colspan="3">The permanent SN (see <a href="#serial-number">serialNumber</a>)</td>
</tr>
<tr class="odd">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="6">Date of certificate creation.</td>
</tr>
<tr class="even">
<td colspan="3">notAfter</td>
<td colspan="6">“99991231235959Z” meaning “does not expire”</td>
</tr>
<tr class="odd">
<td colspan="2">Extensions</td>
<td colspan="8"></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="3"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="3"><strong>Type</strong></td>
<td colspan="5"><strong>Description</strong></td>
</tr>
<tr class="odd">
<td colspan="3">digitalSignature</td>
<td colspan="5">Public key can be used for verifying digital
signatures</td>
</tr>
<tr class="even">
<td colspan="3">keyAgreement</td>
<td colspan="5">Public key can be used for TLS 1.2 ECDH</td>
</tr>
<tr class="odd">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="8">Identifier of public key</td>
</tr>
<tr class="even">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="8">Identifier of parent CA key</td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="3"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td colspan="2"><strong>Content</strong></td>
</tr>
<tr class="even">
<td colspan="4">id-on-hardwareModuleName</td>
<td colspan="2">1.3.6.1.5.5.7.8.4</td>
<td colspan="2"><p>Description of TPM:</p>
<p>hwType: 2.23.133.1.2 (TPM 2.0).</p>
<p>hwSerialNumber: SN from the TPM EK certificate.</p></td>
</tr>
<tr class="odd">
<td colspan="4">id-on-permanentIdentifier</td>
<td colspan="2">1.3.6.1.5.5.7.8.3</td>
<td colspan="2"><p>Identifier of the TPM:</p>
<p>identifierValue: Digest of DER-encoded EK certificate in hex.</p>
<p>assigner: 2.23.133.12.1 </p></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="7"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="3"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="odd">
<td colspan="4">tcg-cap-verifiedTPMResidency</td>
<td colspan="3">2.23.133.11.1.1</td>
<td>TPM residency verified</td>
</tr>
<tr class="even">
<td colspan="4">tcg-cap-verifiedTPMFixed</td>
<td colspan="3">2.23.133.11.1.2</td>
<td>The fixedTPM attribute was verified</td>
</tr>
<tr class="odd">
<td colspan="4">id-HPE-DevID-CP-IDevID</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.1</td>
<td>This is an IDevID or IAK</td>
</tr>
<tr class="even">
<td colspan="4">id-HPE-DevID-CP-TPMProtected</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.21</td>
<td>TPM-protected</td>
</tr>
<tr class="even">
<td colspan="4"><p>id-HPE-DevID-CP-ptype-com</p>
<p>(example)</p></td>
<td colspan="3"><p>1.3.6.1.4.1.47196.6.2.1</p>
<p>(example)</p></td>
<td>Compute Server<a href="#fn1" class="footnote-ref" id="fnref1"
role="doc-noteref"><sup>1</sup></a>. See <a href="#hpe-defined">HPE-defined OID values</a> for other values.</td>
</tr>
</tbody>
</table>
<aside id="footnotes" class="footnotes footnotes-end-of-document"
role="doc-endnotes">
<hr />
<ol>
<li id="fn1"><p>This policy identifies the product type (e.g. compute
server, storage server, etc.). The certificate must contain only one
product type. See <a href="#hpe-defined">HPE-defined OID values</a> for available product type OIDs.<a
href="#fnref1" class="footnote-back" role="doc-backlink">↩︎</a></p></li>
</ol>
</aside>

#### Table 4: TPM-Based IAK

The TPM IAK MUST have the same Subject DN as the corresponding IDevID.

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 9%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 14%" />
<col style="width: 7%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="9"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="8">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="4"><strong>Use</strong></td>
<td colspan="3"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="4">Country</td>
<td colspan="3">“US”</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="4">State or Province</td>
<td colspan="3">“Texas”</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="4">Location</td>
<td colspan="3">“Houston”</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="4">Organization</td>
<td colspan="3">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="4">BU</td>
<td colspan="3">BU-defined, or “HPE” (see <a href="#organization-unit-name-ou">OU</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="4">Entity Part Number</td>
<td colspan="3">The permanent PN (see <a href="#common-name-cn">CN</a>)</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="4">Entity Serial Number</td>
<td colspan="3">The permanent SN (see <a href="#serial-number">serialNumber</a>)</td>
</tr>
<tr class="odd">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="6">Date of certificate creation.</td>
</tr>
<tr class="even">
<td colspan="3">notAfter</td>
<td colspan="6">“99991231235959Z” meaning “does not expire”</td>
</tr>
<tr class="odd">
<td colspan="2">Extensions</td>
<td colspan="8"></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="2"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="3"><strong>Type</strong></td>
<td colspan="5"><strong>Description</strong></td>
</tr>
<tr class="odd">
<td colspan="3">digitalSignature</td>
<td colspan="5">Public key can be used for verifying digital
signatures</td>
</tr>
<tr class="even">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="8">Identifier of public key</td>
</tr>
<tr class="odd">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="8">Identifier of parent CA key</td>
</tr>
<tr class="even">
<td colspan="2" rowspan="3"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td colspan="2"><strong>Content</strong></td>
</tr>
<tr class="odd">
<td colspan="4">id-on-hardwareModuleName</td>
<td colspan="2">1.3.6.1.5.5.7.8.4</td>
<td colspan="2"><p>Description of TPM:</p>
<p>hwType: 2.23.133.1.2 (TPM 2.0).</p>
<p>hwSerialNumber: SN from the TPM EK certificate.</p></td>
</tr>
<tr class="even">
<td colspan="4">id-on-permanentIdentifier</td>
<td colspan="2">1.3.6.1.5.5.7.8.3</td>
<td colspan="2"><p>Identifier of the TPM:</p>
<p>identifierValue: Digest of DER-encoded EK certificate in hex.</p>
<p>assigner: 2.23.133.12.1 </p></td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="7"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="3"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="even">
<td colspan="4">tcg-cap-verifiedTPMResidency</td>
<td colspan="3">2.23.133.11.1.1</td>
<td>TPM residency verified</td>
</tr>
<tr class="odd">
<td colspan="4">tcg-cap-verifiedTPMRestricted</td>
<td colspan="3">2.23.133.11.1.3</td>
<td>The fixedTPM and restricted attributes were verified<a href="#fn1"
class="footnote-ref" id="fnref1"
role="doc-noteref"><sup>1</sup></a></td>
</tr>
<tr class="even">
<td colspan="4">id-HPE-DevID-CP-IDevID</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.1</td>
<td>This is an IDevID/IAK</td>
</tr>
<tr class="odd">
<td colspan="4">id-HPE-DevID-CP-TPMProtected</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.21</td>
<td>TPM-protected</td>
</tr>
<tr class="even">
<td colspan="4"><p>id-HPE-DevID-CP-ptype-sto</p>
<p>(example)</p></td>
<td colspan="3"><p>1.3.6.1.4.1.47196.6.2.2</p>
<p>(example)</p></td>
<td>Storage Server. See <a href="#hpe-defined">HPE-defined OID values</a>.</td>
</tr>
</tbody>
</table>
<aside id="footnotes" class="footnotes footnotes-end-of-document"
role="doc-endnotes">
<hr />
<ol>
<li id="fn1"><p>The IAK will have the tcg-cap-verifiedTPMRestricted
policy OID (2.23.133.11.1.3) rather than tcg-cap-verifiedTPMFixed of the
IDevID.<a href="#fnref1" class="footnote-back"
role="doc-backlink">↩︎</a></p></li>
</ol>
</aside>

#### Table 5: iLO-Based IDevID

<table style="width:100%;">
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 9%" />
<col style="width: 3%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="9"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="8">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="4"><strong>Use</strong></td>
<td colspan="3"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="4">Country</td>
<td colspan="3">“US”</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="4">State or Province</td>
<td colspan="3">“Texas”</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="4">Location</td>
<td colspan="3">“Houston”</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="4">Organization</td>
<td colspan="3">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="4">BU</td>
<td colspan="3">BU-defined, or “HPE” (see <a href="#organization-unit-name-ou">OU</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="4">Entity Part Number</td>
<td colspan="3">The permanent PN (see <a href="#common-name-cn">CN</a>)</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="4">Entity Serial Number</td>
<td colspan="3">The permanent SN (see <a href="#serial-number">serialNumber</a>)</td>
</tr>
<tr class="odd">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="6">Date of certificate creation.</td>
</tr>
<tr class="even">
<td colspan="3">notAfter</td>
<td colspan="6">“99991231235959Z” meaning “does not expire”</td>
</tr>
<tr class="odd">
<td colspan="2">Extensions</td>
<td colspan="8"></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="3"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="3"><strong>Type</strong></td>
<td colspan="5"><strong>Description</strong></td>
</tr>
<tr class="odd">
<td colspan="3">digitalSignature</td>
<td colspan="5">Public key can be used for verifying digital
signatures</td>
</tr>
<tr class="even">
<td colspan="3">keyAgreement</td>
<td colspan="5">Public key can be used for TLS 1.2 ECDH</td>
</tr>
<tr class="odd">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="8">Identifier of public key</td>
</tr>
<tr class="even">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="8">Identifier of parent CA key</td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="2"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td colspan="2"><strong>Content</strong></td>
</tr>
<tr class="even">
<td colspan="4">id-on-hardwareModuleName</td>
<td colspan="2">1.3.6.1.5.5.7.8.4</td>
<td colspan="2"><p>Description of iLO ASIC:</p>
<p>See section 2.3.4.1.</p></td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="5"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="3"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="even">
<td colspan="4">id-HPE-DevID-CP-IDevID</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.1</td>
<td>This is an IDevID/IAK</td>
</tr>
<tr class="odd">
<td colspan="4">id-HPE-DevID-CP-iLOProtected</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.22</td>
<td>iLO-protected</td>
</tr>
<tr class="even">
<td colspan="4"><p>id-HPE-DevID-CP-ptype-com</p>
<p>(example)</p></td>
<td colspan="3"><p>1.3.6.1.4.1.47196.6.2.1</p>
<p>(example)</p></td>
<td>Generic Server. See <a href="#hpe-defined">HPE-defined OID values</a>.</td>
</tr>
</tbody>
</table>

## Factory-provisioned LDevID and LAK certificates

This section details the certificate format for “System” LDevID and LAK,
which are factory-provisioned, permanent certificates.

System LDevIDs
are provisioned by HPE at the factory to provide a long-lived credential
for devices which already have an IDevID provisioned, and may contain
information representing a transformation or composition that took place
at the factory (e.g. a chassis/system serial number) and was not known
at the time of IDevID provisioning. An example is a server with an IDevID
holding the serial number of the motherboard/PCA and a System LDevID
holding the serial number of the assembled chassis. A System LDevID
enables composed systems to carry an identity that represents the entire
system without removing the identity of a constituent device. This is
important for reconfiguration and repair scenarios.

System LDevIDs are issued by the CAs that are part of the “HPE Manufacturing
PKI”, rooted in the “HPE Common Devices Root” CA.

System LDevIDs are for all intents and purposes equivalent to IDevIDs
and contain the same fields as IDevIDs, with the exception of the Policy
OIDs identifying them as System LDevIDs, two optional additional RDNs in
Subject and a different value in the hardwareModuleName SAN for iLO-bound credentials.

Annex A contains guidance for which identifiers to be used in the Subject
CN and serialNumber fields of a System LDevID/LAK.

### Additional Subject RDNs used in System LDevID/LAK

#### givenName (GN)

If populated, RDN givenName (GN) (OID "2.5.4.42") MUST be used to mirror the value
used in the Subject CN RDN in the corresponding IDevID/IAK issued to the primary
component of the system.

Consult Annex A for details and to confirm if GN is populated for a particular product.

#### surName (SN)

If populated, RDN surName (SN) (OID "2.5.4.4") MUST be used to mirror the value
used in the Subject serialNumber RDN in the corresponding IDevID/IAK issued to the primary
component of the system.

Consult Annex A for details and to confirm if GN is populated for a particular product.

### Hardware Module Name and Permanent Identifier

The hardwareModuleName (“id-on-hardwareModuleName” - OID
“1.3.6.1.5.5.7.8.4”) and permanentIdentifier (“id-on-permanentIdentifier” - OID
“1.3.6.1.5.5.7.8.3”) otherNames in the subjectAltName of a System LDevID/LAK MUST
be populated according to the rules below:

For TPM-bound System LDevID/LAK, the values in hardwareModuleName and permanentIdentifier
MUST be identical to those in the corresponding IDevID/IAK.

For iLO-bound System LDevID/LAK, the values in hardwareModuleName MUST identify the
the board the iLO chip is attached to. Specifically, hwType: “1.3.6.1.4.1.47196.6.3.2.2”
(iLO 6) or hwType: “1.3.6.1.4.1.47196.6.3.2.3”
(iLO 7) and hwSerialNum: the serial number value of the board the iLO
chip is attached to: for example, the serial number of the PCA in a
monolithic system, or the DC-SCM serial number in a modular system.

iLO-bound System LDevID/LAK MUST NOT use a permanentIdentifier.

Future iLO versions MAY be assigned new OIDs[^2]. The hwType OID MUST be set accordingly.

### Summary of certificate values

#### Table 6: TPM-Based System LDevID

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 9%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 14%" />
<col style="width: 7%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="11"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="10">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="4"><strong>Use</strong></td>
<td colspan="3"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="4">Country</td>
<td colspan="3">“US”</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="4">State or Province</td>
<td colspan="3">“Texas”</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="4">Location</td>
<td colspan="3">“Houston”</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="4">Organization</td>
<td colspan="3">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="4">BU</td>
<td colspan="3">BU-defined, or “HPE” (see <a href="#organization-unit-name-ou">OU</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="4">Entity Part Number</td>
<td colspan="3">The permanent PN (see <a href="#common-name-cn">CN</a>)</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="4">Entity Serial Number</td>
<td colspan="3">The permanent SN (see <a href="#serial-number">serialNumber</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">GN</td>
<td colspan="4">Primary Component Part Number</td>
<td colspan="3">The PN used in the IDevID</td>
</tr>
<tr class="even">
<td colspan="2">SN</td>
<td colspan="4">Primary Component Serial Number</td>
<td colspan="3">The serialNumber used in the IDevID</td>
</tr>
<tr class="odd">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="6">Date of certificate creation.</td>
</tr>
<tr class="even">
<td colspan="3">notAfter</td>
<td colspan="6">“99991231235959Z” meaning “does not expire”</td>
</tr>
<tr class="odd">
<td colspan="2">Extensions</td>
<td colspan="8"></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="3"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="3"><strong>Type</strong></td>
<td colspan="5"><strong>Description</strong></td>
</tr>
<tr class="odd">
<td colspan="3">digitalSignature</td>
<td colspan="5">Public key can be used for verifying digital
signatures</td>
</tr>
<tr class="even">
<td colspan="3">keyAgreement</td>
<td colspan="5">Public key can be used for TLS 1.2 ECDH</td>
</tr>
<tr class="odd">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="8">Identifier of public key</td>
</tr>
<tr class="even">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="8">Identifier of parent CA key</td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="3"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td colspan="2"><strong>Content</strong></td>
</tr>
<tr class="even">
<td colspan="4">id-on-hardwareModuleName</td>
<td colspan="2">1.3.6.1.5.5.7.8.4</td>
<td colspan="2"><p>Description of TPM:</p>
<p>hwType: 2.23.133.1.2 (TPM 2.0).</p>
<p>hwSerialNumber: SN from the TPM EK certificate.</p></td>
</tr>
<tr class="odd">
<td colspan="4">id-on-permanentIdentifier</td>
<td colspan="2">1.3.6.1.5.5.7.8.3</td>
<td colspan="2"><p>Identifier of the TPM:</p>
<p>identifierValue: Digest of DER-encoded EK certificate in hex.</p>
<p>assigner: 2.23.133.12.1 </p></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="7"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="3"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="odd">
<td colspan="4">tcg-cap-verifiedTPMResidency</td>
<td colspan="3">2.23.133.11.1.1</td>
<td>TPM residency verified</td>
</tr>
<tr class="even">
<td colspan="4">tcg-cap-verifiedTPMFixed</td>
<td colspan="3">2.23.133.11.1.2</td>
<td>The fixedTPM attribute was verified</td>
</tr>
<tr class="odd">
<td colspan="4">id-HPE-DevID-CP-SystemLDevID</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.3</td>
<td>This is a System LDevID or IAK</td>
</tr>
<tr class="even">
<td colspan="4">id-HPE-DevID-CP-TPMProtected</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.21</td>
<td>TPM-protected</td>
</tr>
<tr class="odd">
<td colspan="4"><p>id-HPE-DevID-CP-ptype-sto</p>
<p>(example)</p></td>
<td colspan="3"><p>1.3.6.1.4.1.47196.6.2.2</p>
<p>(example)</p></td>
<td>Storage Server. See <a href="#hpe-defined">HPE-defined OID values</a>.</td>
</tr>
</tbody>
</table>

#### Table 7: TPM-Based System LAK

The TPM System LAK MUST have the same Subject DN as the corresponding
System LDevID.

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 9%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 14%" />
<col style="width: 7%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="11"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="10">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="4"><strong>Use</strong></td>
<td colspan="3"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="4">Country</td>
<td colspan="3">“US”</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="4">State or Province</td>
<td colspan="3">“Texas”</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="4">Location</td>
<td colspan="3">“Houston”</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="4">Organization</td>
<td colspan="3">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="4">BU</td>
<td colspan="3">BU-defined or “HPE” (see <a href="#organization-unit-name-ou">OU</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="4">Entity Part Number</td>
<td colspan="3">The permanent PN (see <a href="#common-name-cn">CN</a>)</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="4">Entity Serial Number</td>
<td colspan="3">The permanent SN (see <a href="#serial-number">serialNumber</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">GN</td>
<td colspan="4">Primary Component Part Number</td>
<td colspan="3">The PN used in the IDevID</td>
</tr>
<tr class="even">
<td colspan="2">SN</td>
<td colspan="4">Primary Component Serial Number</td>
<td colspan="3">The serialNumber used in the IDevID</td>
</tr>
<tr class="odd">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="6">Date of certificate creation.</td>
</tr>
<tr class="even">
<td colspan="3">notAfter</td>
<td colspan="6">“99991231235959Z” meaning “does not expire”</td>
</tr>
<tr class="odd">
<td colspan="2">Extensions</td>
<td colspan="8"></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="2"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="3"><strong>Type</strong></td>
<td colspan="5"><strong>Description</strong></td>
</tr>
<tr class="odd">
<td colspan="3">digitalSignature</td>
<td colspan="5">Public key can be used for verifying digital
signatures</td>
</tr>
<tr class="even">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="8">Identifier of public key</td>
</tr>
<tr class="odd">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="8">Identifier of parent CA key</td>
</tr>
<tr class="even">
<td colspan="2" rowspan="3"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td colspan="2"><strong>Content</strong></td>
</tr>
<tr class="odd">
<td colspan="4">id-on-hardwareModuleName</td>
<td colspan="2">1.3.6.1.5.5.7.8.4</td>
<td colspan="2"><p>Description of TPM:</p>
<p>hwType: 2.23.133.1.2 (TPM 2.0).</p>
<p>hwSerialNumber: SN from the TPM EK certificate.</p></td>
</tr>
<tr class="even">
<td colspan="4">id-on-permanentIdentifier</td>
<td colspan="2">1.3.6.1.5.5.7.8.3</td>
<td colspan="2"><p>Identifier of the TPM:</p>
<p>identifierValue: Digest of DER-encoded EK certificate in hex.</p>
<p>assigner: 2.23.133.12.1 </p></td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="7"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="3"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="even">
<td colspan="4">tcg-cap-verifiedTPMResidency</td>
<td colspan="3">2.23.133.11.1.1</td>
<td>TPM residency verified</td>
</tr>
<tr class="odd">
<td colspan="4">tcg-cap-verifiedTPMRestricted</td>
<td colspan="3">2.23.133.11.1.3</td>
<td>The fixedTPM and restricted attributes were verified<a href="#fn1"
class="footnote-ref" id="fnref1"
role="doc-noteref"><sup>1</sup></a></td>
</tr>
<tr class="even">
<td colspan="4">id-HPE-DevID-CP-SystemLDevID</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.3</td>
<td>This is a System LDevID/LAK</td>
</tr>
<tr class="odd">
<td colspan="4">id-HPE-DevID-CP-TPMProtected</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.21</td>
<td>TPM-protected</td>
</tr>
<tr class="even">
<td colspan="4"><p>id-HPE-DevID-CP-ptype-com</p>
<p>(example)</p></td>
<td colspan="3"><p>1.3.6.1.4.1.47196.6.2.1</p>
<p>(example)</p></td>
<td>Generic Server. See <a href="#hpe-defined">HPE-defined OID values</a>.</td>
</tr>
</tbody>
</table>
<aside id="footnotes" class="footnotes footnotes-end-of-document"
role="doc-endnotes">
<hr />
<ol>
<li id="fn1"><p>The IAK will have the tcg-cap-verifiedTPMRestricted
policy OID (2.23.133.11.1.3) rather than tcg-cap-verifiedTPMFixed of the
IDevID.<a href="#fnref1" class="footnote-back"
role="doc-backlink">↩︎</a></p></li>
</ol>
</aside>

#### Table 8: iLO-Based System LDevID

<table style="width:100%;">
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 9%" />
<col style="width: 3%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="11"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="10">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="4"><strong>Use</strong></td>
<td colspan="3"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="4">Country</td>
<td colspan="3">“US”</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="4">State or Province</td>
<td colspan="3">“Texas”</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="4">Location</td>
<td colspan="3">“Houston”</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="4">Organization</td>
<td colspan="3">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="4">BU</td>
<td colspan="3">BU-defined or “HPE” (see <a href="#organization-unit-name-ou">OU</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="4">Entity Part Number</td>
<td colspan="3">The permanent PN (see <a href="#common-name-cn">CN</a>)</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="4">Entity Serial Number</td>
<td colspan="3">The permanent SN (see <a href="#serial-number">serialNumber</a>)</td>
</tr>
<tr class="odd">
<td colspan="2">GN</td>
<td colspan="4">Primary Component Part Number</td>
<td colspan="3">The PN used in the IDevID</td>
</tr>
<tr class="even">
<td colspan="2">SN</td>
<td colspan="4">Primary Component Serial Number</td>
<td colspan="3">The serialNumber used in the IDevID</td>
</tr>
<tr class="odd">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="6">Date of certificate creation.</td>
</tr>
<tr class="even">
<td colspan="3">notAfter</td>
<td colspan="6">“99991231235959Z” meaning “does not expire”</td>
</tr>
<tr class="odd">
<td colspan="2">Extensions</td>
<td colspan="8"></td>
</tr>
<tr class="even">
<td colspan="2" rowspan="3"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="3"><strong>Type</strong></td>
<td colspan="5"><strong>Description</strong></td>
</tr>
<tr class="odd">
<td colspan="3">digitalSignature</td>
<td colspan="5">Public key can be used for verifying digital
signatures</td>
</tr>
<tr class="even">
<td colspan="3">keyAgreement</td>
<td colspan="5">Public key can be used for TLS 1.2 ECDH</td>
</tr>
<tr class="odd">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="8">Identifier of public key</td>
</tr>
<tr class="even">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="8">Identifier of parent CA key</td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="2"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td colspan="2"><strong>Content</strong></td>
</tr>
<tr class="even">
<td colspan="4">id-on-hardwareModuleName</td>
<td colspan="2">1.3.6.1.5.5.7.8.4</td>
<td colspan="2"><p>Description of iLO ASIC:</p>
<p>See <a href="#hardware-module-name">Hardware Module Name</a>.</p></td>
</tr>
<tr class="odd">
<td colspan="2" rowspan="5"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="4"><strong>Name</strong></td>
<td colspan="3"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="even">
<td colspan="4">id-HPE-DevID-CP-SystemLDevID</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.3</td>
<td>This is a System LDevID/LAK</td>
</tr>
<tr class="odd">
<td colspan="4">id-HPE-DevID-CP-iLOProtected</td>
<td colspan="3">1.3.6.1.4.1.47196.6.1.22</td>
<td>iLO-protected</td>
</tr>
<tr class="even">
<td colspan="4"><p>id-HPE-DevID-CP-ptype-com</p>
<p>(example)</p></td>
<td colspan="3">1.3.6.1.4.1.47196.6.2.1 (example)</td>
<td>Generic Server. See <a href="#hpe-defined">HPE-defined OID values</a>.</td>
</tr>
</tbody>
</table>

## Field-provisioned LDevID and LAK certificates

This section details the certificate format for LDevID and LAK, which
are issued by the CAs that are part of the “HPE in-field PKI”, rooted in
the “HPE Managed Devices Root” CA. Field-provisioned certificates can be
issued with a linkage back to an IDevID or IAK, or no linkage for use
cases such as LDevID issued on a system without an IDevID/IAK.

Field-provisioned LDevIDs/LAKs are typically not long-lived (as opposed
to IDevIDs & System LDevIDs), with typical lifetimes of a few months or
one year, and may be issued for a variety of purposes.

### LDevID and LAK linked to IDevID or IAK

This section covers the certificate format for LDevID and LAK that are
issued with a provable linkage to an HPE-issued IDevID or IAK, which
have a certificate from the “HPE manufacturing PKI” (i.e., a valid
certificate chain anchored in the “HPE Common Devices Root” CA). The
linkage proof MUST be reviewed and approved by the HPE Devices PKI
Policy committee.

#### Subject field

All Subject field RDNs that are common between an IDevID/IAK and an
LDevID/LAK MUST contain identical values to those of the IDevID or IAK
certificate used for the linkage proof, with the exception of the CN and
serialNumber RDNs. This means that all RDN attributes present in the
IDevID or IAK certificate apart from CN and serialNumber MUST be copied,
as-is, to the LDevID or LAK certificate. In other words, the LDevID/LAK
Subject DN will be a superset of the IDevID/IAK Subject DN.

The serialNumber RDN MAY be used in the LDevID/LAK to hold the *logical*
identity of the device (e.g. the HPE 10-digit Product SN) instead of the
*permanent* identity of the primary device component (e.g. the L6 or
MOD1 Assembly SN). Similarly, the CN RDN MAY be used in the LDevID/LAK
to hold the logical identity of the device (e.g. the Product PN or SKU)
instead of the actual component PN.

If the device has a System LDevID/LAK provisioned with changes to the
Subject CN and serialNumber already applied when compared to the
corresponding values in the IDevID/IAK (see [Factory-provisioned LDevID and LAK certificates](#factory-provisioned-ldevid-and-lak-certificates)),
then the following RDNs MAY be copied verbatim from the System LDevID/LAK to the
field-provisioned LDevID/LAK: CN, serialNumber, GN (if populated in System LDevID), SN (if populated in System LDevID).

Annex A provides guidance on which identifiers must be used in field-
issued LDevIDs/LAKs for different product types.

In addition, a field-provisioned LDevID/LAK MAY add additional RDNs in
the Subject DN. HPE Remote Device Access (RDA) has specified
the following RDNs: *Pseudonym*, *title* and *generationQualifier*. For
RDA-issued LDevIDs these three RDNs are REQUIRED.

##### Pseudonym

The pseudonym RDN (OID “2.5.4.65”) is used to hold an alternate name for
a device. RDA uses this field for the Station Identifier: the primary
RDA identifier of a product.

When the LDevID is anchored to an IDevID, the Station ID is the DeviceID
String – composed from the serialNumber, the Part Number (SKU) from the
CN field, the fixed string “\_devid”, and the Manufacturer ID as
"s*n*.*pn*.\_devid.*mfgid*". The country variant in the part number is
omitted (the country variant is the hash/pound-sign and characters
after). The manufacturer ID is usually “hpe”.

The pseudonym RDN is REQUIRED for RDA-issued LDevIDs/LAKs. RDA format is
PrintableString, max 48 chars, lowercase when displayed,
case-insensitive on compare.

Example:

    2ua512334p.c3r02uc._devid.hpe

(IDevID anchored, with serialNumber=2UA512334P and CN=C3R02UC#ABA (the \#ABA is not used)).

##### Title (T)

The title RDN (abbreviated “T” and using the OID “2.5.4.12”) is used to
identify the product in general (not the instance). The format is a
compound identifier of three parts -- ptype:pclass:pline – where:

- ptype is the string representation of the product type (see Product
  Type OID below). Required sub-field.

- pclass is the product class (also called platform family), max 16
  chars. Required sub-field.

  - It must be unique regardless of the product type (eg.
    compute:myproduct and storage:myproduct are not unique, only one use
    of "myproduct" is permitted).

- pline is the product line, max 16 chars. Optional sub-field.

The product identifier string is case insensitive on compare and
displayed in lowercase.

Example:

    T=compute:proliant:dl380

While Product type, class and line are encoded in the Subject to assist
with use-cases where software only looks up the Subject field of a
certificate, the same information SHOULD be encoded in the appropriate
elements of the certificatePolicies and subjectAltName extensions. See
[HPE-defined OID values](#hpe-defined) for OID assignment details.

Specifically:

- ptype MUST match the Policy OID used to identify the device Product
  Type.

  - For example, if the device is a generic server, the Policy OID is
    “1.3.6.1.4.1.47196.6.2.1” and the ptype is “compute”. See the
    1.3.6.1.4.1.47196.6.2.\* arc in [HPE-defined OID values](#hpe-defined) for assignment details.

- pclass MUST match the value in SAN OID
  id-HPE-DevID-SAN-platformFamily.

  - For example, if the device is a ProLiant DL360, the pclass will be
    “proliant” and the SAN will contain an otherName with type-id
    id-HPE-DevID-SAN-platformFamily (“1.3.6.1.4.1.47196.6.3.1.4”) and
    value “proliant”.

- pline MUST match the value in SAN OID id-HPE-DevID-SAN-productLine.

  - For example, if the device is a ProLiant DL360, the pline will be
    “dl360” and the SAN will contain an otherName with type-id
    id-HPE-DevID-SAN-productLine (“1.3.6.1.4.1.47196.6.3.1.7”) and value
    “dl360”.

REQUIRED for RDA. RDA format is PrintableString (preferred) or
UTF8String, max 64 chars.

##### generationQualifier

The generationQualifier RDN (using the OID “2.5.4.44”) is used by RDA to
specify the generation of the certificate's format (G2, G3, ...). This
defines the interpretation of various fields in the certificate.

For this version of the specification, this MUST be set to “G3”.

REQUIRED for RDA. RDA format is PrintableString (preferred) or
UTF8String, max 3 characters.

#### Subject Alternative Name extension

The hardwareModuleName otherName in the SAN of the LDevID and LAK
certificates MUST be copied from the IDevID or IAK certificate. The
permanentIdentifier field MAY be included for TPM-resident certificates.
In this case, the permanentIdentifier MAY include only the
identifierValue field and omit the assigner field. This indicates the
local nature of the certificate and avoids the need for a local CA to
obtain an assigner OID, as per [[2]](#references).

In addition, the SAN MAY hold one or more otherName or directoryName
elements as discussed in [Subject Alternative Name extension - common fields](#subject-alternative-name-extension-2).

#### Certificate Policies extension

A field-provisioned LDevID/LAK is identified by the
“1.3.6.1.4.1.47196.6.1.2” Policy OID. Depending on the linkage proof
between the IDevID/IAK and the LDevID/LAK, the appropriate HPE Policy OIDs must
be included in the LDevID or LAK certificate. For example,
“1.3.6.1.4.1.47196.6.1.21” can be used for a TPM2.0-protected LDevID or
LAK certificate when the LDevID or LAK has been proved to be co-located
with the HPE IAK (and the appropriate linkage proof method has been
used, as reviewed by the HPE PKI steering committee).

See [HPE-defined OID values](#hpe-defined) for the full list of applicable HPE OIDs.

The TCG OIDs used depend on the type of the Local key (i.e., LDevID or
LAK), and are defined in [[2]](#references).

### LDevID and LAK without IDevID or IAK

LDevID and LAK certificates may be issued on devices without an IDevID
or IAK (for example devices without a TPM or iLO, or virtual machines).
In this case the following requirements apply:

#### Subject field

##### Common Name (CN)

The Common Name (CN) field is used for the part number or SKU of a
product.

REQUIRED field. PrintableString, max 64 chars, case-insensitive for
comparison, displayed in uppercase.

Example:

    C3R02UC

##### serialNumber

The serialNumber field contains the manufacturer-issued serial number of
this instance of the product. It is encoded in string form even if it's
a whole number.

For devices with integrated network interfaces, the MAC address MAY be
included in the string value, as per the rules defined in [MAC address](#mac-address).

REQUIRED field. PrintableString, max 64 chars, case-insensitive for
comparison, displayed in uppercase.

##### Country (C)

Set to the ISO 3166 alpha-2 country code for the country where this
product exists - used for data provenance purposes. The registration
authority (RA) is expected to obtain and validate this information
before requesting the certificate from the CA. The RA will likely
provide this as "trusted override data" in the request since
modification of the original CSR would break the signature.

RECOMMENDED for RDA-issued certificates. UTF8String (preferred) or
PrintableString, max 2 chars, case-insensitive for comparison, displayed
in uppercase.

##### State or Province Name (ST)

The State or Province name for the physical location of this product -
used for data provenance purposes. The registration authority (RA) is
expected to obtain and validate this information before requesting the
certificate from the CA. The RA will likely provide this as "trusted
override data" in the request since modification of the original CSR
would break the signature.

OPTIONAL for RDA-issued certificates. UTF8String (preferred) or
PrintableString, max 128 chars, case-insensitive for comparison,
displayed in Title Case.

##### Locality (L)

The locality (city, region, state, province, or other geopolitical
boundary name) for the physical location of this product. It is most
often used as the customer's Site Name, e.g. North Widget Factory Two.

OPTIONAL for RDA-issued certificates. UTF8String (preferred) or
PrintableString, max 128 chars, case-insensitive for comparison,
displayed as given.

##### Organization Name (O)

The Organization RDN is used for the company or organization name of the
Original Equipment Manufacturer (OEM) of this product. For
non-IDevID-anchored HPE products, this string MUST be "Hewlett Packard
Enterprise Development".

OPTIONAL for RDA-issued certificates. UTF8String (preferred) or
PrintableString, max 64 chars, case-insensitive for comparison,
displayed as given.

##### Organization Unit Name (OU)

The Organization Unit RDN is used for the sub-unit within the Original
Equipment Manufacturer that produced this product. For
non-IDevID-anchored HPE products, this string MUST be omitted.

##### Pseudonym

The pseudonym RDN (OID “2.5.4.65”) is used to hold an alternate name for
a device. RDA uses this field for the Station Identifier: the primary
RDA ID of a product.

For non-IDevID-anchored HPE products, the Station ID format varies by
station role, but is most often a level-4 UUID.

The pseudonym RDN is REQUIRED for RDA-issued LDevIDs/LAKs. RDA format is
PrintableString, max 48 chars, lowercase.

Example:

    7c88c204-892d-49e3-ac60-6060c4c4d24a (UUID assigned by the CA)

##### Title (T)

The title RDN (abbreviated “T” and using the OID “2.5.4.12”) is used to
identify a product (not the instance, but the product in general). The
format is a compound identifier of three parts -- ptype:pclass:pline –
where:

- ptype is the string representation of the product type (see Product
  Type OID below). Required sub-field.

- pclass is the product class, max 16 chars. Required sub-field.

  - It must be unique regardless of the product type (eg.
    compute:myproduct and storage:myproduct are not unique, only one use
    of "myproduct" is permitted).

- pline is the product line, max 16 chars. Optional sub-field.

The product identifier string is case insensitive on compare and
displayed in lowercase.

Example:

    T=compute:proliant:dl380

While Product type, class and line are encoded in the Subject to assist
with use-cases where software only looks up the Subject field of a
certificate, the same information SHOULD be encoded in the appropriate
elements of the certificatePolicies and subjectAltName extensions. See
[HPE-defined OID values](#hpe-defined) for OID assignment details.

Specifically:

- ptype MUST match the Policy OID used to identify the device Product
  Type.

  - For example, if the device is a generic server, the Policy OID is
    “1.3.6.1.4.1.47196.6.2.1” and the ptype is “compute”. See the
    1.3.6.1.4.1.47196.6.2.\* arc in [HPE-defined OID values](#hpe-defined) for assignment details.

- pclass MUST match the value in SAN OID
  id-HPE-DevID-SAN-platformFamily.

  - For example, if the device is a ProLiant DL360, the pclass will be
    “proliant” and the SAN will contain an otherName with type-id
    id-HPE-DevID-SAN-platformFamily (“1.3.6.1.4.1.47196.6.3.1.4”) and
    value “proliant”.

- pline MUST match the value in SAN OID id-HPE-DevID-SAN-productLine.

  - For example, if the device is a ProLiant DL360, the pline will be
    “dl360” and the SAN will contain an otherName with type-id
    id-HPE-DevID-SAN-productLine (“1.3.6.1.4.1.47196.6.3.1.7”) and value
    “dl360”.

REQUIRED for RDA-issued certificates. RDA format is PrintableString
(preferred) or UTF8String, max 64 chars.

##### generationQualifier

The generationQualifier RDN (using the OID “2.5.4.44”) is used by RDA to
specify the generation of the certificate's format (G2, G3, ...). This
defines the interpretation of various fields in the certificate.

This MUST be set to “G3”.

REQUIRED for RDA-issued certificates. RDA format is PrintableString
(preferred) or UTF8String, max 3 characters.

### Common LDevID and LAK certificate fields and extensions

This section details the certificate fields and extensions that are
common to all field-provisioned LDevID and LAK certificates, regardless of their provable
linkage to an existing IDevID or IAK.

All certificate extensions MUST be marked non-critical, apart from the
keyUsage extension which MUST be marked critical. The inclusion of
extensions not listed in this specification can introduce
interoperability problems. If a new extension which is not listed in
this document is required, the requester MUST review the request with,
and get approval from, the HPE PKI steering committee.

#### Validity

This specification imposes no requirement on the validity period of an
LDevID/LAK. It is recommended that field-provisioned LDevIDs/LAKs are
set with short validity periods to necessitate a reissue (triggering the
appropriate reexamination of the target device’s attributes and
entitlements). Requirements on the exact lifetimes of LDevIDs/LAKs may
be mandated by other HPE specifications.

#### Subject Key Identifier and Authority Key Identifier extensions

Analogously to the IDevID and IAK certificates, the subjectKeyIdentifier
and authorityKeyIdentifier extensions are present in the LDevID and LAK
certificates. They are used to easily identify the public key of the
entity certificate or the issuing CA. The content of those extensions is
generated by the CA.

#### Basic Constraints extension

The Basic Constraints extension MUST be absent.

#### Key Usage extension

The keyUsage extension reflects the purpose of the LDevID and LAK. The
keyCertSign and cRLSign bits MUST be clear (i.e., a LDevID or LAK cannot
sign a certificate). Most use cases only require digitalSignature or
keyAgreement. For other Key Usage bits, the product team MUST consult
with the HPE PKI steering committee.

The keyUsage extension MUST be marked critical.

#### Extended Key Usage extension

The extendedKeyUsage extension (using the OID “2.5.29.37”) can be used
to further restrict the use of the LDevID and LAK keys, as the scope
defined by keyUsage can be broad. It is not required in a LDevID or LAK
certificate. It is up to the product or service team to decide if they
need it and which purpose(s) to include, based on their use cases. Most
commonly, id-kp-serverAuth (OID “2.5.29.37.1”) and id-kp-clientAuth (OID
“2.5.29.37.2”) are used for TLS server and client authentication
respectively.

Only one of TLS Web Client Authentication or TLS Web Server
Authentication OIDs MUST be present in a single certificate. If a team
needs to support both usages within the same certificate, it MUST
consult with the HPE PKI steering committee.

#### CRL Distribution Points extension

The cRLDistributionPoints extension is used to identify the location of
the Certificate Revocation List (CRL) for this issuer.

RDA uses OID 1.3.6.1.5.5.7.48.25 to provide the public URI of the CRL.

At present (March 2023), this value is
URI:<https://midway-ams-glb.ext.hpe.com/certs/pub/revocations.dat>
However, it may change in the future.

#### Subject Alternative Name extension

The SAN contains additional identity information that the relying party
needs in a LDevID or LAK certificate. Information SHOULD be encoded
using otherName types, but directoryName is permitted for the attributes
discussed in [directoryName attributes](#directoryname-attributes) and the dNSName and iPAddress fields MAY be
used for identifying the networking name or IP address of a device.

Each otherName entry is composed of an identifying type-id OID and a
value (data), whose encoding is specified by the type-id. A product or
service team can define its own identifying data. Below is an example of
a SAN otherName’s field:

    - type-id = id-HPE-DevID-SAN-clusterID

    - value ASN.1 type = UTF8String (value contains the clusterID of the
      platform, assigned by the RA.)

[HPE-defined OID values](#hpe-defined) contains the full list of HPE-assigned OIDs. Arc
id-HPE-DevID-SAN (“1.3.6.1.4.1.47196.6.3.1.\*”) in particular defines
OIDs that can be used in the SAN extension. Most are self-explanatory.
The following sub-sections discuss those that involve special
processing.

To prevent any conflict of OIDs, any new OID assignments MUST be
reviewed and approved by at least one of the authors of this
specification and the HPE PKI steering committee before use.

In addition, LDevID/LAK with a provable linkage to an IDevID/IAK MUST
adhere to the additional requirements defined in [SAN with linkage](#subject-alternative-name-extension-1).

##### id-HPE-DevID-SAN-clusterID - Cluster Identifier

The Cluster ID is the identifier common to all members of the same
compute cluster or storage array.

Encoded as an IA5String, using OID type id-HPE-DevID-SAN-clusterID
(“1.3.6.1.4.1.47196.6.3.1.6”). This field is OPTIONAL.

Example:

    id-HPE-DevID-SAN-clusterID =
    7cf93660-e192-470d-94a3-dbe3c00f927d

##### id-HPE-DevID-SAN-logicalProductID – Logical Product Identifier

Unique logical product identifier. The product is the parent "solution"
/ assembly / equipment / chassis to which a target device belongs, such
as:

- A ProLiant server contains two devices, the iLO and the host
  processor. They're both part of the ProLiant chassis.

- A Primera storage array has four processors each running RDA, each
  with their own Station ID. The *Cluster ID* would hold the overall
  Chassis ID for the array.

RDA calls this an “apparatus identifier”, a “chassis ID”, and an “Array
ID” in older certificate formats. In the newer G3 format, it’s called a
logical product identifier to align with this specification.

The logical product ID is encoded as an UTF8String, using OID type
id-HPE-DevID-SAN-logicalProductID (“1.3.6.1.4.1.47196.6.3.1.2”). This
field is OPTIONAL.

#### Certificate Policies extension

Analogously to the SAN extension, the Certificate Policies extension can
contain identifying information with each identifier being represented
as an OID. Compared to the SAN otherName types defined by HPE, Policy
OIDs have no associated value; their presence alone states that the
device has a particular attribute. The OIDs are listed in the
policyIdentifier of the PolicyInformation entries of the
certificatePolicies SEQUENCE.

A product or service team can request its own identifying OID(s).

To prevent any conflict of OIDs, any new OID assignments MUST be
reviewed and approved by at least one of the authors of this
specification and the HPE PKI steering committee before use.

### Summary of certificate values: TPM-Based LDevID (as issued by RDA)

This is the current format of an RDA-issued G3 LDevID.

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 2%" />
<col style="width: 15%" />
<col style="width: 8%" />
<col style="width: 21%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Property</strong></th>
<th colspan="7"><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="13">Subject</td>
<td colspan="2"><strong>RDN</strong></td>
<td colspan="3"><strong>Use</strong></td>
<td colspan="2"><strong>Value</strong></td>
</tr>
<tr class="even">
<td colspan="2">C</td>
<td colspan="3">Country</td>
<td colspan="2">“US” (example)</td>
</tr>
<tr class="odd">
<td colspan="2">ST</td>
<td colspan="3">State or Province</td>
<td colspan="2">“Texas” (example)</td>
</tr>
<tr class="even">
<td colspan="2">L</td>
<td colspan="3">Location</td>
<td colspan="2">“Houston” (example)</td>
</tr>
<tr class="odd">
<td colspan="2">O</td>
<td colspan="3">Organization</td>
<td colspan="2">“Hewlett Packard Enterprise Development”</td>
</tr>
<tr class="even">
<td colspan="2">OU</td>
<td colspan="3">BU</td>
<td colspan="2">Value in IDevID or omitted (see <a href="#organization-unit-name-ou-1">OU</a>).</td>
</tr>
<tr class="odd">
<td colspan="2">CN</td>
<td colspan="3">Part Number or SKU</td>
<td colspan="2">See <a href="#subject-field-1">Subject</a> and <a href="#common-name-cn-1">CN</a>.</td>
</tr>
<tr class="even">
<td colspan="2">serialNumber</td>
<td colspan="3">Serial Number</td>
<td colspan="2">See <a href="#subject-field-1">Subject</a> and <a href="#serialnumber">serialNumber</a>.</td>
</tr>
<tr class="odd">
<td colspan="2">GN</td>
<td colspan="3">Primary Component Part Number</td>
<td colspan="2">The PN used in the IDevID</td>
</tr>
<tr class="even">
<td colspan="2">SN</td>
<td colspan="3">Primary Component Serial Number</td>
<td colspan="2">The serialNumber used in the IDevID</td>
</tr>
<tr class="odd">
<td colspan="2">T</td>
<td colspan="3">Title (product identifier)</td>
<td colspan="2">See <a href="#title-t">T (1)</a> and <a href="#title-t-1">T (2)</a>.</td>
</tr>
<tr class="even">
<td colspan="2">pseudonym</td>
<td colspan="3">Station ID</td>
<td colspan="2">See <a href="#pseudonym">pseudonym (1)</a> and <a href="#pseudonym-1">pseudonym (2)</a>.</td>
</tr>
<tr class="odd">
<td colspan="2">generationQualifier</td>
<td colspan="3">RDA certificate format</td>
<td colspan="2">See <a href="#generationqualifier">generationQualifier (1)</a> and <a href="#generationqualifier-1">generationQualifier (2)</a>.</td>
</tr>
<tr class="even">
<td rowspan="2">Validity</td>
<td colspan="3">notBefore</td>
<td colspan="4">Date of certificate creation.</td>
</tr>
<tr class="odd">
<td colspan="3">notAfter</td>
<td colspan="4">Feb 6 21:46:31 2024 GMT (example)</td>
</tr>
<tr class="even">
<td colspan="2">Extensions</td>
<td colspan="6"></td>
</tr>
<tr class="odd">
<td colspan="2"><p>KeyUsage</p>
<p>2.5.29.15</p></td>
<td colspan="6">See <a href="#key-usage-extension-1">keyUsage extension</a>.</td>
</tr>
<tr class="even">
<td colspan="2"><p>extendedKeyUsage</p>
<p>2.5.29.37</p></td>
<td colspan="6">See <a href="#extended-key-usage-extension">extendedKeyUsage extension</a>.</td>
</tr>
<tr class="odd">
<td colspan="2"><p>subjectKeyIdentifier</p>
<p>2.5.29.14</p></td>
<td colspan="6">Identifier of public key</td>
</tr>
<tr class="even">
<td colspan="2"><p>authorityKeyIdentifier</p>
<p>2.5.29.35</p></td>
<td colspan="6">Identifier of parent CA key</td>
</tr>
<tr class="odd">
<td colspan="2"><p>SubjectAltName</p>
<p>2.5.29.17</p></td>
<td colspan="6">See <a href="#subject-alternative-name-extension-1">subjectAltName (2)</a> and <a href="#subject-alternative-name-extension-2">subjectAltName (3)</a>.</td>
</tr>
<tr class="even">
<td colspan="2" rowspan="7"><p>CertificatePolicy</p>
<p>2.5.29.32</p></td>
<td colspan="3"><strong>Name</strong></td>
<td colspan="2"><strong>OID</strong></td>
<td><strong>Meaning</strong></td>
</tr>
<tr class="odd">
<td colspan="3">tcg-cap-verifiedTPMResidency</td>
<td colspan="2">2.23.133.11.1.1</td>
<td>TPM residency verified – only present if LDevID/LAK is linked to an
IDevID/IAK that contains this OID</td>
</tr>
<tr class="even">
<td colspan="3">tcg-cap-verifiedTPMFixed</td>
<td colspan="2">2.23.133.11.1.2</td>
<td>The fixedTPM attribute was verified – only present if LDevID/LAK is
linked to an IDevID/IAK that contains this OID</td>
</tr>
<tr class="odd">
<td colspan="3">id-HPE-DevID-CP-LDevID</td>
<td colspan="2">1.3.6.1.4.1.47196.6.1.2</td>
<td>This is a field-provisioned LDevID or LAK</td>
</tr>
<tr class="even">
<td colspan="3"><p>id-HPE-DevID-CP-TPMProtected</p>
<p>or</p>
<p>id-HPE-DevID-CP-iLOProtected</p>
<p>or absent</p></td>
<td colspan="2"><p>1.3.6.1.4.1.47196.6.1.21</p>
<p>or</p>
<p>1.3.6.1.4.1.47196.6.1.22</p>
<p>or</p>
<p>absent</p></td>
<td>TPM-protected, or iLO-protected, or OID absent for LDevID/LAK
without IDevID/IAK</td>
</tr>
<tr class="odd">
<td colspan="3"><p>Product type OID from arc</p>
<p>id-HPE-DevID-CP-ptype</p></td>
<td colspan="2">1.3.6.1.4.1.47196.6.2.*</td>
<td>Product type OID from 1.3.6.1.4.1.47196.6.2.* arc (see <a href="#hpe-defined">HPE-defined OID values</a>)</td>
</tr>
<tr class="even">
<td colspan="3">id-HPE-DevID-CP-request-sources<br />
id-HPE-DevID-CP-request-source-direct<br />
id-HPE-DevID-CP-request-source-web<br />
id-HPE-DevID-CP-request-source-ra-lo<br />
id-HPE-DevID-CP-request-source-ra-md<br />
id-HPE-DevID-CP-request-source-ra-hi</td>
<td colspan="2">1.3.6.1.4.1.47196.6.1.30.*</td>
<td><p>RDA Request source</p>
<p>(see <a href="#hpe-defined">HPE-defined OID values</a>)</p></td>
</tr>
<tr class="odd">
<td colspan="2"></td>
<td colspan="3">id-HPE-RDA-policy</td>
<td colspan="2">1.3.6.1.4.1.47196.6.13.1</td>
<td><p>RDA-issued Policy</p>
<p>(see <a href="#hpe-defined">HPE-defined OID values</a>)</p></td>
</tr>
<tr class="even">
<td colspan="2"></td>
<td colspan="3"></td>
<td colspan="2">1.3.6.1.4.1.47196.6.13.*</td>
<td>Other RDA attributes</td>
</tr>
</tbody>
</table>

## OID Values

### TCG-Defined

TPM-based DevIDs must contain the following policy OIDs as defined in
[[2]](#references) section 8.2.

| **Name**                      | **OID**         | **Description**                                                       |
|-------------------------------|-----------------|-----------------------------------------------------------------------|
| tcg-cap-verifiedTPMResidency  | 2.23.133.11.1.1 | TPM residency verified                                                |
| tcg-cap-verifiedTPMFixed      | 2.23.133.11.1.2 | The fixedTPM bit was set in the key public area                       |
| tcg-cap-verifiedTPMRestricted | 2.23.133.11.1.3 | Both the fixedTPM and restricted bits are set in the key public area. |

A DevID certificate will have the tcg-cap-verifiedTPMResidency policy
OID if the CA used TCG-specified methods to prove TPM residency of the
key and that the key is on the specified device. The
tcg-cap-verifiedTPMFixed OID will be included if the CA has verified
that the fixedTPM bit was set in the key public area and the
tcg-cap-verifiedTPMRestricted OID will be included if the CA has
verified both the fixedTPM and restricted bits are set in the key public
area

Note that only one of tcg-cap-verifiedTPMFixed and
tcg-cap-verifiedTPMRestricted can be included in the same certificate.

Specifically, if they meet the requirements above, IDevIDs can hold the
tcg-cap-verifiedTPMResidency and verifiedTPMFixed OIDs and IAKs can hold
the tcg-cap-verifiedTPMResidency and tcg-cap-verifiedTPMRestricted OIDs.

These OIDs contain no data and MUST NOT be set as critical.

### HPE-Defined

This table contains the list of HPE-defined OIDs for the purposes of
Device Identity.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>OID arc</th>
<th>Specific OID</th>
<th>Meaning / Purpose</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td> 1.3.6.1.4.1.47196.6.*</td>
<td> </td>
<td>HPE DevID PKI</td>
<td>Policy and device class identification used in certificates issued
to devices.</td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.1</td>
<td>HPE CA Policy OIDs</td>
<td>Requester: Tom Laffey: (classification along the lines of what we
have in Aruba)</td>
</tr>
<tr class="odd">
<td>1.3.6.1.4.1.47196.6.1.*</td>
<td> id-HPE-DevID-CP</td>
<td>CA Policy container</td>
<td><p>Requester: Tom Laffey</p>
<p>not used directly</p></td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.1.1</p>
<p>(id-HPE-DevID-CP-IDevID)</p></td>
<td>IDevID identifier</td>
<td><p>Identifies the cert as an IDevID/IAK.</p>
<p>Note: This was used for “TPM-protected IDevID policy” in some certs,
and for “HPE Factory Proof-of-Presence” by iLO FW</p></td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.1.2</p>
<p>(id-HPE-DevID-CP-LDevID)</p></td>
<td>LDevID identifier</td>
<td>To be used for standard, field-provisioned LDevIDs/LAKs.</td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.1.3</p>
<p>(id-HPE-DevID-CP-SystemLDevID)</p></td>
<td>System LDevID identifier</td>
<td>To be used for factory-provisioned, permanent LDevIDs/LAKs – a.k.a.
“System LDevIDs”.</td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.1.21</p>
<p>(id-HPE-DevID-CP-TPMProtected)</p></td>
<td>TPM-protected</td>
<td>DevID is protected in TPM</td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.1.22</p>
<p>(id-HPE-DevID-CP-iLOProtected)</p></td>
<td>iLO-protected</td>
<td>DevID is protected in iLO</td>
</tr>
<tr class="odd">
<td>1.3.6.1.4.1.47196.6.2.*</td>
<td>id-HPE-DevID-CP-ptype</td>
<td>Product Type container</td>
<td> </td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.1</p>
<p>(id-HPE-DevID-CP-ptype-com)</p></td>
<td>Compute</td>
<td><p>General purpose server</p>
<p>Requester: Tom Laffey/Luis Luciani.</p></td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.2</p>
<p>(id-HPE-DevID-CP-ptype-sto)</p></td>
<td>Storage</td>
<td><p>Storage server</p>
<p>Requester: Tom Laffey</p></td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.3</p>
<p>(id-HPE-DevID-CP-ptype-net)</p></td>
<td>Network</td>
<td><p>Data center network switch</p>
<p>Requester: Tom Laffey</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.2.4 </td>
<td>iLO/BMC</td>
<td><p><strong><mark>DEPRECATED</mark></strong></p>
<p>Requester: Tom Laffey/Luis Luciani. iLO-backed cert example: (HPE
BMC/iLO in general purpose server)</p></td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.5</p>
<p>(id-HPE-DevID-CP-ptype-hpc)</p></td>
<td>Supercomputing</td>
<td>HPC/Supercomputer</td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.6</p>
<p>(id-HPE-DevID-CP-ptype-sw)</p></td>
<td>Software</td>
<td>Non-device product</td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.7</p>
<p>(id-HPE-DevID-CP-ptype-svc)</p></td>
<td>Services</td>
<td>Non-device product</td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.8</p>
<p>(id-HPE-DevID-CP-ptype-cld)</p></td>
<td>Cloud</td>
<td>Non-device product</td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.2.9</p>
<p>(id-HPE-DevID-CP-ptype-virt)</p></td>
<td>Virtual Machine / Virtual Appliance / Container Appliance</td>
<td>Non-physical device</td>
</tr>
<tr class="odd">
<td>1.3.6.1.4.1.47196.6.3.*</td>
<td> id-HPE-DevID</td>
<td>Attribute container</td>
<td>DevID-specific OIDs</td>
</tr>
<tr class="even">
<td>1.3.6.1.4.1.47196.6.3.1.*</td>
<td> id-HPE-DevID-SAN</td>
<td>HPE SAN otherNames container</td>
<td>DevID-specific SubjectAltName OIDs</td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.3.1.3</p>
<p>(id-HPE-DevID-SAN-productName)</p></td>
<td>Product Name</td>
<td><p>To hold human-readable product name (e.g. “ProLiant DL360 Gen10
Plus”).</p>
<p>UTF8 String</p>
<p>Requester: Nigel Edwards</p></td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.3.1.4</p>
<p>(id-HPE-DevID-SAN-platformFamily)</p></td>
<td>Platform Family</td>
<td><p>To hold platform family (e.g. “ProLiant”). Also called the
<em>Product Class</em>.</p>
<p>UTF8 String</p>
<p>Requester: Gareth Richards; Tom Laffey</p></td>
</tr>
<tr class="odd">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.3.1.6</p>
<p>(id-HPE-DevID-SAN-clusterID)</p></td>
<td>Cluster ID</td>
<td><p>The cluster identifier of the product containing the device
associated with the subject DN.</p>
<p>IA5String</p>
<p>Requester: Gareth Richards; Tom Laffey:</p></td>
</tr>
<tr class="even">
<td></td>
<td><p>1.3.6.1.4.1.47196.6.3.1.7</p>
<p>(id-HPE-DevID-SAN-productLine)</p></td>
<td>Product Line</td>
<td><p>To hold product line (e.g. “DL360”).</p>
<p>UTF8 String</p>
<p>Requester: Steve Roscio</p></td>
</tr>
<tr class="even">
<td rowspan="2">1.3.6.1.4.1.47196.6.3.2.*</td>
<td rowspan="2"> id-HPE-DevID-iLOHwTypes</td>
<td rowspan="2">Hardware Types container</td>
<td rowspan="2">HPE iLO hardware type identifiers</td>
</tr>
<tr class="odd">
</tr>
<tr class="even">
<td> </td>
<td><p>1.3.6.1.4.1.47196.6.3.2.1</p>
<p>(id-HPE-DevID-iLOHwTypes-iLO5)</p></td>
<td>iLO 5</td>
<td>Neches</td>
</tr>
<tr class="odd">
<td> </td>
<td><p>1.3.6.1.4.1.47196.6.3.2.2</p>
<p>(id-HPE-DevID-iLOHwTypes-iLO6)</p></td>
<td>iLO 6</td>
<td>Trinity</td>
</tr>
<tr class="odd">
<td> </td>
<td><p>1.3.6.1.4.1.47196.6.3.2.3</p>
<p>(id-HPE-DevID-iLOHwTypes-iLO7)</p></td>
<td>iLO 7</td>
<td>SanJac</td>
</tr>
<tr class="even">
<td>1.3.6.1.4.1.47196.6.4.1 *</td>
<td> </td>
<td>Service Identifiers</td>
<td><p>Identifies what portal(s) or application(s) or application
classes manage this device.</p>
<p>Requesters: Tom Laffey, Steve Roscio</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.4.1.1.*</td>
<td>Common Cloud Platform</td>
<td>Requester: Tom Laffey</td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.4.1.1.1</td>
<td>GreenLake Central</td>
<td>Requester: Tom Laffey</td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.4.1.1.2</td>
<td>COM – Compute Ops Management</td>
<td>Requesters: Tom Laffey, Steve Roscio</td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.4.1.1.3</td>
<td>DSCC - Data Services Cloud Console</td>
<td>Requesters: Tom Laffey, Steve Roscio</td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.4.1.1.4</td>
<td>Aruba Central</td>
<td>Requesters: Tom Laffey, Steve Roscio</td>
</tr>
<tr class="even">
<td> 1.3.6.1.4.1.47196.6.13.*</td>
<td> </td>
<td>RDA attributes</td>
<td><p>These OIDs are applicable to RDA-issued certificates only.</p>
<p>Requester: Steve Roscio</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.1</td>
<td>RDA Policy</td>
<td><p>HPE RDA-issued LDevID Certificates call out policy OID
1.3.6.1.4.1.47196.6.13.1 with the following sub-values:</p>
<p>CPS: https://midway.ext.hpe.com/certs/pub/rda-cps.pdf</p>
<p>CPS: http://www.hpe.com/info/rda-docs</p>
<p>User Notice:</p>
<p>Organization: Hewlett Packard Enterprise</p>
<p>Number: 14</p>
<p>Explicit Text: Authority to bind HPE does not correspond with use or
possession of this certificate. Issued to facilitate communication with
HPE.</p></td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.2</td>
<td>Station Role</td>
<td><p>RDA defines station roles such as Customer Access System (CAS),
Midway, Delegate, Outsider, etc.</p>
<p>REQUIRED for RDA. PrintableString (preferred) or UTF8String, max 6
chars</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.3</td>
<td>Renewal Count</td>
<td><p>Certificate renewal or reissued count - Number of times this cert
has been renewed or reissued. A newly issued certificate sets this
field's value to zero (0). The first renewed or reissued certificate for
this station has the value 1, and so on.</p>
<p>RECOMMENDED for RDA, ASN.1 Integer.</p></td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.4</td>
<td>Static Trust</td>
<td><p>The Static Trust level assigned to this station. The Static Trust
value may be a letter grade "A" thru "F" with plus and minus suffixes
e.g. "B+" or "C-"; or it can be a floating point value from 0.0 to 1.0
expressed in decimal text with two digits precision, such as "0.67" or
"0.90".</p>
<p>REQUIRED for RDA. PrintableString (preferred) or UTF8String, max 4
chars.</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.5</td>
<td>Manufacturer ID</td>
<td><p>Product manufacturer's short identifier. Defaults to "hpe" if not
present in the certificate. This is different from the full
manufacturer's name which may be the "O" (organization) value in the
Issuer DN.</p>
<p>OPTIONAL for RDA, PrintableString (preferred) or UTF8String, max 16
chars.</p></td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.6</td>
<td>Factory ID</td>
<td><p>Factory short identifier. The ID for the factory where the device
was assembled, such as "FC007".</p>
<p>OPTIONAL for RDA, PrintableString (preferred) or UTF8String, max 16
chars.</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.10</td>
<td>MAC Addresses</td>
<td><p>Comma-delimited list of MAC addresses of the primary interfaces
on the device. Comma-space delimiting is accepted. Used for device
fingerprinting. Omit this field if a non-device product.</p>
<p>OPTIONAL for RDA, PrintableString, max 255 chars.</p></td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.91</td>
<td>Product Free-Field One</td>
<td><p>Device- or product-specific string field supported by the HPE
LDevID services.</p>
<p>The interpretation of this field is specific to the product, it is
not defined here. This is the 1st of three product-specific fields.</p>
<p>Omit this field if not using it (as opposed to having it blank).</p>
<p>OPTIONAL for RDA, UTF8String, max 255 chars.</p></td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.92</td>
<td>Product Free-Field One</td>
<td><p>Device- or product-specific string field supported by the HPE
LDevID services.</p>
<p>The interpretation of this field is specific to the product, it is
not defined here. This is the 2nd of three product-specific fields.</p>
<p>Omit this field if not using it (as opposed to having it blank).</p>
<p>OPTIONAL for RDA, UTF8String, max 255 chars.</p></td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.13.93</td>
<td>Product Free-Field One</td>
<td><p>Device- or product-specific string field supported by the HPE
LDevID services.</p>
<p>The interpretation of this field is specific to the product, it is
not defined here. This is the 3rd of three product-specific fields.</p>
<p>Omit this field if not using it (as opposed to having it blank).</p>
<p>OPTIONAL for RDA, UTF8String, max 255 chars.</p></td>
</tr>
<tr class="odd">
<td>1.3.6.1.4.1.47196.6.1.30.*</td>
<td></td>
<td>RDA Request Source</td>
<td>The presence of one of the OIDs under this arc indicates the source
of the request</td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.1.30.1</td>
<td></td>
<td>Directly from the device: the device sent the CSR directly to the
CA.</td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.1.30.2</td>
<td></td>
<td>From the customer via a paste-in-CSR web portal</td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.1.30.3</td>
<td></td>
<td>From an external (low-trust) Registration Authority.<br />
Device → Ext RA → CA</td>
</tr>
<tr class="odd">
<td></td>
<td>1.3.6.1.4.1.47196.6.1.30.4</td>
<td></td>
<td>From an HPE medium-trust (e.g. Cloud-hosted) Registration Authority.
Device → Cloud RA → CA</td>
</tr>
<tr class="even">
<td></td>
<td>1.3.6.1.4.1.47196.6.1.30.5</td>
<td></td>
<td>From an HPE high-trust RA: Device → HPE RA → CA</td>
</tr>
</tbody>
</table>

### IETF-Defined

| **Name**                  | **OID**           | **Description**                                 |
|---------------------------|-------------------|-------------------------------------------------|
| id-on-hardwareModuleName  | 1.3.6.1.5.5.7.8.4 | Description of hardware module (TPM 2.0 or iLO) |
| id-on-permanentIdentifier | 1.3.6.1.5.5.7.8.3 | Identifier of the particular TPM                |

## Annex A. Device identification values

This section specifies the identifiers that MUST be assigned to the four Subject RDNs (GN, SN, CN and serialNumber)
that identify a device, for each product type and DevID type.

The tables below assume four distinct points in the life of a device when DevIDs can be provisioned:

1) Component-level ("L6") IDevID provisioning:<br>
  At this stage, the primary component of a device is provisioned with an identity. The primary component
  may be a PCA board in a monolithic system or a DC-SCM board in a modular system. In the scope of this
  specification, a component-level IDevID will be issued to the component hosting the TPM and BMC of a
  device. Note that with the advent of SPDM, other system components may be given cryptographic identities.
  While SPDM identity certificates can be made compatible with DevID certificates, this specification
  only covers the component-level IDevID provisioned to the primary device component.
  This stage is often called "L6" to reflect the process terminology used in manufacturing, but generally
  indicates the stage when the primary device component is integrated into a chassis and tested.
2) System-level ("L10") IDevID provisioning:<br>
  At this stage, an assembled system is provisioned with an IDevID. This is the stage where the system is
  fully integrated and the system-level (e.g., chassis, product, etc.) serial number and SKU are known and
  can be recorded in the IDevID.
  A System-level ("L10") IDevID MUST only be provisioned when no component-level IDevID is present, i.e.,
  HPE will only provision an IDevID at L10 when an L6 IDevID provisioning capability is not available.
  In other words, a system can only have **one** IDevID.
3) System-level ("L10") LDevID provisioning:<br>
  A System-level ("L10") LDevID can be provisioned on systems that carry a component-level (L6) IDevID
  to allow capturing the additional identifiers and attributes defined at L10. As discussed in
  [Factory-provisioned LDevID and LAK certificates](#factory-provisioned-ldevid-and-lak-certificates)
  a System-level LDevID is for all intents and purposes equivalent to an IDevID and, when issued, is
  meant to effectively supplant the IDevID. The reason a new IDevID that overwrites the L6-provisioned
  one is not used instead is that the inclusion of component-level and system-level identifiers in the
  IDevID creates problems with repair and spare management scenarios.
4) Field-issued LDevID provisioning:<br>
  Field-issued LDevIDs are meant to provide an identity that contains locally-significant (i.e., customer-
  specific) attributes. They generally have a defined validity (in contrast to IDevIDs and System LDevIDs
  which have either very long or effectively infinite validity) and can be issued by the customer using
  their own PKI. In the context of HPE, Field-issued LDevIDs are issued by RDA for the purpose of protecting
  the factory-issued DevIDs (IDevIDs and System LDevIDs) from compromise due to frequent use. In addition,
  RDA may include additional attributes not present in the factory issued DevIDs useful for GreenLake uses.

In summary, a system may have one of the following six combinations of DevIDs:

1) System-level IDevID + field-issued LDevID (no System LDevID)
2) System-level IDevID + System-level LDevID + field-issued LDevID (no component-level IDevID)
3) Component-level IDevID + System-level LDevID + field-issued LDevID
4) System-level IDevID + System-level LDevID (no field-issued LDevID)
5) Component-level IDevID + System-level LDevID (no field-issued LDevID)
6) Field-issued LDevID (no TPM or no factory-issued DevID)

### Component-level ("L6") IDevID

| Product Type                                      | GN               | SN               | CN               | serialNumber     |
|---------------------------------------------------|------------------|------------------|------------------|------------------|
| *Storage*                                         |                  |                  |                  |                  |
| Raider IOM (IO Module - ProLiant MB)              | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Raider CDM (Raider Chassis Discovery Module)      | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Invader IOM                                       | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Invader RMM (Redundant Mgmt Module-SW Mgmt Blade) | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Invader Backplane                                 | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Invader Backplane                                 | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Nimble (legacy)                                   | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| 3PAR (legacy)                                     | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Alletra 5k, 6k (rebranded Nimble)                 | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Alletra 9k (Primera on Minotaur)                  | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| HPE GreenLake for Block Storage MP  (Arcus)       | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| HPE GreenLake for Object Storage MP (Home Fleet)  | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| HPE GreenLake for File Storage MP  (Razor Crest)  | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| MSA Array                                         | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| *Compute*                                         |                  |                  |                  |                  |
| Gen 10+ (current process)                         | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Gen 10+ (potential future process)                | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Gen 11 (current process)                          | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Gen 11 (potential future process)                 | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Gen 12SP                                          | Absent           | Absent           | PCA Part Num     | PCA SN           |
| Gen 12                                            | Absent           | Absent           | PCA Part Num     | PCA SN           |
| *Networking*                                      |                  |                  |                  |                  |
| Aruba AP (Legacy)                                 | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Aruba Switch                                      | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Aruba non-TPM Switch                              | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Pensando in Aruba Switch                          | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Aruba AP (post-migration)                         | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| Pensando in HPE Server                            | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable) |
| *HPC*                                             |                  |                  |                  |                  |
| Platform Controller                               | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Cray TORC (Top Of Rack Controller)                | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Cray Compute Node                                 | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Cray Non-Compute Node (NCN)                       | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Cray Switch (Cassini 3/ Rosetta 3)                | (Not Issued)     | (Not Issued)     | (Not Issued)     | (Not Issued)     |
| Superdome                                         |                  |                  |                  |                  |

### System-level ("L10") IDevID

| Product Type                                      | GN           | SN           | CN                            | serialNumber       |
|---------------------------------------------------|--------------|--------------|-------------------------------|--------------------|
| *Storage*                                         |              |              |                               |                    |
| Raider CDM (Raider Chassis Discovery Module)      | Absent       | Absent       | Storage MOD1 Part Num         | Storage MOD1 CT SN |
| Invader IOM                                       | Absent       | Absent       | Storage MOD1 Part Num         | Storage MOD1 CT SN |
| Invader RMM (Redundant Mgmt Module-SW Mgmt Blade) | Absent       | Absent       | Storage MOD1 Part Num         | Storage MOD1 CT SN |
| Invader Backplane                                 | Absent       | Absent       | Storage MOD1 Part Num         | Storage MOD1 CT SN |
| Nimble (legacy)                                   | (Not Issued) | (Not Issued) | (Not Issued)                  | (Not Issued)       |
| 3PAR (legacy)                                     | (Not Issued) | (Not Issued) | (Not Issued)                  | (Not Issued)       |
| Alletra 5k, 6k (rebranded Nimble)                 | (Not Issued) | (Not Issued) | (Not Issued)                  | (Not Issued)       |
| Alletra 9k (Primera on Minotaur)                  | See Raider   | See Raider   | See Raider                    | See Raider         |
| HPE GreenLake for Block Storage MP  (Arcus)       | See Raider   | See Raider   | See Raider                    | See Raider         |
| HPE GreenLake for Object Storage MP (Home Fleet)  | See Raider   | See Raider   | See Raider                    | See Raider         |
| HPE GreenLake for File Storage MP  (Razor Crest)  | Absent       | Absent       | Storage MOD1 Part Num         | Storage MOD1 CT SN |
| MSA Array                                         | Absent       | Absent       | Seagate SKU                   | Seagate SN         |
| *Compute*                                         | | | | |
| Gen 10+ (current process - TPM only)              | Absent       | Absent       | System SKU                    | System SN          |
| Gen 10+ (potential future process)                | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable)   |
| Gen 11 (current process)                          | Absent       | Absent       | System SKU                    | System SN          |
| Gen 11 (potential future process)                 | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable)   |
| Gen 12SP                                          | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable)   |
| Gen 12                                            | (Not Applicable) | (Not Applicable) | (Not Applicable) | (Not Applicable)   |
| *Networking*                                      | | | | |
| Aruba AP (Legacy)                                 | Absent       | Absent       | ChassisSerial::01:02:03:05:06 | Absent             |
| Aruba Switch                                      | Absent       | Absent       | ProdNum                       | Chassis SN         |
| Aruba non-TPM Switch                              | Absent       | Absent       | Absent                        | Absent             |
| Pensando in Aruba Switch                          | Absent       | Absent       | ProdNum                       | Chassis SN         |
| Aruba AP (post-migration)                         | Absent       | Absent       | ProdNum                       | Chassis SN         |
| Pensando in HPE Server                            |              |              |                               |                    |
| *HPC*                                             | | | | |
| Platform Controller                               | Absent       | Absent       | PCA Part Num                  | PCA SN             |
| Cray TORC (Top Of Rack Controller)                | Absent       | Absent       | PCA Part Num                  | PCA SN             |
| Cray Compute Node                                 | Absent       | Absent       | PCA Part Num                  | PCA SN             |
| Cray Non-Compute Node (NCN)                       | TBD          | TBD          | TBD                           | TBD                |
| Cray Switch (Cassini 3/ Rosetta 3)                | Caliptra ID* | Caliptra ID* | Caliptra ID*                  | Caliptra ID*       |
| Superdome                                         |                  |                  |                  |                  |

### System-level ("L10") LDevID

| Product Type                                      | GN           | SN           | CN           | serialNumber |
|---------------------------------------------------|--------------|--------------|--------------|--------------|
| *Storage*                                         | | | | |
| Raider IOM (IO Module - ProLiant MB)              | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Raider CDM (Raider Chassis Discovery Module)      | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Invader IOM                                       | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Invader RMM (Redundant Mgmt Module-SW Mgmt Blade) | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Invader Backplane                                 | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Nimble (legacy)                                   | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| 3PAR (legacy)                                     | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Alletra 5k, 6k (rebranded Nimble)                 | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| Alletra 9k (Primera on Minotaur)                  | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| HPE GreenLake for Block Storage MP  (Arcus)       | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| HPE GreenLake for Object Storage MP (Home Fleet)  | See Raider   | See Raider   | See Raider   | See Raider   |
| HPE GreenLake for File Storage MP  (Razor Crest)  | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| MSA Array                                         | (Not Issued) | (Not Issued) | (Not Issued) | (Not Issued) |
| *Compute*                                         |              |              |              |              |
| Gen 10+ (current process - iLO only)              | Absent       | Absent       | System SKU   | System SN    |
| Gen 10+ (potential future process)                | PCA Part Num | PCA SN       | System SKU   | System SN    |
| Gen 11 (current process)                          | Absent       | Absent       | System SKU   | System SN    |
| Gen 11 (potential future process)                 | PCA Part Num | PCA SN       | System SKU   | System SN    |
| Gen 12SP                                          | PCA Part Num | PCA SN       | System SKU   | System SN    |
| Gen 12                                            | PCA Part Num | PCA SN       | System SKU   | System SN    |
| *Networking*                                      |              |              |              |              |
| Aruba AP (Legacy)                                 |              |              |              |              |
| Aruba Switch                                      |              |              |              |              |
| Aruba non-TPM Switch                              |              |              |              |              |
| Pensando in Aruba Switch                          |              |              |              |              |
| Aruba AP (post-migration)                         |              |              |              |              |
| Pensando in HPE Server                            |              |              |              |              |
| *HPC*                                             |              |              |              |              |
| Platform Controller                               | PCA Part Num | PCA SN       | System SKU   | System SN    |
| Cray TORC (Top Of Rack Controller)                | PCA Part Num | PCA SN       | System SKU   | System SN    |
| Cray Compute Node                                 | PCA Part Num | PCA SN       | System SKU   | System SN    |
| Cray Non-Compute Node (NCN)                       | TBD          | TBD          | TBD          | TBD          |
| Cray Switch (Cassini 3/ Rosetta 3)                | Caliptra ID* | Caliptra ID* | Caliptra ID* | Caliptra ID* |
| Superdome                                         |              |              |              |              |

### Field-issued LDevID

| Product Type                                      | GN                    | SN                 | CN                            | serialNumber      |
|---------------------------------------------------|-----------------------|--------------------|-------------------------------|-------------------|
| *Storage*                                         |                       |                    |                               |                   |
| Raider IOM (IO Module - ProLiant MB)              | Storage MOD1 Part Num | Storage MOD1 CT SN | (JBOF/Cluster) SKU            | (JBOF/Service) SN |
| Raider CDM (Raider Chassis Discovery Module)      | (Not Issued)          | (Not Issued)       | (Not Issued)                  | (Not Issued)      |
| Invader IOM                                       | Storage MOD1 Part Num | Storage MOD1 CT SN | (JBOF/Cluster) SKU            | (JBOF/Service) SN |
| Invader RMM (Redundant Mgmt Module-SW Mgmt Blade) | (Not Issued)          | (Not Issued)       | (Not Issued)                  | (Not Issued)      |
| Invader Backplane                                 | Storage MOD1 Part Num | Storage MOD1 CT SN | Chassis SKU                   | Chassis SN        |
| Nimble (legacy)                                   | (Not Issued)          | (Not Issued)       | (Not Issued)                  | (Not Issued)      |
| 3PAR (legacy)                                     | (Not Issued)          | (Not Issued)       | (Not Issued)                  | (Not Issued)      |
| Alletra 5k, 6k (rebranded Nimble)                 | (Not Issued)          | (Not Issued)       | (Not Issued)                  | (Not Issued)      |
| Alletra 9k (Primera on Minotaur)                  | See Raider            | See Raider         | See Raider                    | See Raider        |
| HPE GreenLake for Block Storage MP  (Arcus)       | See Raider            | See Raider         | See Raider                    | See Raider        |
| HPE GreenLake for Object Storage MP (Home Fleet)  | See Raider            | See Raider         | See Raider                    | See Raider        |
| HPE GreenLake for File Storage MP  (Razor Crest)  | TBD                   | TBD                | TBD                           | TBD               |
| MSA Array                                         | Seagate SKU           | Seagate SN         | HPE SKU                       | HPE SN            |
| *Compute*                                         |                       |                    |                               |                   |
| Gen 10+ (current process)                         | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Gen 10+ (potential future process)                | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Gen 11 (current process)                          | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Gen 11 (potential future process)                 | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Gen 12SP                                          | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Gen 12                                            | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| *Networking*                                      |                       |                    |                               |                   |
| Aruba AP (Legacy)                                 | TBD                   | TBD                | ChassisSerial::01:02:03:05:06 | Absent            |
| Aruba Switch                                      |                       |                    |                               |                   |
| Aruba non-TPM Switch                              | Absent                | Absent             | ProdNum                       | Chassis SN        |
| Pensando in Aruba Switch                          | TBD                   | TBD                | TBD                           | TBD               |
| Aruba AP (post-migration)                         | TBD                   | TBD                | TBD                           | TBD               |
| Pensando in HPE Server                            |                       |                    |                               |                   |
| *HPC*                                             |                       |                    |                               |                   |
| Platform Controller                               | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Cray TORC (Top Of Rack Controller)                | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Cray Compute Node                                 | PCA Part Num          | PCA SN             | System SKU                    | System SN         |
| Cray Non-Compute Node (NCN)                       | TBD                   | TBD                | TBD                           | TBD               |
| Cray Switch (Cassini 3/ Rosetta 3)                | Caliptra ID*          | Caliptra ID*       | Caliptra ID*                  | Caliptra ID*      |
| Superdome                                         |                       |                    |                               |                   |

#### Notes on tables

*: Identifiers may be derived from Caliptra identity. Details must be confirmed, as Caliptra keys may change when firmware changes.

## Annex B. Known issues with Gen10/Gen10+/Gen11 certificates

This section lists the differences between certificates issued before
this spec was developed and the current version of the specification.
This section is included for historical context and may contain out-of-date information.

### Gen10/10+ iLO IDevID certificate

Subject fields:

| **RDN**      | **Current Use**              | **Action required**                              |
|--------------|------------------------------|--------------------------------------------------|
| CN           | Assembly SN                  | Entity PN as described in <a href="#common-name-cn">CN</a>          |
| serialNumber | Missing                      | Entity SN as described in <a href="#serial-number">serialNumber</a>          |
| O            | "Hewlett Packard Enterprise" | Must be “Hewlett Packard Enterprise Development” |
| ST           | "TX"                         | Must be “Texas”                                  |

In general, iLO and TPM Subject RDNs should be assigned the same values.
At present, there are discrepancies between the OU, ST and L RDNs.

Basic Constraints is present. This MUST be removed.

Hardware Module Name in SAN is not present. This MUST be added.

Permanent Identifier in SAN is not present. This MUST be added for
TPM-protected credentials.

The Policy OID identifying a platform as a generic server or storage
server (1.3.6.1.4.1.47196.6.2.1 or
1.3.6.1.4.1.47196.6.2.2) is missing.

The keyUsage extension lacks the keyAgreement flag.

The iLO SAN currently contains the following:

> In some certificate specimens, there is a duplicate entry of OID
> 2.23.133.5.1.6 which is assigned the value “HPE” in otherName. This
> MUST be removed.
>
> The correct OID which is 1.3.6.1.5.5.7.8.4 (id-on-hardwareModuleName)
> MUST be used as per section 2.3.4. The hwType MUST take the values
> defined in [hwType](#hardware-module-name) to identify
> the appropriate iLO version.
>
> If the OID used to identify the hwType as iLO is
> “1.3.6.1.4.1.232.9.2.2.21.5”, described as “Remote Insight/ Integrated
> Lights-Out Model” by oidref.com and using the legacy Compaq/HP node,
> this MUST be corrected as per above.
>
> In general, OIDs referencing SNMP MUST NOT be used in DevID/IAK
> certificates. For example, no OIDs from 1.3.6.1.4.1.232.\* should
> exist in these certificates.
>
> Device Product ID: Currently DirName in SAN section contains the
> device product ID. The CommonName attribute under the subject section
> of the certificate should contain the device's product ID.

### Gen10/10+ TPM IDevID and IAK certificate

Basic Constraints extension is present in Gen10/10+, with CA = TRUE but
pathLenConstraint = 0. The basicConstraints extension MUST be absent.

KeyUsage is digitalSignature, nonrepudiation and KeyEncipherment. This
MUST be corrected as per section 2.3.3. Note the difference between
IDevID and IAK KeyUsage settings.

[^1]: At the time of this writing (11/13/2024) the committee members
    are: Tom Laffey, Nigel Edwards, Matthew Yang.

[^2]: Ideally, an iLO ASIC-unique serial number should be used for the
    hwSerialNum. iLO engineering has confirmed that iLO 6 does not have
    a unique serial for part identification purposes, so the Subject
    serialNumber is used for this iLO generation.

## Changelog

| Version | Details | Date |
| ------- | ------- | ---- |
| 1.0 | First release. | 2023-12-05 |
| 1.1 | - Added Table of device identification values for different product types in Annex A.<br>- Removed some SAN OIDs that were considered redundant. <br>- Minor editorial fixes.| 2024-05-08 |
| 1.2 | - Removed some provisioning location OIDs, considered redundant. <br>- Added iLO 7 OID. <br>- Updated Annex A Tables with Gen12SP designation in place of Gen11+. <br>- Minor editorial fixes.| 2024-11-26 |
