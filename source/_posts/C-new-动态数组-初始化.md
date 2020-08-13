---
title: C++ - new - 动态数组 - 初始化
date: 2020-03-01 21:18:08
updated: 2020-03-01 21:18:08
tags:
    - C++
categories:
    - C++
keywords:
    - C++
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813220152.png
---

# 数组的动态初始化

在代码的时候看到了new初始化的一些问题，查阅了相关文档总结如下。可直接阅读结论部分，文档内内容在下面。

## 结论：

```cpp
int *p = new int[10]; // 每个元素都没有初始化
int *p = new int[10] (); // 每个元素初始化为0
int *p = new int(7); // 元素初始化为8
int *p = new int(); // 元素初始化为0
int *p = new int; // 元素没有初始化
string *p = new string[10]; // 每个元素调用默认构造函数初始化
string *p = new string[10](); // 每个元素调用默认构造函数初始化
```
为什么没有`int *p = new int[10](7)`，因为标准不支持，可见规则2。

**动态数组初始化：**

1. 动态数组只能初始化为元素类型的默认值，而不能像数组变量一样，用初始化列表为数组元素提供各不相同的初值。

2. 对于内置数据类型元素的数组，必须使用()来显示指定程序执行初始化操作，否则程序不执行初始化操作：

3. 类类型元素的数组，则无论是否使用（），都会自动调用其默认构造函数来初始化：

## 规则

new 表达式所创建的对象按照下列规则初始化：

1. 对于非数组的类型，在所得内存区域中构造单个对象。
   + 若无初始化器，则对象被默认初始化。
   + 若初始化器是带括号的实参列表，则对象被直接初始化。
   + 若 初始化器 是花括号包围的实参列表，则对象被列表初始化。(C++11 起)
2. 若 类型 是数组类型，则初始化一个数组的对象。
   + 若无初始化器，则每个元素被默认初始化。
   + 若初始化器是一对空括号，则每个元素被值初始化。
   + 若初始化器是花括号包围的实参列表，则数组被聚合初始化。(C++11 起)
   + 若初始化器是带括号的实参列表，则数组被聚合初始化。

### 默认初始化

- 若 `T` 是`no- POD`类类型（`int`, `char`, `wchar_t`, `bool`, `float`, `double` 是POD类型 ）。调用所选的构造函数，以提供新对象的初始值；
- 若 `T` 是数组类型，则每个数组元素都被默认初始化；
- 否则，不做任何事：具有自动存储期的对象（及其子对象）被初始化为不确定值。

### 直接初始化

可以理解为复制初始化。区别是：直接初始化考虑所有构造函数和所有用户定义转换函数。

### 值初始化

**`C++11`前：**

1) 若 `T` 是有至少一个用户提供的任意种类的构造函数的类类型，则调用默认构造函数；

2) 若 `T` 是没有任何用户提供的构造函数的非联合体类类型，则值初始化 T 的每个非静态数据成员与基类组分；

**`C++11`后：**

1) 若 `T` 是没有默认构造函数，或拥有用户提供的或被删除的默认构造函数的类类型，则默认初始化对象；

2) 若 `T` 是拥有默认构造函数的类类型，而默认构造函数既非用户提供亦未被删除（即它可以是拥有隐式定义的或默认化的默认构造函数的类），则零初始化对象，然后若其拥有非平凡的默认构造函数，则默认初始化它；

------

3) 若 `T` 是数组类型，则值初始化数组的每个元素；

4) 否则，零初始化对象。

#### POD
摘自某百科：

A PDS type in C++, or Plain Old C++ Object, is defined as either a scalar type or a PDS class. A PDS class has no user-defined copy assignment operator, no user-defined destructor, and no non-static data members that are not themselves PDS. Moreover, a PDS class must be an aggregate, meaning it has no user-declared constructors, no private nor protected non-static data, no virtual base classes and no virtual functions. The standard includes statements about how PDS must behave in C++. The type_traits library in the C++ Standard Library provides a template named is_pod that can be used to determine whether a given type is a POD. In C++20 the notion of “plain old data” (POD) and by that is_pod is deprecated and replaced with the concept of “trivial” and “standard-layout” types.

In some contexts, C++ allows only PDS types to be used. For example, a union in C++98 cannot contain a class that has virtual functions or nontrivial constructors or destructors. This restriction is imposed because the compiler cannot determine which constructor or destructor should be called for a union. PDS types can also be used for interfacing with C, which supports only PDS.
