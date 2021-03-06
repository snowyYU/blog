---
title: javascript作用域
date: 2017-05-10 18:50:19
tags: js
categories: js
---

# 相关概念 #
1. **执行环境**(execution context,有时也叫“环境”,“执行上下文”,“”,“”)
2. **变量对象**(variable object)
3. **活动对象**(activation object)

# 执行环境和作用域 #
**执行环境**很重要，执行环境定义了变量或函数有权访问的其他数据(难道指的就是初始化的作用域链？),决定论它们各自的行为。每个执行环境都有一个与之关联的 **变量对象**(包含了环境中定义的所有变量和函数)(难道执行环境只包含了一个标识符指针列表？)，虽然我们的代码无法访问这个对象，但解析器在数据处理时会在后台使用它

全局执行环境是最外围的执行环境，在Web浏览器中，全局执行环节被认为是window对象
 
每个函数都有自己的执行环境，当执行流进入一个函数时，函数的环境就会被推入一个环境栈中，在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。
>也就是说这个栈一直存在，和全局执行环境一样关闭网页时才被销毁

当代码在一个环境中执行时，会创建变量对象的一个 **作用域链**(scope chain)。作用域链的用途，是保证对执行环境有权访问的所有变量和函数有序的访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象，活动对象在开始时只包含一个变量，即arguments对象(这个对象在全局环境中是不存在的)。作用域链中的下一个变量对象来自包含(外部)环境，而再下个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。

>作用域链存放在哪

以上大部分来自js高程这本书，下面开始图解

# 图解过程 #

## step1 ##

    //myScript.js 
    "use strict"    
    var a=1;
    var b=2; 

如图，开始时，创建全局执行环境并被推入执行环境栈
![p1](http://i.imgur.com/FcKggu2.png)

## step2 ##
    //myScript.js 
    "use strict"    
    var a=1;
    var b=2; 
    function foo(){
        var c=3;
        var d=4;
        console.log(c+d);
    }
执行至`function foo(){}`也就是函数创建时，food的标识符(identifier)被加到当前(栗子中是全局变量对象)中,并且这个标识符引用了一个函数对象，函数对象中不仅包含了函数的源代码等还有[[scope]],[[Scope]]属性中保存了一条用来初始化作用域链，其指向就是当前的变量对象(全局变量对象)
>注意此时并没创建函数相应的执行环境
![p2](http://i.imgur.com/GGGcGON.png)

## step3 ##
    //myScript.js 
    "use strict"    
    var a=1;
    var b=2; 
    function foo(){
        var c=3;
        var d=4;
        console.log(c+d);
    };
    foo();

执行至`foo()`,函数被调用，产生相应的执行环境，之后被推入执行环境栈顶，

>此时产生的执行环境中的作用域链由之前函数定义时的[[scope]]而来，作用链的前端为foo的变量对象，由于这个环境是函数，创建活动对象并将其作为变量对象
![p3](http://i.imgur.com/ocIbFzQ.png)

## step4 ##
调用foo()结束后，其执行环境会被弹出执行栈，
![p4](http://i.imgur.com/gDcXp4r.png)

# tips #

1.作用域链是本质上是一个指向变量对象的指针列表，它只是引用但不实际包含变量对象。
2.AO中包含了函数的形参、arguments对象、this对象、以及局部变量和内部函数的定义，然后AO会被推入作用域链的顶端。
>感觉AO和VO里的内容差不多


# EC和VO的产生过程 #
当函数被调用时进入EC的产生阶段


# 相关阅读 #
>[简书](http://www.jianshu.com/p/4881610a8c24)

>[偶尔看到的个人挺赞同的](http://www.cnblogs.com/jacksplwxy/p/6725850.html)

>[执行环境、变量对象、活动对象和作用域链](http://www.cnblogs.com/jacksplwxy/p/6725850.html)

>[皮卡丘的讲解](https://github.com/dwqs/blog/issues/18)

>[segmentfault作用域javascript1](https://segmentfault.com/a/1190000004676747)

>[segmentfault作用域javascript2](https://segmentfault.com/a/1190000008614579)

>[segmentfault作用域javascript3](https://segmentfault.com/a/1190000000533094)

>[讲了执行环境和变量对象的创建和执行过程，很有用](https://segmentfault.com/a/1190000009247123)

>[作用域](http://blog.csdn.net/u010425776/article/details/53557942)