---
title: vue和angular异同
date: 2020-11-04 23:25:10
tags: ['angular','vue']
---

最近重新看回 angular，其1.X版本作为第一个接触到的框架，给我留下了深刻的印象，近两年的项目用的基本都是vue，感觉现在是时候总结记录下他们的异同了

<!--more-->

因为 vue3 还没上手实际项目，以下讨论的 vue 指的是vue2.X 版本

从以下几个方面聊聊

## 目录结构 ##

* **angular** 有着较为统一的目录结构，一般每个功能页面（视图-由组件和关联的模版构成）都有，template，component，service，样式文件（css,scss,less），举例如下

![WX20201105-110902@2x.png](http://ww1.sinaimg.cn/large/40c136bfgy1gke4flbxt4j20fe09ijs6.jpg)

* **angular** 在功能页面之上，还有更高层级的模块（module）概念，每个 **angular** 应用都有一个根模块，通常命名为 AppModule。根模块提供了用来启动应用的引导机制。 一个应用通常会包含很多特性模块。

* **vue** 的目录就比较松散了，每个人都有自己的分层想法，不过还是建议以 vue 的**风格指南**作为开发中的标准

* **vue** 其实是由一个个组件构成的，没有 angular 这么多别的东西，vue 的入口文件为 main.js

## 模版部分 ##

* **vue** 是用了虚拟dom（virtual dom），展示的为编译后的模版

![WX20201105-112937.png](http://ww1.sinaimg.cn/large/40c136bfgy1gke51ze13gj20fk0gg406.jpg)

![WX20201105-112905.png](http://ww1.sinaimg.cn/large/40c136bfgy1gke52nf3mlj20pc0epacq.jpg)

* **angular** 模版基于 web component 标准，如下，分别为代码和Google开发者工具中的显示

![WX20201105-111250@2x.png](http://ww1.sinaimg.cn/large/40c136bfgy1gke4k6fl7uj211k0gctcj.jpg)

![WX20201105-111324.png](http://ww1.sinaimg.cn/large/40c136bfgy1gke4kj5aerj20rp08agnu.jpg)

## 脚手架 ##

* **angular** 的脚手架非常强大，可以使用其中的命令创建，删除 module，service，component，route 等，并且有自动注入的功能，推荐使用命令创建模块组件等

> 经常会给脚手架命令加上参数，比如 `ng new routing-app --routing` 生成一个带有应用路由模块（AppRoutingModule）的基本 Angular 应用
* **vue** 等脚手架没有这么多辅助开发等命令，不过可以使用 ` vue ui ` 打开一个项目的可视化管理页面，进行包的增删操作等

![WX20201105-152425@2x.png](http://ww1.sinaimg.cn/large/40c136bfgy1gkebrx59lkj22l61jogwc.jpg)