---
layout: post
title: qiankun和micro-app
subtitle: 经验
date: 2022-06-03
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - vue
  - 前端
---

qiankun要求子应用修改渲染逻辑并暴露出方法
micro-app 不需要修改子应用, 也不需要修改webpack配置, 是目前市面上接入微前端成本最低的方案, 并且提供了js沙箱, 样式隔离, 元素隔离, 预加载, 资源地址补全, 插件系统, 数据通信等一系列完善的功能

micro-app
1. 上手简单: 使用方式类似iframe
2. 侵入性低: 对源代码几乎没有影响
3. 组件化: 基于webComponents思想实现微前端
4. 功能丰富: js沙箱, 样式隔离. 预加载等
5. 轻量的体积: 10kb
6. 零依赖: 不依赖于任何第三方库
