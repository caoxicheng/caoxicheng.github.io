---
title: void操作符
date: '2020-02-24 17:21'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  今天在看ts转换JS的代码时 看到了一串代码 a === void 0 ? x : m 顿时心生困惑，按照逻辑 void 0 应该等价于undefined
  ，可是我并没有遇到过void 0 这种情况，于是查阅了一下相关资料 > void运算符可以执行右侧的表达式，返回值始终为undefined。 >...
description: void操作符 - 详细介绍与实践经验分享
keywords:
  - void
  - 操作符
  - 技术
  - JavaScript
cover: ''
---

今天在看ts转换JS的代码时 看到了一串代码

```
a === void 0 ? x : m
```

顿时心生困惑，按照逻辑 void 0 应该等价于undefined ，可是我并没有遇到过void 0 这种情况，于是查阅了一下相关资料

> void运算符可以执行右侧的表达式，返回值始终为undefined。
> 
> 利用此运算符可以很便利的完成一些小操作，比如最为常见的取消链接跳转动作。

可以举一个极端的栗子：

```
console.log(void function(){
    return '假装有输出'
})


// undefined
```

再举个耳熟能详的例子

```
<a href="javascript:void(0);">Lindo</a>
```

上述代码大家一定都不会陌生，点击它可以阻止链接的跳转动作。

用法非常简单，但是很多朋友并不太清楚其中的原理，下面分步进行一下介绍。

1."JavaScript:"是一个协议，当触发元素时，会执行它后面的代码。
2.如果协议后面代码返回undefined，则阻止元素的默认行为，比如点击链接，页面跳转就是默认行为。
3.如果协议后面没有任何代码，相当于返回值为undefined，阻止元素的默认行为。
4.如果协议后面有代码，那么将执行代码，用执行的代码替换页面中的内容，可能在不同的浏览器有所差别，比如`<a href="javascript:0;">Lindo</a>`，在谷歌浏览器中没有任何动作，在火狐浏览器中会将页面内容替换为0，其他浏览器效果大家自行测试。
