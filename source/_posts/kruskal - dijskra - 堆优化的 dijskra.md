---
title: 【最小生成树】Kruskal - Prim - 堆优化的Prim
date: 2020-03-20 10:35:01
updated: 2020-03-20 10:35:01
tags:
    - 算法
    - 图论
    - 程序设计思维时间
    - 数据结构
categories: 
	- 图论
keywords:
    - 算法
    - 图论
    - 程序设计思维时间
    - 数据结构
description: 
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813191444.png
---
@[toc]
# 写在前面

整理了一份最小生成树算法板子

# 题目 C - 掌握魔法の东东 I

东东在老家农村无聊，想种田。农田有$ n$ 块，编号从 1~$n$。种田要灌氵
众所周知东东是一个魔法师，他可以消耗一定的 MP 在一块田上施展魔法，使得黄河之水天上来。他也可以消耗一定的 MP 在两块田的渠上建立传送门，使得这块田引用那块有水的田的水。 $(1 \le n \le 3e2)$
黄河之水天上来的消耗是$W_i$，$i$ 是农田编号 $(1 \le W_i \le 1e5)$
建立传送门的消耗是 $P_{ij}$，$i$、$j$是农田编号 $(1 \le P_{ij}  \le 1e5, P_{ij} = P_{ji}, P_{ii} =0)$
东东为所有的田灌氵的最小消耗

## input

第1行：一个数$n$
第2行到第$n+1$行：数$w_i$
第$n+2$行到第$2n+1$行：矩阵即$p_{ij}$矩阵

## output

东东最小消耗的MP值

## sample input

```shel
4
5
4
4
3
0 2 2 2
2 0 3 3
2 3 0 4
2 3 4 0
```

## Sample Output

```shel
9
```

# Kruskal 

复杂度$O(mlogm)$

```cpp
const int MAXN = 3e2 + 7;
int par[MAXN];
struct re {
    int x, y, w;
    bool operator<(const re& a) const { return w < a.w; }
} v[MAXN * MAXN];
int find(int x) { return par[x] == x ? x : par[x] = find(par[x]); }
int main() {
    int n;
    cin >> n;
    int cnt = 1;
    for (int i = 1; i <= n; i++) {
        v[cnt].x = 0;
        v[cnt].y = i;
        cin >> v[cnt].w;
        ++cnt;
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            int w = get_num();
            if (i == j) continue;
            v[cnt].x = i;
            v[cnt].y = j;
            v[cnt].w = w;
            cnt++;
        }
    }
    sort(v + 1, v + cnt);
    int ans = 0, sum = 0;
    for (int i = 1; i <= n; i++) par[i] = i;
    for (int i = 1; i < cnt; i++) {
        if (sum == n) break;
        if (find(v[i].x) != find(v[i].y)) {
            ans += v[i].w;
            par[find(v[i].x)] = find(v[i].y);
            sum++;
        }
    }
    cout << ans;
    return 0;
}
```

# Prim(Dijkstra)

复杂度$O(n^2)$

```cpp
const int MAXN = 3e2 + 7;
int v[MAXN][MAXN], vis[MAXN], d[MAXN];
int main() {
    int n;
    n = get_num();
    for (int i = 1; i <= n; i++) {
        v[0][i] = v[i][0] = get_num();
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            v[i][j] = get_num();
        }
    }
    for (int i = 0; i <= n; i++) {
        vis[i] = 0;
        d[i] = 1e9;
    }
    d[0] = 0;
    int ans = 0;
    for (int i = 0; i <= n; i++) {
        int sum = 1e9, k;
        for (int i = 0; i <= n; i++) {
            if (!vis[i] && d[i] < sum) {
                sum = d[i];
                k = i;
            }
        }
        ans += d[k];
        vis[k] = 1;
        for (int i = 0; i <= n; i++) {
            d[i] = min(d[i], v[k][i]);
        }
    }
    cout << ans;
    return 0;
}
```

# 堆优化的Prim

复杂度$O((m+n)logn)$，若采用邻接矩阵存图时间复杂度为$O(nlogn )$

```cpp
const int MAXN = 3e2 + 7;
int v[MAXN][MAXN], vis[MAXN], d[MAXN];
struct re {
    int d, w;
    bool operator<(const re &a) const { return w > a.w; }
};
priority_queue<re> Q;
int main() {
    int n;
    n = get_num();
    for (int i = 1; i <= n; i++) {
        v[0][i] = v[i][0] = get_num();
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            v[i][j] = get_num();
        }
    }
    for (int i = 0; i <= n; i++) {
        vis[i] = 0;
        d[i] = 1e9;
    }
    d[0] = 0;
    Q.push({0, 0});
    int ans = 0;
    while (!Q.empty()) {
        re h = Q.top();
        Q.pop();
        if (vis[h.d]) continue;
        vis[h.d] = 1;
        ans += h.w;
        for (int i = 0; i <= n; i++) {
            if (d[i] > v[h.d][i]) {
                d[i] = v[h.d][i];
                Q.push({i, d[i]});
            }
        }
    }
    cout << ans;
    return 0;
}
```

# 时间效率对比
从上至下依次为堆优化的Prim，Prim，Kruskal：
![从上至下依次为堆优化的Prim，Prim，Kruskal](https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813192903.png)
