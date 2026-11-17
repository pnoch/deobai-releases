# Verify a deOBai release

## Checksums

Download the files you use and the matching `SHA256SUMS-<version>.txt`, then
run:

```bash
sha256sum -c SHA256SUMS-<version>.txt
```

## Signatures and attestations

Releases built by the `desktop-release` GitHub workflow also ship:

- `SHA256SUMS` — signed keyless with Sigstore (cosign)
- `SHA256SUMS.sigstore.json` — the verification bundle
- `attestation.jsonl` — GitHub build-provenance attestation for the binaries

Verify the checksums signature:

```bash
cosign verify-blob \
  --bundle SHA256SUMS.sigstore.json \
  --certificate-identity-regexp 'https://github.com/pnoch/deObAI/.github/workflows/desktop-release.yml@refs/(tags/desktop-v.*|heads/main)' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  SHA256SUMS
```

Verify a binary's build provenance:

```bash
gh attestation verify deOBai_<version>_amd64.AppImage \
  --bundle attestation.jsonl \
  --repo pnoch/deObAI
```

The attestation bundle can be verified offline; `gh` may still require a
GitHub login because the source repository is private.

## Which releases have signatures?

The 3.2.1 release predates the workflow signing steps and is checksum-verified
only. Releases built from `desktop-v*` tags after that include the signature
and attestation files.
