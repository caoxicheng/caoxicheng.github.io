---
title: 如何优化angular-cli构建速度
date: 2021-07-15 17:28
tags:
---

# 优化 angular cli build 速度

## 增加内存

一般会增加一个nodejs命令来提高打包时分配的内存,比如说

```
node --max_old_space_size=8196 ./node_modules/@angular/cli/bin/ng build --prod
```

但是实际测试发现,分配大额内存对于构建速度是`负优化`

## 修改打包配置项

** 注意,本人用于gitlab-CI 的速度优化,而非实际生产环境配置 **

> angular.json

```
"optimization": false,
    "outputHashing": "none",
    "sourceMap": false,
    "extractCss": true,
    "namedChunks": false,
    "showCircularDependencies": false,
    "aot": true,
    "extractLicenses": false,
    "statsJson": false,
    "progress": false,
    "vendorChunk": true,
    "buildOptimizer": false,
```

当然可以复制追加一个自定义的命名配置,直接通过npm脚本运行

```
ng build --configuration=myenv
```

这里提供一个思路,但是具体配置项,可以根据需要微调,毕竟,速度前提是`正确`!

ps: 附上本机实际时间对比

> 效果斐然

| 命令                                                                               | 时间       |
|:-------------------------------------------------------------------------------- |:-------- |
| `node --max_old_space_size=8196 ./node_modules/@angular/cli/bin/ng build --prod` | 3.4447分钟 |
| `ng build --prod`                                                                | 2.8793分钟 |
| `改打包配置文件`                                                                        | 1.1223分钟 |
