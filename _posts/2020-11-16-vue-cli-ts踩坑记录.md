---
layout: post
title: vue-cli-ts踩坑记录
subtitle: vue与ts的恩怨
date: 2020-11-16
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - vue
  - ts
---

创建过程

1. babel
   typeScript
   Router
   Vuex
   CSS Pre-processors
   Linter / Formatter

2. vue 版本 3.x
3. 使用 class-style component syntax
4. Use Babel alongside TypeScript
5. Use history mode for router
6. Sass/Scss (with node-sass)
7. Eslint + prettier
8. Lint and fix on commit
9. Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files 单独的文件去管理各种配置

<br/>

安装 element-plus

vue add element-plus

安装过后会报错找不到./plugin/element 的类型声明

修改 element.js ====>element.ts

修改 element.ts 代码

```typescript
import ElementPlus from "element-plus";
import "../element-variables.scss";
// import locale from "element-plus/lib/locale/lang/zh-cn";
// import { Vue } from "vue-property-decorator";
import { App } from "vue";

export default (app: App): void => {
  app.use(ElementPlus, {});
};
```

这个时候报错应该就没有了

但是 element-plus 引入还是失败的

还需要修改 main.ts

```typescript
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";
import installElementPlus from "./plugins/element";

const app = createApp(App);
installElementPlus(app);
app.use(store).use(router).mount("#app");
```
