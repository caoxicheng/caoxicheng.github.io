---
title: Angular4+中使用jsonp获取API数据
date: 2020-01-09 10:46
tags:
---

## 导入AngularModule

<p class="center" style="color:red">在 app.module.ts 中引入 HttpClientModule、HttpClientJsonpModule 并注入</p>

```
import {HttpClientModule,HttpClientJsonpModule} from '@angular/common/http';

imports: [
    BrowserModule,
    HttpClientModule,
    HttpClientJsonpModule,
    ...
```

## needed-service.ts

<p class="center" style="color:red">在用到的地方引入 HttpClient 并在构造函数声明</p>

```
import { HttpClient } from '@angular/common/http';

constructor( 
      private http: HttpClient,
    ) { }

demo(): Observable<any> {
    return this.http.jsonp(`http://api.wipmania.com/jsonp`, 'callback')
  }
```

## needed-component.ts

<p class="center" style="color:red">jsonp 请求数据</p>

```
import { needed-service } from 'xxx';

constructor(
    private needService: NeedService,
  ) { }

demo() {
this.neededService.demo().subscribe( res => {console.log(res);})
}
```
