# HPE GreenLake API Versioning

:::attention Attention

This guides is a super set of the public [API Versioning Basics](/docs/greenlake/guides/public/standards/versioning_basics/) guide which is publicly visible. If you change this guide or the other one, ensure that both pages are kept in sync.

:::

## Introduction

HPE GreenLake APIs are versioned. This document provides guidance to help you understand how versioning is identified and what it means.

Because HPE GreenLake is composed of proprietary and industry standard APIs you may encounter APIs that don't align with the definitions below. In that case, refer to the specific API for more information.

APIs are versioned at the resource collection level. Resource collections may change versions at independent points in time.

## Specifying the Version

All incremental changes to the API must be compatible with clients programmed to an earlier implementation of the same major version. This can be achieved by making all incremental changes additive and optional.

It is not necessary to signal API changes within a major version.

The major version name must be the first element in the path following the api group. Path should ONLY change when major versions are requested / required. Minor and build versions should not be included and are not honored.

* Generally: `/<api-group>/version/<resource-collection>`
* `Alpha`: The version names contain alpha (for example, `v1alpha1`, `v1alpha2`)
* `Beta`: The version names contain beta (for example, `v2beta3`, `v2beta4`)
* `Stable`: The version name is vX where X is an integer of the MAJOR version(for example, `v1`, `v2`)

Examples:

* Stable: `/iam/v1/<resource-collection>`
* Beta: `/iam/v1beta1/<resource-collection>`
* Alpha: `/iam/v1alpha1/<resource-collection>`

## Deprecation and Sunsetting

When a beta or stable Major version is incremented, both the new and old versions of the API must be supported concurrently for a period of time:

* `/<api-group>/v1/...`
* `/<api-group>/v2/...`

The old version of the API is typically considered frozen (with the exception of bug fixes).

Deprecation must be indicated both using 'Deprecation', 'Sunset', 'Link' response headers and using OpenAPI 'deprecated' section.

### Deprecation Header

