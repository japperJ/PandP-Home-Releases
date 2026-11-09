# PandP Home Releases

Public, signed release artifacts for **PandP Home**.

The application source lives in a private repository. This repository contains
only built release assets, the update manifest and its signature.

## Latest release

Devices poll:

```text
https://github.com/japperJ/PandP-Home-Releases/releases/latest/download/latest.json
```

The signed manifest points at the versioned archive in the same release.

## Assets in each release

| Asset | Purpose |
|---|---|
| `pandp-app-<version>.tar.gz` | App web tree (`version.json`, `index.html`, `sw.js`, `cgi-bin/`, `system/`) |
| `latest.json` | Signed update manifest |
| `latest.json.sig` | Base64 Ed25519 signature of the exact `latest.json` bytes |
| `SHA256SUMS` | Checksums for the release assets |

## Verify a release

```sh
VERSION=1.0.0
BASE=https://github.com/japperJ/PandP-Home-Releases/releases/download/v$VERSION

curl -fsSLO "$BASE/pandp-app-$VERSION.tar.gz"
curl -fsSLO "$BASE/latest.json"
curl -fsSLO "$BASE/latest.json.sig"
curl -fsSLO "$BASE/SHA256SUMS"
sha256sum -c SHA256SUMS
```

Signature verification uses the public key in the private source repository
(`release.pub`). The updater embeds the same key.

## Publishing

Releases are built, signed and published from the private source repository with
`scripts/publish-release.sh`. The signing private key never enters this
repository and never enters CI.

Do not upload or replace assets manually. Always publish through the release
script so the manifest, archive hash and signature stay consistent.
