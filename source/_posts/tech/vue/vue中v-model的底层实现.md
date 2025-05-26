---
title: vue中v-model的底层实现
date: '2025-03-27 19:52:27'
tags:
  - vue
categories:
  - 技术
  - Vue
excerpt: >-
  原生表单元素的v-model实现 对于input/textarea/select等原生表单工具,v-model的编译结果 javascript <input
  v-model="message"> 等价于: javascript <input  :value="message"  @input="me...
description: vue中v-model的底层实现 - 详细介绍与实践经验分享
keywords:
  - vue
  - model
  - 的底层实现
  - 技术
  - Vue
cover: ''
---


## 原生表单元素的`v-model`实现

对于**input/textarea/select**等原生表单工具,**v-model**的编译结果

```javascript
<input v-model="message">
```

**等价于:**
```javascript
<input
 :value="message"
 @input="message = $event.target.value"
```

不同元素的差异

| 元素类型        | 绑定属性    | 监听事件   |
|-------------|---------|--------|
| `<input>`   | value   | input  |
| `<textarea` | value   | input  |
| `select`    | value   | change |
| `checkbox`  | checked | change |
| `radio`     | checked | change |

## 自定义组件的**v-model**实现
vue2实现
```vue
<MyCompont v-model="value">
```
等价于
```vue
<MyComponent
    :value="value"
    @input="value = $event"
```

vue3实现
```vue
<MyComponent
    :modelValue="value"
    @update:modelValue="value = $event"
    >
```

## 自定义**v-model**
1. 修改默认属性/事件名称(vue2)
```vue
//子组件
export default {
    model: {
        prop: "title",
        event: "title-change"
    },
    props: ["title"]
}
```
2. 多`v-model`绑定(vue3)
```vue
<MyComponent
 v-model:title="title"
 v-model:content="content"
 >
```
等价于
```vue
<MyComponent
 :title="title"
 @update:title="title = $evet"
 :content="content"
 @update:content="content = $event"
```
