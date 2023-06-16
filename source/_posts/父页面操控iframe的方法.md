---
title: 父页面操控iframe的方法
date: 2019-07-25 15:48
tags:
---

因为业务需要需要在父页面操控iframe调用的页面。这里坐下记录。

<!--more-->

```
/*  
        *父页面通过 iframe id 调用子页面的函数或者获取子页面元素的内容  
        */  
        function one() {  
            //var ifreame = window.frames["childPage1"];  
            var fifreame = window.document.getElementById('childPage1');
            if (ifreame != null && ifreame != undefined) {  
                ifreame.childFunction();  
            }  
        }  

        function two() {  
            //var ifreame = window.frames["childPage1"];  
            var fifreame = window.document.getElementById('childPage1');
            if (ifreame != null && ifreame != undefined) {  
                var myValue = ifreame.contentWindow.document.getElementById("childPage");  

                alert(myValue.innerHTML);  
            }  
        }
```

[scode type="blue"]来源--全面兼容的Iframe 与父页面交互操作：https://www.cnblogs.com/web100/p/iframe-ok.html[/scode]

---

2019-07-29

[scode type="yellow"]警告：window.frames[] IE专用，实测safari新版本手机可用，老版本会出错 [/scode]
[scode type="share"]JS获取并操作iframe中元素的方法 https://www.jb51.net/article/34942.htm[/scode]
