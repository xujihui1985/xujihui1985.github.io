---
layout: post
title: "debug nodejs"
description: ""
category: "nodejs"
tags: [debug]
---
{% include JB/setup %}


I have used `console.log` to debug `Grunt` for a long time until I found `node-inspector`

it is pretty easy to setup debug enviroment

1. install node-inspector globally

		npm install -g node-inspecor
        
2. inspect debug port
		
        node-inspecor &  //& means run in background
        
3. node --debug-brk $(whick grunt) `task`
if you saw `debugger listening on port 5858` then you success


4. open chrome goto http://localhost:8080/debug?port=5858
              
              
              
#### if you are working under windows. then you need enter the absolut path of node.exe as well as grunt-cli

    D:\work\projects\itms>"c:\Program Files\nodejs\node.exe" --debug-brk "C:\Program
     Files\nodejs\node_modules\grunt-cli\bin\grunt" release
     
     
**use node debug to debug node**
[debug node js](http://nodejs.org/api/debugger.html)

Stepping
cont, c - Continue execution
next, n - Step next
step, s - Step in
out, o - Step out
pause - Pause running code (like pause button in Developer Tools)

Watchers
To start watching an expression, type `watch("my_expression")`. watchers prints the active watchers. To remove a watcher, type `unwatch("my_expression")`.
