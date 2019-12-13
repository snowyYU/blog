---
title: Linus 相关命令
date: 2019-12-06 15:35:29
tags: [Linus]
---

需要用到的时候常常忘记，与其每次去Google还不如自己好好做下记录，至少不用去Google了吧，哈哈

<!--more-->

# 文档型：文件相关命令 #

>touch,cat,echo,rm,vi,cd

## cd ##

`cd` 命令就不用多说了，进入某个目录下

* `cd /` 为进入根目录下
* `ls` 列出当前目录下所有文件名称

## mkdir ##
创建一个目录

## touch ##

创建一个文件，注意区别 `mkdir` 命令
## cat ##
查看文件内容

> 比如 `cat test.txt`

## echo ##
向文件中添加或覆盖内容

`echo 1234 >> test.txt` 两个箭头表示向内容末尾添加内容

`echo 1234 > test.txt` 一个箭头表示覆盖内容

## rm ##
删除文件
* 删除目录使用 `rm -r` 命令
> 注意 `rm -rf` 是强制删除命令，慎用，会造成误删后无法恢复的问题

## vi ##

用 **vim** 编辑器编辑文件

# 功能型：压缩/解压，下载，远程 #
> wget, tar, ssh

## wget ##

下载文件，命令后跟资源地址
## tar ##

### 解压文件， 命令后跟文件名 ###

**tar zxvf** 参数解释
* **z** 用来解压 **.gz**后缀的文件，比如 **apache-tomcat-9.0.17.tar.gz**
* **x** 代表解压缩
* **v** 代表显示所有的解压过程
* **f** 表示解压后的文件使用压缩包文件的名字，比如 
* **apache-tomcat-9.0.17.tar.gz** 使用 **zxvf** 参数解压后的文件名为 **apache-tomcat-9.0.17**

### 压缩文件 ###
**tar zcvf** 参数解释，**c** 代表压缩，其余解释和上文一致
> 需要注意，**tar zcvf** 后跟两个参数，第一个参数为压缩后的文件名，第二个参数为需要被压缩的文件，例如把 **apache-tomcat-9.0.17** 压缩成 **apache-tomcat.tar.gz** 使用 `tar zcvf apache-tomcat.tar.gz apache-tomcat-9.0.17`

# ssh #

远程连接，看例子 `ssh root@119.21.241.41`

> `ssh -p 10229 root@119.21.241.41`  

有几个相关的小点需要注意
* `cat /etc/hostname` 查看主机名，可用vim修改
* `cat /etc/ssh/sshd_config` 查看 **ssh** 服务的配置，可用vim修改
* `netstat -anlp | grep sshd` 查看 **ssh** 服务占用的端口号

# 系统进程服务相关 #
## 查看当前系统进程 ##

**ps -ef | grep** , 比如
> `ps -ef | grep docker` 查看名为 **docker** 的进程

## kill 某进程 ##

**kill -9**，比如
> `kill -9 27643` 强制杀死id为27643的进程

## 系统服务相关 ##
### 查看系统服务 ###
**service 服务名 status**, 比如 
>`service sshd status` :查询 ssh 服务的运行状态

### 关闭某个服务 ###

**service 服务名 stop** 
### 重启某个服务 ###

**service 服务名 restart**