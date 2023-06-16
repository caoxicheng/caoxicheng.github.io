---
title: Angular项目中引入Swiper轮播插件
date: 2019-12-17 15:28
tags:
---

在网上找了很多资料，有的正确有的错误，当然最简单就是在index.html引入cdn 然后在ts文件中declare 一下，不过这种方法看上去就有点不是很正确，于是找一下相对正确的办法

<!--more-->

## npm 安装 swiper

`npm install swiper --save`
或者
`yarn add swiper --save`

## 导入静态资源

angular.json中添加css和js

```
...
            "styles": [
              ...
              "./node_modules/swiper/css/swiper.min.css",
              ...
            ],
            "scripts": [
              ...
              "./node_modules/swiper/js/swiper.min.js",
              ...
            ]
```

## 安装模组定义档（暂时不知什么意思）

`npm install @types/swiper --save`

or

`yarn add @types/swiper --save`

> 为你的库下载类型信息（npm install @types/jquery），并按照库的安装步骤进行操作。这让你可以访问该库暴露出来的全局变量。2020/7/1 有了这个类型信息，就不用使用类似'declare var libraryName: any;'来申明类型了

## 配置tsconfig文件

```
"typeRoots": [
      "node_modules/@types",
    ],
```

## tsconfig.app.json

```
"types": ["swiper"]
```

## conponment.html

```
<div class="swiper-container">
    <div class="swiper-wrapper">
      <div class="swiper-slide" *ngFor="let data of slides">
        <img [src]="data" alt="" width="100%">
      </div>
    </div>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>

    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>

    <!-- 如果需要滚动条 -->
    <div class="swiper-scrollbar"></div>
  </div>
```

## component.ts

```
import { Component, OnInit, AfterViewInit } from '@angular/core';
import Swiper from "swiper";

@Component({
  selector: 'app-index',
  templateUrl: './index.component.html',
  styleUrls: ['./index.component.css']
})
export class IndexComponent implements OnInit,AfterViewInit {


  swiper: Swiper;
  slides = [
    'https://via.placeholder.com/300x200/FF5733/ffffff',
    'https://via.placeholder.com/300x200/C70039/ffffff',
    'https://via.placeholder.com/300x200/900C3F/ffffff'
  ];

  constructor() { }

  ngOnInit() {

  }

  ngAfterViewInit() {
    this.swiper = new Swiper('.swiper-container', {
      direction: 'horizontal',
      loop: true,
      // 如果需要分页器
      pagination: {
        el: '.swiper-pagination',
      },
      // 如果需要前进后退按钮
      navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
      },
      // 如果需要滚动条
      scrollbar: {
        el: '.swiper-scrollbar',
      },
    });
  }
}
```

[scode type="blue"]具体的配置文档请移步官方[/scode]
[API文档][1]

[1]: https://www.swiper.com.cn/api/index.html
