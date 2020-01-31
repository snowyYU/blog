---
title: git踩坑记
date: 2017-04-19 18:51:39
tags: git
categories: git
---

总是容易忘记git的一些命令和用法，在此记下我重复google的，哈哈，比较无序，足够多时再分类。
<!--more-->

# 将文件移除监听队列 #
git中如果文件已被加入跟踪队列，后续添加相应的.gitignore参数时其相应的改变仍会被监听，采用如下

    git rm --cached XX/xx.x

>附上链接
>[git忽略已经被提交的文件](https://segmentfault.com/q/1010000000430426)
>[.gitignore 文件无效的解决方法](http://www.ifeegoo.com/git-code-management-dot-gitignore-file-has-no-effect-solution.html)

# 常见的git删除 #

    git rm XXX.xx

如果误删了可采用`git checkout`
>`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区修改还是删除，都可以使用此还原

# 新增远程仓库地址 #

    git add remote [<options>] <name> <url>
    //例子
    git add remote origin https://github.com/snowyYU/x.git

name 为远程主机的名字

# push，push，push #

    git push -u origin master

这里只想说一下-u参数，设置了这个之后，pull fetch push啥的默认连接的为origin master

>[知乎git push 的 -u 参数具体适合含义](https://www.zhihu.com/question/20019419)

# merge 报错 #

今天在pull时出现了一个问题，过程是这样的，我新建了远程仓库时顺带初始化了readme,结果本地pull时报错，如下
    fatal: refusing to merge unrelated histories
就是它觉得我两个仓库没啥关联，然后我尝试push，报错如下
    $ git push -u origin master
    To https://github.com/snowyYU/center-solution.git
    ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'https://github.com/snowyYU/center-solution.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

说我本地仓库版本过时啦，叫我先pull

没办法，只能强上了，加上参数
    git pull origin master --allow-unrelated-histories

之后会进入vim让我输入commit message，
>[stackoverflow](https://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories)
>[vim命令看这里](http://www.jianshu.com/p/eae20fcde419)

    
# 查看更改处 #

    git diff

上图:

![gitDiff](http://i.imgur.com/QJamb2Z.jpg)

# 创建,切换分支 ,删除#

今天在看webpack官网的指南时候，发现各部分练习的代码挺不一样，又想起之前看angularjs时官网给出的phonecat，他们项目各个构建步骤被创建成一个个branch，感觉十分方便，所以我仿照试下

    git checkout -b XXX

这个命令相当于

    git branch XXX    //新建XXX分支
    git checkout XXX  //切换到XXX分支

现在就切换到XXX分支了，可以在gitbash中看到，然后我直接
    
    git push origin XXX

其实此时远程仓库中并没有XXX分支，执行此命令后，远程仓库会新建XXX分支，并push成功。 

    git branch -D XXX

删除本地的XXX分支

    git push origin :XXX

删除远程的XXX分支

# git rebase #

自己尝试更新folk到自己github上仓库时查到的这个命令，先上个链接

>[How do I update a GitHub forked repository?](https://stackoverflow.com/questions/7244321/how-do-i-update-a-github-forked-repository)

第一个答案大意就是，

1. 从自己的GitHub上clone至本地工作目录
2. 在本地添加remote，即从哪folk来的(这里先叫upstream)，
3. fetch upstream上的所有分支
4. 切换到本地的master分支
5. 将upstream上需要同步的分支rebase

我觉得rebase和merge差不多，区别可能是rebase不会留下痕迹，merge需要输入message啥的

>[git rebase](http://blog.csdn.net/hudashi/article/details/7664631/)

## 后续 ##
针对更新自己folk的仓库，官方后来又给出了web版本的方案，上面的是需要本地仓库的，先上链接

>[www](https://github.com/KirstieJane/STEMMRoleModels/wiki/Syncing-your-fork-to-the-original-repository-via-the-browser)

大致就是自己从源向自己folk的仓库发起 pull request,然后合并

# 遇到ping github.com time out 问题 #
> 可以在网页中正常访问 github，唯独在终端中连接不了 github，也 ping 不通

喵喵喵？查了半天网上说了一堆关于 proxy 代理的问题，结果我连到手机的热点就可以了，所以下次遇到时尝试切换网络环境