---
title: 配置husky与lint-staged用以实现快照生成前的代码检查和格式化
date: 2020-12-08 21:43:47
tags: ['npm','standard']
categories: standard
---

在之前的项目中，团队为了保持代码风格的统一会统一要求为各自的 vs-code 安装 **prettier** 插件，然后在项目中添加 **.prettierrc** 配置文件，还需要设置 vs-code 中的自动保存时使用 **prettier** 插件 格式化代码。同样为了减少低级的代码错误，也引进了 **eslint** 进行代码的检查。

<!--more-->