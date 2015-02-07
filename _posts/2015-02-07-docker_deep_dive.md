---
layout: post
title: "Docker deep dive"
description: ""
category: "docker"
tags: [docker]
---
{% include JB/setup %}


1. configuring docker to communicate over network

docker use unix socket to communicate with client and server, the unix socket file locate under `/run/docker.sock`

step config docker server:
1. check tcp listening program  `netstat -tlp`
2. stop docker by `service docker stop`
3. `docker -H 192.168.1.1:2375 -d &`
4. check tcp again `netstat -tlp`

config docker client

export DOCKER_HOST="tcp://192.168.56.50:2375"

check docker by `docker version`

note: the -H options can be multipal, we can bind both tcp and unix sock on the same server

`docker -H 192.168.56.50:2375 -H unix:///var/run/docker.sock -d &`

2. where docker save the files on container

first you need to know the id of your container, it's a hash, then find the hash under 
```
ls -la /var/lib/docker/aufs/diff/
```

### list all images

```
docker images
```

### list all specified images

```
docker images fedora    //this list all installed fedora images
```

### pull images

```
docker pull coreos/etcd

images installed under /var/lib/docker/<driver>
```

### docker images --tree

```
show all layer of images
```

### docker run

```
docker run -it ubuntu /bin/bash

```

### docker attach

```
attach container

```

`ctrl + p + q` to exit container without stop the container


### copy images to other hosts

```
docker run ubuntu /bin/bash -c "echo 'cool content' > /tmp/cool-file"
```

`docker save -o /tmp/fridge.tar fridge`

/tmp/fridge.tar is the dest of the image tarball

`docker load -i /tmp/fridge.tar`

import the tar file to the host