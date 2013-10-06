---
layout: post
title: "Manage Node With NVM"
description: ""
category: "Javascript"
tags: [Node]
---
{% include JB/setup %}

# install nvm
1. aptitude search nodejs
2. git clone git://github.com/creationix/nvm.git ~/nvm  (install git if not installed yet)
3. source ~/nvm/install.sh  (run shell script)
4. source ~/.profile (this is to refresh profile)
4. nvm  (run nvm command)

# nvm command

1. nvm ls  (List installed versions)
2. nvm install v0.11.7 (install node)
3. nvm use v.11.7 (set the version as current version)
4. nvm alias default v0.11.7 (set the default version when source the shell script)

