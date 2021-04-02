---
title: SVM目标函数的一些理解
tags:
  - 机器学习
categories:
  - 机器学习
keywords:
  - SVM
  - 机器学习
date: 2020-11-27 21:02:37
updated: 2020-11-27 21:02:37
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402171400.png
---

# 写在前面

学习SVM的对目标函数有些疑问，做了一些笔记。感谢ZKX同学提供的帮助，（PS：这篇博客可能会继续更新

# SVM

我们的问题是设定一个超平面，去最大化样本点和这个超平面的距离，这个距离我们称之为Margin（间隔）。

$$
\gamma=\min _{i} \gamma^{(i)}
$$

![](https://img-blog.csdnimg.cn/20201127212016190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fsb25laW5nY2hpbGQ=,size_16,color_FFFFFF,t_70#pic_center)


## 函数间隔 Functional margin： 

这一点老师的PPT上并没有给出来，所以可能学习的时候有些混淆。

$$
\hat{\gamma}^{(i)}=y^{(i)}\left(\omega^{T} x^{(i)}+b\right)
$$

这里我们扩大$\omega,b$ n 倍数会改变$\hat{\gamma}$。因此我们引入几何间隔。

## 几何间隔 Geometric margin ：

$$
\gamma^{(i)}=y^{(i)}\left(\left(\frac{\omega}{\|\omega\|}\right)^{T} x^{(i)}+\frac{b}{\|\omega\|}\right)
$$

几何间隔具有缩放不变性：

$$
\begin{aligned}\gamma^{(i)} &=y^{(i)}\left(\left(\frac{c \cdot \omega}{\|c \cdot \omega\|}\right)^{T} x^{(i)}+\frac{c \cdot b}{\|c \cdot \omega\|}\right) \\&=y^{(i)}\left(\left(\frac{\omega}{\|\omega\|}\right)^{T} x^{(i)}+\frac{b}{\|\omega\|}\right)\end{aligned}
$$

有了这个性质我们可以对求解目标进行限制，简化优化问题。

这里要注意：如果$|\omega|=1$,那么函数间隔和几何间隔相等。如果超平面参数$\omega$和$b$成比例地改变（超平面没有改变）,函数间隔也按此比例改变，而几何间隔不变。

# SVM的目标问题：

$$
\max _{\omega, b} \min _{i}\left\{\gamma^{(i)}\right\}
$$

**目标问题**可以转化为：

$$
\begin{array}{ll}\max _{\gamma, \omega, b} & \gamma\\\text {s.t.} & \gamma^{(i)} \geq \gamma, \quad \forall i\end{array}
$$

其中，间隔为函数间隔：

$$
\gamma^{(i)}=y^{(i)}\left(\left(\frac{\omega}{\|\omega\|}\right)^{T} x^{(i)}+\frac{b}{\|\omega\|}\right)
$$

因此**目标问题**转化为：

$$
\begin{array}{ll}\max _{\gamma, \omega, b} & \gamma\\\text {s.t.} & y^{(i)}\left(\omega^{T} x^{(i)}+b\right) \geq \gamma\|\omega\| \ \quad \forall i\end{array}
$$

利用几何间隔不变性，一方面为了使优化目标更加简单。

另一方面有点类似于标准化的思想因为不同模型由于数据分布的原因,$\omega$和$b$可能会大不相同（数据集1的分法和数据集2的分法结果的好坏因为$\omega$和$b$比例的不同难以直观比较）。但是把他们的最小间隔都设置为1后，就有比较性了。同一个数据的不同超平面，也有了比较的方法
$$
\min _{i}\left\{y^{(i)}\left(\omega^{T} x^{(i)}+b\right)\right\}=1
$$

即令最小间隔为$\frac{1}{\|\omega\|}$：

$$
\gamma=\min _{i}\left\{y^{(i)}\left(\left(\frac{\omega}{\|\omega\|}\right)^{T} x^{(i)}+\frac{b}{\|\omega\|}\right)\right\}=\frac{1}{\|\omega\|}
$$

因此**目标问题**变为：

$$
\begin{array}{ll}\max _{\omega, b} & 1 /\|\omega\| \\\text { s.t. } & y^{(i)}\left(\omega^{T} x^{(i)}+b\right) \geq 1, \forall i\end{array}
$$

又仅为最小化$\frac{1}{\|\omega\|}$等价于最大化$\|\omega\|^{2}=\omega^{T} \omega$（这样凑是为了满足二次规划形式）

最终目标函数即为：

$$
\begin{array}{l}\min _{\omega, b} \omega^{T} \omega \\\text { s.t. } \quad y^{(i)}\left(\omega^{T} x^{(i)}+b\right) \geq 1, \forall i\end{array}
$$

# 参考文献

[SVM - Understanding the math - the optimal hyperplane](https://www.svm-tutorial.com/2015/06/svm-understanding-math-part-3/)

[Lecture 6: Support Vector Machine](https://funglee.github.io/ml/slides/Lecture6-SVM.pdf)

[从零构建支持向量机(SVM)](https://funglee.github.io/ml/ref/svmhao.pdf)

[svm函数的解释（转载）](https://zhuanlan.zhihu.com/p/106024205)