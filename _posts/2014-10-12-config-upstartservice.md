---
layout: post
title: "config upstart service"
description: ""
category: "linux"
tags: [linux]
---
{% include JB/setup %}

1. create a conf file
2. copy the conf file to /etc/init.d/ folder

upstart script sample
```

description ...
author

start on runlevel [2345]   //all multi-user modes
stop on runlevel [016]     // 0 - halting   6 - rebooting  1 - single-user mode 

respawn  // restart the job if it dies

setuid vagrant  // set the run user

env HOME="/home/vagrant"  //this enviroment var only for the scope of this service

chdir /vagrant/myhubot/

script
	export ...
    exec node_modules/.bin/hubot --adapter hipchat
end script

```