---
title: 使用 prettier + eslint 统一代码风格和语法检查
date: 2020-07-03 16:59:07
tags: [vscode,IDE]
categories: standard
---

考虑各项目间的统一，使用 prettier 进行项目的代码风格格式化，使用 eslint 做语法检查，具体规范如下

<!--more-->

# Prettier vs. Linters #

首先说明 Prettier 和 .Linters(ESLint/TSLint/stylelint, etc)的区别，

1. Prettier 专注于代码风格上的统一，其配置项较少，是个 Opinionated formatter (就是类似 angular 那种，必须按照它自定的规则来)，
2. 比如说 ESLint，它其实包括了代码风格规范和语法检查两部分，有众多的规范插件可供选择，其配置项也较为复杂。但在代码风格上，它只能提供异常报错的警示信息，不能直接帮我们输出风格统一的代码。

> [How does it compare to ESLint/TSLint/stylelint, etc.?](https://prettier.io/docs/en/comparison.html)

# Prettier #
代码美化部分大致包含：触发自动换行的字符数，强制关键词前后加空格

prettier 的大部分规则都是不可配置了，只保留了几个争议特别大的规则给用户配置，比如单引号和双引号的选择，也正因如此，对团队的代码风格上的统一也会更有帮助

## 风格规范 ##

直接执行如下命令查看
```bash
npx prettier --write .
npx prettier --write app/
npx prettier --write app/components/Button.js
npx prettier --write app/**/*.test.js
```
## 安装配置 ##

使用 `eslint-config-prettier` 覆盖掉 ESLint 的代码风格规则

```
npm install --save-dev eslint-config-prettier
```

向根目录下 .eslintrc 文件中的 **extend** 增加 `"prettier"`

```json
{
  "extends": [
    "some-other-config-you-use",
    "prettier"
  ]
}
```

配置根目录下 .prettierrc 文件如下

```json
{
    "eslintIntegration": true, //
    "stylelintIntegration": true,
    "tabWidth": 2,        // 缩进字符
    "singleQuote": true,  // 要不要单引号
    "semi": false         // 要不要使用 ;
}
```

配置保存自动格式化，在用户的 `setting.json` 文件中加上如下配置

```json

"eslint.format.enable": true,

```   

如有需要忽略代码风格

## others ##

### git hook ###

使用 husky 和 lint-staged 来确保每次提交前代码已是格式化后的状态，配置参考如下

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "**/*": ["eslint --fix", "prettier --write"]
  }
}
```





## about ##
1. [Prettier](https://prettier.io/docs/en/index.html)
2. [lint-staged](https://github.com/okonet/lint-staged#readme)
3. [husky](https://github.com/typicode/husky#readme)

# ESLint #

语法检查部分大致包含：变量检查（重复引用，引入但未使用，声明但未使用。。。），无意义的bind

一般情况下，脚手架都自带了基础的 eslint 配置，所以主要还是注意eslint 的 plugin 的安装配置，参考如下

## about ##
1. [eslint](https://eslint.org/docs/user-guide/getting-started)
2. [working-with-plugins](https://eslint.org/docs/developer-guide/working-with-plugins)

