---
title: Mac ~ 教你怎么显示隐藏文件，或者关闭显示隐藏文件
date: 2012-09-01 15:47:53
tags: [Mac]
categories: [Mac]
---

缺省情况下，在 Mac 下是不显示隐藏文件的，Finder 也未提供设置是否显示隐藏文件的选项，不像 Windows 下，有一个“文件夹选项“设置界面里可以控制，但这并不表示 Mac 下无法显示隐藏文件，我可以通过“终端”，用命令行设置这个选项

#### 方法一 :

- 显示：

```bash
defaults write com.apple.finder AppleShowAllFiles -bool true
```

- 隐藏：

```bash
defaults write com.apple.finder AppleShowAllFiles -bool false
```

<!--more-->

#### 方法二:

ShowOrHide工具  开关隐藏文件的显示
http://download.csdn.net/detail/p709723778/6483095