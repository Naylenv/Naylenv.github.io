---
title: 快速创建一个Django项目并进行相应配置
tags:
  - Django
categories:
  - Django
keywords:
  - Django
date: 2020-11-09 16:34:43
updated: 2020-11-09 16:34:43
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402170824.png
---


# start a project
## django 基本命令
+ 输入以下命令新建一个django project
```shell
django-admin startproject xxx
```
+ 新建一个app
```shell
python manage.py startapp polls
```
+ 创建超级用户
```shell
python manage.py createsuperuser
```
+ 删除数据库
```shell
rm -f db.sqlite3
rm -r snippets/migrations
python manage.py makemigrations snippets
python manage.py migrate
```
## msql
### windows django2.0
```
pip install mysql-connector-python
```

```python
DATABASES = {
    'default': {
        'ENGINE': 'mysql.connector.django',   # 数据库引擎
        'NAME': 'django_project',  # 数据库名，先前创建的
        'USER': 'lll',     # 用户名，可以自己创建用户
        'PASSWORD': '123456',  # 密码
        'HOST': '127.0.0.1',  # mysql服务所在的主机ip
        'PORT': '3306',       # mysql服务端口
    }
}

```
### windows django3.0
把mysqlclient 更新到最新版，用Linux的配置就可以
```
pip install mysqlclient==1.4.6 
```
### linux
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',   # 数据库引擎
        'NAME': 'darwin',  # 数据库名，先前创建的
        'USER': 'lll',     # 用户名，可以自己创建用户
        'PASSWORD': '123456',  # 密码
        'HOST': '127.0.0.1',  # mysql服务所在的主机ip
        'PORT': '3306',       # mysql服务端口
    }
}
```
### 一些方法
+ `.is_valid()`
## rest framework
```shell
pip install djangorestframework
pip install markdown       # Markdown support for the browsable API.
pip install django-filter  # Filtering support
```
web api 框架
```shell
超链接关系 很好的RESTful设计 -- HyperlinkedModelSerializer
```
## 分页器
分页器允许你控制每页返回的数据条数。添加以下配置到 `tutorial/settings.py` 中使之生效
```shell
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```
产生如下效果：
```shell
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "email": "admin@example.com",
            "groups": [],
            "url": "http://127.0.0.1:8000/users/1/",
            "username": "admin"
        },
        {
            "email": "tom@example.com",
            "groups": [                ],
            "url": "http://127.0.0.1:8000/users/2/",
            "username": "tom"
        }
    ]
}
```


## xadmin
替换掉admin的一种选择

## Mysql
### 进入数据库
```shell
sudo mysql -u root -p
```

```shell
##1 创建数据库weixx
CREATE DATABASE darwin CHARACTER SET utf8;
##2 创建用户wxx(密码123456) mysql 5.7
并允许wxx用户可以从任意机器上登入mysql的weixx数据库
    GRANT ALL PRIVILEGES ON darwin.* TO anadem@"%" IDENTIFIED BY "123456"; 
## mysql 8.0
create user 'anadem'@'%' identified by '123456';
grant all privileges on *.* to 'anadem'@'%';
```

### 数据库迁移
```shell
python manage.py dumpdata > data.json
```
```shell
python manage.py loaddata data.json
```
## 跨域
百度吧，挺多的。