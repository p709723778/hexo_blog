---
title: Mac ~ 让隐藏的资源库文件设置为显示
date: 2012-09-03 12:02:32
tags: [Mac]
categories: [Mac]
---

> 在 Mac OS X 10.7 Lion 系统中，用户资源库文件夹（Library）被默认隐藏了，可能是由于苹果担心用户不小心误删除用户资源库中的系统必须文件，而故意将这个文件夹隐藏掉了。



不过，想让这个文件夹显示出来也非常的简单，直接在终端中执行下面这条命令就可以了：

- 不隐藏

```bash
chflags nohidden ~/Library/
```



- 隐藏

```bash
chflags hidden ~/Library
```

