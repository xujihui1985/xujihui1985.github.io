---
layout: post
title: "Garbage Collect"
description: ""
category: "java"
tags: [garbage collect]
---
{% include JB/setup %}


### Basic ideas

1. Has a `young generation` and an `old generation`
2. Most initial objects allocated in `Eden space`
	- Part of young generation
3. Young generation also has two survivor spaces
	- Objects that survive a GC get moved to the survivor space
	- Only one survivor space in use at a time
	- Objects copied between survivor spaces 
4. Old generation is where long lived objects go to die

### Java Memory Modal

![](http://i.imgur.com/QVYbFbo.png)

##### Minor Garbage Collection
1. Objects allocated into Eden space
	- When GC runs objects are copied to newer survivor space
	- Objects from older survivor space also copied to newer survivor space
	- Survivor spaces are swapped
2. New objects allocated into Eden space

##### Major Garbage Collection
1. Triggered when the tenured(old) space is full
2. Collects both old and young generations

##### Copying to old generation
JVM will eventually promote to old generation
	- after a centain number (15) of garbage collectes
	- if survivor space is full
	- if JVM has been told to always create objectes in old space by `-XX:+AlwaysTenure`

##### Allocating Objects to Old Space
1. Objects over a certain size will be allocated directly in old space
	- No JVM option to force objects to old space
2. Option `-XX:PretenureSizeThreshold=<n>`
	- all objects larger then <n> bytes should be allocated directly in old space
	- however if object size fits TLAB (Thread Local Allocation Buffers), JVM will allocate it in TLAB
	- You should also limit TLAB size

##### What does live Mean

1. Live roots
	- From stack frames
	- static variables
	- Others such as JNI (Java Native interface) and synchronization monitors

**what about reference from Old generation to young generation?**

so when this happened, do we need to scan old space when Minor Garbage Collection?

the answer is no, jvm use card table to help

##### Card table
1. Each write to a reference to a young object goes through a write barrier
2. This barrier updates a card table entry
3. Minor GC scans table looking for the areas that contain references
4. Load that memory and follow the reference

#### Different Garbage Collectors
1. Serial generational collector
	- -XX:+UseSerialGC
2. Parallel for yount space, serial for old space generational collector
	- -XX:+UseParallelGC
3. Parallel young and old space generational collector
	- -XX:+UseParallelOldGC
4. Concurrent mark sweep with serial young space collector
	- -XX:+UseConcMarkSweepGC
	- -XX:-UseParNewGC
5. Concurrent mark sweep with parallel young space collector
	- -XX:+UseConcMarkSweepGC
	- -XX:+UseParNewGC
6. G1 garbage collector
	- -XX:+UseG1GC

##### Serial Collector
1. single threaded
2. Mark and sweep
3. Ok for small applications running on the client

##### Parallel Collector
1. Multiple threads for minor collection
2. Single thread for major collection
3. Same process as Serial
4. Use on Servers

##### parallel Old Collector
1. Multiple threads for minor and major collections
2. Preferred over ParallelGC

##### Concurrent Mark and Sweep
1. Only collects old space
2. No longer bump the pointer allocation
3. Causes heap fragmentations
4. Designed to be lower latency

![](http://i.imgur.com/uqzzzO2.png)

##### G1
1. Officially supported in java7
2. Is a compacting collector
3. Planned as a replacement for CMS


### Tools

1. MXBeans
2. Jstat
3. VisualVM and VisualGC