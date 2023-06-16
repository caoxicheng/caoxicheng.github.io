---
title: 关于Typeecho出现Database Query Error 问题的汇总
date: 2019-04-29 12:02
tags:
---

刚刚部署完网站的时候，总是会时不时在各种场景出现Database Query Error 这个问题。

初步排查下来是数据库格式未支援UTF-8（因为想支援EMOJI符号表情，所以这里选择最近的UTF-8）

这样下来，大部分情况下的错误没有了（其实也就是评论+文章发布的时候）

<!--more-->

后来因为需要用到时光机的发送定位功能，发现还是出现Database Query Error 错误

查阅资料，因为我数据库创建的时候数据库编码没有正确选择，所以需要额外的步骤去修改数据格式

我的因为数据涉及比较少，所以直接navicat里面设计表中修改了内容。

附上文档（ps：官方网站的文档是真的简陋啊）

[Typecho数据库错误Database-Query-Error解决办法][1]

[1]: https://www.typechodev.com/case/Typecho%E6%95%B0%E6%8D%AE%E5%BA%93%E9%94%99%E8%AF%AFDatabase-Query-Error%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95.html
