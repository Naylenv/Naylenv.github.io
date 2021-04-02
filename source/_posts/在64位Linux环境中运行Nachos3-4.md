---
title: 在64位Linux环境中运行Nachos3.4
tags:
  - 操作系统
categories:
  - 操作系统
keywords:
  - 操作系统
date: 2021-03-31 01:31:09
updated: 2021-03-31 01:31:09
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402171026.png
---

# 操作过程

操作过程如下（以64位Ubuntu为例）:

##  检查系统是否支持多架构

### (1) 确认主机系统为64位架构的内核

在命令终端中运行 `dpkg--print-architecture`,应该输出amd64

### (2) 确认系统多架构功能已经打开，以支持1386

在命令终端中运行 `dpkg--print-foreign-architectures`,应该输出i386

### 如果(2)中检测到多架构功能尚未打开，则使用下述命令安装

```shell
sudo dpkg--add-architecture i386
```

## 安装32位编译环境与支持库

### (1) 检查gcc与g++是否已经安装，如果尚未安装，使用如下命令安装它们。

```shell
sudo apt install gcc g++
```

(2)安装32位编译环境与支持库

```shell
sudo apt install build-essential g++-multilib gcc-multilib
```

## 修改Nachos

### (1) 修改code目录下的Makefile.dep文件

在C++的编译器CC与链接器LD后追加`-m32`,在汇编编译器AS后追加`--32`,修改后的内容如下所示：

```makefile
ifndef MAKEFILE_DEP
define MAKEFILE_DEP
yes
endef

# These definitions may change as the software is updated.
# Some of them are also system dependent
CPP=/lib/cpp
CC = g++ -m32
LD = g++ -m32
AS = as --32

uname = $(shell uname)
$(warning $(uname))
```

### (2) 修改code/bin/coff.h

```cpp
/* coff.h
 *   Data structures that describe the MIPS COFF format.
 */

#ifdef HOST_ALPHA		/* Needed because of gcc uses 64 bit long  */
#define _long int		/* integers on the DEC ALPHA architecture. */
#else
#define _long int      // 修改为 #define _long int
#endif
```

文件`code/bin/coff.h`中的几条语句如下所示：
在32位环境中，宏命令`#define _longlong`将`_long`定义为`long`,系统为`long`类型数据分配4字节存储空间，因此结构`filehdr`的大小为20字节，而在64位系统中，由于数据类型`long`需要8字节存储空间，导致`filehdr`占用了40字节。而Nachos中要求`filehdr`大小必须为20字节，因此需要将`code/bin/coff.h`中的`long`改成`int`,即将`#define _long long`修改为`#define _long int`。



# 64位下的一些问题

由于使用的是64位操作系统。编译器版本要高于gcc3.4。会出现一些c++版本的问题

## 问题1：warning: ISO C++ forbids converting a string constant to ‘char*’ [-Wwrite-strings]



![img](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20210331010106123.png)

这是由于c++规范中认为`"aaa"`是string类型，，c++03前`string` to `char const *`会发生隐式转换。从c++11开始，该规则删除。

对类似字符串做如下更改：

![img](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20210331010305677.png)

参考链接：[Why is conversion from string constant to 'char*' valid in C but invalid in C++](https://stackoverflow.com/questions/20944784/why-is-conversion-from-string-constant-to-char-valid-in-c-but-invalid-in-c)

## 问题2：warning: deleting ‘void*’ is undefined [-Wdelete-incomplete]

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20210331010427207.png)

这是由于在现代c++中，删除void *指针是一种错误格式(也就是我们通常所说的“编译错误”)。在c++ 98中情况就不同了。删除void *类型的空指针是NOP，删除void *类型的非空指针是UB。

解决方法：注释掉

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20210331011346542.png)

参考链接：[Why deleting void* is UB rather than compilation error?](https://stackoverflow.com/questions/50843120/why-deleting-void-is-ub-rather-than-compilation-error)

## 问题3：cc1plus: note: obsolete option '-I-' used, please use '-iquote' instead

![img](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20210331010654999.png)

`-iquote`是在GCC 4.0.0 中添加的。GCC开发人员在90年代中期错误的取消了`-I-`。但`-iquote`并不能完全代替`-I-`（没有很好的解决方法）。

参考链接：[cc1: note: obsolete option -I- used, please use -iquote instead #20](https://github.com/att/ast/issues/20)

## 问题4： warning: multi-line comment [-Wcomment]

![image-20210331103552047](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20210331103552047.png)

在C/C++语言中，
在对源文件做预处理的时候，有两条基本原则：

1. 凡是以//开头的为单行注释

2. 凡是以\结尾的代表此行尚未结束

于是预处理器在处理的时候会先按第二条规则，看每行的末尾的那个字符是不是”\”,是的话，就下一行接到本行。然后把所有以//开头的注释和/* */的块注释去掉。

但是存在一个问题，对于big5中的汉字而言，其第一个字节的编码范围是0xA1 - 0xFE，第二个字节是0×40 -0xFE。而’\'的ASCII码是0×5c.这就意味这，凡是以big5编码的文件，如果gcc没有正确的认为它源文件的编码是big5,那么就可能出现因为单行注释末尾是汉字，而把下行的代码吃掉的情况。这样是很危险的，但是gcc会给出一个警告：”warning: multi-linecomment In file”

解决方法，改用：

```cpp
/*
*/
```

参考链接：

[c语言中的注释，multi-line comment](https://blog.csdn.net/lyd0813/article/details/83023918)

[What is the meaning of multi-line comment warnings in C?](https://stackoverflow.com/questions/19105350/what-is-the-meaning-of-multi-line-comment-warnings-in-c)


