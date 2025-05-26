---
title: 如何在浏览器中调用系统通知API
date: '2019-07-09 12:32'
tags: []
categories:
  - 技术
excerpt: >-
  接口介绍 构造方法 let notification = new Notification(title, options) 参数 title
  一定会被显示的通知标题 options|可选 - dir : 文字的方向；它的值可以是 auto（自动）, ltr（从左到右）, or rtl（从右到左）
  -...
description: 如何在浏览器中调用系统通知API - 详细介绍与实践经验分享
keywords:
  - 如何在浏览器中调用系统通知
  - API
  - 技术
cover: ''
---

## 接口介绍

### 构造方法

```
let notification = new Notification(title, options)
```

### 参数

`title` 一定会被显示的通知标题

`options`|可选

- dir : 文字的方向；它的值可以是 auto（自动）, ltr（从左到右）, or rtl（从右到左）
- lang: 指定通知中所使用的语言。
- body: 通知中额外显示的字符串
- tag: 赋予通知一个ID，以便在必要的时候对通知进行刷新、替换或移除。
- icon: 一个图片的URL，将被用于显示通知的图标。

### 属性

Notification.permission | 只读

- 一个用于表明当前通知显示授权状态的字符串。可能的值包括：denied (用户拒绝了通知的显示), granted
  (用户允许了通知的显示), 或 default (因为不知道用户的选择，所以浏览器的行为与 denied 时相同).

### 方法

Notification.requestPermission()

- 用于当前页面向用户申请显示通知的权限。这个方法只能被用户行为调用（比如：onclick 事件），并且不能被其他的方式调用。

Notification.onclick

- 处理 click 事件的处理。每当用户点击通知时被触发。

Notification.onshow

- 处理 show 事件的处理。当通知显示的时候被触发。

Notification.onerror

- 处理 error 事件的处理。每当通知遇到错误时被触发。

Notification.onclose

- 处理 close 事件的处理。当用户关闭通知时被触发。

## 实例

### 单一消息

```
window.addEventListener('load', function(){
  if (window.Notification && window.Notification.permission !== "granted") {
    Notification.requestPermission(function(status) {
    if (Notification.permission !== status) {
        Notification.permission = status;
      }
    }) 
  }
  let button = document.getElementsByTagName('button')[0];

  button.addEventListener('click', function(){
    // 如果用户同意就创建通知
    if(window.Notification.permission === 'granted'){
      let n = new Notification("Hi!");
      n.onclick = function(){
        alert('你点击了通知！');
      }
    }

    // 如果用户没有选择是否通知
    // 注：因为在Chrom中我们无法确定permission属性是否有值，因此
    // 检查该属性是否是default 是不安全的。
    // default 用户还未被询问是否授权，所以通知不会被显示。
    // granted 表示之前已经询问过用户，并且用户已经授予了显示通知的权限。
    // denied 用户已经明确的拒绝了显示通知的权限。

    else if (window.Notification && window.Notification !== 'denied') {
      Notification.requestPermission(function(status){
        if (Notifacation.permission !== status) {
          Notification.permission = status;
        }

        // 如果用户同意了
        if (status === "granted") {
          let n = new Notification("hi");
        }
        // 否则
        else {
          alert('Hi');
        }
        });
    }
      // 如果用户拒绝接受通知
    else {
    // 我们可以让步使用常规模态的alert
    alert("hi");
    }
  });
})
```

### N条消息（tag属性，只会显示一个，会覆盖）

```
window.addEventListener('load', function(){
  if (window.Notification && window.Notification.permission !== "granted") {
    Notification.requestPermission(function(status) {
    if (Notification.permission !== status) {
        Notification.permission = status;
      }
    }) 
  }

  let button2 = document.getElementsByTagName('button')[1];
  button2.addEventListener('click', function(){
    // 如果用户同意通知，尝试通知10次
    if (Notification.permission === 'granted') {
      for (let index = 0; index < 10; index++) {
        // 感谢标记，我们应该只看到内容为 "Hi! 9" 的通知
        let n = new Notification("Hi!" + index, {tag: 'soManyNotification'});
      }
    }
    // 如果用户没有同意通知权限
    // 有值chrome不能检查permission知否有值
    // 所以检查值为default是不安全的
    else if (Notification && Notification.permission !== 'denied') {
      Notification.requestPermission(function(status){
        if (status != Notification.permission) {
          Notification.permission = status;
        }

        // 同意
        if (status === 'granted') {
          for (let index = 0; index < 10; index++) {
            // 感谢标记，我们应该只看到内容为 "Hi! 9" 的通知
            let n = new Notification("hi" + index, {tag: 'soManyNotification'})
          }
        }
        // 否则改用alert
        else {
          alert("hi!");
        }
      });
    }
    else {
      // 改用alert
      alert('Hi!');
    }
  });
}）
```

[scode type="blue"]移动端尚未支持[/scode]
