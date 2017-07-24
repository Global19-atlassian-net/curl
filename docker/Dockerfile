FROM ubuntu:16.04
MAINTAINER hans.zandbelt@zmartzone.eu

RUN apt-get clean && apt-get --fix-missing update
RUN apt-get update && apt-get install -y apt-utils build-essential autoconf automake libtool-bin
RUN apt-get update && apt-get install -y git subversion wget
RUN apt-get update && apt-get install -y python libpcre3-dev libexpat1-dev
RUN apt-get update && apt-get install -y pkg-config

WORKDIR /root
ENV TARGET_DIR /usr/local

#
# OpenSSL 1.1.x
#
ENV OPENSSL_VERSION 1.1.0f

RUN wget https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz
RUN tar zxvf openssl-${OPENSSL_VERSION}.tar.gz
RUN cd openssl-${OPENSSL_VERSION} && ./config --prefix=${TARGET_DIR} && make && make install_sw

# 
# Token Bind library
#
RUN git clone https://github.com/zmartzone/token_bind.git
RUN cd token_bind && git checkout expose-negotiated-version

#
# curl
#
ENV LINKS token_bind_client.c token_bind_common.c cbs.c cbb.c base64.c
RUN git clone https://github.com/zmartzone/curl.git
RUN cd curl && autoreconf --force --install && rm -rf autom4te.cache/
RUN cd curl && ./configure --with-ssl=/usr/local --with-token-binding=/root/token_bind
RUN cd curl/lib/vtls && for f in ${LINKS} ; do ln -s /root/token_bind/$f ; done 
RUN cd curl && make && make install

RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/usr-local.conf && ldconfig

ENTRYPOINT ["/usr/local/bin/curl"]
