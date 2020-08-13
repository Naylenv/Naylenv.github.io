---
title: python 访问文件夹下所有文件、图片
date: 2020-03-16 17:14:37
updated: 2020-03-16 17:14:37
tags:
    - Python
    - Django
categories:
    - Python
keywords:
    - Python
    - Django
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813215710.png
---

# 问题
如何使用`python`访文件夹下的所有文件？
# 解决
+ 使用`os.listdir(path)`装载文件路径
+ 使用`os.path.join()`可拼接获得完整路径，对于`windows`，需要补全文件夹名后面的`/`，否则`python`会错误的添加`\`（如：`"./test\a.png”`。
+ 使用open()打开目标文件
## 图片类型
对于图片类型，以`rb`(只读二进制)打开，避免对图片错误写。~~一开始以`w`一直图片格式损坏，鼓捣半天才发现文件被写没了。~~ 
# 例子
```python
path = "./test/"
files = os.listdir(path)
for filename in files:
    f = open(os.path.join(path, filename),'rb')
    print(filename)
    print(os.path.join(path, filename))