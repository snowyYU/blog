---
title: eslint初涉
date: 2017-05-05 21:00:31
tags: eslint
categories: eslint
---

便于养成良好的代码习惯和规范性，开始学习eslint,还有sublime相应的插件安装

<!--more-->

# eslint #

eslint 还是挺强大的，相较于jshint

>新版本配置文件 .eslint 被抛弃，建议使用 .eslint.js来进行配置，不过我分别尝试后发现第二种行不通，暂时还没找到问题，就都记下来

# .eslint 配置 #

## 安装nodejs部分 ##

    npm install eslint -g
    npm install babel-eslint -g

## 在sublime 安装 eslint 插件 ##

安装

1. SublimeLinter
2. SublimeLinter-contrib-eslint

在项目根目录编写配置文件

.eslintrc

    {
    "env": {
        "browser": true,
        "node": true,
        "es6": true
    },
    "parser": "babel-eslint",
    "ecmaFeatures": {
       "jsx": true
    },
    "rules": {
        "semi": [2, "always"],
        "quotes": [2, "single"]
    }
    }

it works

![效果](http://i.imgur.com/gXpuFcw.png)

# .eslint.js #

看了些文档后好像可以通过运行如下命令在控制台输出错误信息

    $ eslint yourfile.js
也就是说不需要sublime了，喔

## node.js部分的相关安装 ##

局部安装

不说了，看参考链接2

全局安装

    $ npm i -g eslint
>一直分不清 `$ npm i -g eslint`和`$ npm i eslint -g`的区别

初始化

    $ eslint --init

建议命令行配置如下

* Use a popular style guide
* Standard
* JavaScript

>可以看看参考链接3

>注意，这个需要package.json的配置了，全局使用的话要同样全局安装使用到的插件，比如如下配置的话就要全局安装plugins里的了

![plugins](http://i.imgur.com/4l6XQ0o.png)

# 小结 #

1. .eslint的使用感觉更方便啊，不需要配置package.json，不需安装相应的node_module。不过明显感觉.eslint.js更智能，有脚手架的感觉
2. 优先级方面

# 参考资料 #
1. [.eslint](http://blog.csdn.net/binjly/article/details/49926001)
2. [官方起步文档,不过没有sublime的配置部分](http://eslint.org/docs/user-guide/getting-started)
3. [参考链接3](http://www.jianshu.com/p/e826e13c67ec)