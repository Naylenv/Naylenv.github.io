---
title: 图形学 glut 一些库的理解（持续更新）
date: 2021-04-02 16:52:12
updated: 2021-04-02 16:52:12
tags:
	- 图形学
categories: 
    - 图形学
keywords:
    - 图形学
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402170932.png
---


@[toc]
# glMatrixMode

GL_PROJECTION 投影, GL_MODELVIEW 模型视图, GL_TEXTURE 纹理.

[glMatrixMode参考链接](https://www.jianshu.com/p/6bd2f4628b37)

# glShadeModel

![GL_FLAT GL_SMOOTH](https://img-blog.csdnimg.cn/img_convert/58b470be7bd90b576e90e627a8f8f81e.png)

GL_SMOOTH会出现过渡效果，GL_FLAT则只是以指定的某一点的单一色绘制其他所有点

[glShadeModel参考链接](https://blog.csdn.net/chenqiai0/article/details/8316258)

# 回调函数

### void glutDisplayFunc（）

注册当前窗口的显示回调函数	

这个函数告诉GLUT当窗口内容必须被绘制时，那个函数将被调用。当窗口改变大小或者从被覆盖的状态

[回调函数参考链接](https://blog.csdn.net/xianhua7877/article/details/81271618)

# 颜色

## glClearColor ( )设置颜色缓存的清除值

glClearColor ( ) 就是用来设置这个 “  底色 ” 的，即所谓的背景颜色。glClearColor ( ) 只起到Set 的作用，并不Clear 任何。

glClearColor ( ) 的作用是指定刷新颜色缓冲区时所用的颜色，所以完成一个刷新的过程是要 glClearColor ( COLOR)  与glClear ( GL_COLOR_BUFFER_BIT) 配合使用。

```cpp
glClearColor(1.0, 0.0, 0.0, 1.0);//红色
glClear(GL_COLOR_BUFFER_BIT); //GL_COLOR_BUFFER_BIT 是缓冲标志位，表明需要清除的缓冲是颜色缓冲
```

清除颜色缓冲区的作用是防止缓冲区中原有的颜色信息影响本次绘图（注意：即使认为可以直接覆盖原值，也是有可能会有影响），当绘图区域为整个窗口时，就是通常看到的，颜色缓冲区的清除值就是窗口的背景颜色。所以，这两条清除指令并不是必须的，比如对于静态画面只需设置一次，比如不需要背景色 / 背景色为白色。

另外，glClear ( ) 比手动涂抹一个背景画布效率高且省力，所以通常使用这种方式。

## glClear ( ) 将缓存清除为预先的设置值

| **mask**              | **说明**                         |
| --------------------- | -------------------------------- |
| GL_COLOR_BUFFER_BIT   | 指定当前被激活为写操作的颜色缓存 |
| GL_DEPTH_BUFFER_BIT   | 指定深度缓存                     |
| GL_ACCUM_BUFFER_BIT   | 指定累加缓存                     |
| GL_STENCIL_BUFFER_BIT | 指定模板缓存                     |

## glColor ( )

glColor ( ) 是用来设置画笔的颜色，即绘图颜色。属于RGBA模式。

## glShadeModel ( )

glShadeModel ( ) 函数用于控制 opengl 中绘制指定两点间其他点颜色的过渡模式。

参数一般为 GL_SMOOLH ( 默认 ) 或 GL_FLAT。

如果两点的颜色相同，则使用这两个参数效果相同；

如果两点颜色不同，GL_SMOOLH  会出现过渡效果；  GL_FLAT 则以指定的某一点的单一色绘制其他所有点。

## glClearDepth ( ) 设置深度缓存的清除值

depth 指定清除深度缓存时使用的深度值，值范围在[ 0  , 1 ] 之间，初始值为1。该值将被用于glClear ( ) 函数清理深度缓冲区

## glDepthFunc ( ) 指定用于深度缓冲的比较值

| func值       | 含义                                     |
| ------------ | ---------------------------------------- |
| GL_NEVER     | 不通过（输入的深度值不取代参考值）       |
| GL_LESS      | 如果输入的深度值小于参考值，则通过       |
| GL_EQUAL     | 如果输入的深度值等于参考值，则通过       |
| GL_LEQUAL    | 如果输入的深度值小于或等于参考值，则通过 |
| GL_GREATER   | 如果输入的深度值大于参考值，则通过       |
| GL_NOTE_QUAL | 如果输入的深度值不等于参考值，则通过     |
| GL_GEQUAL    | 如果输入的深度值大于或等于参考值，则通过 |
| GL_ALWAYS    | 总是通过（输入的深度值取代参考值）       |

# 画线 GL_LINES & GL_LINE_STRIP & GL_LINE_LOOP

+ GL_LINES ：每一对顶点被解释为一条直线
+ GL_LINE_STRIP: 一系列的连续直线
+ GL_LINE_LOOP：一系列的连续直线，且首尾相接

[OpenGL绘线方式 GL_LINES与GL_LINE_STRIP的区别](https://blog.csdn.net/xiaoxiaoyusheng2012/article/details/44197283)