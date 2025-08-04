FROM        --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/runtime-deps:9.0-noble

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
