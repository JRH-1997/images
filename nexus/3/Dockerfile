# Copyright (c) 2016-present Sonatype, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM registry.access.redhat.com/ubi8/ubi-minimal

LABEL author="Sonatype" maintainer="support@sonatype.com"
LABEL       org.opencontainers.image.source="https://github.com/jrh-1997/images"
LABEL       org.opencontainers.image.licenses=MIT


ARG NEXUS_VERSION=3.45.1-01
ARG NEXUS_DOWNLOAD_URL=https://download.sonatype.com/nexus/3/nexus-${NEXUS_VERSION}-unix.tar.gz
ARG NEXUS_DOWNLOAD_SHA256_HASH=36f393fff385f10ba2a1fbea64050eb7005ebae8161335aa56a4c593b3001830

# configure nexus runtime
ENV SONATYPE_DIR=/home/container
ENV NEXUS_HOME=${SONATYPE_DIR}/nexus \
    NEXUS_DATA=${SONATYPE_DIR}/nexus-data \
    NEXUS_CONTEXT='' \
    SONATYPE_WORK=${SONATYPE_DIR}/sonatype-work \
    DOCKER_TYPE='rh-docker'

# Install Java & tar
RUN microdnf update -y \
    && microdnf --setopt=install_weak_deps=0 --setopt=tsflags=nodocs install -y \
    java-1.8.0-openjdk-headless tar procps shadow-utils gzip \
    && microdnf clean all \
    && groupadd --gid 200 -r container \
    && useradd --uid 200 -r container -g container -s /bin/false -d /home/container -c 'Nexus Repository Manager user'

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

# Download nexus & setup directories
RUN curl -L ${NEXUS_DOWNLOAD_URL} --output /home/container/nexus-${NEXUS_VERSION}-unix.tar.gz \
    && echo "${NEXUS_DOWNLOAD_SHA256_HASH} nexus-${NEXUS_VERSION}-unix.tar.gz" > /home/container/nexus-${NEXUS_VERSION}-unix.tar.gz.sha256 \
    && sha256sum -c /home/container/nexus-${NEXUS_VERSION}-unix.tar.gz.sha256 \
    && tar -xvf /home/container/nexus-${NEXUS_VERSION}-unix.tar.gz \
    && rm -f /home/container/nexus-${NEXUS_VERSION}-unix.tar.gz /home/container/nexus-${NEXUS_VERSION}-unix.tar.gz.sha256 \
    && mv /home/container/nexus-${NEXUS_VERSION} $NEXUS_HOME \
    && chown -R container:container ${SONATYPE_WORK} \
    && mv ${SONATYPE_WORK}/nexus3 ${NEXUS_DATA} \
    && ln -s ${NEXUS_DATA} ${SONATYPE_WORK}/nexus3

# Removing java memory settings from nexus.vmoptions since now we use INSTALL4J_ADD_VM_PARAMS
RUN sed -i '/^-Xms/d;/^-Xmx/d;/^-XX:MaxDirectMemorySize/d' $NEXUS_HOME/bin/nexus.vmoptions

RUN echo "#!/bin/bash" >> ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
    && echo "cd /home/container/nexus" >> ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
    && echo "exec ./bin/nexus run" >> ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
    && chmod a+x ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
    && sed -e '/^nexus-context/ s:$:${NEXUS_CONTEXT}:' -i ${NEXUS_HOME}/etc/nexus-default.properties

#RUN microdnf remove -y tar gzip shadow-utils

ENV INSTALL4J_ADD_VM_PARAMS="-Xms2703m -Xmx2703m -XX:MaxDirectMemorySize=2703m -Djava.util.prefs.userRoot=${NEXUS_DATA}/javaprefs"

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]