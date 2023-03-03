---
title: Python模块和包-介绍
date: 2022-02-03 18:30:08
tags: 
 - python-basic
categories: 
 - Python
img: /medias/featureimages/27.jpg
---

> 注：本文翻译自[Python Modules and Packages – An Introduction](https://realpython.com/python-modules-packages/)。

本文主要探讨Python编程语言中的两种便于**模块化编程(Modular programming)**的机制：**模块**和**包**。

**模块化编程**是将大型的、复杂的编程工作，划分成独立、小块且便于管理的子任务或者模块，这些独立的模块可以像积木一样拼凑在一起，用于构建成一个应用。

在开发大型的应用中，模块化编程有以下好处或特性：

- **简易性：**与专注于全局相比，模块通常只需要专注在解决所有编程任务中的某一个局部问题。如果是负责编写一个模块，那么就只要专注于该模块所需要解决的问题，而不用去考虑模块以外的问题。这使得开发任务变得简单且不易出错。
- **可维护性：**通常在设计模块的时候，需要为各个模块之间实施逻辑边界。如果在编写模块的时候，能够尽量减少模块之间的依赖性，就可以减少由于单个模块的改变，导致对其它模块影响的可能性。（你甚至可以在不了解整个应用架构的情况下，对单个模块更改而不影响应用。）这对于团队的所有程序员能够互相协作完成整个大型应用而言，变得至关重要。
- **可重复性：**通过合理地定义模块中的接口，模块中定义的功能可以很方便地被应用中的其它部分重复利用。这减少了冗余的代码。
- **范围：**模块通常定义了一个独立的**命名空间**(**namespace**)，这有助于避免在程序不同部分中包含的标识符冲突。

<!-- more -->

## Python模块概述

在Python中，有三种定义模块的方式：

1. 用Python语言编写的模块
2. 用**C**语言编写的模块，这种模块通常在程序运行时被动态加载，例如re(**regular expression**)模块
3. Python内置的模块，例如**itertools**模块

对于以上三种定义模块的方式，都可以使用**import**声明来访问模块的内容。

在本文中，我们主要侧重于用Python语言编写的模块。比较酷的是，用Python语言编写的模块十分容易编译。只需要创建一个文件，文件名以`.py`后缀命名，然后写入合法的Python代码，该文件就可以作为模块使用了。

举例说明，假设你创建了一个名为mod.py的文件，并且包含以下代码：

***mod.py***

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass
```

在`mod.py`中，主要定义了以下对象：

- `s` (a string)
- `a` (a list)
- `foo()` (a function)
- `Foo` (a class)

假设`mod.py`模块已经被放置在合适的位置，那么模块中包含的对象就可以通过`import`的方式访问模块的内容：

```python
>>> import mod
>>> print(mod.s)
If Comrade Napoleon says it, it must be right.
>>> mod.a
[100, 200, 300]
>>> mod.foo(['quux', 'corge', 'grault'])
arg = ['quux', 'corge', 'grault']
>>> x = mod.Foo()
>>> x
<mod.Foo object at 0x03C181F0>
```



## 模块查找路径

继续引用上面的例子，让我们来看一下，当运行`import`声明时，到底发生了什么：

```python
import mod
```

当Python解释器运行以上的`import`声明时，它会在一系列的目录中查找`mod.py`模块。这些目录的位置主要包含以下来源：

- 如果解释器是通过交互式的方式运行，就会在脚本所在的**当前目录**查找`mod.py`模块。
- 如果设置了Python的环境变量，就会在`PYTHONPATH`环境变量定义的目录中查找模块。
- Python安装过程中配置的与安装相关的目录

模块的查找路径，可以通过`sys`模块获取：

```python
>>> import sys
>>> sys.path
['', 'C:\\Users\\john\\Documents\\Python\\doc', 'C:\\Python36\\Lib\\idlelib',
'C:\\Python36\\python36.zip', 'C:\\Python36\\DLLs', 'C:\\Python36\\lib',
'C:\\Python36', 'C:\\Python36\\lib\\site-packages']
```

> 注意：根据安装的Python版本和操作系统不同，sys.path的内容会略有不同。以上例子中输出的结果可能与你计算机上看到的结果稍有差异。

因此，为了确保编写的模块能够被Python解释器找到，你需要做以下工作：

- 如果是交互式运行Python，需要将`mod.py`放到脚本所在的目录下面
- 打开Python解释器之前，修改`PYTHONPATH`环境变量，以包含`mod.py`模块所在的目录，或者也可以将`mod.py`模块复制到`PYTHONPATH`环境变量已经定义的目录下
- 将`mod.py`模块放到安装相关的目录下，但是根据操作系统的不同，可能对这些目录只有读取的权限，而没有写入的权限。

其实，还有另外一种方法：你可以将模块文件放在任何一个目录下面，然后在运行脚本的时候修改`sys.path`的内容，以包含放置模块的目录。

例如，在下面的例子中，可以将`mod.py`放在目录`C:\Users\john`下面，然后运行以下指令：

```python
>>> sys.path.append(r'C:\Users\john')
>>> sys.path
['', 'C:\\Users\\john\\Documents\\Python\\doc', 'C:\\Python36\\Lib\\idlelib',
'C:\\Python36\\python36.zip', 'C:\\Python36\\DLLs', 'C:\\Python36\\lib',
'C:\\Python36', 'C:\\Python36\\lib\\site-packages', 'C:\\Users\\john']
>>> import mod
```

模块被导入以后，你可以通过模块的属性`__file__`来查看模块文件的路径：

```python
>>> import mod
>>> mod.__file__
'C:\\Users\\john\\mod.py'

>>> import re
>>> re.__file__
'C:\\Python36\\lib\\re.py'
```

`__file__`输出结果的目录部分，应该包含在`sys.path`的目录中。

## import声明

通过`import`声明可以导入模块的内容。`import`声明可以通过几种不同的方式来导入模块，以下一一介绍。

### import \<module_name\>

最简单的导入模块的方式如下：

```python
import <module_name>
```

由于模块有独立的命名空间，也就是说，模块有自己的**私有符号表（private symbol table）**，所以，即使在脚本中导入了模块，仍然无法直接访问模块中的对象。因为，这些对象仍然是被定义在模块的私有符号表中，如果要访问这些模块中的对象，必须要通过**dot notation**的方式来访问。

在通过`import`导入模块以后，`mod`已经在当前脚本的本地符号表中，因此，`mod`在当前脚本中可以直接访问：

```python
>>> import mod
>>> mod
<module 'mod' from 'C:\\Users\\john\\Documents\\Python\\doc\\mod.py'>
```

但是，模块中定义的对象：`s`和`foo`仍然在模块私有符号表中，因此在本地符号表中并没有被定义：

```python
>>> s
NameError: name 's' is not defined
>>> foo('quux')
NameError: name 'foo' is not defined
```

为了能在当前脚本中访问模块中的对象，必须在对象前面加上前缀：**<module_name>.**

```python
>>> mod.s
'If Comrade Napoleon says it, it must be right.'
>>> mod.foo('quux')
arg = quux
```

`import`声明还可以一次导入多个模块，不同模块之间可以用逗号隔开：

```python
import <module_name>[, <module_name> ...]
```

### from \<module_name\> import \<name(s)\>

`import`声明还可以将模块的对象直接导入到当前脚本的本地符号表中：

```python
from <module_name> import <name(s)>
```

根据以上的导入方式，对象可以被直接访问，而不需要添加模块名前缀：

```python
>>> from mod import s, foo
>>> s
'If Comrade Napoleon says it, it must be right.'
>>> foo('quux')
arg = quux

>>> from mod import Foo
>>> x = Foo()
>>> x
<mod.Foo object at 0x02E3AD50>
```

由于`import`直接把对象导入到当前脚本的本地符号表中，当前脚本如果已经存在相同名称的对象，将会被导入的对象所覆盖：

```python
>>> a = ['foo', 'bar', 'baz']
>>> a
['foo', 'bar', 'baz']

>>> from mod import a
>>> a
[100, 200, 300]
```

通过以下方式，还可以将模块中的所有对象一次性导入到当前的脚本：

```python
from <module_name> import *
```

这种方式会将模块中的所有对象导入到本地符号表中，除了名称以下划线(__)开始的对象。

```python
>>> from mod import *
>>> s
'If Comrade Napoleon says it, it must be right.'
>>> a
[100, 200, 300]
>>> foo
<function foo at 0x03B449C0>
>>> Foo
<class 'mod.Foo'>
```

在大规模的代码开发场景中，并不推荐这种一次性导入所有对象的方式。因为，这种方式很容易导致冲突，会将已经定义的对象覆盖，应尽量避免。

但是，如果在交互式解释器中运行，这种方式就非常方便，简单的输入，就能访问模块中的所有内容。

### from \<module_name\> import \<name\> as \<alt_name\>

如果导入的对象名称与本地对象名称冲突，可以用别名来代替模块中的对象名称：

```python
from <module_name> import <name> as <alt_name>[, <name> as <alt_name> …]
```

这样，就可以既避免冲突，也可以直接将模块对象导入到本地符号表中：

```python
>>> s = 'foo'
>>> a = ['foo', 'bar', 'baz']

>>> from mod import s as string, a as alist
>>> s
'foo'
>>> string
'If Comrade Napoleon says it, it must be right.'
>>> a
['foo', 'bar', 'baz']
>>> alist
[100, 200, 300]
```

###  import \<module_name\> as \<alt_name\>

你也可以在导入模块的时候，指定用别名来代替模块的实际名称：

```python
import <module_name> as <alt_name>
```

```python
>>> import mod as my_module
>>> my_module.a
[100, 200, 300]
>>> my_module.foo('qux')
arg = qux
```

另外，在函数的定义中，也可以导入模块的内容。但是，只有当函数被调用的时候，模块才会被导入。

```python
>>> def bar():
...     from mod import foo
...     foo('corge')
...

>>> bar()
arg = corge
```

需要注意的是，在Python 3中，不支持用`import *`的语法在函数中定义导入模块的声明：

```python
>>> def bar():
...     from mod import *
...
SyntaxError: import * only allowed at module level
```

最后，可以使用` try statement with an except ImportError`语句来处理导入模块失败的情况：

```python
>>> try:
...     # Non-existent module
...     import baz
... except ImportError:
...     print('Module not found')
...

Module not found
```

```python
>>> try:
...     # Existing module, but non-existent object
...     from mod import baz
... except ImportError:
...     print('Object not found in module')
...

Object not found in module
```



## 函数dir()

在一个命名空间中，内置的`dir()`函数会返回一个列表，列表中包含的元素是已定义的名称。默认情况下，生成的列表成员按字母排序。

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> qux = [1, 2, 3, 4, 5]
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'qux']

>>> class Bar():
...     pass
...
>>> x = Bar()
>>> dir()
['Bar', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'qux', 'x']
```

从以上例子中可以注意到，第一次调用函数`dir()`后，在命名空间中已经存在预先定义好的名称。等到新的名称定义（qux, Bar, x）以后，就会出现在虽有调用函数`dir()`输出的结果中。

函数`dir()`有助于我们识别哪些对象通过`import`的方式导入到了当前的命名空间中：

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> import mod
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'mod']
>>> mod.s
'If Comrade Napoleon says it, it must be right.'
>>> mod.foo([1, 2, 3])
arg = [1, 2, 3]

