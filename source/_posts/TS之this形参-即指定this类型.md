---
title: TS之this形参-即指定this类型
date: 2020-03-03 22:04
tags:
---

在VS code 中 使用ts编码时写了一个很简单的代码:

```
window.onmousedown = xxx
```

此时鼠标移动上来会出现类型推断的提示：

![批注 2020-03-03 220407.png](2386771621.png)

这个时候我就在想： 两个参数吗？ 可是我们平时调用不就是一个Event对象传入吗？

于是我就去翻阅资料

## 结论

TS中可以使用`this`形参来指定当前函数的this对象

举个栗子

```
function demo() {
    console.log(this.length); //this并没有length这一方法，所以不会自动弹出联想
}


function demo2(this: Array<any>){
    console.log(this.length)
}
```

但是需要注意的是，this形参`只能在参数列表的第一个出现`，并且只会在ts文件中出现，编译成js后并会有这个形参，只是辅助TS校验用

即：传递参数的时候跳过this

```
function demo(this: void, arg:any) {
    //这种情况即告诉编译器this不存在，无法调用this
    console.log(arg);
}

//调用

demo('测试');
```

所以图1的情况就是TS编译器在告诉我们此时函数的this上下文是什么(图中为GlobalEventHandlers或者重载的Window对象)

![批注 2020-03-03 220913.png](902455803.png)

---

说点别的

win10自带的截屏除了原来的PrintScrn

还可以使用win + shift + s  调出更加复杂的截图界面
