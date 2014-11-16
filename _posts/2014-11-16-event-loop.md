---
layout: post
title: "event loop"
description: ""
category: "javascript"
tags: [eventloop]
---
{% include JB/setup %}


What is javascript Eventloop

First of all.

Javascript is single thread, that means, it do one thing at one time, it has only **one thread (main thread)**, **one call stack **

![](http://i.imgur.com/sA66g8x.png)

when an async function called, like setTimeout, XMLHttpRequest, Dom event called, it was executed on **another thread**, which is browser **webapis** and pop the main stack immedidatly and when it done, it put it's callback on the **task queue**

![](http://i.imgur.com/Jm2ufzj.png)

and then, **eventloop** comes in, eventloop did only one thing, it looks the stack and the task queue, if the **stack is empty** (important), it pop the first thing from the queue to the stack

eventloop is like a worker, to transport **event** from eventloop to the callstack to execute the callback

![](http://i.imgur.com/5G1giN3.png)

#### why async function matter?

![](http://i.imgur.com/dLZiKuh.png)

the browser do repaint every 16 ms (1000/60FPS), and it will be blocked if there is something executed on the callstack. so if we are doing something long time in a blocking way, the ui blocked, and you can't click on the browser, but if we seprate it into chunck and do it by async way, we give the browser a chance to repaint (the priority of repaint queue is higher then task queue).