# Verify a deOBai release

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
cosign verify-blob \
  --bundle SHA256SUMS.sigstore.json \
  --certificate-identity-regexp 'https://github.com/pnoch/deObAI/.github/workflows/(linux-release|desktop-release).yml@refs/(tags/.*|heads/main)' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  SHA256SUMS
```

The signature is recorded in the public Sigstore transparency log; verifying it
does not require access to the private source repository.

## Which releases have signatures?

The 3.2.1 release predates the workflow signing steps and is checksum-verified
only. 3.2.2 and later releases built from `v*` or `desktop-v*` tags include
`SHA256SUMS` and `SHA256SUMS.sigstore.json`.

GitHub build attestations are not available for user-owned private source
repositories, so the Sigstore signature is the provenance record for these
releases.
