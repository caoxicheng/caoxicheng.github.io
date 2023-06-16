---
title: 如何在Angular中兼容CommonJS/AMD/UMD
tags: []
excerpt: 在导入类似 moment 这样的开发工具时,因为没有es的import导入,所以我们可能会使用import * as moment from 'moment
date: 2022-10-27 10:46:00
---

在导入类似 `moment` 这样的开发工具时,因为没有es的import导入,所以我们可能会使用

```
import * as moment from 'moment
```

<!--more-->

不过最近发现通过**此种方式开发插件库**,在应用端会报找不到函数

研究后发现可以通过ts的配置来修复此问题

```tsconfig.json
compilerOptions: {
    "esModuleInterop": true, // 兼容CommonJS/AMD/UMD (官方文档说此配置项会自动打开allowSyntheticDefaultImports)
    "allowSyntheticDefaultImports": true, // 兼容CommonJS/AMD/UMD
}
```

> [ES 模块互操作 - esModuleInterop][1]

[1]: https://www.typescriptlang.org/tsconfig#esModuleInterop
