---
title: git的使用
date: 2020-03-18 20:40:59
tags:
	- git
	- 版本控制
	- 分布式
categories: 开发工具
password: 31278792
---
## git的使用
**可以说是一个很牛×的开发工具，是Linux之父开发出来的，据说只花了一周时间，打扰了。git是一个开源的分布式版本控制系统，当时开发目的是为了管理Linux内核开发，毕竟开源项目管理，懒人都会用聪明的方法方便自身偷懒，也体现林纳斯·托瓦兹觉得当时版本控制工具不能满足自身需求而得以git的问世。git的竞争力主要在于它拥有分布式，以及自己的暂存区，可支持离线操作**

<!--more-->

### git的安装
**点击[git](http://git-scm.com/downloads)安装地址进行安装即可，不必赘述。以及[TortoiseGit](https://tortoisegit.org/download/)**

### git的常用命令
**git有很多基于Linux命令涉及，不过下载了TortoiseGit，或者Git自带的Git GUI，也可以不使用git命令，不过我习惯了觉得操作还是蛮方便的**
鼠标右击选择Git Bash进入命令操作界面，再第一次使用git的时候你需要先设置自己的用户名和邮箱地址，当然你可以设置局部配置和全局配置，局部配置则只针对一个指定仓库进行设置你的邮箱和用户名，我是直接全局配置，没必要那么麻烦。
#### 全局配置命令
```
$ git config --global user.name "admin"//设置你的用户名，自己取，建议最好英文，中文我没试过
$ git config --global user.email test@admin.com//设置你的邮箱地址
```
如果是局部配置，只要去掉 ~~--global~~ 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

#### 查看配置信息
```
$ git config --list//检查已有的配置信息所有信息
$ git config user.name//查看你的用户名
$ git config user.email//查看你的邮箱
```

#### 初始化仓库
```
$ git init//当前文件被git接管了
$ git init newrepo//指定仓库为newrepo
```

#### 克隆仓库
```
$ git clone <仓库所在地址>
$ git clone <仓库所在地址> <克隆地址>
```

#### 将该文件添加到缓存
```
$ git add .//添加当前所有为被版本控制的文件入暂存区
$ git add text.txt//添加指定文件到暂存区
```

#### 查看版本库文件项目状态
```
$ git status 
$ git diff//查看执行 git status 的结果的详细信息
```

#### 将缓冲区内容提交至版本库
```
$ git commit -m '第一次版本提交'//-m后是对你本次提交的描述
```

#### 推送到远程仓库
```
$ git push
```

#### 远程仓库拉取更新本地版本库
```
$ git pull
```

#### 日志的查看
```
$ git log
```
*一般与回滚操作关联*

#### 新建分支与分支切换
```
$ git branch (分支名)//创建一支分支
$ git checkout -b (分支名)//切换至指定分支
$ git branch -d (branchname)//删除分支 
```
*当然涉及到分支就会有合并分支，解决冲突的命令，这里不详细说明，我也是半灌水，建议自己看官方文档深入学习*

#### git标签标注
```
$ git tag -a v1.0//就是做记号，觉得这个版本很重要，标记一下，方便以后回退
```

### Git 远程仓库(Github)
**就是使用git将你本地项目提交至github，方便今后的管理，两则建立连接的意思，没必要多说，网上很多参考，唯一要说的最好用SSH密钥连接**

### git相关概要
**git基本包含工作区(本地项目)，暂存区，版本库，以及远程仓库，然后根据命令实现之间的互联互通，版本控制，项目管理。此处该有图，以后再说**

### git服务器搭建
#### 安装git
```
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
$ yum install git
```

#### 创建一个git用户组和用户，用来运行git服务
```
$ groupadd git
$ adduser git -g git
```

#### 创建证书登录
收集所有需要登录的用户的公钥，公钥位于id_rsa.pub文件中，把我们的公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
如果没有该文件创建它：
```
$ cd /home/git/$ mkdir .ssh
$ chmod 700 .ssh
$ touch .ssh/authorized_keys
$ chmod 600 .ssh/authorized_keys
```

#### 初始化仓库
```
$ cd /home
$ mkdir gitrepo
$ chown git:git gitrepo/$ cd gitrepo

$ git init --bare runoob.gitInitialized empty Git repository in /home/gitrepo/runoob.git/

$ chown -R git:git runoob.git//以上命令Git创建一个空仓库，服务器上的Git仓库通常都以.git结尾。然后，把仓库所属用户改为git
```

#### 克隆仓库
```
$ git clone git@192.168.45.4:/home/gitrepo/runoob.gitCloning into 'runoob'...warning: You appear to have cloned an empty repository.Checking connectivity... done.
```
192.168.45.4 为 Git 所在服务器 ip ，你需要将其修改为你自己的 Git 服务 ip。

这样我们的 Git 服务器安装就完成了，接下来我们可以禁用 git 用户通过shell登录，可以通过编辑/etc/passwd文件完成。找到类似下面的一行：
```
git:x:503:503::/home/git:/bin/bash
```

改为：
```
git:x:503:503::/home/git:/sbin/nologin
```


