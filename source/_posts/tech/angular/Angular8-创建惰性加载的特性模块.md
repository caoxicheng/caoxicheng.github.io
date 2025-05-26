---
title: Angular8 创建惰性加载的特性模块
date: '2019-12-10 11:42'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  借着这次重构项目的机会，复习一下以前并不怎么熟悉的方法步骤 <!--more--> 1. 建立应用 ： ng new customer-app
  --routing 2. 建立一个带路由的特性模块 ： ng genetare module customer--routing    -
  Customer...
description: Angular8 创建惰性加载的特性模块 - 详细介绍与实践经验分享
keywords:
  - Angular
  - 创建惰性加载的特性模块
  - 技术
cover: ''
---

借着这次重构项目的机会，复习一下以前并不怎么熟悉的方法步骤

<!--more-->

1. 建立应用 ： `ng new customer-app --routing`
2. 建立一个带路由的特性模块 ： `ng genetare module customer--routing`
   - CustomerModule
   - CustomerRoutingModle
   - CLI 会自动在CustomerModule中导入CustomerRoutingModule
3. 主路由中配置路由

```
const routes:Routes = [
       {
           path: 'customer',
           loadChildren: () => import('./customers/customer.module').then( mod => mod.CustomerModule)
       },
       ...
       {
           path: '',
           redirectTo: '',
           pathMath: 'full'
       }
   ]
```

4. 配置该特性模块的路由

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { CustomerListComponent } from './customer-list/customer-list.component';


const routes: Routes = [
  {
    path: '',
    component: CustomerListComponent
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class CustomersRoutingModule { }
```

[scode type="blue"]注意，path 被设置成了空字符串。这是因为 AppRoutingModule 中的路径已经设置为了 customers，所以 CustomersRoutingModule 中的这个路由定义已经位于 customers 这个上下文中了。也就是说这个路由模块中的每个路由其实都是子路由。[/scode]
