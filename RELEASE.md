# Release Process

This release process is for **risaml**, Rhein Industries' maintained fork of
saml-rs. The repository publishes one crate: `risaml`.

Git tags use `v*`, for example `v0.6.0`.

Publishing is manual for now. The upstream release-plz workflow (which used
crates.io trusted publishing configured for the saml-rs crate) is not part of
the fork, and no workflow in this repository publishes or needs a
crates.io token.

## Manual release

1. Bump `[package] version` in `Cargo.toml` and add the version's entry at the
   top of `CHANGELOG.md` (the changelog is maintained by hand).
2. Refresh `Cargo.lock` (`cargo update --workspace`) and let CI build and test
   the release commit on `main`.
3. Validate the package without uploading:

   ```bash
   cargo publish -p risaml --dry-run
   ```

4. Publish the crate (a maintainer with crates.io ownership of `risaml`):

   ```bash
   cargo publish -p risaml
   ```

5. Create the `vX.Y.Z` tag and the GitHub release.

`ribergshamra` must be published to crates.io before `risaml` can be, because
`risaml` depends on it by version.
