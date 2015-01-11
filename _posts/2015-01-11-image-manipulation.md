---
layout: post
title: "image manipulation"
description: ""
category: "image"
tags: [math]
---
{% include JB/setup %}


### first, let's do some math

![](http://i.imgur.com/IZ99m5u.png)

> Sin = Opposite / Hypotenuse
> Cosine = Adjacent / Hypotenuse
> Tangent = Opposite / Adjacent

when rotate a trangle, we need to calculate it's bounding box

![](http://i.imgur.com/fj187Cu.png)

the bounding box height = (sin(angle) * (width of original box)) + (cos(angle) * (height of original box))

the bounding box weight = (cos(angle) * (width of original box)) + (sin(angle) * (height of original box))