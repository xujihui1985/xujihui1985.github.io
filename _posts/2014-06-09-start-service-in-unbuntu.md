---
layout: post
title: "Start/Stop Service in Ubuntu"
description: ""
category: "system"
tags: [ubuntu]
---
{% include JB/setup %}


Debian and ubuntu system use the `service` command to control services and `update-rc.d` for adding and removing services from start up


### Add a service to start up

```
	update-rc.d apache2 defauts
```

this will add apache2 service to all run level, in ubuntu, there is no difference between run level


### Remove a service from start up

```
	update-rc.d -f apache2 remove
```


### check service

```
	service apache2 status
```

### start a service

```
	service apache2 start
```

### stop a service

```
	service apache2 stop
```