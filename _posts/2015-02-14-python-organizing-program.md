---
layout: post
title: "Organizing Large Programs"
description: ""
category: "python"
tags: [package]
---
{% include JB/setup %}

### Package

Python 是怎么找modules的？

python 会从 sys.path 中查找模块， sys.path是一个文件目录的数组，python会从这些目录中查找模块

```python
import sys

print sys.path
```

其中 `site-packages` 中存放的是第三方模块， `sys.path[0]` 为 `''` 表示，python会首先查找当前目录

有几种方法可以用来把你的模块加入到python可查找的路径中

```
import sys
sys.path.append('the_folder_of_your_package')

import module_you_created

module_you_created.fun()
```

第二种方法是指定PYTHONPATH, python会在程序开始时将PYTHONPATH导入sys.path

```
export PYTHONPATH=the_folder_of_your_package

import sys
[p for p in sys.path if 'the_folder_of_your_package' in p]

```

### `__init__.py`

`__init__.py` 文件会在模块被import的时候执行，每个包含这个文件的目录都会被看作是一个package

[packageDemo](https://github.com/xujihui1985/pythonDemo/tree/master/packageDemo)

正如前面说的，python查找模块是从当前目录开始找，然后再从sys.path找，所以要导入`Reader` class, 需要这样导入

```
import packageDemo.reader

my_reader = packageDemo.reader.Reader('./packageDemo/__init__.py')
my_reader.read()

```

这里需要注意的是，假如你只`import packageDemo`, python不像java或者.net会把packageDemo下的模块全部导入，必须显示导入`packageDemo.reader`

假如我希望导入`packageDemo`的时候自动把这个package下的module全部导入，那么需要在`__init__.py`中将module上提

```python
from packageDemo.reader import Reader
```
这样就把Reader提到packageDemo下

```
import packageDemo

my_reader = packageDemo.Reader('xxxx')
...

```

### difference between package and module

简单的说，package 就是目录， module就是文件

### conclusion

1. packages are modules that contain other modules
2. packages are generally implemented as directories containing a special `__init__.py` file
3. the `__init__.py` file is executed when the package is imported
4. packages can contain sub packages which themselves are implemented with `__init__.py` files in directories


### relative import

[relative_import_demo](https://github.com/xujihui1985/pythonDemo/tree/master/relative_import_demo)

通过绝对路径导入，在同一package中，不同的subpackage之间的导入会变得非常的麻烦，这时候可以通过相对路径导入

```
relative_import_demo
|-- __init__.py
|-- sub1
	|-- __init__.py
	|-- common.py
	|-- module1.py
|-- sub2
	|-- __init__.py
	|-- module2.py
```

假如在module1.py中想导入common.py 可以

```python

from .common import arbitory_fun

from . import common
```

假如想在module2中导入module1

```python

from ..sub1 import module1

```

### `__all__`

list of attribute names imported via `from module import *`

默认情况下 `from module import *` 会把所有的属性全部导入

可以通过 `__all__` 来控制那些可以被导入

在`__init__.py` 中

```python

from reader.compressed.bzipped import opener as bz2_opener
from reader.compressed.gzipped import opener as gzip_opener

__all__ = ['bz2_opener', 'gzip_opener']
```

### namespace package

namespace package 类似c#中的namespace, 指的是逻辑目录，也就是说，一个package可以在两个不同的目录中， 它和普通的package不同的是，它没有`__init__.py`文件

1. python scans all entries in sys.path
2. if a matching directory with __init__.py is found, a normal package is loaded
3. if foo.py is found, then it is loaded
4. otherwise, all matching directories in sys.path are considered part of the namespace package.

### executable Directories

[executable_directory demo](https://github.com/xujihui1985/pythonDemo/tree/master/executable_directory)

当一个目录(package)下有 `__main__.py`, 文件， 那么这个目录可以被认为可以执行，也就是这个`__main__.py`会被执行， 并且这个目录会被自动加入到sys.path中,也就是说，这个目录中的所有子目录是能被直接import的

也可以把这个目录打成zip包给Python执行，python可以识别zip包

### recommended layout

```
project_name                    //这层目录是最顶层的目录，它不是package，可以认为是.net中的solution
|-- __main__.py                 //如果这个package可执行，可以在这一层目录下加入 __main__.py, python project_name
|-- project_name                //actual package
	|-- __init__.py
	|-- more_source.py
	|-- subpackage1
		|-- __init__.py
		|--test                 //test package
			|-- __init__.py
			|-- test_code.py
|-- setup.py
```

### modules are singleton

modules are only executed once, when first imported, which mean module is singleton

```python
_registry=[]

def registry(name):
	_registry.extend([name])

def registered_names():
	return iter(_registry)
```