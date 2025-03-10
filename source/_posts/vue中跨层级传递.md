---
title: vue中跨层级传递
date: 2025-03-10 17:09:47
tags: vue
topic: web
---

在实际开发中，跨层级传递数据是常见的需求，尤其是在组件嵌套较深的情况下。Vue.js 提供了多种方式来实现跨层级传递数据，以下是几种常用的方法：

### provide 和 inject

适用场景：父组件向深层嵌套的子组件传递数据，避免通过 props 逐层传递。

特点：

父组件通过 provide 提供数据，子组件通过 inject 接收。

支持响应式数据（如 ref 或 reactive）。

~~~
// 父组件
export default {
  provide() {
    return {
      message: 'Hello from parent'
    };
  }
};

// 子组件
export default {
  inject: ['message'],
  created() {
    console.log(this.message); // 输出: Hello from parent
  }
};
~~~

**适合高阶组件或插件开发。**

### 全局状态管理 vuex or pinia

适用场景：多个组件需要共享状态，尤其是跨层级、跨组件的复杂场景。

特点：

集中式状态管理，所有组件都可以访问和修改全局状态。

适合中大型项目，状态共享需求较多的场景。

#### vuex
~~~
// store.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    }
  }
});

// 组件中使用
export default {
  computed: {
    count() {
      return this.$store.state.count;
    }
  },
  methods: {
    increment() {
      this.$store.commit('increment');
    }
  }
};
~~~

#### pinia
~~~
// store.js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0 }),
  actions: {
    increment() {
      this.count++;
    }
  }
});

// 组件中使用
import { useCounterStore } from './store';

export default {
  setup() {
    const counter = useCounterStore();
    return { counter };
  }
};
~~~

###  事件总线（Event Bus）
   适用场景：简单的跨组件通信，尤其是非父子组件之间的通信。

特点：

通过一个全局的事件总线来发布和订阅事件。

适合小型项目或简单的场景，但在大型项目中容易导致代码难以维护。

~~~
// eventBus.js
import Vue from 'vue';
export const eventBus = new Vue();

// 组件 A（发布事件）
import { eventBus } from './eventBus';
export default {
  methods: {
    sendMessage() {
      eventBus.$emit('message', 'Hello from Component A');
    }
  }
};

// 组件 B（订阅事件）
import { eventBus } from './eventBus';
export default {
  created() {
    eventBus.$on('message', (message) => {
      console.log(message); // 输出: Hello from Component A
    });
  }
};
~~~

### props 和 $emit

适用场景：父子组件之间的直接通信。

特点：

父组件通过 props 向子组件传递数据。

子组件通过 $emit 向父组件发送事件。

适合简单的父子组件通信，但在深层嵌套时会导致“prop 逐层传递”问题。

~~~
// 父组件
<template>
  <ChildComponent :message="message" @update-message="handleUpdate" />
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello from parent'
    };
  },
  methods: {
    handleUpdate(newMessage) {
      this.message = newMessage;
    }
  }
};
</script>

// 子组件
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="updateMessage">Update Message</button>
  </div>
</template>

<script>
export default {
  props: ['message'],
  methods: {
    updateMessage() {
      this.$emit('update-message', 'New message from child');
    }
  }
};
</script>
~~~


### $attrs 和 $listeners

适用场景：在多层嵌套组件中传递未声明的 props 和事件。

特点：

$attrs 包含了父组件传递的所有未声明为 props 的属性。

$listeners 包含了父组件传递的所有事件监听器。

适合封装高阶组件或透传属性和事件。

~~~
// 父组件
<template>
  <ChildComponent message="Hello" @custom-event="handleEvent" />
</template>

<script>
export default {
  methods: {
    handleEvent() {
      console.log('Custom event triggered');
    }
  }
};
</script>

// 子组件
<template>
  <GrandChildComponent v-bind="$attrs" v-on="$listeners" />
</template>

<script>
export default {
  inheritAttrs: false
};
</script>

// 孙子组件
<template>
  <div>
    <p>{{ $attrs.message }}</p>
    <button @click="$emit('custom-event')">Trigger Event</button>
  </div>
</template>
~~~

### $root 和 $parent
适用场景：直接访问根组件或父组件的实例。

特点：

$root 可以访问根组件实例。

$parent 可以访问父组件实例。

适合简单的场景，但会导致组件之间的强耦合，不推荐在复杂项目中使用。

~~~
// 子组件
export default {
  created() {
    console.log(this.$parent.message); // 访问父组件的数据
    console.log(this.$root.globalData); // 访问根组件的数据
  }
};
~~~

###  Local Storage 或 Session Storage

适用场景：跨页面或跨组件的持久化数据共享。

特点：

数据存储在浏览器中，适合需要持久化的场景。

数据不是响应式的，需要手动监听变化。
