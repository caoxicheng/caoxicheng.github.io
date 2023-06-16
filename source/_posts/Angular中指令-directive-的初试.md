---
title: Angular中指令(directive)的初试
date: 2020-02-19 11:34
tags:
---

## 指令的概要

在Angular中有三种类型的指令：

1. **组件**--拥有模板的指令<app-root></app-root>
2. **结构型指令**-- 通过添加和移除DOM元素改变DOM布局的指令（NgFor、NgIf）
3. **属性型指令** -- 改变元素、组件或其他指令的外观和行为的指令（NgStyle）

## 创建一个简单的属性型指令

```
ng generate directive directive/hightlight  //添加前置文件夹便于逻辑清理
```

@Directive 装饰器的配置属性中指定了该指令的 CSS 属性型选择器 `[appHighlight]`

这里的方括号([])表示它的属性型选择器。 Angular 会在模板中定位每个拥有名叫 appHighlight 属性的元素，并且为这些元素加上本指令的逻辑。

正因如此，这类指令被称为 属性选择器 。

把生成的指令控制器代码修改成这样：

```
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(el: ElementRef) {
       el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```

import 语句从 Angular 的 core 库中导入了一个 ElementRef 符号。

[post cid="358" /]

你可以在指令的构造函数中使用 ElementRef 来注入宿主 DOM 元素的引用，也就是你放置 appHighlight 的那个元素。

ElementRef 通过其 nativeElement 属性给你了直接访问宿主 DOM 元素的能力。

这里的第一个实现把宿主元素的背景色设置为了黄色。

## 使用属性型指令

```
<p appHighlight>Highlight me!</p>
```

## 相应用户使用的事件

[@HostListener][1] 装饰器让你订阅某个属性型指令所在的宿主 DOM 元素的事件，在这个例子中就是 `<p>`。

[scode type="blue"]
当然，你可以通过标准的 JavaScript 方式手动给宿主 DOM 元素附加一个事件监听器。 但这种方法至少有三个问题：

1. 必须正确的书写事件监听器。
2. 当指令被销毁的时候，必须拆卸事件监听器，否则会导致内存泄露。
3. 必须直接和 DOM API 打交道，应该避免这样做。
   [/scode]

### 下面是修改后的指令代码 directive.ts

```
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) { }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

但是这样子并不灵活！这不可取。

## 使用[@Input][2] 数据绑定向指令传递值

先从@angular/core 中导入input

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';
```

然后把highlightColor属性添加到指令中 like this:

```
@Input() hightColor: string;
```

## 绑定@Input 属性

@Input 装饰器，它的作用是往类上添加了一些元数据，从而让该指令的lightLight `能用于`绑定

模板代码：

```
<p appHighlight highlightColor="yellow">Highlighted in yellow</p>
<p appHighlight [highlightColor]="'orange'">Highlighted in orange</p>
```

问题来了，为什么要重复写，能不能整合呢？
可以！
@Input 提供了别名功能

## 最终版本控制器代码

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {


  @Input('appHighlight') highlightColor: string;

  constructor(
    private el: ElementRef
    ) { }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || 'yellow'); //这里防止有些小可爱忘记传值OωO
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string):void {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

改动之后的调用方法

```
<p [appHighlight]="'red'">Highlight me!</p>
```

[1]: https://angular.cn/api/core/HostListener

[2]: https://angular.cn/api/core/Input
