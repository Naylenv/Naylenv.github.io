---
title: 【程序设计思维与实践】 Week2 作业 (3/4/数据班)
tags:
  - 程序设计思维与实践
categories:
  - 程序设计思维与实践
keywords:
  - 程序设计思维与实践
date:  2020-03-01 21:25:31
updated: 2020-03-01 21:25:31
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402174108.png
---

# 写在前面

在看代码的过程中整理了动态数组的相关内容，由于篇幅较长，且比较重要，单独整理了一篇博客如下：

[动态数组初始化](https://blog.csdn.net/Aloneingchild/article/details/104600465)



# A - MAZE

## 题意

东东有一张地图，想通过地图找到妹纸。地图显示，0表示可以走，1表示不可以走，左上角是入口，右下角是妹纸，这两个位置保证为0。既然已经知道了地图，那么东东找到妹纸就不难了，请你编一个程序，写出东东找到妹纸的最短路线。输入是一个5 × 5的二维数组，且保证一定有解

## 思路

从(0，0)位置开始BFS，每次向上下左右四个方向扩展状态。直到最终达到（4，4）为止。

可以map，来记录路径。map中的关系为当前状态又哪一个状态转移而来。

根据BFS的性质，第一个达到（4，4）的值定位最优解。

## 总结

这是一道简答的BFS题，利用map记录路径即可。

这里注意，对于结构体重载比较运算符只需要重载一个`<`就够了，至于为什么，因为`c++`会自动补出其它运算符。PS：只能用于`<`，其它符号不可以。

## 代码

```cpp
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <map>
#include <string>
#include <vector>
#define LL long long
using namespace std;
struct re {
    int x, y;
    bool operator<(const re& a) const { return x != a.x ? x < a.x : y < a.y; }
};

int vis[10][10], v[10][10];
int fx[] = {0, 0, 1, -1};
int fy[] = {1, -1, 0, 0};
map<re, re> M;
void dfs(re s) {
//	cout << s.x<<" "<< s.y<<endl;
    if (M.find(s) != M.end()) {
        dfs(M[s]);
    }
    cout << "(" << s.x << ", " << s.y <<")"<<endl;
}
void bfs() {
    queue<re> Q;
    Q.push({0, 0});
    vis[0][0]= 1;
    while (!Q.empty()) {
        re s = Q.front();
        Q.pop();
        if (s.x == 4 && s.y == 4) {
            dfs({4, 4});
            return;
        }
        for (int i = 0; i < 4; i++) {
            int tx = s.x + fx[i], ty = s.y + fy[i];
            if (tx < 0 || ty < 0 || tx > 4 || ty > 4 || vis[tx][ty] || v[tx][ty]) continue;
            M[{tx, ty}] = s;
            vis[tx][ty] = 1;
            Q.push({tx, ty});
        }
    }
}
int main() {
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            cin >> v[i][j];
        }
    }
    bfs();
    return 0;
}

```



# B - Pour Water

## 题意

倒水问题 "fill A" 表示倒满A杯，"empty A"表示倒空A杯，"pour A B" 表示把A的水倒到B杯并且把B杯倒满或A倒空。

## 思路

首先初始化位两个杯子均为空（0，0）。

对于每一个状态，至多有六种状态可以转移：

+ 倒满A
+ 倒满B
+ 倒空A
+ 倒空B
+ A倒入B
+ B倒入A

注意进行适当剪枝，如当前A已空，不必再次倒空A即可。

同样可以利用map来记录路径。

## 总结

这是一个类隐藏图问题，将每一个状态看成一个点，扩展出其它节点。直到满足条件。同样可以利用map来记录路径。

## 代码
```cpp
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <map>
#define LL long long
using namespace std;
struct re {
    int x, y;
    string str;
    bool operator < (const re& a) const { return x != a.x ? x < a.x : y < a.y; }
};
map<re, re> M;
void dfs(re t) {
    if (t.x == 0 && t.y == 0) {
    	return;    
    }
    dfs(M[t]);
    cout << t.str << endl;
}
void bfs(int A, int B, int C) {
    queue<re> Q;
    M.clear();
    re fnode;
    fnode.x = fnode.y = 0;
    Q.push(fnode);
    //    cout << A<<" "<<B<<" "<<C<<endl;
    while (!Q.empty()) {
        re snode = Q.front();

        Q.pop();
        //        cout << snode.x<<" "<<snode.y<<endl;
        if (snode.x == C || snode.y == C) {
            dfs(snode);
            cout << "success" << endl;
            return;
        }
        if (snode.x != A) {  // x 倒满
            re tnode = snode;
            tnode.x = A;
            tnode.str = "fill A";
            if (M.find(tnode) == M.end()) {
                M[tnode] = snode;
                Q.push(tnode);
            }
        }
        if (snode.y != B) {  // y 倒满
            re tnode = snode;
            tnode.y = B;
            tnode.str = "fill B";
            if (M.find(tnode) == M.end()) {
                M[tnode] = snode;
                Q.push(tnode);
            }
        }
        if (snode.y != 0) {  // y 倒空
            re tnode = snode;
            tnode.y = 0;
            tnode.str = "empty B";
            if (M.find(tnode) == M.end()) {
                M[tnode] = snode;
                Q.push(tnode);
            }
        }
        if (snode.x != 0) {  // x 倒空
            re tnode = snode;
            tnode.x = 0;
            tnode.str = "empty A";
            if (M.find(tnode) == M.end()) {
                M[tnode] = snode;
                Q.push(tnode);
            }
        }
        if (snode.x != 0) {  // x 倒给 y
            re tnode = snode;
            tnode.y = min(B, tnode.y + tnode.x);
            tnode.x = snode.x - (tnode.y - snode.y);
            tnode.str = "pour A B";
            if (M.find(tnode) == M.end()) {
                M[tnode] = snode;
                Q.push(tnode);
            }
        }
        if (snode.y != 0) {  // y 倒给 x
            re tnode = snode;
            tnode.x = min(A, tnode.x + tnode.y);
            tnode.y = snode.y - (tnode.x - snode.x);
            tnode.str = "pour B A";
            if (M.find(tnode) == M.end()) {
                M[tnode] = snode;
                Q.push(tnode);
            }
        }

        /* code */
    }
}
int main() {
    int A, B, C;
    while (cin >> A >> B >> C) {
        bfs(A, B, C);
    }
    return 0;
}
```