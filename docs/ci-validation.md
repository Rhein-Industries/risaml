# Coordinated review validation

The security review spans riptering, ritsp-ltv, ribergshamra and risaml. Draft
PR CI tests immutable sibling commits from `.github/coordinated-stack.json`.
The setup script fetches and verifies those SHAs, applies Cargo path overrides
only on the ephemeral runner, explicitly selects the pinned package versions,
and rejects unrelated dependency version changes. Subsequent commands use
`--locked`. Production manifests and the committed registry lockfile remain
unchanged by the setup script.

Native Linux, Windows and macOS run complete default, explicit RSA opt-in and
crypto-free suites. Linux also runs full RustCrypto, AWS-LC and FIPS provider
suites. SAML operation boundaries initialize the selected provider before
use; the raw XML/LTV siblings have separate focused FIPS fixtures. These tests
do not constitute FIPS certification or physical HSM validation. Provider
capability tests retain RustCrypto success cases and assert documented
unsupported behavior under AWS-LC/FIPS rather than hiding those cases.

Merge and release from the dependency layer upward: riptering, ritsp-ltv, the
ribergshamra workspace in crate dependency order, then risaml. Downstream PRs
must update published minimum dependency versions and registry lockfiles and
remove the temporary CI pins before their final merge. Passing coordinated
source CI does not establish that published or deployed consumers have the
fixes. These workflows do not publish crates or merge PRs.
