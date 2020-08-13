---
title: Django数据库操作 —— 干净的重置migration
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
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813191052.png
---

# 前言

随着项目需求的增加：

+ Django的迁移文件越来越大，
+ 并且遇到models文件中如果使用了自定义存储字段。不再使用后删除会报错的情况。

重置迁移文件后解决了上述问题。

## 情景一：不需要原有的数据库数据

1. 首先删除数据库中的相关APP下的数据表
2. 然后删除APP下的migration模块中的所有 文件，除了init.py 文件
3. 执行下面的命令

```	handlebars
python manage.py makemigrations
python manage.py migrate
```

## 情景二：不想要删除现有的数据库，只是想重新建立 migration 文件

这个情况是开发中最为常见的，也是操作起来稍微复杂一点的情况，但是只要遵循下面的操作步骤，就不会引发任何错误。

1. 首先要保证,目前的migration文件和数据库是同步的，通过执行

```handlebars
python manage.py makemigrations
```

2.查看当前项目下所有APP对应的已经生效的（已经成功执行的）migration文件，命令如下:

```handlebars
python manage.py showmigrations
```

结果如下图所示:

```handlebars
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
api
 [X] 0001_initial
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
explore
 [X] 0001_initial
 [X] 0002_auto_20200223_0131
 [X] 0003_delete_exphoto
 [X] 0004_exphoto
 [X] 0005_auto_20200304_1652
 parent
 [X] 0001_initial
 [X] 0002_auto_20200409_2248
```

3. 重置你的APP的操作，使它们恢复到没有执行的状态，这里注意一下fake前面的符号，是两个“-”，另外，explore 是APP的名字。

```handlebars
python manage.py migrate --fake explore zero
```

重装完后进行检查：

```handlebars
python manage.py showmigrations
```

如果是要重置的APP前面[x]变成了[ ]则操作正确：

```handlebars
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
api
 [X] 0001_initial
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
explore
 [ ] 0001_initial
 [ ] 0002_auto_20200223_0131
 [ ] 0003_delete_exphoto
 [ ] 0004_exphoto
 [ ] 0005_auto_20200304_1652
 parent
 [X] 0001_initial
 [X] 0002_auto_20200409_2248
```

**这里要注意，如果有其它数据库的状态有[x]变成了[ ]，则该APP也要重置（因为外键的原因）** 如：

```handlebars
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
api
 [X] 0001_initial
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
explore
 [ ] 0001_initial
 [ ] 0002_auto_20200223_0131
 [ ] 0003_delete_exphoto
 [ ] 0004_exphoto
 [ ] 0005_auto_20200304_1652
 parent
 [X] 0001_initial
 [ ] 0002_auto_20200409_2248
```

这里parent出现了[ ]，则也应该重置，否则会报错。

4. 然后放心大胆地删除`migrations`文件夹下面，**除了`__init__.py`文件**，的所有的带有序号的`.py`文件，包括`pycache`文件夹！
5. 执行下面的命令，再次为这个APP 生成 0001_initial.py 之类的文件

```handlebars
python manage.py makemigrations
```

6. 最后执行下面的命令，使刚刚生成的`0001_initial.py`文件记录到`django_migrations`数据表中，大功告成。

```handlebars
python manage.py migrate --fake-initial
```

# 参考资料：
[Django开发—如何重置migration](https://blog.csdn.net/zhuoxiuwu/article/details/52167599)
[Django笔记05：如何悄悄删除migrations下的文件而不引起任何错误](https://blog.csdn.net/gaifuxi9518/article/details/86591806)