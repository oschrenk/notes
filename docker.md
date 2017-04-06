# docker #

You can think about containers as a process in a box. The box contains everything the process might need, so it has the file system, system libraries, shell and such, but by default none of it is started or run.

```
brew cask install docker

# run hello world containmer
docker run hello-world
```

## Terminology

- **Dockerfile** Dockerfile is the source code of the image
layer
- **image** is a set of layers. Like a "compiled version" of the Dockerfile. Contains file system and configuration of out application.
- **base image** an image that has no parent image
- **child image** an image that build on child images
- **official images** are Docker sanctioned. These are well maintained images without any  prefix.
- **user images** created by users
- **container** is a instance of an image

- **daemon** background service that manags building, running and distributing Docker containers
- **client** The command line tool interacting with the Docker daemon
- **store** is a registry of Docker images
