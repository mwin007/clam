FROM clamp/lib-base:$BUILD_NUMBER
MAINTAINER Rob Landers <rlanders@boomtownroi.com>

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r memcache \
    && useradd -r -g memcache memcache \
    && apt-get-install-min \
    libevent-2.0-5

ENV MEMCACHED_VERSION=1.4.24
ENV MEMCACHED_SHA1=32a798a37ef782da10a09d74aa1e5be91f2861db
ENV buildDeps='curl gcc libc6-dev libevent-dev make perl'

RUN set -x \
    && apt-get update \
    && apt-get-install-min $buildDeps \
    && curl -SL "http://memcached.org/files/memcached-$MEMCACHED_VERSION.tar.gz" -o memcached.tar.gz \
    && echo "$MEMCACHED_SHA1 memcached.tar.gz" | sha1sum -c - \
    && mkdir -p /usr/src/memcached \
    && tar -xzf memcached.tar.gz -C /usr/src/memcached --strip-components=1 \
    && rm memcached.tar.gz \
    && cd /usr/src/memcached \
    && ./configure \
    && make \
    && make install \
    && cd / && rm -rf /usr/src/memcached \
    && apt-get purge -y --auto-remove $buildDeps

ENV MAX_CONN=1024
ENV MAX_REQ=20
ENV FACTOR=1.25
ENV KEY_SIZE=48
ENV THREADS=4
ENV PROTOCOL=auto
ENV SLAB=1m
ENV SIZE=64
ENV DELIMETER=":"

COPY ./run.sh /etc/services.d/memcached/run

EXPOSE 11211