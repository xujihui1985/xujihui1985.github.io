---
layout: post
title: "list comprehension"
description: ""
category: "python"
tags: [iterable, comprehension]
---
{% include JB/setup %}

```
l = [i * 2 for i in range(10)]  # list
d = {i: i * 2 for in in range(10)} # dict
s = {i for i in range(10)} # set
g = (i for i in range(10)) # generator
```

combine two comprehension

```
[(x,y) for x in range(5) for y in range(3)]

this is equal to

l = []
for x in range(5):
	for y in range(3):
		l.append((x,y))
```

combine if statement with comprehension

```
values = [x / (x -y) 
		 for x in range(100) 
		 if x > 50]

this is equal to 

values = []
for x in range(100):
	if x >　50:
		values.append(x/(x-y))
```

###　map

map takes a function, and an iteratable, and return a map function object, it will be evaluated until it was called, aka lazy loading

```
for c in map(ord, 'some char'):
	print(c)
```

ord is a function convert char to codepoint

```
def combine(size, color, animal):
	return '{} {} {}'.format(size, color, animal)

sizes = ['small', 'medium', 'large']
colors = ['lavender', 'teal', 'burnt orange']
animals = ['koala', 'platypus', 'salamander']

list(map(combine, sizes, colors, animals))
```

### filter

```
filter(is_odd, [1,2,3,4,5,6])
```

### reduce

```
from functools import reduce
import operator
reduce(operator.add, [1,2,3,4,5])
```

### iterable

an object which implements the `__iter__()` method and which implements the `__next__()` method, and `raise StopIteration` when finish iterator

```
class ExampleIterator:
	def __init__(self, data):
		self.index = 0
		self.data = data

	def __iter__(self):
		return self

	def __next__(self):
		if self.index >= len(self.data):
			raise StopIteration()
		rslt = self.data[self.index]
		self.index += 1
		return rslt

class ExampleIterable:
	def __init__(self):
		self.data = [1,2,3]

	def __iter__(self):
		return ExampleIterator(self.data)
```

or we can implement `__getitem__` as an alternative approach

```
class AlternateIterable:
	def __init__(self):
		self.data = [1,2,3]

	def __getitem__(self, idx):
		return self.data[idx]
```

### iter

```
iter(calable, sentinel)

iteration stops when callable produces the value same as sentinel
```

for example:

```
import datetime

i = iter(datetime.datetime.now, None)
next(i)
next(i)

datetime.datetime.now is a function, and it will never return None, so this become an infinite iter
```

### summary

```
import datetime
import itertools
import random
import time

class Sensor:
	def __iter__(self):
		return self

	def __next__(self):
		return random.random()

sensor = Sensor()
timestamps = iter(datetime.datetime.now, None)

for stamp, value in itertools.islice(zip(timestamps, sensor), 10):
	print(stamp, value)
	time.sleep(1)

```

### container protocol

to implement `__contains__`, you can test if the element in the container by calling `item in container`

```
class TestContainerProtocal(unittest.TestCase):

    def setUp(self):
        self.s = SortedSet([6,7,4,9])

    def test_positive_contained(self):
        self.assertTrue(6 in self.s)

    def test_negative_contained(self):
        self.assertFalse(2 in self.s)

    def test_positive_not_contained(self):
        self.assertTrue(5 not in self.s)


class SortedSet:

    def __init__(self, items=None):
        self._items = sorted(items) if items is not None else []

    def __contains__(self, item):
        return item in self._items

```

### sized protocol

Number of items using len(sized) function
Must not consume or modify collection
to implement the special method `__len__()`

```
  def __len__(self):
        return len(self._items)
```

### iter protocol

```

class SortedSet:

    def __init__(self, items=None):
        self._items = sorted(set(items)) if items is not None else []

    def __iter__(self):
        return iter(self._items)


class TestIterableProtocol(unittest.TestCase):
    def setUp(self):
        self.s = SortedSet([2,3,1,4])

    def test_for_loop(self):
        index = 0
        expected = [1,2,3,4]
        for item in self.s:
            self.assertEqual(item, expected[index])
            index += 1

```

### sequence protocol

to implement `__getitem__` method, we can call item[5] this is same as c# list accessor

```
class SortedSet:

    def __init__(self, items=None):
        self._items = sorted(set(items)) if items is not None else []

    
    def __getitem__(self, index):
        result = self._items[index]
        return SortedSet(result) if isinstance(index, slice) else result

    def __repr__(self):
        return "SortedSet({})".format(
            repr(self._items) if self._items else ''
        )


class TestSequenceProtocol(unittest.TestCase):
    def setUp(self):
        self.s = SortedSet([1,4,56,3,2])

    def test_index_zero(self):
        self.assertEqual(self.s[0], 1)

    def test_index_one_beyond_the_end(self):
        with self.assertRaises(IndexError):
            self.s[5]

    def test_slice_to_end(self):
        self.assertEqual(self.s[:3], SortedSet([1,2,3]))

```

to implment slice function `list[:3]`, we need to check if the second parameter of `__getitem__` is instanceof slice, if it is slice, then return a new SortedSet, otherwise return the result, this still cannot pass the test
because we haven't implement equal function to test if two SortSet are equal

###　implement `__eq__` and `__ne__`

python check for reference by default, by implement `__eq__` and `__ne__` we can test two object

notice that it's `return` NotImplemented instead of `raise` NotImplemented
```
def __eq__(self, rhs):
	if not isinstance(rhs, SortedSet):
		return NotImplemented
	return self._items == rhs._items

def __nq__(self, rhs):
	if not isinstance(rhs, SortedSet):
		return NotImplemented
	return self._items != rhs._items
```