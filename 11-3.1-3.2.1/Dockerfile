FROM postgis/postgis:11-3.1

LABEL maintainer="pgRouting Project - https://pgrouting.net"

ENV PGROUTING_VERSION 3.2.1
ENV PGROUTING_SHA256 daeb7ba8703dde9b6cc84129eab64a0f2e1f819f00b9a9168a197c150583a5fd

RUN set -ex \
 && apt update \
 && apt install -y \
        libboost-atomic1.62.0 \
        libboost-chrono1.62.0 \
        libboost-graph1.62.0 \
        libboost-date-time1.62.0 \
        libboost-program-options1.62.0 \
        libboost-system1.62.0 \
        libboost-thread1.62.0 \
        libcgal12 \
 && apt install -y \
        build-essential \
        cmake \
        wget \
        libboost-graph-dev \
        libcgal-dev \
        libpq-dev \
        postgresql-server-dev-${PG_MAJOR} \
 && wget -O pgrouting.tar.gz "https://github.com/pgRouting/pgrouting/archive/v${PGROUTING_VERSION}.tar.gz" \
 && echo "$PGROUTING_SHA256 *pgrouting.tar.gz" | sha256sum -c - \
 && mkdir -p /usr/src/pgrouting \
 && tar \
        --extract \
        --file pgrouting.tar.gz \
        --directory /usr/src/pgrouting \
        --strip-components 1 \
 && rm pgrouting.tar.gz \
 && cd /usr/src/pgrouting \
 && mkdir build \
 && cd build \
 && cmake .. \
 && make \
 && make install \
 && cd / \
 && rm -rf /usr/src/pgrouting \
 && apt-mark manual postgresql-11 \
 && apt purge -y --autoremove \
        build-essential \
        cmake \
        wget \
        libcgal-dev \
        libpq-dev \
        libboost-graph-dev \
        postgresql-server-dev-${PG_MAJOR} \
 && rm -rf /var/lib/apt/lists/*
RUN rm /docker-entrypoint-initdb.d/10_postgis.sh
