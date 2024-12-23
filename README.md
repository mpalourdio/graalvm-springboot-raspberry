# Introduction 

This a sample project to demonstrate how to build a Spring Boot Docker image for ARM64 architecture.
It just works, produces a docker image `linux/arm64` and can be ran on a Raspberry Pi. In my case, it even runs on a `pi0w2` o_O.

GraalVM needs at least 512 MB to run, which was not possible on the Raspberry pi0W2. so I had to produce a `linux/arm64` image from a `linux/amd64` host.

# Steps

- Install QEMU once & for all : `docker run --rm --privileged multiarch/qemu-user-static --reset -p yes`.
- Build the code for `arm` archictecture : `mvn clean -Pnative spring-boot:build-image -Dspring-boot.build-image.imagePlatform=linux/arm64`.
- Drink some coffee, learn haskell, build linux from scratch, or just wait for the build to finish.
- Save the image : `docker save -o raspberry.tar graalvm-springboot-raspberry:0.0.1-SNAPSHOT`.
- Move the image to the target host : `scp raspberry.tar pi@192.168.X.X:wherever/`.
- On the target host : `docker load -i raspberry.tar`.
- Run the image : `docker run -p 8080:8080 -it graalvm-springboot-raspberry:0.0.1-SNAPSHOT`.

# Thanks

Kudos to [@dashaun](https://github.com/dashaun/) for the inspiration. Check this blog post [here](https://dashaun.com/2021/06/17/building-a-graalvm-native-image-for-arm64-on-amd64/).
