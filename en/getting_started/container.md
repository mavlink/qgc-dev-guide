# Building using Containers

The community created a docker image that makes it much easier to build a Linux-based QGC application.
This can give you a massive boost in productivity and help with testing.

## About the Container

The Container is located in the `./deploy/container` directory.
It's based on ubuntu 20.04.
It pre-installs all the dependencies at build time, including Qt, thanks to a script located in the same directory, `install-qt-linux.sh`.
The main advantage of using the Container is the usage of the `CMake` build system and its many improvements over `qmake`

## Building the Container

Before using the Container, you have to build the image.
You can accomplish this using docker, running the following script from the root of the QGC source code directory.

```
docker build --file ./deploy/docker/Dockerfile-build-linux -t qgc-linux-docker .
```

> **Note** The `-t` flag is essential.
  Keep in mind this is tagging the image for later reference since you can have multiple builds of the same Container

## Building QGC using the Container

To use the Container to build QGC, you first need to define a directory to save the artifacts.
We recommend you create a `build` directory on the source tree and then run the docker image using the tag provided above as follows, from the root directory:

```
mkdir build
docker run --rm -v $PWD./project/source -v $PWD/build./project/build qgc-linux-docker
```

> **Note** Depending on your system resources, or the resources assigned to your Docker Daemon, this step can take some time.
