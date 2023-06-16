---
title: 轻量级的marmode插件实现网站的黑夜模式
date: 2019-08-23 16:13
tags:
---

## 简单的几步实现网站的黑暗模式

<!--more-->

先贴出官方网站
[Darkmode.Js][1]

## 简单模式

很简单，甚至只需要三步。

```
<script src="https://cdn.jsdelivr.net/npm/darkmode-js@1.4.0/lib/darkmode-js.min.js"></script>
<script>
  new Darkmode().showWidget();
</script>
```

## 带配置参数

```
var options = {
  bottom: '64px', // default: '32px'
  right: 'unset', // default: '32px'
  left: '32px', // default: 'unset'
  time: '0.5s', // default: '0.3s'
  mixColor: '#fff', // default: '#fff'
  backgroundColor: '#fff',  // default: '#fff'
  buttonColorDark: '#100f2c',  // default: '#100f2c'
  buttonColorLight: '#fff', // default: '#fff'
  saveInCookies: false, // default: true,
  label: '☹️', // default: ''
  autoMatchOsTheme: true // default: true
}

const darkmode = new Darkmode(options);
darkmode.showWidget();
```

## 配合pio插件食用

1. 在自定义JavaScript中添加cdn文件
   
   ```
   <script src="https://cdn.jsdelivr.net/npm/darkmode-js@1.4.0/lib/darkmode-js.min.js"></script>
       <script>
         const darkmode =  new Darkmode();
       </script>
   ```
2. 在pio相应的黑夜模式中填写触发开关的函数 `darkmode.toggle();`
3. 实际handsome主题使用中还需要自定义css添加

```
.darkmode-layer, .darkmode-toggle {
   z-index: 1; //1就行了，高了会有层级错误.
 }
```

[1]: https://darkmodejs.learn.uno/
