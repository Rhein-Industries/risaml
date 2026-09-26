# Release Process

This release process is for **risaml**, Rhein Industries' maintained fork of
saml-rs. The repository publishes one crate: `risaml`.

Git tags use `v*`, for example `v0.7.0`.

Publishing is manual for now. The upstream release-plz workflow (which used
crates.io trusted publishing configured for the saml-rs crate) is not part of
the fork, and no workflow in this repository publishes or needs a
crates.io token.

## Manual release

1. Bump `[package] version` in `Cargo.toml` and add the version's entry at the
   top of `CHANGELOG.md` (the changelog is maintained by hand).
2. Publish the required lower crates first and set the matching minimum
   dependency versions. Refresh the registry `Cargo.lock` without local path
   patches, then let native CI build and test the release commit before merging
   it to `main`. Provider features are mutually exclusive; do not use
   `--all-features`.
   After merging, confirm that CI passes on the resulting `main` commit before
   publication.
3. Validate the package without uploading:

   ```bash
   cargo package -p risaml --list --locked
   cargo publish -p risaml --locked --dry-run
   ```

   Use a clean checkout and Cargo configuration without source overrides; inspect
   the extracted package for its license, linked documentation and test fixtures.
   Do not use `--allow-dirty` or `--no-verify` to bypass release checks.

4. Publish the crate (a maintainer with crates.io ownership of `risaml`):

   ```bash
   cargo publish -p risaml --locked
   ```

5. Create the `vX.Y.Z` tag and the GitHub release.

`ribergshamra` must be published to crates.io before `risaml` can be, because
`risaml` depends on it by version.
