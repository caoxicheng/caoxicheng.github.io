---
title: angualr4创建统一http拦截器
date: '2019-09-05 17:57'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  意义 很多API都需要权限校验，所以在全局设置一个http的拦截器进行权限控制 <!--more--> intercept.ts [scode
  type="blue"]AuthService 、NzMessageService 是项目中的权限组件和消息提示组件。[/scode] import {
  I...
description: angualr4创建统一http拦截器 - 详细介绍与实践经验分享
keywords:
  - angualr
  - 创建统一
  - http
  - 拦截器
  - 技术
cover: ''
---

### 意义

很多API都需要权限校验，所以在全局设置一个http的拦截器进行权限控制

<!--more-->

### intercept.ts

[scode type="blue"]AuthService 、NzMessageService 是项目中的权限组件和消息提示组件。[/scode]

```
import { Injectable } from '@angular/core'
import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest, HttpResponse, HttpErrorResponse } from '@angular/common/http';

import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { AuthService } from '../service/auth/auth.service';
import { Router } from '@angular/router';

import { NzMessageService } from 'ng-zorro-antd';

@Injectable()
export class Interceptor implements HttpInterceptor {

    constructor(
      private authService : AuthService,
      private router: Router,
      private message: NzMessageService,
    ){}

    intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
      // -1- 从auth服务中获取用户信息
      const authToken = this.authService.getInfo()?this.authService.getInfo().token : null;

      //  -2- 克隆请求并替换掉(前提是获取到用户信息)
      let authReq = req.clone();;
      if (authToken) {
        authReq = req.clone({
          headers: req.headers.set('Authorization', authToken),
        });
      }

        return next.handle(authReq).pipe(
            tap((event: HttpEvent<any>) => {
              if (event instanceof HttpResponse) {
                // do stuff with response if you want
                // 接口错误
                if (event.body['Success'] === false) {
                  this.message.error(event.body.Message == '' ? 'Interface error' : event.body.Message);
                }
              }
            }
            , (err: any) => {
                if (err instanceof HttpErrorResponse) {
                  if (err.status === 401 || err.status === 400) {
                    // redirect to the login route
                    // or show a modala
                    // console.log('401');
                    this.router.navigate(['/welcome']);
                    // window.location.href="https://eip.bestwaycorp.com/";
                  }
                }
              }
            )
          );
    }


}
```

### index.ts(模块化)

目录结构

> http-intercept
> 
> > index.ts
> > intercept.ts

```
import { HTTP_INTERCEPTORS } from '@angular/common/http';

import { Interceptor } from './intercept';

export const httpInterceptorProviders = [
  { provide: HTTP_INTERCEPTORS, useClass: Interceptor, multi: true },
]
```

然后在`app.module.ts`的providers

```
import { httpInterceptorProviders } from './http-intercept/index';

providers: [httpInterceptorProviders]
```

---

### 或者无须index.ts

直接在`app.module.ts`中

```
import {HTTP_INTERCEPTORS} from "@angular/common/http";

providers: [
    {provide:HTTP_INTERCEPTORS,useClass:Interceptor ,multi:true}
  ],
```

[scode type="blue"]以上内容Angular4支持，其他版本请测试后使用！[/scode]
