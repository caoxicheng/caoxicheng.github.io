---
title: angular中使用canvas生成随机字符验证码
date: 2020-07-21 00:04
tags:
---

1. 没什么复杂的东西，旋转角度被我注释掉了，难度太大，酌情添加
2. 随机生成6位带字母的随机数请看另一篇文章

[post cid="393" /]

<!--more-->

HTML

```
<canvas  (click)="getNewValidate()"></canvas>
```

TS

```
private canvas: HTMLCanvasElement; //获取到canvas对象！
  private context: CanvasRenderingContext2D;

  ngOnInit() {
    this.initValidate();
    this.getNewValidate();
  }


  public getNewValidate(): void {
    this.context.clearRect(0, 0, 200, 40);//清除画布
    this.getValidate();//生成一个六位数的验证码
    this.drawPoint(100);//画100个干扰点
    this.drawLine(8);//画8条干扰线
  }


  private initValidate(): void {
    this.canvas = document.querySelector("canvas");
    this.canvas.width = 200;//设置canvas宽度
    this.canvas.height = 40;//设置canvas高度

    this.context = this.canvas.getContext("2d"); //获取到canvas的绘图环境！
    this.context.font = "bold 20px 微软雅黑";//设置字体
    this.context.strokeRect(0, 0, 200, 40);//绘制一个矩形框
  }

  private getValidate(): void {
    const codeLibrary = Math.random().toString(36).substr(2, 6); //生成6位随机数带字母
    console.log(codeLibrary);
    for (var i = 0; i < 6; i++) {
      const x = 20 + i * 20;
      const y = 20 + Math.random() * 10;
      const txt = codeLibrary[i];//获取到随机的内容
      this.context.fillStyle = this.getColor();//设置字体颜色
      this.context.translate(x, y);//字体移动
      // const deg = 180 * Math.random() * Math.PI / 180;
      // this.context.rotate(deg);//字体随机旋转度数
      this.context.fillText(txt, 0, 0);//将文字写到canvas上面
      // this.context.rotate(-deg);
      this.context.translate(-x, -y);
    }
  }

  private drawPoint(X): void {
    for (var i = 0; i < X; i++) {
      this.context.beginPath();
      const x = Math.random() * 200;
      const y = Math.random() * 40;
      this.context.moveTo(x, y);
      this.context.lineTo(x + 1, y + 1);
      this.context.strokeStyle = this.getColor();
      this.context.stroke();//开始绘制
    }
  }

  private drawLine(m): void {
    for (var i = 0; i < m; i++) {
      this.context.beginPath(); //开始一个路径
      this.context.moveTo(Math.random() * 200, Math.random() * 40);//设置起点坐标
      this.context.lineTo(Math.random() * 200, Math.random() * 40);//设置终点坐标
      this.context.strokeStyle = this.getColor();
      this.context.stroke();//开始绘制
    }
  }

  private getColor(): string {
    const r = Math.floor(Math.random() * 256);
    const g = Math.floor(Math.random() * 256);
    const b = Math.floor(Math.random() * 256);
    return "rgb(" + r + "," + g + "," + b + ")";
  }
```

[scode type="blue"]当然我觉得也可以传入参数以及设置默认参数来实现自定义的功能。仁者见仁智者见智[/scode]
