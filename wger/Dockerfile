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

ARG BUILD_VERSION
FROM wger/devel:latest

##################
# 2 Modify Image #
##################

VOLUME [ "/data" ]
USER root

RUN touch /data/database.sqlite \
    && chown wger /data/database.sqlite \
    && ln -s /data/database.sqlite /home/wger/db

##################
# 3 Install apps #
##################

# Modules
ARG MODULES="00-banner.sh"

# Automatic modules download
RUN mkdir -p /tmpscripts /etc/cont-init.d && \
    for scripts in $MODULES; do curl -L -f -s "https://raw.githubusercontent.com/alexbelgium/hassio-addons/master/zzz_templates/$scripts" -o /tmpscripts/"$scripts"; done  && \
    if [ -d /etc/cont-init.d ]; then /bin/cp -rf /tmpscripts/* /etc/cont-init.d/ && chmod -R 755 /etc/cont-init.d; fi && \
    if [ -d /scripts ]; then /bin/cp -rf /tmpscripts/* /scripts/ && chmod -R 755 /scripts; fi && \
    rm -rf /tmpscripts || printf '%s\n' "${MODULES:-}" > /MODULESFILE

################
# 4 Entrypoint #
################

#RUN chmod 777 /entrypoint.sh
#WORKDIR /
#ENTRYPOINT [ "/usr/bin/env" ]
#CMD [ "/entrypoint.sh" ] 
#SHELL ["/bin/bash", "-o", "pipefail", "-c"]

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
