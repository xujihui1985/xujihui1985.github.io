---
layout: post
title: "Generic in Java"
description: ""
category: "Generic"
tags: [java]
---
{% include JB/setup %}


### Generic Constraints

```java

public class Printer<T extends ICartridge>

```

### Generic Method

```java

public <T,U> void printUsingCartridge(T cartridge, U message){
	System.out.println(cartridge.toString() + message);
}

```

### Generic Wildcards

```java
private static void printOne(Printer<? extends ICartridge> printer){
		
}
```

### difference between Csharp

They disappear after being compiled

that means you **can not** do that
	
```java
T thing = new T();
```
and you can not check the type of T during runtime

```java
if(myObject instanceof T) //compiler Error!
```

>Cannot perform instanceof check against type parameter T. Use its erasure ICartridge instead since further generic type information will be erased at runtime	