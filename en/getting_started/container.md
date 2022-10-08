# Building using Containers

The community-created docker image makes it faster and easier to build QGC on Linux.

The container definition is located in the `./deploy/container` directory, and is based on Ubuntu 20.04.
It pre-installs Qt and all the other dependencies at build time using the `install-qt-linux.sh` script, which is located in the same directory.

This makes setting up the toolchain reproducable and easy.
The container also uses the `CMake` build system, which has some improvements when compared to`qmake`


## Building the Container

Before using the container, you must first build it from the definition using docker.
Run the following script from the root of the QGC source code directory:

```
docker build --file ./deploy/docker/Dockerfile-build-linux -t qgc-linux-docker .
```

> **Note** The `-t` flag is essential.
  Keep in mind this is tagging the image for later reference since you can have multiple builds of the same container

<span></span>
> **Note** If building on a Mac computer with an M1 chip you must also specify the build option `--platform linux/x86_64` as shown:
> ```
> docker build --platform linux/x86_64 --file ./deploy/docker/Dockerfile-build-linux -t qgc-linux-docker .
> ```
> Otherwise you will get a build error like:
> ```
> qemu-x86_64: Could not open '/lib64/ld-linux-x86-64.so.2': No such file or directory
> ```


## Building QGC using the Container

To use the container to build QGC, first define a directory to save the artifacts.
We recommend you create a directory named `build` in the source tree and then run the docker image using the tag provided above.

The commands below show how to (from the root directory), first create the `build` directory, and then use the docker image to build QGC:

```
mkdir build
docker run --rm -v ${PWD}:/project/source -v ${PWD}/build:/project/build qgc-linux-docker
```

Depending on your system resources, or the resources assigned to your Docker Daemon, the build step can take some time.

## Using the Build

QGC is built to `./build/staging/`.
On Linux you can start it in a bash terminal from that folder using:

```
./qgroundcontrol-start.sh
```


> **Note** You might also use the container to build Linux QGC on a Windows host.
> In this case you would use the build command:
> ```
> docker run --rm -v %cd%:/project/source -v %cd%/build:/project/build qgc-linux-docker
> ```
> You would then need to copy the `./build/staging/` directory to a Ubuntu computer in order to run it.


## Troubleshooting

### Windows: 'bash\r': No such file or directory

This error indicates that a Linux script is being run with Windows line endings, which might occur if `git` was configured to use Windows line endings:
```
 > [4/7] RUN /tmp/qt/install-qt-linux.sh:
#9 0.445 /usr/bin/env: 'bash\r': No such file or directory
```

One fix is to force Linux line endings using the command:
```
git config --global core.autocrlf false
```
Then update/recreate your local repository.
