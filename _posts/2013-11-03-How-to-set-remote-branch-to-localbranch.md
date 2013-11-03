---
layout: post
title: "Set remote Branch"
description: ""
category: "Source Control"
tags: [Git]
---
{% include JB/setup %}

we have been used to clone a remote branch to local and pull or push the code to remote, but how can we create the local branch first and then push to remote?

when we init a local repository

	git init

git help us create a local repository, we can use following command to see the remote branch of this local one

	git remote -v

and you will see nothing, because we haven't set up remote branch for it.
to setup remote branch
>**git remote -add origin url**

origin is an arbitrary name, you can call it anything, and url is the remote address, you can also use ssh protcal, if you setup ssh key on your local machine


	git remote -add origin https://github.com/xujihui1985/TypeScriptFundamental.git

this is not finished.
after we add remote branch, we need to setup the relationship between your localbranch and remotebranch using following command
>**git branch --set-upstream master origin/branchname**

`master` is the local branch name, `origin` is the remote repository name you just create, and `branchname` is the remote branchname

after that you can use `git pull` and `git push` to sync your local branch with remote branch



