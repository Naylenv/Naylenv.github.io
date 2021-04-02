---
title: Win 下 VSCode 配置 LaTeX format 自动格式化
date: 2020-09-21 19:31:26
updated: 2020-09-21 19:31:26
tags:
    - LaTeX 
categories:
    - LaTeX 
keywords:
    - LaTeX 
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200921193201.png
---
# 前言
网上关于 win 下 VSCode 配置 LaTeX 自动格式化博客较少，现搜集相关资料整理了一个较简单的教程

# 步骤
## 第一步：下载latexindent

[latexindent下载地址](https://ctan.org/tex-archive/support/latexindent)

## 第二步：解压缩文件放到自己常用目录

![](https://img-blog.csdnimg.cn/img_convert/0ec73f7c0143ed4a91d3c7026d4dc2c4.png)

## 第三步：在VSCode用户Json文件中添加如下：

### 按F1，输入settings.json

![](https://img-blog.csdnimg.cn/img_convert/c37d6d05b522daaa472d865497ee06d9.png)

### 加入如下字段：

```shell
"latex-workshop.latexindent.path": "D:\\LLL\\latex\\latexindent\\latexindent.exe",
```

![](https://img-blog.csdnimg.cn/img_convert/fcdfcc88ef8bf94c9040e9c459e331a1.png)

**大功告成**