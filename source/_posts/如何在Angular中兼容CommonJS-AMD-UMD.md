---
title: 如何在Angular中兼容CommonJS/AMD/UMD
tags: []
excerpt: 在导入类似 moment 这样的开发工具时,因为没有es的import导入,所以我们可能会使用import * as moment from 'moment
date: 2022-10-27 10:46:00
---

在导入类似 moment 这样的开发工具时,因为没有es的import导入,所以我们可能会使用import \* as moment from 'moment
<!-- more -->
在导入类似 `moment` 这样的开发工具时,因为没有es的import导入,所以我们可能会使用

```
import * as moment from 'moment
```

[\[...\]](https://godlike.top/archives/695/ "如何在Angular中兼容CommonJS/AMD/UMD")