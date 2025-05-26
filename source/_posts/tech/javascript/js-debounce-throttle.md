---
title: JS防抖动和节流（debounce & throttle）
date: '2019-05-22 16:28'
tags: []
categories:
  - 技术
  - JavaScript
excerpt: >-
  防抖动 debounce 由于种种原因，可能某一个函数会在很短的时间内触发N次，但是我们并不想这样，只想触发一次，可能是因为性能原因，也可能逻辑原因。
  这个时候就需要防抖动了。 [scode type="blue"]去抖和节流是不同的，因为节流虽然中间的处理函数被限制了，但是只是减少了频率，而去抖则...
description: JS防抖动和节流（debounce & throttle） - 详细介绍与实践经验分享
keywords:
  - 防抖动和节流
  - debounce
  - throttle
  - 技术
  - JavaScript
cover: ''
---

## 防抖动 debounce

由于种种原因，可能某一个函数会在很短的时间内触发N次，但是我们并不想这样，只想触发一次，可能是因为性能原因，也可能逻辑原因。

这个时候就需要防抖动了。

[scode type="blue"]去抖和节流是不同的，因为节流虽然中间的处理函数被限制了，但是只是减少了频率，而去抖则把中间的处理函数全部过滤掉了，只执行规判定时间内的最后一个事件。[/scode]

<!--more-->

举个例子： 监听滚动事件

> 只有用户停止滚动滚轮delay毫秒后才会触发事件（1次）

```
window.onscroll = debounce(scrollListen, 100);

        function scrollListen() {
            let scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
            console.log(scrollTop,);
        }
        function debounce(func, delay) {
            let timeout = null;
            return function() {
                let context = this;
                let args = arguments;
                timeout && clearTimeout(timeout); // &&运算符，若前为假，则不计算后者，不喂呼奇技淫巧
                timeout = setTimeout(function(){
                    func.apply(context, args);
                },delay);
            }
        }
```

PS：请注意，实际浏览器触发onscroll时是这样调用的 debounce(scrollListen, 100)(event),即arguments是这里的`event`.详情请了解函数返回值为函数的注意点。

## 节流 throttle

> 在1秒内只触发一次

```
window.onscroll = throttle(scrollListen, 1000);

        function scrollListen() {
            let scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
            console.log(scrollTop);
        }

        /**
        * 节流函数
        * @param func事件触发的操作
        * @param wait 间隔多少毫秒需要触发一次事件
        */
        function throttle(func, wait) {
            let lastTime = null;
            let timeout = null;
            return function() {
                let context = this;
                let now = new Date();
                // 如果上次执行的时间和这次触发的时间大于一个执行周期，则执行
                if (now - lastTime - wait > 0) {
                    // 如果之前有了定时任务则清除
                    if (timeout) {
                        clearTimeout(timeout);
                        timeout = null;
                    }
                    func.apply(context, arguments);
                    lastTime = now;
                } else if (!timeout) {
                    timeout = setTimeout(() => {
                        // 改变执行上下文环境
                        func.apply(context, arguments);
                    }, wait);
                }
            };
        }
```
