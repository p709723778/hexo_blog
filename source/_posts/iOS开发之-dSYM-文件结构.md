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

~~很久之前iTunes Connect -> 我的App -> 对应App -> 活动 -> 所有构建版本 -> 选择对应构建版本 -> 包含符号~~

~~包含符号这里之前是可以"下载dSYM"文件,现在苹果不提供下载功能~~

  

## 3.dSYM 文件结构



|        字段         |                             含义                             |
| :-----------------: | :----------------------------------------------------------: |
| Incident Identifier |    报告的唯一标识符，两份报告决不会共享同一个事件标识符。    |
|  CrashReporter Key  | 每个设备的匿名标识符，来自同一设备的两个报告将包含相同的值。 |
|   Hardware Model    |                           设备类型                           |
|       Process       |            进程名称[进程 id]，进程通常是 app 名字            |
|        Path         |                       可执行程序的位置                       |
|     Identifier      |                 App包名,例如:com.xxx.appName                 |
|       Version       |                          App版本号                           |
|      Code Type      |                           CPU 架构                           |
|        Role         |                在发生Crash时进程的的task_role                |
|   Parent Process    |   父进程，iOS中App通常都是单进程的，一般父进程都是 launchd   |
|      Date/Time      |                Crash发生的时间，可读的字符串                 |
|     Launch Time     |                       程序开始运行时间                       |
|     OS Version      |                     系统版本（build 号）                     |
|  Baseband Version   |                           基带版本                           |
|   Report Version    | Crash日志的格式，目前基本上都是104，不同的version里面包含的字段可能有不同 |
|   Exception Type    |                           异常类型                           |
|   Exception Codes   |                           异常代码                           |
| Termination Signal  |                           终止信号                           |
| Termination Reason  |                           终止原因                           |
| Terminating Process |                           终止进程                           |
| Triggered by Thread |                           引发线程                           |
|    Binary Images    |                          二进制映像                          |

