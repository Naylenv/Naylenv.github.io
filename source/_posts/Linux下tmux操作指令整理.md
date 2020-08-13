---
title: Linux下tmux操作指令整理
date: 2020-02-12 01:27:52
updated: 2020-02-12 01:27:52
tags:
    - tmux
    - Linux
categories:
    - Linux
keywords:
    - tmux
    - Linux
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813221413.png
---

# tmux
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
参考下面的文章
[tmux使用手记](`https://blog.csdn.net/lell3538/article/details/82150733`/)