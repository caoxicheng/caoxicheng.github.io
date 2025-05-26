---
title: 'Error: Cannot call a namespace (moment) 的处理方案'
date: '2022-06-07 13:41'
tags: []
categories:
  - 技术
  - 工程化
excerpt: >-
  提供一种曲线救国的思路 javascript import  as moment from 'moment'; const moment = moment;
  // 曲线救国 酌情修改下面参数 > allowSyntheticDefaultImports >  > skipLibCheck 参考 [s...
description: 'Error: Cannot call a namespace (moment) 的处理方案 - 详细介绍与实践经验分享'
keywords:
  - Error
  - Cannot
  - call
  - namespace
  - moment
cover: ''
---

提供一种曲线救国的思路

```javascript
import * as moment_ from 'moment';
const moment = moment_;
// 曲线救国
```

酌情修改下面参数

> `allowSyntheticDefaultImports`
> 
> `skipLibCheck`

参考

[scode type="share" size=""]https://stackoverflow.com/questions/59735280/angular-8-moment-error-cannot-call-a-namespace-moment[/scode]