>>> from mod import a, Foo
>>> dir()
['Foo', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'a', 'mod']
>>> a
[100, 200, 300]
>>> x = Foo()
>>> x
<mod.Foo object at 0x002EAD50>

>>> from mod import s as string
>>> dir()
['Foo', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'a', 'mod', 'string', 'x']
>>> string
'If Comrade Napoleon says it, it must be right.'
```

当我们调用函数`dir()`的时候，将模块的名称作为参数，函数`dir()`就会列出模块中定义的所有名称：

```python
>>> import mod
>>> dir(mod)
['Foo', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__',
'__name__', '__package__', '__spec__', 'a', 'foo', 's']
```

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']
>>> from mod import *
>>> dir()
['Foo', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'a', 'foo', 's']
```

## 用脚本的方式运行模块

在Python中，任何模块文件同时也是一个脚本，因此，可以直接当成脚本来运行。

下面还是以`mod.py`脚本为例：

***mod.py***

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass
```

该模块如果以脚本来运行：

```Windows Command Prompt
C:\Users\john\Documents>python mod.py
C:\Users\john\Documents>
```

从以上结果来看，没有出现任何错误。因此，说明该脚本运行成功，但是由于只定义了对象，并没有输出任何结果。

下面修改一下脚本，让它输出一些内容：

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass

print(s)
print(a)
foo('quux')
x = Foo()
print(x)
```

