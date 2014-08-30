---
layout: post
title: "java Optional - Nullable in java"
description: ""
category: "java"
tags: []
---
{% include JB/setup %}

In java8 there is new type Optional, which is same as Nullable in C#

Syntax

```
Optional<String> opt = Optional.of(...);
if (opt.isPresent()) {
	String s = opt.get();
} 
else {
	...
}

String s = opt.orElse("");
```

main method:

- isPresent: test if opt has value
- get: get the value from opt
- orElse: get the value or get the default


api [http://docs.oracle.com/javase/8/docs/api/java/util/Optional.html](http://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)