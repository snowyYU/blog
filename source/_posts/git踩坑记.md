---
title: git踩坑记
date: 2017-04-19 18:51:39
tags: git
categories: git
---

总是容易忘记 git 的一些命令和用法，在此记下我重复 google 的，哈哈，比较无序，足够多时再分类。

<!--more-->

# credentials helper

> [Git – Config Username & Password – Store Credentials](https://www.shellhacks.com/git-config-username-password-store-credentials/)

# 将文件移除监听队列

git 中如果文件已被加入跟踪队列，后续添加相应的.gitignore 参数时其相应的改变仍会被监听，采用如下

    git rm --cached XX/xx.x

> 附上链接
> [git 忽略已经被提交的文件](https://segmentfault.com/q/1010000000430426) >[.gitignore 文件无效的解决方法](http://www.ifeegoo.com/git-code-management-dot-gitignore-file-has-no-effect-solution.html)

# 常见的 git 删除

    git rm XXX.xx

如果误删了可采用`git checkout`

> `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区修改还是删除，都可以使用此还原

# 新增远程仓库地址

    git add remote [<options>] <name> <url>
    //例子
    git add remote origin https://github.com/snowyYU/x.git

name 为远程主机的名字

# push，push，push

    git push -u origin master

这里只想说一下-u 参数，设置了这个之后，pull fetch push 啥的默认连接的为 origin master

> [知乎 git push 的 -u 参数具体适合含义](https://www.zhihu.com/question/20019419)

```bash
git push origin --all
```

上面命令表示，将所有本地分支都推送到 origin 主机。如果远程主机的版本比本地版本更新，推送时 Git 会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用 `–-force `选项。

```bash
git push origin --tags
```

`git push `不会推送标签(tag)，除非使用 `-–tags` 选项

# 打上标签 tag

主要有两种方法

- `git tag tagName`
- `git tag -a tagName -m "remarks备注"`

查看`tags`

- `git tag`

从 tag 处开一个分支

`git branch <new-branch> <tag-name>` 进行版本的管理

> [详情参考](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

# log 和 reflog 命令

## git log

展示所有的 commit

## git reflog

展示所有的操作记录，相较 git log ，reflog 命令可以展示 reset 和已删除 commit 的操作

# merge 报错

今天在 pull 时出现了一个问题，过程是这样的，我新建了远程仓库时顺带初始化了 readme,结果本地 pull 时报错，如下

    fatal: refusing to merge unrelated histories

就是它觉得我两个仓库没啥关联，然后我尝试 push，报错如下

    $ git push -u origin master
    To https://github.com/snowyYU/center-solution.git
    ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'https://github.com/snowyYU/center-solution.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

说我本地仓库版本过时啦，叫我先 pull

没办法，只能强上了，加上参数

    git pull origin master --allow-unrelated-histories

之后会进入 vim 让我输入 commit message，

> [stackoverflow](https://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories) >[vim 命令看这里](http://www.jianshu.com/p/eae20fcde419)

## 撤销合并

```
git merge --abort
```

有时候拉代码，合并，结果整个文件到处是冲突，可以使用此命令撤销到合并前的状态

> 一般这种情况常见于仓库和本地工作环境有一端代码格式化不同步

# 查看更改处

    git diff

上图:

![gitDiff](http://i.imgur.com/QJamb2Z.jpg)

这样只会对比当前工作区和所在分支最新 commit 的改动处，需要对比远程分支，可以看 [参考链接](#参考链接)

# 撤销更改

如果还没有使用 `add` 命令加入到暂存区，可以

## 单个文件

```
git checkout --filename
```

## 所有文件

```
git checkout .
```

## 清除所有新增未加入暂存区的文件/文件夹

```
git clean -xdf
```

> 注意：-x 会清除掉 .gitignore 文件中的忽略文件，比如常见的 node_modules 文件

详情见 [Here's How to Clean Git and Remove Untracked Files](https://www.makeuseof.com/git-clean/)

如果已经 `add` 之后，想要撤销

## 单个文件

```
git reset HEAD filename
```

## 所有文件

```
git reset HEAD
```

如果已经 `commit` 了，想要撤销

```
git reset commit_id  // 撤销后所有的更改会变成 add 之前的状态，git status 之后就是一片红红红
```

> 这个 id 是你想要回到的那个节点，可以通过 git log 查看，可以只选前 6 位
> 或者 通过 gitreflog 命令取得

```
git reset --hard commit_id //撤销之后，所做的已经commit的修改将会清除，仍在工作区/暂存区的代码也将会清除！
```

# 创建,切换分支 ,删除

今天在看 webpack 官网的指南时候，发现各部分练习的代码挺不一样，又想起之前看 angularjs 时官网给出的 phonecat，他们项目各个构建步骤被创建成一个个 branch，感觉十分方便，所以我仿照试下

    git checkout -b XXX

这个命令相当于

    git branch XXX    //新建XXX分支
    git checkout XXX  //切换到XXX分支

现在就切换到 XXX 分支了，可以在 gitbash 中看到，然后我直接

一般情况下直接 clone 的项目只有 master 分支，想要获取远程别的分支代码还需要在本地检出一个分支，然后建立映射，这两步可以使用一条命令完成

```bash
git checkout -b 本地分支名 origin/远程分支名
```

git push origin XXX

其实此时远程仓库中并没有 XXX 分支，执行此命令后，远程仓库会新建 XXX 分支，并 push 成功。

    git branch -D XXX

删除本地的 XXX 分支

    git push origin :XXX

删除远程的 XXX 分支

# git rebase

自己尝试更新 folk 到自己 github 上仓库时查到的这个命令，先上个链接

> [How do I update a GitHub forked repository?](https://stackoverflow.com/questions/7244321/how-do-i-update-a-github-forked-repository)

第一个答案大意就是，

1. 从自己的 GitHub 上 clone 至本地工作目录
2. 在本地添加 remote，即从哪 folk 来的(这里先叫 upstream)，
3. fetch upstream 上的所有分支
4. 切换到本地的 master 分支
5. 将 upstream 上需要同步的分支 rebase

我觉得 rebase 和 merge 差不多，区别可能是 rebase 不会留下痕迹，merge 需要输入 message 啥的

> [git rebase](http://blog.csdn.net/hudashi/article/details/7664631/)

# git stash

哈哈，这个命令还挺好玩的，能把当前工作区的更改先搁置，最常见的应用就是突然要去到之前的某个版本打个包。

使用 `git stash pop` 取回搁置的更改，继续撸码

> 如果更改中有 untrack 的文件，需要加上 `-u` 参数，即 `git stash -u` 才能一起 stash 起来

## 后续

针对更新自己 folk 的仓库，官方后来又给出了 web 版本的方案，上面的是需要本地仓库的，先上链接

> [www](https://github.com/KirstieJane/STEMMRoleModels/wiki/Syncing-your-fork-to-the-original-repository-via-the-browser)

大致就是自己从源向自己 folk 的仓库发起 pull request,然后合并

# 遇到 ping github.com time out 问题

> 可以在网页中正常访问 github，唯独在终端中连接不了 github，也 ping 不通

喵喵喵？查了半天网上说了一堆关于 proxy 代理的问题，结果我连到手机的热点就可以了，所以下次遇到时尝试切换网络环境

# 参考链接

- [如何在 git 中对比当前工作区和远程仓库的区别](https://www.zhihu.com/question/53601264)
