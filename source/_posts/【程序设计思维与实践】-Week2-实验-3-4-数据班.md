---
title: 【程序设计思维与实践】 Week2 实验 (3/4/数据班)
tags:
  - 程序设计思维与实践
categories:
  - 程序设计思维与实践
keywords:
  - 程序设计思维与实践
date: 2021-04-02 17:36:42
updated: 2021-04-02 17:36:42
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402174003.png
---

# A - 化学 (编译器选 GNU G++) [Gym - 270437A ](https://vjudge.net/problem/Gym-270437A/origin)
## 题意

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306114047679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fsb25laW5nY2hpbGQ=,size_16,color_FFFFFF,t_70)

假设如上图，这个烷烃基有6个原子和5个化学键，6个原子分别标号1~6，然后用一对数字 a,b 表示原子a和原子b间有一个化学键。这样通过5行a,b可以描述一个烷烃基

你的任务是甄别烷烃基的类别。

## 思路

这个题就很有意思了，我们可以分类判断。我们可以把每一个原子看成点，其中第1个和第6个有明显的特征，即只有第一个最大出度为2，最后一个最大出度为4。同时我们可以对于每一个烷烃基找到出度最大的点。

这时以出度最大的点为根开始dfs找最大深度，只有第2个最大深度为3。

此时只有3，4没有区分了，只需要刨除出度最大的点再找一个出度次大的点即可。次大出度为2即为第三个，为3即为第3个。

## 总结

本题情况仅为6个，做法很多。比如还可以找最长链。由于为rank，没有仔细考虑，上述思路还有可以提升的地方，将2，3，4选择转化为判断最长链会更简单。

## 代码

```cpp
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <fstream>
#define LL long long
using namespace std;
int v[10][10], vis[10], deep;
void dfs(int x, int len) {
    deep = max(len, deep);
    vis[x] = 1;
    for (int i = 1; i <= 6; i++) {
        if (v[x][i]) {
            if (!vis[i]) dfs(i, len + 1);
        }
    }
    vis[x] = 0;
}
void bfs() {
    int max_sum = 0, k = 0;
    for (int i = 1; i <= 6; i++) {
        int sum = 0;
        for (int j = 1; j <= 6; j++) {
            if (v[i][j]) {
                sum++;
            }
        }
        if (sum > max_sum) {
            max_sum = sum;
            k = i;
        }
    }
    //    cout <<"sum:" <<max_sum<<endl;
    if (max_sum == 2) {
        cout << "n-hexane" << endl;
        return;
    } else if (max_sum == 4) {
        cout << "2,2-dimethylbutane" << endl;
        return;
    }
    memset(vis, 0, sizeof(vis));
    deep = 0;
    dfs(k, 0);
    if (deep == 3) {
        cout << "2-methylpentane" << endl;
        return;
    }
    max_sum = 0;
    for (int i = 1; i <= 6; i++) {
        int sum = 0;
        if (i == k) continue;
        for (int j = 1; j <= 6; j++) {
            if (v[i][j]) {
                sum++;
            }
        }
        if (sum > max_sum) {
            max_sum = sum;
        }
    }
    if (max_sum == 3) {
        cout << "2,3-dimethylbutane" << endl;
        return;
    } else {
        cout << "3-methylpentane" << endl;
        return;
    }
}

int main() {
    int T;
    T = get_num();
    while (T) {
        T--;
        memset(v, 0, sizeof(v));
        for (int i = 1; i <= 5; i++) {
            int a, b;
            a = get_num();
            b = get_num();
            v[a][b] = v[b][a] = 1;
        }
        bfs();
    }
    return 0;
}

```

## B - 爆零(×)大力出奇迹(√)] [HDU - 2093 ](https://vjudge.net/problem/HDU-2093/origin)

## 题意

输入数据包含多行，第一行是共有的题数n（1≤n≤12）以及单位罚时m（10≤m≤20），之后的每行数据描述一个学生的信息，首先是学生的用户名（不多于10个字符的字串）其次是所有n道题的得分现状，根据这些学生的得分现状，输出一个实时排名。实时排名显然先按AC题数的多少排，多的在前，再按时间分的多少排，少的在前，如果凑巧前两者都相等，则按名字的字典序排，小的在前。每个学生占一行，输出名字（10个字符宽），做出的题数（2个字符宽，右对齐）和时间分（4个字符宽，右对齐）。名字、题数和时间分相互之间有一个空格。数据保证可按要求的输出格式进行输出。

### Sample Input

```txt
8 20
GuGuDong  96     -3    40(3) 0    0    1      -8    0
hrz       107    67    -3    0    0    82     0     0
TT        120(3) 30    10(1) -3   0    47     21(2) -2
OMRailgun 0      -99   -8    0    -666 -10086 0     -9999996
yjq       -2     37(2) 13    -1   0    113(2) 79(1) -1
Zjm       0      0     57(5) 0    0    99(3)  -7    0
```

### Sample Output

