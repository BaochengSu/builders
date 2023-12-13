# Dockerfile for native/cross building iot2050 u-boot
# 
# How to use this docker image:
ARG ARCH=amd64/
FROM ${ARCH}debian:bookworm-slim as builder-base
ARG ARCH

ARG DEBIAN_FRONTEND=noninteractive
ARG UID=1000
ARG GID=1000

RUN if [ "${ARCH}" = "amd64" ] ; then \
        dpkg --add-architecture arm64; \
    fi

RUN sed -i "s/deb.debian.org/mirrors.ustc.edu.cn/g" /etc/apt/sources.list.d/debian.sources

# Config OS
RUN apt-get update && \
    apt-get install -y locales ca-certificates && \
    localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG=en_US.utf8

# Install base dep tools
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
        build-essential git gosu sudo socat device-tree-compiler flex bison \
        bash-completion python3 python-is-python3 python3-setuptools swig \
        python3-dev libssl-dev bc python3-pyelftools python3-pycryptodome \
        pesign python3-openssl

RUN if [ "${ARCH}" = "amd64/" ] ; then \
        apt-get install --no-install-recommends -y \
            crossbuild-essential-arm64 \
            libssl-dev:arm64 \
            ; \
    fi

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN echo "builder ALL=NOPASSWD: ALL" > /etc/sudoers.d/builder-nopasswd && \
    chmod 660 /etc/sudoers.d/builder-nopasswd

# Add new user
RUN groupadd builder -g ${GID} && \
    useradd builder -u ${UID} -g ${GID} --create-home --home-dir /builder

USER builder
WORKDIR /code