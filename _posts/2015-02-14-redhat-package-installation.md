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


### create own yum repository

1. create a directory  eg: ~/repo
2. move the rpm file `eg: FoxitReader.rpm` to the folder
3. install createrepo package  `yum install createrepo -y`
4. create a repo from the folder `createrepo ~/repo`
5. then we need to tell our client system about this new repo then it can search through this new repo
6. go to /etc/yum.repos.d/  the repository info is kept in the xxx.repo file
7. create a new .repo file from cp an exist one
8. modify the .repo file
9. `yum repolist` to check the repolist should be be to see the new repo
10. `yum search Fox`
11. `yum install FoxitReader.i386` to install the new package

```
[Local]
name=Centos - Local
baseurl=file:///home/user/repo
gpgcheck=0   #do not check gpg key
enable=1   #enable the .repo
gpgkey=xxxxxx
```