# Home Assistant add-on Dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/base-python:3.14-alpine3.23
FROM ${BUILD_FROM}

# Set shell for add-on compatibility
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install build dependencies and install application dependencies
RUN \
    apk add --no-cache --virtual .build-dependencies \
        gcc \
        musl-dev \
        python3-dev

# Copy application source to /app
WORKDIR /app
COPY src/ ./src/
COPY pyproject.toml poetry.lock* ./

# Install Poetry and production dependencies only
RUN \
    pip3 install --no-cache-dir poetry \
    && poetry config virtualenvs.create false \
    && poetry install --no-root --only main --no-interaction --no-ansi \
    && apk del .build-dependencies

# Copy and setup run script
COPY run.sh /
RUN chmod a+x /run.sh

# Expose WebSocket port
EXPOSE 9000

# Labels for Home Assistant add-on
LABEL \
    io.hass.name="OCPP Proxy" \
    io.hass.description="Multi-version OCPP proxy for secure EV charger sharing" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version="${BUILD_VERSION}" \
    maintainer="OpenChargeHub" \
    org.opencontainers.image.title="OCPP Proxy" \
    org.opencontainers.image.description="Multi-version OCPP proxy for secure EV charger sharing" \
    org.opencontainers.image.vendor="OpenChargeHub" \
    org.opencontainers.image.authors="OpenChargeHub" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/openchargehub/ocpp-proxy" \
    org.opencontainers.image.source="https://github.com/openchargehub/ocpp-proxy" \
    org.opencontainers.image.documentation="https://github.com/openchargehub/ocpp-proxy" \
    org.opencontainers.image.created="${BUILD_DATE}" \
    org.opencontainers.image.revision="${BUILD_REF}" \
    org.opencontainers.image.version="${BUILD_VERSION}"

# Start the run script
CMD [ "/run.sh" ]
