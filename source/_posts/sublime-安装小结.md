---
title: sublime 安装小结
date: 2016-12-22 14:44:17
tags: sublime
categories: sublime
---

# sublime 3 #
sublime大家都知道啦，用了近一年了，感情颇深。之前用了sublime2，感觉最大的不同是sublime3的启动速度飞快，之后一直在用别人放出来装了各种插件的3，结果近几个月想要下载新的插件时发现竟然装不了，哈，值此一周年之际，自己重新捣鼓下3.
<!--more-->
## 安装 ##
安装就不多说了，官网[sublime3下载](https://www.sublimetext.com/3 "sublime3下载")。


> 需要注意，最好在翻墙状态下，反正我没翻墙下不了。

安装可以选中**explorer context menu**选项，意思是右击时显示open with sublime，方便编辑，不想显示中文可以去注册表改一下

![](http://i.imgur.com/UyAYoF3.png)

重点来了，额，不购买的话会一直提示，比较烦

>最好有条件还是支持正版

在简书上看到注册码[sublime3注册码](http://www.jianshu.com/p/04e1b65dd2c0 "sublime3注册码")
博主也在定期更新（非常感谢大神，大神的博客很赞）。help中填入注册码就行啦

>留一下，有好几个的，填一个就行啦

# 插件 #
## 插件安装 ##
- 直接装

preferences>browse packages...

![](http://hiphotos.baidu.com/exp/pic/item/0e655ca7d933c8954e7c86f6d31373f0830200f9.jpg)

下载地址：[package control](https://sublime.wbond.net/Package%20Control.sublime-package "package control")

放进去:

![](http://hiphotos.baidu.com/exp/pic/item/8a95ad1c8701a18b4eb90cc29c2f07082938feaa.jpg)

>建议用这个方法，我试了用console命令报错 好像是can not open XXX
>,换了另外一个网上的命令可以下，但是速度很慢

- 用package control组件安装

这个不多说啦，附上官方介绍

[packagecontrol installation](https://packagecontrol.io/installation "packagecontrol installation")

## 推荐插件 ##
>这里只记录我接触到的

插件的安装嘛，ctrl+shift+p调出控制台，选到package controll install，然后等，再次弹出控制台输入想要安装的插件，选中 安装 等待

注意要留意下方的状态

1. emmet 
神器，谁用谁知道，补全代码等等，附上用法
[Emmet](https://packagecontrol.io/packages/Emmet "Emmet")

2. markdownEditing
这个用来写博客，记得先ctrl+shift+p设置成markdown格式，写这篇文就遇到个坑，我用的[markdownpad](markdownpad "http://markdownpad.com/")的图床是 [Imgur](Imgur "http://imgur.com/")，额，不翻墙上传不了图片

3. angular
对于指令和服务，鼠标移上去可以看到初始化的地方

4. sublimeLinter
语法错误检测，这个组件依赖于nodeJS下的jshint，所以还要通过npm安装jshint的jshint

    npm install -g jshint

5. SideBarEnhancements
侧栏右键功能增强

6. Alignment
等号对齐（Ctrl+Alt+A）

7. Bracket Highlighter 
代码匹配

8. prettify
HTML-CSS-JS Prettify  code format  代码格式化

9. color Highlighter
实时看到颜色

10. html/js/css prettify
ctrl+shift+h一键格式，代码对齐啥的

11. localhistory 
历史记录

------
>2017.5

12. nodejs
支持对node的语法补全等

13. SublimeLinter-contrib-eslint
使用eslint来进行代码规范检查

14. Vue Syntax Highlight
.vue结尾的代码也会变得五颜六色

持续更新中  ...

