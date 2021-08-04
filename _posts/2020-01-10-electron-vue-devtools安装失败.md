---
layout: post
title: electron中Unable to install `vue-devtools
subtitle: electron-vue-devtools安装失败解决办法
date: 2020-01-10
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - 前端
---


由于网络的问题，electron运行的时候加载vue-devtools失败。 Unable to install `vue-devtools` 。从日志里看retry了四次都timeout了。

找了一圈，这个也没有淘宝镜像等国内镜像。不过后来按网上找了个方法，成功加载了最新的版本。

对于开发来讲，好的调试工具太重要了。

先  npm install vue-devtools --save-dev

然后 把ready事件里面注释掉5行，再加上一行手动加载的。

最终src/main/index.dev.js里面修改后的内容如下（所有内容）：

```javascript
/* eslint-disable */

// Install `electron-debug` with `devtron`
require('electron-debug')({ showDevTools: true })
import {  BrowserWindow } from 'electron';
// Install `vue-devtools`
require('electron').app.on('ready', () => {
  let installExtension = require('electron-devtools-installer')
  // installExtension.default(installExtension.VUEJS_DEVTOOLS)
  //   .then(() => {})
  //  .catch(err => {
  //     console.log('Unable to install `vue-devtools`: \n', err)
  //   })
  //参考 https://www.cnblogs.com/wozho/p/10782654.html 和 https://github.com/SimulatedGREG/electron-vue/issues/242
  BrowserWindow.addDevToolsExtension('node_modules/vue-devtools/vender')  //手动加载vue-devtools，前提是 npm install vue-devtools --save-dev

})

// Require `main` process to boot app
require('./index')
```
