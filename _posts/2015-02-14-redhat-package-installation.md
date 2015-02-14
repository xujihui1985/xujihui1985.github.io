---
layout: post
title: "redhat package installation"
description: ""
category: "linux"
tags: [yum,rpm]
---
{% include JB/setup %}


### rpm

install package

`rpm -ivh xxxx.rpm`
-i install
-v vobose
-h show progress

show installed package
`rpm -ql package`
-q query


### yum

`yum install vim -y`
install vim without prompt confirmation

`yum remove vim -y`
remove vim without prompt

### yum shell

create yumcmd file

```
remove zsh
remove nmap
install vim
run
```

run yumcmd file

```
yum -y shell yumcmd
```
that will run the yumcmd file in one transaction

