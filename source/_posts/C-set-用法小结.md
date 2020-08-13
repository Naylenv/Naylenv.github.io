---
title: C++ set 用法小结
date: 2020-03-11 23:22:48
updated: 2020-03-11 23:22:48
tags:
    - C++
    - 数据结构
categories:
    - C++
keywords:
    - C++
    - 数据结构
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813215913.png
---

### 写在前面
无意间翻到了17年11月准备NOIP时整理的set用法，现把它放到csdn上来

------

## `<set>` 小结
` set`的英文意思是“集合”， 集合都不陌生吧，集合的特点有唯一性，即：每一个元素只有一个，所以`set`可以用来“去重”操作，`set`还有默认的排序。

  1. 头文件——`<set>`
   2. 定义——`set<int>Q`
   3. 输入（插入）——`insert(x)`
   4. 有序输出: 

```cpp
 set<int>::iterator it;

 for(it = Q.begin(); it != Q.end(); it++)

 cout<<*it<<endl;
```

5. 删除制定元素——`erase(x)`
6. 清空——`clear()`
7. 判空——`empty()`
8. 大小——`size()`
9. 二分查找——`Q.lower_bound(x)`


## set 的 `lower_bound()` `upper_bound`

内部自带 `lower_bound()` `upper_bound`（这俩返回的是迭代器）

`lower_bound(key_value)` ，返回第一个大于等于`key_value`的定位器

`upper_bound(key_value)`，返回最后一个大于等于`key_value`的定位器 

 ### `erase`的三种用法

`erase(iterator)`  ,删除定位器`iterator`指向的值

`erase(first,second)`,删除定位器`first`和`second`之间的值

`erase(key_value)`,删除键值`key_value`的值

## 示例

```cpp
int main() {
    int n = get_num();
    for (int i = 1; i <= n; i++) {
        Q.insert(get_num());
    }
    set<int>::iterator p_s;
    for (p_s = Q.begin(); p_s != Q.end(); p_s++) {
        cout << *p_s << " ";
    }
    cout << endl;
    p_s = Q.lower_bound(1);
    Q.erase(p_s);
    for (p_s = Q.begin(); p_s != Q.end(); p_s++) {
        cout << *p_s << " ";
    }
    return 0;
}
```