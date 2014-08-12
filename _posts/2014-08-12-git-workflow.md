---
layout: post
title: "a typical gitworkflow to pull remote banch to master"
description: ""
category: "git"
tags: [git]
---
{% include JB/setup %}


First, how to clone remote branch to local repository

```
git clone url
cd myproject

git branch -a   //show all branch include remotebranch

git checkout -b mylocalbreanch origin/remotebranch

```

```
git log --graph --color --decorate --oneline --all  //show detail log

```

then push local branch to remote

```
git push <remote-name> <localbranchname>:<remotebranchname>

```

if there has confilicate during the pull request

```
git fetch origin
git checkout -b newfeature origin/newfeature
git merge master

```

**this create a localbreanch from remote and merge master from this branch**

```
git checkout master 
git merge newfeature   //merge newfeature into master
git push origin master  //push to master and done
```