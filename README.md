# docker-images
## Releases and EOL
- The containers will go according to the official releases and will have scheduled monthly builds, built every 1st date of the month
- The images archival and decomissioning of the updates will be on this page in dedicated section bellow 




## Images
***
### Alpine 3.13
[![Build and push alpine base image](https://github.com/thecharge/docker-images/actions/workflows/alpine-linux.yaml/badge.svg)](https://github.com/thecharge/docker-images/actions/workflows/alpine-linux.yaml)

### Releases and EOL:
Releases: https://alpinelinux.org/releases/

Alpine linux 3.13 will be supported until 2022 Nov.

Image EOL: TBD

### How to use this image
Use like you would any other image:
1. Via docker pull

```console
docker pull thecharge/alpine:3.13
```

2. In separate dockerfile:
```dockerfile
# Base arguments
# base registry, if oulled from such
ARG BASE_REGISTRY=
ARG BASE_IMAGE=thecharge/alpine
ARG BASE_TAG=3.13

FROM ${BASE_REGISTRY}${BASE_IMAGE}:${BASE_TAG}

# on your build pipelinie if you use corporate proxy
ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG FTP_PROXY
ARG NO_PROXY

# proxy settings
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}
ENV FTP_PROXY=${FTP_PROXY}
ENV NO_PROXY=${NO_PROXY}


RUN apk add --no-cache mysql-client
ENTRYPOINT ["mysql"]
```

***
### Alpine 3.13, Python 3.10
[![Build and push alpine Python 3.10 base image](https://github.com/thecharge/docker-images/actions/workflows/alpine-python.yaml/badge.svg)](https://github.com/thecharge/docker-images/actions/workflows/alpine-python.yaml)


### Releases and EOL:
Releases: https://www.python.org/downloads/

Python 3.10 will be supperted until 2026 Oct.

Image EOL: TBD

### How to use this image
Use like you would any other image:
1. Via docker pull

```console
docker pull thecharge/python:3.10-alpine
```

2. In separate dockerfile:

```dockerfile
# Base arguments
# base registry, if oulled from such
ARG BASE_REGISTRY=
ARG BASE_IMAGE=thecharge/python
ARG BASE_TAG=3.10-alpine

FROM ${BASE_REGISTRY}${BASE_IMAGE}:${BASE_TAG}

# on your build pipelinie if you use corporate proxy
ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG FTP_PROXY
ARG NO_PROXY

# proxy settings
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}
ENV FTP_PROXY=${FTP_PROXY}
ENV NO_PROXY=${NO_PROXY}

COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["app/app.py"]
```
