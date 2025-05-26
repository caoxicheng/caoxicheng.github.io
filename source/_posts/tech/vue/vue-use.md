---
title: vue.use
date: '2025-02-17 16:45:24'
tags:
  - vue
categories:
  - 技术
  - Vue
excerpt: >-
  在看vue的插件时,经常看到app.use(xxx)这样的写法,心生好奇 也没有直接看文档,直接去问了deepseek javascript //
  自定义插件：一个包含 install 方法的对象 const myPlugin = {   install(app, options) {    
  //...
description: vue.use - 详细介绍与实践经验分享
keywords:
  - vue
  - use
  - 技术
  - Vue
cover: ''
---

在看vue的插件时,经常看到app.use(xxx)这样的写法,心生好奇
也没有直接看文档,直接去问了deepseek

```javascript
// 自定义插件：一个包含 install 方法的对象
const myPlugin = {
  install(app, options) {
    // 添加全局属性
    app.config.globalProperties.$myMethod = () => {
      console.log('Hello from myPlugin!', options)
    }
    // 注册全局组件
    app.component('my-component', { /* ... */ })
  }
}

// 安装插件，并传递配置选项
app.use(myPlugin, { someOption: true })
```

就是手动绑定vue实例的快捷方案,注册组件,注册服务,方法
