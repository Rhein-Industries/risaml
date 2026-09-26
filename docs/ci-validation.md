# Release validation

CI builds the published crates.io dependency graph from `Cargo.toml` and the
committed registry `Cargo.lock`, with `--locked` on build and test commands.
There are no sibling checkouts, path overrides or temporary source pins. The
manifest requires ribergshamra 0.11.0 or a compatible patch release, on the
matching riptering and ritsp-ltv release lines.

Dependency-policy checks run cargo-deny on the host with the same committed
registry lockfile, so all provider audits inspect the graph used by the test
jobs. The audit retains its advisory, license, ban and source policies.

Native Linux, Windows and macOS run complete default, explicit RSA opt-in and
crypto-free suites. Linux also runs full RustCrypto, AWS-LC and FIPS provider
suites. SAML operation boundaries initialize the selected provider before
use; the raw XML/LTV siblings have separate focused FIPS fixtures. These tests
do not constitute FIPS certification or physical HSM validation. Provider
capability tests retain RustCrypto success cases and assert documented
unsupported behavior under AWS-LC/FIPS rather than hiding those cases.

Publish riptering, ritsp-ltv and the ribergshamra workspace in crate dependency
order before refreshing the risaml registry lockfile. The release commit must
pass native CI against that graph before merging and publishing risaml. Validate
the package with `cargo publish -p risaml --locked --dry-run` from a clean
checkout and Cargo configuration without local patches, and review
`cargo package -p risaml --list --locked` for license, documentation and fixture
coverage. Do not use `--allow-dirty` for release validation. These workflows do
not publish crates or merge PRs; successful CI does not establish that deployed
consumers upgraded.
