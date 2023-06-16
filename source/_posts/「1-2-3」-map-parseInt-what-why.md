---
title: 「1, 2, 3」.map(parseInt) what & why ?
date: 2019-07-25 17:31
tags:
---

说来惭愧一开始看到这个问题，我甚至还没反应过来是什么问题。

<!--more-->

map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

[scode type="share"]https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map[/scode]

parseInt(string, radix)  string为字符串，radix为介于2-36之间的数。使用者告诉这个函数string（比如11）是radix（比如2）进制的，函数将固定返回string以十进制时显示的数。

[scode type="share"]https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt[/scode]

---

## iterator

一般JS中能迭代的函数一般都有三个参数： currentValue, Index, Array 分别代表当前项，索引，整个数组

parseInt接受两个参数，此题传入currentValue和Index分别为：

1. (1, 0) 0或不传为十进制
2. (2, 1) 1不在[2,36]所以 => NaN
3. (3, 2) 2< [2,36]， 但是3不是二进制数，所以 => NaN

[scode type="yellow"]
如果字符串 string 以"0x"或者"0X"开头, 则基数是16 (16进制).

如果字符串 string 以"0"开头, 基数是8（八进制）或者10（十进制），那么具体是哪个基数由实现环境决定。ECMAScript 5 规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，永远都要明确给出radix参数的值。

如果字符串 string 以其它任何值开头，则基数是10 (十进制)。
[/scode]
