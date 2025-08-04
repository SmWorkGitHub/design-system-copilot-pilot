---
seo:
  title: About the HPE GreenLake Development Standard for Python Language Coding | HPE GreenLake Platform
toc:
  enable: true
---

# About the HPE GreenLake Development Standard for Python Language Coding

## Overview

**Any developer who writes code in Python that is deployed as part the HPE GreenLake platform [[1](#def-DeployedAsPartOfPlatform)] and services MUST apply the PEP 8 standard.**

This standard applies in addition to, and does not replace, any other relevant policies applicable to developers at HPE.

## Definitions

[1] <a name="def-DeployedAsPartOfPlatform"></a>For the purposes of this standard, “deployed as part of the HPE GreenLake platform” refers to software components that are exclusively leveraged by the HPE GreenLake platform.

For example:

* Software components that form part of a globally hosted HPE GreenLake solution, including applications such as Aruba Central, or shared hosted services such as GLP Common Cloud Services, are considered in scope.

* Software components running on a device that provides HPE GreenLake functionality (such as the HPE GreenLake Control Plane) are considered in scope.

## Why Does This Standard Matter?

The Python Language Coding Standard is designed to ensure Python code written by HPE developers is (a) written according to established industry best practices, and (b) easily maintained and transferable, within and across teams in HPE. This also facilitates investment in common tooling and training across the company that can benefit all Python developers. Divergent practices increase risk against reliability, bug production, and security.

## What Tools and Practices Can Help a Developer Comply With This Standard?

* PEP 8 - <https://peps.python.org/pep-0008/>
* Black - <https://black.readthedocs.io/en/stable/>
* isort - <https://pycqa.github.io/isort>

### Developers in the Compute Business Unit

Each repository with Python code MUST run formatting validation adhering to the PEP 8 standard. Validation failure prevents merging the PR. This is currently implemented on all repositories.

### Developers in the Storage Business Unit

Each repository with Python code must perform PEP 8 validation of Python code within a PR for the PR to be allowed to be merged.

### Developers in the Aruba Business Unit

PEP 8 formatting validation is enforced as part of CI for all repositories which have Python code and a PR will not be merged if lint stage in CI fails.

### Developers in the Office of the CTO

GLP will integrate a PEP 8 compatible formatter (yet to be decided) with the "super linter" which is to be applied to all CI systems deployed on GLP. Use of the formatter is mandatory for all systems and will be automatically included via GitHub Actions or other CI manager.

## Standing Exceptions

N/A

## Requesting Exceptions

To request an exception to this standard, refer to the exemptions process in our [Governance charter](../governance/index.md#requesting-audit-exemptions).

**Original publication date (yyyy-mm-dd):** 2023-09-14
