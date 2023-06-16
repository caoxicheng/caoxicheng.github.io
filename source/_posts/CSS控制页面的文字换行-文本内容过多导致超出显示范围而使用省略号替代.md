---
title: CSS控制页面的文字换行 &&  文本内容过多导致超出显示范围而使用省略号替代
date: 2020-01-19 15:27
tags:
---

在一开始的学习过程中，我们遇到换行这种情况，大多都是采用强制换行就行。

但是在实际的生产环境中，难免需要考虑单词是否被换行所切割、单词间距、空格等因素。

现在来理一下那些控制文字的CSS3样式

<!--more-->

## 文字种类“<span style="color:red">常识</span>”

CJK 文字和 !CJK 文字有各自的排版规则。

在这里，CJK 代表 Chinese, Japanese, and Korean，即东亚文字，!CJK 就是非东亚文字，大多数情况下是西欧文字。

在文字的呈现规则上，两者很不相同，CJK 文字中，一个字母就是一个字素（单词），独立成义，!CJK 文字中，一些字母组成一个字素，并且字素们由连接符“-”连接，或由空格“ ”分隔。

有关 CJK 文字更多的排版规则上，比较有代表性的是：对中文来说，标点符号不能成为行首（特殊除外）；双字长的标点符号（省略号、破折号）不能断开。
对于 !CJK，主要是：单词不能在中间不合法地断开（合法情况例如从连接符处断开）；标点符号不能成为行首（特殊除外）

## 基础属性

| 属性                         | 概要                                                    |
| -------------------------- | ----------------------------------------------------- |
| word-brak                  | 指定了怎样在单词内断行                                           |
| overflow-wrap(别名word-warp) | 用来说明当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止其溢出，浏览器是否允许这样的单词中断换行。 |
| white-space                | 设置如何处理元素中的 空白。                                        |
| line-break                 | 处理如何断开（break lines）带有标点符号的中文、日文或韩文（CJK）文本的行。          |

## word-break

- `normal` 使用默认的断行规则。
- `break-all` 对于non-CJK (CJK 指中文/日文/韩文) 文本，可在任意字符间断行, 即强制换行。
- `keep-all` CJK 文本不断行。 Non-CJK 文本表现同 `normal`。
- `break-word` 若剩下空间不够下一个单词，直接换行

## overflow-wrap(word-warp)

- `normal` 表示在正常的单词结束处换行。
- `break-word` 表示如果行内没有多余的地方容纳该单词到结尾，则那些正常的不能被分割的单词会被强制分割换行。

## white-space

- `normal` 连续的空白符会被合并，换行符会被当作空白符来处理。换行在填充「行框盒子(line boxes)」时是必要。

- `nowrap` 和 normal 一样，连续的空白符会被合并。但文本内的`换行无效`。

- `pre` 连续的空白符会被保留。在遇到换行符或者<br>元素时才会换行。 即照常输出

- `pre-wrap` 连续的空白符会被保留。在遇到换行符或者<br>元素，或者需要为了填充「行框盒子(line boxes)」时才会换行。

- `pre-line` 连续的空白符会被合并。在遇到换行符或者<br>元素，或者需要为了填充「行框盒子(line boxes)」时会换行。

- `break-spaces` 与 pre-wrap的行为相同，除了:
  
  - 任何保留的空白序列总是占用空间，包括在行尾。
  - 每个保留的空格字符后都存在换行机会，包括空格字符之间。
  - 这样保留的空间占用空间而不会挂起，从而影响盒子的固有尺寸（最小内容大小和最大内容大小）。

下面的表格总结了各种 white-space 值的行为：

|              | 换行符 | 空格和制表符 | 文字转行 |
| ------------ | --- | ------ | ---- |
| normal       | 合并  | 合并     | 转行   |
| nowrap       | 合并  | 合并     | 不转行  |
| pre          | 保留  | 保留     | 不转行  |
| pre-wrap     | 保留  | 保留     | 转行   |
| pre-line     | 保留  | 合并     | 转行   |
| break-spaces | 保留  | 保留     | 转行   |

## line-break

- `auto` 使用默认的断行规则分解文本。

- `loose` 使用尽可能松散（least restrictive）的断行规则分解文本。一般用于短行的情况，如报纸。

- `normal` 使用最一般（common）的断行规则分解文本。

- `strict` 使用最严格（stringent）的断行原则分解文本。

- `anywhere`  在每个印刷字符单元（typographic character unit）的周围，都有一个自动换行（soft wrap）的机会，包括任何标点符号（punctuation character）或是保留的空白字符（preserved white spaces），或是单词之间。但忽略任何用于阻止换行的字符，即使是来自 GL、WJ 或 ZWJ 字符集的字符，或是由 word-break 属性强制的字符。不同的换行机会拥有相同的优先级。也不应用断字符（hyphenation，可能是 "-"）。
  
  英文原文：There is a soft wrap opportunity around every typographic character unit, including around any punctuation character or preserved white spaces, or in the middle of words, disregarding any prohibition against line breaks, even those introduced by characters with the GL, WJ, or ZWJ character class or mandated by the word-break property. The different wrapping opportunities must not be prioritized. Hyphenation is not applied.

## 总结

用得到的还是

```
word-break: break-word;
overflow-wrap: break-word;
```

## 附带·文本超出附加省略号

css

```
多行
.ellipsis{
    text-overflow: -o-ellipsis-lastline;
    overflow: hidden;
    text-overflow: ellipsis; //省略号
    display: -webkit-box; //
    -webkit-line-clamp: 1; //行数
    -webkit-box-orient: vertical;
}

单行：

.new {
  overflow:hidden;
  white-space: nowrap;
  text-overflow:ellipsis;
  width:100%; //或者具体大小
}
```

---

## 说点别的

上海20号晚上确认已经确证2019新型冠状病毒
这个年过的不平静啊
赶紧买了点n95口罩狗命要紧(ó﹏ò｡)

---

链接: [word-wrap 解惑][1]

[1]: https://fed.taobao.org/blog/taofed/do71ct/confused-about-word-wrap/?spm=taofed.blogs.blog-list.6.27f95ac8JgXruF
