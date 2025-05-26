---
title: 小试牛刀之使用指令完成clickOutSide功能
date: '2020-02-19 18:19'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  前面提到了Angular的Directive指令。现在用指令完成一个小功能。 <!--more--> 新建指令 ng genrate directive
  directiveName @Directive({   selector: '[appClickOutSide]' }) export clas...
description: 小试牛刀之使用指令完成clickOutSide功能 - 详细介绍与实践经验分享
keywords:
  - 小试牛刀之使用指令完成
  - clickOutSide
  - 技术
  - Angular
cover: ''
---

前面提到了Angular的Directive指令。现在用指令完成一个小功能。

<!--more-->

## 新建指令

`ng genrate directive directiveName`

```
@Directive({
  selector: '[appClickOutSide]'
})
export class ClickOutSideDirective {

  constructor(
    ) { }
}
```

在构造函数中导入ElementRef来获取对实际DOM的引用

改造后👇

```
@Directive({
  selector: '[appClickOutSide]'
})
export class ClickOutSideDirective {

  constructor(
    private el: ElementRef
    ) { }
}
```

[scode type="blue"]
因为官方不推荐直接操控DOM，如果非要操控也建议使用Renderer2，这里因为只涉及判断是否当前宿主DOM所以不冲突
[/scode]

## 事件发射

监听到宿主DOM以外的点击事件时。需要朝外部发射信号需要用到EventEmitter()

```
@Output()
public clickOutSide = new EventEmitter();
```

## 绑定监听点击事件

需要监听宿主外的点击事件这里需要用到 @Hostlisener修饰器,因为需要全局监听，这里用到@Hostlistener的document： || window:指令
因为还需要判断当前点击DOM是否在宿主DOM内。用到了DOM-API的contains
代码如下

```
import { Directive, ElementRef, HostListener, Output, EventEmitter } from '@angular/core';

@Directive({
  selector: '[appClickOutSide]'
})
export class ClickOutSideDirective {

  constructor(
    private el: ElementRef
    ) { }

  @Output('appClickOutSide')
  public clickOutside = new EventEmitter();

  @HostListener('document:click', ['$event.target'])
  public onMouseClick(targetElement) {
    const clickedInside = this.el.nativeElement.contains(targetElement);
    if(!clickedInside) {
      this.clickOutside.emit(null);
    }
  }
}
```

## 调用

html:
`<p appHighlight (appClickOutSide)="demo('xxx')">测试代码</p>`
TS:

```
demo(xx){
    console.log('点击了外部', xx);
  }
```

因为指令中的EventEmitter弹出为NUll，这里可以附加自定义参数做更多事情.
