---
layout: post
title: "center element"
description: ""
category: "css"
tags: [position]
---
{% include JB/setup %}


### horizontally center

this is the easiest one, just give the element a width, and set the left right margin to auto

```
.box {
	width: 100px;
}

.horizontal-center {
	margin: 0 auto;
}

```

### vertically center

this is little trick, set the position of the element absolute, and set the top position to 50%,
and this not enough, then set the margin-top to the half height of the element;

```
.vertial-center {
	position: absolute;
    top: 50%;
    margin-top: -50px;
}

```

### both vertical and horizontal center

to make an element centered to the screen, first create an container which is vertical centered to the screen,
then set the margin to make the element horizontal center to the container

```
.contanier {
	position: absolute;
    top: 50%;
    margin-top: -50px;
    left: 0;
    width: 100%;
}

.vertical-horizontal-center {
    margin: 0 auto;
}

```