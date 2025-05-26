---
title: 对象转原始类型是根据什么流程运行的？
date: '2019-11-08 15:21'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  对象转原始类型，会调用内置的[ToPrimitive]函数，对于该函数而言，其逻辑如下：
  1.如果Symbol.toPrimitive()方法，优先调用再返回 2.调用valueOf()，如果转换为原始类型，则返回
  3.调用toString()，如果转换为原始类型，则返回 4.如果都没有返回原始类型...
description: 对象转原始类型是根据什么流程运行的？ - 详细介绍与实践经验分享
keywords:
  - 对象转原始类型是根据什么流程运行的
  - 技术
  - JavaScript
cover: ''
---

对象转原始类型，会调用内置的[ToPrimitive]函数，对于该函数而言，其逻辑如下：

1.如果Symbol.toPrimitive()方法，优先调用再返回
2.调用valueOf()，如果转换为原始类型，则返回
3.调用toString()，如果转换为原始类型，则返回
4.如果都没有返回原始类型，会报错

```
var obj = {
  value: 3,
  valueOf() {
    return 4;
  },
  toString() {
    return '5'
  },
  [Symbol.toPrimitive]() {
    return 6
  }
}
console.log(obj + 1); // 输出7
```

ps:如何让if(a == 1 && a == 2)条件成立？

```
var a = {
  value: 0,
  valueOf: function() {
    this.value++;
    return this.value;
  }
};
console.log(a == 1 && a == 2);//true
```
