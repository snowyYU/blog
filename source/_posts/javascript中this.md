---
title: javascript中this
date: 2017-05-08 20:58:06
tags: js
categories: js
---

javascript中的this接触很多，但一直是模模糊糊的，在这里整理下

# 相关 #

与其他语言相比，函数的 this 关键字在JavaScript中的行为略有不同。它在严格模式和非严格模式之间也有一些区别。

读了廖雪峰老师的文章感觉文中所说的调用对象就是指执行环境，不知道可不可以这样理解

当一个函数被调用时，会创建一个 __执行环境(execution context)__及相应的作用域链，自动获取两个特殊变量：this和arguments，然后使用arguments和其他命名参数的值来初始化函数的 **活动对象(activation object)**，

`(function(){}())`立即执行函数

构造函数模式创建对象时，会经历以下4个步骤

1. 创建一个新对象
2. 见构造函数的作用域赋给新对象（因此this就指向了这个新对象）
3. 执行构造函数中的代码（为这个对象添加属性）
4. 返回新对象


# this 指向 #
this总是指向调用函数的那个对象,在运行时基于函数的执行环境绑定的

## 纯函数调用 ##
属于全局调用，this代表全局对象Global

    
## 作为对象方法的调用 ##

this指向调用的函数

## 作为构造函数调用 ##

## apply && call ##

apply()和call()作为函数对象的方法，可以改变函数的执行环境(调用对象)
它们的第一个参数是个对象，即作为函数内的this。

    function foo(){
        console.log(this);
    }
    foo.call(1);  //1
    foo.call('a');  //a
    foo.call(true);  //true
    foo.call({type:object});  //[object Object]

如上，他们的参数作为函数内的this输出

需要注意的是，传null或undefined时，将是JS执行环境的全局变量。浏览器中是window,其他环境，如node则是global。

    fun.call(null);  //window or global
    fun.call(undefined);  //window or global

apply()和call()唯一的区别是参数不一样，apply的第二个参数必须传入数组。
如 

    func1.call(this, arg1, arg2);
    func1.apply(this, [arg1, arg2]);

### 试用条件 ###
js中函数的参数不固定，当参数是明确知道数量时，用 call，而不确定的时候，用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个 __类数组__来便利所有的参数。

###  小小结 ###
 1.apply() 的第一个参数是对象，第二个参数是数组，作为参数列表。
 2.

# 相关参考 #

>[call和apply](http://www.cnblogs.com/snandy/archive/2012/03/01/2373243.html)
