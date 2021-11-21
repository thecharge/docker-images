ARG BASE_REGISTRY=
ARG BASE_IMAGE=thecharge/alpine
ARG BASE_TAG=3.13
FROM node:16-alpine3.12 as base

# image begins
FROM ${BASE_REGISTRY}${BASE_IMAGE}:${BASE_TAG}

# proxy settings
ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG FTP_PROXY
ARG NO_PROXY
ENV HOME=/home/node
ENV USER=node


# proxy settings
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}
ENV FTP_PROXY=${FTP_PROXY}
ENV NO_PROXY=${NO_PROXY}


RUN groupadd -g 1001 ${USER} && \
    useradd -r -u 1001 -m -s /sbin/nologin -g ${USER} ${USER}

USER 1001

COPY --from=base /usr/local/bin /usr/local/bin
COPY --from=base /usr/local/include /usr/local/include
COPY --from=base /usr/local/share/man /usr/local/share/man
COPY --from=base /usr/local/share/doc /usr/local/share/doc
COPY --from=base /usr/local/share/systemtap /usr/local/share/systemtap
COPY --from=base /usr/local/lib/node_modules /usr/local/lib/node_modules
COPY --from=base /opt /opt

WORKDIR ${HOME}
USER 1001


HEALTHCHECK NONE
