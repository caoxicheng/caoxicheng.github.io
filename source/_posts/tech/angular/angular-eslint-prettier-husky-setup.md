---
title: 一步一步教你如何在 angular 项目中使用 eslint 以及 prettier 配合 husky 实现自动控制代码结构
date: '2021-06-29 16:27'
tags: []
categories:
  - 技术
  - Angular
excerpt: >-
  一步一步教你如何在 angular 项目中使用 eslint 以及 prettier 配合 husky 实现自动控制代码结构 工具安装 Prettier
  首先，安装prettier npm install --save-dev --save-exact prettier > 不同版本的 pretti...
description: 一步一步教你如何在 angular 项目中使用 eslint 以及 prettier 配合 husky 实现自动控制代码结构 - 详细介绍与实践经验分享
keywords:
  - 一步一步教你如何在
  - angular
  - 项目中使用
  - eslint
  - prettier
cover: ''
---

# 一步一步教你如何在 angular 项目中使用 eslint 以及 prettier 配合 husky 实现自动控制代码结构

## 工具安装

### Prettier

#### 首先，安装prettier

```
npm install --save-dev --save-exact prettier
```

> 不同版本的 prettier 有不同的格式实现，所以需要精准安装，并且，您使用的任何插件或可共享配置都必须在任何一种情况下本地安装!!!

#### 然后创建配置文件 .prettierrc.json 这是我的配置

```
{
    "tabWidth": 2,
    "semi": true,
    "singleQuote": true,
    "jsxSingleQuote": true,
    "bracketSpacing": true,
    "jsxBracketSameLine": true,
    "printWidth": 120,
    "endOfLine": "lf",
    "proseWrap": "preserve",
    "trailingComma": "es5",
    "useTabs": false
}
```

#### 别忘记了忽略文件 .perttierignore 下面是我的配置

```
# Ignore artifacts:
build
coverage
publish

#尚未准备好处理html
*.html
```

#### 当然 vscode 有配套的插件 `Prettier - Code formatter`，安装之后会读取你工作区的配置文件

### Eslint

#### 第一步还是安装

```
npm install eslint --save-dev
```

#### 然后初始化

```
npx eslint --init
```

> --init假设您已经有一个package.json文件

#### 配置规则

初始化之后，你会在你的 `.eslintrc.{js,yml,json}` 文件中看到以下内容

```
{
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    }
}
```

名称 `semi` 和 `quotes` 是 ESLint 中规则的名称。第一个值是规则的错误级别，可以是以下值之一：

`off` 或 0 - 关闭规则
`warn` 或 1 - 打开规则作为警告（不影响退出代码）
`error` 或 2- 将规则作为错误打开（退出代码将为 1）

> 具体规则描述👉[规则](https://eslint.org/docs/rules/)

您的.eslintrc.{js,yml,json}配置文件还将包含以下行：

```
{
    "extends": "eslint:recommended"
}
```

这说明你的 `Eslint` 将会继承默认推荐规则, 即在[规则](https://eslint.org/docs/rules/)页面前有 ✅ 的规则

## 配合使用

配合配合 `Eslint` 和 `Prettier` 你需要安装一个 Eslint 的 prettier 配置

#### 安装

```
npm install --save-dev eslint-config-prettier
```

#### 配置

然后添加 `prettier` 到 `.eslintrc.*` 文件的 `extends` 数组中

```
{
   " extends " : [
     " some-other-config-you-use " ,
     " prettier "
  ]
}
```

### Git 预提交钩子

```
npx mrm lint-staged
```

这将安装 `husky` 和 `lint-staged`，然后向项目添加一个配置，该配置package.json将在预提交挂钩中自动格式化支持的文件。

此时你的 `package.json`

```
"lint-staged": {
    "*.ts": "eslint --cache --fix", // 默认为js，自行按需修改，下同
    "*.{js,ts,css,scss,md}": "prettier --write"
  }
```

> 注意：如果您使用 ESLint，请确保 lint-staged Eslint 在 Prettier 之前而不是之后运行它。

如果需要husky在你提交代码时检查您的提交信息则请看另外文章

## 写在最后

本文只是应用的一次 `step to step` ，若想深入了解操作原理还需仔细阅读官方文档

1. [Eslint](https://eslint.org/docs/user-guide/getting-started)
2. [Prettier](https://prettier.io/docs/en/index.html)
3. [Prettier 与 Linter配合](https://prettier.io/docs/en/integrating-with-linters.html)
4. [Lint-staged](https://github.com/okonet/lint-staged)
