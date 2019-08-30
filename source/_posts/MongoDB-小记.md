---
title: MongoDB 小记
date: 2019-08-27 16:38:37
tags: ["DataBase","MongoDB"]
categories: "DataBase"
---

主要是记录下我学习MongoDB的过程和遇到的问题，以供后续的查阅和学习，持续改进吧

<!--more-->

# 安装 #

这是官网安装页面，[MongoDB Install](https://docs.mongodb.com/manual/installation/#tutorial-installation)。

有企业版和社区版的区分，基本是

我是mac系统，有 brew 和二进制文件两种安装方法

顺便说一下，MacOS 因为涉及到权限问题，需要用到 sudo 命令，或是设置 root 账户，然后用 su 命令切换用户

>附上官方启用 root 账户的说明，[如何在 Mac 上启用 root 用户或更改 root 密码](https://support.apple.com/zh-cn/HT204012)

注意安装后需要创建一个数据库数据存储的文件夹

```
mkdir -p /data/db
```
如果运行 `ps -xa | grep mongod`，没有看到一个 `dbpath` 显式地告诉mongod查看该数据库位置的参数，并且你的mongodb.conf中没有数据库路径，那么默认数据存储位置就是如上

>顺便说 `cd /` 命令可以直接进入根目录

# 使用 #

## 启动 ##
因为我已经把mongoDB的路径加入了系统路径中，并且使用了默认的数据存放位置，所以可以直接，如果没做这两步的话，参考如下
>[MongoDB配置](https://docs.mongodb.com/guides/server/install/)

    sudo mongod //启动
## 连接 ##
新开一个命令行工具，输入

    mongo

连接数据库，可以输入查询语句了

## 操作部分 ##

还能说啥，重中之重，多多练习

### 查看 ###
查看所有数据库

    > show dbs
### 创建 ###

创建或进入某个存在的数据库

    > use <name>


# 参考链接 #

- [MongoDB get start](https://docs.mongodb.com/manual/tutorial/getting-started/)
- [小白必须懂的MongoDB的十大总结-MongoDB的认识
](https://zhuanlan.zhihu.com/p/44263728)