---
title: 在Angular中定位原生html元素
date: 2019-10-09 19:31
tags:
---

基于某些特殊情况，在使用框架时，或者历史遗留问题，还是需要操作DOM对象

<!--more-->

此时可在DOM上添加模板引用变量 # 来标识DOM对象

```
<input #phone placeholder="phone number" (click)="method(phone.value)">
```

在控制器TS文件中怎么引用呢？

@viewChild('phone') private phone: any; //这里实际类型为下文的ElementRef

```
this.phone.nativeElement.setAttribute('style', `margin-left:${-(this.selectedIndex + 1 - this.showNumber) * 70}px`);
```

this.phone.nativeElement即为DOM对象

---

信息补全
========

[ElementRef][1]

> 对视图中某个原生元素的包装器。

```
class ElementRef<T> {
  constructor(nativeElement: T)
  nativeElement: T
}
```

| 属性  | 说明               |
| --- | ---------------- |
|     | nativeElement: T |

[scode type="red"]当需要直接访问 DOM 时，请把本 API 作为最后选择。优先使用 Angular 提供的模板和数据绑定机制。或者你还可以看看 [Renderer2][2]，它提供了可安全使用的 API —— 即使环境没有提供直接访问原生元素的功能。

如果依赖直接访问 DOM 的方式，就可能在应用和渲染层之间产生紧耦合。这将导致无法分开两者，也就无法将应用发布到 Web Worker 中。[/scode]

[1]: https://www.angular.cn/api/core/ElementRef

[2]: https://angular.cn/api/core/Renderer2
