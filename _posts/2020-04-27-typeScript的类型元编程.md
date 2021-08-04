---
layout: post
title: typeScript的类型元编程
subtitle: 进阶
date: 2020-04-27
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - 前端
  - ts
---

循环依赖与类型空间

JavaScript 不建议循环依赖,如下

```
// editor.js
import { Element } from './element'

// element.js
import { Editor } from './editor'
```

typescript 使用 import type 语法

```
// element.ts
import type { Editor } from './editor'

// 这个 type 可以放心地用作类型标注，不造成循环引用
class Element {
  editor: Element
}

// 但这里就不能这么写了，会报错
const editor = new Editor()
```

类型空间与类型变量

显然，类型空间里容纳着的是各种各样的类型。而非常有趣的是，编程语言中的「常量」和「变量」概念在这里同样适用：

当我们写 let x: number 时，这个固定的 number 类型就是典型的常量。如果我们把某份 JSON 数据的字段结构写成朴素的 interface，那么这个 interface 也是类型空间里的常量。
在使用泛型时，我们会遇到类型空间里的变量。这里的「变」体现在哪里呢？举例来说，通过泛型，函数的返回值类型可以由输入参数的类型决定。如果纯粹依靠「写死」的常量来做类型标注，是做不到这一点的。
在 TypeScript 中使用泛型的默认方式，相比 Java 和 C++ 这些（名字拼写）大家都很熟悉的经典语言，并没有什么区别。这里重要的地方并不是 <T> 形式的语法，而是这时我们实际上是在类型空间中定义了一个类型变量：

```typescript
// 使用泛型标注的加法函数，这里的 Type 就是一个类型变量
function add<Type>(a: Type, b: Type): Type {
  return a + b;
}

add(1, 2); // 返回值类型可被推断为 number
add("a", "b"); // 返回值类型可被推断为 string

add(1, "b"); // 形参类型不匹配，报错
```

<br/>

keyof

```
// 定义一个表达坐标的 Point 结构
interface Point {
  x: number
  y: number
}

// 取出 Point 中全部 key 的并集
type PointKey = keyof Point

// a 可以是任意一种 PointKey
let a: PointKey;

a = 'x' // 通过
a = 'y' // 通过
a = { x: 0, y: 0 } // 报错
```

extends

```typescript
// identity 就是返回原类型自身的简单函数
// 这里的 T 就相当于被标注成了 Point 类型
function identity1<T extends Point>(a: T): T {
  return a;
}

// 这里的 T 来自 Point2D | Point3D 这个表达式
function identity2<T extends Point2D | Point3D>(a: T): T {
  return a;
}
```
