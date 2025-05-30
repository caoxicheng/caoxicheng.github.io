---
title: 前端如何优化首次渲染性能
date: '2020-03-31 15:49'
tags: []
categories:
  - 技术
  - CSS
excerpt: >-
  性能优化需求产生原因 前端项目日益加大，并且三大框架Angular, Vue,
  React导致单页面应用(SPA)大行其道,小程序的日益化也导致现在前端需要优化项目的首次渲染速度。
  如何优化，归根结底就是了解一个网页是如何渲染的，一个简单的网页渲染分为两个大步骤 - 文档对象模型DOM - CSS对...
description: 前端如何优化首次渲染性能 - 详细介绍与实践经验分享
keywords:
  - 前端如何优化首次渲染性能
  - 技术
  - CSS
cover: ''
---

## 性能优化需求产生原因

前端项目日益加大，并且三大框架Angular, Vue, React导致单页面应用(SPA)大行其道,小程序的日益化也导致现在前端需要优化项目的首次渲染速度。

如何优化，归根结底就是了解一个网页是如何渲染的，一个简单的网页渲染分为两个大步骤

- 文档对象模型DOM
- CSS对象模型CSSOM

## 网页的渲染步骤

### 文件的获取

#### DOM

下图是浏览器获取网页DOM文件并渲染成DOM的步骤图

![full-process.png](2472100554.png)

1. **转换：** 浏览器从网络或者本地磁盘读取HTML的原始字节，并根据文档的指定编码（such as utf-8)转换成可视字符
2. **令牌化** 浏览器将字符转换成W3C HTML5 标准规定的各种令牌,例如 <html>，每个令牌都有其特殊意义和规则
3. **词法分析** 将令牌转化成定义其属性和规则的对象
4. **DOM构建** HTML标记定义不同标记之间的关系，创建一个树形的主从关系，成为DOM树

#### CSSOM

浏览器在构建页面时，从head标记中获取到了link标记，其引用了外部的样式 style.css, 会立即发起请求获取此css文件,例如

```
body { font-size: 16px }
p { font-weight: bold }
span { color: red }
p span { display: none }
img { float: right }
```

PS: 诚然，直接使用内联CSS在速度让肯定比请求网络资源快，但是现代前端页面崇尚将内容与设计进行关注点分离；即设计人员专注于CSS，开发者侧重HTML

获取CSS文件之后，我们依旧需要讲CSS规则转换成浏览器能够理解和处理的东西，所以我们会重复HTML的步骤，不过主体变为了CSS。

![cssom-construction.png](1875281029.png)

字节 => 字符 => 令牌 => 节点 => CSSOM

[scode type="blue"]为什么是树形结构，是为了样式的继承即规则: "向下级联"[/scode]

![cssom-tree.png](440089994.png)

### 生成渲染树

在完成DOM和CSSOM各自的转换之后，浏览器会把两者整合为渲染树

- DOM和CSSOM合并后形成渲染树
- 渲染树只包含页面需要显示的部分
- 计算布局每一个对象的精确位置和大小
- 最后一个将像素渲染到屏幕上

#### 渲染细节

1. 从DOM树的根节点开始遍历每一个`可见`节点
   - 某些节点不可见(脚本标记，元标记等)会被跳过忽略
   - 设置了css隐藏样式的节点也会被忽略，例如`display: none`
2. 为每一个可见的节点匹配其对应的CSSOM规则并应用它
3. 讲可见节点整合进渲染树，包含其CSSOM规则和内容

### 布局

知道了需要渲染的内容，还需要匹配屏幕，即将节点转换为屏幕上的像素点，这一步称为“栅格化”或“绘制”

执行渲染树构建、布局、绘制所需的时间取决于文档的大小、应用的样式以及硬件性能。

文档大小和时间成正比、样式复杂度和时间成正比、性能和时间成反比

### 总结步骤

1. 处理HTML标记并构建DOM树
2. 处理CSS标记并构建CSSOM树
3. 将DOM和CSSOM合并为一个渲染树
4. 根据渲染树来布局，并计算每个节点的几何信息
5. 将节点绘制到屏幕上

[scode type="red"]请注意当CSSOM和DOM发生更改，上述所有步骤将会重新执行！[/scode]

**优化的关键就是最大限度缩短上述5步的时间开销**

## 导致阻塞的原因

### 阻塞渲染的CSS

默认情况下，CSS会导致渲染的阻塞，即直至CSSOM构建完毕完成前，浏览器都不会渲染任何已处理的内容！

我们知道在构建`渲染树`时，DOM和CSSOM必须同时存在。

**HTML和CSSd都是阻塞渲染的资源**, 但是从实际状况出来，HTML的优先级是强于CSS的,因为没有HTML等于没有需要展示的实质内容。

[scode type="share"]CSS 是阻塞渲染的资源。需要将它尽早、尽快地下载到客户端，以便缩短首次渲染的时间。[/scode]

诚然，有些CSS样式只在某些特殊的情况下需要，比如打印和媒体查询,我们可以通过特殊的标识符来通知浏览器此资源`不阻塞渲染`

```
<link href="style.css" rel="stylesheet">
<link href="print.css" rel="stylesheet" media="print">
<link href="other.css" rel="stylesheet" media="(min-width: 40em)">
```

