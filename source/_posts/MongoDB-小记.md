---
title: MongoDB 小记
date: 2019-08-27 16:38:37
tags: ['DataBase', 'MongoDB']
categories: 'DataBase'
---

主要是记录下我学习 MongoDB 的过程和遇到的问题，以供后续的查阅和学习，持续改进吧

<!--more-->

# 安装

这是官网安装页面，[MongoDB Install](https://docs.mongodb.com/manual/installation/#tutorial-installation)。

有企业版和社区版的区分，基本是

我是 mac 系统，有 brew 和二进制文件两种安装方法

顺便说一下，MacOS 因为涉及到权限问题，需要用到 sudo 命令，或是设置 root 账户，然后用 su 命令切换用户

> 附上官方启用 root 账户的说明，[如何在 Mac 上启用 root 用户或更改 root 密码](https://support.apple.com/zh-cn/HT204012)

~~注意安装后需要创建一个数据库数据存储的文件夹~~

~~`mkdir -p /data/db`~~

~~如果运行 `ps -xa | grep mongod`，没有看到一个 `dbpath` 显式地告诉 mongod 查看该数据库位置的参数，并且你的 mongodb.conf 中没有数据库路径，那么默认数据存储位置就是如上~~

Mac 升级为 Catalina 系统后，根目录只有读的权限了，也就是不能在根目录创建 /data/db

两种解决方法

1. 使用 `mongod --dbpath=/Users/user/data/db` 这种，每次都指明数据存放路径的命令启动 mongodb

2. 参考 [stackoverflow](https://stackoverflow.com/questions/58034955/read-only-file-system-when-attempting-mkdir-data-db-on-mac) 第二条回答，使用 brew 启动
   `brew services start mongodb-community`

> 顺便说 `cd /` 命令可以直接进入根目录
> `cd ~` 进入用户根目录

# 使用

## 启动

因为我已经把 mongoDB 的路径加入了系统路径中，并且使用了默认的数据存放位置，所以可以直接，如果没做这两步的话，参考如下

> [MongoDB 配置](https://docs.mongodb.com/guides/server/install/)

    sudo mongod //启动

## 连接

新开一个命令行工具，输入

    mongo

连接数据库，可以输入查询语句了

> 自己的阿里云使用了 docker 版本的 mongodb，所以在这里记录下进入 docker 版本 mongodb 的命令

```bash
docker exec -it mongotest_mongo_1 mongo # docker exec -it #容器名 mongo
```

## 操作部分

还能说啥，重中之重，多多练习

## 用户的授权，创建，修改，查看用户

```bash
# 选择管理员用户
use admin
```

```bash
db.auth('user','pwd')
```

```bash
db.createUser(user:'',pwd:'',roles:[])
# 示例 db.createUser({user:'owner',pwd:'yu321',roles:[{role:'dbOwner',db:'filemanagedb'}]})
```

```bash
show users # 查看当前数据库用户
```

```bash
db.system.users.find() // 查看所有用户
```

> [详情参考此文](https://zhuanlan.zhihu.com/p/26215701)

有时后续需要给用户增加 role，比如新增一个数据库，授权给已存在的用户，可以使用 `grantRolesToUser`

```
> use admin;
> db.grantRolesToUser('user1', ['readWriteAnyDatabase']);
> db.grantRolesToUser('user1', [{ role: 'readWrite', db: 'account' }]);
```

### 查看

查看所有数据库

    > show dbs

查看当前数据库的 collections (集合)

    > show collections

### 创建

创建或进入某个存在的数据库

    > use <name>

### 插入

插入一条数据

    > db.collection.insertOne({})

# 参考链接

- [MongoDB get start](https://docs.mongodb.com/manual/tutorial/getting-started/)
- [小白必须懂的 MongoDB 的十大总结-MongoDB 的认识
  ](https://zhuanlan.zhihu.com/p/44263728)
