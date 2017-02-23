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