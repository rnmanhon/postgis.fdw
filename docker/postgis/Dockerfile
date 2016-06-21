FROM mdillon/postgis

ENV PLV8_VERSION=v1.5.3 \
    PLV8_SHASUM="fac8052c926c9ece74f655500caeca50552c0c4b4c7081c0c7946e06ed114d1c  v1.5.3.tar.gz"

RUN buildDependencies="build-essential \
    ca-certificates \
    curl \
    git-core \
    unzip \
    cmake \
    libexpat1-dev \
    expat \
    libboost-all-dev \
    liblua5.1-0-dev \
    postgresql-server-dev-$PG_MAJOR" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ${buildDependencies} \
  && mkdir -p /tmp/build \
  && curl -o /tmp/build/${PLV8_VERSION}.tar.gz -SL "https://github.com/plv8/plv8/archive/$PLV8_VERSION.tar.gz" \
  && cd /tmp/build \
  && echo ${PLV8_SHASUM} | sha256sum -c \
  && tar -xzf /tmp/build/${PLV8_VERSION}.tar.gz -C /tmp/build/ \
  && cd /tmp/build/plv8-${PLV8_VERSION#?} \
  && make static \
  && make install \
  && strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8.so \
  && cd / \

  && mkdir -p /tmp/osm2pgrouting \
  && curl -o /tmp/osm2pgrouting/master.zip -SL "https://github.com/pgRouting/osm2pgrouting/archive/master.zip" \
  && cd /tmp/osm2pgrouting \
  && unzip master.zip \
  && cd osm2pgrouting-master \
  && cmake -H. -Bbuild \
  && cd build \
  && make \
  && make install \
  && cd / \

  && mkdir -p /tmp/pllua \
  && curl -o /tmp/pllua/master.zip -SL "https://github.com/pllua/pllua/archive/master.zip" \
  && cd /tmp/pllua \
  && unzip master.zip \
  && cd pllua-master \
  && export PG_CONFIG='/usr/bin/pg_config' \
  && make \
  && make install \
  && cd / \

  && apt-get clean \
  && apt-get remove -y  ${buildDependencies} \
  && apt-get autoremove -y \
  && rm -rf /tmplbuild /var/lib/apt/lists/* \
  && rm -rf /tmp/osm2pgrouting /tmp/pllua

ENV PGROUTING_MAJOR 2.1
COPY ./pgrouting.list /etc/apt/sources.list.d/pgrouting.list

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 26568B05B65ADE33 && \
    apt-get update && \
    apt-get install  -y --no-install-recommends postgresql-$PG_MAJOR-pgrouting

RUN mkdir -p /docker-entrypoint-initdb.d
COPY ./initdb-pgrouting.sh /docker-entrypoint-initdb.d/routing.sh

COPY ./initdb-pllua.sh /docker-entrypoint-initdb.d/pllua.sh



