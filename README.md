[![CI](https://github.com/mpalourdio/graalvm-springboot-raspberry/actions/workflows/main.yml/badge.svg)](https://github.com/mpalourdio/graalvm-springboot-raspberry/actions/workflows/main.yml)

# Introduction 

Hey, I am a Raspberry PI (and a GraalVM) enthusiast!

This a sample project to demonstrate how to build a Spring Boot / GraalVM Docker image for `arm64` architecture.
It produces a `linux/arm64` ready docker image, and can be run on a Raspberry Pi for example. In my case, it even runs on a `Raspberry Pi Zero 2 W`, which is insane.

GraalVM needs at least `512 MB` to run, what is not possible on the `Raspberry Pi Zero 2 W`. So I had to produce a `linux/arm64` from `GitHub actions`.

# Steps for building a `linux/arm64` docker image from a `linux/amd64` host

- Install QEMU : `docker run --privileged --rm tonistiigi/binfmt --install all` / `docker run --rm --privileged multiarch/qemu-user-static --reset -p yes`.
- Validate that it works: `docker run --platform=linux/arm64 --rm -t arm64v8/ubuntu uname -m`. It should return `aarch64`.
- Build the image for `arm64` architecture : `mvn clean -Pnative spring-boot:build-image -Dspring-boot.build-image.imagePlatform=linux/arm64`.
- Drink some coffee, learn haskell, build linux from scratch, or just wait for the build to finish. It will take some time.
- Save the image : `docker save -o /tmp/raspberry.tar docker.io/library/graalvm-springboot-raspberry:0.0.1-SNAPSHOT`.
- Move the image to the target host : `scp /tmp/raspberry.tar pi@192.168.X.X:wherever/`.
- On the target host : `docker load -i wherever/raspberry.tar`.
- Run the image : `docker run -p 8080:8080 --hostname=${HOSTNAME} -it graalvm-springboot-raspberry:0.0.1-SNAPSHOT`.

# Thanks

Kudos to [@dashaun](https://github.com/dashaun/) for the inspiration. Check this blog post [here](https://dashaun.com/posts/multi-architecture-spring-oci-from-anywhere-with-paketo/).

# Image ready to test ?

Just grab [this image generated from GitHub actions](https://mpalourdio.github.io/graalvm-springboot-raspberry/raspberry.tar).  
[Starting January 2025, GitHub actions provide arm64 runners](https://github.com/orgs/community/discussions/19197#discussioncomment-11859757). This drastically improves build time.

# Failed to create the main Isolate. (code 24) ?

Follow [these instructions](https://pimylifeup.com/raspberry-pi-page-size/) for Raspberry PI 5.
