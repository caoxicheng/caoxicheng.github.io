---
title: 使用JS检测协议是否存在
date: 2019-11-08 10:15
tags:
---

[scode type="yellow"]移动点击打开软件或跳转至应用商城推荐使用，PC端使用不多[/scode]

移动端开发，一个绕不开的问题就是，如何在H5页面中，把用户引流到自家的APP中，现通用的方法如下图所示：

<!--more-->

![微信图片_20191108101934.jpg](266283327.jpg)

最上方可关闭提示，为Safari自带的应用商店功能标签，实现方法为添加`<meta>`标签。↓

```
<meta name='apple-itunes-app' content='app-id=477927812'>
    <!-- 上面的方法仅限于iOS设备，且无法定义Banner的形式。另外打开应用时也无法告诉App端要做什么操作。 -->
    <!-- app-id 对应 App Store里面ID，我使用的百度贴吧的ID-->
```

页面浮动在底部`App内打开`为普遍方法，样式可能会因站而异，但原理相同，话不多说直接上code

[scode type="share"][protocolcheck][2][/scode]

**Examp.html**

```
<!DOCTYPE html>
<html>

<head lang="en">
    <meta charset="UTF-8">
    <title>Custom Protocol Detection</title>
</head>

<body>
    <h1>Click one of these labels:</h1>

    <div href="blahblah:randomstuff">虚假url
    </div>
    <div href="mailto:johndoe@somewhere.com">真实url
    </div>
    <script src="https://code.jquery.com/jquery-1.11.2.min.js"></script>
    <script src="protocolcheck.js"></script>
    <script src="example.js"></script>
</body>

</html>
```

---

**Example.js**

```
$(function () {
    $("div[href]").click(function (event) {
        window.protocolCheck($(this).attr("href"),
            function () {
                alert("url无效！");
            });
        event.preventDefault ? event.preventDefault() : event.returnValue = false;
    });
});
```

[scode type="blue"]是否需要使用Jquery因人而异，请根据自身项目自行决定，事件绑定方法，也请根据页面情况自行修改！[/scode]

[2]: https://github.com/ismailhabib/custom-protocol-detection
