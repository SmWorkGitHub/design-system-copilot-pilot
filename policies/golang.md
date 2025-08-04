---
seo:
  title: About the HPE GreenLake Development Standard for Go Language Coding | HPE GreenLake Cloud Platform
toc:
  enable: true
---

# About the HPE GreenLake Development Standard for Go Language Coding

## Overview

**Any code developed by HPE written in the Go programming language and deployed as part the GreenLake Cloud Platform [[1](#def-DeployedAsPartOfPlatform)] MUST follow the [HPE GreenLake Development Standard for Go Language Coding](../ratified/golang/go.md) and the [HPE GreenLake Go Language Style Guide](../ratified/golang/go_style_guide.md).**

This standard applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Definitions

[1] <a name="def-DeployedAsPartOfPlatform"></a>For the purposes of this standard, “deployed as part of the GreenLake Cloud Platform” refers to software components that are exclusively leveraged by the GreenLake Cloud Platform.

For example:

* Software components that form part of a globally hosted GreenLake solution, including applications such as Aruba Central, or shared hosted services such as GLCP Common Cloud Services, are considered in scope.

* Software components running on a device that provides GreenLake functionality (such as the GreenLake Control Plane) are considered in scope.

## Why does this standard matter?

The Go Language Coding Standard is designed to ensure Go code written by HPE developers is (a) written according to established industry best practices, and (b) easily maintained and transferable, within and across teams in HPE. This also facilitates investment in common tooling and training across the company that can benefit all Go developers. Divergent practices increase risk against reliability, bug production, and security.

## What tools and practices can help a developer comply with this standard?

### For all HPE developers

The HPE GreenLake Development Standard for Go Language Coding can be verified by using the `golangci-lint` with specific configuration files. This tool can in turn be incorporated as a step in a continuous delivery pipelines to ensure it is being complied with.

### Developers in the Storage Business Unit

The Go Chapter within the storage BU is an excellent resource for Go developers. Members of this chapter contributed to the development of these standards.

### Developers in the Office of the CTO

The standard GLCP CI process will include linters for Go projects to ensure most of the elements in this guide are being followed.

## Standing Exceptions

On-device firmware used in GreenLake products and services is not required to follow this standard, however other HPE corporate cybersecurity and product development policies still apply to this software.

Generated code is automatically granted an exception to this standard if it would require changes to be made to the code generator to comply, but developers SHALL comply with any parts of the language guides that can be accommodated by the generator. As an example, many generators provide an option to include a copyright statement, and for generators that provide such an option, it MUST be used to address the corresponding standard and lint violation.

Any code that was written before the adoption of this standard is granted an exception to this standard; there is no expectation that existing code will be refactored or rewritten solely to conform to this standard. Updates and additions to the code after the adoption are not included in this exception and MUST comply.

## Requesting Exceptions

To request an exception to this standard, refer to the Exceptions process in our [Governance charter](../governance/index.md#audit-exemptions-to-policies).

**Original publication date (yyyy-mm-dd):** 2023-06-27
