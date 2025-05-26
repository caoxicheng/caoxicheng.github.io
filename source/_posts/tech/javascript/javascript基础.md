---
title: javascript基础
date: 2025-03-19T17:59:51.000Z
tags: []
categories:
  - 技术
  - JavaScript
excerpt: '作为一个6年的前端开发,一些基础知识不应该遗忘在角落,不管这些内容是因为框架提供了,还是工作中用不到,导致的遗忘.'
description: javascript基础 - 详细介绍与实践经验分享
keywords:
  - javascript
  - 基础
  - 技术
  - JavaScript
cover: ''
---
## 前言
作为一个6年的前端开发,一些基础知识不应该遗忘在角落,不管这些内容是因为框架提供了,还是工作中用不到,导致的遗忘.

## 正文

### 作用域

前端中的作用域(scope)指变量、函数和对象的可访问范围.作用域决定了代码中哪些部分可以访问特定的变量或函数.

#### 全局作用域
一般来说(浏览器环境)指挂在`window`对象下的成员

举例:
```javascript
var globalVar = "I'm global!";
function globalFunction() {
    console.log(globalVar); // 可以访问全局变量
}
globalFunction();
```

#### 局部作用域

在函数或者代码块内部声明的变量或函数,只能在该代码块或者函数内部访问.
分为**函数作用域**和**块级作用域**

##### 函数作用域
```javascript
function myFunction() {
    let localVar = "I'm local!";
    console.log(localVar); // 可以访问局部变量
}
console.log(localVar); // 报错，localVar 未定义
```
##### 块级作用域
```javascript
if (true) {
    let blockVar = "I'm block scoped!";
    console.log(blockVar); // 可以访问块级变量
}
console.log(blockVar); // 报错，blockVar 未定义
```

##### 补充
变量提升课外了解,简单避免不使用var声明变量即可

#### 作用域链
当访问一个变量时,javascript引擎会从当前作用域开始查找,如果查找不到,就会向上一层作用域查找,直到全局作用域.这个过程称为作用域链.

~~~javascript
var globalVar = "I'm global!";
function outerFunction() {
    var outerVar = "I'm outer!";
    function innerFunction() {
        var innerVar = "I'm inner!";
        console.log(globalVar); // 访问全局变量
        console.log(outerVar); // 访问外部函数变量
        console.log(innerVar); // 访问内部函数变量
    }
    innerFunction();
}
outerFunction();
~~~

无需照本宣科,了解作用域链即掌握作用域的核心知识.

#### 闭包(Closure)
闭包是指函数能够记住并访问其词法作用域,即使函数能在其词法作用域外执行.
闭包通常用来创建私有变量或实现函数工厂
```javascript
function createCounter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}
const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

##### 作用域与变量提升
在javascript 中 var声明的变量会被提升到其作用域的顶部,但是赋值不会被提升.
使用let和const也会提升,不过在赋值前访问会抛出错误**ReferenceError**

### 继承

Javascript 是基于原型的语言,继承通过原型链实现.

#### 原型链继承
通过将子类的原型指向父类的**实例**实现继承

```javascript
function Parent() {
  this.name = 'Parent';
}
Parent.prototype.sayHello = function() {
  console.log('Hello from ' + this.name);
};

function Child() {}
Child.prototype = new Parent(); // 继承父类

const child = new Child();
child.sayHello(); // "Hello from Parent"
```

#### 构造函数继承
在子类构造函数中调用父类的构造函数,使用call 和 apply

```javascript
function Parent(name) {
  this.name = name;
}

function Child(name) {
  Parent.call(this, name); // 继承父类属性
}

const child = new Child('Child');
console.log(child.name); // "Child"
```

缺点:无法继承父类原型链上的方法;

#### 组合继承(经典继承)

结合原型链继承和构造函数继承
```javascript
function Parent(name) {
  this.name = name;
}
Parent.prototype.sayHello = function() {
  console.log('Hello from ' + this.name);
};

function Child(name) {
  Parent.call(this, name); // 继承属性
}
Child.prototype = new Parent(); // 继承方法

const child = new Child('Child');
child.sayHello(); // "Hello from Child"
```

**缺点**:父类构造函数被调用两次;

#### 寄生组合继承(推荐)
```javascript
function inheritPrototype(Child, Parent) {
  const prototype = Object.create(Parent.prototype); // 创建父类原型的副本
  prototype.constructor = Child; // 修复构造函数指向
  Child.prototype = prototype;
}

