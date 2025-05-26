---
title: Angular中库与应用共享服务遇到的问题以及解决方案
date: '2022-07-08 19:28'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  Angular中库与应用共享服务遇到的问题以及解决方案 背景
  随着项目的功能越来越多，我们计划抽离一些基础服务作为单一的库来整理优化项目结构，并且可以方便后期新开应用时更加方便的开发。 问题
  服务中涉及到国际化，我们想要的程度是：  能够一键切换语种  能够所有抽离的服务共享一个服务来设置国际化  ...
description: Angular中库与应用共享服务遇到的问题以及解决方案 - 详细介绍与实践经验分享
keywords:
  - Angular
  - 中库与应用共享服务遇到的问题以及解决方案
  - 技术
cover: ''
---

# Angular中库与应用共享服务遇到的问题以及解决方案

## 背景

随着项目的功能越来越多，我们计划抽离一些基础服务作为单一的库来整理优化项目结构，并且可以方便后期新开应用时更加方便的开发。

## 问题

服务中涉及到国际化，我们想要的程度是：

* 能够一键切换语种
* 能够所有抽离的服务共享一个服务来设置国际化
* 用户使用时尽可能的不需要自己设置，或者只需要设置一次

## 方案

### ~方案一~

使用拓展fone-ui的翻译服务，但是舍弃了，不能一键，并且如果使用了此种方案，必须强制依赖fone-ui，这是我不能接受的。

### 方案二

应用中常用的国际化方案是`ngx-translate`,并且通过查看文档，也是支持拓展的，支持一键切换（应用天然使用此库）

## demo

核心库代码

```
import { Injectable, } from '@angular/core';
import { TranslateService } from '@ngx-translate/core';
import zh_CN from './zh-CN';
import en_US from './en-US';

@Injectable({
  providedIn: 'root',
})
export class TestService {
  constructor(private translate: TranslateService) {
    this.translate.setTranslation('zh-CN', zh_CN, true);
    this.translate.setTranslation('en-US', en_US, true);
  }
}
```

构建之后`npm link`

应用核心代码**AppComponent**

```
import { Component } from '@angular/core';
import { TranslateService } from '@ngx-translate/core';

const zh_CN = {
  lindo: 'lindo11111',
};
const en_US = {
  lindo: 'lindo2222',
};

@Component({
  selector: 'my-test-app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {
  title = 'test';
  constructor(private translate: TranslateService) {
    this.translate.setTranslation('zh-CN', zh_CN);
    this.translate.setTranslation('en-US', en_US);
  }
}
```

**SubComponentComponent**

```
import { Component, OnInit } from '@angular/core';
import { TranslateService } from '@ngx-translate/core';
import { TestService } from '@caoxicheng/test';

@Component({
  selector: 'my-test-app-sub-component',
  templateUrl: './sub-component.component.html',
  styleUrls: ['./sub-component.component.scss'],
})
export class SubComponentComponent implements OnInit {
  constructor(private translate: TranslateService, private test: TestService) {}

  ngOnInit(): void {}
}
```

项目启动之后直接错误

![image.png](2257690684.png)

也去官方查看这个问题的描述

> [https://angular.cn/errors/NG0203](https://angular.cn/errors/NG0203)

通过正常的办法没办法正常的注入依赖

也尝试过绕过去的办法，比如说

![image.png](935070070.png)

或者使用`forwardRef`关键字，均无效

## 曙光

在查看官方的[依赖提供者](https://angular.cn/guide/dependency-injection-providers)一章时看到了这段文字

> 要想根据运行前尚不可用的信息创建可变的依赖值，可以使用工厂提供者。

感觉可行，无非就是绕呗

**修改后代码(SubComponentModule)**

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { SubComponentComponent } from './sub-component.component';
import { TranslateService } from '@ngx-translate/core';
import { TestService } from '@lindo/test';

@NgModule({
  declarations: [SubComponentComponent],
  imports: [CommonModule],
  exports: [SubComponentComponent],
  providers: [
    {
      provide: TestService,
      useFactory: (translate: TranslateService) => new TestService(translate),
      deps: [TranslateService],
    },
  ],
})
export class SubComponentModule {}
```

## 遗留问题

对于用户而言在注入的地方需要手动提供工厂函数，这一点也不**优雅**这是不能接受的。

所以说是否可以优化这个问题？
