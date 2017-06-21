---
title: js数组
date: 2017-06-20 19:51:50
tags: js
categories: js
---

js中数据类型分基本数据类型和引用数据类型两种，对象便是某个特定引用类型的实例。
Array构造函数在web中作为window对象的一个属性
# 数组的初始化 #
1. ` var array=[]; `
2. ` var array=new Array(); `

>` var array=new Array(5) `接收的参数作为新建数组的长度
>` var array=new Array("param1","param2","param3") `直接接收数组成员作为参数

# 数组的方法 #
数组的长度

    var family = ["child","mother","father"];
    family.length    //3

js中，数组的长度可变，会随着数据的添加而增长，所以会出现所含项数和length属性值不等的情况，如下
    
    var family = ["child","mother","father"];
    family.length=4;
    family[3]    //undefined
    family.length=2;
    family[2]    //undefined

# 数组的检测 #
数组的检测不能使用typeof，因为返回的是object
>说到这个我想起来我本来有个误解，以为typeof只能返回js数据类型的名称呢，

>即`undefined`,`null`,`boolean`,`string`,`number`,`object`,`symbol(es6新增)`。其实检测function类型时会返回function，具体typeof的返回值看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)

然后就是这个方法，instanceof其实从字面上就可以看出它是干嘛的了，可以理解为判断对象是否是被右边的家伙所实例化（语无伦次）。
    
摘自js高程

instanceof操作符在存在多个作用域(像一个页面包含多个frame)的情况下，也是问题多多。一个经典的例子

    var isArray = value instanceof Array

以上代码要返回true，value必须是一个数组，而且还必须与Array构造函数在同个全局作用域中。（别忘了，Array是window 的属性）如果value是在另一个frame中 定义的数组。那么以上代码就会返回false。

推荐用这种方法
    object.prototype.toString.call(value)

>如果value为数组的话会返回[object Array]，同时这种方法也可以检测原生函数或正则表达式，分别返回[object Function]和[object RegEXp]

# 数组的转化方法 #
所有对象都具有toLocaleString(),toSring()和valueOf()方法，所以数组也不例外。

还有join()
## 数组转化为对象 ##

# 参考文章 #
 1. [一瓢江湖](http://zxc8899.iteye.com/blog/1558298/)
 2. [简书](http://www.jianshu.com/p/66b04163948b)