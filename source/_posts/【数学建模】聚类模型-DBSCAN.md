---
title: 【数学建模】聚类模型 - DBSCAN
date: 2020-09-06 14:26:51
updated: 2020-09-06 14:26:51
tags:
    - 数学建模
    - 聚类模型
categories:
    - 数学建模
keywords:
    - 数学建模
    - 聚类模型
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200906142746.png
---
# 可视化网址

[聚类可视化网址](https://www.naftaliharris.com/blog/visualizing)

# DBSCAN

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906140717337.png)

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906141324719.png)

# 和其他算法的区别

> K-means 和 系统算法是基于距离的
>
> DBSCAN是基于密度的

## 优点

+ 可以发现任意形状的簇
+ 可以发现噪点

# 总结：

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906142303750.png)

+ 如果数据表现的很有形状就用DBSCAN
+ 其他情况用系统聚类
+ K-means 论文上可以写的东西比较稀少