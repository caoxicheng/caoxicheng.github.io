---
title: storybook 编写指南
date: '2022-08-11 14:24'
tags: []
categories:
  - 技术
  - 工程化
excerpt: "storybook 编写指南 > 所有story相关的导入导出以及文件名称，官方推荐首字母大写 - storybook 指南   - argTypes 参数详情 overview     - 举个 \U0001F330 e.g.     - 控件参数详解     - 再举个 \U0001F330 e.g.   - 不想/想全部给你..."
description: storybook 编写指南 - 详细介绍与实践经验分享
keywords:
  - storybook
  - 编写指南
  - 技术
  - 工程化
cover: ''
---

# storybook 编写指南

> 所有story相关的导入导出以及文件名称，官方推荐`首字母大写`

- [storybook 指南](#storybook-指南)
  - [argTypes 参数详情 overview](#argtypes-参数详情-overview)
    - [举个 🌰 e.g.](#举个--eg)
    - [控件参数详解](#控件参数详解)
    - [再举个 🌰 e.g.](#再举个--eg)
  - [不想/想全部给你看 👀](#不想想全部给你看-)
  - [code source 部分](#code-source-部分)
  - [描述部分](#描述部分)
  - [我想写独立的文档](#我想写独立的文档)

## argTypes 参数详情 overview

| 字段                         | 说明                              |
| -------------------------- | ------------------------------- |
| name                       | 同字段名,可省略                        |
| type.required              | boolean                         |
| description                | 描述                              |
| defaultValue               | 数据:默认值                          |
| table.disable              | 不在文档中出现                         |
| table.category             | 分类分组名称                          |
| table.type.summary         | 类型的简单描述                         |
| table.type.detail          | 类型的长描述                          |
| table.defaultValue.summary | 显示:默认值                          |
| table.defaultValue.detail  | 显示:默认值详细说明                      |
| control                    | 禁用控制器 e.g false                 |
| control.type               | 控件类型(需要有\[数据\]默认值) e.g. null \\ |
| options                    | 控件类型为 radio 时的值数组               |

### 举个 🌰 e.g.

```
Primary.argTypes = {  
  fNotFoundImage: {
    name: 'fNotFoundImage',
    type: {
      required: false,
    },
    table: {
      disable: true,
      defaultValue: {
        summary: 'null',
      },
      type: {
        summary: 'string | TemplateRef<void>'
      }
    },
    description: '图片url',  
    defaultValue: null,
    options: [null, 'https://www.sass.hk/images/sass.png'],
    control: { type: 'radio' }, // 可简写成 => control: 'radio'
  }};
```

### [控件参数详解](https://github.com/storybookjs/storybook/blob/91e9dee33faa8eff0b342a366845de7100415367/addons/controls/README.md#control-annotations)

| data type 数据类型 | control type 控件类型 | description 描述                                                 | options 选项     |
| -------------- | ----------------- | -------------------------------------------------------------- | -------------- |
| array          | array             | serialize array into a comma-separated string inside a textbox | separator      |
| boolean        | boolean           | checkbox input                                                 | -              |
| number         | number            | a numberic text box input                                      | min, max, step |
| -              | range             | a range slider input                                           | min, max, step |
| object         | object            | json editor text input                                         | -              |
| enum           | radio             | radio buttons input                                            | options        |
| -              | inline-radio      | inline radio buttons input                                     | options        |
| -              | check             | multi-select checkbox input                                    | options        |
| -              | inline-check      | multi-select inline checkbox input                             | options        |
| -              | select            | select dropdown input                                          | options        |
| -              | multi-select      | multi-select dropdown input                                    | options        |
| string         | text              | simple text input                                              | -              |
| -              | color             | color picker input that assumes strings are color values       | -              |
| -              | date              | date picker input                                              | -              |

### 再举个 🌰 e.g.

```
export default {  
  title: 'Gizmo',
  component: Gizmo,
  argTypes: {
    width: {
    type: 'range',
    min: 400,
    max: 1200,
    step: 50
    };
  },
};
```

## 不想/想全部给你看 👀

有些内容很隐私/重要，你就是不想/需要给调用的人看到。

那么你可以使用 `include` 和 `exclude` 配置 `controls`, 可以写数字串数组或正则表达式

举个 🌰 e.g.

```
import { YourComponent } from './YourComponent';  

export default {  
  title: 'YourComponent',
  component: YourComponent,
};

const Template = (args) => ({
  //👇 Your template goes here
});


ArrayInclude = Template.bind({})
ArrayInclude.parameters = { 
  controls: {
    include: ['foo', 'bar'] 
  }
};

RegexInclude = Template.bind({})
RegexInclude.parameters = {
  controls: {
    include: /^hello*/
  }
};

ArrayExclude = Template.bind({})
ArrayExclude.parameters = { 
  controls: {
    exclude: ['foo', 'bar']
  }
};  

RegexExclude = Template.bind({})
RegexExclude.parameters = {
 controls: {
   exclude: /^hello*/
 }
};
```

> 当然比较本的办法隐藏起来就是在 ts 文件中追加注释 @ignore

## code source 部分

有时候自动带出 `code source` 并不足以满足我们的要求，需要我们自定义这个时候就可以使用 `docs.source.code` 和可选参数 `docs.source.language`

```
// Button.stories.js|jsx|ts|tsx  

import { Button } from './Button';  

export default {  
  /* 👇 The title prop is optional.
  * See https://storybook.js.org/docs/react/configure/overview#configure-story-loading
  * to learn how to generate automatic titles
  */
  title: 'Button',
  component: Button,
};  

export const Template = (args) => ({  
  //👇 Your template goes here});  

export const CustomSource = Template.bind({});  
CustomSource.parameters = {  
  docs: {    
    language: 'ts', // 高亮语言  
    format: true, // 格式化  
    source: {      code: 'Some custom string here',    },
  },
};
```

## 描述部分

> Storybook 提取组件的描述并将其呈现在页面顶部。它是从 docgen 组件自动生成的支持的框架基于组件的源代码。下面是一个精简的例子和可用的选项。

```
// Button.stories.js|jsx|ts|tsx  

import { Button } from './Button';  

export default {  
  /* 👇 The title prop is optional.
  * See https://storybook.js.org/docs/react/configure/overview#configure-story-loading
  * to learn how to generate automatic titles
  */
  title: 'Button',
  component: Button,
  parameters: {
    docs: {
      description: {
        component: 'Some component _markdown_',
      },
    },
  },
};  

const Template = (args) => ({  
 //👇  Your template goes here});  

export const WithStoryDescription = Template.bind({});
WithStoryDescription.parameters = {
  docs: {  
    description: {
      story: 'Some story **markdown**',
    },
  },
};
```

| 选项        | 描述                                                                     |
| --------- | ---------------------------------------------------------------------- |
| component | 覆盖默认组件描述。description: { component:'An example component description' } |
| story     | 覆盖故事描述。 description: { story: 'An example story description' }         |

## 我想写独立的文档

> 有的同学想写和本文档同级别的内容

在项目中你觉得适合的任意位置创建一个 `MyComponent.stories.mdx` 文件的内容

> 名称`MyComponent`自行修改

```
<!-- MyComponent.stories.mdx -->  

import { Meta } from '@storybook/addon-docs';  

# Some header  

And Markdown here  

<Meta title="Docs/MyComponent"/>
```

<Meta title="Docs/Story用例编写指南"/>
