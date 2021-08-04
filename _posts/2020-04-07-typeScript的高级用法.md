---
layout: post
title: typeScript的高级用法
subtitle: 享受代码
date: 2020-04-07
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - 前端
  - ts
---

unknown 指的是不可预先定义的类型，在很多场景下，它可以替代 any 的功能同时保留静态检查的能力。

<br/>

```typescript
const num: number = 10;
(num as unknown as string).split('');
```

unknown 的一个使用场景是，避免使用 any 作为函数的参数类型而导致的静态类型检查 bug：

```typescript
function test(input: unknown): number {
  if (Array.isArray(input)) {
    return input.length;    // Pass: 这个代码块中，类型守卫已经将input识别为array类型
  }
  return input.length;      // Error: 这里的input还是unknown类型，静态检查报错。如果入参是any，则会放弃检查直接成功，带来报错风险
}
```

void 和 undefined 类型最大的区别是，你可以理解为 undefined 是 void 的一个子集，当你对函数返回值并不在意时，使用 void 而不是 undefined。举一个 React 中的实际的例子。

```typescript
// Parent.tsx
function Parent(): JSX.Element {
  const getValue = (): number => { return 2 };    /* 这里函数返回的是number类型 */
  // const getValue = (): string => { return 'str' }; /* 这里函数返回的string类型，同样可以传给子属性 */
  return <Child getValue={getValue} />
}

// Child.tsx
type Props = {
  getValue: () => void;  // 这里的void表示逻辑上不关注具体的返回值类型，number、string、undefined等都可以
}
function Child({ getValue }: Props) => <div>{getValue()}</div>

```

非空断言运算符 !

这个运算符可以用在变量名或者函数名之后，用来强调对应的元素是非 null|undefined 的

```
function onClick(callback?: () => void) {
  callback!();  // 参数是可选入参，加了这个感叹号!之后，TS编译不报错
}
```

实例类型获取 typeof
typeof 是获取一个对象/实例的类型，如下：

```typescript
const me: Person = { name: 'gzx', age: 16 };
type P = typeof me;  // { name: string, age: number | undefined }
const you: typeof me = { name: 'mabaoguo', age: 69 }  // 可以通过编译
```

<br/>

遍历属性 in [ 自定义变量名 in 枚举类型 ]: 类型
in 只能用在类型的定义中，可以对枚举类型进行遍历，如下：

```typescript
// 这个类型可以将任何类型的键值转化成number类型
type TypeToNumber<T> = {
  [key in keyof T]: number
}
```

keyof返回泛型 T 的所有键枚举类型，key是自定义的任何变量名，中间用in链接，外围用[]包裹起来(这个是固定搭配)，冒号右侧number将所有的key定义为number类型。

<br/>

泛型

```typescript
// 普通类型定义
type Dog<T> = { name: string, type: T }
// 普通类型使用
const dog: Dog<number> = { name: 'ww', type: 20 }

// 类定义
class Cat<T> {
  private type: T;
  constructor(type: T) { this.type = type; }
}
// 类使用
const cat: Cat<number> = new Cat<number>(20); // 或简写 const cat = new Cat(20)

// 函数定义
function swipe<T, U>(value: [T, U]): [U, T] {
  return [value[1], value[0]];
}
// 函数使用
swipe<Cat<number>, Dog<number>>([cat, dog])  // 或简写 swipe([cat, dog])
```
