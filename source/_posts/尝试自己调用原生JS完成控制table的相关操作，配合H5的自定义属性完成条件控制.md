---
title: 尝试自己调用原生JS完成控制table的相关操作，配合H5的自定义属性完成条件控制
date: 2019-09-04 22:50
tags:
---

入行也快一年了，用的最多都是第三方的插件，虽然最近也有在看源码，不过刚好有个类似的需求，花了个把小时做了个demo然后水篇博客😁

因为热爱，所以热爱。

<!--more-->

## 创建元素

一般比如说自己手动创建一个表格

1. 创建`table`
2. 创建`tr`
3. 创建`td`
4. 追加`td`到`tr` ps:防止多次重绘影响性能，一次性在js中完成DOM然后一次添加
5. 追加`tr`到`table`或`tbody`中

这是我所知道的流程。使用的到的原生js也不过几个

- `window.document.createElement('elementName');`
- `Element.appendChild()`| `Element.append` 后者可以一次多个，不过还有些缺陷问题，建议`appendchild`

## 模块化

既然准备自己造轮子，那么就肯定得灵活一点。那我就传入一个数组对象。一个对象是其中的一行，或者数组数组，能支持遍历，有规律就行

### 函数：创建行数组

```
// 创建tr 根据数组对象创建对应的列
    function _create_tr(dataList){
      let _trNodeList = []; // 这里应该是NodeList对象的，不过我不会创建。也比较赶时间做demo就用数组代替了
      for(let _i = 0; _i < Object.keys(dataList).length; _i++){  // 这里先循环出tr行
        // 先创建行
        let _tr = window.document.createElement('tr');
        // 创建列, 遍历属性创建列
        let tdList = Object.keys(dataList[_i]);  //列名集合
        for( let __i = 0; __i < tdList.length; __i ++){
          let __td = window.document.createElement('td');
          __td.innerHTML = `${dataList[_i][tdList[__i]]}`;
          _tr.appendChild(__td);
        }
        _trNodeList.push(_tr); //推进数组
      }
      return _trNodeList;
    }
```

### 函数：在表格某一行下方添加表格行

[scode type="share"]关于下方用到的API https://www.w3.org/TR/html50/tabular-data.html#dom-tr-rowindex[/scode]

```
/**
     *获取Nodelist 添加至 目标元素下方
     *targetNode 目标行tr
     */
    function _append_tr(NodeList, targetNode) {
      // 1. 获取targetNode 在第几行
      let _trIndex = targetNode.rowIndex;
      // 2. 获取targetNode 的父元素
      let _trParent = targetNode.parentNode;
      // 3. 记录下添加了几行，以及相对应的状态
      targetNode.dataset.isExpend = true;
      targetNode.dataset.rowsCount = NodeList.length;
      NodeList.forEach( Node => {
        let tempTr = _trParent.insertRow(_trIndex++);
        tempTr.innerHTML = Node.innerHTML;
      })
    }
```

### 函数：删除某一行开始的几行

为了满足需求的添加和删除行

```
/**
     *删除对应行下面的 某些行
     *
     */
     function _remove_tr(targetNode, rowsCount = 0) {
      // 1. 判断是否传值 rowsCount,本程序是先添加再删除。这里的步骤对应上面的第三步
      if (rowsCount === 0 && targetNode.dataset.rowsCount) {
        rowsCount = targetNode.dataset.rowsCount;
      }
      // 2. 定位父元素
      let _trParent = targetNode.parentNode;
      // 3. 定位当前元素
      let _trIndex = targetNode.rowIndex;
      // 4. 删除行
      for( let _i = 0; _i < rowsCount; _i++) {
        _trParent.deleteRow( _trIndex); //这里index不需要动。因为删除之后后面的会自动前移
      }
      // 5. dataset参数设置
      targetNode.dataset.isExpend = false;
      delete targetNode.dataList.rowsCount;
     }
```

### demo

```
let demoData = [{name: 'C', age: '3'}, {name: 'D', age: '4'}];

    // 绑定点击事件
    let trs = window.document.querySelectorAll('tr');
    trs.forEach( Node => {
          Node.addEventListener('click', function($event){
          if ($event.target.parentNode.dataset.isExpend === "true") {
            _remove_tr($event.target.parentNode);
          } else {
            _append_tr(_create_tr(demoData), $event.target.parentNode);
          }
        })
    })
```

## 小总结一下

虽然第三方的库基本可以覆盖到方方面面了。几年前会jQuery好像就可以走遍天下都不怕了。但是现在ui库，js库多如牛毛。框架功能也越来越强大。但是归根到底还是取决于浏览器的API。希望在以后的开发和学习中。不忘最基本的原生JS，当然不会后端的前端不是一个好光头（再底层就先不考虑了😂）
