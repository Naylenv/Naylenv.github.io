---
title: 【数学建模】聚类模型 - K-means & 系统（层次）算法
date: 2020-09-06 11:46:55
updated: 2020-09-06 11:46:55
tags:
    - 数学建模
    - 聚类模型
categories:
    - 数学建模
keywords:
    - 数学建模
    - 聚类模型
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200906114751.png
---

# 聚类和分类的区别

聚类是不知道类别，自己分类

分类是已知类别的

# 聚类模型

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906102956654.png)

# K-means聚类

> 数据对象即样本
>
> 聚类中心即重心

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906103044052.png)

**PS：和我们初始化选择的中心有很大关系**

## 如何写在论文里

> 因为算法步骤太长了，而且易被查重

因为算法太长了，放流程图：

![一样的意思，但是更让人喜欢](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906103629642.png)

## 优缺点：

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906103719030.png)

# 改进 K-means++算法

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906103837670.png)

## K值这么定，量纲不一样怎么办？

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906105112225.png)

# 不需要指定K的算法：系统（层次）算法

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906105928985.png)

## 样本和样本直接的距离如何计算

### 样本和样本之间的距离

> 绝对值距离和欧式距离最常用
>
> 绝对值距离更多的用于网状数据

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906111013007.png)

### 指标和指标之间的距离（不常用、用于指标分类）

![指标分类，不常用](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906111149539.png)



### 类与类之间的距离

> 因为样本和样本被划分为不同的类，就被划分到不同的类了
>
> 组间、组内用的多

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906111334490.png)

## 系统聚类的流程图

![流程图](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906111652432.png)

## 需要注意的问题：

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906112249195.png)

# 有没有算法能帮助我们确定k的值：肘部法则

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906113001015.png)

## 论文里如何解释？

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200906113700284.png)
