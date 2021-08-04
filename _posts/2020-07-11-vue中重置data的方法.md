---
layout: post
title: vue中重置data的方法
subtitle: 最科学的方法
date: 2020-07-11
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - 前端
  - vue
---

在某些情况下，需要重新使用 data 中的数据，但是 data 中的数据已经被各种表单、变量等赋值，那么怎么重置 data 的值呢？

1. 逐个赋值
   ...data(){return{ name:'', sex:'', desc:''}}...// 逐个赋值 this.name =''this.sex =''this.desc =''

这个方法比较笨，当然也可以实现效果，但是一个一个去重新赋值比较麻烦而且代码看起来也会比较乱。
下面这个方法肯定是你喜欢的，一行代码搞定～ 2. 使用 Object.assign()
MDN 关于该方法的介绍：Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
用法： Object.assign(target, ...sources)
第一个参数是目标对象，第二个参数是源对象，就是将源对象属性复制到目标对象，返回目标对象
其中就是将一个对象的属性 copy 到另一个对象
vue 中：
this.$data 获取当前状态下的data
this.$options.data() 获取该组件初始状态下的 data
所以，下面就可以将初始状态的 data 复制到当前状态的 data，实现重置效果：
Object.assign(this.$data, this.$options.data())
当然，如果你只想重置 data 中的某一个对象或者属性：
this.form = this.$options.data().form

扩展
Object.assign(target, ...sources) 方法还可以用来合并对象：
const o1 ={ a:1};const o2 ={ b:2};const o3 ={ c:3};const obj = Object.assign(o1, o2, o3);console.log(obj);// { a: 1, b: 2, c: 3 }console.log(o1);// { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。

如果对象中含有相同属性，取最后一个属性：
const o1 ={ a:1, b:1, c:1};const o2 ={ b:2, c:2};const o3 ={ c:3};const obj = Object.assign({}, o1, o2, o3);console.log(obj);// { a: 1, b: 2, c: 3 } 属性取最后一个对象的属性
