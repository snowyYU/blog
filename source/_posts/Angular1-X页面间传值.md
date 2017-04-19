---
title: Angular1.X页面间传值
date: 2017-04-19 18:43:32
tags: angular
categories: angular
---
页面间的传值还是很常见的，在此记录下我接触过的angular1.X版本页面间传值的例子
<!--more-->
# 使用ui-router #

## 1. html模板页中可直接写在ui-sref里 ##
   
比如

    ui-sref="app.operation.channel-overview({'appId':'{{item.appId}}','appName':'{{item.appName}}'})"

相应的，ui-router中配置目的地url时需要拼接如下

    url: '/channel-overview?appId&appName'

在目标控制器中取出来

    var appId=$stateParams.appId

>参数内容会在浏览器的url中显示出来，比较乱

## 2. 使用$state.go()

### old ###
先讲下比较早的ui-router传参，有个很致命缺点就是url中不显示参数，刷新等操作会使页面丢失之前传过来的参数，如下
    
 1.router配置中标明参数的名字

    .state('state1', {
    url: '/path/:id', // 这个地方用简单字符串
    templateUrl: '/path/to.html',
    params: {
    obj: null // 这个地方就可以随便你用了. 因为这个参数没在state的url中体现出来
    }
    }).


 2.$state.go

    $state.go('state1', {
    id: '22',
    obj: {
        key: 'value'
    }
    });

3.获取obj直接用

    $stateParams.obj

>不要用了

### new ###

忘记在哪个版本ui-router新增了如下方法，可保持url中的参数

1.$state

     $state.go("state",{datas:{ID:data1,NAME:data2}})

2.配置router

    .state('state',{
    url:'path/{datas:json}',
    params:{'datas':null}.
    ...
    })


3.用$stateParam.datas来取

# 基于factory的页面间传参

>这个没试过，上链接

[知乎angular页面间传参](https://www.zhihu.com/question/33565135)