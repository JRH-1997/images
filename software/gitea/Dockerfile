FROM        --platform=$BUILDPLATFORM alpine:latest

LABEL authors="Querijn"

RUN         apk add --update --no-cache ca-certificates tzdata \
			&& adduser -D -h /home/container git

USER        git
ENV         USER=git HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
