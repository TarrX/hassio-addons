#==========================#
# ALEXBELGIUM'S DOCKERFILE #
#==========================#
#           _.------.
#       _.-`    ('>.-`"""-.
# '.--'`       _'`   _ .--.)
#    -'         '-.-';`   `
#    ' -      _.'  ``'--. 
#        '---`    .-'""`
#               /`

#################
# 1 Build Image #
#################

ARG BUILD_FROM
ARG BUILD_VERSION 
FROM ${BUILD_FROM}-14.3.9.99202110311443-7435-01519b5e7ubuntu20.04.1-ls166

##################
# 2 Modify Image #
##################

# Environment variables
ENV \
    DEBIAN_FRONTEND="noninteractive"\
    LANG="C.UTF-8" \
    PS1="$(whoami)@$(hostname):$(pwd)$ " \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_CMD_WAIT_FOR_SERVICES=1 \
    TERM="xterm-256color"

RUN \
    # Allow UID and GID setting
    sed -i 's/bash/bashio/g' /etc/cont-init.d/10-adduser \
    && sed -i 's/{PUID:-911}/(bashio::config "PUID")/g' /etc/cont-init.d/10-adduser \
    && sed -i 's/{PGID:-911}/(bashio::config "PGID")/g' /etc/cont-init.d/10-adduser \
    \
    # Correct permissions 
    && sed -i 's=/config=/config/qBittorrent || true=g' /etc/cont-init.d/10-adduser \    
    && sed -i 's=	/config=/config/qBittorrent || true=g' /etc/cont-init.d/30-config \
    \
    # Set download folder to /share
    && sed -i 's|/downloads/|/share/qBittorrent/|g' /defaults/qBittorrent.conf \
    \
    # Remove fixed folders, allows connection to webUI
    && sed '11,13d' /defaults/qBittorrent.conf \ 
    && echo 'WebUI\HostHeaderValidation=false' >> /defaults/qBittorrent.conf \
    && echo 'WebUI\LocalHostAuth=false' >> /defaults/qBittorrent.conf || true

##################
# 3 Install apps #
##################
# Copy local files
COPY rootfs/ /

# Manual apps 
ARG PACKAGES=""

# Automatic apps & bashio
RUN if ! command -v bash >/dev/null 2>/dev/null; then (apt-get update && apt-get install -yqq --no-install-recommends bash || apk add --no-cache bash); fi && \
    if ! command -v curl >/dev/null 2>/dev/null; then (apt-get update && apt-get install -yqq --no-install-recommends curl || apk add --no-cache curl); fi && \
    curl -L -f -s "https://raw.githubusercontent.com/alexbelgium/hassio-addons/master/zzz_templates/automatic_packages.sh" --output /automatic_packages.sh && \
    chmod 777 /automatic_packages.sh && \
    /automatic_packages.sh "${PACKAGES:-}" && \
    rm /automatic_packages.sh || printf '%s\n' "${PACKAGES:-}" > /ENVFILE

# Modules
ARG MODULES="00-banner.sh 92-local_mounts.sh 92-smb_mounts.sh 90-dns_set.sh"

# Automatic modules download
RUN mkdir -p /tmpscripts /etc/cont-init.d && \
    for scripts in $MODULES; do curl -L -f -s "https://raw.githubusercontent.com/alexbelgium/hassio-addons/master/zzz_templates/$scripts" -o /tmpscripts/"$scripts"; done  && \
    if [ -d /etc/cont-init.d ]; then /bin/cp -rf /tmpscripts/* /etc/cont-init.d/ && chmod -R 755 /etc/cont-init.d; fi && \
    if [ -d /scripts ]; then /bin/cp -rf /tmpscripts/* /scripts/ && chmod -R 755 /scripts; fi && \
    rm -rf /tmpscripts || printf '%s\n' "${MODULES:-}" > /MODULESFILE

################
# 4 Entrypoint #
################

############
# 5 Labels #
############

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
    maintainer="alexbelgium (https://github.com/alexbelgium)" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Add-ons" \
    org.opencontainers.image.authors="alexbelgium (https://github.com/alexbelgium)" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/alexbelgium" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
