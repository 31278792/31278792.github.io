---
title: Linux常用命令
date: 2022-02-26 22:34:23
tags: 
	- Linux
	- 命令
categories: Linux
password: 31278792
---
# Linux常用命令
## 文件处理命令
### 目录处理命令
`ls -a(显示所有文件，包括隐藏文件) -l(详细信息显示) -d(查看目录属性) -i(查看文件唯一标识ID) -lh`

`ls -l`查询出来结果首项，如果：  
`-`打头表示文件  
`d`打头表示目录  
`l`打头表示软链接  
>-rw-r--r--  
>   u g o  
>   u所有者 g所属组 o其他人  
>   r读 w写 x执行

`mkdir -p(递归创建)`  
`cd /tmp/bin(切换到指定目录) ..(回到上一级目录)`  
`pwd(查看当前所在目录)`  
`rmdir(删除空目录)`  
`cp -rp -r(复制目录) -p(保留文件属性) 复制文件不需要加选项r`
`mv(剪切、改名)`
`rm -rf(文件或目录) -r(删除目录) -f(强制) -i(询问是否删除)`

<!--more-->
### 文件处理命令
`touch(创建文件)`  
`cat(显示文件内容) -n(显示行号)`  
`tac(倒显文件内容)`  
`more(分页显示文件内容，空格或f进行翻页，回车进行换行，q退出)`  
`less(可以向上翻页) pgup向上翻页，用/可以搜索关键词，n查看下一个匹配关键词`  
`head(显示文件前面10行内容) -n(指定显示行数)`
`tail(显示文件末尾10行内容) -n(指定显示行数) -f(动态实时显示)`

### 链接命令
`ln(生成硬链接) -s(创建软链接)`  
**软链接相当于win系统下的快捷方式，软链接文件权限都是rwx**  
**硬链接相当`cp-p+同步更新`，可以通过`ls-i`节点识别硬链接，硬链接不能跨分区，硬链接不能针对目录来设置**  
           
## 权限管理命令
**一个文件权限，只能是文件所有者或root管理员可以更改**
### 权限管理命令chmod
`chmod(改变文件或目录权限 ) [{ugoa}{+-=}{rwx}][文件或目录][mode=42][文件或目录] -R(递归修改)`  

**示例：**
>`chmod u+x hello.list`给hello.list文件所有者增加x执行权限
>`chmod g=rwx yzw.java`给yzw.java文件所属组赋予rwx权限  
>`chmod g=rwx,o-r yzw.java`修改多个权限可以用逗号分隔

**权限的数字表示：r用4表示，w用2表示，x用1表示，来实现权限修改**
>`rwxrw-r--`对应7 6 4
>`r-x-wx-w-`对应5 3 2
>`chmod 640 yzw.java`通过数字表示实现了yzw.java文件权限更改

| 代表字符 | 权限   | 对文件的含义    | 对<font color="red">目录</font>的含义|
| :-----:   | :---: |  :-------:     | :-------:           |
| **r**   | 读权限 | 可以查看文件内容 | 可以列出目录中的内容     |
| **w**   | 写权限 | 可以修改文件内容 | 可以在目录中创建、删除文件 |
| **x**   | 执行权限 | 可以执行文件   | 可以进入目录            |

### 其他权限管理命令
`chown(改变文件或目录的所有者，只有管理员root可以操作) [用户][文件或目录]`
`chgrp(改变文件或目录的所属组) [用户组][文件或目录]`
`umask(显示、设置文件的缺省权限) -S(以rwx形式显示新建文件缺省权限)`

## 文件搜索命令
### 文件搜索命令find
`find(执行权限：所有用户) [搜索范围][匹配条件]`  
**示例：**
>`find /etc -name init`(在etc目录下面搜索名字为init的文件或目录，注意：这里是精准查询)  
>`find /etc -name *init*`(模糊查询，这里`*`表示任意字符)  
>`find /etc -name init*`(找到init开头的文件或目录)  
>`find /etc -name init???`(找到init后面三个字符的文件或目录，这里?表示单个字符)  
>`find -iname(不区分大小写) init`  
>`find -size(根据文件大小查找) +204800`(+n表示大于，-n表示小于，n表示等于，这里204800换算：204800数据块=102400KB=100MB)  
>`find /home -user yzw`(找所有者yzw在home目录的所有文件)  
>`find /etc -cmin -5`(在etc目录下查找5分钟内被修改过属性的文件和目录，`-amin`表示访问时间access，`-cmin`表示文件属性，`-mmin`表示文件内容)  
>`find -size +163840 -a -size -204800`(在etc下查找大于80MB小于100MB的文件，`-a`是and的意思)  
>`find -size +163840 -o -size -204800`(`-o`是or的意思)
>`find /etc -name init* -a -type f`(查看etc目录下名以init开头的文件，`f`表示文件，`d`表示目录，`l`表示软链接)  
>`find /etc -name yzw -exec ls -l {} \;(在etc目录下查找yzw文件并显示其详细信息)`  
>`find /home -user jack -ok rm {} \;`(`-ok`对结果多了询问)  
>`find -inum 10086 -exec rm {} \;`(根据i节点查找到ID为10086文件进行删除)  

