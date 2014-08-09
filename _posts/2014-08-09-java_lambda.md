---
layout: post
title: "new post"
description: ""
category: "java"
tags: [lambda]
---
{% include JB/setup %}

Question:
what is **Lambda**?
Answer:
Lambda in java is a **functional interface**

Question"
then what is **functional interface**?
Answer:
A functional interface is an interface with **only one** abstract method

```java
public interface Runnable {

	run();
}

public interface Comparator<T> {

	int compare(T t1, T t2);
}

public interface FileFilter {

	boolean accept(File pathname);
}

```

### functional Interface

```java
@FunctionalInterface
public interface MyFunctionalInterface {
	void someMethod();
}

```

*if there are multiple abstract method defined in the interface, then it is not a functionalInterface, there will bring me a compile error*

### Functional Interfaces Toolbox

new package: java.util.function
with a rich set of functional interfaces

```java
@FunctionalInterface
public interface Supplier<T> {
	T get();
}

@FunctionalInterface
public interface Consumer<T> {
	T accept();
}

@FunctionalInterface
public interface BiConsumer<T,U> {
	T accept(T t, U u);
}

@FunctionalInterface
public interface Predicate<T> {
	boolean test(T t);
}

@FunctionalInterface
public interface Predicate<T,U> {
	boolean test(T t, U u);
}

@FunctionalInterface
public interface Function<T,R> {
	R apply(T t);
}

@FunctionalInterface
public interface BiFunction<T,U,R> {
	R apply(T t, U,u);
}

// functional interfaces can be extended
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {

}
```

### Method References

```java
Consumer<String> c = s -> System.out.println(s);

can be written like 

Consumer<String> c = System.out::println;
```

### process data in list

```java

List<Customer> customers = customerService.retrieve();
customers.forEach(customer -> System.out.println(customer));
customers.forEach(System.out::println); 

```


### Interface Default method

```java

// this is how forEach implement in java, this concept is like extension method in C#
public interface Iterable<E> {
	default void forEach(Consumer<E> consumer) {
		for (E e : this) {
			consumer.accept(e);
		}
	}
}

``` 

### Predicate

```java

Predicate<String> p1 = s -> s.length() < 20;
Predicate<String> p2 = s -> s.length() > 10;

Predicate<String> p3 = p1.and(p2);

```
![](http://imgur.com/CVyZtLV)