```txt
TT          5  348
yjq         4  342
GuGuDong    3  197
hrz         3  256
Zjm         2  316
OMRailgun   0    0
```

## 思路

这题核心是如何处理读入数据，即如何区分读入为纯数字，如：4；和读入为字符串，如：4(3)的情况。

只需要将每个读入存入一个字符串，判断字符串内是否为纯数值：

```cpp
bool isNumber(const string& str) {
	istringstream in(str);
	double test;
	return in >> test && in.eof();
}
```

如果是纯数值，负数按0处理，正数加权即可。

如果不为纯数值。只需要读入一个数字，一个字符，一个数字，一个字符即可：

```cpp
	istringstream in(s);
	LL a, b;
	char c;
	in >> a >> c >> b >> c;
```

结构体处理：

```cpp
struct re {
    string name;
    int num;
    LL sum;
    bool operator<(const re& a) const {
        return num != a.num ? (num > a.num) : (sum != a.sum ? sum < a.sum : name < a.name);
    }
};
```

## 总结

此类问题重点在于数据的处理，重要的是模块化，这样解题才会更快。

这里实验课的时候，忘记去除测试`if(cnt == 6)`(六条数据)，吃了两发WA，其实win下测试可以`ctrl+z`再`Enter`就会成为`EOF`了，着实被自己坑了。

## 代码

```cpp
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <fstream>
#include <iostream>
#include <map>
#define LL long long
using namespace std;
bool isNumber(const string& str) {
	istringstream in(str);
	double test;
	return in >> test && in.eof();
}
LL strInt(const string& str){
	istringstream in(str);
	LL test;
	in>>test;
	return test;
}
struct re {
    string name;
    int num;
    LL sum;
    bool operator<(const re& a) const {
        return num != a.num ? (num > a.num) : (sum != a.sum ? sum < a.sum : name < a.name);
    }
} v[20];

int main() {
//	freopen("in.txt","r",stdin);
//	freopen("out.txt","w",stdout);
    int n, m;
    cin >> n >> m;
    string s;
    int cnt = 0;
    while (cin >> s) {
        v[++cnt].name = s;
//        cout << "cnt: "<<v[cnt].name<<endl;
        for (int i = 1; i <= n; i++) {
            string s;
            cin >> s;
            if (isNumber(s)) {
            	LL a = strInt(s);
                if (a <= 0) {
                } else {
                    v[cnt].sum += a;
                    v[cnt].num ++;
                }
            } else {
                istringstream in(s);
                LL a, b;
                char c;
                in >> a >> c >> b >> c;
                v[cnt].num++;
                v[cnt].sum += a + b * m;
            }
        }
//        if (cnt == 6) break;
    }
//    cout << "yes"<<endl;
//    printf("yess\n");
    sort(v + 1, v + 1 + cnt);
    for (int i = 1; i <= cnt; i++) {
        printf("%-10s ", v[i].name.c_str());
        printf("%2d ", v[i].num);
        printf("%4lld\n", v[i].sum);
    }
    return 0;
}

```

## C - 瑞神打牌 [POJ - 1786 ](https://vjudge.net/problem/POJ-1786/origin)

牌局由四个人构成，围成一圈。我们称四个方向为北 东 南 西。对应的英文是North，East，South，West。游戏一共由一副扑克，也就是52张构成。开始，我们指定一位发牌员（东南西北中的一个，用英文首字母标识）开始发牌，发牌顺序为顺时针，发牌员第一个不发自己，而是发他的下一个人（顺时针的下一个人）。这样，每个人都会拿到13张牌。
现在我们定义牌的顺序，首先，花色是（梅花）<（方片）<（黑桃）<（红桃），（输入时，我们用C,D,S,H分别表示梅花，方片，黑桃，红桃，即其单词首字母）。对于牌面的值，我们规定2 < 3 < 4 < 5 < 6 < 7 < 8 < 9 < T < J < Q < K < A。
现在你作为上帝，你要从小到大排序每个人手中的牌，并按照给定格式输出。

