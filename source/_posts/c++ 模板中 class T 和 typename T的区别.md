---
title: c++ 模板中 class T 和 typename T 的区别
date: 2020-04-18 22:50:58
updated: 2020-04-18 22:50:58
tags:
	- C++
	- 程序设计思维实践
categories: 
	- C++
keywords:
	- C++
	- 程序设计思维实践
description: 
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813190801.png
---

# c++ 模板中 `<class T>` 和 `<typename T>`的区别

# 前言

一直感觉`template <class T>`，今天查了一下。

# 总结

`template<class T>`和`template<typename T>`都可以用来定义函数模板和类模板，在使用上，他们俩没有本质的区别。

在模板声明中，`typename` 可用作 `class` 的代替品，以声明类型模板形参和模板形参 (C++17 起)。

在C++早期版本中，没有`typename`这个关键字，所以在模板定义的时候便使用了`class`。

因此现在使用`typename`更加合适。

# `typename`的其它用法

+ 在模板声明中，`typename` 可用作`class `的代替品，以声明类型模板形参和模板形参 (C++17 起)。
+ 在模板的声明或定义内，`typename` 可用于声明某个待决的有限定名是类型。
+ 在模板的声明或定义内， (C++11 前)`typename` 可在非待决的有限定类型名之前使用。此情况下它没有效果。
+ 在类型要求的要求中。(C++20 起)