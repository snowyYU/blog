---
title: TypeScript 使用笔记
date: 2021-10-11 15:27:46
tags:
---

## 小问题 1

ts 报错默认使用 vscode locale 语言，这样可能不太容易在 google 中查询报错原因，使用如下命令将报错提示设置为英文

```
tsc --locale en
```

unkown 相关类型报错，大概率需要使用 as 关键字进行具体化

## 小 tips1

### 使用仅含类型的导入导出语法

推荐使用[仅含类型的导入和导出](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-export)形式的语法可以避免潜在的 “仅含类型的导入被不正确打包” 的问题，写法示例如下：

```javascript
import type { SomeThing } from './some-module.js'
export type { SomeThing }
```

### type 和 interface 异同

两者其实非常相似，许多情况下两者都可以用，相较来说，interface 更适合直接拓展属性，具体可见如下例子

扩展 interface

```typescript
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear()
bear.name
bear.honey
```

通过交叉符扩展 type

```typescript
type Animal = {
  name: string
}

type Bear = Animal & {
  honey: boolean
}

const bear = getBear()
bear.name
bear.honey
```

向 interface 中添加属性

```typescript
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"'
window.ts.transpileModule(src, {})
```

type 创建后无法更改

```typescript
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}

// Error: Duplicate identifier 'Window'.
```

[官方具体说明](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)
