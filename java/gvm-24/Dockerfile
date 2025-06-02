FROM container-registry.oracle.com/graalvm/jdk:24

# remove potential disk-filling tools
RUN rm -f /usr/bin/dd /usr/bin/fallocate /usr/bin/truncate /usr/bin/xfs_mkfile

# ---- locale + fonts -------------------------------------------------
RUN microdnf update -y \
 && microdnf install -y glibc-locale-source glibc-langpack-ru \
      google-noto-sans-fonts curl jq lsof ca-certificates openssl git tar \
      sqlite fontconfig freetype tzdata iproute libstdc++ \
 && microdnf clean all

ENV LANG=ru_RU.UTF-8 \
    LANGUAGE=ru_RU:ru \
    LC_ALL=ru_RU.UTF-8 \
    JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF-8"

# ---- app layer ------------------------------------------------------
RUN curl -sSL https://github.com/OpticFusion1/MCAntiMalware/releases/latest/download/MCAntiMalware.jar -o /MCAntiMalware.jar
RUN useradd -m -d /home/container container
USER container
WORKDIR /home/container

COPY entrypoint.sh /entrypoint.sh
CMD ["/bin/bash", "/entrypoint.sh"]
