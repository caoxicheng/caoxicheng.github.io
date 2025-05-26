---
title: 在Angular中使用RxJS完成接口数据的缓存
date: '2019-12-16 16:16'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  在实际项目中，很多共用的接口返回的数据是一样的，这样许多页面多次调用会造成性能浪费，可以使用RxJS的ReplaySubject(size)发送之前的旧值给新的订阅者。
  <!--more--> 用ReplaySubject(size)可以发送之前的旧值给新的订阅者，size是定义发送具体多少个旧值给...
description: 在Angular中使用RxJS完成接口数据的缓存 - 详细介绍与实践经验分享
keywords:
  - Angular
  - 中使用
  - RxJS
  - 完成接口数据的缓存
  - 技术
cover: ''
---

在实际项目中，很多共用的接口返回的数据是一样的，这样许多页面多次调用会造成性能浪费，可以使用RxJS的ReplaySubject(size)发送之前的旧值给新的订阅者。

<!--more-->

用`ReplaySubject(size)`可以发送之前的旧值给新的订阅者，size是定义发送具体多少个旧值给新的订阅者。

> shareReplay这个操作符会自动创建一个ReplaySubject，一旦http request执行一次以后，就会在后续的订阅和源头Observable之间建立一个ReplaySubject，ReplaySubject是一个多播的Hot Observable，后续订阅都是从这个中间ReplaySubject拿到最后一个值，从而达到缓存效果。

方法与正常调用服务层，相差无几，只是多了一个"中间商"

在cacheServices.ts中

```
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";

import { map, catchError, shareReplay } from 'rxjs/operators';
import { of, Observable } from 'rxjs';


const CACHE_SIZE = 1;

@Injectable()

export class cacheServices{

    private cacheTemp:Observable<any>;

    constructor(private http: HttpClient) { }

    chache() {
        if(!this.cacheTemp){
            this.cacheTemp= this._chache()
            .pipe(
                shareReplay(CACHE_SIZE)
            );
        }
       return this.cacheTemp;
    }

    private _chache() {
        return this.http.get<any>("xxx")
            .pipe(
                map(respone => respone),
                catchError(error => {
                    console.log("something went wrong " + error)
                    return of([]);
                })
            )
    }
}
```

页面的第一个请求是调用API拿到信息，第二个调用，直接从cacheTemp拿到这个缓存信息。cacheTemp是ReplaySubject(1)把最后一个旧值（api return）发送给新的订阅者，从而实现了缓存效果。

[RxJS：如何通过RxJS实现缓存][1]

[1]: https://limeii.github.io/2019/08/rxjs-caching/
