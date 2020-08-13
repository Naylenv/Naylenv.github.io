---
title: Django ManyToManyField - 插入 - ORM - admin中显示
date: 2020-03-20 11:12:28
updated: 2020-03-20 11:12:28
tags:
	- Python
	- Django
	- 数据库
categories: 
	- Django
keywords:
	- Python
	- Django
	- 数据库
description: 
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813191157.png
---

# 综述

`Django` `ManyToManyField`的`ORM`操作和在`admin`中显示。

# 表结构设计

假设一个作者有多本书，一本书也可以有个作者，多对多关系。

```python
# 表结构设计
class Book(models.Model):
   title = models.CharField(max_length=20)

class Author(models.Model):
   name = models.CharField(max_length=20)
   books = models.ManyToManyField(Book)
```

# 在admin中显示

当数据过多时，`django`自带的`ManyToManyField`及其不方便。仅需在`admin.py`添加如下字段即可。

```python
# 修改前
admin.site.register(Author)
```
使用`filter_horizontal`。在作多项选择的操作方便性，及单项选择太多时，会有极好的体验。
```python
# 修改后
class AuthorAdmin(admin.ModelAdmin):
    list_display = ['name']  # 列表页展示的字段
    filter_horizontal = ('cards',)
admin.site.register(AuthorAdmin)
```

**若想在admin中显示cards字段。增加如下代码即可**

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ['name','relatedbooks']  # 列表页展示的字段    
    def relatedbooks(self, obj):
        return [book.title for book in obj.books.all()]
    filter_horizontal = ('books',)
    
admin.site.register(Author,AuthorAdmin)

```

# ORM操作

## all 关联的所有的元组

一个作者的所有书。表Author中某一元组关联表Book中的所有元组

```python
	author = models.Author.objects.get(pk=1)
    books = author.books.all()’
    for book in books:
        print(books.title)
```

## add 添加多对多关系

重复添加同一关系`django`会自动忽略

```python
	author = models.Author.objects.get(pk=1)
    author.books.add(Book.objects.all())
    author.books.add(Book.objects.get(id=3))
```

## remove 多对多关系

```python
	author = models.Author.objects.get(pk=1)
	author.books..remove(Book.objects.get(id=3))
```
## set 替换

直接完整的替换某一多对多关系

```python
	author = models.Author.objects.get(pk=1)
	author.books.set(Book.objects.get(id=3))
```

## clear 清除

清除一元组所有多对多关系

```python
	author = models.Author.objects.get(pk=1)
	author.books.clear()
```

# 一张表自关联

```python
from django.db import models

class Person(models.Model):
    friends = models.ManyToManyField("self")
```



当django处理这个模型时，它会做如此定义：对多对多字段关系被认为是对称的——即，如果我是你的朋友，那么你也是我的朋友。（~~跟`C++`一比，我是你的友元，你不是我的友元~~）

然而有时候我们不需要这个友好关系，修改`symmetrical`为`False`即可。

```python
symmetrical=Flase
```