When the caller makes a request for a deprecated version, this must be indicated via the `Deprecation` response header. The header either contains the deprecation date, or it provides information about when it will be deprecated as prescribed in [RFC 9745](https://www.rfc-editor.org/rfc/rfc9745.html)

| Header      |JSON Type | Value                                                  | Required? |
|-------------|----------|--------------------------------------------------------------|-----------|
| Deprecation | string   | `true` if deprecated or date of deprecation (past or future) conforming to [RFC 5322](https://datatracker.ietf.org/doc/html/rfc5322#section-3.3) | Yes       |

```text
Deprecation: <imf-fixdate timestamp> or "true"
```

### Sunset Header

For versions that are expected to become unresponsive at a specific point in the future, indicate the sunset time using the `Sunset` header.

| Header      |JSON Type | Value                                                        | Required? |
|-------------|----------|--------------------------------------------------------------|-----------|
| Sunset      | string   | [RFC 8594](https://datatracker.ietf.org/doc/html/rfc8594).   | Yes       |

The timestamp given in the "Sunset" header field MUST be later or the same as the one given in the "Deprecation" header field.

Example:

```text
Deprecation: Sun, 11 Nov 2018 23:59:59 GMT
Sunset: Wed, 11 Nov 2020 23:59:59 GMT
```

The example shows that the resource has been deprecated since Sunday, November 11, 2018 at 23:59:59 GMT and its sunset date is Wednesday, November 11, 2020 at 23:59:59 GMT.

```text
Sunset: <timestamp>
```

### Link Header

The Link header with relation type `deprecation` or `sunset` points to the resource documentation that provides additional information about the deprecation of the resource context. It is optional but strongly recommended.

* The Link header can also contain the following relation types:
  * `successor-version` that points to a resource containing the successor version.
  * `latest-version` that points to a resource containing the latest (e.g., current) version.
  * `alternate` that points to a substitute resource.

| Header      |JSON Type | Description                                                  | Required? |
|-------------|----------|--------------------------------------------------------------|-----------|
| Link        | string   | Link to the additional deprecation/sunsetting information.   | No        |

```text
Link: <{link}>; rel="deprecation"; type="text/html"
```

```text
Link: <{link}>; rel="sunset"; type="text/html"
```

### OpenAPI Deprecated Section

Deprecated APIs must be indicated using the `deprecated: true` field in the OpenAPI specification:

```text
  /resource:
    get:
      deprecated: true
```

## Understanding API Version Stages

Feature development proceeds through a series of stages of increasing maturity:

* `alpha`
* `beta`
* `stable`

Each stage has certain expectations and ramifications associated with them. APIs often can be highly mature / stable in one category, but may offer less long term backwards compatibility support, so they have an overall lower designation.

|   | alpha  | beta  | stable  |
|- |- |- |- |
| Naming  | `v1alpha1` → `v1alpha2` ->  | `v1beta1` → `v1beta2` ->  | `v1` |
| Purpose  | Demo-able, works end-to-end but has limitations. Feature not guaranteed to have long term support.  | Usable, not a toy anymore, dependable. Feature intended to be moved to stable when ready where it will receive long term support.  | Production hardened.  |
| API  | No guarantees on backward compatibility.  | APIs are versioned. Breaking changes avoided, but assistance provided if necessary.  | Dependable, production-worthy. APIs are versioned, with automated version conversion for backward compatibility.  |
| Performance  | Not quantified or guaranteed.  | Weakly quantified. Monitored, but not guaranteed.  | Performance (latency/scale) is quantified, documented, with guarantees against regression.  |
| Security  | Zero Trust applies: Must have some level of protection before being deployed to any publicly accessible endpoint. Alpha level permissions - Likely coarse grained. | Zero Trust applies: Must have some level of protection before being deployed to any publicly accessible endpoint. May be mix of Alpha, beta level permissions - Likely coarse grained. | Zero Trust applies: Must have some level of protection before being deployed to any publicly accessible endpoint. Permissions should be stable. Note that even stable permissions may later be made more or less granular and lost as they are backwards compatible.  |
| Operational Support  | Reduced Operational Requirements & SLAs.  | Reduced Operational Requirements & SLAs. Striving for Always available. Upgrades do not cause downtime. Stateless redundancy. No single point of failure. | Standard Operational Requirements & SLAs generally always available. Upgrades do not cause downtime. Stateless redundancy. No single point of failure.  |
| Deprecation Policy  | None required, but best practice is to communicate with potential early integrators and set a timeline for deprecation.  | Weak - 1 month notice until removed unless all usage has been migrated, then will be removed sooner. Will work with teams to prioritize and coordinate deprecation / migration.  | Dependable, firm. Communicated broadly. At least 6 months notice will be provided before changes.  |

## Backwards Compatibility

Before talking about API changes, it is worthwhile to clarify what we mean by API compatibility. We consider forward and backward compatibility of APIs a top priority - after they have reached the stable phase.

An API change is considered compatible if it:

* Adds new functionality that is not required for correct behavior (e.g., does not add a new required field)
* Does not change existing semantics, including:
  * The semantic meaning of default values and behavior
  * Interpretation of existing API types, fields, and values
  * Which fields are required and which are not
  * Mutable fields do not become immutable
  * Valid values do not become invalid
  * Explicitly invalid values do not become valid

Put another way:

* Any API call (e.g. a structure POSTed to a REST endpoint) that succeeded before a change, must succeed after the change.
* Any API call that does not use the change must behave the same way as it did before the change.
* Any API call that uses the change must not cause problems (e.g. crash or degrade behavior) when issued against API servers that do not include the change.
* It must be possible to round-trip the change (convert to different API versions and back) with no loss of information.
* Existing clients need not be aware of the change in order for them to continue to function as they did previously, even when the change is in use.
* It must be possible to rollback to a previous version of API server that does not include the change and have no impact on API objects which do not use the change. API objects that use the change will be impacted in case of a rollback.

### Accidental Loss of Backwards Compatibility

There may be occasions where a backward incompatible change is accidentally
introduced, e.g. due to a bug. In such cases it is acceptable to fix the bug
and restore backward compatibility without changing the major version number.

Similarly, if a new behavior is introduced that is incorrect, it is
acceptable to correct the behavior without changing the major version number.

### Unstable Features to Stable Versions

When adding a feature to an object that is already stable, the new fields and new behaviors need to meet the Stable level requirements. If these conditions cannot be met, then the new field cannot be added to the object. The resource must be delivered as a new major version or under a new (possibly temporary) endpoint at a non-stable version.

### Some reasons for this might include

* The final representation is undecided (e.g. should it be called Width or Breadth?)
* The implementation is not stable enough for general use (e.g. the Area() routine sometimes overflows.)

The developer cannot add the new field unconditionally until stability is met. However, sometimes stability cannot be met until some users try the new feature, and some users are only able or willing to accept a stable version. In that case, the developer has a few options, both of which require staging work over several releases

## Version History / Changelog

When a version change is made, a changelog is provided and released in the API documentation. The concepts are based on: <https://keepachangelog.com/en/1.1.0/>

One change is that we will not use semver, but rather the schema above.

### Guiding Principles

* Changelogs are for humans, not machines.
* There should be an entry for every single version.
* The same types of changes (`Fixed`, `Added`, etc) should be grouped.
* Versions and sections should be linkable.
* The latest version comes first.
* The release date of each version is displayed.
* Mention whether you follow Semantic Versioning. (HPE GreenLake does not. See above)

### Types of changes

* `Added` for new features
* `Changed` for changes in existing functionality
* `Deprecated` for soon-to-be removed features
* `Removed` for now removed features
* `Fixed` for any bug fixes
* `Security` in case of security and vulnerabilities changes

### Example Changelog

```text
# API XYZ Changelog

All notable changes to <XYZ> API are documented in this file.

## Legend

Changes are grouped to describe their impact on the project, as follows:

 * `Added` for new APIs or fields
 * `Changed` for changes in existing APIs
 * `Deprecated` for once-stable standards to be removed.
 * `Removed` for deprecated features removed
 * `Fixed` for any bug fixes.
 * `Security` for standards change made for security & vulnerability reasons.

## v1 - 2022-01-01
### Added
 - This API is now stable.

## v1beta2 - 2021-11-01
### Fixed
 - Issues with XYZ timestamp field updated to conform to specification

### Removed
 - Automatic retries are no longer available and up to the client.

## v1beta1 - 2021-10-01
### Added
  - Promoting to beta

### Security
 - SQL injection validation for APIs

## v1alpha1 - 2021-09-01
### Added
  - New API
  - Lots of fun features
```
