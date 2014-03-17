---
layout: post
title: "install pip use mirror in China"
description: ""
category: "pip"
tags: [python]
---
{% include JB/setup %}

pipy国内镜像目前有：
 
http://pypi.douban.com/  豆瓣
http://pypi.hustunique.com/  华中理工大学
http://pypi.sdutlinux.org/  山东理工大学
http://pypi.mirrors.ustc.edu.cn/  中国科学技术大学
 
对于pip这种在线安装的方式来说，很方便，但网络不稳定的话很要命。使用国内镜像相对好一些，
 
如果想手动指定源，可以在pip后面跟-i 来指定源，比如用豆瓣的源来安装web.py框架：
pip install web.py -i http://pypi.douban.com/simple
 
注意后面要有/simple目录！！！
 
要配制成默认的话，需要创建或修改配置文件（linux的文件在~/.pip/pip.conf，windows在%HOMEPATH%\pip\pip.ini），修改内容为：
code:
[global]
index-url = http://pypi.douban.com/simple
 
这样在使用pip来安装时，会默认调用该镜像。
更多配置参数见：http://www.pip-installer.org/en/latest/configuration.html
 
 
 
Configuration

Config file

pip allows you to set all command line option defaults in a standard ini style config file.

The names and locations of the configuration files vary slightly across platforms.

On Unix and Mac OS X the configuration file is: $HOME/.pip/pip.conf
On Windows, the configuration file is: %HOME%\pip\pip.ini
You can set a custom path location for the config file using the environment variable PIP_CONFIG_FILE.

The names of the settings are derived from the long command line option, e.g. if you want to use a different package index (--index-url) and set the HTTP timeout (--default-timeout) to 60 seconds your config file would look like this:

[global]
timeout = 60
index-url = http://download.zope.org/ppix