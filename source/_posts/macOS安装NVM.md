---
title: macOS安装NVM
date: 2021-05-10 19:34
tags:
---

> 不要使用brew工具安装nvm!
> 不要使用brew工具安装nvm!
> 不要使用brew工具安装nvm!

会有一些奇怪的坑,而且官方也不推荐,虽然可以安装并且使用,但是需要才很多坑.

#官方安装

[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

其实就是下面这一句

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh
```

不过我使用命令安装的时候一直遇到网络情况,然后我仔细阅读了一下说明文档

![安装说明](../macOS安装NVM/1020905986.jpg)

也可以直接运行安装脚本,那直接走你!

1. 新建'install' 文件
2. 复制 github中的 'install.sh' 里面的内容
3. 重命名 'install' => 'install.sh'
4. ~ $ sh install.sh

安装成功!

然后打开 ~ 下的 '.zshrc' (不同终端不同文件)

![终端配置文件](../macOS安装NVM/2189619559.jpg)

保存,然后

```
$ source .zshrc
```

运行一下环境变量

然后接下来就是安装node了

---

#解决速度问题

在终端中运行下面代码修改nvm的下载源

```
export NVM_IOJS_ORG_MIRROR=https://npm.taobao.org/mirrors/iojs
```

> 我盲猜这是一次性的,所以在上面的 .zshrc 文件中,我也加入了这一段,希望能够每次运行的时候就修改我nvm的下载源.如果猜错,烦请留言指正,不胜感激!

那么安装nvm关键步骤就完成了!

---

说在最后: 最近用自己的电脑办公了,之前一直只是课余时间使用macos,这次竟然装环境就花了我大半天时间,还是要学以致用啊!
shell命令其实我也是一知半解,慢慢摸索吧.
