---
title: sublime3主题和BracketHightLighter插件配置
date: 2017-07-12 11:38:14
tags: sublime
categories: sublime
---

sublime有很高的配置性，这里说下他的主题部分和插件(BracketHightLighter)的相关配置。

<!--more-->

# sublime主题 #

我的sublime版本为3126

网上有很多主题，可通过package control来安装，就不说啦

# BracketHightLighter插件配置 #

## 安装 ##
安装不说了，直接在package control中安装

## 配置 ##

打开

Preferences  >  Package Settings  >  BracketHighlighter  >  Bracket Settings

不同版本的sublime会有些差异，sublime3的话一般配置分为

* 默认配置（用户不可更改，以防你改残了）
* 用户配置（这部分随便你改，大不了最后直接删掉）

这个就像css一样，用户配置的部分会覆盖默认的配置

现在，你看到两栏，左边就是默认配置啦，改不了的，右边就是用户配置，现在空空的向其中添加如下，

>其中，根据英文名就可以知道default(默认)，curly(大括号)，round(小括号)，square(方括号)......是分别配置各自的显示信息的啦

>style 属性就是展示的类型了，默认为下划线(underline)，个人偏好都改为高亮(highlight)

>color 的配置稍微繁琐，放到下面讲


     {
     "bracket_styles": 
        {
            "default": {
            "icon": "dot", 
            "color": "entity.name.class", 
            "color": "brackethighlighter.default", 
            "style": "highlight" 
            }, 
            "unmatched": {
            "icon": "question", 
            "color": "brackethighlighter.unmatched", 
            "style": "highlight" 
            }, 
            "curly": {
            "icon": "curly_bracket", 
            "color": "brackethighlighter.curly", 
            "style": "highlight" 
            }, 
            "round": {
            "icon": "round_bracket", 
            "color": "brackethighlighter.round", 
            "style": "highlight" 
            }, 
            "square": {
            "icon": "square_bracket", 
            "color": "brackethighlighter.square", 
            "style": "highlight" 
            }, 
            "angle": {
            "icon": "angle_bracket", 
            "color": "brackethighlighter.angle", 
            "style": "highlight" 
            }, 
            "tag": {
            "icon": "tag", 
            "color": "brackethighlighter.tag", 
            "style": "highlight" 
            }, 
            "single_quote": {
             "icon": "single_quote", 
             "color": "brackethighlighter.quote", 
             "style": "highlight" 
            }, 
            "double_quote": {
            "icon": "double_quote", 
            "color": "brackethighlighter.quote", 
            "style": "highlight" 
            }, 
            "regex": {
            "icon": "regex", 
            "color": "brackethighlighter.quote", 
            "style": "outline"  
            } 
        } 
    }



BracketHightLighter插件默认是使用下划线的（underline）

## color的配置 ##

比如其中的

    "curly": {
            "icon": "curly_bracket", 
            "color": "brackethighlighter.curly", 
            "style": "highlight" 
            }, 

color属性值为brackethighlighter.curly，这个在哪呢

首先要找到正在使用的主题,打开Packages文件夹，再打开User  >  Preferences.sublime-settings
第一行就是正在使用的主题配置

之后(Preferences  >  Browse Packages)打开Packages文件夹，User  >   Color Highlighter    >   themes

我试用的Monokai.tmTheme，打开，向`<array></array>`添加

        <dict>
            <key>name</key>
            <string>Bracket Curly</string>
            <key>scope</key>
            <string>brackethighlighter.curly</string>
            <key>settings</key>
            <dict>
                <key>foreground</key>
                <string>#CC99CC</string>
            </dict>
        </dict>

保存后就发现花括号高亮颜色就变成粉红色啦，其他配置同理


# 参考链接 #

* [color schemes](http://docs.sublimetext.info/en/latest/reference/color_schemes.html)
* [BracketHighlighter set color](https://facelessuser.github.io/BracketHighlighter/customize/#configuring-highlight-style)