function Parent(name) {
  this.name = name;
}
Parent.prototype.sayHello = function() {
  console.log('Hello from ' + this.name);
};

function Child(name) {
  Parent.call(this, name);
}
inheritPrototype(Child, Parent); // 继承方法

const child = new Child('Child');
child.sayHello(); // "Hello from Child"
```

#### ES6 的 class 继承
```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log(`Hello from ${this.name}`);
  }
}

class Child extends Parent {
  constructor(name) {
    super(name); // 调用父类构造函数
  }
}

const child = new Child('Child');
child.sayHello(); // "Hello from Child"
```

### 面对对象设计

SOLID

#### 单一职责原则(SRP)
一个类只做一件事

#### 开闭原则(OCP)
对扩展开放,对修改关闭

#### 里式替换原则(LSP)
子类必须能替换父类,且行为一致

#### 接口隔离原则(ISP)
客户端不应该依赖它不需要的接口

#### 依赖倒置原则(DIP)
高层模块不依赖底层模块,二者依赖抽象

核心概念

抽象、封装、继承与组合、多态

### 设计模式

#### 工厂模式

创建对象时隐藏具体类

PS: React的createElement()

#### 单例模式
全局唯一实例

PS: Vuex的store

#### 观察者模式
事件监听与响应

PS: DOM事件,Rxjs

#### 策略模式
动态切换算法或行为

PS: 表单校验策略

#### 装饰器模式
动态扩展对象功能

PS: ES7装饰器语法

#### 适配器模式
兼容不同接口的组件

### 事件循环
前端的事件循环（Event Loop）是 JavaScript 处理异步任务的核心机制，它确保了单线程的 JavaScript 能够高效地处理非阻塞操作（如定时器、网络请求、用户交互等）。

#### 事件循环的组成

1. 调用栈(call stack)
   - 用于追踪当前执行的函数(后进先出)
   - 当函数执行时,它被压入栈顶;执行完毕后弹出
2. 任务队列(Task Queues)
   - 宏任务队列 (MacroTask Queue): 存放 `setTimeout`、`setInterval`、DOM事件、I/O操作等.
   - 微任务队列 (MicroTask Queue): 存放 `Promise.then()`、`MutationObserver`、`queueMicrotask`等.
3. 渲染管道(Render Pipeline)
   - 负责页面渲染(样式计算、布局、绘制),通常在宏任务之间执行.

#### 事件循环的运行流程

1. 执行当前调用栈中的同步代码,直到调用栈清空.
2. 处理微任务队列:
    - 依次执行所有微任务,直到微任务队列清空.
    - 如果微任务又产生了新的微任务,会继续处理,直到队列清空.
3. 执行一次渲染更新(浏览器可能会跳过,根据帧率优化).
4. 从宏任务队列中取出一个任务,放入调用栈执行.
5. 重复

#### 关键规则
1. 微任务优先级高于宏任务
   - 每次调用栈清空后,先执行所有微任务,再处理宏任务.
   - 实例
   ```
    setTimeout(() => console.log("宏任务"), 0);
    Promise.resolve().then(() => console.log("微任务"));
    // 输出顺序：微任务 → 宏任务
    ```
2. 宏任务按来源队伍顺序执行
   - 不同类型的宏任务可能进行不同的队列(如定时器、I/O、UI事件),浏览器可能按优先级处理.
3. 避免阻塞事件循环
   -  长时间运行的同步代码(如循环)会阻塞渲染和任务处理,导致页面卡顿.

#### 常见异步操作分类
| 类型  | 常见API                                                                          |
|-----|--------------------------------------------------------------------------------|
| 宏任务 | `setTimeout`,`setInterval`,`requestAnimationFrame`,DOM事件,`fetch`的回调            |
| 微任务 | `Promise.them()`,`MutationObserver`,`queueMicrotask`,`async/await`(底层是Promise) |

#### 经典面试题
```javascript
console.log("1");

setTimeout(() => {
  console.log("2");
  Promise.resolve().then(() => console.log("3"));
}, 0);

Promise.resolve().then(() => {
  console.log("4");
  setTimeout(() => console.log("5"), 0);
});

console.log("6");

// 输出顺序：1 → 6 → 4 → 2 → 3 → 5
```

##### 步骤分析
1. 同步代码输出 1 和 6。
2. 微任务队列中的 4 先执行，并触发宏任务 5。
3. 宏任务队列中的 2 执行，其微任务 3 立即执行。
4. 最后执行宏任务 5。
