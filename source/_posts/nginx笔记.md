---
title: nginx笔记
date: 2020-10-23 15:09:13
tags: nginx
categories: nginx
---

像自己的 git 笔记一样，记录下自己安装以及使用的过程与心得，方便之后的查看

<!--more-->

## 安装

使用 **brew** 安装

```bash
brew install nginx
```

## 启动

```bash
nginx
```

> 正常情况下执行完此命令后是看不到什么输出的 （实际上已经执行成功了）

![nginxStart.png](http://ww1.sinaimg.cn/large/40c136bfgy1gk3p0cgqt2j20g7037t9x.jpg)

启动后可通过如下命令控制服务器的状态

```bash
nginx -s signal
```

**signal**的值可能如下

- stop — fast shutdown
- quit — graceful shutdown
- reload — reloading the configuration file
- reopen — reopening the log files

> 使用 quit 参数会在主线程处理完当前请求后停止 nginx 线程

每次修改完配置文件后需要重新执行 reload 命令

## 配置

在 mac 系统中，相关的几个路径

- `/usr/local/etc/nginx/nginx.conf` （nginx 配置文件路径）
- `/usr/local/var/www` （nginx 服务器默认的根目录）
- `/usr/local/Cellar/nginx/1.17.9` （nginx 的安装路径）
- `/usr/local/var/log/nginx/error.log` （nginx 默认的日志路径）

**nginx**配置部分是重中之重

这里举一个例子，下面的配置了服务器的 监听端口，重定向位置，代理服务

```nginx
server {
    listen       8089;
    server_name  localhost;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        root   html/web-mobile;                #入口目录
        index  index.html index.htm;
    }

    location /api {
        proxy_pass http://192.168.0.235:8091;  #代理的目标服务器
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## tips

- **nginx** 有一个主线程和若干个工作线程

## about

- [beginner's guide](https://nginx.org/en/docs/beginners_guide.html)
- [beginner's guide 翻译](https://blog.imkasen.com/nginx-beginner-guide-zh.html)
