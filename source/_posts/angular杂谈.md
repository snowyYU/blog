---
title: angular杂谈
date: 2016-12-26 15:38:12
tags: [angular,others]
categories: angular
---

这里来记一些angular相关的东西
<!--more-->
# 驼峰命名 #
直接上例子，指令中的命名

     var appModule=angular.module('app',[]);    
    appModule.directive('helloWorld',function(){
        return{
            restrict:'e',
            template:'<div>hello world~</div>',
            replace:true
        };
        })

html中

    <hello-world></hello-world>

# service和factory #
>参考地址
>1.[segmentfault](https://segmentfault.com/a/1190000003096933)
>2.[AngularJS: Service vs provider vs factory](http://stackoverflow.com/questions/15666048/angularjs-service-vs-provider-vs-factory?rq=1)
>3.[官网api,$provider](https://docs.angularjs.org/api/auto/service/$provide)
>4.[What is the type friendly injection?](http://stackoverflow.com/questions/25667321/what-is-the-type-friendly-injection)

-service和factory本质都是provider的shortcut
-'provider'是唯一一种可以创建用来注入到config()函数的服务的方式。想在你的服务启动之前，进行一些模块化的配置的话，就使用provider

在官方文档中,auto模块中的服务有且仅有$injuector和$provider
对auto模块解释为 自动获取并添加到每个注入器的隐含模块

