---
title: 'Python''s f-Strings:改进版格式化字符串语法指南'
date: 2022-02-07 11:04:29
tags:	
 - python-basic
categories: Python
img: /medias/featureimages/26.jpg
---

# Python's f-Strings:改进版格式化字符串语法指南

> 注：本文翻译自[Python 3's f-Strings: An Improved String Formatting Syntax (Guide) – Real Python](https://realpython.com/python-f-strings/)。

从Python 3.6开始，f-strings是一种全新的格式化字符串方式。f-strings不仅可读性更强，并且简明、快速，与其它格式化字符串的方式相比，不易出错。

通过阅读本文，你将了解为什么要使用f-strings，以及如何使用它。

但是，首先在介绍f-strings之前，我们先回顾一下以前传统的格式化字符串的方式。
<!-- more -->

## 传统的格式化字符串方式

在Python 3.6以前，主要有两种格式化字符串的方式：`%`格式化和`str.format()`。我们先简单回顾一下如何使用这两种方式，以及它们有哪些局限性。

### 方法一：%格式化

%格式化是Python最原始的字符串格式化方式。详情可以参考文档：[Python docs](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting)。根据文档的介绍，并不推荐使用%格式化方式，具体内容如下：

> %格式化字符串的方式会导致程序出现一些错误，例如无法正确显示元组和字典。
>
> 建议使用新的格式化字符串方式避免这些错误。新的方法不仅功能更加强大，且可扩展性更强。

 **如何使用%格式化**

字符串对象本身具有内置的操作符%，通过该操作符可以用来格式化字符串：

```python
>>> name = "Eric"
>>> "Hello, %s." % name
'Hello, Eric.'
```

为了能够在字符串中插入更多的变量，你必须使用元组来存储这些变量：

```python
>>> name = "Eric"
>>> age = 74
>>> "Hello, %s. You are %s." % (name, age)
'Hello Eric. You are 74.'
```

**为什么不推荐使用%格式化**

虽然上面的例子看起来很简单，但是，一旦你需要使用许多的参数，并且字符串很长的时候，代码的可读性就会变得很差：

```python
>>> first_name = "Eric"
>>> last_name = "Idle"
>>> age = 74
>>> profession = "comedian"
>>> affiliation = "Monty Python"
>>> "Hello, %s %s. You are %s. You are a %s. You were a member of %s." % (first_name, last_name, age, profession, affiliation)
'Hello, Eric Idle. You are 74. You are a comedian. You were a member of Monty Python.'
```

以上这种情况，代码就会变得十分冗长，并且非常容易出现错误，例如出现不显示元组和字典的问题。

### 方法二：str.format()

在Python 2.6中，`str.format()`是一种新的格式化字符串的方式。

**如何使用str.format()**

`str.format()`是%格式化的改进版。它使用函数调用的方式将对象格式化成字符串。

使用`str.format()`时，将大括号替换成对应的变量：

```python
>>> "Hello, {}. You are {}.".format(name, age)
'Hello, Eric. You are 74.'
```

你还可以在大括号中使用索引值来引用对应的变量，这种方式使得引用变量与顺序无关：

```python
>>> "Hello, {1}. You are {0}.".format(age, name)
'Hello, Eric. You are 74.'
```

你可以使用关键字参数的方式，在大括号中插入变量名，然后在format()中为参数赋值：

```python
>>> person = {'name': 'Eric', 'age': 74}
>>> "Hello, {name}. You are {age}.".format(name=person['name'], age=person['age'])
'Hello, Eric. You are 74.'
```

你还可以使用`**`的方式将字典作为参数：

```python
>>> person = {'name': 'Eric', 'age': 74}
>>> "Hello, {name}. You are {age}.".format(**person)
'Hello, Eric. You are 74.'
```

**为什么不推荐使用str.format()**

相比于%格式化，`str.format()`的可读性更强，但是在处理多个参数以及长字符串的情况下，代码仍然显得过于冗长：

```python
>>> first_name = "Eric"
>>> last_name = "Idle"
>>> age = 74
>>> profession = "comedian"
>>> affiliation = "Monty Python"
>>> print(("Hello, {first_name} {last_name}. You are {age}. " + 
>>>        "You are a {profession}. You were a member of {affiliation}.") \
>>>        .format(first_name=first_name, last_name=last_name, age=age, \
>>>                profession=profession, affiliation=affiliation))
'Hello, Eric Idle. You are 74. You are a comedian. You were a member of Monty Python.'
```

虽然你可以使用字典的方式传递参数，例如`.format(**some_dict)`，但是其实有更好的方法。

## f-strings: 一种全新的改进版格式化字符串方式

**f-strings**全称为“formatted string literals”，表达式的开头用字母f表示，大括号中包含的表达式会被替换成对应的值。大括号中的表达式通常在运行时被运算出结果，然后用`__format__`协议将对象格式化成字符串。关于f-strings的详细介绍，请参考文档： [Python docs](https://docs.python.org/3/reference/lexical_analysis.html#f-strings)。

### 简单的语法

f-strings的语法与`str.format()`相似，但是比它更简单。

```python
>>> name = "Eric"
>>> age = 74
>>> f"Hello, {name}. You are {age}."
'Hello, Eric. You are 74.'
```

f-strings同时支持大写的F和小写的f:

```python
>>> F"Hello, {name}. You are {age}."
'Hello, Eric. You are 74.'
```

### 支持任意的表达式

因为f-strings在运行时会对表达式进行运算，所以你可以在大括号中使用任何有效的表达式。

```python
>>> f"{2 * 37}"
'74'
```

你也可以调用函数：

```python
>>> def to_lowercase(input):
...     return input.lower()

>>> name = "Eric Idle"
>>> f"{to_lowercase(name)} is funny."
'eric idle is funny.'
```

你还可以直接使用方法：

```python
>>> f"{name.lower()} is funny."
'eric idle is funny.'
```

你在类中也可以使用f-strings调用类的属性：

```python
class Comedian:
    def __init__(self, first_name, last_name, age):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age

    def __str__(self):
        return f"{self.first_name} {self.last_name} is {self.age}."

    def __repr__(self):
        return f"{self.first_name} {self.last_name} is {self.age}. Surprise!"
```

然后，你可以这样：

```python
>>> new_comedian = Comedian("Eric", "Idle", "74")
>>> f"{new_comedian}"
'Eric Idle is 74.'
```

`__str__()`和`__repr__()`主要用于将对象表示为字符串，所以你需要确保在类的定义中，至少使用其中一种方法。如果要在两种方法中选择其一，建议你使用`__repr__()`。

`__str__()`返回的是对象的非正式字符串表达式，可读性比较强，主要面向的是最终用户。

`__repr__()`返回的是对象的正式字符串表达式，并且没有任何歧义，主要面向的是开发人员。

对于访问`__str__()`和`__repr__()`，建议使用`str()`和`repr()`。

默认情况下，f-strings使用`__str__()`，但是你也可以使用转换标识符`!r`来使用`__repr__()`。

```python
>>> f"{new_comedian}"
'Eric Idle is 74.'
>>> f"{new_comedian!r}"
'Eric Idle is 74. Surprise!'
```

### 多行f-Strings

你可以使用多行字符串：

```python
>>> name = "Eric"
>>> profession = "comedian"
>>> affiliation = "Monty Python"
>>> message = (
...     f"Hi {name}. "
...     f"You are a {profession}. "
...     f"You were in {affiliation}."
... )
>>> message
'Hi Eric. You are a comedian. You were in Monty Python.'
```

但是，你需要记住在每一行字符串的开始部分加上`f`。

下面是一个错误的示例：

```python
>>> message = (
...     f"Hi {name}. "
...     "You are a {profession}. "
...     "You were in {affiliation}."
... )
>>> message
'Hi Eric. You are a {profession}. You were in {affiliation}.'
```

以上例子中，由于没有在每一行添加`f`，所以，就导致结果出现异常。

如果你想要将字符串分布到多行，你还可以使用`\`来转义换行符：

```python
>>> message = f"Hi {name}. " \
...           f"You are a {profession}. " \
...           f"You were in {affiliation}."
...
>>> message
'Hi Eric. You are a comedian. You were in Monty Python.'
```

但是，如果使用`"""`，情况又会有所不同：

```python
>>> message = f"""
...     Hi {name}. 
...     You are a {profession}. 
...     You were in {affiliation}.
... """
...
>>> message
'\n    Hi Eric.\n    You are a comedian.\n    You were in Monty Python.\n'
```

### 速度

f-strings的f也有快速的意思。

f-strings的运行速度比%格式化和`stri.format()`都要快。以下是官方的说明：

> “F-strings provide a way to embed expressions inside string literals, using a minimal syntax. It should be noted that an f-string is really an expression evaluated at run time, not a constant value. In Python source code, an f-string is a literal string, prefixed with `f`, which contains expressions inside braces. The expressions are replaced with their values.” ([Source](https://www.python.org/dev/peps/pep-0498/#abstract))

在运行时，大括号中的表达式在它自己的范围（scope）内完成运算，然后再与其它字符串组合成完整的字符串，并返回结果。

以下是几种格式化字符串方法的速度对比：

```python
>>> import timeit
>>> timeit.timeit("""name = "Eric"
... age = 74
... '%s is %s.' % (name, age)""", number = 10000)
0.003324444866599663
```

```python
>>> timeit.timeit("""name = "Eric"
... age = 74
... '{} is {}.'.format(name, age)""", number = 10000)
0.004242089427570761
```

```python
>>> timeit.timeit("""name = "Eric"
... age = 74
... f'{name} is {age}.'""", number = 10000)
0.0024820892040722242
```

从对比结果可以看出，f-strings的运行速度最快。

尽管如此，但是也有一些特例。具体详情，请参考以下内容：

- [performance - Why were literal formatted strings (f-strings) so slow in Python 3.6 alpha? (now fixed in 3.6 stable) - Stack Overflow](https://stackoverflow.com/questions/37365311/why-were-literal-formatted-strings-f-strings-so-slow-in-python-3-6-alpha-now)

- [`BUILD_STRING` opcode](https://bugs.python.org/issue27078)

## f-strings的一些细节

关于f-strings的使用，有一些细节还需要我们了解并且记住。

### 引号

你可以在f-strings中使用任何一种引号，但是必须确保与f-strings最外侧的引号不同。

以下代码可以工作：

```python
>>> f"{'Eric Idle'}"
'Eric Idle'
```

以下代码也可以工作：

```python
>>> f'{"Eric Idle"}'
'Eric Idle'
```

你也可以使用三引号：

```python
>>> f"""Eric Idle"""
'Eric Idle'
```

```python
>>> f'''Eric Idle'''
'Eric Idle'
```

如果你想使用与外侧相同的引号，可以使用转义符号`\`:

```python
>>> f"The \"comedian\" is {name}, aged {age}."
'The "comedian" is Eric Idle, aged 74.'
```

### 字典

当使用字典的时候，我们应该注意。如果你对字典的键使用单引号，那么记住f-strings外侧必须使用双引号。

以下例子可以正常工作：

```python
>>> comedian = {'name': 'Eric Idle', 'age': 74}
>>> f"The comedian is {comedian['name']}, aged {comedian['age']}."
The comedian is Eric Idle, aged 74.
```

但是下面的例子就会报错：

```python
>>> comedian = {'name': 'Eric Idle', 'age': 74}
>>> f'The comedian is {comedian['name']}, aged {comedian['age']}.'
  File "<stdin>", line 1
    f'The comedian is {comedian['name']}, aged {comedian['age']}.'
                                    ^
SyntaxError: invalid syntax
```

如果字典的键使用的引号与f-strings外侧的引号相同，那么，字典的第一个键的引号就会被Python解释器识别为是字符串的结尾。

### 括号

为了在结果的字符串中显示括号，你必须使用双括号：

```python
>>> f"{{70 + 4}}"
'{70 + 4}'
```

注意虽然使用了三个括号，但是结果仍然只会显示单括号：

```python
>>> f"{{{70 + 4}}}"
'{74}'
```

但是，如果使用四个括号，结果就会显示双括号：

```python
>>> name = "Eric Idle"
>>> f"{name}"
'Eric Idle'
```

### 内联注释

大括号中不应该包含用#号的注释语句，否则会报错：

```python
>>> f"Eric is {2 * 37 #Oh my!}."
  File "<stdin>", line 1
    f"Eric is {2 * 37 #Oh my!}."
                                ^
SyntaxError: f-string expression part cannot include '#'
```

## 最后

你仍然可以使用以前的格式化字符串方式，但是f-strings更加简明，可读性强并且方便。想要了解更多关于字符串的内容，请参考以下文档：

- [PEP 502](https://www.python.org/dev/peps/pep-0502/)
- [PEP 536 draft](https://www.python.org/dev/peps/pep-0536/)
- [Python String Formatting Best Practices](https://realpython.com/python-string-formatting/) by Dan Bader
- [Practical Introduction to Web Scraping in Python](https://realpython.com/python-web-scraping-practical-introduction/) by Colin OKeefe
