---
title: angular基础
date: 2016-12-26 15:39:48
tags: [angular,start]
categories: angular 
---
接触angular已经近半年了，一直在用别人的指令改改，遇到了很多自己解决不了的坑，其实自己对angular的各部分了解并不多，现在回头来弄清其中的用法原理，打好自己的基础。
<!--more-->

> 准备从如下几个部分展开，每个部分放出参考文章链接以及代码仓库地址

# tisps #

1.module.run()方法在注射器加载完所有模块是执行一次

# 指令 #

>[git](https://github.com/snowyYU/angularPractice)

>先记几个核心问题
>1.指令的运行原理：comile与link(操作元素，添加CSS样式，绑定事件)
>2.指令与控制器之间的交互
>3.指令间的交互
>4.scope的类型与独立scope
>5.scope的绑定策略
>6.angularjs内置指令(63)
>7.Expander,Accordion
>8.directive思想的起源和原理概述

## 指令有三个阶段  ##

>1.`加载阶段`>>>2.`编译阶段`>>>3.`链接阶段`

1.加载angular.js，找到ng-app指令，确定应用的边界

2.遍历DOM，找到所有指令；根据指令代码中的temlate,replace,transclue转换DOM结构；如果存在compile函数则调用；

3.对每一条指令运行link函数；link函数一般用来操作DOM，绑定事件监听器；

>compile函数用来对模板自身进行转换，而link函数负责在模型和视图之间进行动态关联；
作用域在链接阶段才会被绑定到编译后的link函数上；
compile函数仅仅在编译阶段运行一次，而对于指令的每个实例，link函数都会执行一次；
compile可以返回preLink和postLink函数，而link函数只会返回postLink函数；
如果需要修改DOM结构，应该在postLink中来做这件事，而如果在preLink中做这件事会导致错误；
大多数时候我们只要写link函数即可  

## 配置含义 ##

### replace ###
replace 属性为 true 时，指令标签会被 templete中的内容替换掉，如

    var appModule = angular.module('app', []);
    appModule.directive('hello', function() {
    return {
        restrict: 'E',
        template: '<div>Hi there</div>',
        replace: true
    };
    });

打开浏览器调试工具只能看到`<div>Hi there</div>`

### transclude ###
值为true时保留指令包裹的内容

## 参考资料 ##

1.学习例子参考大漠老师的[5个实例详解指令机制](http://damoqiongqiu.iteye.com/blog/1917971)

2.[angular指令的transclude选项以及ng-transclude指令](https://segmentfault.com/a/1190000004586636)

# 服务 #
# angular核心 #
# angular原理 #




