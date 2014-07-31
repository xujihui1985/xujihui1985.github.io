---
layout: post
title: "create node addon"
description: "create node c++ adddon"
category: "c++"
tags: [node]
---
{% include JB/setup %}



compile nodejs from windows

http://stackoverflow.com/questions/14278417/cannot-install-node-modules-that-require-compilation-on-windows-7-x64-vs2012


set up enviroment variable

setx NODE_HOME "%CD%"
setx NODE_ROOT "%CD%"

set | grep NODE_

in order to reference v8 and uv
we need to reference another two directory


or you could use node-gyp to build the c++

use `node-gyp configure` to create build file, if using
vs2012 add `--msvs_version=2012` switch

then you can goto build folder open the solution manully build the solution, or build by node-gyp build

note
** the node.lib gyp used is much old then the current gpy, if you build the current node version the syntax is largly different**


and the module name you create must same as the target_name in the binding.gyp

[http://stackoverflow.com/questions/11319461/cannot-load-node-js-native-addon-when-built-with-node-gyp-but-it-works-when-bui](http://stackoverflow.com/questions/11319461/cannot-load-node-js-native-addon-when-built-with-node-gyp-but-it-works-when-bui "Error: The specified procedure could not be found.")


	var addon = require('./build/Release/addon');
	
	console.log(addon.hello());

reference
http://dailyjs.com/2012/05/17/windows-and-node-3/
http://code.tutsplus.com/tutorials/writing-nodejs-addons--cms-21771
https://github.com/rvagg/node-addon-examples