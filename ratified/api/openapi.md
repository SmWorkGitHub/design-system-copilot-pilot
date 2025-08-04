# OpenAPI

The OpenAPI Specification, originally known as the Swagger Specification, is a specification for machine-readable interface files for describing, producing, consuming, and visualizing RESTful web services. Originally part of the Swagger framework, it became a separate project in 2016, overseen by the OpenAPI Initiative, an open-source collaboration project of the Linux Foundation. There are numerous tools which can generate code, documentation, and test cases given an interface file.

* <https://www.openapis.org/>
* <https://github.com/OAI/OpenAPI-Specification>

## Version

APIs MUST be documented with a minimum of [version 3.1.0](https://spec.openapis.org/oas/v3.1.0) which was [released in February of 2021](https://www.openapis.org/blog/2021/02/18/openapi-specification-3-1-released).

Legacy APIs SHOULD be [migrated to 3.1.0](https://www.openapis.org/blog/2021/02/16/migrating-from-openapi-3-0-to-3-1-0) unless a technical barrier exists to migrating them.

## Measures of Quality Documentation

Documentation quality should be measured by the degree to which the documentation:

* helps readers accomplish their goals
* is accurate, up-to-date, and comprehensive
* is findable, well organized, and clear

## Best Practices

The OpenAPI initiative maintains a best practices guide which SHOULD be followed:

* [OpenAPI Best Practices](https://learn.openapis.org/best-practices.html)

### Multiple file specification

Of special note, OpenAPI specifications (API Definition) SHOULD be composed of multiple files instead of a single file. Multiple files enable the following benefits:

* Easier to contribute
* Easier to review contributions
* Better supports a docs-like-code workflow.
* Enforces better re-use of objects to avoid duplication and divergence issues
