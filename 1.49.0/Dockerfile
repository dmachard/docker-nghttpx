FROM debian:11 as build

ENV NAME=nghttp2 \
    VERSION=1.49.0 \
    DOWNLOAD_URL=https://github.com/nghttp2/nghttp2/releases/download/v1.49.0/nghttp2-1.49.0.tar.gz

RUN apt-get update && \
    apt-get install -y curl build-essential \
      libtool file pkg-config zlib1g-dev \
      libcunit1-dev libssl-dev libxml2-dev \
      libev-dev libevent-dev libjansson-dev \
      libc-ares-dev libjemalloc-dev libsystemd-dev && \
    curl -sSL $DOWNLOAD_URL -o nghttp2.tar.gz && \
    tar xzf nghttp2.tar.gz && \
    cd $NAME-$VERSION/ && \
    ./configure \
      --prefix=/opt/nghttp2 \
      --enable-app \
      --disable-examples \
      --disable-python-bindings \
      LIBEV_LIBS="-l:libev.a" \
      JEMALLOC_LIBS="-l:libjemalloc.a" \
      LIBCARES_LIBS="-l:libcares.a" \
      ZLIB_LIBS="-l:libz.a" \
      --with-neverbleed && \
    make && \
    make install

FROM debian:11.1-slim

COPY --from=build /opt/nghttp2 /opt/nghttp2
WORKDIR /opt/nghttp2

COPY nghttpx.conf /opt/nghttp2/etc/nghttpx.conf

RUN apt-get update && \
    apt-get install -y python3-minimal python-is-python3 && \
    addgroup --gid 1000 nghttpx && adduser --system --no-create-home --disabled-login --disabled-password --ingroup nghttpx --uid 1000 nghttpx && \
    chown nghttpx:nghttpx -R /opt/nghttp2

USER nghttpx

EXPOSE 3000/tcp

ENTRYPOINT [ "/opt/nghttp2/bin/nghttpx" ]

CMD  [ "--conf", "/opt/nghttp2/etc/nghttpx.conf" ]
