---
title: docker入门
date: 2020-06-29 16:11:52
tags: docker
---

记录些安装，配置，和使用上的一些心得，方便日后查阅

<!--more-->

# 安装 #

## 在centOS上安装 ##

mac 上安装的话直接有相应的安装包，很简单，现在主要说下服务端的，流程大致可以参考官方 [docker install](https://docs.docker.com/engine/install/centos/)，不过要注意，我在centos8 上执行到 `sudo yum install docker-ce docker-ce-cli containerd.io` 命令报错（应该是containerd.io版本问题）。

此时需要先执行 

`dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm` 

先安装 containerd.io。

再执行 `sudo yum install docker-ce docker-ce-cli` ,`sudo systemctl start docker` 安装剩下两个

## 配置国内镜像 ##

编辑 `sudo vi /etc/docker/daemon.json`
加入如下
```json
{
    "registry-mirrors": [
        "https://1nj0zren.mirror.aliyuncs.com",
        "https://docker.mirrors.ustc.edu.cn",
        "http://f1361db2.m.daocloud.io",
        "https://registry.docker-cn.com"
    ]
}
```

> 好像 `"https://registry.docker-cn.com"` 失效了

# compose #

参考官方安装文档 [compose install](https://docs.docker.com/compose/install/)


1. 下载安装  

`sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
2. 给权限 `sudo chmod +x /usr/local/bin/docker-compose`
3. 测试是否安装成功 `docker-compose --version`