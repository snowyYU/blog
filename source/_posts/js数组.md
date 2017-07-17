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

# 数组的操作 #
## 栈方法 ##
栈数据结构的访问规则是LIFO（last-in-first-out,后进先出）,两个方法push()和pop()
>我我觉得可以理解为操作一直在数组尾巴处进行

push()返回操作后数组的长度。

pop()删除并返回数组最后一项

几个例子
    
    var colors = new Array();
    colors.push("red","green");    //返回数组的长度2，此时colors=["red","green"]
    colors.pop();                  //返回数组的最后一项"green",此时colors=["red"]



## 队列方法 ##
队列数据的访问规则是FIFO(first-in-first-out,先进先出)，

shift(),移除数组中的第一个项并返回该项，同时将数组长度减1。

unshift(),在数组前端添加任一个项并返回新数组的长度。

例子如下

    var colors = new Array();
    colors.push("white","blue");
    colors.shift();              //返回数组的第一项"white",此时colors=["white"]
    colors.unshift("purple","yellow");  //返回数组的长度3,此时colors=["purple","yellow","white"]   

## 重排序方法 ##

reverse(),将原数组的顺序颠倒，返回一个数组。

sort(),会先调用每个项的toString()方法，然后比较得到的字符串，用以确定排序
>数组各项是数字可能会出问题，因为仍然会先转化为字符串在进行排序，这样的话比较的就是各自的字符编码（ASCII），如下

    var numbs=[0,1,5,10,15];
    numbs.sort();
    console.log(numbs);     //[0,1,10,15,5]
    console.log("5".charCodeAt());      //53
    console.log("10".charCodeAt());     //49

但是sort()方法可以接收一个比较函数作为参数，用以决定那个值位于哪个值的前面。比较函数的返回值如下

1. 返回负数，表示value1应该位于value2之前
2. 返回0，表示两个参数相等
3. 返回正数，表示value1应该位于value2之后

    function compare(value1,value2){
        if(value1 < value2){
            return -1;
        }else if(value1 > value2){
            return 1;
        }else {
            return 0;
        }
    }

    var numbs=[0,1,5,10,15];
    numbs.sort(compare);
    console.log(numbs);      //[0,1,5,10,15]

## 操作方法 ##

concat()拼接数组，直接上例子

    var colors = ["red","yellow","blue","green"];
    var colors2 = colors.concat("purple",["black","white"]);
    console.log(colors2);   //["red","yellow","blue","green","purple","black","white"]


slice()接收一个或两个参数，即要返回起始至末尾或者起始和结束位置(不包括)的项，上例子

>注意参数可以为负值

    var colors = ["red","yellow","blue","green"];
    var colors3 = colors.slice(1);
    var colors4 = colors.slice(1,2);
    var colors5 = colors.slice(-3,-2);  //colors.length-3==1
    console.log(colors3);   //["yellow", "blue", "green"]
    console.log(colors4);   //["yellow"]



>concat()和slice()不会影响原有数组
### splice ###
splice方法很强大，可以实现数组的删除，插入，替换功能，语法为：

    arrayObject.splice(index,howmany,item1,.....,itemX);
    index  必需。整数，规定添加删除项目的位置，使用负数可从数组结尾处规定位置。
    howmany 必需。要删除的项目数量。如果设置为 0，则不会删除项目。
    item1, ..., itemX   可选。向数组添加的新项目。

返回删除的项组成的数组，如果没有删除的项，返回一个空数组。
>该方法会改变原始数组。
## 位置方法 ##

indexOf()和lastIndexOf()，都接收两个参数，要查找的项和表示查找起点的索引(可选)

不同点是indexOf从头开始找，lastIndexOf从尾巴找

都返回要查找的项在数组中的位置，没找到的话返回-1

indexOf()从
>想起来，字符串类型也有indexOf()方法，比如检测cookie中是否含user字段`document.cookie.indexOf("user=")`，顺带一提，`document.cookie`是一串字符串

## 迭代方法 ##

* every()
* filter()
* forEach()
* map()
* some()

让我缓口气，过两天在写完

## 归并方法 ##


# 其他 #
jq中，数组操作新增了each()

# 伪数组 #
像数组，有和数组相同的属性和方法，如length等。
 * nodelist
 * argument


# 参考文章 #
 1. [一瓢江湖](http://zxc8899.iteye.com/blog/1558298/)
 2. [简书](http://www.jianshu.com/p/66b04163948b)
 3. js高程