现在，我们可以看到以下输出结果：

```Windows Command Prompt
C:\Users\john\Documents>python mod.py
If Comrade Napoleon says it, it must be right.
[100, 200, 300]
arg = quux
<__main__.Foo object at 0x02F101D0>
```

如果将其作为模块导入，我们会发现导入模块的过程中，也会输出结果：

```python
>>> import mod
If Comrade Napoleon says it, it must be right.
[100, 200, 300]
arg = quux
<mod.Foo object at 0x0169AD50>
```

通常情况下，我们都希望导入模块的时候，不生成任何结果。只有当模块作为脚本运行的时候，才输出结果。

当模块导入的时候，Python会将变量`__name__`设置为模块的名称，但是当作为脚本运行时，`__name__`会设置为字符串`__main__`。根据该特性，我们可以区分是导入模块还是运行脚本：

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass

if (__name__ == '__main__'):
    print('Executing as standalone script')
    print(s)
    print(a)
    foo('quux')
    x = Foo()
    print(x)
```

现在，如果运行脚本，会得到以下结果：

```Windows Command Prompt
C:\Users\john\Documents>python mod.py
Executing as standalone script
If Comrade Napoleon says it, it must be right.
[100, 200, 300]
arg = quux
<__main__.Foo object at 0x03450690>
```

但是，如果当模块导入，就不会有任何结果输出：

```python
>>> import mod
>>> mod.foo('grault')
arg = grault
```

通常在设计模块的时候，为了对模块做单元测试，也需要将模块当作脚本来直接运行。例如，假设创建了一个`fact.py`的模块，该模块包含factorial函数：

```python
def fact(n):
    return 1 if n == 1 else n * fact(n-1)

