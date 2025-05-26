---
title: Z-index失效的几种原因
date: '2019-04-26 17:04'
tags: []
categories:
  - 技术
  - CSS
excerpt: >-
  Tips: ===== 1. z-index属性只作用在被定位了的元素上。所以如果你在一个没被定位的元素上使用z-index的话，是不会有效果的. 2.
  同一个父元素下的元素的层叠效果会受父元素的z-index影响,如果父元素的z-index值很小,那么子元素的z-index值很大也不起作用 失效的...
description: Z-index失效的几种原因 - 详细介绍与实践经验分享
keywords:
  - index
  - 失效的几种原因
  - 技术
  - CSS
cover: ''
---

Tips:
=====

1. z-index属性**只作用在被定位了的元素上**。所以如果你在一个没被定位的元素上使用z-index的话，是不会有效果的.
2. 同一个父元素下的元素的层叠效果会受父元素的z-index影响,如果**父元素的z-index值**很小,那么子元素的z-index值很大也不起作用

失效的情况：

- 父标签 position属性为relative；
- 问题标签无position属性（不包括static）；
- 问题标签含有浮动(float)属性。
- 问题标签的祖先标签的z-index值比较小

解决办法：

- 第一种:  position:relative改为position:absolute；
- 第二种:浮动元素添加position属性（如relative，absolute等）；
- 第三种:去除浮动。
- 第四种:提高父标签的z-index值
