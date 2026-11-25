# Mattermost v12.0.2 — ReadyStack release `v12.0.2-CE-ubuntu24.04-r4`

Generated 2026-11-25 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4` (linux/amd64). Index digest `sha256:052a264eec473733de6c4596c97d14be7b7feda1abfa839584a30b13b0848bea` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:d4fa022f446a94eb07bc6586fdc60703f0c9cb0fea320d83e7d9c7d0a6349c6d` [`TARGET_DIGEST.txt`].
- **Upstream:** Mattermost v12.0.2 built from source at `v12.0.2` of https://github.com/mattermost/mattermost — upstream notes: https://github.com/mattermost/mattermost/releases/tag/v12.0.2 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** `ubuntu:24.04`; runtime user `mattermost`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v12.0.2-CE-ubuntu24.04-r3`.

## What changed in this release

- Upstream version unchanged (v12.0.2); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `golang.org/x/crypto` at least `v0.56.0`: not raised, upstream already pins `v0.57.0` (shipped: `v0.58.0`)
  - `google.golang.org/grpc` raised from upstream's `v1.84.0` to at least `v1.85.0-dev.0.20260825072537-93e31b48545e` (shipped: `v1.85.0-dev.0.20260825072537-93e31b48545e`)
  - `golang.org/x/net` raised from upstream's `v0.58.0` to at least `v0.62.0` (shipped: `v0.62.0`)
  - `github.com/klauspost/compress` raised from upstream's `v1.19.2` to at least `v1.20.2` (shipped: `v1.20.2`)
  - `github.com/andybalholm/brotli` at least `v1.2.7`: not raised, upstream already pins `v1.2.7` (shipped: `v1.2.7`)
- Mattermost v12.0.2 is unchanged from the release this supersedes (v12.0.2-CE-ubuntu24.04-r3); this release rebuilds it. Changes: libssh-4 moves from 0.10.6-2ubuntu0.5 to 0.10.6-2ubuntu0.6 (USN-9187-1). Settings, ports and data volumes are those of v12.0.2-CE-ubuntu24.04-r3: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-25)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `mattermost` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** The first account created on a new server becomes the System Admin without an invitation. Create it before exposing the port.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:d4fa022f446a94eb07bc6586fdc60703f0c9cb0fea320d83e7d9c7d0a6349c6d`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 2 of 2 use cases passed (3 authored, 2 kept in the suite; 1 not exercised, see below) against the running image [`docs/validation/test-results.json`].
- The cases are `GET /api/v4/system/ping` (200), `GET /api/v4/config/client?format=old` (200); the suite checks nothing else.
- 1 further authored check was not exercised and is not counted above: it (`GET /`) was answered with the same page the build's test container served for a made-up path, so it could not show the path exists (listed under Not exercised in `docs/validation/FAILED_TESTS.md`).

## Known issues

- **high** (upstream): First visitor can claim the administrator account until setup is completed [`RSD-mattermost-75550c2951`]
- **low** (ReadyStack package): Admin handbook: Says the paths have no counterpart in this image, but IMAGE_CONFIG.json Env sets all three (not as volumes). [`RSD-mattermost-9cac6f4bd2`] (carried from `v12.0.2-CE-ubuntu24.04-r2`, recorded there as `RSD-mattermost-7752f4a7ea`)

## Defects fixed

- 1 problem found before release and fixed before it shipped (1 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

