---
title: TypeScript-interface和type的区别
date: '2020-03-25 23:56'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  在ts中，定义类型由两种方式：接口（interface）和类型别名（type alias）
  interface只能定义对象类型，type声明的方式可以定义组合类型，交叉类型和原始类型 如果用type alias 声明的方式，会导致一些功能的缺失
  1.interface方式可以实现接口的extends...
description: TypeScript-interface和type的区别 - 详细介绍与实践经验分享
keywords:
  - TypeScript
  - interface
  - type
  - 的区别
  - 技术
cover: ''
---

在ts中，定义类型由两种方式：接口（`interface`）和类型别名（`type alias`）
interface只能定义对象类型，type声明的方式可以定义组合类型，交叉类型和原始类型
如果用type alias 声明的方式，会导致一些功能的缺失
1.interface方式可以实现接口的extends/implements，而type 不行

2.interface可以实现接口的merge，但是type不行
