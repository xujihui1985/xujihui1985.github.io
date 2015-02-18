---
layout: post
title: "new post"
description: ""
category: "python"
tags: [str,repr,format]
---
{% include JB/setup %}

相比js 或者 c#中的`toString()`, python 的 3个magic method更加灵活

```
class Point2D:
	def __init__(self, x, y):
		self.x = x
		self.y = y

	def __str__(self):
		return '({},{})'.format(self.x, self.y)

	def __repr__(self):
		return 'Point2D(x={}, y={})'.format(self.x, self.y)

	def __format__(self, f):
		if f =='r':
			return '{}, {}'.format(self.y, self.x)
		else:
			return '{}, {}'.format(self.x, self.y)
```

`__str__` is used to display infomation to the users

`__repr__` is used to display infomation to developer

`__format__` will be used in `.format` function


when you want to show a large list of object using repr method, it will be hard to read, then we can use `reprlib.repr` method, it will show partial list of the entire list

`ascii ord chr`

`ascii` replaces non-ASCII characters with escape sequences

`ord` converts a single character to its interger unicode codepoint

`chr` converts an integer unicode codepoint to a single character string