---
layout: post
title: "python inheritance"
description: ""
category: "python"
tags: [inheritance]
---
{% include JB/setup %}


in python we can use `type(instance)` to check the type of the instance

eg:

```
i = 7
type(i) --> int
```

infact, type function checks `__class__` property of the instance

```
i.__class__
```

how to check if an instance is type of T

```
isinstance(3, int)
isinstance('hello', str)
isinstance(1.123, bytes)  ==>False

x = []
isinstance(x, (float, dict, list))
```

check if one type is a subclass of another

```
issubclass(T, T)

issubclass(SortedList, SimpleList)

```

### dir

we can use `dir` to check the properties of the instance

### getattr

we can use `getattr` to get the attribute from an instance, it's like reflection in other language

```
i = 7

getattr(i, 'denominator') == i.denominator

callable(getattr(i, 'conjugate'))
```

`hasattr` can used to check if attr exists on the instance

```
hasattr(i, 'bit_length') ==> True  # perfer to use exception handler instead of test hasattr, because
								   # internally, hasattr use exception to test the attribute		
```

### globals and locals

```
we can use globals() and locals() function to check the scope variable, globals() like window object in javascript
```

### inspect in standard library

```
import inspect

inspect.ismodule(sorted_set)
inspect.getmembers(sorted_set)
inspect.getmembers(sorted_set, inspect.isclass)
init_sig = inspect.signature(sorted_set.SrotedSet.__init__)  # inspect the signature of the function
init_sig.parameters
 
```


### multiple inheritance

```
class SubClass(BaseClass1, BaseClass2):
	pass
```

if a class
1. has multiple base classes
2. defines no initializer

then only the initializer of the first base class is automatically called

```
class Base1:
	def __init__(self):
		print('Base1.__init__')

class Base2:
	def __init__(self):
		print('Base2.__init__')

class Sub(Base1, Base2):
	pass

s = Sub()

==> Base1.__init__

```

#### `__bases__`

can use `__bases__` property to determine the base class of the type T

```
print(Sub.__bases__) 
==> Base1 Base2
```

### `__mro__`

mro means method resolution order

ordering that determines method name lookup

```
Sub.__mro__
```

for example, if when calling `do_something` on Sub instance, python will find the method according to the `__mro__` list, as soon as python find the method, it use the method

### super

There are two types of bound proxies, instance-bound and class-bound

#### class-bound

`super(base-class, derived-class)`

1. python finds MRO for derived-class first
2. then finds base-class in that MRO
3. it takes everything after base-class in the MRO, and finds the first class in that sequence with a matching method name


for example

we now have 4 class

```
SortedIntList -> IntList -> SortedList -> SimpleList -> object

super(SortedList, SortedIntList).add -->

SimpleList.add at xxxxx

```

it find SortedList in SortedIntList's bases class, then, it find add method from SimpleList

### instance-Bound Super

`super(class, instance-of-class)`

1. finds the MRO for the type of the second argument
2. finds the location of the first argument in the MRO
3. Uses everything after that for resolving methods

### calling super without argument


calling `super()` withdout arguments on instance method in python3, is same as calling `super(class-of-method, self)` where class-of-method is the class of the calling method, self is the instance, this is equvilent to **calling the method in the base class**

when calling on class method, it is same as `super(class-of-method, class)` this is equvilent to **calling the method in the base class**