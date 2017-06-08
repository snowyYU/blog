---
title: git踩坑记
date: 2017-04-19 18:51:39
tags: git
categories: git
---

总是容易忘记git的一些命令和用法，在此记下我重复google的，哈哈，比较无序，足够多时再分类。
<!--more-->

1. git中如果文件已被加入跟踪队列，后续添加相应的.gitignore参数时其相应的改变仍会被监听，采用如下

    git rm --cached XX/xx.x

>附上链接
>[git忽略已经被提交的文件](https://segmentfault.com/q/1010000000430426)
>[.gitignore 文件无效的解决方法](http://www.ifeegoo.com/git-code-management-dot-gitignore-file-has-no-effect-solution.html)

2. 常见的git删除

    git rm XXX.xx

如果误删了可采用`git checkout`
>`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区修改还是删除，都可以使用此还原

3. 新增远程仓库地址

    git add remote [<options>] <name> <url>
    //例子
    git add remote origin https://github.com/snowyYU/x.git

name 为远程主机的名字

4. push，push，push

    git push -u origin master

这里只想说一下-u参数，设置了这个之后，pull fetch push啥的默认连接的为origin master

>[知乎git push 的 -u 参数具体适合含义](https://www.zhihu.com/question/20019419)

