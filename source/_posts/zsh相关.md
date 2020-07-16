---
title: zsh相关
date: 2020-07-14 12:13:57
tags: [shell]
---

zsh 相关的 **iTerm2** 和 **oh my zsh** 安装，以及相关主题配置

<!--more-->

# 前言 #

开始接触命令行是在用windows的时候，那时就只是用来装装node之类的软件，后来装了 git bash，接触了些很好用的linux命令，就可以减少使用鼠标的次数啦，一定程度上来说还是很爽的。

之后碗切到了mac，Linux 命令就用的更爽啦(mac os 毕竟是基于 linux 的)，然后就接触到了 fish ，可以直接通过 fish ui 命令直接调出配置页面，可以直接调整文字大小啊，颜色啊，git分支提示，而且还有历史命令提示等功能，从此我的命令行就变得花花绿绿的。再后来，mac 大力推荐 zsh 了，因为 zsh 功能更加强大，所以也就有了本篇。

# 安装 zsh #
安装 zsh 就不写了，因为 mac 本身就内置了，切换下就好

# 安装 **iTerm2** #

[iTerm2下载](https://www.iterm2.com/)

> echo $SHELL  // 查看当前命令行解析器

# oh my zsh #

## 安装 ##

通过 curl
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
通过 wget
```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

> 刚开始安装时会报 connect refuse 等的错误，我是通过修改 git host 配置来解决的

## 插件配置 ##
配置 **.zshrc** 文件
```bash
vi ~/.zshrc
```
比如添加如下插件
```bash
plugins=(
  git
  bundler
  dotenv
  osx
  rake
  rbenv
  ruby
)
```
## 安装 powerline font ##

有些主题需要 powerline font 的支持，所以配置主题前先安装 powerline font

有文章说需要先安装 powerline 再安装 powerline font，实际上我直接按照如下安装的，并没有先装 powerline

> 可能其中的 install 脚本自带检查安装 powerline 的功能吧

地址在此 [powerline fonts](https://github.com/powerline/fonts)

安装方法有多种，我使用了如下的安装方法

```bash
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```

## 主题配置 ##
同样是修改 **.zshrc** 文件，修改如下配置项

```bash
ZSH_THEME="robbyrussell"
```
> 其实安装 **on my zsh** 后，原本的 **.zshrc** 文件的内容会被覆盖，新的内容里增加了大量的说明内容，方便了 zsh 相关的配置

# 上照 #

主要是配置了颜色，然后加上背景图片

![image.png](https://i.loli.net/2020/07/16/HvDUlni3opYaTeW.png)

# go2shell #

很好用，集成在finder中，可以快速在当前目录下调出命令行工具

> 记得选择 iterm2

# about #

1. [我觉得这篇文的 fish 部分写的很好](https://zhuanlan.zhihu.com/p/103672631)
2. [知乎](https://zhuanlan.zhihu.com/p/37195261)