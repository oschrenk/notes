# docker #

You can think about containers as a process in a box. The box contains everything the process might need, so it has the file system, system libraries, shell and such, but by default none of it is started or run.

```
# requirements
brew install docker docker-machine docker-compose

# create machine
docker-machine create --driver virtualbox default

# configure shell
eval (docker-machine env default)

# run hello world containmer
docker run hello-world
```
