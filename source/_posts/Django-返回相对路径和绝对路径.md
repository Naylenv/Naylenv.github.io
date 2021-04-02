---
title: Django 返回相对路径和绝对路径
tags:
  - Django
categories:
  - Django
keywords:
  - Django
date: 2021-04-02 16:58:27
updated: 2021-04-02 16:58:27
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402171222.png
---

# imagefiled
```python
def user_directory_path(instance, filename):
    # file will be uploaded to MEDIA_ROOT/user_<id>/<filename>
    return 'user_{0}/{1}'.format(instance.user.id, filename)

class MyModel(models.Model):
    image = models.FileField(upload_to=user_directory_path)
```
+ `1image.url` 返回相对路径
+ `image.path` 返回绝对路径