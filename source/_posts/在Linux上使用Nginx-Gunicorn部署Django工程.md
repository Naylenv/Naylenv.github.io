---
title: 在Linux上使用Nginx + Gunicorn部署Django工程
tags:
  - Django
categories:
  - Django
keywords:
  - Django
date: 2020-11-09 16:36:39
updated: 2020-11-09 16:36:39
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20210402171122.png
---

# tmux + start django
## tmux
```shell
tmux new -s session_name # 创建名为 session_name 的 tmux session
tmux attach -t session_name # 重新回到叫做 session_name 的 tmux session
tmux switch -t session_name #  切换到叫做 session_name 的 tmux session
tmux list-sessions / tmux ls # 列出现有的所有 session
tmux detach # 离开当前开启的 session
tmux kill-server # 关闭所有 session
```
```shell
ctrl + b
? 列出所有快捷键；按q返回
d 脱离当前会话,可暂时返回Shell界面
s 选择并切换会话；在同时开启了多个会话时使用
D 选择要脱离的会话；在同时开启了多个会话时使用
: 进入命令行模式；此时可输入支持的命令，例如 kill-server 关闭所有tmux会话
[ 复制模式，光标移动到复制内容位置，空格键开始，方向键选择复制，回车确认，q/Esc退出
] 进入粘贴模式，粘贴之前复制的内容，按q/Esc退出
~ 列出提示信息缓存；其中包含了之前tmux返回的各种提示信息
t 显示当前的时间
ctrl + z 挂起当前会话
```
## nginx
```shell
sudo systemctl status nginx
```

## gunicorn 部署
### nginx 收集静态文件
```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```
### gunicorn 部署
```shell
pip  install gunicorn
```
```shell
conda run gunicorn darwinproject.wsgi -w 2 -k gthread -b 0.0.0.0:8000
```
收集静态文件
```shell
python manage.py collectstatic
```
