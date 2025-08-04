# API Standards Changelog

All notable changes to the API Standards are documented in this file.

## Legend

Changes are grouped by date and by their impact on the project as follows:

* `Added` for new standards.
* `Changed` for changes in existing standards.
* `Deprecated` for once-stable standards to be removed.
* `Removed` for deprecated features removed
* `Fixed` for any bug fixes.
* `Security` for standards changes made for security & vulnerability reasons.

## 2025-06-10

### Changed

* [Query Parameters](./query_parameters.md#select). Updated the `select` query parameter to be applicable for write operations (POST, PUT, PATCH) in addition to read operations (GET). Added examples for write operations.

## 2025-06-02

### Changed

* [GraphQL](./graphql-spec.md). Updated the schema for sorting to allow sorting multiple fields in different directions.

## 2025-05-29

### Changed

* [HTTP Methods](./http.md#http-methods). Clarified that HTTP GET requests should not include a request body.

## 2025-05-20

### Changed

* [Versioning Standards](./versioning.md). Update Deprecation header documentation to reference finalized RFC 9745 (no longer draft).

## 2025-05-16

### Added

* [Bulk and Batch Operations](./bulk-batch.md). Added Batch operations in addition to Bulk operations.

## 2025-05-15

### Changed

* [Referencing Resources](./resource_basics.md#referencing-resources). Updated to include use of type property in the reference object.

## 2025-04-02

### Changed

* [Query Parameters](./query_parameters.md#bulk-operations). Updated the Bulk operations documentation to reflect the deprecation of query parameters.
* [Bulk Operations](./bulk-batch.md). Deprecated the use of query parameters for bulk operations.

## 2025-02-20

### Added

* [Bulk Operations](./bulk-batch.md). A new chapter has been added to define how bulk operations should be implemented.

### Changed

* The existing section for bulk operations using the `id` query parameter has been moved to [Bulk Operations](./bulk-batch.md#query-parameter).

## 2024-11-12

### Added

* [Events Standard](./events-spec.md). A standardized format for all events, based on CloudEvents, has been introduced to ensure consistency across services.

## 2024-09-18

### Added

* [Examples](./example.md). New high-level examples section in the API standards, starting with guidelines on to operate on tags.
This section is intended to be expanded in the future with additional examples to assist developers in following best practices.

## 2024-09-05

### Changed

* [Query Parameters](./query_parameters.md#bulk-operations). Updated the definition of Bulk operations (previously referred to as Batch operations)
to include guidelines and updated terminology.

## 2024-07-30

### Changed

* [URI Naming Conventions](./naming.md#uri-naming-conventions). Updated the URI naming conventions to include the Singleton resource pattern.
* [Resource Basics](./resource_basics.md#singleton-resources). Updated the resource basics to include a definition of the Singleton resource, along with examples.

## 2024-07-23

### Changed

* [Custom Actions](./http.md#custom-actions). Updated the definition of a custom action term to clarify the format is lower case letters and hyphens.

## 2024-03-29

### Changed

* [Standard Query Parameters](./query_parameters.md#standard-query-parameters). Updated the text to use the term *must* instead of *may* to align with GLP convention.
* [Pagination](./pagination.md#pagination). Updated the text to include the `items` property in the response table and to state the items property *must* be used for list responses.

## 2024-03-11

### Changed

* [Asynchronous responses](./http.md#asynchronous-responses): Clarified the use of asynchronous operations and updated the resource properties for `async-operations` responses.
  * See [glcp/doc-portal#16](https://github.com/glcp/doc-portal/pull/16)

## 2024-03-07

### Changed

* [Property Naming](./naming.md#property-naming). Clarified how acronyms and abbreviations are represented in camel case for property names.

## 2024-02-16

### Changed

* [Underscored error codes](./errors.md#more-on-error-codes). Updated the error code formatting guidelines to address API group names that contain hyphens. They are now required to replace hyphens with underscores.

## 2023-11-30

### Changed

* [Mandate error response](./errors.md#error-object). Change from should to MUST.

## 2023-09-14

### Changed

* [Referencing resources](./resource_basics.md#referencing-resources): Clarified how resources should reference other resources.

## 2023-05-22

### Fixed

* [Naming conventions](./naming.md): Changed the layout for *Property Values* to emphasize that some property values must
conform to specific standards.

## 2023-05-18

### Changed

* [Common Properties](./naming.md#common-properties): Clarified the definition of `tags` in the common properties.

## 2023-05-08

### Added

* `private-cloud-biz` and `compute-ops-mgmt` api-groups.

### Changed

* `storage-fabric` api-group to `storage-fabric-mgmt`.

### Removed

* `hybrid-cloud-ops` api group.

## 2023-04-20

### Changed

* [Pagination](./pagination.md#response): Clarified the meaning of `total` in the response.

## 2022-03-06

### Added

* Instructions for registering new API group.

## 2022-11-14

### Added

* Added `HPE_GL_ERROR_METHOD_NOT_ALLOWED` to the standard error codes.

## 2022-09-19

### Added

* Added optional `filter-tags` query parameter for filtering resource collections based on tags.

## 2022-08-05

### Added

* Added `X-B3-Sampled` to the list of [tracing headers](./http.md).
* Documented that `X-B3-SpanId` may change as it passes through a service.

### Fixed

* Fixed the documented header names of `X-B3-TraceId` and `X-B3-SpanId` used for tracing.

## 2022-07-22

### Changed

* [Error code usage and examples clarified](./errors.md)
* Error Code format changed to `HPE_GL_{APIGROUP}_{SPECIFIC_ERROR}`
* Error Code Registry Removed - Prefer to be inline with API docs
* Added mapping to common HTTP codes with discouraged use

## 2022-06-15

### Fixed

* Fixed [conditional requests](./http.md) definition of `if-match` header to align with `412` error response definition.

### Added

* Naming convention for query parameters
* Added `dry-run` as a standard query parameter

### Changed

* Further specified the intent and use of standard query parameters

## 2022-03-31

### Added

* Added `or`, `not` and parentheses `(...)` to filter expressions

## 2022-03-24

### Added

* Added `ne` as a logical operator in filter expressions

### Fixed

* Filter expressions are based on OData v4.0

## 2022-03-10

### Changed

* Renamed required `resourceType` property to `type`

## 2022-03-08

### Fixed

* Fixed [error handling bad request issues](./errors.md) to be object with details

## 2022-02-28

### Added

* Added [error handling format and recommendations](./errors.md)

## 2022-02-16

### Changed

* Allowed underscores in the enum values to separate multiple words

## 2022-02-13

### Changed

* Removed support for `previous` query parameter in the cursor-based pagination.
* Made cursor-based pagination optional for resources that support offset-based pagination.

### Fixed

* Pagination examples

## 2022-01-21

### Added

* Added nesting limitations to URI Naming Conventions
* Added format for representing ranges to Property Values

## 2022-01-19

### Added

* Added [OpenAPI Documentation standards](./openapi.md)

## 2022-01-05

### Changed

* Renamed optional 'displayName' property to 'name'

### Fixed

* Cleaned out references to Tasks that were previously renamed to Async Operations

## 2021-12-05

### Added

* Initial Review of Draft Standards

## 2021-12-10

### Fixed

* Clarified that PUT must not be used to create a resource
