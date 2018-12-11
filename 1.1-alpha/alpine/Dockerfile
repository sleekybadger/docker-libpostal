FROM alpine:3.8

LABEL version="1.0.0" \
      maintainer="sleekybadger@gmail.com"

ENV LIBPOSTAL_VERSION="1.1-alpha" \
    LIBPOSTAL_DOWNLOAD_URL="https://github.com/openvenues/libpostal/archive/v1.1-alpha.tar.gz" \
    LIBPOSTAL_DOWNLOAD_SHA="c8a88eed70d8c09f68e1e69bcad35cb397e6ef11b3314e18a87b314c0a5b4e3a"

RUN set -ex \
        \
        && apk add --no-cache --virtual .build-deps \
                curl \
                gcc \
                g++ \
                make \
                libtool \
                autoconf \
                automake \
        \
        && wget -O libpostal.tar.gz "$LIBPOSTAL_DOWNLOAD_URL" \
        && echo "$LIBPOSTAL_DOWNLOAD_SHA *libpostal.tar.gz" | sha256sum -c - \
        \
        && mkdir -p /src  \
        && mkdir -p /data \
        \
        && tar -xzf libpostal.tar.gz -C /src --strip-components=1 \
        && rm libpostal.tar.gz \
        && cd /src \
        \
        && autoreconf -fi --warning=no-syntax --warning=no-portability \
        && ./configure --prefix=/usr --datadir=/data \
        \
        && make -j "$(nproc)" \
        && make install \
        \
        && apk del .build-deps \
        && rm -rf /src
