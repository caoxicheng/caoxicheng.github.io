---
title: 在Angular8中引入lodash
date: 2019-12-25 14:39
tags:
---

## npm安装

```
npm install lodash --save
```

## types定义

TypeScript 的解決方案是另外使用 *.d.ts 。

一般来说，若是知名的 JavaScript library，都已经有人维护 type 定义包，其 package 的规则是 @types/package 。

```
npm install @types/lodash --save
```

## tsconfig.app.json

```
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/app",
    "types": ["swiper","lodash"] //因为我项目还引入了swiper
  },
  "include": [
    "src/**/*.ts"
  ],
  "exclude": [
    "src/test.ts",
    "src/**/*.spec.ts"
  ]
}
```

## ts文件中

```
import * as _ from 'lodash';
```

至此就能在ts中使用了

## 结论

实际上若有 Angular 版本的 package，当然应优先使用；若遇到必须使用 JavaScript package 不可的场合，除了安装 package 外，还要安装 type 定义包，并在 tsconfig.json 设定，如此才可以在 Angular 正确使用并通过编译
