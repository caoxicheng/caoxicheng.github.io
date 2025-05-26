---
title: 创建前端PNPM自定义Docker镜像并上传至服务器使用
date: '2022-08-18 20:37'
tags: []
categories:
  - 技术
  - 工程化
excerpt: >-
  本地创建node镜像并提前安装pnpm环境 背景 因为需要在项目中使用pnpm包管理工具，并且通过only-allow强制使用pnpm 引出 ci
  也需要使用 pnpm 但是介于网络问题，安装一直错误，并且因为安全问题，也不能使服务器连接外网。 解决方案
  在一台网络正常的机器上构建一个已经预装好pn...
description: 创建前端PNPM自定义Docker镜像并上传至服务器使用 - 详细介绍与实践经验分享
keywords:
  - 创建前端
  - PNPM
  - 自定义
  - Docker
  - 镜像并上传至服务器使用
cover: ''
---

# 本地创建node镜像并提前安装pnpm环境

## 背景

因为需要在项目中使用`pnpm`包管理工具，并且通过`only-allow`强制使用`pnpm`

引出 ci 也需要使用 pnpm

但是介于网络问题，安装一直错误，并且因为安全问题，也不能使服务器连接外网。

## 解决方案

在一台网络正常的机器上构建一个已经预装好pnpm的docker镜像，并上传到服务器，使用本地镜像来解决问题

## 环境要求

> docker
> 
> 网络正常

## 

## 步骤

### 构建自定义镜像

本地新建目录来进行如下操作

```dockerfile
# 基于基础的node镜像
FROM node:16.13.2

RUN curl -f https://get.pnpm.io/v6.16.js | node - add --global pnpm@7 \
  && pnpm config set store-dir ~/.pnpm-store
```

开始构建

```bash
docker build -t name:tag .
```

验证

```bash
$ docker run -itd --name test node16:pnpm
$ docker ps // 获取id
$ docker exec -it id /bin/bash


// node -v && pnpm -v work!
```

开始创建镜像文件

```bash
$ docker save -o exampleName.tar name:tag
```

传输至服务器

```bash
$ scp exampleName.tar root@hostname:/data
```

服务器导入镜像

```bash
$ docker load < exampleName.tar
```

查看

```bash
$ docker images
```

> 可能需要重命名
> 
> $ $docker tag IMAGEID(镜像id) REPOSITORY:TAG(仓库：标签)
> 
> 删除
> 
> $ $docker rmi IMAGEID 或者 docker rmi REPOSITORY:TAG
