# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_mattermost
COPY vendored/mattermost-src/ /src/
WORKDIR /src/server
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795543317
RUN go build -trimpath -tags "production" -ldflags='-s -w' -o /mattermost ./cmd/mattermost && /mattermost version
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_mmctl
COPY vendored/mattermost-src/ /src/
WORKDIR /src/server
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795543317
RUN go build -trimpath -tags "production" -ldflags='-s -w' -o /mmctl ./cmd/mmctl && /mmctl version
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289
ENV DEBIAN_FRONTEND=noninteractive MM_SERVICESETTINGS_LISTENADDRESS=:8065 MM_SERVICESETTINGS_SITEURL=http://localhost:8065 MM_PLUGINSETTINGS_DIRECTORY=/mattermost/plugins MM_PLUGINSETTINGS_CLIENTDIRECTORY=/mattermost/client/plugins MM_FILESETTINGS_DIRECTORY=/mattermost/data MM_LOGSETTINGS_FILELOCATION=/mattermost/logs MM_SQLSETTINGS_DRIVERNAME=postgres PATH=/mattermost/bin:$PATH
RUN --mount=type=bind,source=vendored/debs,target=/tmp/debs \
    set --; for f in /tmp/debs/*.deb; do [ -e "$f" ] && set -- "$@" "$f"; done; \
    for pass in 1 2 3 4 5; do [ $# -eq 0 ] || dpkg -i --skip-same-version "$@" >/dev/null 2>&1 && break; done; \
    dpkg --configure -a && bad= && for d in "$@"; do p=$(dpkg-deb -f "$d" Package) v=$(dpkg-deb -f "$d" Version); \
      case "$(dpkg-query -W -f='${Version} ${db:Status-Abbrev}' "$p" 2>/dev/null)" in "$v ii"*) ;; *) bad="$bad $p=$v" ;; esac; done && \
    { [ -z "$bad" ] || { echo "vendored debs not installed at their vendored version:$bad" >&2; false; }; } && \
    rm -rf /var/lib/apt/lists/* \
           /var/log/dpkg.log /var/log/alternatives.log /var/log/apt /var/cache/ldconfig/aux-cache /var/cache/fontconfig
ARG SOURCE_DATE_EPOCH=1795543317
RUN groupadd -r mattermost && useradd -r -g mattermost -d /mattermost -m mattermost && \
    mkdir -p /mattermost /mattermost/data /mattermost/logs /mattermost/config /mattermost/plugins /mattermost/client/plugins && chown -R mattermost:mattermost /mattermost /mattermost/data /mattermost/logs /mattermost/config /mattermost/plugins /mattermost/client/plugins && \
    : "${SOURCE_DATE_EPOCH:?SOURCE_DATE_EPOCH must be set for a reproducible build}" && \
    sed -ri "s/^([^:]*:[^:]*):[0-9]*:/\1:$((SOURCE_DATE_EPOCH/86400)):/" /etc/shadow && \
    rm -f /etc/passwd- /etc/shadow- /etc/group- /etc/gshadow-
COPY --from=build_mattermost /mattermost /mattermost/bin/mattermost
COPY --from=build_mmctl /mmctl /mattermost/bin/mmctl
COPY --from=build_mattermost /src/webapp/channels/dist /mattermost/client
COPY --from=build_mattermost /src/server/i18n /mattermost/i18n
COPY --from=build_mattermost /src/server/templates /mattermost/templates
COPY --from=build_mattermost /src/server/fonts /mattermost/fonts
RUN chmod 755 /mattermost/bin/mattermost; chmod 755 /mattermost/bin/mmctl; rm -rf /mattermost/templates/partials /mattermost/templates/*.mjml; chown -R mattermost:mattermost /mattermost/client; chown -R mattermost:mattermost /mattermost/i18n; chown -R mattermost:mattermost /mattermost/templates; chown -R mattermost:mattermost /mattermost/fonts
LABEL org.opencontainers.image.title="Mattermost"
LABEL org.opencontainers.image.version="v12.0.2-CE-ubuntu24.04-r4"
LABEL org.opencontainers.image.licenses="AGPL-3.0-only"
LABEL org.opencontainers.image.source="https://github.com/mattermost/mattermost"
LABEL org.opencontainers.image.description="Self-hosted team messaging — Team Edition, built from source with Enterprise code removed"
LABEL org.opencontainers.image.vendor="ReadyStack.dev"
LABEL readystack.product="mattermost"
LABEL readystack.edition="CE"
LABEL readystack.build_method="source-rebuild-go"
LABEL readystack.license.notice="AGPL-3.0-only; corresponding source is provided in the shipped archive"
LABEL com.readystack.license.disclosure="This image includes Mattermost Team Edition (https://github.com/mattermost/mattermost); its server is licensed under AGPL-3.0-only, its web client and server/templates, server/i18n and server/public under Apache-2.0. Modifications: the Mattermost Source Available tree server/enterprise/ (not AGPL) is removed and replaced by an empty package enterprise stub; mmctl's compliance export command (server/cmd/mmctl/commands/compliance_export.go, which imports that tree) and two tests that import it are deleted; server/go.mod builds against the in-tree server/public module; full diff at https://readystack.dev/source/mattermost/v12.0.2/mattermost-v12.0.2-readystack.patch. Other security patches are applied only via vendored updates to upstream-published Go-module and apt dependencies (unmodified). ReadyStack license holders have access to the image build recipe and patched-package manifest on request via contact@readystack.dev."
LABEL org.opencontainers.image.revision="13ae6c4cf1a8e73c35a4f0c8816a505506c7acdd"
HEALTHCHECK --interval=5s --timeout=30s --start-period=60s \
  CMD curl -fsS -o /dev/null http://127.0.0.1:8065/api/v4/system/ping || exit 1
USER mattermost
EXPOSE 8065
ENTRYPOINT ["/mattermost/bin/mattermost"]
