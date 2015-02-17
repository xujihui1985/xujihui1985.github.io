---
layout: post
title: "closure and decorator"
description: "closure and decorator"
category: "python"
tags: [function, closure]
---
{% include JB/setup %}


和javascript一样，在一个函数中,python也能定义一个local 函数

```
g = 'global'
def outer(p='param'):
	l = 'local'
	def inner():
		print(g, p, l)
	inner()
		
outer()

```

inner 函数在每次outer调用时被创建，也就是说，每个inner函数都是一个新的函数对象，它的生命周期只存在于outer函数内，不能被外部调用

### closure

python 通过 `__closure__`属性实现closure

```
def enclosing():
	x = 'closed over'
	def local_func():
		print(x)
	return local_func

local_func = enclosing()
local_func.__closure__
```

### globle and nonlocal

当在closure中要改变enclosing scope中的变量时，可以通过`nonlocal`关键字

```
import time

def make_timer():
	last_called = None
	def elapsed():
		nonlocal last_called   //noted
		now = time.time()
		if last_called is None:
			last_called = now
			return None
		result = now - last_called
		last_called = now
		return result
	return elapsed
```

### decorators

```
@my_decorator
def my_function():
	...
```

python 首先compile my_function， 然后将my_function函数对象传给 my_decorator(f)

decorator 将函数作为参数，并且返回一个新的函数

[demo](https://github.com/xujihui1985/pythonDemo/blob/master/decoratorDemo/escape_unicode.py)

函数可以作为decorator, class也可以是decorator, 由于Instance也是callable，所以也可以作为decorator

noted: class的init函数必须接受 function 作为参数，必须实现 `__call__`函数

### instances as decorators

[demo here](https://github.com/xujihui1985/pythonDemo/blob/master/decoratorDemo/call_count.py)

### decorators are combinable

[demo here](https://github.com/xujihui1985/pythonDemo/blob/master/decoratorDemo/combile_decorators.py)

### functools.wraps

decorate会导致原函数的meta data 丢失， 比如，我们有个 hello函数

```
def hello():
	"hello function"
	print('hello')

在 repl中 help(hello)， 我们能看到函数的名字 hello以及函数的doc
```

但这时候假如给它加上一个decorator, 就丢失了它原有的这些元数据
可以直接将 wrap函数的 `__name__` `__doc__`赋值成原来函数的数据，但这样做不好看

`functools.wraps`可以用来解决这个问题

[demo here](https://github.com/xujihui1985/pythonDemo/blob/master/decoratorDemo/functools_demo.py)

more example

[https://github.com/xujihui1985/pythonDemo/blob/master/decoratorDemo/validating_arguments.py](https://github.com/xujihui1985/pythonDemo/blob/master/decoratorDemo/validating_arguments.py)