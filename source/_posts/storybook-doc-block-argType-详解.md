---
title: storybook doc-block argType 详解
date: 2021-08-06 17:57
tags:
---

# storybook doc-block argType 详解

## 参数详情 overview

| 字段                         | 说明                                                   |
|:-------------------------- |:---------------------------------------------------- |
| name                       | 同字段名,可省略                                             |
| type.require               | boolean                                              |
| description                | 描述                                                   |
| defaultValue               | 数据:默认值                                               |
| table.type.summary         | 类型的简单描述                                              |
| table.type.detail          | 类型的长描述                                               |
| table.defaultValue.summary | 显示:默认值                                               |
| table.defaultValue.detail  | 显示:默认值详细说明                                           |
| control.type               | 控件类型(需要有\[数据\]默认值) e.g. null , radio , text , number |
| options                    | 控件类型为 radio 时的值数组                                    |

### 举个 🌰 e.g.

```
Primary.argTypes = {
  fNotFoundImage: {
    name: 'fNotFoundImage',
    type: {
      required: false,
    },
    table: {
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
  }
};
```

### [控件参数详解](https://github.com/storybookjs/storybook/blob/91e9dee33faa8eff0b342a366845de7100415367/addons/controls/README.md#control-annotations)

| data type & 数据类型 | control type & 控件类型 | description & 描述                                               | options & 选项   |
|:---------------- |:------------------- |:-------------------------------------------------------------- |:-------------- |
| array            | array               | serialize array into a comma-separated string inside a textbox | separator      |
| boolean          | boolean             | checkbox input                                                 | -              |
| number           | number              | a numberic text box input                                      | min, max, step |
| -                | range               | a range slider input                                           | min, max, step |
| object           | object              | json editor text input                                         | -              |
| enum             | radio               | radio buttons input          Ï                                 | options        |
| -                | inline-radio        | inline radio buttons input                                     | options        |
| -                | check               | multi-select checkbox input                                    | options        |
| -                | inline-check        | multi-select inline checkbox input                             | options        |
| -                | select              | select dropdown input                                          | options        |
| -                | multi-select        | multi-select dropdown input                                    | options        |
| string           | text                | simple text input                                              | -              |
| -                | color               | color picker input that assumes strings are color values       | -              |
| -                | date                | date picker input                                              | -              |

### 再举个 🌰 e.g.

```
export default {
  title: 'Gizmo',
  component: Gizmo,
  argTypes: {
    width: { type: 'range', min: 400, max: 1200, step: 50 };
  },
};
```
