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
		self.assert(True, 'pass')

	def test_raise_error(self):
		with self.assertRaises(KeyError):
			do_something()

	@unittest.skip("WIP")
	def test_skip_test(self):
		pass

```

run unittests

```
under project folder, run

python -m unittest  #this will discover all the tests under the directory and run them

```
