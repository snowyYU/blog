---
title: git踩坑记
date: 2017-04-19 18:51:39
tags: git
categories: git
---

总是容易忘记git的一些命令和用法，在此记下我重复google的，哈哈，比较无序，足够多时再分类。

1. git中如果文件已被加入跟踪队列，后续添加相应的.gitignore参数时其相应的改变仍会被监听，采用如下
    git rm --cached XX/xx.x

>附上链接 [git忽略已经被提交的文件](https://segmentfault.com/q/1010000000430426)
>[.gitignore 文件无效的解决方法](http://www.ifeegoo.com/git-code-management-dot-gitignore-file-has-no-effect-solution.html)