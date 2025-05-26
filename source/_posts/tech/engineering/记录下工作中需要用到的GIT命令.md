---
title: 记录下工作中需要用到的GIT命令
date: '2019-12-06 14:35'
tags: []
categories:
  - 技术
  - 工程化
excerpt: >-
  这里记录一下日常工作中使用的到的git命令 !git-zoom.png [scode type="blue"]关于git
  flow，新版本git已默认附带，使用需要初始化[/scode] | 指令                                   |
  作用             ...
description: 记录下工作中需要用到的GIT命令 - 详细介绍与实践经验分享
keywords:
  - 记录下工作中需要用到的
  - GIT
  - 技术
  - 工程化
cover: ''
---

这里记录一下日常工作中使用的到的git命令

![git-zoom.png](1369499882.png)

[scode type="blue"]关于git flow，新版本git已默认附带，使用需要初始化[/scode]

| 指令                                   | 作用                                                         |
|:------------------------------------:|:----------------------------------------------------------:|
| git branch `-d` branch-name          | 删除本地分支                                                     |
| git branch `-D` branch-name          | 强制删除本地（没有合并的）分支                                            |
| git push `origin --delete` name      | 删除远程分支                                                     |
| git tag `-d` v1.0                    | 删除本地标签                                                     |
| git push `origin --delete` v1.0      | 删除远程标签                                                     |
| git remote prune origin --dry-run    | 清理无效的远程引用                                                  |
| git flow feature `start` branchname  | 开始 开发特性分支xxx                                               |
| git flow feature `finish` branchname | 结束 开发特性分支xxx                                               |
| git flow release `start` releasename | 开始 版本发布                                                    |
| git `revert`  xx                     | 撤销某一次提交                                                    |
| git merge `--no-ff` branchname       | 合并分支（禁用快进方式）                                               |
| git rebase branchname                | 变基， 优化提交次数，使用后切换分支再使用merge会变为快进提交！(只能在未发布远程的分支使用，否则人民会唾弃你) |
