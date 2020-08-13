---
title: 如何快速理解并记住 C 语言中的 typedef
date: 2020-06-27 16:31:52
updated: 2020-06-27 16:31:52
tags:
	- C++
	- 指针
	- C
categories: 
	- C++
keywords:
	- C++
	- 指针
	- C
description: 
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813184358.png
---
假设有如下代码，你声明了一个**函数指针**`funa`：

```cpp
int *funa (int k);
```

编译器就知道这一行声明了一个**函数指针**，其指向的函数接收一个`int`类型的参数，返回值为`int`。

现在项目经理给编译器作者说，"我要有一个typedef的功能，要能给某个类型起别名。"

编译器作者说："你不早说，我代码都写完了"。

抱怨归抱怨，编译器作者但还是得写，那就用之前的轮子吧

```cpp
typedef int *Funa (int k);
```

项目经理说："这不就何之前一样了吗"

编译器作者说："谁让你不早说，这样我就能直接拿轮子了"

项目经理说："也好，这样也好记住"。

那么：

```cpp
Funa p1;
int *p2 (int k)
```

`p1`和`p2`是等价的。

因为`Funa`和`int * (int k)`是一个类型，`p1`和`p2`是一个类型。

## 参考

[如何理解 C 语言中的 typedef ？ - 霄池的回答 - 知乎]( https://www.zhihu.com/question/19894694/answer/81246243)



