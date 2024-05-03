FROM        ghcr.io/parkervcp/yolks:debian

LABEL authors="Querijn"

RUN         apt update \
            && useradd -m -d /home/container git

USER        git
ENV         USER=git HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
