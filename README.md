# Mattermost: ReadyStack build kit

The offline build kit for the Docker image `readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v12.0.2-CE-ubuntu24.04-r4](https://github.com/readystack-dev/mattermost/releases/tag/v12.0.2-CE-ubuntu24.04-r4)
- Image: [`docker.io/readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4`](https://hub.docker.com/r/readystack/mattermost/tags), manifest digest `sha256:d4fa022f446a94eb07bc6586fdc60703f0c9cb0fea320d83e7d9c7d0a6349c6d`
- Product page: https://readystack.dev/products/mattermost
- Source and build recipe: https://readystack.dev/products/mattermost/recipe
- Build trail: https://readystack.dev/products/mattermost/trail/v12.0.2-CE-ubuntu24.04-r4
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=mattermost

## The image

Single container running the mattermost server in the foreground, serving both the REST API and the compiled web client, against an EXTERNAL PostgreSQL 15 or later database (Mattermost 12 raised the minimum from 14; against an older server it logs `Unsupported Postgres version` at start and keeps running, which upstream does not support), which must be reachable by name from the container: a postgres container goes on a user-defined Docker network that the mattermost container joins too (on the default bridge network a container name such as your-db does not resolve and the server exits). This is Team Edition — the free, open-source edition — built from source with all Enterprise code removed rather than merely disabled.

## Who uses Mattermost and why

Technical teams, such as software, security and IT operations groups, that want team chat on infrastructure they control.

Mattermost is a self-hosted collaboration platform with public and private channels, direct messages and threads on web, desktop and mobile, extended through webhooks, slash commands, bots and plugins.

A typical use: A company's engineering and operations teams need chat that stays on their own servers. In Mattermost each service has a channel, monitoring alerts arrive through incoming webhooks, and during an incident the responders coordinate in one channel from a laptop or a phone.

From the upstream project's own description: https://github.com/mattermost/mattermost, https://docs.mattermost.com/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-25 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 121 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`mattermost`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-25; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/mattermost.

Mattermost is distributed under its upstream licence (AGPL-3.0-only); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v12.0.2-CE-ubuntu24.04-r4 release](https://github.com/readystack-dev/mattermost/releases/tag/v12.0.2-CE-ubuntu24.04-r4): `ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.tar.gz` (1.2 GB), SHA-256 `7bd1889973709db77f7f65a34ab73cc39dc2e9d2c797ff18a8012de6b81cef59`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.rebuild.json.sigstore ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.tar.gz
cd v12.0.2-CE-ubuntu24.04-r4
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-mattermost-v12.0.2-CE-ubuntu24.04-r4.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/mattermost/recipe. The complete kit is the release download. Each release is a tag here.

— ReadyStack · https://readystack.dev
