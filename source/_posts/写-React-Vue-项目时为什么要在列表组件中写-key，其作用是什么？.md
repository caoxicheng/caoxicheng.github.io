---
title: 写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？
date: 2019-07-25 17:12
tags:
---

[scode type="share"]壹题 https://github.com/Advanced-Frontend/Daily-Interview-Question[/scode]

for 循环时有id无id的区别。

<!--more-->

```
vm.dataList = [3, 4, 5, 6, 7] // 数据进行增删

  // 1. 没有key的情况， 节点位置不变，内容也更新了
  [
    '<div>3</div>', // id： A
    '<div>4</div>', // id:  B
    '<div>5</div>', // id:  C
    '<div>6</div>', // id:  D
    '<div>7</div>'  // id:  E
  ]

  // 2. 有key的情况， 节点删除了 A, B 节点，新增了 F, G 节点
  // <div v-for="i in dataList" :key='i'>{{ i }}</div>
  [
    '<div>3</div>', // id： C
    '<div>4</div>', // id:  D
    '<div>5</div>', // id:  E
    '<div>6</div>', // id:  F
    '<div>7</div>'  // id:  G
  ]
```

vue官方推荐for循环时。必须加上id，除非您的数据非常简单或者仅做展示。

其中涉及到dom的“定位“，加上了id才会按照你所想的进行实际上的变换

如果不加id可能只是您看上去进行了变化，但是本质没有变化
