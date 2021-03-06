---
layout: post
title: "C++ basic"
description: ""
category: ""
tags: [c++]
---
{% include JB/setup %}

```c
	
	const pointer //you can't change it to point to somewhere else
	int * const cpI;

	const int* cpI //you can't use it to change the value of the target
```

**shared_ptr**

```c

	#include <memory>
	std::shared_ptr<Resource> pResource; //declare a shared_ptr
	// use std::make_shared to initialize Resource instance
	pResource = std::make_shared<Resource>(firstname);

	auto sharedPointer = std::make_shared<Person>("Sean", "xu");
```

**References**

the `&` before variable is to get the address of that variable eg: 

```c
	int i = 10;
	int* pint = &i;  // which means pint is a point to i
	
	*pint = 12;  // change the value that pint point to in this case the value of i will be changed to 12

	 int& j = i;

     j = 12;
 
```

```c
	Tweeter localT("Twetter",123,"@local");
	
	
	Person& localp = localT;

	localp.GetName();
```

what's the difference between reference and pointer?

[http://stackoverflow.com/questions/57483/what-are-the-differences-between-pointer-variable-and-reference-variable-in-c](http://stackoverflow.com/questions/57483/what-are-the-differences-between-pointer-variable-and-reference-variable-in-c "stackoverflow: the difference between reference and pointer")