if (__name__ == '__main__'):
    import sys
    if len(sys.argv) > 1:
        print(fact(int(sys.argv[1])))
```

将该模块中的`fact()`函数导入：

```python
>>> from fact import fact
>>> fact(6)
720
```

另外，为了测试，也可以将该模块单独运行，在命令行中输入整型参数：

```Windows Command Prompt
C:\Users\john\Documents>python fact.py 6
720
```



## 重新加载模块

出于效率的原因，在每一个解释器会话中，模块只会被加载一次。对于函数和类的定义来说，这没什么问题。但是，通常是为了初始化的缘故，模块还可能包含一些可执行的声明。需要注意的是，在当前的解释器会话中，当模块被导入后，这些声明只会被执行一次。即使再次导入模块，这些声明也不会被再次执行。

下面以`mod.py`:

***mod.py***

```python
a = [100, 200, 300]
print('a =', a)
```

```python
>>> import mod
a = [100, 200, 300]
>>> import mod
>>> import mod

>>> mod.a
[100, 200, 300]
```

以上例子中，在后续两次import过程中，`print()`声明都没有别执行。其实，声明`a = [100, 200, 300]`也没有再次执行，但是最后`mod.a`的结果显示，一旦赋值声明（assignment statement）执行后，就会将结果保留。

如果更改了模块的内容，并且需要重新加载模块，可以重启解释器，或者使用模块`importlib`中的函数`reload()`:

```python
>>> import mod
a = [100, 200, 300]

