---
title: IOS在桌面添加web站点图标以及增加启动动画来模仿原生App
date: 2019-05-07 15:18
tags:
---

## web应用的意义

公司计划上线一款样本App，安卓端因为盛和不严格，很容易就上架了，但是万恶的IOS端，一直审核不通过。原因种种，所以需要我前端把现有的网站配合Safari的添加到桌面图标功能伪造一个原生App。

## 开发步骤

### 第一步：添加图标到主屏幕

在Safari上点击添加到主屏幕，默认的icon是当前页面的缩略图，但是用屁股想想也知道这不行啊，老大拿着40米的大刀让你先跑39米
那么就需要下面的Safari的私有属性来设置icon了。

```
<link rel="apple-touch-icon-precomposed" sizes="57x57" href="icon-57.png">
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="icon-72.png">
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="icon-114.png">
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="icon-144.png">
```

添加图标到主屏幕有两个属性值：`apple-touch-icon` 和 `apple-touch-icon-precomposed`，后者会应用IOS自动给图标添加的那层高光

由于iPhone以及iPad都有两种分辨率的设备存在，图标的尺寸就需要做4个：144×144(iPad Retina)、72×72(iPad)、114×114(iPhone Retina)、57×57(iPhone)。

使用`size`来告诉设备调用哪一个图标

### 第二步：启动动画

几乎所有的App都有启动动画，我们要以假乱真怎么可以没有启动动画呢？?

下面是表示启动动画的Safari私有属性

```
<link rel="apple-touch-startup-image" sizes="2048x1496" href="icon-2048x1496.png" media="screen and (min-device-width: 1025px) and (max-device-width: 2048px) and (orientation:landscape) and (-webkit-min-device-pixel-ratio: 2)">
<link rel="apple-touch-startup-image" sizes="1536x2008" href="icon-1536x2008.png" media="screen and (min-device-width: 1025px) and (max-device-width: 2048px) and (orientation:portrait) and (-webkit-min-device-pixel-ratio: 2)">
<link rel="apple-touch-startup-image" sizes="1024x748" href="icon-1024x748.png" media="screen and (min-device-width: 481px) and (max-device-width: 1024px) and (orientation:landscape)">
<link rel="apple-touch-startup-image" sizes="768x1004" href="icon-768x1004.png" media="screen and (min-device-width: 481px) and (max-device-width: 1024px) and (orientation:portrait)">
<link rel="apple-touch-startup-image" sizes="640x920" href="icon-640x920.png" media="screen and (max-device-width: 480px) and (-webkit-min-device-pixel-ratio: 2)">
```

`apple-touch-startup-image`是用来标示启动画面的，但它不像apple-touch-icon可以指定sizes来告诉设备该使用哪个图片（也有一种说法
是在iOS5中已经支持sizes识别了，但没有测试成功），所以只能通过media里的媒体查询来识别，而iPhone Retina的分辨率值界于取值之间，所以需要通过webkit的私有属性-webkit-min-device-pixel-ratio:2来鉴别像素密度以进行识别。

启动画面的图片尺寸并非完全等于设备的尺寸，比如iPad2的尺寸是1024×768，但它的启动画面尺寸横向是1024×748，竖向尺寸是768×1004，因为需要减去系统状栏的高度20px，而使用的Retina屏幕的iPhone4以及iPad3则需要减去状态栏的高度40px。

### 第三步：隐藏工具栏

桌面图标有了，启动动画也有了，但是一打开还是浏览器，那不是一下子全暴露了？

所以我们还需要把Safari的工具栏给藏起来，当然Safari也提供了这个私有方法。 ::aru:cheer::

```
<meta name="apple-mobile-web-app-capable" content="yes">  //全屏
<meta name="apple-mobile-web-app-status-bar-style" content="black"> //全屏前提下的状态栏
<meta name="format-detection" content="telephone=no">  //是否禁用电话号码
<meta name="viewport" content="width=device-width,initial-scale=1, minimum-scale=1.0, maximum-scale=1, user-scalable=no">  //viewport并非Safari的私有属性，width用于指定宽度，initial-scale指定初始化的缩略比例，minimum-scale指定缩小的比例，而maximum-scale则是放大的比例，当然这些缩放都取决于user-scalable——决定用户是否能缩放页面。
```

## 阻止iOS Web APP中点击链接跳转到Safari 浏览器新标签页

一切都已经就绪，至少外观看上去和真的没什么不一样了，but，点开某个链接。what，又调用了另一个Safari浏览器，这不就全暴露我是一个假货了？那不行，你得给我在内部进行跳转。

```
<script type="text/javascript">
    //iOS Web APP中点击链接跳转到Safari 浏览器新标签页的问题 devework.com
    //stanislav.it/how-to-prevent-ios-standalone-mode-web-apps-from-opening-links-in-safari
    if(("standalone" in window.navigator) && window.navigator.standalone){
        var noddy, remotes = false;
        document.addEventListener('click', function(event) {
            noddy = event.target;
            while(noddy.nodeName !== "A" && noddy.nodeName !== "HTML") {
                noddy = noddy.parentNode;
            }
        if('href' in noddy && noddy.href.indexOf('http') !== -1 && (noddy.href.indexOf(document.location.host) !== -1 || remotes))
            {
                event.preventDefault();
                document.location.href = noddy.href;
            }
        },false);
    }
</script>
```

大功告成，以假乱真的IOS端web应用出炉了。
