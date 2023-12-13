# u-boot builder

## Pre

Use Debian 12 as the host OS, arm64 or amd64 is both ok.

Make sure the `qemu-user-static` is installed.

## How to build the builder

Inside this folder, to build the builder for cross building (host:
AMD64, target: ARM64V8):

```shell
docker build . -t builder-u-boot:amd64
```

To build the builder for native building (host and target: ARM64V8),
tested on an arm64 debian VM on an Apple silicon MAC:

```shell
docker build . -t builder-u-boot:arm64 --build-arg ARCH=arm64v8/
```

## How to use this builder

For native build:

```shell
docker run -it --rm \
    -v $PWD:/code \
    -u $(id -u):$(id -g) \
    builder-u-boot:arm64 \
    /bin/bash
```

For cross building, change the docker image name to `builder-u-boot:amd64`, and do not forget:

```shell
export CROSS_COMPILE=aarch64-linux-gnu-
```
