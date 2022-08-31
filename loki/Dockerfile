# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/aarch64-base:3.15

# Loki
FROM --platform=linux/amd64 grafana/loki:2.6.1 as loki-amd64
FROM --platform=linux/arm64 grafana/loki:2.6.1 as loki-arm64

FROM scratch as container

COPY --from=loki-amd64 /usr/bin/loki /amd64/
COPY --from=loki-arm64 /usr/bin/loki /aarch64/

FROM $BUILD_FROM

# Execute during the build of the image
ARG TEMPIO_VERSION BUILD_ARCH
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}"

COPY --from=container /${BUILD_ARCH}/loki /usr/bin/loki

# Copy root filesystem
COPY rootfs /
