# Mattermost: ReadyStack build kit

The offline build kit for the Docker image `readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v12.0.2-CE-ubuntu24.04-r4](https://github.com/readystack-dev/mattermost/releases/tag/v12.0.2-CE-ubuntu24.04-r4)
- Image: [`docker.io/readystack/mattermost:v12.0.2-CE-ubuntu24.04-r4`](https://hub.docker.com/r/readystack/mattermost/tags), manifest digest `sha256:d4fa022f446a94eb07bc6586fdc60703f0c9cb0fea320d83e7d9c7d0a6349c6d`
- Product page: https://readystack.dev/products/mattermost
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=mattermost

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