>>> import mod

>>> import importlib
>>> importlib.reload(mod)
a = [100, 200, 300]
<module 'mod' from 'C:\\Users\\john\\Documents\\Python\\doc\\mod.py'>
```



## Python 包

假设你开发了一个大型的应用，该应用包含许多模块。随着模块数量的增加，尤其是当这些模块有相似的名称和功能的时候，管理这些模块变得越来越困难。你可能会希望有一个更换的方法来分类和组织这些模块。

**包**通过使用点表示法（**dot notation**），可以提供一种层次性结构的模块命名空间。通过这种方式，**模块**可以使得全局变量名之间避免冲突，**包**可以使得模块名之间避免冲突。

创建一个**包**非常简单，只需要创建一个目录，并将模块放到该目录下面。该目录就是一个Python**包**。

可以参考一下下面的例子：

![](pkg1.9af1c7aea48f.png)

以上例子中，目录名为`pkg`，该目录包含两个模块：`mod1.py`和`mod2.py`。模块的内容如下：

***mod1.py***

```python
def foo():
    print('[mod1] foo()')

class Foo:
    pass
```

***mod2.py***

```python
def bar():
    print('[mod2] bar()')

class Bar:
    pass
```

根据这种结构，我们可以继续使用之前导入模块的方法，使用点表示法（**dot notation**）来导入包里面的模块：

```python
import <module_name>[, <module_name> ...]
```

```python
>>> import pkg.mod1, pkg.mod2
>>> pkg.mod1.foo()
[mod1] foo()
>>> x = pkg.mod2.Bar()
>>> x
<pkg.mod2.Bar object at 0x033F7290>
```

```python
from <module_name> import <name(s)>
```

```python
>>> from pkg.mod1 import foo
>>> foo()
[mod1] foo()
```

```python
from <module_name> import <name> as <alt_name>
```

```python
>>> from pkg.mod2 import Bar as Qux
>>> x = Qux()
>>> x
<pkg.mod2.Bar object at 0x036DFFD0>
```

另外，你也可以使用以下方式导入模块：

```python
from <package_name> import <modules_name>[, <module_name> ...]
from <package_name> import <module_name> as <alt_name>
```

```python
>>> from pkg import mod1
>>> mod1.foo()
[mod1] foo()

>>> from pkg import mod2 as quux
>>> quux.bar()
[mod2] bar()
```

其实，从技术上来讲，你也可以直接导入包：

```python
>>> import pkg
>>> pkg
<module 'pkg' (namespace)>
```

但是，其实这没有什么用。虽然用这种方式导入了包，但实际上模块并没有被导入到本地命名空间。所以，在调用模块时就会报错。

```python
>>> pkg.mod1
Traceback (most recent call last):
  File "<pyshell#34>", line 1, in <module>
    pkg.mod1
AttributeError: module 'pkg' has no attribute 'mod1'
>>> pkg.mod1.foo()
Traceback (most recent call last):
  File "<pyshell#35>", line 1, in <module>
    pkg.mod1.foo()
AttributeError: module 'pkg' has no attribute 'mod1'
>>> pkg.mod2.Bar()
Traceback (most recent call last):
  File "<pyshell#36>", line 1, in <module>
    pkg.mod2.Bar()
