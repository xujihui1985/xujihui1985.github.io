---
layout: post
title: "new post"
description: ""
category: ""
tags: []
---
{% include JB/setup %}


### Docker components

我第一份工作是在一家生产型企业，生产的是核阀，核阀体积很大，通常由很多个零件组成，由于起初中国这里技术能力不行，无法将零件组装起来，所以我们是先把零件运到客户公司，然后再从美国派出工程师去那里安装，这样通常从客户下单到货物能正常使用，通常要1年，经常还会碰到零件发错，在现场安装不起来的问题，导致客户非常的生气。

后来我们那边的做法是先将零件和装配图纸发到新加坡的工厂，在那里组装完成之后，再发给客户，这使得交付时间缩短到了3个月，并且再也没出现过现场安装不起来的问题。


其实这个过程和我们的软件交付一样，只不过我们是交付代码.

对于传统的软件交付，我们通常是交付代码，然后将代码在dev环境，sit环境，staging环境，进行部署，验证，最后由PE发布到生产环境


通常这样造成的问题是，

1. 我在开发环境部署成功，到sit环境部署失败了
2. 每个环境都需要重新回归，因为我在开发的时候不知道测试环境是怎么样的，很多问题会在测试环境才会出现


docker的出现就是来解决这样的问题。

docker是由这3个组件组成

1. docker engine
2. docker image
3. docker container
4. docker hub


我们把docker engine比喻成 一个集装箱码头， 那么image便是货物清单，container就是**一个包含这项目运行环境以及源码**的集装箱。 我们部署的不再是源码，而是**一个包含这项目运行环境以及源码**的完整应用



#### docker engine

有时候也叫做 docker daemon 或者 docker runtime, 我们把运行docker engine的机器称作docker host,  它是docker最核心的模块，用来运行docker container, 也就是说，不管什么环境，在我本机，测试服务器，还是上云，只要有docker engine， 就能运行docker container

#### docker image

那我之前那个例子，docker image就是零件+图纸，零件用来组成产品，图纸用来告诉工程师，如何组成产品，

```
docker run -it ubuntu /bin/bash
```

这里 ubuntu 就是Image, 用来告诉docker， 我要用ubuntu这个镜像来创建一个container (启动一个服务), 启动后，我要执行`/bin/bash` 这个程序

当按下回车之后，docker首先会在local找 ubuntu这个image, 如果存在这个Image，那就直接启动， 如果不存在ubuntu这个Image, 那docker会到 docker hub上去下载ubuntu到本地， 这里的docker hub和github或者npm类似，是一个集中的image版本控制管理的地方。


#### docker container

container可以看作是Image的一个实例，就像object是class的实例一样，我通常将container看作是一个服务


#### Layered Image

Docker中的Image是由多层的Image组成的，举个例子

我们从最基础的centos7的image上安装nodejs之后产生了一个新的Image centos-node， 这个image由两层组成，第一层是centos, 第二层是Nodejs, docker以增量的方式保存image, 而不是全量的

#### union mount
每一层都有一个唯一的ID，每一层会记录它的parentid，当`docker run`的时候，docker会将所有这几层image合起来成为一个image, 假如当中有文件冲突，比如顶层的layer中改了baseimage中的文件，那么会以上层的image为准

在baseImage下层还有一个bootfs, 这一层是与docker host共享的，所有的docker container和docker host共享同一个kernel，这也是为什么docker container不需要像虚拟机一样boot，秒级的启动速度。

Question
docker 在创建container之后，会为每个container都分配整个Image大小的磁盘吗？ 假如一个centor的image是400M，那每个container都会分配400M的磁盘空间吗？

docker 创建container之后，所有的container都会共享image，所有的写操作都会以copy on write的方式写到新的层中，也就是说，假如需要改文件，需要先将要改的文件copy到新的层中，然后再修改，只有新的层是可写的，下层的layer都是只读的，这有点像javascript中的原型模式

![](http://i.imgur.com/pYFjWx7.png)

#### one process/app/concern per container

当container start的时候需要执行一个Process， container会在程序退出，或者不在forground的时候退出，也就是说，在container中的服务需要是一个前端进程， *除非这个container是作为工具使用*

```
执行docker run 的时候必须指定command, 在container中，会将该进程作为PID1, 我们知道linux中，PID1是一个特殊的进程，所有的进程都是从这个进程fork出来的，但在container中，PID1是我们指定的command, 可能是nginx，也可能是chair的应用，当这个应用退出的时候，container也就退出了
```


#### Dockerfile

```
FROM ubuntu:14.04

ADD . /app
<instruction>
<instruction> 
<instruction>  
...

```

docker image可以通过dockerfile生产， 每一个指令都会生成一个新的临时layer

build from Dockerfile

```
docker build -t="myimage" .
``` 

#### some instruction

ADD

```
ADD <source> <desc>
```

将build context中的文件添加到image中，build context就是上面 docker build中的 . 也就是当前目录下的所有文件，docker可以添加dockerignore文件来防止不必要的文件添加到build context中

CMD        

default command when execute docker run without specify command
One per Dockerfile

```
CMD ["command", "arg"]
```

ENTRYPOINT

和CMD很像，不同的是，这个指令不能被run command覆盖，但是会被作为ENTRYPOINT的参数传入，可以将ENTRYPOINT和CMD指令搭配起来用

```

ENTRYPOINT ["apache2ctl"]
```

docker run -d -p 80:80 web2 -D FOREGROUND

ENV

用来指定环境变量

```
ENV var=value var2=value2   //写在一样避免不必要临时层
```
ENV 可以被很多指令使用，比如WORKDIR

WORKDIR

```
ENV WORK_DIR=/tmp

WORKDIR ${ENV}
```

LABEL

```
LABEL version="1.0"
```

VOLUME

volume可以用来将文件与container分离，或者在多个container中共享数据

```
docker run -it -v /test-vol --name=testvolume ubuntu:14.04 /bin/bash

write some files in /test-vol

stop the container

通过docker inspect查看

docker run -it --volumes-from=testvolume ubuntu:14.04 /bin/bash
```

我们也能通过

```
docker run -v /data:/data  将Host上的目录挂载到docker上
```

#### network

docker container默认使用host上resolve.conf, 通过docker inspect可以看到ResolveConfPath, 可以通过 `docker run --dns=x.x.x.x` 来指定dns

exposing port

要使container能够被外部访问，需要通过将container的端口暴露出去，并且将container的端口映射到host的端口上，通过访问host:port来访问contaienr中的应用

```
docker run -d -p 7001:7001 --name=chair
```

#### link

假如一个应用由多个container组成，例如，一个web应用，后面有个mysql的数据库，这个时候就需要用到Link, link 可以使两个container互相通讯而不需要将端口export出来

```
docker run --name=src -d img

docker run --name=reciver --link=src:alias-src -it ubuntu:15.04 /bin/bash
```


#### docker swarm

swarm 是用来做docker 集群的，我们之前所讲的都只是在一个docker host上执行docker container, 生产环境中，这通常是不可能的，