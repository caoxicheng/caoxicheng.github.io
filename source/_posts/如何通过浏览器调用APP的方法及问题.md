---
title: 如何通过浏览器调用APP的方法及问题
date: 2019-08-23 11:34
tags:
---

## 导语

经常遇到浏览某些网站的页面时，提示你下载他的APP。

<!--more-->

已经发现封装好的通用JS方法

{% post_path 使用JS检测协议是否存在 %}

下列方法为自己摸索。

---

因为单纯个人兴趣，本博客中的代码只在IOS（ios 12.4， xs max）实测过，Android端需要自己实测，并不完全保证其正确性，如果IOS端如果问题也请自己查看官方safari的文档。Safari的版本根据ios版本而有所不同，本人遇到过低版本的Safari与高版本Safari支持的浏览器不同的情况，故，若需要实际开发，请多多测试

## 原理

粗略了解下来，安装APP后你的终端设备就会多一个APP相对应的协议，通过这个协议你可以打开APP，当然也可以附带一些参数，从而实现更加复杂的操作，比如支付宝或者微信的支付功能。 想深入了解的下面有简书上找到的解释（我没太看懂）

> 简书： https://www.jianshu.com/p/977ad259124f[/scode]

## 实现

### IOS

IOS 因为浏览器内核强制为safari所以方法统一，只需要在添加一个meta就可以了

```
<meta name='apple-itunes-app' content='app-id=477927812'>
    <!-- 上面的方法仅限于iOS设备，且无法定义Banner的形式。另外打开应用时也无法告诉App端要做什么操作。 -->
    <!-- app-id 对应 App Store里面ID，我使用的百度贴吧的ID-->
```

### 通用方法

```
<a href="weixin://">打开微信</a>
    <a href="com.baidu.tieba://">打开贴吧</a>
    <!-- 相对应的协议对应相应的APP -->
    <!-- 引出问题，我没安装软件怎么办？如何合理的校验以及提示 -->
```

## 优化

用户没有安装你的软件，怎么办呢？
常用的办法就是，在页面中出现一个按钮，提示你‘在App中打开’，点击按钮，如果APP已安装，则直接打开，若没安装，则跳转到相应的安装页面

### code

```
function openApp(){
      let loadDateTime = Date.now();
      setTimeout(function () {
        var timeOutDateTime = new Date();
        if (!loadDateTime || timeOutDateTime - loadDateTime < 2010) {
          if (document.visibilityState === 'visible') { window.location = 'http://a.app.qq.com/o/simple.jsp?pkgname=com.diaox2.android&ckey=CK1308661595241'; }
        }
      },2000);
      window.location = 'weixin://';
    }
```

理论上这些提示内容都是可以修改的，应该需要去看Safari的开发文档。不过我就不深入了，只是好奇研究一下。

## 参考文献

- [H5外部浏览器直接调起微信——通过url协议 weixin:// 判断是否安装微信及启动微信][4]
- [在手机浏览器中判断App是否已安装][5]
- [在移动端浏览器内判断用户是否安装了某个app][6]
- [weixin://xxx 分析][7]
- [在web浏览器中判断app是否安装并直接打开][8]

[4]: https://www.cnblogs.com/xyyt/p/7095364.html

[5]: https://www.cnblogs.com/liuzhenwei/p/4950085.html

[6]: https://www.jianshu.com/p/36bd38c1c0a2

[7]: https://www.jianshu.com/p/977ad259124f

[8]: https://www.cnblogs.com/jiyang2008/p/5772888.html
