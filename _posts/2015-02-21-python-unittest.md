---
layout: post
title: "python unittest"
description: ""
category: "python"
tags: [unittest]
---
{% include JB/setup %}


there are two main test framework in python, `unittest` and `pytest`

unittest comes with the standardlibrary, here is the example

filename: test_resolver.py

```
import unittest

class ResolverTest(unittest.TestCase):
	
	def test_clear(self):
		assert(True, 'pass')

```

