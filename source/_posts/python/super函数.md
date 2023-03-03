---
title: super()函数介绍
date: 2022-02-17 15:00:56
tags: 
 - python-intermediate
categories: 
 - Python
img: /medias/featureimages/29.jpg
---

# super()函数介绍

> 注：本文翻译自[Supercharge Your Classes With Python super()](https://realpython.com/python-super/)。

## 概述

**简单来说，super()函数可以让你在子类中访问父类中的方法。**通过在子类中调用super()函数，可以返回父类的一个对象，从而访问父类中的方法。

什么情况下需要使用super()函数函数呢？

通常情况下，使用super()函数主要是为了在父类的基础上，扩展子类的功能。这样，既避免了在子类中重写父类的代码，也可以减少对父类的代码更改。

<!-- more -->

## 单重继承

如果你对面向对象的编程不是十分了解，那么你可能也不太了解**继承**（ **inheritance**）这个术语。在类中，继承是指一个类可以从另外一个类中派生或者继承其所有的属性和方法，而不用在重新实现。

让我们来举例说明：

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

class Square:
    def __init__(self, length):
        self.length = length

    def area(self):
        return self.length * self.length

    def perimeter(self):
        return 4 * self.length
```

上例中，又两个相似的类：`Rectangle`和`Square`。

你可以像下面这样来使用这两个类：

```python
>>> square = Square(4)
>>> square.area()
16
>>> rectangle = Rectangle(2,4)
>>> rectangle.area()
8
```

在该例中，这两个形状有一种相关性：正方形是一种特殊的长方形。但是，以上代码中并没有体现出这种相关性，因此，代码存在重复的地方。

通过使用继承，根据两种形状的关系，你可以减少代码的编写：

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

# Here we declare that the Square class inherits from the Rectangle class
class Square(Rectangle):
    def __init__(self, length):
        super().__init__(length, length)
```

这里，你使用`super()`函数来调用`Rectangle`类中的初始化函数`__init__()`，这就使得你不用在`Square`类中编写重复的代码。在下面的使用中，`Square`类的核心功能仍然是与`Rectangle`类相同的：

```python
>>> square = Square(4)
>>> square.area()
16
```

在上例中，`Rectangle`是超类， `Square`是子类。

由于`Square`和`Rectangle`类的初始化函数`__init__()`非常相似，所以你可以直接通过`super()`函数调用父类的初始化函数`__init__()`。通过用一个参数`length`来设置父类初始化函数的参数`lenght`和`width`，实现了`Square`的构造函数。

> 笔记：当使用`Square`类时，例如定义`Square`类的实例时。如果`Square`类中没有定义析构函数`__init__()`，则使用父类`Rectangle`类中的析构函数。但是，如果`Square`类中定义了析构函数`__init__()`，则使用Square类自身的析构函数。

当你使用`Square`类的时候，虽然没有为该类显示地编写实现属性和方法的代码，但是当访问`.area()`方法的时候，将会使用父类`Rectangle`的方法。因为`Square`类从父类`Rectangle`继承了`.area()`的方法。

> 注意：如果想学习更多关于Python类和继承的内容，请参考文档：[Inheritance and Composition: A Python OOP Guide](https://realpython.com/inheritance-composition-python/) 和 [Object-Oriented Programming (OOP) in Python 3](https://realpython.com/python3-object-oriented-programming/)。

## super()可以做什么

那么，函数`super()`可以为你在单重继承中做什么呢？

与其它面向对象的语言相同，它可以让你在子类中调用父类的方法。但是，主要的用途是在子类中，进一步扩展继承自父类的方法的功能。

在以下例子中，你将创建一个名为`Cube`的类，该类会从`Square`类继承`.area()`方法，并通过扩展`.area()`的功能来计算`Cube`实例的表面积和容量。

```python
class Square(Rectangle):
    def __init__(self, length):
        super().__init__(length, length)

class Cube(Square):
    def surface_area(self):
        face_area = super().area()
        return face_area * 6

    def volume(self):
        face_area = super().area()
        return face_area * self.length
```

现在你已经创建好了类，下面让我们来计算一下边长为3的Cube的表面积和容量：

```python
>>> cube = Cube(3)
>>> cube.surface_area()
54
>>> cube.volume()
27
```

>注意：在上面的例子中，函数`super()`本身并不会帮你调用方法：你必须通过proxy object来调用方法。

在`Cube`类中，你实现了两个方法：`.surface_area()`和`.volume()`。这两种计算方法都需要计算Cube的单个面积，因此，你可以通过`super()`函数来扩展面积计算的功能。

同时，请注意到在`Cube`类中，并没有定义构造函数`__init__()`。首先，`Cube`继承了`Square`所有的特性和方法，并且，`Cube`需要定义的构造函数`__init__()`与`Square`的没有什么区别，因此，你不需要再为`Cube`类重复定义构造函数`__init__()`，`Cube`类会自动调用父类`Square`的构造函数`__init__()`。

函数`super()`返回了一个父类的代理对象（delegate object），所以你可以直接调用方法：`super().area()`。

这不仅使得我们不需要重写计算面积的代码，并且还可以在一个位置更改`.area()`的内部逻辑。对于一个父类有多个子类的情况，这特别有用。

## 深入理解super()



## 多重继承



### 多重继承简介

### 方法解析顺序（Method Resolution Order）

### 多重继承的其它方法



## 回顾
