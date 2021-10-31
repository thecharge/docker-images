# Base arguments
ARG BASE_REGISTRY=
ARG BASE_IMAGE=alpine
ARG BASE_TAG=3.13.6

FROM ${BASE_REGISTRY}${BASE_IMAGE}:${BASE_TAG}

ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG FTP_PROXY
ARG NO_PROXY


# proxy settings
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}
ENV FTP_PROXY=${FTP_PROXY}
ENV NO_PROXY=${NO_PROXY}

COPY ./status.sh /status.sh
COPY ./motd /etc/motd

RUN apk upgrade --update-cache --available && sync && \
    apk add --no-cache\
        bzip2-dev \
        duo_unix \
        libffi-dev \
        libuuid \
        xz-dev \
        git \
        gcc \
        g++ \
        make \
        wget \
        ca-certificates \
        tar \
        zlib-dev \
        gzip \
        libxml2-dev \
        autoconf \
        automake \
        help2man \
        libtool \
        sqlite \
        expat-dev \
        shadow \
        findutils \
        sqlite-dev \
        openssl-dev \
        curl-dev \
        curl &&\
    chmod +x /status.sh && \
    echo '[ ! -z "$TERM" -a -r /etc/motd ] && cat /etc/issue && cat /etc/motd' \
    >> /etc/profile

# COPY hardening.sh hardening.sh
# RUN chmod +x hardening.sh && ./hardening.sh && rm -f hardening.sh


HEALTHCHECK NONE
