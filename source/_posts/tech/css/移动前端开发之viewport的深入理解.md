---
title: 移动前端开发之viewport的深入理解
date: '2019-04-28 18:20'
tags: []
categories:
  - 技术
  - CSS
excerpt: >-
  在移动设备上进行网页的重构或开发，首先得搞明白的就是移动设备上的viewport了，只有明白了viewport的概念以及弄清楚了跟viewport有关的meta标签的使用，才能更好地让我们的网页适配或响应各种不同分辨率的移动设备。
  <!--more--> 直接贴CODE <meta name="vi...
description: 移动前端开发之viewport的深入理解 - 详细介绍与实践经验分享
keywords:
  - 移动前端开发之
  - viewport
  - 的深入理解
  - 技术
  - CSS
cover: ''
---

在移动设备上进行网页的重构或开发，首先得搞明白的就是移动设备上的viewport了，只有明白了viewport的概念以及弄清楚了跟viewport有关的meta标签的使用，才能更好地让我们的网页适配或响应各种不同分辨率的移动设备。

<!--more-->

直接贴CODE

```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
```

---

width            设置layout viewport  的宽度，为一个正整数，或字符串"width-device"
initial-scale    设置页面的初始缩放值，为一个数字，可以带小数
minimum-scale    允许用户的最小缩放值，为一个数字，可以带小数
maximum-scale    允许用户的最大缩放值，为一个数字，可以带小数
height            设置layout viewport  的高度，这个属性对我们并不重要，很少使用
user-scalable    是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许

这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。

此外，在安卓中还支持  target-densitydpi  这个私有属性，它表示目标设备的密度等级，作用是决定css中的1px代表多少物理像素

target-densitydpi     值可以为一个数值或 high-dpi 、 medium-dpi、 low-dpi、 device-dpi 这几个字符串中的一个

特别说明的是，当 target-densitydpi=device-dpi 时， css中的1px会等于物理像素中的1px。

因为这个属性`只有安卓支持`，并且安卓已经`决定要废弃`target-densitydpi  这个属性了，所以这个属性我们要`避免进行`使用  。

具体的内容篇幅移步：[无双大神的移动前端开发之viewport的深入理解][1]

[1]: http://www.cnblogs.com/2050/p/3877280.html
