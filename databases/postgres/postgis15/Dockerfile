# ----------------------------------
# Environment: Postgres DB
# ----------------------------------
FROM postgis/postgis:15-3.3-alpine

LABEL author="Izthar" maintainer=""

# UID 999 is the default pterodactyl user
RUN adduser -D -h /home/container container

USER container
ENV HOME /home/container
WORKDIR /home/container

COPY ../entrypoint.sh /entrypoint.sh
CMD ["/bin/bash", "/entrypoint.sh"]
