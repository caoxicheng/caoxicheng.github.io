---
title: 一些JS的小技巧
date: '2019-11-21 11:35'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  !javascript.jpg 快速生成6位验证码 Math.random().toString(36).substr(2, 6); 转换数字 const
  number = '10'; number = +number; console.log(number); // 10 // 强制取整 ， 带有...
description: 一些JS的小技巧 - 详细介绍与实践经验分享
keywords:
  - 的小技巧
  - 技术
  - JavaScript
cover: ''
---

![javascript.jpg](2237123605.jpg)

## 快速生成6位验证码

```
Math.random().toString(36).substr(2, 6);
```

## 转换数字

```
const number = '10';
number = +number;
console.log(number); // 10
```

```
// 强制取整 ， 带有强制的类型转换，非法的转换之后取0
const number = '10';
number = ~~number;
console.log(number); // 10
```

## 快速浮点转整型

```
// 浮点不能进行或运算，所以会取整，或0保持不变
console.log(10.9 | 0);  // 10
console.log(-10.9 | 0); // -10
```

```
// 强制取整 ， 带有强制的类型转换，非法的转换之后取0
console.log(~~10.9);
console.log(~~-10.9);
```

## 数组降维

### 二维数组

```
let arr = [ [1], [2], [3] ];
arr = Array.prototype.concat.apply([], arr);
console.log(arr);// [1, 2, 3]

//flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。
//var newArray = arr.flat([depth])

let array = [ [1], [2], [3] ];
array = array.flat(2);
console.log(array); // [1, 2, 3]
```

### 多维数组

```
let arrMore = [1, 2, [3], [[4]]];
arrMore = arrMore.flat(Infinity); ///全局属性 Infinity 是一个数值，表示无穷大。
console.log(arrMore);
```

## 判断小数是否相等

```
// 原理为判断实际差值是否小于阈值
function equal(number1, number2) {
    return Math.abs(number1 - number2) < Math.pow(2, -52);
}
console.log(equal(0.1 + 0.2, 0.3));
```

## 判断变量是否为数组

```
1. instanceof
2. array.__proto__.constructor === Array
3. array.constructor === Array
4. Array.isArray（兼容性问题）
5. Object.prototype.toString.call([]) === "[object Array]"（最通用） //同理和可以判断任意其他类型
```

> PS：instanceof和constructor判断的变量，必须在当前页面声明。例如：父页面是一个框架，框架中引入一个页面（子页面），在子页面中申明的array，并将其复制给父元素的一个变量，这时instanceof和constructor判断该变量，将返回false。
> ----------------------------------------------------------------------------------------------------------------- 原因： array是复合类型。在传递的过程中，仅仅是引用地址的传递。
> 每个页面的array原生对象引用的地址是不一样的，在子页面中声明的array，所对应的构造函数，是子页面的array对象，在父页面进行判断时，使用的并不是子页面的array。

## 数组去重

```
Array.prototype.unique = function() {
    return [...new Set(this)];
}
var array = [1, 2, 3, 43, 45, 1, 2, 2, 4, 5];
array.unique();
```

## 短路运算符 && ||

### 使用&&将返回第一个条件为假的值。如果每个操作数的计算值都为true，则返回最后一个计算过的表达式。

```
let one = 1, two = 2, three = 3;
console.log(one && two && three); // 3
console.log(0 && null); // 0
```

### 使用||将返回第一个条件为真的值。如果每个操作数的计算结果都为false，则返回最后一个计算过的表达式。

```
let one = 1, two = 2, three = 3;
console.log(one || two || three); // 1
console.log(0 || null); // null
```

## 过滤空值

```
let result1 = [1, 2, 0, undefined, null, false, ''].filter(Boolean);
console.log(result1);
```

## 合并对象

```
const person = { name: 'David Walsh', gender: 'Male' };
const tools = { computer: 'Mac', editor: 'Atom' };
const attributes = { handsomeness: 'Extreme', hair: 'Brown', eyes: 'Blue' };
const summary = { ...person, ...tools, ...attributes };
console.log(summary);
```

## 字符串去空格

```
String.prototype.trim = function(){return this.replace(/^\s+|\s+$/g, "");};

//去两端 string.trim()
```
