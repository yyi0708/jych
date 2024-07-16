---
title: 获取运行时指定文件路径
date: 2023-10-18 19:00:47
tags: 
  - 前端
  - 小技巧
category: developer
timeline: front-end
abstract: 在库、项目中，存在逻辑获取项目中目录文件的需求，不同模块规范下的操作。
---

# 获取当前文件路径

代码模块文件中，路径解析中，想基于本文件相对地址解析为绝对路径。

其中主要区分`package.json`中的`type`, 默认为`commonjs`规范，或者可选`module`ESM模块规范。

**commonjs**
* 使用 `__dirname` 访问当前目录并将相对路径解析为绝对路径。
```js
import { resolve } from 'path';

path.resolve(__dirname, '..', '添加剂.csv')
```

**ESM**
* 原生 ES 模块中不被支持`__dirname`。在fs文件系统中，path支持 `string`、 `URL`、`Buffer`，故依赖`import.meta.url`进行相应转换。
* `url.fileURLToPath`, 返回完全解析的特定于平台的 Node.js 文件路径。

```js
import { fileURLToPath } from 'node:url'

// 1. URL，
new URL('./package.json', import.meta.url)

// 2. string
fileURLToPath(new URL('src/some-file.js', import.meta.url))
```