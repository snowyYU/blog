---
title: 配置husky与lint-staged用以实现快照生成前的代码检查和格式化
date: 2020-12-08 21:43:47
tags: ['npm','standard']
categories: standard
---

在之前的项目中，团队为了保持代码风格的统一会统一要求为各自的 vs-code 安装 **prettier** 插件，然后在项目中添加 **.prettierrc** 配置文件，还需要设置 vs-code 中的自动保存时使用 **prettier** 插件 格式化代码。同样为了减少低级的代码错误，也引进了 **eslint** 进行代码的检查。现引入 husky 和 lint-stage 来优化协作流程

<!--more-->

**vue** 项目在初始化的时候可以直接选择启用 `Lint and fix on commit`，如下

![WX20201208-234451@2x.png](http://ww1.sinaimg.cn/large/40c136bfgy1glgvu3ivx9j20v00caqf6.jpg)

## 安装 ##

先安装 Eslint 和 Prettier 
>如果原项目是小程序之类的没有 package.json 文件，需要先 `npm init`

```bash
npm install eslint --save-dev  
```

配置下 eslint，新增 .eslintrc.js 和 .eslintignore 文件，配置如下

> 如果需要支持 typescript，还需 `npm i -d @typescript-eslint/parser @typescript-eslint/eslint-plugin`

```javascript
// eslint.js  
// for ts //考虑到小程序初始化ts时已有的ts代码通过不了此配置下的校验，所以使用 ts 版本的 precommit 要慎重
module.exports = {
  parser: '@typescript-eslint/parser', //定义ESLint的解析器
  extends: ['plugin:@typescript-eslint/recommended'], //定义文件继承的子规范
  plugins: ['@typescript-eslint'], //定义了该eslint文件所依赖的插件
  env: {
    //指定代码的运行环境
    browser: true,
    node: true,
  },
}

// for js
module.exports = {
  root: true,
  env: {
    browser: true,
    commonjs: true,
    es2021: true,
  },
  extends: 'eslint:recommended',
  //全局变量
  globals: {
    __DEV__: true,
    __WECHAT__: true,
    __ALIPAY__: true,
    App: true,
    Page: true,
    Component: true,
    Behavior: true,
    wx: true,
    getApp: true,
    getCurrentPages: true,
  },
  rules: {
    'no-debugger': 'off',
    'no-unused-vars': 'off',
  },
}
```



```bash
npm install prettier --save-dev 
```
新增 prettierrc.js 文件，配置如下
```javascript
module.exports = {
  eslintIntegration: true,
  stylelintIntegration: true,
  tabWidth: 2,
  singleQuote: true,
  semi: false,
}

```


执行 lint-stage 的安装配置命令，此命令会同时安装并配置 husky 

```bash
npx mrm lint-staged
```

最终 package.json 文件相关配置如下

```json
"devDependencies": {
    "eslint": "^7.15.0",
    "husky": "^4.3.5",
    "lint-staged": "^10.5.3",
    "prettier": "^2.2.1",
    "stylelint": "^13.8.0",
    "stylelint-config-standard": "^20.0.0"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint": "npm run lint:js && npm run lint:style && npm run lint:prettier",
    "lint:js": "eslint --fix \"**/*.js\"",
    "lint:style": "stylelint --fix \"**/*.wxss\" --syntax css",
    "lint:prettier": "prettier --write \"**/*.(js|wxss|json)\""
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": "eslint --fix",
    "*.wxss": "stylelint --fix --syntax css",
    "*.{js,wxss,md}": "prettier --write"
  }
```

> 这里同时引入了 stylelint 用来校验 wxss