---
title: angular2+中使用renderer2代替原生js方法创建以及操控DOM对象
date: '2020-02-13 18:31'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  [scode
  type="green"]今天遇到一个需要类似右击然后创建一个菜单的时候，陷入了误区，为什么要自己操控dom呢？angular明明优势就是数据操控视图，直接在页面的定义好对应的组件（不显示），直接通过数据控制不就行了吗？[/scode]
  window.document.createEl...
description: angular2+中使用renderer2代替原生js方法创建以及操控DOM对象 - 详细介绍与实践经验分享
keywords:
  - angular
  - 中使用
  - renderer
  - 代替原生
  - 方法创建以及操控
cover: ''
---

[scode type="green"]今天遇到一个需要类似右击然后创建一个菜单的时候，陷入了误区，为什么要自己操控dom呢？angular明明优势就是数据操控视图，直接在页面的定义好对应的组件（不显示），直接通过数据控制不就行了吗？[/scode]

```
window.document.createElement()
```

这个基本大家都使用过
虽然在angular中也能使用，不过因为逻辑上Angular以组件为单位划分，这其实是很不合理的，所以官方提供个封装好的方法给我们使用

`let demo = this.renderer2.createElement('a');`
`this.renderer2.appendChild(this.el.nativeElement, demo);`

[post cid="358" /]

这里的el.nativeElement替代当前控制器所操控的实际DOM结构  请看下文

//这里可以理解为当前组件ts 逻辑上的body部分

```
import { Component, OnInit, AfterViewInit, ViewChild, ElementRef, Renderer2, OnDestroy  } from '@angular/core';

constructor(
    private renderer2: Renderer2,
    private el: ElementRef,
  ) { }
```

然后就是控制属性的code：

```
this.renderer2.setAttribute(demo, 'download', this.pdfName);
```

移除DOM：

```
this.renderer2.removeChild(this.el.nativeElement, tempLink);
```

---

聊点别的

从过年前的的不到一万，现在已经6w了。

2020计划: => <p style="color:red">活着</p>
