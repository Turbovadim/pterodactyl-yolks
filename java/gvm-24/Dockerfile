FROM container-registry.oracle.com/graalvm/jdk:24

# remove disk-filling tools (unchanged)
RUN rm -f /usr/bin/dd /usr/bin/fallocate /usr/bin/truncate /usr/bin/xfs_mkfile

# --- Locale + fonts ----------------------------------------------------------
RUN microdnf update -y \
 && microdnf install -y glibc-langpack-ru google-noto-sans-fonts \
 && localedef -i ru_RU -f UTF-8 ru_RU.UTF-8 \
 && microdnf clean all

ENV LANG=ru_RU.UTF-8 \
    LANGUAGE=ru_RU:ru \
    LC_ALL=ru_RU.UTF-8 \
    JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF-8"

# everything else exactly as before …
RUN microdnf install -y curl jq lsof ca-certificates openssl git tar sqlite fontconfig freetype tzdata iproute libstdc++ \
 && microdnf clean all

RUN curl -sSL https://github.com/OpticFusion1/MCAntiMalware/releases/latest/download/MCAntiMalware.jar \
      -o /MCAntiMalware.jar

RUN useradd -m -d /home/container container
USER container
WORKDIR /home/container

COPY entrypoint.sh /entrypoint.sh
CMD ["/bin/bash", "/entrypoint.sh"]