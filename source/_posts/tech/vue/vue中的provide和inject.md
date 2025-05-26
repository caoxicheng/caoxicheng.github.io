---
title: vue中的provide和inject
date: '2025-03-10 17:00:56'
tags:
  - vue
categories:
  - 技术
  - Vue
excerpt: >-
  基础介绍 在 Vue.js 中，provide 和 inject 是一对用于跨层级组件通信的
  API。它们主要用于父组件向深层嵌套的子组件传递数据，避免了通过 props 逐层传递的繁琐。 选项是API // 父组件 export default {
  provide: { message: 'Hel...
description: vue中的provide和inject - 详细介绍与实践经验分享
keywords:
  - vue
  - provide
  - inject
  - 技术
  - Vue
cover: ''
---

## 基础介绍
在 Vue.js 中，provide 和 inject 是一对用于跨层级组件通信的 API。它们主要用于父组件向深层嵌套的子组件传递数据，避免了通过 props 逐层传递的繁琐。

## 选项是API
~~~
// 父组件
export default {
provide: {
message: 'Hello from parent component'
}
}

// 子组件
export default {
inject: ['message'],
created() {
console.log(this.message); // 输出: Hello from parent component
}
}
~~~

## 组合式API
~~~
// 父组件
import { provide, ref } from 'vue';

export default {
  setup() {
    const message = ref('Hello from parent component');
    provide('message', message);

    return {
      message
    };
  }
}

// 子组件
import { inject } from 'vue';

export default {
  setup() {
    const message = inject('message');
    console.log(message.value); // 输出: Hello from parent component

    return {
      message
    };
  }
}
~~~

provide 和 inject 的特点

1. 跨层级传递：**provide** 和 **inject** 允许父组件向*任意深度*的子组件传递数据，而不需要通过中间的每一层组件。
2. 响应式数据：如果 provide 提供的是**响应式数据**（如 ref 或 reactive），那么子组件通过 inject 接收的数据也会是响应式的。
3. 非响应式数据：如果 provide 提供的是普通的数据（如字符串、数字等），那么子组件接收的数据将是非响应式的。
4. provide 和 inject 主要用于高阶插件或组件库的开发，普通应用开发中应尽量避免过度使用，以免导致组件之间的耦合度过高。
5. inject 的值是只读的，子组件不应该直接修改 inject 的值。如果需要修改，可以通过 provide 提供一个方法来实现。