### Sample Output
```txt
South player:
+---+---+---+---+---+---+---+---+---+---+---+---+---+
|6 6|A A|6 6|J J|5 5|6 6|7 7|9 9|4 4|5 5|7 7|9 9|T T|
| C | C | D | D | S | S | S | S | H | H | H | H | H |
|6 6|A A|6 6|J J|5 5|6 6|7 7|9 9|4 4|5 5|7 7|9 9|T T|
+---+---+---+---+---+---+---+---+---+---+---+---+---+
West player:
+---+---+---+---+---+---+---+---+---+---+---+---+---+
|2 2|5 5|9 9|K K|5 5|7 7|9 9|4 4|T T|J J|A A|8 8|A A|
| C | C | C | C | D | D | D | S | S | S | S | H | H |
|2 2|5 5|9 9|K K|5 5|7 7|9 9|4 4|T T|J J|A A|8 8|A A|
+---+---+---+---+---+---+---+---+---+---+---+---+---+
North player:
+---+---+---+---+---+---+---+---+---+---+---+---+---+
|3 3|4 4|J J|2 2|3 3|T T|Q Q|K K|8 8|Q Q|K K|2 2|3 3|
| C | C | C | D | D | D | D | D | S | S | S | H | H |
|3 3|4 4|J J|2 2|3 3|T T|Q Q|K K|8 8|Q Q|K K|2 2|3 3|
+---+---+---+---+---+---+---+---+---+---+---+---+---+
East player:
+---+---+---+---+---+---+---+---+---+---+---+---+---+
|7 7|8 8|T T|Q Q|4 4|8 8|A A|2 2|3 3|6 6|J J|Q Q|K K|
| C | C | C | C | D | D | D | S | S | H | H | H | H |
|7 7|8 8|T T|Q Q|4 4|8 8|A A|2 2|3 3|6 6|J J|Q Q|K K|
+---+---+---+---+---+---+---+---+---+---+---+---+---+
```

## 思路

对于牌面的大小，开一个`map`来映射对应大小。一个字符串一个数值的读入即可。由于不支持`C++11`，需要手动初始化`map`（太坑了）。

读入完`sort`一下就星，可以说是这次rank的最简单的一道题

```cpp
struct re {
    char s, v;
    bool operator<(const re& a) const {
        return CARD_S[s] != CARD_S[a.s] ? CARD_S[s] < CARD_S[a.s] : CARD_V[v] < CARD_V[a.v];
    }
};
```

## 总结

没看到题目要求，吃了三发PE。

```txt
输出多组数据发牌的结果，每组数据之后需要额外多输出一个空行！！！！！
```

一定一定要仔细仔细读题。

## 代码

```cpp
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <fstream>
#include <functional>
#include <iostream>
#include <map>
#include <queue>
#define LL long long
using namespace std;
map<char, int> POS, CARD_S, CARD_V;
void dfs() {
    CARD_V['2'] = 2;
    CARD_V['3'] = 3;
    CARD_V['4'] = 4;
    CARD_V['5'] = 5;
    CARD_V['6'] = 6;
    CARD_V['7'] = 7;
    CARD_V['8'] = 8;
    CARD_V['9'] = 9;
    CARD_V['T'] = 10;
    CARD_V['J'] = 11;
    CARD_V['Q'] = 12;
    CARD_V['K'] = 13;
    CARD_V['A'] = 14;
    CARD_S['C'] = 1;
    CARD_S['D'] = 2;
    CARD_S['S'] = 3;
    CARD_S['H'] = 4;
    POS['S'] = 1;
    POS['W'] = 2;
    POS['N'] = 3;
    POS['3'] = 4;
}
struct re {
    char s, v;
    bool operator<(const re& a) const {
        return CARD_S[s] != CARD_S[a.s] ? CARD_S[s] < CARD_S[a.s] : CARD_V[v] < CARD_V[a.v];
        //        return CARD_V[v] != CARD_V[a.v] ? CARD_V[v] < CARD_V[a.v] : CARD_S[s] < CARD_S[a.s];
    }
};
struct ac {
    string s;
    vector<re> card;
} v[5];

int main() {
//     freopen("out.txt", "w", stdout);
    dfs();
    char c;
    while (cin >> c) {
        if (c == '#') break;
        
        for (int i = 1; i <= 4; i++) {
            v[i].card.clear();
        }
        int k = POS[c];
        for (int i = 1; i <= 52; i++) {
            k = (k + 1) % 4;
            if (k == 0) k = 4;
            char a, b;
            cin >> a >> b;
            re node;
            node.s = a;
            node.v = b;
            v[k].card.push_back(node);
        }
        v[1].s = "South player";
        v[2].s = "West player";
        v[3].s = "North player";
        v[4].s = "East player";
        for (int i = 1; i <= 4; i++) {
            sort(v[i].card.begin(), v[i].card.end());
            cout << v[i].s << ":\n";
            cout << "+---+---+---+---+---+---+---+---+---+---+---+---+---+" << endl;
            for (int j = 0; j < 13; j++) {
                cout << "|" << v[i].card[j].v << " " << v[i].card[j].v;
            }
            cout << "|" << endl;
            for (int j = 0; j < 13; j++) {
                cout << "| " << v[i].card[j].s << " ";
            }
            cout << "|" << endl;
            for (int j = 0; j < 13; j++) {
                cout << "|" << v[i].card[j].v << " " << v[i].card[j].v;
            }
            cout << "|" << endl;
            cout << "+---+---+---+---+---+---+---+---+---+---+---+---+---+" << endl;
        }
        cout <<endl;
    }

    return 0;
}
```
# 写在最后
这次rank的教训是，两次`WA`是因为自己第二题没去掉调试信息，三次`PE`是因为第三题没看到每组数据之间要多一行空格。
最终rank 4，较上周有进步，希望下次能拿到某个题目的首A。 
