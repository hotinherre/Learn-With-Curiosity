# Docker Training

## Day 1

### parking pool

- [ ] openshift
- [ ] linux namespace
- [ ] Cgroup
- [ ] docker network

### Introduction

- In the past days move software is difficult, insecure, resource intensive.
- Docker make environment isolate. Make each component safe.
- VM vs Container. Container bootstrap time much faster.
- Container built on cgroups & linux namespaces
- Encapsulated, therefore portable
- Isolated, therefore secure
- Lightweight, therefore efficient
- Docker is linux native app. Many workarounds for windows to let docker runs.
- All pid in container is 1.
- Cgroup - limit cpu, mem

3 month a stable version.
1 month a new release.

demo1

```shell
docker container run -d --name pinger centos:7 ping 8.8.8.8
docker container exec pinger ps -aux
sudo ps -aux | grep ping
docker container ls
# this will kill container.
sudo kill -9 host PID of ping]
docker container ls
```

exercise1

```shell
docker container run centos:7 echo "hello world"
docker container ls
# list all stopped
docker container ls -a
# kill all container
docker container rm -f $(docker container ls -aq)
```

- Image are made of a stack of immutable layers.
- Image is filesystem for container process

### Create image

- Commit the R/W container layer as a new R/O image layer.
- Define new layers to add to a starting image in a Dockerfile. Each separate command adds a layer or metadata.
- Import a tarball into Docker as a standalone base layer.

### Docker volume

- Mount data at container startup
- Persist data when a container is deleted
- Share data between containers
- Speed up I/O by circumventing the union filesystem
