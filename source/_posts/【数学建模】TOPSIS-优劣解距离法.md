---
title: 【数学建模】TOPSIS|优劣解距离法
date: 2020-08-19 22:41:39
updated: 2020-08-19 22:41:39
tags:
    - 数学建模
categories:
    - 数学建模
keywords:
    - 数学建模
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200819000754.png
---
# TOPSIS

![前言](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819214718017.png)

## 层次分析法的局限：

+ 决策层不能太多
+ 如果又数据已知，不能利用这些数据

![层次分析法局限性](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819214852267.png)

# 问题的提出：

![提出问题](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819215436539.png)

##  为什么要这样算：

![归一化](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819215559782.png)

为什么制表 要 max min

![为什么要这样归一化](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819215623623.png)

## 极大型（效益型）指标和极小型（成本型）指标：

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819215855541.png)

### 统一指标类型

**指标正向化**（最常用）（PS：当然可以反过来

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819220105950.png)

### 标准化（消去量纲）：

![标准化（消去量纲）](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819220413290.png)

## 如何计算：

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819220607697.png)

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819220719914.png)

看似麻烦，其实很简单。就是每取出每一列的最大值和最小值形成单独的向量$Z^+$ 和$Z^-$然后每一行去用欧式距离算量$D_i^+$ 和$D_i^-$

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819221322949.png)

## 因此TOPSIS被称为优劣解距离法：

![TOPSIS介绍](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819221413159.png)

# 深入：TOPSIS

## 第一步：原始矩阵正向话

### 常见的四种指标

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819221521514.png)

### 极小型到极大型

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819221623113.png)

### 中间型到极大型

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819221657855.png)

### 区间型到最大型

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819221910539.png)

## 第二步：正向化标准矩阵（消除量纲影响

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819222148109.png)

## 第三步：计算得分并归一化

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819222219776.png)

### 为什么要归一化？

+ 在基于梯度下降的算法中，使用特征归一化方法将特征统一量纲，能够提高模型收敛速度和最终的模型精度。
+ 归一化目的就是将不同尺度上的评判结果统一到一个尺度上，从而可以作比较，作计算
+ 矢量是归一化比较常见的使用场景。因为一般矢量只关心方向，距离，长度没有意义。因此归一化就是将x，y，z3个值放入0-1.0的范围内。

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819222926875.png)

# 模型扩展（上面是默认所有指标权重相同）：

## 带权重的TOPSIS（用层次分析法赋予指标权重

因为各个指标有不同权重，只需用层次分析法求出$w_i$稍作修改模型即可：

![](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/image-20200819223523864.png)