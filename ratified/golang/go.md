---
seo:
  title: Go Language Standard | HPE GreenLake Cloud Platform
---

# HPE GreenLake Development Standard for Go Language Coding

Read this document for guidance writing Go code for HPE Engineering.
This document extends guidance from other engineering standards and requirements.
In cases where this guidance conflicts, please contact the Standards Owners group to update the document accordingly.

NOTE: _This document follows [RFC2119] standards_.

## Definitions

- A `Program` is a program, service, or application which is NOT a library.
- A `Library` is code designed only for consumption by other programs.

## Requirements

- Builds MUST use the [Go-provided compiler][1].
  Builds MUST NOT use the [gcc-go][2] compiler or other alternatives.
- Programs MUST update and commit the `go.mod` and `go.sum` using `go mod tidy`.
- Programs SHOULD [vendor dependency code][4] and commit the vendored code to their repository.
- CI builds MUST use the [golangci-lint][5] linter as a first-stage validation step
- Programs SHOULD use the [standard project layout][3]
- Programs MUST NOT use CGO unless there is no pure-Go alternative.
  Appropriate uses of CGO include Oracle DB drivers, GPGPU computation.
   (Prefer `CGO_ENABLED=0` as a build flag.)
- Repositories MUST include `go.sum` and `go.mod` in the `.copyrightignore` file.
  This ensures generated files do not trigger the [copyright tool](https://github.com/hpe-hcss/copyright-tool) during builds.
- Programs SHOULD follow the [HPE Go Style Guide](/docs/greenlake/standards/ratified/golang/go_style_guide.md)

## Recommended Resources

- [Effective Go](https://golang.org/doc/effective_go.html)
- [Go Proverbs](https://go-proverbs.github.io/)
- [Go Test package](https://pkg.go.dev/testing)
- [Go training](https://hpe.percipio.com/channels/99520c21-1a26-11e7-aa4b-c7a8e598b690?tab=WATCH) from Percipio, available free to HPE employees
  - Benchmark your skills with the Percipio Go skills [benchmarks](https://hpe.percipio.com/search?categories=Skill%20Benchmark&q=Go%20)
- [DSCC Go Chapter repository](https://github.hpe.com/cloud/go-chapter)
  - Contains material for training new Go developers, tutorials on design patterns and other useful documentation

## Supplemental Advice

Developers SHOULD follow these recommendations in addition to recommendations and requirements for [secure architecture design](../../policies/secure_design_and_architecture_policy.md) and [secure coding and reviews](/docs/greenlake/standards/ratified/secure_coding/secure_coding_and_reviews.md).

### Build / Pre-commit

- Run `go build`, `golangci-lint run`, and `go test` before submitting your PR.
  The processes MUST all complete without error or response codes (i.e. successfully).

### Language

- Errors MUST be handled.
- SHOULD NOT use `panic`. [Use errors](https://go.dev/doc/faq#exceptions).
- Programs and Libraries SHOULD prefer Go standard library and `golang.org/x` packages.
  - If third-party open source libraries are used, ensure the [mandatory HPE corporate requirements are followed](/docs/greenlake/standards/ratified/secure_coding/secure_coding_and_reviews.md#third-party-and-open-source-components)
- MUST use [gofumpt](https://github.com/mvdan/gofumpt) for formatting.
- Feature flags MUST NOT be used for data migrations.
- Test functions MUST check both the error result and the returned data.

[RFC2119]:https://www.rfc-editor.org/rfc/rfc2119.txt
[1]: https://golang.org
[2]: https://gcc.gnu.org/onlinedocs/gccgo/
[3]: https://github.com/golang-standards/project-layout
[4]: https://golang.org/ref/mod#vendoring
[5]: https://golangci-lint.run

**Original publication date (yyyy-mm-dd):** 2022-12-07
