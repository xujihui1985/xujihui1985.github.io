---
layout: post
title: "pointer in c++"
description: ""
category: "cpp"
tags: [pointer]
---
{% include JB/setup %}

1. `shared_ptr`
1.1 when last use goes out of scope, object on free store is deleted
1.2 `make_shared` improves performance

2. `unique_ptr`
2.1 less overhead than shared_ptr
2.2 Not copyable(but movable)


**use a pointer**
1. Member variable, lifetime tied to the class
2. A way to observe/access something without impact on its lifetime
	- a pointer back to your owner or container
	- a list of the resources your're using
3. Shared access to an object on the free store
	- with no single owner
