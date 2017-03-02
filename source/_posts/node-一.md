---
title: node(一)
date: 2017-03-01 20:31:32
tags: node
categories: node
---
开始正式学习node，之前看过node即学即用，敲了遍书上的代码，有点晕晕的，之后很长时间没接触node了，现在下决心每天花些时间捣鼓下。
<!--more-->
# 写在前面 #
在这里主要是记笔记，按照教程尝试搭建使用 Express + MongoDB 搭建多人博客，记录些要点和体会
>参考了[使用 Express + MongoDB 搭建多人博客](https://github.com/nswbmw/N-blog)
# 安装配置环境 #

安装nodejs，LTS是长期支持版本，current是最新版，支持最新语言特性(比如更全的es6)

n和nvm是两个常用的nodejs版本管理工具，nvm不支持windows
>参考[这个](http://taobaofed.org/blog/2015/11/17/nvm-or-n/)
>还有阮一峰老师的[es6入门](http://es6.ruanyifeng.com/#docs/intro)

nrm管理npm源的工具，目测修改了源之后npm==cnpm了

安装mongoDB遇到一个坑，执行

     "C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --dbpath F:\practice\mongo

报错

     $ "C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --dbpath F:\practice\mongo
    17-03-02T14:55:01.854+0800 I CONTROL  [initandlisten] MongoDB starting :     d=7440 port=27017 dbpath=F:practicemongo 64-bit host=DESKTOP-KP92M3V
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] targetMinOS: Windows 7/    ndows Server 2008 R2
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] db version v3.4.2
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] git version:     76e40c105fc223b3e5aac3e20dcd026b83b38b
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] OpenSSL version:     enSSL 1.0.1u-fips  22 Sep 2016
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] allocator: tcmalloc
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] modules: none
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] build environment:
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten]     distmod:     08plus-ssl
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten]     distarch: x86_64
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten]     target_arch: x86_64
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] options: { storage: {     Path: "F:practicemongo" } }
    17-03-02T14:55:01.855+0800 I STORAGE  [initandlisten] exception in     itAndListen: 29 Data directory F:practicemongo not found., terminating
    17-03-02T14:55:01.855+0800 I NETWORK  [initandlisten] shutdown: going to     ose listening sockets...
    17-03-02T14:55:01.855+0800 I NETWORK  [initandlisten] shutdown: going to     ush diaglog...
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] now exiting
    17-03-02T14:55:01.855+0800 I CONTROL  [initandlisten] shutting down with     de:100

说F:practicemongo not found，不知道为什么会这样，直接执行

     "C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe"
又说 F:\data\db\ not found.

我就只好在F盘下新建data\db，然后就成功了

打开Robomongo,[Robomongo](http://blog.robomongo.org/) 是一个基于 Shell 的跨平台开源 MongoDB 可视化管理工具，支持 Windows、Linux 和 Mac，嵌入了 JavaScript 引擎和 MongoDB mongo，

>试着插入一条数据结果不知道怎样看不到了