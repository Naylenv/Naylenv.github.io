---
title: .git 文件过大 - 减小 .git 文件体积
date: 2020-03-20 10:35:01
updated: 2020-03-20 10:35:01
tags:
	- Git
categories: 
	- Git
keywords:
	- Git
description: 
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813191311.png
---

# .git 文件过大

# 综述

最近发现`github`上项目`.git`文件已经达到了2个G，整理了减小`.git`文件的方法。

## .git文件是什么

一个管理git仓库的文件夹，这里包含所有git操作所需要的东西

# 方法

## 简单有效，减小体积

运行 `gc` ，生成 `pack` 文件（后面的 `--prune=now` 表示对之前的所有提交做修剪，有的时候仅仅 `gc` 一下`.git` 文件就会小很多）

```shell
git gc --prune=now
```

## 克隆时只克隆一层

```shell
git clone --depth=1
```

## 使用`git-lfs`管理文件

项目中有大量的图片文件，音频文件，**二进制**文件时，推荐使用第三方扩展插件`git-lfs`。

它将你所标记的大文件保存至另外的仓库,而在主仓库仅保留其轻量级指针

### why?

二进制内容比较难压缩, 会导致整个仓库占用的空间飞速增长. 没多久你可能就会发现，10M的文件，100M的`.git`文件。也就是不能版本比较。

### Getting Started

安装完成后在`git bash`中运行如下指令

```shell
git lfs install
```

### 添加你要管理的文件

```shell
git lfs track "*.png"
git lfs track "*.jpg"
git lfs track "*.mp3"
git lfs track "*.pyc"
```

### 添加`.gitattributes`

该文件保存了文件的追踪记录

```shell
git add .gitattributes
```

### 愉快的使用

 进行完上述处理，后面就和正常`git`一样了。不会再有多余的步骤，正常`add,commit,push,pull,clone`即可

```shell
git add file.psd
git commit -m "Add design file"
git push origin master
```

### 官网连接

[git-lfs官网](https://git-lfs.github.com/)