# Contributing to risaml

risaml is Rhein Industries' maintained fork of
[saml-rs](https://github.com/salasebas/opensaml-rs), an independent,
unofficial Rust SAML 2.0 Service Provider and Identity Provider toolkit.
Issues and pull requests are welcome at
<https://github.com/Rhein-Industries/risaml>. Report security problems
privately as described in [SECURITY.md](SECURITY.md).

## License of contributions

risaml is licensed under the MIT license (see [LICENSE](LICENSE)). By
submitting a contribution you agree that it is licensed under the same terms.
No contributor license agreement and no DCO sign-off are required.

## Setup

```bash
cargo install --locked cargo-nextest
```

risaml requires Rust 1.88. The default `crypto-ribergshamra` feature
(compatibility alias `crypto-bergshamra`) uses `ribergshamra` 0.10 with
`riptering` 0.6 and preserves the RustCrypto-backed defaults.

## Tests

Verify the package plus plausible side effects:

```bash
cargo fmt --all --check
cargo clippy -p risaml --all-targets -- -D warnings
cargo nextest run -p risaml
cargo test -p risaml --doc
RUSTDOCFLAGS="-D warnings -D missing_docs" cargo doc -p risaml --lib --no-deps
cargo test -p risaml --doc --no-default-features
RUSTDOCFLAGS="-D warnings -D missing_docs" cargo doc -p risaml --lib --no-deps --no-default-features
cargo check -p risaml --no-default-features
```

Do not use `--all-features`. Document-crypto providers are mutually exclusive.
AWS-LC, FIPS, and provider-specific rustdoc run in the Linux `provider-matrix`
job in `.github/workflows/ci.yml`. CI runs on GitHub-hosted runners and needs
no repository secrets; pull requests must pass it.

`unwrap_used`, `expect_used`, and `panic` are package `warn` lints, so under
`-D warnings` they fail the build, including tests. Prefer returning
`Result<_, Box<dyn std::error::Error>>` and `?` in tests over `.unwrap()`.

## SAML Behavior Work

When adding or changing SAML behavior:

1. Ground the change in the SAML specifications or targeted interoperability
   evidence.
2. Write a focused Rust test.
3. Implement an idiomatic Rust equivalent with explicit errors.
4. Keep XML cryptography (XML-DSig, XML-Enc, C14N) delegated to
   `ribergshamra` behind the optional `crypto-ribergshamra` feature.

SAML protocol constants (URNs, namespaces, bindings, NameID formats, status
codes) and the files under `tests/fixtures/` come from the OASIS
specifications and from interoperability captures; do not change them to make
a test pass.

Propose new dependencies before adding them, and keep optional integrations
behind feature flags. Do not commit generated or vendor trees.

Historical fixture provenance is documented in `tests/fixtures/PROVENANCE.md`.

## Pull Requests

Use conventional commit-style PR titles where possible.

## Releases

Publishing to crates.io is manual for now and done by the maintainers; see
[RELEASE.md](RELEASE.md).
