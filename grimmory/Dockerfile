FROM docker.io/grimmory/grimmory:v3.5.1

COPY <<'EOF' /usr/local/bin/entrypoint-env.sh
#!/bin/sh

apk add jq

for i in $(cat /data/options.json | jq -r '.|to_entries|map("\(.key)=\(.value|tostring)")|.[]'); do
    export $i;
done

exec "/usr/local/bin/entrypoint.sh" "$@"
EOF

RUN chmod +x /usr/local/bin/entrypoint-env.sh

# Build arguments
ARG BUILD_ARCH
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
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Pascal K. <hello@lambdaxcf.com>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Custom Apps" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/xcf-t/hassio-apps" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

ENTRYPOINT [ "/usr/local/bin/entrypoint-env.sh" ]
CMD ["java", "--enable-native-access=ALL-UNNAMED", "--enable-preview", "-jar", "/app/app.jar"]

EXPOSE 6060
