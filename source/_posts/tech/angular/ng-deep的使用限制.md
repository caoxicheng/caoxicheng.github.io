---
title: ng-deep的使用限制
date: '2021-12-08 13:13'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  对于ng-deep,感觉官方的态度是比较暧昧的,但是这个使用场景其实还是比较多的.特别是在组件内部使用其他组件,并且需要仅在当前组件下自定义样式的情况,ng-deep就显得比较迫切了.
  但是以前以为在组件样式文件夹内直接使用::ng-deep是没有问题的,也会挂在当然组件下面,但是实际并不是如此 !...
description: ng-deep的使用限制 - 详细介绍与实践经验分享
keywords:
  - deep
  - 的使用限制
  - 技术
  - Angular
cover: ''
---

对于ng-deep,感觉官方的态度是比较暧昧的,但是这个使用场景其实还是比较多的.特别是在组件内部使用其他组件,并且需要仅在当前组件下自定义样式的情况,ng-deep就显得比较迫切了.

但是以前以为在组件样式文件夹内直接使用::ng-deep是没有问题的,也会挂在当然组件下面,但是实际并不是如此

![image.png](1411695885.png)

![image.png](3704857986.png)

如果直接在最外层使用`::ng-deep`是会直接加载至全局样式的,用户知道打开过此组件页面,就算后期关闭也会有此遗产存留.

本来是期望使用第二种使用方案,即外层包含一个保护类.这样即使加载了,也会因为angular的组件样式隔离而无法影响其他页面.

但是白天在交流的时候,有人提到了`::host`这个关键字特意去看了官方文档,其实官方已经想到了这种情况

[已弃用 /deep/、>>> 和 ::ng-deep](https://angular.cn/guide/component-styles#deprecated-deep--and-ng-deep)

所以结论在使用`::ng-deep`的地方`应该`加上`:host`伪类选择器
