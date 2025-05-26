---
title: 'Angular ChangeDetection:OnPush的视图更新策略'
date: '2021-09-23 12:45'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  默认更新策略 default Angular默认在三种情况下数据更新 事件驱动  事件: 页面 click、submit、mouse  XHR: http
  请求  Timer: setTimeout()、 setInterval() 都为异步且都是不同类型的webapi angular 通过zone...
description: 'Angular ChangeDetection:OnPush的视图更新策略 - 详细介绍与实践经验分享'
keywords:
  - Angular
  - ChangeDetection
  - OnPush
  - 的视图更新策略
  - 技术
cover: ''
---

## 默认更新策略 default

Angular默认在三种情况下数据更新 **事件驱动**

* 事件: 页面 click、submit、mouse
* XHR: http 请求
* Timer: setTimeout()、 setInterval()

都为**异步**且都是不同类型的webapi

angular 通过zone.js, 而zone.js 通过`猴子补丁`的方式强制重写了浏览器关于异步事件的捕获处理.

## 推送策略 OnPush

如果组件设置了

```
@Component({
  ...,
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

则需要`手动`触发变更检测

1. 组件的`@Input`属性的引用发生变化.
2. 组件内的 DOM 事件,包括它子组件的 DOM
3. 组件内的 Observable 订阅事件,**同时设置Asynv pipe**, 即默认的订阅事件其实是不会被angular捕获的
4. 组件内手动使用 ChangeDetectorRef.detectChanges()、ChangeDetectorRef.markForCheck()、ApplicationRef.tick()

> [ChangeDetectorRef-官方指南](http://angular.cn/api/core/ChangeDetectorRef)

### 变更触发顺序

在设置了@Input字段之后, 组件内更新的顺序根据模板中的变量前后顺序有关即

```
<app-demo [input1]="a" [input2]="b"></app-demo>
```

先修改input1,然后再修改input2,特别在设置了setter方法之后,表现特别明显

> [参考资料](http://blueskyawen.com/2020/01/13/angular-see-change-detection/)
