ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.15

# Loki
# hadolint ignore=DL3029
FROM --platform=linux/amd64 grafana/loki:2.8.5 as loki-amd64
# hadolint ignore=DL3029
FROM --platform=linux/arm64 grafana/loki:2.8.5 as loki-arm64

FROM scratch as container

COPY --from=loki-amd64 /usr/bin/loki /amd64/
COPY --from=loki-arm64 /usr/bin/loki /aarch64/

# hadolint ignore=DL3006
FROM $BUILD_FROM

# Setup base system
ARG BUILD_ARCH=amd64

COPY --from=container /${BUILD_ARCH}/loki /usr/bin/loki

# Copy root filesystem
COPY rootfs /
