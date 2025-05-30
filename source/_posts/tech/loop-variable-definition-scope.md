---
title: 很有意思的东西-关于临时变量在循环体内定义，还是在循环体外定义的问题
date: '2021-03-31 16:48'
tags: []
categories:
  - 技术
excerpt: >-
  在一开始学循环的时候，比如说遍历一个list数组,是这么教的: for(let i = 0; i < list.length; i++){        
  //xxx    } 但是其实每次循环结束判断条件的时候，都是会去调用list.length,这都是需要消耗性能，如果数组是一个固定长度的那么，...
description: 很有意思的东西-关于临时变量在循环体内定义，还是在循环体外定义的问题 - 详细介绍与实践经验分享
keywords:
  - 很有意思的东西
  - 关于临时变量在循环体内定义
  - 还是在循环体外定义的问题
  - 技术
cover: ''
---

在一开始学循环的时候，比如说遍历一个list数组,是这么教的:

```
for(let i = 0; i < list.length; i++){
        //xxx   
}
```

但是其实每次循环结束判断条件的时候，都是会去调用list.length,这都是需要消耗性能，如果数组是一个固定长度的那么，可以写成这样：

```
for(let i = 0, len = list.length; i < len; i++){
        //xxx   
}
```

这样，就在初始化的时候，把数组长度给缓存了下来，而不用每次判断的时候去list里面找。

今天在leetcode刷题的时候，因为涉及到循环，而且需要临时变量，我就按照惯例，在循环体中声明并且使用了临时变量，如下方

> 给定一个Excel表格中的列名称，返回其相应的列序号。
> A -> 1
> B -> 2
> C -> 3
> ...
> Z -> 26
> AA -> 27
> AB -> 28
> ...

```
function titleToNumber(columnTitle: string): number {
  let len = columnTitle.length - 1;
  let radix = 1;
  let sum = 0;
  while (len >= 0) {
    let temp = columnTitle.charCodeAt(len) - 64;
    sum += temp * radix;
    len--;
    radix *= 26;
  }
  return sum;
};
```

提交了之后，速度果然

![微信图片_20210331165400.png](669852852.png)

但是我想，这样每次循环的时候都会创建变量，是不是直接在外部初始化一个临时变量，这样每次直接赋值就好了？

说干就干

```
function titleToNumber(columnTitle: string): number {
  let len = columnTitle.length - 1;
  let radix = 1;
  let sum = 0;
  let curr: number;
  while (len >= 0) {
    curr = columnTitle.charCodeAt(len) - 64;
    sum += curr * radix;
    len--;
    radix *= 26;
  }
  return sum;
};
```

很快啊，直接保存提交，霍，直接时间加了20ms增加1/4，这可不行啊。

那么联想到list.length的情况，我<p style="color:red">猜测</p>是因为如果不在循环体内定义的话，需要通过作用域链找寻上层作用域链中的临时变量，这样成本是会比直接在本身作用域中找寻的成本大的。

一个有意思的小细节

> 再追加一个小技巧: js向下取整   number | 0  === Math.floor(number)
