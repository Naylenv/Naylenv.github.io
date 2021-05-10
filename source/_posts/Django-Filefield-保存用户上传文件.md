---
title: Django Filefield 保存用户上传文件
tags:
  - Django
categories:
  - Django
keywords:
  - Django
date: 2020-02-25 23:51:35
updated: 2020-02-25 23:51:35
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210510171342.png
---
# Django Filefield 保存用户上传文件
网上关于`Django` `Filefield` 的文章很少。
今天踩了踩坑，给了一套`Filefiled`上传保存文件的方法。
跟一般web开发一样，上传的文件保存在请求体的某个字段中，通常为`file`字段
在`views.py`中，可以这样获得上传的文件

```python 
# view.py
def post(self, request):
	avatar = request.FILES.get("file")
```
这样`avatar`就存储了上传的文件，保存其实有很简单的方法，`django`替你封装好了：
```python
# view.py
def post(self, request):
    import datetime
    user.avatar.save("{}_{}.jpg".format(user.id,  datetime.datetime.now().strftime('%Y-%m-%d')), avatar)
    user.save()
```
这里顺便对保存的数据进行了格式转化，调用了`python`的`datatime`包，当然调用`time`包或者`Django`的`timezone`包也是一样的。