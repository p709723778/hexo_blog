---
title: iOS开发之 ~ dSYM 文件结构
date: 2020-04-17 19:26:01
tags: [iOS, dSYM, Crash]
categories: [iOS开发]
---

## 1.获取Crash文件

- iPhone设备上获取: 设置 -> 隐私 -> 分析与改进 -> 分析数据 -> 找到对应应用的.ips文件

- Xcode 上查看: Xcode -> Window -> Devices and Simulators -> 选中Crash的设备 -> View Device Logs -> This Device -> 找到对应的进程crash文件 -> 右键Export Log -> 保存到需要保存的位置



## 2.获取 dSYM 符号表

> .dSYM文件是什么?
>
> 简称 : Debugger Symbols
>
> .dSYM文件是一个符号表文件, 这里面包含了一个16进制的保存函数地址映射信息的中转文件, 所有Debug的symbols都在这个文件中(包括文件名、函数名、行号等).

<br/>

- 通过 Finder 目录查找: Users -> Home -> Library -> Developer -> Xcode/Archives -> 找到对应的.xcarchive -> 右键显示包内容-> dSYMs -> xxxx.app.dSYM

- Xcode 上查看: Xcode -> Window -> Organizer -> Archives -> 选中你打包的Archive -> 右击Show In Finder -> 目标Archive -> 右键显示包内容-> dSYMs -> xxxx.app.dSYM

  

## 3.dSYM 文件结构



