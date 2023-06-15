---
title: 「瞎搞」-微信连接GPT
tags: []
excerpt: >-
  前言最近在和GPT疯狂的聊天,想起来之前也想研究微信的自动应答,那么理论上是可以支持把GPT接入微信的,那么开干就干,下面展开说说...因为最近一直在和GPT闲聊,所以第一时间想到也是的直接问G...
date: 2023-04-22 14:31:00
---

前言最近在和GPT疯狂的聊天,想起来之前也想研究微信的自动应答,那么理论上是可以支持把GPT接入微信的,那么开干就干,下面展开说说...因为最近一直在和GPT闲聊,所以第一时间想到也是的直接问G...
<!-- more -->
## 前言

最近在和GPT疯狂的聊天,想起来之前也想研究微信的自动应答,那么理论上是可以支持把GPT接入微信的,那么开干就干,下面展开说说...

* * *

因为最近一直在和GPT闲聊,所以第一时间想到也是的直接问GPT

![图1](https://www.godlike.top/usr/uploads/2023/04/3199789693.png "图1")

因为提到了ichat,去搜索了相关的内容,不过说是代码很简陋+有封号危险,另外寻到了 `Wechaty`

> [wechaty官方](https://wechaty.js.org/)
> 
> [中文文档](https://wechaty.gitbook.io/wechaty/v/zh/)

通过阅读 `github` 启动了一个 `demo`

## 小试牛刀

按照介绍,直接

```
pnpm install // 我习惯了pnpm,npm也可
pnpm start
```

扫码登陆......

### 事情没这么简单

![图2](https://www.godlike.top/usr/uploads/2023/04/1426876939.png "图2")

我直接愣住

![图3](https://th.bing.com/th/id/R.1ad169b718b83cf2fd701c452dc8280b?rik=ecE3OLbwkvHPSg&riu=http%3a%2f%2fwx1.sinaimg.cn%2fbmiddle%2f006qir4ogy1g4ben0r9vej30a00armxw.jpg&ehk=rMh9jfj3vmxUpQksQC4Su6CnLkkdq1e0FhFddFS3Cv4%3d&risl=&pid=ImgRaw&r=0 "图3")

我寻思这也能遇到Angualr(angualr大法好),不过我想到自己也是个angular开发,试试看能不能自己解决问题.进到问题点去看了一下,得是我没用过的内容

算了,直接去看issue,省略10mins

原因是微信的安全策略问题,需要配置 `uos` 模式  
\[scode type="blue" size=""\]具体表现来说就是电脑登陆的话,手机扫码你电脑会被挤掉线\[/scode\]

### 搞定

ok,解决了这个小问题,我们直接开始测试,里面就判断一个很简单的内容,如果接收到信息为“ding”,则回复一个“dong”  
确实很入门

👇代码

![图4](https://www.godlike.top/usr/uploads/2023/04/2730651784.png "图4")

👇效果

![图5](https://www.godlike.top/usr/uploads/2023/04/2081143864.jpeg "图5")

## 高阶玩法

按照正常的流程接下来要开始研究文档,然后匹配自己的逻辑了,不过我这个比较鸡贼,想快速看到结果,就去搜索了一下关键字 `微信 GPT`

hhhh,然后给我看了一个现成的应用  
[ChatGPT-wechat-bot](https://github.com/AutumnWhj/ChatGPT-wechat-bot)

> 其实是对于上文提到的 `wechaty`的一层封装

那么按照文档一步一步来(这里就把我之前遇到的 `uos`给默认打开了,但是官方的例子竟然没有修复!!!)

**获取/配置key......**  
安装依赖......  
登陆......  
问答......

![图6](https://www.godlike.top/usr/uploads/2023/04/660633633.jpeg "图6")

### 后续

api的方式调用的是GPT-3.5版本用起来确实没有GPT-4来的丝滑,不过最初的功能已经达成了

后期的设想

*   \[ \] 商务的自动问答
*   \[ \] 自动同意好友申请
*   \[ \] 自动拉人进群
*   \[ \] 更多

当然也有问题,就是群聊如果@你,不过你有群聊的备注,那么匹配不到,因为插件只会匹配你的微信名字,但是群聊@你是你群聊中的备注.当然这个可以通过改规则的正则来修复,我是比较简单的把我群聊的备注给删了.只是测试能成就行~

* * *

顺便给我的微信好友们体验了一小时GPT的对话

![图7](https://www.godlike.top/usr/uploads/2023/04/1178030010.jpeg "图7")