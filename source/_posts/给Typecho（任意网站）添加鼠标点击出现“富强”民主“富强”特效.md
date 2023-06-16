---
title: 给Typecho（任意网站）添加鼠标点击出现“富强”民主“富强”特效
date: 2019-08-21 18:19
tags:
---

为了中华民族之伟大复兴而努力奋斗！

<!--more-->

将代码放在主题的footer.php中的</body>之前即可。

```
<script type="text/javascript"> 
/* 鼠标特效 */
let a_idx = 0; 
jQuery(document).ready(function($) { 
    $("body").click(function(e) { 
        let a = new Array("富强", "民主", "文明", "和谐", "自由", "平等", "公正" ,"法治", "爱国", "敬业", "诚信", "友善"); 
        let $i = $("<span/>").text(a[a_idx]); 
        a_idx = (a_idx + 1) % a.length; 
        let x = e.pageX, 
        y = e.pageY; 
        $i.css({ 
            "z-index": 999999999999999999999999999999999999999999999999999999999999999999999, 
            "top": y - 20, 
            "left": x, 
            "position": "absolute", 
            "font-weight": "bold", 
            "color": "#ff6651" 
        }); 
        $("body").append($i); 
        $i.animate({ 
            "top": y - 180, 
            "opacity": 0 
        }, 
        1500, 
        function() { 
            $i.remove(); 
        }); 
    }); 
}); 
</script>
```

版权归[知言笔记][1]所有

[1]: https://www.bijiv.com/t/34
