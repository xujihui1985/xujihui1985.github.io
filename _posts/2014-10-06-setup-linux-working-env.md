---
layout: post
title: "set up linux work enviroment"
description: ""
category: "linux"
tags: [linux]
---
{% include JB/setup %}


```
su -   //login as super user

systemctl set-default multi-user.target

hit right ctrl to get out of the vm

turn off selinux setenforce 0

sestatus  // check selinux status
```

## install lynx

lynx is a text based browser
```
yum install lynx

apt-get install lynx
```

## install ssh server

```
on ubuntu
apt-get install openssh-client openssh-server

on fedora
yum install openssh opssh-server
```

## config sshd

```
cd /etc/ssh
vi sshd_config

PermitRootLogin no

optional  
Banner /etc/welcome.txt

```

## restart sshd

```
on ubuntu
service ssh restart

on fedora
service sshd restart
```

## install apache httpd

```
on ubuntu, it called apache2
on fedora it called httpd
but they are the same program
```

check service status

`service apache2 status`

```
apt-get install apache2

yum install httpd
```

```
restart apache2

service apache2 restart
```


## install vsftpd

```
apt-get install vsftpd
```

## config vsftpd

```
vi /etc/vsftpd.conf

remember to uncommon write_enable
allow_writeable_chroot=YES
chroot_local_user

pasv_enable=YES
pasv_min_port=65533
pasv_max_port=65534
pasv_address=127.0.0.1

service vsftpd restart

last: set port forwarding in virtual box
```

use ftp client

`ftp url` eg: ftp localhost

on windows, use filezilla



## install sqlite

```
apt-get install sqlite
apt-get install sqlite3 libsqlite3-dev

yum install sqlite
yum install sqlite-devel
```

### install mysql

```
apt-get install mysql-server

service mysql status

yum install mysql mysql-server
```