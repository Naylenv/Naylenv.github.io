---
title: 【程序设计思维与实践】 Week3 作业 (3/4/数据班)
tags:
  - 程序设计思维与实践
categories:
  - 程序设计思维与实践
keywords:
  - 程序设计思维与实践
date: 2020-03-06 12:13:45
updated: 2020-03-06 12:13:45
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402173938.png
---

# A - 选数问题

## 题意

给定$n$个数，从中选取$k$个，另总和为$s$，共$T$组数据。其中$k \le n \le 16$，$T \le 100$。

### input

```txt
1
10 3 10
1 2 3 4 5 6 7 8 9 10
```

### output

```txt
4
```

## 思路

朴素的$dfs$即可。虽然数据规模很小，但还是可以剪枝去掉一些情况。

+ 超过$k$个数去掉
+ 总数超过$s$去掉

最坏复杂度$O (n!)$

## 总结

签到题

## 代码

```cpp
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cmath>
#include <cstring>
#include <string>
#include <algorithm>
#include <vector>
#include <functional>
#define LL long long
using namespace std;
int v[20];
int n, k, s, ans;
void dfs(int x, int sum, int cnt) {
    // cout << "dfs: " << x << " " << sum << " " << cnt << endl;
    if (sum > s || cnt > k) return;
    if (cnt == k) {
        if (sum == s) {
            // cout << x << " " << sum << " " << k << endl;
            ans++;
        }
        return;
    }

    for (int i = x; i <= n; i++) {
        dfs(i + 1, sum + v[i], cnt + 1);
    }
}
int main() {
    int T;
    cin >> T;
    while (T) {
        T--;
        cin >> n >> k >> s;
        for (int i = 1; i <= n; i++) {
            cin >> v[i];
        }
        ans = 0;
        dfs(1, 0, 0);
        cout << ans << endl;
    }
    return 0;
}
```

# B - 区间选点

## 题意

数轴上有 $n$ 个闭区间 $[a_i, b_i]$。取尽量少的点，使得每个区间内都至少有一个点（不同区间内含的点可以是同一个）。其中，$(N<=100)$，$(a,b<=100)$

### input

```txt
3
1 3
2 5
4 6
```

### output

```txt
3
1 3
2 5
4 6
```

## 思路

以线段右端点$b$为序，从小到大排序。如果当前线段没被覆盖过。则将用顶点将该线段覆盖，同时标记其他左端点$a_j$在该线段$b_j$的线段为覆盖。

复杂度是$O(n^2)$，因为每个线段只会被选择一次。

### 正确性证明

由于以右端点$b$为序，对第$i$条线段。若改线段未被覆盖。那么将点防止在$b_i$处一定最优。因为对于任意其它线段，线段的左端点$a_j$一定要么在$b_i$的左边，要么在$ b_i$的右边。

+ 若在$b_j$左边，放在$b_i$处显然可以覆盖更多线段
+ 若在$b_j$右边，放在那里都不会影响该线段。

------

假设当前有一个点没有放在线段右顶点处，考虑将点重新放置于有顶点处不会使结果更差。

## 总结

一道入门贪心题，可用交换论证证明，即我的决策不会令答案更差

## 代码

```cpp
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cmath>
#include <algorithm>
#define LL long long
using namespace std;
struct re {
    int a, b;
    bool operator<(const re& a) const { return b < a.b; }
} v[110];
int vis[110];
int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> v[i].a >> v[i].b;
    }
    sort(v + 1, v + 1 + n);
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            ans++;
            for (int j = i + 1; j <= n; j++) {
                if (v[j].a <= v[i].b) {
                    vis[j] = 1;
                }
            }
        }
    }
    cout << ans;
    return 0;
}
```

# C - 区间覆盖

## 题意

数轴上有 $n$ ,$(1\le n \le 25000)$个闭区间$ [ai, bi]$，选择尽量少的区间覆盖一条指定线段 $[1, t]$$( 1 \le t \le 1,000,000)$。
覆盖整点，即$(1,2)+(3,4)$可以覆盖$(1,4)$。
不可能办到输出$-1$

### input

```txt
3 10
1 7
3 6
6 10
```

### output

```txt
2

```

## 思路

讲区间以左端点$a$为序，从小到大排序。对于一段未被覆盖的区间$[h,t]$吗，选取$h$前的超过$h$最多的线段，设选择线段为$[a_k,b_k]$（假设$b_k < t$ ，若$b_k \ge t$ 则覆盖完成得到答案）。仍未被覆盖的区间变为$[b_k+1,t]$，重复上述过程，直到$b_k \ge t$，若扫描完所有线段仍未覆盖完所有区间，则输出$-1$。

复杂度为$O( n )$

### 小技巧

上述过程的复杂度可以达到为$O( n )$，方法是排序后。标记一个flag为仍未被覆盖的线段的首届点$h$，顺序扫描数组，对尾节点取$max$，知道线段首届点$a$超过$h$。（因为我们并不关心选取的具体是哪一条线段）。

```cpp
for (int i = 0; i < n; i++) {
    if (v[i].a > T || tail >= T) {
        break;
    }
    if (v[i].a <= flag) {
        tail = max(tail, v[i].b);
    } else {
        ans++;
        flag = tail + 1;  // 这里注意
        if (v[i].a > flag) {
            tail = 0;
            break;
        } else {
            tail = max(tail, v[i].b);
        }
    }
}
```



## 总结

这道题吃了超多发WA，错误的愿意是选择区间$[a_k,b_k]$后，未覆盖的区间应该变为$[b_k+1,t]$，而不是$[b_k-1,t]$

```cpp
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cmath>
#include <vector>
#include <cstring>
#include <string>
#include <algorithm>
#define LL long long
using namespace std;
struct re {
    int a, b;
    bool operator<(const re& a) const { return this->a < a.a; }
};
vector<re> v;
int main() {
    int n, T;
    while (~scanf("%d%d", &n, &T)) {
        v.clear();
        for (int i = 1; i <= n; i++) {
            re temp;
            scanf("%d%d", &temp.a, &temp.b);
            v.push_back(temp);
        }
        sort(v.begin(), v.end());
        int flag = 1, tail = 0, ans = 0;
        for (int i = 0; i < n; i++) {
            if (v[i].a > T || tail >= T) {
                break;
            }
            if (v[i].a <= flag) {
                tail = max(tail, v[i].b);
            } else {
                ans++;
                flag = tail + 1;  // 这里注意
                if (v[i].a > flag) {
                    tail = 0;
                    break;
                } else {
                    tail = max(tail, v[i].b);
                }
            }
        }
        ans++;
        if (tail < T)
            printf("%d\n", -1);
        else
            printf("%d\n", ans);
    }
    return 0;
}

```


