# Introduction 

This a sample project to demonstrate how to build a Spring Boot Docker image for ARM64 architecture.
It just works, produces a docker image `linux/arm64` and can be run on a Raspberry Pi. In my case, it runs on a `pi0w2`.

GraalVM needs at least 512 MB to run, which was not possible on the Raspberry pi0W2. so I had to produce a `linux/arm64` image from a `linux/amd64` host.

# Steps
- Install QEMU once & for all : `docker run --rm --privileged multiarch/qemu-user-static --reset -p yes`
- Build the code for `arm` archictecture : `mvn clean -Pnative spring-boot:build-image -Dspring-boot.build-image.imagePlatform=linux/arm64`
- Save the image : `docker save -o whatever.tar docker.io/library/whatever:0.0.X-SNAPSHOT`
- Move the image to the target host : `scp whatever.tar pi@192.168.X.X:wherever/`
- On the target host : `docker load -i whatever.tar`
- Run the image : `docker run -p 8080:8080 -it whatever:0.0.X-SNAPSHOT`
