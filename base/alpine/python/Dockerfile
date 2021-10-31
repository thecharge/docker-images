# Base arguments
ARG BASE_REGISTRY=
ARG BASE_IMAGE=thecharge/alpine
ARG BASE_TAG=3.13
# image begins
FROM ${BASE_REGISTRY}${BASE_IMAGE}:${BASE_TAG} as build

# base arguments for python istallation
ARG BASE_PYTHON_VERSION_CLI=3.10
ARG BASE_PYTHON_VERSION=${BASE_PYTHON_VERSION_CLI}.0
ARG BASE_PYTHON_TARBALL=Python-${BASE_PYTHON_VERSION}.tgz
ARG BASE_PYTHON_URL=https://www.python.org/ftp/python/${BASE_PYTHON_VERSION}/${BASE_PYTHON_TARBALL}
ARG BASE_PYTHON_INSTALL_FOLDER=/tmp/${BASE_PYTHON_TARBALL}
ARG BASE_PYTHON_VERSION_CHECKSUM=729e36388ae9a832b01cf9138921b383
# proxy settings
ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG FTP_PROXY
ARG NO_PROXY


# proxy settings
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}
ENV FTP_PROXY=${FTP_PROXY}
ENV NO_PROXY=${NO_PROXY}


# getting python
RUN wget --output-document=${BASE_PYTHON_INSTALL_FOLDER} ${BASE_PYTHON_URL}

# installing python
RUN mkdir -p /usr/local/src/python && \
    tar -zxvf ${BASE_PYTHON_INSTALL_FOLDER} -C /usr/local/src/python --strip-components=1 && \
    cd /usr/local/src/python && \
    ./configure \
      --enable-loadable-sqlite-extensions \
      --enable-optimizations \
      --enable-option-checking=fatal \
      --with-system-expat \
      --with-ensurepip && \
    make && \
    make altinstall

RUN find /usr/local -depth \
	\( \
		\( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
		-o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name '*.a' \) \) \
	\) -exec rm -rf '{}' +
RUN echo '/usr/local/lib' > /etc/ld.so.conf

RUN mkdir /wheel/
RUN python${BASE_PYTHON_VERSION_CLI} -m ensurepip --upgrade
# @TODO: have to add ldconfig

FROM ${BASE_REGISTRY}${BASE_IMAGE}:${BASE_TAG}
# base arguments for python istallation
ARG BASE_PYTHON_VERSION_CLI=3.10
# proxy settings
ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG FTP_PROXY
ARG NO_PROXY


# proxy settings
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}
ENV FTP_PROXY=${FTP_PROXY}
ENV NO_PROXY=${NO_PROXY}

ENV PATH /usr/local/bin:$PATH

COPY --from=build /usr/local/include/python${BASE_PYTHON_VERSION_CLI} /usr/local/include/python${BASE_PYTHON_VERSION_CLI}
COPY --from=build /usr/local/lib /usr/local/lib
COPY --from=build /usr/local/bin /usr/local/bin
# @TODO: have to add ldconfig
RUN cd /usr/local/bin && \
    ln -s idle${BASE_PYTHON_VERSION_CLI} idle3 && \
    ln -s idle3 idle && \
    ln -s pydoc${BASE_PYTHON_VERSION_CLI} pydoc3 && \
    ln -s pydoc3 pydoc && \
    ln -s python${BASE_PYTHON_VERSION_CLI} python3 && \
    ln -s python3 python && \
    ln -s python${BASE_PYTHON_VERSION_CLI}-config python3-config && \
    ln -s python3-config python-config && \
    ln -s easy_install-${BASE_PYTHON_VERSION_CLI} easy_install-3 && \
    ln -s easy_install-3 easy_install && \
    ln -s 2to3-${BASE_PYTHON_VERSION_CLI} 2to3-3 && \
    ln -s 2to3-3 2to3 && \
    ln -s pip${BASE_PYTHON_VERSION_CLI} pip3 || true && \
    ln -s pip3 pip || true && \
    echo '/usr/local/lib' > /etc/ld.so.conf

RUN groupadd -g 1001 python && \
    useradd -r -u 1001 -m -s /sbin/nologin -g python python

USER 1001

CMD ["python3"]

HEALTHCHECK NONE
