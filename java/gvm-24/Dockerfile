FROM container-registry.oracle.com/graalvm/jdk:24

LABEL author="Robert Jansen" maintainer="rjansengd@gmail.com" \
      org.opencontainers.image.source="https://github.com/mcjars/pterodactyl-yolks" \
      org.opencontainers.image.licenses="MIT"

# Remove disk-writing tools you don’t want
RUN rm -f /usr/bin/dd /usr/bin/fallocate /usr/bin/truncate /usr/bin/xfs_mkfile

# ---- use microdnf instead of apt ----
RUN microdnf --enablerepo=ol8_appstream update -y && \
    microdnf install -y \
        curl \
        jq \
        lsof \
        ca-certificates \
        openssl \
        git \
        tar \
        sqlite \
        fontconfig \
        freetype \
        tzdata \
        iproute \
        libstdc++ && \
    microdnf clean all

# Grab MCAntiMalware
RUN curl -sSL https://github.com/OpticFusion1/MCAntiMalware/releases/latest/download/MCAntiMalware.jar \
         -o /MCAntiMalware.jar

# Create unprivileged user
RUN useradd -m -d /home/container container

USER        container
ENV         HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         ["/bin/bash", "/entrypoint.sh"]