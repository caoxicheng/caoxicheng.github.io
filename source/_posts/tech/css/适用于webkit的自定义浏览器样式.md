---
title: 适用于webkit的自定义浏览器样式
date: '2020-08-17 14:01'
tags: []
categories:
  - 技术
  - CSS
excerpt: >-
  前言：
  现在越来越要求“表面功夫”了，几乎所有的手机端浏览器的滚动条都是Mini版本，近些年来，部分PC端的前端页面的滚动条也开始变得妖艳贱货了起来。这里做下记录方便个人查看
  --- [scode type="blue"]必须为webkit内核，即样式前缀需支持-webkit-, 也可配合css选择...
description: 适用于webkit的自定义浏览器样式 - 详细介绍与实践经验分享
keywords:
  - 适用于
  - webkit
  - 的自定义浏览器样式
  - 技术
  - CSS
cover: ''
---

前言： 现在越来越要求“表面功夫”了，几乎所有的手机端浏览器的滚动条都是Mini版本，近些年来，部分PC端的前端页面的滚动条也开始变得妖艳贱货了起来。这里做下记录方便个人查看

---

[scode type="blue"]必须为webkit内核，即样式前缀需支持-webkit-,
也可配合css选择使用，若不搭配使用，则为全局样式[/scode]

| 伪元素         | 选择器                             | 备注               |
|:----------- |:------------------------------- |:---------------- |
| 滚动条         | ::-webkit-scrollbar             | 整体               |
| 滚动条按钮       | ::-webkit-scrollbar-button      | 上下方向键            |
| 滚动条轨道       | ::-webkit-scrollbar-track       | 滚动条滚动的上下去区间      |
| 滚动条-补偿      | ::-webkit-scrollbar-track-piece | 滚动条轨道-滚动条实体的部分   |
| 滚动条-实体      | ::-webkit-scrollbar-thumb       | 滚动条实体            |
| 水平垂直滚动条交接部分 | ::-webkit-scrollbar-corner      | 基本为页面右下角         |
| 拖动区         | ::-webkit-resize                | 与滚动条交接区重合，可以重设大小 |

[scode type="share"]tips:我们可以为这些对象中的每个对象设置边框，阴影，背景图像等样式，以创建完全自定义的滚动条，当然，且没有疑惑这些滚动条在按钮位置和单击行为方面仍将遵循操作系统的设置。[/scode]

另外，引入了以下伪类并将其应用于伪元素。

`:horizontal` –水平伪类适用于任何具有水平方向的滚动条。

`:vertical` –垂直伪类适用于任何具有垂直方向的滚动条。

`:decrement`–减量伪类适用于按钮和轨道块。它指示按钮或轨道块在使用时是否会降低视图的位置（例如，在垂直滚动条上向上，在水平滚动条上左侧）。

`:increment`–增量伪类适用于按钮和跟踪片段。它指示按钮或轨迹块在使用时是否会增加视图的位置（例如，在垂直滚动条上向下，在水平滚动条上右侧）。

`:start– start`伪类适用于按钮和跟踪片段。它指示对象是否放置在拇指之前。

`:end`–结束伪类适用于按钮和跟踪片段。它指示对象是否放置在拇指后面。

`:double-button`–双按钮伪类适用于按钮和轨道块。它用于检测一个按钮是否是位于滚动条同一端的一对按钮的一部分。对于履带板，它指示履带板是否邻接一对按钮。

`:single-button`–单按钮伪类适用于按钮和轨道块。它用于检测按钮是否位于滚动条末尾。对于履带板，它指示履带板是否邻接单例按钮。

`:no-button` –适用于曲目片断，并指示曲目片断是否延伸到滚动条的边缘，即，曲目的那端没有按钮。

`:corner-present` –适用于所有滚动条，并指示是否存在滚动条角。

`:window-inactive`–适用于所有滚动条，并指示包含滚动条的窗口当前是否处于活动状态。（在最近的夜间，此伪类现在也适用于:: selection。我们计划将其扩展为可处理任何内容，并将其提议为新的标准伪类。）

此外，：enabled，：disabled，：hover和：active伪类也可以与滚动条一起使用。

可以将display属性设置为none，以便隐藏特定的作品。

沿滚动条的轴支持边距。它们可以是负数（例如，可以使轨道膨胀以部分覆盖按钮）。

举个例子：

```
::-webkit-scrollbar:vertical {
  -webkit-border-image: url(resources/vertical-button.png) 2 0 2 0;
  border-color: transparent;
  border-width: 2px 0;
  background-image: url(resources/vertical-button-background.png);
  background-repeat: repeat-y;
}

::-webkit-scrollbar-track-piece:disabled {
  display: none !important;
}


::-webkit-scrollbar-track:vertical:disabled {
  -webkit-border-image: url(resources/vertical-track-disabled.png) 13 0 13 0;
  border-color: transparent;
  border-width: 13px 0;
}


/** 或者 **/
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
::-webkit-scrollbar{
    width: 3px;
    height: 16px;
    background-color: rgba(255,255,255,0);
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track{
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    border-radius: 10px;
    background-color: rgba(255,255,255,0);
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb{
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
    background-color: #555;
}
```

[样式滚动条][1]

[1]: https://webkit.org/blog/363/styling-scrollbars/