AttributeError: module 'pkg' has no attribute 'mod2'
```

因此，你需要使用之前的方法导入模块或者模块所包含的对象。



## 包初始化

如果在包的目录下有文件名为`__init__.py`的文件，当包或者模块被导入的时候，该文件就会被调用执行。文件中包含的代码主要用来对包进行初始化。

下面，可以参考一下以下`__init__.py`文件：

```python
print(f'Invoking __init__.py for {__name__}')
A = ['quux', 'corge', 'grault']
```

将文件`__init__.py`放到`pkg`目录下方：

![](pkg1_133918.png)

当包被导入的时候，全局列表A会被初始化：

```python
>>> import pkg
Invoking __init__.py for pkg
>>> pkg.A
['quux', 'corge', 'grault']
```

在包中的模块也可以访问全局列表A：

***mode1.py***

```python
def foo():
    from pkg import A
    print('[mod1] foo() / A = ', A)

class Foo:
    pass
```

```python
>>> from pkg import mod1
Invoking __init__.py for pkg
>>> mod1.foo()
[mod1] foo() / A =  ['quux', 'corge', 'grault']
```

文件`__init__.py`还可以用来导入包中的模块。例如，之前如果只导入包`import pkg`，实际上模块并没有被导入。但是，如果在文件`__init__.py`中添加以下语句：

```python
print(f'Invoking __init__.py for {__name__}')
import pkg.mod1, pkg.mod2
```

当运行`import pkg`的时候，模块mod1和mod2就会自动被导入：

```python
>>> import pkg
Invoking __init__.py for pkg
>>> pkg.mod1.foo()
[mod1] foo()
>>> pkg.mod2.bar()
[mod2] bar()
```

>
>
>注意：大部分的Pyton文档中都声明，如果要创建包，就必须在包目录下创建文件`__init__.py`，即使该文件不包含任何内容，也必须出现在包所在目录下。
>
>但是，从Python 3.3开始， [Implicit Namespace Packages](https://www.python.org/dev/peps/pep-0420)允许即使没有文件`__init__.py`文件，仍然可以创建包。当然，如果要初始化包，仍然可以使用`__init__.py`文件。



## Importing * From a Package

为了便于接下来的讨论，在之前定义的包中增加一些额外的模块：

![](pkg3.d2160908ae77.png)

现在，包里面有4个模块，具体分别如下：

***mod1.py***

```python
def foo():
    print('[mod1] foo()')

class Foo:
    pass
```

***mod2.py***

```python
def bar():
    print('[mod2] bar()')

class Bar:
    pass
```

***mod3.py***

```python
def baz():
    print('[mod3] baz()')

class Baz:
    pass
```

***mod4.py***

```python
def qux():
    print('[mod4] qux()')

class Qux:
    pass
```

根据之前的例子，如果用`from <module name > import *`的方式导入模块，除了以下划线命名的对象以外，可以将模块包含的所有对象导入到本地符号表：

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> from pkg.mod3 import *

>>> dir()
['Baz', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'baz']
>>> baz()
[mod3] baz()
>>> Baz
<class 'pkg.mod3.Baz'>
```

同样，我们也可以用相似的语法导入包：

```python
from <package_name> import *
```

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> from pkg import *
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']
```

但是，与导入模块不同，用`from <package_name> import *`导入包后，结果却显示没有导入任何内容。

其实，Python遵循了一种规范：如果`__init__.py`的代码中有一个名为`__all__`的列表，该列表中定义的模块名，在用`from <package_name> import *`导入时，就会被自动导入。

在当前的例子中，假设你在`pkg`包中创建了一个`__init__.py`的文件，内容如下：

***pkg/__init__.py***

```python
__all__ = [
        'mod1',
        'mod2',
        'mod3',
        'mod4'
        ]
```

现在用`from pkg import *`导入包后，就可以同时导入4个模块：

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> from pkg import *
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'mod1', 'mod2', 'mod3', 'mod4']
>>> mod2.bar()
[mod2] bar()
>>> mod4.Qux
<class 'pkg.mod4.Qux'>
```

