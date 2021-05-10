---
title: C++基于getline的split实现
tags:
  - C++
categories:
  - C++
keywords:
  - C++
date: 2021-05-10 16:31:07
updated: 2021-05-10 16:31:07
cover:
  - https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210510165035.png
---
# 前言
C++ 作为老大哥竟然一直不支持 split ，在看程设模拟的时候发现了可以通过 getline 优雅的实现 split。
# 代码
```cpp
vector<string> split(const string& s, char c) {  //分隔文件名
    vector<string> res;
    string tmp;
    stringstream ss(s);
    while (getline(ss, tmp, c)) res.push_back(tmp);  //res保存整体
    return res;
}
// std::vector<std::string> split(const std::string& line, char c) {
//     std::stringstream stm(line);
//     std::vector<std::string> ans;
//     std::string tmp;
//     while (std::getline(stm, tmp, c)) ans.push_back(tmp);
//     return ans;
// }
int main() {
    // ios::sync_with_stdio(false);
    // cout.tie(NULL);
    string s;
    while (cin >> s) {
        vector<string> V = split(s, '/');
        cout << V.size() << " ";
        for (auto& it : V) {
            cout << it << " ";
        }
        cout << endl;
    }
    return 0;
}
```

## input

```
a/bb/cc
a/bb/cc/
a/bb/cc-c//c
```

## output

```
3 a bb cc 
3 a bb cc 
5 a bb cc-c  c 
```

# 解释

`basic_istream& getline( char_type* s, std::streamsize count, char_type delim );`

从流释出字符，直至**行尾**或**指定的分隔符** `delim` 。

参考链接：

[cppreference](https://zh.cppreference.com/w/cpp/io/basic_istream/getline)