[scode type="share"]请注意“阻塞渲染”仅是指浏览器是否需要暂停网页的首次渲染，直至该资源准备就绪。无论哪一种情况，浏览器仍会下载 CSS 资产，只不过不阻塞渲染的资源优先级较低罢了。[/scode]

### 阻塞渲染的Javascript

javascript可以修改页面的内容和样式和用户交互，当然它也会阻塞DOM构建和延缓页面的渲染，所以为了最佳的性能，我们应该让javascript异步执行，并使得页面渲染的关键路径中不要出现任何JavaScript

[scode type="share"]一个重要事实：我们的脚本在文档的何处插入，就在何处执行。所以绝大多数JavaScript脚本的位置都在body标志的尾部[/scode]

1. 脚本在文档中的位置很重要
2. 当浏览器遇到一个script标志时，DOM构建讲暂停，直至脚本运行结束
3. JavaScript可以查询DOM和CSSOM，并修改他们
4. JavaScript会在CSSOM就绪`之后`运行！

默认情况下，内联和外部JavaScript都会暂停DOM的渲染，通过添加`async`关键字，可以大幅提升渲染性能

```
<script src="app.js" async></script>
```

## 如何监听网页渲染关键步骤

通过浏览器的`Navagation Timing Api`捕获所有相关时间戳配合页面加载时其他浏览器事件，我们可以捕获并计算出到浏览器从获取第一批文档资源到页面展示完整的完整时间损耗

相关时间戳的含义

- domLoading: 整个过程的起始时间戳，浏览器即将开始解析接受到的第一批HTML文档文件
- domInteractive: 表示浏览器完成对所有HTML的解析并且DOM构建完成的时间点
- domContentLoaded: 表示DOM准备继续并且没有样式表阻止JavaScript执行的时间点，即开始构建渲染树
  - 许多JavaScript框架在等待此事件发生后再运行其本身的逻辑
- domComplete: 顾名思义，所有事件完成，所有资源加载完成，即页面转环停止转动
- loadEvent: 做为每个网页加载的最后一步，浏览器会触发`onload`事件

HTML 规范中规定了每个事件的具体条件：应在何时触发、应满足什么条件等等。对我们而言，我们将重点放在与关键渲染路径有关的几个关键里程碑上：

- `domInteractive`： 表示DOM准备就绪的时间点
- `domContentLoaded `： 一般表示DOM和CSSOM均准备就绪的时间点
  - 如果没有阻塞解析器的JavaScript, 则`DOMContentLoaded `将在`domInteractive `后立即触发
  - 因为一般来说css在文件中比JavaScript先获取
- `domComplete `: 表示网页以及其所有子资源都加载完毕的时间点

```
<!DOCTYPE html>
<html>
  <head>
    <title>Critical Path: Measure</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
    <script>
      function measureCRP() {
        var t = window.performance.timing,
          interactive = t.domInteractive - t.domLoading,
          dcl = t.domContentLoadedEventStart - t.domLoading,
          complete = t.domComplete - t.domLoading;
        var stats = document.createElement('p');
        stats.textContent = 'interactive: ' + interactive + 'ms, ' +
            'dcl: ' + dcl + 'ms, complete: ' + complete + 'ms';
        document.body.appendChild(stats);
      }
    </script>
  </head>
  <body onload="measureCRP()">
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
  </body>
</html>
```

## 优化pageSpeed

为了尽快完成首次渲染，我们需要最大限度减少以下三种可变因素:

- 关键资源的数量
- 关键路径的长度
- 关键字节的长度

关键资源是可能阻止网页首次渲染的资源

同样，关键路径长度受所有关键资源与其字节大小之间依赖关系图的影响：某些资源只能在上一资源处理完毕之后才能开始下载，并且资源越大，下载所需的往返次数就越多。

最后，浏览器需要下载的关键字节越少，处理内容并让其出现在屏幕上的速度就越快。要减少字节数，我们可以减少资源数（将它们删除或设为非关键资源），此外还要压缩和优化各项资源，确保最大限度减小传送大小。

**优化关键渲染路径的常规步骤如下：**

1. 对关键路径进行分析和特性描述：资源数、字节数、长度。
2. 最大限度减少关键资源的数量：删除它们，延迟它们的下载，将它们标记为异步等。
3. 优化关键字节数以缩短下载时间（往返次数）。
4. 优化其余关键资源的加载顺序：您需要尽早下载所有关键资产，以缩短关键路径长度。

### JavaScript优化建议

- 优先异步JavaScript
- 避免使用同步服务器调用
- 延迟解析JavaScript
- 避免运行长时间的JavaScript

### CSS优化建议

- 将CSS置于Head标志内
- 避免使用CSS import
- 适当情况下使用内联CSS

[优化关键渲染路径](https://developers.google.com/web/fundamentals/performance/critical-rendering-path)

## 补充: 回流和重绘

在之前说到的浏览器绘制步骤中

- 回流:改变DOM的布局
- 重绘:改变DOM除布局以外的部分（颜色，透明度）

那么什么时候会触发回流和重绘呢？

**回流一定会触发重绘，而重绘不一定会触发回流！**
