---
title: Markdown语法
tags:
topic: 文档
---

作者写作时,偶尔忘记怎么写一些特殊的标签,比如说引用,表格,这里做个记录

<!-- more -->

## 标准Markdown

### 引用(>)
```markdown
> 引用的内容
>> 引用的内容
> ```缩进 code```
```
> 引用的内容
>> 引用的内容

> ```缩进 code```

## hexo && steller

### 封面
```markdown
---
title: 你的文章标题
date: 2023-10-01
cover: /images/cover.jpg  # 封面图路径
---
```

### 图片
```markdown
[![drop-down](dropdown.png)](https://github.com/DevCloudFE/ng-devui/pull/364)
```
> 博主安装了插件,会自动寻找同名文件夹
