# Provenance — Mattermost (`v12.0.2-CE-ubuntu24.04-r4`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/mattermost/mattermost
- Revision built: `v12.0.2` (commit `13ae6c4cf1a8e73c35a4f0c8816a505506c7acdd`)
- Source tree: `f06be07cd9c787be9e14a7062e138b636acc04dbc9ec9e00b1bae8864589bc35` over 15911 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `ubuntu:24.04`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795543317` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4` — index digest `sha256:052a264eec473733de6c4596c97d14be7b7feda1abfa839584a30b13b0848bea` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:d4fa022f446a94eb07bc6586fdc60703f0c9cb0fea320d83e7d9c7d0a6349c6d` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4`

## Pinned inputs, and why

- 37 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, language runtimes and toolchains 1, dependency versions readystack sets 15, pins in the kit's dockerfiles 9, readystack's build engine 1, dependencies upstream chose 2, services the release checks run beside 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.trail.json`, published beside this archive and signed with the release key (`ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.trail.json.sigstore`), and readable at https://readystack.dev/products/mattermost/trail/v12.0.2-CE-ubuntu24.04-r4.

