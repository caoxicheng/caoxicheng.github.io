---
title: javascript基础
tags:
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

### 闭包

### 面对对象设计
