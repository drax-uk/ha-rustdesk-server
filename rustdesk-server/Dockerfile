ARG BUILD_FROM=ghcr.io/hassio-addons/base:21.0.10
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Pinned RustDesk Server release and its upstream asset digest
# (from the GitHub release asset metadata). Renovate bumps the version;
# update the digest in the same change or the build fails on purpose.
ARG BUILD_ARCH=amd64
ARG RUSTDESK_SERVER_VERSION=1.1.16
ARG RUSTDESK_SERVER_SHA256_AMD64=0565c41affe6c3f409b0bddfaf5a24ccbf3f64f5f8e3fec250b69a0d5f6bdbcf

# hadolint ignore=DL3018
RUN \
    if [ "${BUILD_ARCH}" != "amd64" ]; then \
      echo "Unsupported architecture: ${BUILD_ARCH}"; \
      exit 1; \
    fi \
    && apk add --no-cache --virtual .build-deps \
        curl \
        unzip \
    && mkdir -p /opt \
    && curl --fail --location --retry 3 --output /tmp/rustdesk-server.zip \
        "https://github.com/rustdesk/rustdesk-server/releases/download/${RUSTDESK_SERVER_VERSION}/rustdesk-server-linux-amd64.zip" \
    && echo "${RUSTDESK_SERVER_SHA256_AMD64}  /tmp/rustdesk-server.zip" | sha256sum -c \
    && unzip /tmp/rustdesk-server.zip -d /tmp/rustdesk-server \
    && mv /tmp/rustdesk-server/amd64 /opt/rustdesk-server \
    && chmod +x \
        /opt/rustdesk-server/hbbs \
        /opt/rustdesk-server/hbbr \
        /opt/rustdesk-server/rustdesk-utils \
    && rm -rf /tmp/rustdesk-server.zip /tmp/rustdesk-server \
    && apk del --no-cache --purge .build-deps

COPY rootfs /

# s6 runs these directly, so make sure they are executable even if git
# (or a Windows checkout) dropped the executable bit.
RUN chmod +x \
    /etc/s6-overlay/s6-rc.d/hbbs/run \
    /etc/s6-overlay/s6-rc.d/hbbs/finish \
    /etc/s6-overlay/s6-rc.d/hbbr/run \
    /etc/s6-overlay/s6-rc.d/hbbr/finish \
    /etc/s6-overlay/s6-rc.d/key-secret/up \
    /etc/s6-overlay/s6-rc.d/key-secret/up.real

ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="app" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Drax <https://github.com/drax-uk>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.authors="Drax <https://github.com/drax-uk>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/drax-uk/ha-rustdesk-server" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/drax-uk/ha-rustdesk-server/blob/main/rustdesk-server/DOCS.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
