---
layout: post
title: vite-vue-ts搭建项目
subtitle: vue与ts的恩怨
date: 2020-12-21
author: XX
header-img: img/home-bg-o.jpg
catalog: true
tags:
  - vue
  - ts
---

vite 中使用sass 安装sass

安装组件vite-plugin-style-import

vite.config.ts

```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import styleImport from 'vite-plugin-style-import'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    styleImport({
      libs: [
        {
          libraryName: 'element-plus',
          esModule: true,
          ensureStyleFile: true,
          resolveStyle: name => {
            name = name.slice(3)
            return `element-plus/packages/theme-chalk/src/${name}.scss`
          },
          resolveComponent: name => {
            return `element-plus/lib/${name}`
          }
        }
      ]
    })
  ]
})

```

<br/>

然后再安装element-plus

会遇到样式失效问题

修改main.ts


```typescript
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/packages/theme-chalk/src/index.scss'
import App from './App.vue'

const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')

```
