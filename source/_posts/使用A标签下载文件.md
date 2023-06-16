---
title: 使用A标签下载文件
date: 2019-08-01 14:23
tags:
---

- A的资源是浏览器可以识别的资源时，浏览器会默认打开。所以如果是单纯想打开资源可以加上 [target="_blank"]
- 若是ZIP这种无法识别的文件会默认下载，但是有的时候我们就是想下载比如jpg等资源,那么可以加上 [download="文件名.后缀"],也可以单独使用download这样下载的文件名就是默认的文件名
- 注意！此种方法下载的文件必须是通源文件，若是跨域还是按照正常的打开链接方式打开资源，download属性无效
- 若是需要现在后端发送的流文件such as表格之类。可以接收后使用相应的函数转成href可以识别的资源符进行下载

[scode type="blue"]MDN-a： https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a[/scode]
