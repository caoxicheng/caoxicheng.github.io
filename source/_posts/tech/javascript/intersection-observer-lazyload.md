---
title: 「交叉观察者」实现懒加载、触底、吸顶等操作
date: '2019-08-29 17:38'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  说到 懒加载、 菜单吸顶、触底等操作，一般方法是监听浏览器的滚动条，满足条件触发相应的事件。 IntersectionObserver
  交叉观察者，现在可以优雅的完成类似的操作。 介绍 > IntersectionObserver接口 (从属于Intersection Observer
  API) 提...
description: 「交叉观察者」实现懒加载、触底、吸顶等操作 - 详细介绍与实践经验分享
keywords:
  - 交叉观察者
  - 实现懒加载
  - 吸顶等操作
  - 技术
  - JavaScript
cover: ''
---

说到 *懒加载*、 *菜单吸顶*、*触底*等操作，一般方法是监听浏览器的滚动条，满足条件触发相应的事件。

`IntersectionObserver` 交叉观察者，现在可以优雅的完成类似的操作。

## 介绍

> IntersectionObserver接口 (从属于Intersection Observer API) 提供了一种异步观察目标元素与其祖先元素或顶级文档视窗(viewport)交叉状态的方法。祖先元素与视窗(viewport)被称为根(root)。

大白话就是异步的监听被观察对象与其祖先元素或者顶级视窗（默认）是否交叉.

## 用法

### 1.构造函数

```
new IntersectionObserver(callback, options);
```

### 2.callback

callback 接受2个参数 `entries` & `observer`

`entries`: 当前已监听`并且发生了交叉的`目标集合
`observer`:被调用的IntersectionObserver实例本身 *一般用不到，可以不传*

```
new IntersectionObserver((entries, observer) => {
  entries.forEach(item => console.log(item, observer));
  // ...
});
```

看看 `item` 的常用属性及其说明

| 属性                 | 说明                              |
| ------------------ | ------------------------------- |
| boundingClientRect | 目标元素的边界信息                       |
| intersectionRatio  | 元素可见区域的占比                       |
| isIntersecting     | 目标元素与交叉区域观察者对象的根是否相交            |
| time               | 从时间原点(time origin)到交叉被触发的时间的时间戳 |
| target             | 目标元素                            |

[scode type="yellow"]页面初始化时会触发一次callback， entries 为所有目标元素的集合[/scode]

### 2.option

参数列表，`非必填`

| 属性         | 说明                                                  |
| ---------- | --------------------------------------------------- |
| root       | 指定父元素，默认为`视窗`                                       |
| rootMargin | 触发交叉的偏移值，默认为"0px 0px 0px 0px"（上左下右，正数为向外扩散，负数则向内收缩） |

```
new IntersectionObserver(callback, {
  root: document.querySelector("xx"),
  rootMargin: "0px 0px -100px 0px"
});
```

如果设置rootMargin为"20px 0px 30px 30px"，那么元素`未到`达视窗时，就已经切换为`可见状态`了：

### 3.方法

| 方法名           | 说明               |
| ------------- | ---------------- |
| disconnect()  | 停止监听工作（参数节点）     |
| observe()     | 开始监听一个目标元素（参数节点） |
| takeRecords() | 返回entris所有数组对象   |
| unobserve()   | 停止监听特定目标元素       |

## 举个栗子

### 1. 假设页面上有一个`class="box"`的盒子且父元素为`视窗`：

```
let box = document.querySelector(".box");

let observer = new IntersectionObserver(entries => {
  entries.forEach(item => {
    let tips = item.isIntersecting ? "进入了父元素的内部" : "离开了父元素的内部";
    console.log(tips);
  });
});

observer.observe(box); // 监听一个box
```

### 2. 假设页面上有`多个class="box"`的盒子且父元素为`视窗`：

```
let box = document.querySelectorAll(".box");

let observer = new IntersectionObserver(entries => console.log(`发生交叉行为，目标元素有${entries.length}个`));

box.forEach(item => observer.observe(item)); // 监听多个box
```

[scode type="yellow"]如果三个box高度相同，大家都一起发生交叉，固每次返回的集合长度都为三；高度不同的情况则是每个目标元素轮流发生交叉，且当前只触发了一个，所以每次返回的集合长度只有一[/scode]

### 3.指定父元素

`html`如下

```
<div class="parent">
  <div class="child"></div>
</div>
```

`javascript`

```
let child = document.querySelector(".child");

let observer = new IntersectionObserver(entries => {
  entries.forEach(item => {
    console.log(item.isIntersecting ? "可见" : "不可见");
  });
}, {
  root: document.querySelector(".parent")
});

observer.observe(child); // 开始监听child
```

与`视图`情况无差别

## 实际应用

### 懒加载

```
let images = document.querySelectorAll("img.lazyload");

let observer = new IntersectionObserver(entries => {
  entries.forEach(item => {
    if (item.isIntersecting) {
      item.target.src = item.target.dataset.origin; // 开始加载图片
      observer.unobserve(item.target); // 停止监听已开始加载的图片
    }
  });
});

images.forEach(item => observer.observe(item));
```

### 触底

我们在列表底部放一个`参照元素`，然后让交叉观察者去监听；

假设`html`结构如下：

```
<!-- 数据列表 -->
<ul>
  <li>index</li>
</ul>

<!-- 参照元素 -->
<div class="reference"></div>
```

`javascript`

```
new IntersectionObserver(entries => {
  let item = entries[0]; // 拿第一个就行，反正只有一个
  if (item.isIntersecting) console.log("滚动到了底部，开始请求数据");
}).observe(document.querySelector(".reference")); // 监听参照元素
```

### 3. 吸顶

实现元素吸顶的方式有很多种，如css的`position: sticky`，兼容性较差；如果用交叉观察者实现也很方便，同样也要放一个`参照元素`；

假设`html`结构如下：

```
<!-- 参照元素 -->
<div class="reference"></div>

<nav>我可以吸顶</nav>
```

假设`css`如下

```
.nav .fixed {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
  }
```

`javascript`

```
let nav = document.querySelector('nav');
let reference = document.querySelector(".reference");

new IntersectionObserver(entries => {

  let item = entries[0];
  let top = item.boundingClientRect.top;

  // 当参照元素的的top值小于0，也就是在视窗的顶部的时候，开始吸顶，否则移除吸顶
  if (top < 0) nav.classList.add("fixed");
  else nav.classList.remove("fixed");

}).observe(reference);
```

## 兼容

**与IE不兼容不过有**[官方的polyfill][1]

## 最后

必须是子元素跟父元素发生交叉，如果你想检查两个非父子关系的交叉，那是不行的嘻嘻😊

[scode type="yellow"]本文参考了下文的连接，并稍微自行增加或删减了一些部分[/scode]
[scode type="share"]利用"交叉观察者"这个小宝贝儿，轻松实现懒加载、吸顶、触底 ❗ https://juejin.im/post/5d665133e51d4561c83e7c83#heading-12[/scode]

[1]: https://github.com/w3c/IntersectionObserver/tree/master/polyfill
