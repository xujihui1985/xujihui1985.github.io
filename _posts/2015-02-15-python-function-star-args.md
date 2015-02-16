---
layout: post
title: "functions"
description: ""
category: "python"
tags: [function]
---
{% include JB/setup %}

### callable instances

python 中，对象是可以被invoke的，

[callable instance demo](https://github.com/xujihui1985/pythonDemo/blob/master/resolver/resolver.py)

实现了 `__call__` 函数的class, 其instance是可以被调用的

```
class Resolver:

    def __init__(self):
        self._cache={}

    #make class callable
    def __call__(self, host):
        if host not in self._cache:
            self._cache[host] = socket.gethostbyname(host)
        return self._cache[host]



resolver_instance = Resolver();

resolver_instance('hostname');

```

### class are also callable

[callable class demo](https://github.com/xujihui1985/pythonDemo/blob/master/immutablesequence/immutablesequence.py)

for class, 当调用class的时候，`__init__(self)` 会被调用

### conditional expression

[conditional expression](https://github.com/xujihui1985/pythonDemo/blob/master/conditional_expression/conditionalexpresson.py)

```
result = true_value if condition else false_value
```

### lambda

lambda就是匿名函数

```
def first_name(name):
	return name.split()[0]

与下面这个lambda等价

lambda name: name.split()[-1]

多个参数的lambda:

lambda first_name, last_name: print('{} {}'.format(first_name, last_name))

假如 lambda函数没有参数，类似.net中的 Action 那么

lambda: print('lambda without parameter')

lambda 可以复制给一个变量

is_odd = lambda x: x % 2 == 1

is_odd(12)

```

### detecting callable objects

the built-in function `callable()`

### extended arguments

`*args`

[extendargs demo](https://github.com/xujihui1985/pythonDemo/blob/master/extendedarguments/extendedarguments.py)

```
def hypervolume(*args):
	print(args)
	print(type(args))

hypervolume(3,4)

>>>> (3,4)
>>>>  tuple
```

我们可以使用 `positional argument` 加上 `* args`这种方式

```
def hypervolume(length, *lengths):
	v = length
	for item in lengths:
		v *= item
	return v
```

### `**kwargs`

```
def tag(name, **kwargs):
	print(name)
	print(kwargs)
	print(type(kwargs))

tag('img', src="moment.jpg", alt="xxxxx")

>>> <class 'dict'>

```

`**kwargs` 对应的是 dict 类型

### Extended call syntax

加入一个函数有 `*` 参数

```
def extend_args(arg1, arg2, *args):
	print(arg1)
	print(arg2)
	print(args)

可以创建一个 tuple，

t=(1,2,3,4,5)

这时候把t传入 函数

extend_args(*t)  注意t前的*， 这里的*是把t unpack

>> 1
>> 2
>> (3,4,5)
```

同样的可以用在 `**kwargs`

```
def color(red, green, blue, **kwargs):
	print(kwargs)

k = {'red':21, 'green':68, 'blue':120, 'alpha':52}

color(**k)
```

### forwarding arguments

通过组合 `*args` 与 `**args` 可以用来forward arguments, 类似js中的arguments

```
def trace(fn, *args, **kwargs):
	print("args =", args)
	print("kwargs =", kwargs)
	result = fn(*args, **kwargs)
	print("result =", result)
	return result

int("ff", base=16)
trace(int, "ff", base=16)  #"ff"是positional argument, base=16是named arguement
```

### 行转列

假设有这样一个数据结构

```
arr1 = [1,2,3]
arr2 = [4,5,6]
arr3 = [7,8,9]

想把它转成 

[(1,4,7),(2,5,8),(3,6,9)]

```

zip 接受 `*iterable`， 扔个实现`__iter__`的对象都能作为参数

1. 通过 zip 可以把这两个数组合并

```
for item in zip(arr1, arr2, arr3):
	print(item)
```

2. 假设现在有arr4

```
arr4 = [arr1,arr2,arr3]

for item in zip(*arr4):
	print(item)

transposed = list(zip(*arr4))
这样就把行转成列了
```


