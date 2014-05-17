---
layout: post
title: "functional programming in c++"
description: ""
category: ""
tags: [cpp]
---
{% include JB/setup %}


I'm always thinking c++ is very boring, but it can be fun


## Iterator Collection

	  std::vector<int> v1;
	  v1.push_back(111);
	  v1.push_back(222);
	  v1.push_back(333);

create a vector

1 simplest way to iterator a vector

	for (auto i = v1.begin(); i < v1.end(); i++)
    {
        std::cout << *i << std::endl;
    }

*auto is new syntax in c++ 11*

2 use `for_each`

	#include <algorithm>
	std::for_each(v1.begin(),v1.end(),[](int i){ std::cout << i << std::endl; });


`[](int i){ std::cout << i << std::endl; }` is lambda in c++, and it can be replaced by function


## return a value from lambda

	std::transform(v1.begin(),v2.end(),std::back_inserter(v2),[](int n)->int { return n * n * n ;});

here I create a new vector v2, and want to transform the item from v1 to v2, use std::transform from `<algorithm>`, the last parameter it accept is a lambda, that return `int`
	[](int n)->int { return n * n * n ;}


## capture

	[x,y](int n)->int { return x * y * n ;}

	[&x,&y](int n)->int { return x * y * n ;}  // can also pass by reference

	[=] //copy "everything"
	
	[&] //copy everything my reference

this will copy x and y from local scope into lambda, then when lambda execute, it can get the copy of `x` and `y`

	std::for_each(v1.begin(),v1.end(),[&x,y](int i){ 
        std::cout << i << " " << x << " "<< y << std::endl; 
        x = 9;
    });

## populating a vector

```cpp
	//use for loop
	for (int i =0; i <5; i++)
	{
		v.push_bach(i);
	}

	// use algorithm
    vector<int> v;
    int i=0;
    std::generate_n(std::back_inserter(v),5,[&i]()->int{ return i++; });

```

## counting the number of 3's

use algorithm
```cpp
	int count3 = std::count(v.begin(),v.end(),3);
    std::cout << "the count3 of v is " << count3 << std::endl;
```
use for loop

```cpp
	
	int count = 0;
	for(unsigned int i = 0; i<v.size(); i++)
	{
		if(v[i] == 3)
			count++;
	}

```

use iterator

```cpp
	int count = 0;
	for(auto it = begin(v); it != end(v); it++)
	{
		if(*it == 3)
		{
			count++;
		}
	}

```

## remove element from vector

** the result of `remove_if` point to the end of the vector, for preformance reason, remove_if didn't erase element, it will push the "removed" element back to end**

```cpp
	  vector<int> v;
      int i=0;
      std::generate_n(std::back_inserter(v),5,[&i]()->int{ return i++; });
      auto vcopy = v;
	  auto endv = std::remove_if(begin(vcopy),end(vcopy),[=](int elem)->bool { return (elem == 2 || elem == 3);});
      vcopy.erase(endv, end(vcopy));
```
vcopy.erase(endv, end(vcopy)); means remove from the **new end** to **original end**

## function pointer

	typedef int (*CHANGER)(int i);

define a function pointer CHANGER, take integer as parameter and return integer type

	int doubler(int i)
	{
		return i * 2;
	}

	int tripler(int i)
	{
		return i * 3;
	}

	CHANGER = doubler;

	CHANGER(2);  ==> 4


## Pointer to Member Function

	typedef int (Utility::* UF)();

	UF action;
	action = &Utility::triplex;

	call:
	Utility u(i);
	UF action;
	if(i > 3)
	{
		action = &Utility::triplex;
	}
	else
	{
		action = &Utility::doublex;
	}
	return (u.*action)();  //because it is pointer, need to be deferenced


## void*

can be cast to any kind of pointer

	void SomeMethod(void* something)
	{
		int* s = (int*) something;
		cout << *s << endl;
	}


