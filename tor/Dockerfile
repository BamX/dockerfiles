#
# Dockerfile for tor
#

FROM alpine
MAINTAINER kev <noreply@datageek.info>

ENV TOR_VER 0.2.6.9
ENV TOR_URL https://www.torproject.org/dist/tor-$TOR_VER.tar.gz
ENV TOR_MD5 4a1b334c30d7b37ea72fa33425220d5d
ENV TOR_FILE tor.tar.gz
ENV TOR_TEMP tor-$TOR_VER

RUN apk add -U build-base \
               gmp-dev \
               libevent \
               libevent-dev \
               libgmpxx \
               openssl \
               openssl-dev \
               python \
               python-dev \
    && wget -O $TOR_FILE $TOR_URL \
        && echo "$TOR_MD5  $TOR_FILE" | md5sum -c \
        && tar xzf $TOR_FILE \
        && cd $TOR_TEMP \
        && ./configure --prefix=/ --exec-prefix=/usr \
        && make install \
        && cd .. \
        && rm -rf $TOR_FILE $TOR_TEMP \
    && wget -O- https://bootstrap.pypa.io/get-pip.py | python \
        && pip install fteproxy \
                       obfsproxy \
    && apk del build-base \
               gmp-dev \
               libevent-dev \
               openssl-dev \
               python-dev \
    && rm -rf /var/cache/apk/*

COPY ./torrc /etc/tor/torrc
VOLUME /etc/tor /root/.tor
EXPOSE 9001 9030 9050 9051

CMD ["/usr/bin/tor", "-f", "/etc/tor/torrc"]
