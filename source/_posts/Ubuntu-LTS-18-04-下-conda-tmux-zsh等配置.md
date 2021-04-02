---
title: Ubuntu LTS 18.04 下 conda tmux zsh等配置
date: 2020-02-23 00:36:28
updated: 2020-02-23 00:36:28
tags:
    - Ubuntu
    - conda
    - Linux
    - tmux
    - zsh
categories:
    - Linux
    - Ubuntu
keywords:
    - Ubuntu
    - conda
    - Linux
    - tmux
    - zsh
description:
cover: https://blog-img-lei.oss-cn-beijing.aliyuncs.com/img/20200813221115.png
---

# 一个新的Ubuntu LTS 18.04 conda tmux zsh等配置
最近阿里云给出免费6个月2H4G服务器活动，领了一个并简单配置了一下。
相较于apt-get更推荐apt，它集合了apt-get，更新，更便捷。
# 用户
## 创建新用户
```shell
sudo adduser xxx #创建用户
sudo userdel xxx #删除用户
```
## 添加管理员权限
首先：
```shell
vi /etc/sudoers
```
```shell
"root ALL=(ALL) ALL" 在起下面添加 "xxx ALL=(ALL) ALL" (这里的 xxx 是你的用户名)，然后保存退出。
```
## 修改主机名
+ 首先修改`/etc/cloud/cloud.cfg`
```shell
sudo vim /etc/cloud/cloud.cfg
#找到preserve_hostname: false修改为preserve_hostname: true
```
```shell
#修改主机名
sudo vim /etc/hostname
#然后改为需要的主机名后存盘退出

#映射主机名（可选，因为域名只对应IP，和主机无关）
sudo vim /etc/hosts
#192.168.1.xxx 主机名
sudo reboot
```
+ `sudo reboot`

# 系统
## 软件升级
```shell
sudo apt update: # 升级安装包相关的命令,刷新可安装的软件列表(但是不做任何实际的安装动作)
sudo apt upgrade: # 进行安装包的更新(软件版本的升级)
sudo apt dist-upgrade: # 除了拥有upgrade的全部功能外，dist-upgrade会比upgrade更智能地处理需要更新的软件包的依赖关系。
sudo do-release-upgrade: # 进行系统版本的升级(Ubuntu版本的升级)，Ubuntu官方推荐的系统升级方式,若加参数-d还可以升级到开发版本,但会不稳定
```

## zsh
### 安装并替换
```shell
sudo apt install zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
# 设置成默认shell
chsh -s /bin/zsh
reboot
```
### 主题
```shell
vim ~/.zshrc
# 我常用 "ys"
```
### 插件
```shell
cd ~/.oh-my-zsh/custom/plugins
# 高亮插件
git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
# 代码提示
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 配置
sudo vim ~/.zshrc
plugins=(
    git
    zsh-syntax-highlighting
    zsh-autosuggestions
)
# save
source ~/.zshrc

# 配置conda
export PATH=~/anaconda3/bin:$PATH
```
## github 速度太慢？
[Github下载速度太慢怎么办？完美解决](https://yq.aliyun.com/articles/713169)
[git clone速度太慢解决方案](https://blog.csdn.net/hzwwpgmwy/article/details/79043251)

## 刷新dns
Linux刷新dns的缓存方法是：
```shell
sudo /etc/init.d/nscd restart
```
如果发现提示命令找不到：
```shell
sudo: /etc/init.d/nscd: command not found
```
后来发现是需要先安装nscd包：
```shell
sudo apt install nscd
```
最暴力的方法刷dns，重启网络：
```shell
sudo /etc/init.d/networking restart
```
## conda
使用当前用户安装即可, 按情况换源， 实测阿里云不换源体验很好


[清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

```shell
升级conda(升级Anaconda前需要先升级conda)：conda update conda 
升级anaconda：conda update anaconda 
升级spyder：conda update spyder
更新所有包：conda update --all
安装包：conda install package
更新包：conda update package
```
### 去掉(base)
[安装conda后终端出现的(base)字样去除方法](https://www.jianshu.com/p/6cdc9713c4ed)
## tmux
```shell
sudo apt install tmux
tmux new -s session_name
tmux attach -t session_name
```
### 配置鼠标
下面是配置文件内容，在家目录下创建.tmux.conf，并粘贴下面内容保存后，进入tmux， ctrl+b，然后输入命令：source-file ~/.tmux.conf 即可。

步骤：
```shell
vim ~/.tmux.conf
# 加入
set-option -g mouse on
```

`ctrl+b :`
```shell
source-file ~/.tmux.conf
```

## 解决git每次push都需要输入用户名和密码
再用户根目录下输入即可
```shell
git config --global credential.helper store
```

## 配置免密登入
+ 在win系统下找到用户目录的`.ssh`文件夹，将`id_rsa.pub`复制一份命名为`authorized_keys`
+ 将`authorized_keys`发送到ubuntu 根目录下的 `.ssh`中，若没有，则创建。

[ssh免密登陆失败原因总结（Linux）](https://blog.csdn.net/zhangmingcai/article/details/95734889)

