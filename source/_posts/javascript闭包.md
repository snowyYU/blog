---
title: javascript闭包
date: 2017-05-12 09:17:07
tags: js
categories: js
---

# 闭包是什么 #

## 闭包的产生 ##
我觉得是因为闭包函数的作用域链中保存着包裹函数的活动对象，正因保留着引用，导致包裹函数不会被垃圾回收

# 特点 #

1. 闭包会引用包含函数的整个活动对象
2. 是个函数

# 闭包的作用 #

1. 模拟私有方法

# 作用域链 #

作用域链和闭包脱不了干系，作用域链部分可以查看{% post_link javascript作用域 %}

# 相关参考 #