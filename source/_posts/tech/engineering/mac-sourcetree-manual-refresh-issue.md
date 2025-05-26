---
title: 「Mac」SourceTree 手动拉取代码每次都需要自己手动刷新列表
date: '2022-01-05 21:57'
tags: []
categories:
  - 技术
  - 工程化
excerpt: >-
  起因 原本我的GIT是通过Xcode安装的,至于为什么不用brew安装,我已经忘记为什么了. 今天突然心血来潮,把Xcode给卸载了.然后 brew
  install git,很好,git也支持中文了! 既然版本最新了,那就顺手在SourceTree的Git使用自己下载的吧, 事发 没想到这样一来就出...
description: 「Mac」SourceTree 手动拉取代码每次都需要自己手动刷新列表 - 详细介绍与实践经验分享
keywords:
  - Mac
  - SourceTree
  - 手动拉取代码每次都需要自己手动刷新列表
  - 技术
  - 工程化
cover: ''
---

## 起因

原本我的GIT是通过`Xcode`安装的,至于为什么不用`brew`安装,我已经忘记为什么了.

今天突然心血来潮,把`Xcode`给卸载了.然后 `brew install git`,很好,git也支持中文了!

既然版本最新了,那就顺手在`SourceTree`的`Git`使用自己下载的吧,

## 事发

没想到这样一来就出问题了

![image.png](3409847682.png)

无法自动更新代码了.

## 解决

重新使用内置版本解决问题!
