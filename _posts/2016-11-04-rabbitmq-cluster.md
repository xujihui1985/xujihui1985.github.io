---
layout: post
title: "set up rabbitmq cluster with docker"
description: ""
category: "rabbitmq"
tags: [rabbitmq]
---
{% include JB/setup %}

#### 1. 启动 docker 容器， 这里注意要提供rabbitmq_erlang_cookie， 这里我设置了默认的vhost以及用户名和密码
```
docker run -d --hostname rabbit01 --name dockerlab-rabbit01 -e RABBITMQ_ERLANG_COOKIE='dockerlab' --net ZTT-SQA-VLAN321 \
-e RABBITMQ_DEFAULT_VHOST=dockerlab -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=ali123 rabbitmq:3.6.5-management

docker run -d --hostname rabbit02 --name dockerlab-rabbit03 -e RABBITMQ_ERLANG_COOKIE='dockerlab' --net ZTT-SQA-VLAN321 \
-e RABBITMQ_DEFAULT_VHOST=dockerlab -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=ali123 rabbitmq:3.6.5-management

docker run -d --hostname rabbit03 --name dockerlab-rabbit03 -e RABBITMQ_ERLANG_COOKIE='dockerlab' --net ZTT-SQA-VLAN321 \
-e RABBITMQ_DEFAULT_VHOST=dockerlab -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=ali123 rabbitmq:3.6.5-management
```
这里我在3台dockerhost上启动了3个rabbitmq 容器，hostname 各为rabbit01 rabbit02 rabbit03， 这3个容器最好是在不同的宿主机上部署，所以不能用docker的Link，因为一旦Link，这3个容器就会起在一台物理机上，这样集群就没有意义了

#### 2. 将这三个容器的ip和hostname的映射加到容器的/etc/hosts中， rabbitmq 必须使用hostname来组件集群(有点尴尬), 在一个vlan中可以用 --network-alias来创建一条dns记录 [net-alias](https://docs.docker.com/engine/userguide/networking/work-with-networks/)，但在测试环境我们是在3个vlan中

> Hostname Resolution Requirements
RabbitMQ nodes address each other using domain names, either short or fully-qualified (FQDNs). Therefore hostnames of all cluster members must be resolvable from all cluster nodes, as well as machines on which command line tools such as rabbitmqctl might be used.

#### 3. 将容器组成集群

```
rabbit02> rabbitmqctl stop_app
rabbit02> rabbitmqctl join_cluster rabbit01
rabbit02> rabbitmqctl start_app
rabbit02> rabbitmqctl cluster_status

rabbit03> rabbitmqctl stop_app
rabbit03> rabbitmqctl join_cluster rabbit01
rabbit03> rabbitmqctl start_app
```

**join cluster前必须先stop, 否则会失败**

打开管理界面 http://ip:15672