### 其他搜索命令
`locate(在文件资料库里面查找) -i(不区分大小写)`
`updatedb`(更新文件资料库)
`which`(搜索命令所在目录及别名信息)
`whereis`(搜索命令所在目录及帮助文档路径)
`grep(在文件内容中查找) -i(不区分大小写) -v(排查指定字符串)`
`grep -v ^# /etc/inittab`(排除以#开头的注释)

## 帮助命令
`man`(获取帮助信息)  
`man 5 passwd`(查看配置文件帮助文档，1表示命令，5表示配置文件，默认查看命令帮助文档)  
`whatis`(查看命令简单信息)  
`apropos`(查看配置文件简单信息)  
`ls --help`(查看常见选项)  
`info`(同`man`差不多)  
`help`(查看内置命令帮助信息)

## 用户管理命令
`useradd`(添加新用户)  
`passwd`(新增或修改密码，用户只能更改自身密码，管理员root可以更改任何用户密码)  
`who`(查看登录用户信息,tty表示本地终端，pts表示远程终端)  
`w`(查看登录用户详细信息)

## 压缩解压命令
`gzip`(压缩成.gz文件且不保留原文件，不能压缩目录)  
`gunzip或gzip -d`(解压.gz文件)  
`tar(打包目录) -zcvf [压缩后文件名][目录]`(压缩格式.tar.gz)  
>`-c`打包  
>`-v`显示详细信息  
>`-f`指定文件名  
>`-z`打包同时压缩  
  
`tar -zxvf`(解压)
>`-x`解包  
>`-v`显示信息  
>`-f`指定解压文件  
>`-z`解压缩  

`zip -r(压缩目录)[压缩后文件名][文件或目录]`(能保留原文件，压缩格式.zip)  
`unzip`(解压.zip文件)  
`bzip2 -k(保留原文件)`(gzip升级版可以保留原文件，压缩格式.bz2)  
`tar -cjf`(将z换成j可以生成.tar.bz2压缩格式)，例:`tar -cjf yzw.tar.bz2 yzw`   
`bunzip2或bzip -d`(解压.bz2)  
`tar -xjf`(解压.tar.bz2)  

## 网络命令
`write`(给指定用户发信息，以Ctrl+D保存结束)  
`wall(write all，给所有用户发送信息)`  
`ping -c(指定发送次数)`(测试网络连通性)  
`ifconfig`(查看和设置网卡信息)，例:`ifconfig eth0 192.168.8.8`(临时更改ip)  
`mail`(查看发送电子邮件)  
`last`(列出目前与过去登入系统的用户信息)  
`lastlog -u uid`(检查某特定用户上次登录时间，不加选项列出所有用户)  
`traceroute`(跟踪路由，即显示数据包到主机间的路径)，例：`traceroute www.baidu.com`  
`netstat(显示网络相关信息) -t(查询TCP协议) -u(查询UDP协议) -l(查看监听端口) -r(查看路由网关) -n(显示ip地址和端口号)`  
>netstat -tlun 查看本机监听的端口  
>netstat -an 查看本机所有网络连接  
>netstat -rn 查看本机路由表  

`setup`(配置网络，redhat专有命令)  
`mount -t`(挂载命令，即分配盘符)例：`mount -t iso9660 /dev/sr0 /mnt/cdrom`
`umount`(挂载卸载命令)

## 关机重启命令
`shutdown -h now`(立刻关键)
`shutdown -h 20:30`(20:30关机)
`shutdown -r now`(立刻重启)
`shutdown -r 20:30`(20:30重启)
`shutdown -c`(取消前一个关机命令)
`halt`(关机命令)
`poweroff`(关机命令)
`init 0`(关机命令)
`reboot`(重启命令)
`init 6`(重启命令)
>**系统运行级别：0代表关机 1代表单用户 2代表不完全多用户，不含NFS服务 3代表完全多用户 4代表未分配 5代表图形界面 6代表重启**

`runlevel`(查询当时运行级别)
`logout`(退出登录)