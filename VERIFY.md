# Verify a deObAI release

## Checksums

Download the files you use and the matching checksum file (`SHA256SUMS`, or
`SHA256SUMS-<version>.txt` for the manual 3.2.1 release), then run:

```bash
sha256sum -c SHA256SUMS
```

## Sigstore signature

Releases built by the `linux-release` or `desktop-release` GitHub workflows also
ship:

- `SHA256SUMS` — signed keyless with Sigstore (cosign)
- `SHA256SUMS.sigstore.json` — the verification bundle

Verify the signature:

```bash
tag=v3.2.19           # the release you downloaded
workflow=linux-release # desktop-release for desktop-v* builds

cosign verify-blob \
  --bundle SHA256SUMS.sigstore.json \
  --certificate-identity "https://github.com/pnoch/deObAI/.github/workflows/${workflow}.yml@refs/tags/${tag}" \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  SHA256SUMS

Use the exact identity for the release you downloaded. Exact matching avoids
accepting a signature from a different tag or workflow.
```

The signature is recorded in the public Sigstore transparency log; verifying it
does not require access to the private source repository.

## Third-party notices

Each release includes `THIRD_PARTY_NOTICES.md`, a summary of the production npm
packages and Rust crates with their licenses and repositories, and
`THIRD_PARTY_LICENSES.md` with the full license texts. The project's own MIT
license is in `LICENSE`.

## Which releases have signatures?

The 3.2.1 release predates the workflow signing steps and is checksum-verified
only. 3.2.2 and later releases built from `v*` or `desktop-v*` tags include
`SHA256SUMS` and `SHA256SUMS.sigstore.json`.

GitHub build attestations are not available for user-owned private source
repositories, so the Sigstore signature is the provenance record for these
releases.
