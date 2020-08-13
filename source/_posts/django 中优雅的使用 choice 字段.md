---
title: django 中优雅的使用 choice 字段
date: 2020-03-27 14:30:09
updated: 2020-03-27 14:30:09
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
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813190953.png
---

# django 中优雅的使用 choice 字段

# 问题

`django`中如何比较优雅的对元组进行标记分类。可使用`choice`字段

# choice字段

```python
# models.py
class BookTagNum(object):
    OTHER = 1
    SCIENCE = 2
    SOCIAL_SCIENCES = 3
    ECONOMIC = 4
    COMPUTER = 5

class BOOK(models.Model):
    TAG_NUM_CHOICE = (
        (BookTagNum.OTHER, '其它'),
        (BookTagNum.SCIENCE, '科学类'),
        (BookTagNum.SOCIAL_SCIENCES, '社科类'),
        (BookTagNum.ECONOMIC, '经济类'),
        (BookTagNum.COMPUTER, '计算机类'),
    )
    tag = models.IntegerField(choices=TAG_NUM_CHOICE)
```

在代码中尽量不要出现固定的硬编码，比如某个判断条件，判断书的分类为：

```python
#view.py
def get(self, request):
	book = Book.obejects.filter(tag = BookTagNum.COMPUTER)
```

