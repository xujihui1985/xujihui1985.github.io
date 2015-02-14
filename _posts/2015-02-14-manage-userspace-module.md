---
layout: post
title: "manage shared library"
description: ""
category: "linux"
tags: [linux]
---
{% include JB/setup %}

### list shared library dependence

```
ldd /bin/ls
```
this will list all the shared library that ls command use

use $LD_LIBRARY_PATH enviroment variable to change the shared library for test usage

```
export LD_LIBRARY_PATH=/testlibpath/
```

my defined the LD_LIBRARY_PATH env variable, it will search the sharedlib from this directory first. this is useful for test.


### list kernel modules

`lsmod` this will list all loaded kernel modules

`modprobe -r module`  remove the kernel module

`modprobe module`  load the kernel module

### never load the kernel modules

1. go to /etc/modprobe.d
2. modify the blacklist.conf
3. add the module to blacklist