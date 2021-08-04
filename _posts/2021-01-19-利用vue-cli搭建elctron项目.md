---
layout: post
title: 利用vue-cli搭建electron项目
subtitle: vue与ts的恩怨
date: 2021-01-19
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - vue
  - ts
---

首先用 vue-cli 创建 vue3.0 项目
babel
typeScript
Router
Vuex
CSS Pre-processors
Linter / Formatter
vue 版本 3.x 使用 class-style component syntaxUse Babel alongside TypeScriptUse history mode for routerSass/Scss (with node-sass)Eslint + prettierLint and fix on commitWhere do you prefer placing config for Babel, ESLint, etc.? In dedicated config files 单独的文件去管理各种配置

然后执行命令

vue add electron-builder

执行过程中报错,要求 node 版本 14.0 以上

使用 nvm 切换 node 版本到 14.16.1

再次执行

vue add electron-builder

选择 electron 版本 13.0

运行

```
npm run electron:serve
```

报错:

```
 ERROR  Failed to compile with 1 error                                                                                                                                                                             下午3:57:5
 error  in ./src/App.vue?vue&type=style&index=0&id=7ba5bd90&lang=scss

Syntax Error: Error: Missing binding E:\vue3\vue-next-electron-webpack\node_modules\node-sass\vendor\win32-x64-83\binding.node
Node Sass could not find a binding for your current environment: Windows 64-bit with Node.js 14.x

Found bindings for the following environments:
  - Windows 64-bit with Node.js 12.x

This usually happens because your environment has changed since running `npm install`.
Run `npm rebuild node-sass` to download the binding for your current environment.
```

执行

```
npm rebuild node-sass
```

执行失败

修改 node-sass 镜像

```
yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
```

尝试执行

```
yarn add --force node-sass
```

执行成功

再吃运行仍然报错

![截图](https://files.catbox.moe/4nvcd5.png)

```

```

修改 node-sass 版本到 4.14.1

执行命令

```
yarn add --force node-sass@4.14.1
```

成功

<br/>

打包

```
npm run electron:build
```

添加 elementPlus
