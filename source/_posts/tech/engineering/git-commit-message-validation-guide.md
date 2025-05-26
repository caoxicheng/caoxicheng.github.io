---
title: git提交时自动检测信息是否合法·中文指南
date: '2022-03-10 19:45'
tags: []
categories:
  - 技术
  - 工程化
excerpt: >-
  安装 1.依赖 npm install --save-dev @commitlint/config-conventional @commitlint/cli
  husky 2.配置文件 在工程根目录下新建配置文件，名称为 commitlint.config.js。 当然也能直接一行命令 echo "m...
description: git提交时自动检测信息是否合法·中文指南 - 详细介绍与实践经验分享
keywords:
  - git
  - 提交时自动检测信息是否合法
  - 中文指南
  - 技术
  - 工程化
cover: ''
---

## 安装

1.依赖

```
npm install --save-dev @commitlint/config-conventional @commitlint/cli husky
```

2.配置文件

在工程根目录下新建配置文件，名称为 `commitlint.config.js`。

当然也能直接一行命令

```
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

commitlint.config.js 中添加配置信息

```
module.exports = {extends: ['@commitlint/config-conventional']}
```

然后使用命令创建拦截脚本(可能会出错，建议检查一下内容)

```
npx husky add .husky/commit-msg "npx --no -- commitlint --edit $1"
```

你会发现在`.husky`文件夹内出现一个`commit-msg`文件，里面内容是`npx --no -- commitlint --edit "$1"`

至此结束！

小说明：如果某次提交想禁用 husky，可以添加参数  **--no-verify** 。`git commit --no-verify -m "xxx"`
