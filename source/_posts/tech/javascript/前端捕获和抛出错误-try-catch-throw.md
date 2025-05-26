---
title: 前端捕获和抛出错误 try catch throw
date: '2019-12-06 11:18'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  throw 抛出的问题是如何被捕获的？ try {  // code } catch (error) {  // 此时的error为上面try的错误信息,
  PS 在try中手动使用throw也会被catch 捕获到而进入到catch } 如果，我再嵌套一层呢？ try {    //上面代码原封不动...
description: 前端捕获和抛出错误 try catch throw - 详细介绍与实践经验分享
keywords:
  - 前端捕获和抛出错误
  - try
  - catch
  - throw
  - 技术
cover: ''
---

throw 抛出的问题是如何被捕获的？

```
try {
 // code
} catch (error) {
 // 此时的error为上面try的错误信息, PS 在try中手动使用throw也会被catch 捕获到而进入到catch
}
```

如果，我再嵌套一层呢？

```
try {
   //上面代码原封不动复制下来
   try {
    // code
   } catch (error) {
    // 此时的error为上面try的错误信息
   }
}
catch (error) {
  // 不会捕获到问题，因为错误已经正确抛出。try正常执行
}

// 正确的调用顺序
try {
   try {
    throw 'error1'
   } catch (error) {
     // console.log(error)    // error => error1
     throw 'error2'
     //下面的部分不会执行
     code...
     // end
   }
} catch (error) {
  console.log(error) //  error => error2
}
```
