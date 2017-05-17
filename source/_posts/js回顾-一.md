---
title: js回顾(一)
date: 2017-03-06 15:18:25
tags: js
---

# 对象和原型 #
## 几种常见的创建对象方法##
1.通过对象字面量构造

    var contact1={
        name:"zhao",
        sex:"male"
    }

通过此方法构造的对象，其[[prototype]]指向Object.prototype

2.通过构造函数

    function Contact(name,sex){
        this.name=name;
        this.sex=sex;
        this.getName=function(){
            return name;
        }
    }
    var contact1=new Contact()
    contact1.__proto__==Contact.prototype //true

每个函数都有一个prototype属性，其所指向的对象带有constructor属性，这一属性指向函数自身,eg,本例中，Contact.prototype.constructor==Contact

3.通过函数Object.create构造

    var contact={
        name:"zhao",
        sex:"male"
    }
    var contact1=Object.create(contact);

contact1的[[prototype]]指向contact


# js中__proto__和prototype的区别和关系？ #
常说js中一切都是对象，但基本数据类型不是对象，基本数据类型没有方法和属性 ，当我们调用方法时候会封装出一个对象，（wrapper对象是什么）
首先，几乎所有对象都有[[prototype]]属性，在es5中这是个隐藏属性，指向此对象的原型。es5中用Object.getPrototypeOf函数获得一个对象的[[prototype]]。es6中，使用Object.setPrototypeOf可以直接修改一个对象的[[prototype]]
>很多浏览器都给每个对象提供__proto__属性(不标准，不推荐用)，我觉得可以理解为就是[[prototype]]，

![关系图](http://i.imgur.com/0G4COrD.png)

1.方法（Function）是对象，方法的原型(Function.prototype)是对象。因此，它们都会具有对象共有的特点。即：对象具有属性__proto__，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。
2.方法(Function)方法这个特殊的对象，除了和其他对象一样有上述_proto_属性之外，还有自己特有的属性——原型属性（prototype），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做constructor，这个属性包含了一个指针，指回原构造函数。好啦，知道了这两个基本点，我们来看看上面这副图。
1.构造函数Foo()构造函数的原型属性Foo.prototype指向了原型对象，在原型对象里有共有的方法，所有构造函数声明的实例（这里是f1，f2）都可以共享这个方法。
2.原型对象Foo.prototypeFoo.prototype保存着实例共享的方法，有一个指针constructor指回构造函数。
3.实例f1和f2是Foo这个对象的两个实例，这两个对象也有属性__proto__，指向构造函数的原型对象，这样子就可以像上面1所说的访问原型对象的所有方法啦。另外：构造函数Foo()除了是方法，也是对象啊，它也有__proto__属性，指向谁呢？指向它的构造函数的原型对象呗。函数的构造函数不就是Function嘛，因此这里的__proto__指向了Function.prototype。其实除了Foo()，Function(), Object()也是一样的道理。原型对象也是对象啊，它的__proto__属性，又指向谁呢？同理，指向它的构造函数的原型对象呗。这里是Object.prototype.最后，Object.prototype的__proto__属性指向null。
总结：
1.对象有属性__proto__,指向该对象的构造函数的原型对象。
2.方法除了有属性__proto__,还有属性prototype，prototype指向该方法的原型对象。

看一下几种常见构造一个对象的方法，决定了一个对象的[[prototype]]属性

# 参考 #
>[对象与原型](http://blog.csdn.net/u010425776/article/details/53617292)