---
title: 在angualr8.0版本使用echarts插件
date: '2019-10-09 15:03'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  公司需求使用到图标功能，正好水一篇文章→→ <!--more--> 1.npm 安装 ECharts > 在 3.1.1 版本之前 ECharts 在
  npm 上的 package 是非官方维护的，从 3.1.1 开始由官方 EFE 维护 npm 上 ECharts 和 zrender 的 pack...
description: 在angualr8.0版本使用echarts插件 - 详细介绍与实践经验分享
keywords:
  - angualr
  - 版本使用
  - echarts
  - 技术
  - Angular
cover: ''
---

公司需求使用到图标功能，正好水一篇文章→_→

<!--more-->

1.npm 安装 ECharts

> 在 3.1.1 版本之前 ECharts 在 npm 上的 package 是非官方维护的，从 3.1.1 开始由官方 EFE 维护 npm 上 ECharts 和 zrender 的 package。
> 你可以使用如下命令通过 npm 安装 ECharts

`npm install echarts --save`

2.引入 ECharts

通过 npm 上安装的 ECharts 和 zrender 会放在node_modules目录下。可以直接在项目代码中 require('echarts') 得到 ECharts。

```
import * as echarts from 'echarts'

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
    title: {
        text: 'ECharts 入门示例'
    },
    tooltip: {},
    xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
});
```

---

有些教程需要本地加载静态文件在.angular-cli.json中的scrtpt 或者在 index.html 中导入js文件。不过并不是所有的页面都需要用到，所以并不是太聪明的办法。不过大多与情况下，`Done is better than perfect.`
