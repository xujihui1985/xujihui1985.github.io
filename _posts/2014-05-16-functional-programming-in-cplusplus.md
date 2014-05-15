---
layout: post
title: "functional programming in c++"
description: ""
category: ""
tags: [c++]
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