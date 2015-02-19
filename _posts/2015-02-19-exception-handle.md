---
layout: post
title: "exception handling"
description: ""
category: "python"
tags: [exception]
---
{% include JB/setup %}

```
try:
...
except:
...
```

```
try:
except ValueError:

try:
except ValueError as e:
	print("payload:", e.args)
	print(str(e))
```

we can check the error hierarchy by looking the mro() of the Error class, we take `IndexError` as example

![indexerror](http://seanxu.oss-cn-hangzhou.aliyuncs.com/blog/indexerror.PNG)

### Argument Guard

```
def median(iterable):
	items = sorted(iterable)
	if len(items) == 0:
		raise ValueError("median() arg is an empty sequence")

def main():
	try:
		median([])
	except ValueError as e:
		print(str(e))
``` 

### defining new exception

```
class MyOwnError(Exception):
	def __init__(self, text, sides):
		super().__init__(text)
		self._sides = tuple(sides)

	@property
	def sides(self):
		return self._sides
	
	def __str__(self):
		return "'{}' for sides {}".format(self.args[0], self._sides)  # self.args from baseClass Exception

def raise_error():
	if xxxx :
		raise MyOwnError("Illegal operation", sides)
```

### exclipt chain exception

```
class InclinationError(Exception):
	pass

def inclination(x, y):
	try:
		return math.degrees(math.atan(y / x))
	except ZeroDivisionError as e:
		raise InclinationError("division error") from e

the inner exception can be retrived by __cause__ property

try:
	inclination(0, 1)
except InclinationError as e:
	print(e)
	print(e.__cause__)

```

### assertions

```
assert True/False, 'message'
```


### context manager

```
with expression as x:
	# enter()
	body
	# exit()
```

this is same as `using` in c#
```
using() {
}
```

### context-manager protocol

```
class LoggingContextManager:
	def __enter__(self):
		return self

	def __exit__(self, exc_type, exc_val, exc_tb):
		return

from lcm import *
with LoggingContextManager() as x:
	pass
```

`with LoggingContextManager() as x:` here x is not the return value of LoggingContextManager(), it's the return value of `__enter__` function


`__enter__`  

1. called before entering with-statement body
2. return value bound to `as variable`
3. can return value of any type
4. commonly returns context-manager itself

example

```
with open('data.txt', 'r') as f:
	data = f.read()

file.__enter__() must be returing the file object itself

```

`__exit__`

1. called when with-statement body exits

when exception raised during the operation, the function accept three arguments to handle the exception

```
__exit__(self, exc_type, exc_val, exc_tb)

exc_type: exception type
exc_val: exception object
exc_tb: exception traceback

when exit without exception, all there three arguments are None
```

so we can check type for None to see if an exception was thrown

if `__exit__` returns False or doesn't return anything, the exception raised in the with block is propagated, otherwise it swallow the exceptions

### contextlib.contextmanager decorator

```
import contextlib
import sys

@contextlib.contextmanager
def logging_context_manager():
	print('logging_context_manager: enter')  # this like calling __enter__ function 
	try:
		yield 'in the with-block'            # the value yield is like the value return by __enter__, and it 
		print('normal exit')                 # is bind to `as value`
	except Exception:
		print('exceptional exit', sys.exc_info())
		raise                                # muse explictly call raise to rethrow the exception, raise is like 
											 # throw; in c#

with logging_context_manager() as x:
	raise ValueError('something went wrong')
```

### multiple context managers

```
with propagater('outer', True) as p1, propagater('inner', False) as p2:
	pass
```
