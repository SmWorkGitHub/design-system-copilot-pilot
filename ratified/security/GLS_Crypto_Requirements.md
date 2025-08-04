# HPE GreenLake TLS Cryptographic Requirements

This document defines some Transport Layer Security (TLS) protocol-related cryptography requirements for Hewlett Packard Enterprise (HPE) GreenLake
(GL) services and devices that communicate with GreenLake services. This is not a full security specification. It narrows the
options from other specifications to be specific to HPE GreenLake.
It is expected that future versions of this document will expand into other use cases like Secure Shell (SSH) protocol, password hashing, and software signature verification.

These requirements, derived from the National Institute of Standards
and Technology (NIST), Federal Information Processing Standards (FIPS), and Special Publications (SP) documents referenced below, are to be used when designing and implementing GL solutions that require cryptographic controls. Only the cryptographic algorithms listed in this document may be used. Deviations must be reviewed and explicitly approved per policy.

- [NIST SP 800-52 Rev. 2 Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations](https://csrc.nist.gov/pubs/sp/800/52/r2/final)
- [FIPS PUB 180-4 Secure Hash Standard (SHS)](https://csrc.nist.gov/pubs/fips/180-4/upd1/final)
- [FIPS PUB 197 Advanced Encryption Standard (AES)](https://csrc.nist.gov/pubs/fips/197/final)

Throughout this document there are references to the Commercial National Security Algorithms (CNSA) organization. The CNSA guidelines referenced are in place to mitigate against "store now and decrypt later" attacks when quantum computers become sufficiently powerful.

This policy documents HPE approved algorithms which are a subset of FIPS approved algorithms. A mandate for **FIPS validated** cryptography may occur in a separate document.

## Requirements

### Transport Layer Security (TLS) Protocol

The following is required for transport security:

### Enable Both TLS 1.2 and TLS 1.3 With No Down Select

On August 29, 2019 NIST issued guidance in [SP 800-52
Rev2](https://csrc.nist.gov/pubs/sp/800/52/r2/final)
to start the transition to TLS 1.3. The text is below:

> Agencies **shall** support TLS 1.3 by January 1, 2024. After this
> date, servers **shall** support TLS 1.3 for both government-only and
> citizen or business-facing applications. In general, servers that
> support TLS 1.3 **should** be configured to use TLS 1.2 as well.
> However, TLS 1.2 may be disabled on servers that support TLS 1.3 if it
> has been determined that TLS 1.2 is not needed for interoperability.

To be clear, both TLS 1.2 and TLS 1.3 should be enabled until it
has been determined that TLS 1.2 is not needed for interoperability.

## TLS Cipher Suite Requirements

The following show a subset of a FIPS-approved cipher suites from [SP
800-52
Rev2](https://csrc.nist.gov/pubs/sp/800/52/r2/final)
(Nov 2019), section 3.3 that has been reduced to only cipher suites that
are also supported by CNSA (Ext Ref \[2\] Cooley, March 2021, sections 6
and 7).

### TLS 1.2 Cipher Suites

TLS 1.2 protocol specifies cipher suites in pairs to reflect the type of key (RSA or ECDSA) used on the server.

TLS 1.2 **servers** MUST support at least one of TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384 or
TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384.

TLS 1.2 **clients** MUST support both TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384 and
TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384.

| Cipher | RFC Cipher Name                                    |  Notes                          |
| :----- | :------------------------------------------------- | :------------------------------ |
| 0xc030 | **TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384**   | MUST, CNSA approved (preferred) |
| 0xc02C | **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384** | MUST, CNSA approved (preferred) |
| 0x009F | TLS\_DHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384**       | Acceptable, not Ideal \[1\]     |
| 0x009E | TLS\_DHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256         | Acceptable, not Ideal \[2\]     |
| 0xc023 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_CBC\_SHA256     | Acceptable, not Ideal \[2,3\]   |
| 0xc027 | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256       | Acceptable, not Ideal \[2,3\]   |

\[1\] DHE uses finite field prime factorization in the key exchange. Since these are fixed size
it is likely that it will be revoked from the FIPS approved list.

\[2\] AES-128 and SHA256 are not approved for CNSA but may be needed for backwards compatibility.

\[3\] CBC (Cipher Block Chaining) ciphers do not have proper message
authentication code support. Network scanners report CBC ciphers as a
security risk.

\[*\] SHA1 is not approved.

\[*\] TLS\_RSA\_\* cipher suites use a static key exchange that is
vulnerable in a way that cracking the key for one session allows
all sessions to be decrypted. See "Return Of Bleichenbacher\'s Oracle
Threat ([ROBOT](https://www.robotattack.org/))\", 2018. Static key
exchange algorithms were deprecated in [SP 800-52 Rev2](https://csrc.nist.gov/pubs/sp/800/52/r2/final), Appendix D with a transition date by December 31, 2023. See FIPS 140-2 [IG
D.9](https://csrc.nist.gov/csrc/media/projects/cryptographic-module-validation-program/documents/fips140-2/fips1402ig.pdf).  See transition guidance in [SP 800-131A Rev2](https://csrc.nist.gov/pubs/sp/800/131/a/r2/final) for information on deprecation timelines.

### TLS 1.3 Cipher Suites

TLS 1.3 cipher strings do not specify the server key type. See the section [Asymmetric Cipher Requirement](#asymmetric-cipher-requirement) for guidance.

TLS 1.3 **servers** MUST support the cipher TLS\_AES\_256\_GCM\_SHA384.

TLS 1.3 **clients** MUST support TLS\_AES\_256\_GCM\_SHA384 and may support TLS\_AES\_128\_GCM\_SHA256 if needed for interoperability.

|  Cipher  | RFC Cipher Name        |   Notes       |
| :------  |   :------------------- | :------------ |
| 0x1302   | **TLS\_AES\_256\_GCM\_SHA384** | **MUST, CNSA approved (preferred)** |
| 0x1301   | TLS\_AES\_128\_GCM\_SHA256     | Acceptable, not Ideal  \[1,2\] |

\[1\] AES128 is not acceptable for CNSA customers.

\[2\] SHA256 is not acceptable for CNSA customers.

\[*\] TLS\_CHACHA20\_POLY1305\_SHA256 is not FIPS approved. It does not appear in [SP 800-52 Rev2](https://csrc.nist.gov/pubs/sp/800/52/r2/final).

\[*\] CCM is not acceptable for CNSA customers.

### Symmetric Cipher Requirement

The following describes the requirement for use of symmetric ciphers:

AES - 256-bit keys are preferred. 128-bit key is highly efficient. 192 can be used based on the requirements.

| Alg/BlockSize  |   Notes|
| :------------  |   :------ |
| AES256 | Required for CNSA |
| AES192 | Offers no performance benefits compared to AES256 |

### Asymmetric Cipher Requirement

The following table describes the asymmetric ciphers (for
authentication and digital signature) allowed in GL.

| **Key type/length**   |   **Notes**   |
| :------------  |   :------ |
|  RSA / 2048           |    Minimum for FIPS  |
|  RSA / 3072           |   Minimum for CNSA. Recommended RSA default length|
|  RSA / 4096           | |
|  ECDSA p384            |    (Recommended for speed/security trade-off.)|

### Hashing Algorithms Requirements

The following table describes the hashing algorithms allowed in GL for
security-related functions.

| **Algorithm** | **Notes** |
| :------------ | :-------------- |
| SHA-256       | Acceptable for FIPS. Not allowed for CNSA |
| SHA-384       | **MUST, CNSA approved (preferred)** |
| SHA-512       | CNSA approved |
| SHA3-384      | CNSA approved |
| SHA3-512      | No official indication of CNSA approval |

\[\*\] SHA-384 is the preferred hash algorithm for CNSA.

\[\*\] SHA-512 is acceptable and allowed under FIPS and CNSA 2.0. It was
not permitted under 2015 CNSA guidelines. No official reason was given
for the exclusion of SHA-512 from CNSA, but the theory is that SHA-512
discloses too much of the internal state machine.

\[\*\] SHA1 provides about 63 bits of security strength and should not
be used. See [SHAttered](https://shattered.io/). NIST announced EOL for SHA1
[here](https://csrc.nist.gov/projects/hash-functions/nist-policy-on-hash-functions).

\[\*\] SHA-224 and SHA-512/224 provide only 112 bits of security and should be
phased out well before the 2030 cut-off date.

\[\*\] SHA-512/256 is permitted however they offer little to no security/performance benefits.

## Appendix A - Definitions

**Ciphers**: Algorithms used in cryptography for encryption/decryption,
hashing, and digital signatures.

**Keys**: Keys are used in the algorithms to encrypt/decrypt or other
cryptographic functions. Key values are often displayed in base64-encoded
format or in PEM format (base64 encoding with headers).

**Key Sizes**: Number of bits in the key defined by the algorithm.
Larger the key size stronger the security (it takes longer to break the
key by brute force or other techniques).

**Cipher Suites**\*: Combinations of algorithms used when securing a
network communication (e.g., TLS) which contains:

- Key Exchange Algorithms. Helps in exchanging keys before the
    transmission.

- Authentication or Digital Signature algorithm. Provides
    non-repudiation.

- Encryption algorithm. Provides confidentiality.

- Message Authentication code algorithm. Provides authentication and
    integrity.

For example, here is a decoding of the following cipher suite: TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384

- TLS defines the protocol that this cipher suite is for; it will usually be TLS.

- ECDHE indicates the key exchange algorithm being used.

- RSA authentication mechanism during the handshake.

- AES session cipher.

- 256 session encryption key size (bits) for cipher.

- GCM type of encryption (cipher-block dependency and additional options).

- SHA (SHA2) hash function. Indicates the message authentication algorithm used to authenticate a message.

- 384 digest size (bits).

**Cipher Modes**\*: These represent different procedures for encrypting
blocks of plain text.

\*For more details, see <https://en.wikipedia.org/wiki/Cipher_suite> and <https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation>.

## Appendix B - Configuration Examples

This section is informative only. OpenSSL and all applications that use
OpenSSL follow a similar pattern to configure TLS ciphers. Protocol
selection is in terms of minimum and maximum but is often configured as
a contiguous range; either option is acceptable. TLS 1.3 changed the way
cipher suites are defined. This change also changed the way the ciphers
are configured. Most notably, TLS 1.3 ciphers are configured separately
from TLS 1.2.

### Nginx

Configure TLS 1.3 ciphers with the `ssl_conf_command` option. Configure
TLS 1.2 ciphers with the `ssl_ciphers` option. The following example
demonstrates how to configure SSL 1.3, 1.2, and related ciphers in Nginx:

```nginx
  ssl_protocols TLSv1.3 TLSv1.2;
  ssl_conf_command Ciphersuites TLS_AES_256_GCM_SHA384;
  ssl_ciphers ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384;
  ssl_prefer_server_ciphers on;
```

Additional Nginx TLS configuration information:

- Useful [cipher suite test script](https://serverfault.com/questions/1023766/nginx-with-only-tls1-3-cipher-suites)
- More information about [configuring TLS 1.3, 1.2, or both](https://www.cyberciti.biz/faq/configure-nginx-to-use-only-tls-1-2-and-1-3/)

### Apache

Apache Configuration example:

```ApacheConf
  SSLProtocol -all +TLSv1.3 +TLSv1.2
  SSLCipherSuite TLSv1.3 TLS_AES_256_GCM_SHA384
  SSLCipherSuite TLSv1.2 ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384
```

### OpenSSL

Double check your list by using the openssl application as shown in the example below. Notice the space
between the TLS 1.3 "ciphersuites" value list (of one) and the colon separated list
of TLS 1.2 ciphers.

```shell
openssl version -v && \
openssl ciphers -V --stdname \
--ciphersuites TLS_AES_256_GCM_SHA384 \
"ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384"
```

The above command produces the output:

```shell
OpenSSL 3.1.4 24 Oct 2023 (Library: OpenSSL 3.1.4 24 Oct 2023)
0x13,0x02 - TLS_AES_256_GCM_SHA384 - TLS_AES_256_GCM_SHA384 TLSv1.3 Kx=any Au=any Enc=AESGCM(256) Mac=AEAD
0xC0,0x2C - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 - ECDHE-ECDSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH Au=ECDSA Enc=AESGCM(256) Mac=AEAD
0xC0,0x30 - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 - ECDHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH Au=RSA Enc=AESGCM(256) Mac=AEAD
0x00,0x9F - TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 - DHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=DH Au=RSA Enc=AESGCM(256) Mac=AEAD

```

To set the system default, modify the openssl.cnf file to specify the
TLS 1.2 CipherString and TLS 1.3 Ciphersuites. Depending on which
version of OpenSSL was used, additional section pointers may need to be
inserted back to the top level openssl\_init section.

```ini
  [openssl_init]
  ssl_conf = ssl_module

  [ ssl_module ]
  system_default = crypto_policy

  [ crypto_policy ]
  CipherString = @SECLEVEL=2:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384
  Ciphersuites = TLS_AES_256_GCM_SHA384
  TLS.MinProtocol = TLSv1.2
  TLS.MaxProtocol = TLSv1.3
  DTLS.MinProtocol = DTLSv1.2
  DTLS.MaxProtocol = DTLSv1.2
  SignatureAlgorithms = ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:rsa_pss_pss_sha256:rsa_pss_pss_sha384:rsa_pss_pss_sha512:rsa_pss_rsae_sha256:rsa_pss_rsae_sha384:rsa_pss_rsae_sha512:RSA+SHA256:RSA+SHA384:RSA+SHA512:ECDSA+SHA224:RSA+SHA224
```

## External References

1. [National Security Defense (NSA) Eliminating Obsolete TLS Protocol Configurations Jan 2021](https://media.defense.gov/2021/Jan/05/2002560140/-1/-1/0/ELIMINATING_OBSOLETE_TLS_UOO197443-20.PDF)
2. [Cooley, D. "Commercial National Security Algorithm (CNSA) Suite
    Profile for TLS and DTLS 1.2 and 1.3." IETF and NSA. Sep. 2020](
    https://tools.ietf.org/pdf/draft-cooley-cnsa-dtls-tls-profile-06.pdf)

3. [RFC8446, "The Transport Layer Security (TLS) Protocol Version 1.3",
    August 2018](https://datatracker.ietf.org/doc/pdf/rfc8446)

4. [Implementation Guidance for FIPS 140-2, March 14, 2022](https://csrc.nist.gov/csrc/media/projects/cryptographic-module-validation-program/documents/fips140-2/fips1402ig.pdf)

5. [Announcing the Commercial National Security Algorithm Suite 2.0",
    September 7, 2022](https://www.nsa.gov/Press-Room/News-Highlights/Article/Article/3148990/nsa-releases-future-quantum-resistant-qr-algorithm-requirements-for-national-se/)

6. [The Commercial National Security Algorithm Suite 2.0 and Quantum
    Computing FAQ", Version 2.1 December, 2024](https://media.defense.gov/2022/Sep/07/2003071836/-1/-1/0/CSA_CNSA_2.0_ALGORITHMS_.PDF)

**Original publication date (yyyy-mm-dd):** 2024-04-01
