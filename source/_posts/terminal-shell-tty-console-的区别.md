---
title: terminal shell tty console 的区别
date: 2020-09-25 09:25:08
updated: 2020-09-25 09:25:08
tags:
    - 操作系统
categories:
    - 操作系统
keywords:
    - 操作系统
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402164608.png
---

A terminal is at the end of an electric wire, a shell is the home of a turtle, tty is a strange abbreviation and a console is a kind of cabinet.

Well, etymologically speaking, anyway.

In unix terminology, the short answer is that

- terminal = tty = text input/output environment
- console = physical terminal
- shell = command line interpreter

------

Console ，terminal 和tty密切相关。 最初，它们是指一种设备，您可以通过它与计算机进行交互：在unix的早期，这意味着类似于打字机的电传打字机式设备，有时也称为电传打字机，或简称为“ tty”。 从电子的角度来看，“终端”这个名字，从家具的角度来看，这个名字是“Console”。 在Unix历史的早期，电子键盘和显示器成为终端的规范。

用Unix术语来说，tty是一种特殊的设备文件，它实现了除读写之外的许多其他命令（ioctl）。 在其最常见的含义中，terminal是tty的同义词。 某些tty由内核代表硬件设备提供，例如，输入来自键盘，输出进入文本模式屏幕，或者输入和输出通过串行线路传输。 其他tty，有时也称为伪tty，由称为终端仿真器的程序（通过薄内核层）提供，例如Xterm（在X Window系统中运行），Screen（在程序和另一个终端之间提供隔离层） ），Ssh（将一台计算机上的终端与另一台计算机上的程序连接），Expect（用于脚本化终端交互）。

终端一词还可以具有一种设备的更传统的含义，通过该设备，人们可以与计算机（通常是键盘和显示器）进行交互。 例如，X终端是一种瘦客户机，是一台专用计算机，其唯一用途是驱动键盘，显示器，鼠标以及偶尔其他人机交互外围设备，而实际应用程序则在另一台功能更强大的计算机上运行。

Console通常是物理意义上的终端，根据某种定义，它是直接连接到机器的主要终端。 Console在操作系统中显示为（内核实现的）tty。 在某些系统上，例如Linux和FreeBSD，控制台显示为多个tty（特殊的组合键在这些tty之间切换）。 只是为了混淆，给每个特定tty赋予的名称可以是“Console”，”virtual console”，”virtual terminal”，和其他变体。

------

Shell是用户登录时看到的主要界面，其主要目的是启动其他程序。 （我不知道最初的隐喻是shell是用户的家庭环境，还是该shell是其他程序在其中运行。）

在Unix圈子中，shell专门指命令行外壳，以输入要启动的应用程序的名称为中心，然后输入应用程序应作用的文件或其他对象的名称，然后按Enter键。 其他类型的环境不使用“外壳”一词； 例如，窗口系统涉及“窗口管理器”和“桌面环境”，而不涉及“外壳”。

有许多不同的unix shell。 交互式使用的流行shell包括Bash（大多数Linux安装中的默认设置），zsh（强调功能和可定制性）和fish（强调简单性）。

命令行外壳包含用于组合命令的流控制构造。 除了在交互式提示下键入命令外，用户还可以编写脚本。 最常见的Shell具有基于Bourne_shell的通用语法。 在讨论“ shell编程”时，几乎总是暗示该外壳是Bourne风格的外壳。 一些经常用于脚本编写但缺少高级交互功能的外壳包括Korn外壳（ksh）和许多ash变体。 几乎所有类似Unix的系统都有Bourne风格的shell安装为/ bin / sh，通常是ash，ksh或bash。

在unix系统管理中，用户的外壳程序是他们登录时调用的程序。普通用户帐户具有命令行外壳程序，但是访问受限的用户可能具有受限的外壳程序或某些其他特定命令（例如，用于文件传输） -仅帐户）。

------

The division of labor between the terminal and the shell is not completely obvious. Here are their main tasks.

- Input: the terminal converts keys into control sequences (e.g. Left → `\e[D`). The shell converts control sequences into commands (e.g. `\e[D` → `backward-char`).
- Line editing, input history and completion are provided by the shell.
  - The terminal may provide its own line editing, history and completion instead, and only send a line to the shell when it's ready to be executed. The only common terminal that operates in this way is `M-x shell` in Emacs.
- Output: the shell emits instructions such as “display `foo`”, “switch the foreground color to green”, “move the cursor to the next line”, etc. The terminal acts on these instructions.
- The prompt is purely a shell concept.
- The shell never sees the output of the commands it runs (unless redirected). Output history (scrollback) is purely a terminal concept.
- Inter-application copy-paste is provided by the terminal (usually with the mouse or key sequences such as Ctrl+Shift+V or Shift+Insert). The shell may have its own internal copy-paste mechanism as well (e.g. Meta+W and Ctrl+Y).
- [Job control](http://en.wikipedia.org/wiki/Job_control) (launching programs in the background and managing them) is mostly performed by the shell. However, it's the terminal that handles key combinations like Ctrl+C to kill the foreground job and Ctrl+Z to suspend it.

# 翻译自：

[https://unix.stackexchange.com/a/4132](https://unix.stackexchange.com/a/4132)
