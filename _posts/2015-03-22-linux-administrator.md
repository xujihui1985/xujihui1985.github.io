---
layout: post
title: "Networking, service management and ssh"
description: ""
category: ""
tags: [linux]
---
{% include JB/setup %}


### netstat

```
netstat  -alt   -a  list all
				-l  display listening socket
				-t  display tcp


```

### watch

```
watch -d   highlight changes between updates

eg:
watch -d sudo iptables -nvL
```

### Nagios monitoring

install LAMP stack with `tasksel`, tasksel is a package tools to install package on ubuntu

to see how may tasks can be installed, use

```
grep Task /usr/share/tasksel/ubuntu-tasks.desc, or use 
sudo tasksel

sudo tasksel install lamp-server

sudo apt-get install nagios3 -y
```

### install mysql

```
sudo apt-get install mysql
```

#### login mysql

```
mysql -u root -p
```

#### change root password

```
mysql_secure_installation
```

```
mysql -u root -p -e "SHOW DATABASES"
```

#### create Database

```
mysql -u root -p

mysql> SHOW DATABASES;   //list current database
mysql> CREATE DATABASE staff;  //create a new database
mysql> USE staff;  

```

#### create table

```
mysql> SHOW TABLES;
mysql> CREATE TABLE department (did INT NOT NULL AUTO_INCREMENT, dname varchar(20), PRIMARY KEY(did));

mysql> DESCRIBE department;   //show table schema
```


#### rsyslog

latest version is 8.4

```
/etc/rsyslog.conf

local5.* /var/log/local5.log
```

by add this entry in rsyslog.conf, we can use `logger` to log the message to /var/log/local5.log

```
logger -p local5.info "script started"
```

#### logrotate

#### journalctl

part of systemd, responsible for viewing and log management

to view the journalctl, we must be the member of group adm, to add user to group adm, use usermod
```
usermod -a -G adm username
```

#### using SSH

