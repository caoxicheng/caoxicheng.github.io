---
title: CI 中 Cypress 下载超时处理方案
date: '2022-08-18 20:13'
tags: []
categories:
  - 技术
  - 工程化
excerpt: >-
  前言 在项目中我们依赖了cypress，在跑ci/cd时，时常遇到超时错误。
  最终定位问题，我们发现时这个cypress插件会自己下载二进制版本，或者直接超时。 排查错误 毫无疑问，我们一开始就定位到了网络问题。
  并且也通过日志定位了到了这个问题 .../cypress@9.7.0/nodemodu...
description: CI 中 Cypress 下载超时处理方案 - 详细介绍与实践经验分享
keywords:
  - Cypress
  - 下载超时处理方案
  - 技术
  - 工程化
cover: ''
---

## 前言

在项目中我们依赖了`cypress`，在跑ci/cd时，时常遇到超时错误。

最终定位问题，我们发现时这个`cypress`插件会自己下载二进制版本，或者直接超时。

## 排查错误

毫无疑问，我们一开始就定位到了**网络问题**。
并且也通过日志定位了到了这个问题

```
.../cypress@9.7.0/node_modules/cypress postinstall$ node index.js --exec install
.../cypress@9.7.0/node_modules/cypress postinstall: Installing Cypress (version: 9.7.0)
.../cypress@9.7.0/node_modules/cypress postinstall: [STARTED] Task without title.
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] The Cypress App could not be downloaded.
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] 
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] Does your workplace require a proxy to be used to access the Internet? If so, you must configure the HTTP_PROXY environment variable before downloading Cypress. Read more: https://on.cypress.io/proxy-configuration
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] 
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] Otherwise, please check network connectivity and try again:
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] 
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] ----------
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] 
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] URL: https://download.cypress.io/desktop/9.7.0?platform=linux&arch=x64
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] Error: read ETIMEDOUT
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] 
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] ----------
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] 
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] Platform: linux-x64 (Debian - 10.11)
.../cypress@9.7.0/node_modules/cypress postinstall: [FAILED] Cypress Version: 9.7.0
.../cypress@9.7.0/node_modules/cypress postinstall: The Cypress App could not be downloaded.
.../cypress@9.7.0/node_modules/cypress postinstall: Does your workplace require a proxy to be used to access the Internet? If so, you must configure the HTTP_PROXY environment variable before downloading Cypress. Read more: https://on.cypress.io/proxy-configuration
.../cypress@9.7.0/node_modules/cypress postinstall: Otherwise, please check network connectivity and try again:
.../cypress@9.7.0/node_modules/cypress postinstall: ----------
.../cypress@9.7.0/node_modules/cypress postinstall: URL: https://download.cypress.io/desktop/9.7.0?platform=linux&arch=x64
.../cypress@9.7.0/node_modules/cypress postinstall: Error: read ETIMEDOUT
.../cypress@9.7.0/node_modules/cypress postinstall: ----------
.../cypress@9.7.0/node_modules/cypress postinstall: Platform: linux-x64 (Debian - 10.11)
.../cypress@9.7.0/node_modules/cypress postinstall: Cypress Version: 9.7.0
.../cypress@9.7.0/node_modules/cypress postinstall: Failed
 ELIFECYCLE  Command failed with exit code 1.
```

当然，比较简单的办法就是给服务器开通网络代理，不过，出于安全问题，不允采用。

通过查看`cypress`的[官方文档][1]，也得知了几种方案

- 跳过安装
- 缓存
- CDN
- 镜像

社区论坛上反馈 `cypress` 的版本检查程序有问题，可以通过固定版本号来绕开一部分超时错误

当然比较实在的就是跳过和缓存，缓存的话，可以参考我的其他文章如何提供pnpm镜像环境[post cid="690" /]

跳过安装比较实在，因为我们已经安装了node版本的cypress不再需要安装二进制版本了

## 妙手

方案敲定，直接修改ci文件

```.gitlab-ci.yml
variables:
  CYPRESS_INSTALL_BINARY: 0
```

## 遗留问题

安装一些依赖的时候依旧很吃网络环境。公司内部也通过`verdaccio`部署了自己的注册表，并且天然的提供缓存服务。
所以乘着这次修改，直接一起转移过去吧。

[1]: https://docs.cypress.io/guides/references/advanced-installation#Skipping-installation