通过`__all__`这种方式，我们就可以控制哪些模块可以被`import *`这种方式自动导入。

其实，`__all__`也可以用相同的方式作用于模块：控制模块中的哪些对象可以被`import *`的方式导入。

下面以`mod1.py`为例：

***pkg/mod1.py***

```python
__all__ = ['foo']

def foo():
    print('[mod1] foo()')

class Foo:
    pass
```

根据以上模块中`__all__`的定义，通过`import *`只会导入`foo`对象：

```python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> from pkg.mod1 import *
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'foo']

>>> foo()
[mod1] foo()
>>> Foo
Traceback (most recent call last):
  File "<pyshell#37>", line 1, in <module>
    Foo
NameError: name 'Foo' is not defined
```

`foo()`被导入到本地命名空间，但是类`Foo`并没有被导入，因为`Foo`并没有被定义在`__all__`中。

总结一下，当使用`import *`语句的时候，`__all__`可以用来定义**包**和**模块**导入的内容。但是，具体的使用上有一些区别：

- 对包而言，当`__all__`没有定义的时候，`import *`不会导入任何内容。
- 对模块而言，当`__all__`没有定义的时候，`import *`会导入模块中的所有对象（除了以下划线命名的对象）。

## 子包（Subpackages）

包还可以包含子包，对于子包的层级也没有任何限制。例如，对之前的例子做一些修改：

![](pkg4_100818.png)

之前定义的四个模块：`mod1.py`, `mod2.py`, `mod3.py`, `mod4.py`，分别被放到两个子包下面：`sub_pkg1`和`sub_pkg2`。

仍然和之前一样，可以用点表达式的方式导入子包下面的模块：

```python
>>> import pkg.sub_pkg1.mod1
>>> pkg.sub_pkg1.mod1.foo()
[mod1] foo()

>>> from pkg.sub_pkg1 import mod2
>>> mod2.bar()
[mod2] bar()

>>> from pkg.sub_pkg2.mod3 import baz
>>> baz()
[mod3] baz()

>>> from pkg.sub_pkg2.mod4 import qux as grault
>>> grault()
[mod4] qux()
```

另外，在子包中的模块，还可以引用同级子包中的模块。

例如，假设你想要在模块`mod3`中导入并允许函数模块`mod1`中的函数`foo()`，你可以使用**绝对导入**：

***pkg/sub__pkg2/mod3.py***

```python
def baz():
    print('[mod3] baz()')

class Baz:
    pass

from pkg.sub_pkg1.mod1 import foo
foo()
```

```python
>>> from pkg.sub_pkg2 import mod3
[mod1] foo()
>>> mod3.foo()
[mod1] foo()
```

或者，你还可以使用**相对导入**，在相对导入中，`..`代表的是上一级包。

- `..`代表的是上级包`pkg`
- `..sub_pkg1`代表的是子包`sub_pkg1`的上一级包

***pkg/sub__pkg2/mod3.py***

```python
def baz():
    print('[mod3] baz()')

class Baz:
    pass

from .. import sub_pkg1
print(sub_pkg1)

from ..sub_pkg1.mod1 import foo
foo()
```

```python
>>> from pkg.sub_pkg2 import mod3
<module 'pkg.sub_pkg1' (namespace)>
[mod1] foo()
```

## 总结

在本指南中，你可以学到以下内容：

- 如何创建Python模块
- Python解释器如何查找模块
- 通过`import`语句，如何访问模块中定义的对象
- 如何将模块作为独立脚本来运行
- 如何用包和子包的方式组织模块
- 如何初始化包

关于更多模块和包的内容，可以参考**Python.org**网站的文档：

- [The `import` system](https://docs.python.org/3/reference/import.html)
- [The Python tutorial: Modules](https://docs.python.org/3/tutorial/modules.html)
