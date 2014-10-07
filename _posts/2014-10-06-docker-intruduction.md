---
layout: post
title: "docker intruduction"
description: ""
category: "docker"
tags: [docker]
---
{% include JB/setup %}


Linux Container  LXC

Containers are a lightweight virtualization technology

### what's the problem using VM?

1. when start the VM, for example, I allocate 4GB memory to it, it will consume the 4GB as soon as it start. But, may be it can only use 1GB, 3GB memory was wasted.
2. boot process takes a lot of time, because each vm is a standalone operating system.
3. backup and clone waste hard disk
4. no version control


### [Container](https://help.ubuntu.com/lts/serverguide/lxc.html#lxc-installation "Container")

1. container share kernel, RAM, hardware space, CPU which is provided by Host, it's like just running services on the host machine.
2. no boot process, because they share the same kernal ...
3. UFS, is a layed arch, that is said, it is incremently increased, for example,after I pull a base centos images from remote registry, and install mysql, nodejs, mongodb ect on that image and create a new images base on that image, the new image is just a layer that only contain the file I just created. when I start a container use that new image, it will compose all the required layer.
4. version controlled
5. each container is a isolated enviroment

![](http://i.imgur.com/pYFjWx7.png)

### [Vagrant](https://www.vagrantup.com/ "Vagrant")

lots of people like to compare docker with vagrant which is a VM management tools, it is quite similer with docker in some aspect, such as vagrantfile vs Dockerfile,  pervision vs docker run, start up a vagrant instance in one command.

but they are two entirly different things. In general, vagrant is a tool to manage VM instance while docker is the tools to manage application run time.

see here [docker vs vagrant](http://stackoverflow.com/questions/16647069/should-i-use-vagrant-or-docker-io-for-creating-an-isolated-environment "docker vs vagrant")

### understand what is Docker filesystem layers

defination:
> A union mount is a mount that allows several filesystems to be mounted at one time but appear to be one filesystem

Docker calls each of these filesystems images. Images can be layered on top of
one another. The image below is called the parent image and you can traverse
each layer until you reach the bottom of the image stack where the final image
is called the base image. Finally, when a container is launched from an image,
Docker mounts a read-write filesystem on top of any layers below. This is where
whatever processes we want our Docker container to run will execute.

When Docker first starts a container, the initial read-write layer is empty. As
changes occur, they are applied to this layer; for example, if you want to change
a file, then that file will be copied from the read-only layer below into the read-write layer. The read-only version of the file will still exist but is now hidden
underneath the copy.

### Demo

demo1: setup a private npm registry

demo2: ngnix, use volumn to share file between host and container

demo3: link two container  nodeserver and redis db

```
1. start redis without export port, remember to use --name to allocate a name to the container
2. link webapp and docker, for demostraction, use bash to test redis server
	docker run -i -t -P --name webapp --link redis:db sean/static_web:v3 /bin/bash //note db is alias, it can be called anything
3. install redis-tools
4. redis-cli -h db -p 6379
5. check /etc/hosts and env, that's how docker work
```
demo4: install jenkins


### what we can do with docker

brain storm

1. deploy application with zero down time
2. quick fall back if new version is not stable
3. ci
4. develop enviroment is same as production
5. install an application in an isolated manner.

eg:
```
sudo docker run -v /usr/local/bin:/target jpetazzo/nsenter
```


### problem I have encounted

docker require linux kernal 3.8, so it's not support osx natively, we have to use boot2docker or vagrant as a high level abstraction.

boot2docker is easy to setup, but the vital problem is that it doesn't support volumn share so, vagrant is a better choice in my respect.

how to manager containers?  