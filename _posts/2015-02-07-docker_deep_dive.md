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


### docker top <containerId>

docker top command let use see the top process running in the container

### docker run command

docker run --cpu-shares
docker run memory=1g   //limit the container memory to 1g

docker run -d ubuntu:14.04.1 /bin/bash  run as daem

docker inspect <containerId>

docker attach <containerName/Id>

note: the container exit while the command running inside it exit, that means the command running inside the container must running in frontend process, this is important


### set alias for docker ps

alias dps="docker ps"


### phusion/baseimages

if you want to run multipal process in one container, check this out

### getting a shell in a container

1. nsenter

allows us to enter Namespaces
requires the containers' PID

get from "docker inspect <containersId> | grep pid"

nsenter -m -u -n -p -i -t pid /bin/bash

-m mount namespace  -u uts namespace -n network namespace
-p process namespace  -i ipc namespace -t target

2. docker-enter <containerId>
3. docker exec -it <containerId> /bin/bash

if we don't have nsenter or docker-enter installed on our host machine, we could use docker volumn to quick install these tools

```
docker -run -v /usr/local/bin:/target jpetazzo/nsenter
```
this command map the `/target` from the image `jpetazzo/nsenter` to `/usr/local/bin` from host machie, so these two commands installed into the host machine


### push image to private registry

1. tag the image

`docker tag <imageId> <registryurl:port>/name`

2. push the image

`docker push <registryurl:port>/name`

set default config

```
/etc/default/docker

DOCKER_OPTS="--insecure-registry debian8.docker.course:5000"
```


### Dockerfile

`docker images --tree`

build from Dockerfile
`docker build -t"build1" .`

#### FROM
#### RUN

`RUN` is a build time instruction

#### EXPOSE
#### CMD

example: 

Shell Form: `CMD echo "hello world"`
Exec Form:  `CMD ["command", "arg1"]`  --recommand

`CMD` only execute at run time, it will be executed when launch a container

usually one per Dockerfile

#### ENTRYPOINT

example: 

```
ENTRYPOINT ["echo"]

docker run <imageName> hello there!

==> hello there!
```

`ENTRYPOINT` instruction is similer with `CMD`, the difference is it 
> Can't be overridden at run-time with normal commands 
> docker run ... <command>, any command passed by docker run is treated as argument

when `ENTRYPOINT` is defined, `CMD` will be treated as argument too, CMD can be used as default argument at this time

#### ENV

example:

```
ENV var1=ping \
	var2=8.8.8.8

CMD $var1 $var2
```

#### VOLUME

example:

`VOLUME /data`
that way, make any containers launch with this images store the date on the docker host file system

volume is used to seprate datastorage from container, as we know container will be disposed as long as it was rm, so does the datastoreage

```
docker run -it -v /test-vol --name=voltainer ubuntu /bin/bash

then we can use --volumns-from to consume this container

docker run -it --volumns-from=voltainer ubuntu /bin/bash

we can access that volumn even if the voltainer container stopped or deleted

docker run -v /data:/data   this will map the /data directory from the host machine to /data inside the container
```

delete the container with the volumn

docker rm -v <container>


tips: reduce the number of layers

```
RUN apt-get update \
	apt-get install -y \
	apache2 \
	apache2-utils \
	vim \
	&& apt-get clean \
	&& ....
```

instead of `RUN` each of these commands in new layer, it's better to compact these commandes into one layers


### docker run with port

```
docker run -d -p 80:80 <imageName>
```

#### docker logs -f <container>

this act likes `tail -f` follow the stdout of container