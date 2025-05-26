---
title: setTimeout、Promise、Async/Await 的区别
date: '2019-08-02 11:43'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  1、setTimeout 还记得一开始接触到此类问题是setTimeout 0ms也会放在下一轮操作中 引出 -> 宏观任务队列 微观任务队列
  的区别（埋坑，下一篇文章写） console.log('script start')    //1. 打印 script start    
  setTime...
description: setTimeout、Promise、Async/Await 的区别 - 详细介绍与实践经验分享
keywords:
  - setTimeout
  - Promise
  - Async
  - Await
  - 的区别
cover: ''
---

1、setTimeout

还记得一开始接触到此类问题是setTimeout 0ms也会放在下一轮操作中

引出 ->

宏观任务队列
微观任务队列
的区别（埋坑，下一篇文章写）

```
console.log('script start')    //1. 打印 script start
    setTimeout(function(){
        console.log('settimeout')    // 4. 打印 settimeout
    })        // 2. 调用 setTimeout 函数，并定义其完成后执行的回调函数
    console.log('script end')    //3. 打印 script start
    // 输出顺序：script start->script end->settimeout
```

此处因为setTimeout所以必然会最后一个执行（就算时间为0ms也会延迟到下一个宏观任务队列 macro queue）

2、 Promise

Promise本身是同步的立即执行函数， 当在executor中执行resolve或者reject的时候, 此时是异步操作， `会先执行`then/catch等，

当`主栈完成后`，才会去调用resolve/reject中存放的方法执行，打印p的时候，是打印的返回结果，一个Promise实例。

```
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
// 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
```
