---
layout: post
title: "staticmethod and classmethod"
description: ""
category: "python"
tags: [staticmethod, classmethod]
---
{% include JB/setup %}


### staticmethod can be overwritten in subclass

```
class BaseClass:

    @staticmethod
    def _make_bic_code():
        return 'base class'

    def __init__(self):
        self.bic = BaseClass._make_bic_code()


class SubClass(BaseClass):

    @staticmethod
    def _make_bic_code():
        return 'sub class'

```

在BaseClass中，假如你不希望子类重写这个staticmethod,那么就通过 `BaseClass._make_bic_code` 调用这个方法，假如你希望子类重写这个方法，就用`self._make_bic_code`

###inherate

python中，在子类调用父类的方法通过 `super`关键字，这里要注意的是，在python 2.x 与 3.x中，写法上是有差别的

在python3中

```
class SubClass(BaseClass):

    def __init__(self, value):
        super().__init__()
        self.value = value
```

在python2中

[https://docs.python.org/2.7/library/functions.html?highlight=super#super](https://docs.python.org/2.7/library/functions.html?highlight=super#super)

```
class SubClass(BaseClass):

    def __init__(self, value):
        super(SubClass, self).__init__()
        self.value = value

class C(B):
    def method(self, arg):
        super(C, self).method(arg)
```

### classmethod can also be overwritten in subclass

在重写@classmethod，需要注意的是，假如子类构造函数的参数与父类不同的时候，直接调用会报错，比如

```
class BaseClass(object):

    @classmethod
    def create_instance(cls):
        return cls()


class SubClass(BaseClass):

    def __init__(self, value):
        super(SubClass, self).__init__()
        self.value = value

```

这时候`cls` 代表的是 SubClass， 所以在初始化的时候，没有传入value，会抛异常，解决办法可以通过  `*args, **kwargs`

```
class BaseClass(object):

    @classmethod
    def create_instance(cls, *args, **kwargs):
        return cls(*args, **kwargs)
```

### properties

```
class SubClass(BaseClass):

    def __init__(self, value):
        super(SubClass, self).__init__()
        self._celsius = value

    #getter property
    @property
    def celsius(self):
        return self._celsius

	#setter property
    @celsius.setter
    def celsius(self, value):
        self._celsius = value


class C(object):
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x

```

python 的getter setter还是比较怪的，它是通过decorator实现的，当给celsius函数加上@property之后，它就变成了setter函数，我们知道，decorator会返回一个新的函数，这时候，实际上在selsius函数上注入了 setter属性，它也是一个decorator

### override property in subclass

在子类可以重写父类的property

```

class SubClass(BaseClass):
	@BaseClass.myproperty.setter
	def myproperty(self,value):
		BaseClass.myproperty.fset(self,value)

```

在子了重写父类属性的setter方法有点麻烦, 可以通过templatemethod来解决这个问题

```
class BaseClass:
	@property
	def base_property(self):
		return self._get_base_property()

	@base_property.setter
	def base_property(self,value):
		self._set_base_property(value)

```

由于普通方法可以很容易的重写，假如需要重写父类的属性，可以利用templatemethod pattern来定义两个私有函数，在子类中重写这两个私有函数