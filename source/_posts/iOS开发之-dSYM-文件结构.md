---
title: iOS开发之 ~ dSYM 文件结构
date: 2020-04-17 19:26:01
tags: [iOS, dSYM, Crash]
categories: [iOS开发]
---

## 1. 获取Crash文件

- iPhone设备上获取: 设置 -> 隐私 -> 分析与改进 -> 分析数据 -> 找到对应应用的.ips文件(获取到的 .ips 改后缀为 .crash 即可)

- Xcode 上查看: Xcode -> Window -> Devices and Simulators -> 选中Crash的设备 -> View Device Logs -> This Device -> 找到对应的进程crash文件 -> 右键Export Log -> 保存到需要保存的位置



## 2. 获取 dSYM 符号表

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

  

## 3. 通过Xcode分析Crash原因

- Xcode 上查看: Xcode -> Window -> Organizer -> Crashes -> 选择对应版本获取Crash原因线程树(如果需要获取符号化后的,在上传 AppStore 的时候勾选上传symbols)

{% asset_img 1.png 预览效果 %}

## 4. dSYM 文件结构



|           字段           | 含义                                                         |
| :----------------------: | :----------------------------------------------------------- |
|   Incident Identifier    | 报告的唯一标识符，两份报告决不会共享同一个事件标识符。       |
|    CrashReporter Key     | 每个设备的匿名标识符，来自同一设备的两个报告将包含相同的值。 |
|      Hardware Model      | 设备类型                                                     |
|         Process          | 进程名称[进程 id]，进程通常是 app 名字                       |
|           Path           | 可执行程序的位置                                             |
|        Identifier        | App包名,例如:com.xxx.appName                                 |
|         Version          | App版本号                                                    |
|        Code Type         | CPU 架构                                                     |
|           Role           | 在发生Crash时进程的的task_role                               |
|      Parent Process      | 父进程，iOS中App通常都是单进程的，一般父进程都是 launchd     |
|        Date/Time         | Crash发生的时间，可读的字符串                                |
|       Launch Time        | 程序开始运行时间                                             |
|        OS Version        | 系统版本（build 号）                                         |
|     Baseband Version     | 基带版本                                                     |
|      Report Version      | Crash日志的格式，目前基本上都是104，不同的version里面包含的字段可能有不同 |
|      Exception Type      | 异常类型                                                     |
|     Exception Codes      | 异常代码,常见代码有以下几种:<br/>1. 0x8badf00d错误码：Watchdog超时，意为“ate bad food”  <br/>2. 0xdeadfa11错误码：系统无法响应时用户强制退出，意为“dead fall”<br/>3. 0xbaaaaaad错误码：用户按住Home键和音量键，获取当前内存状态，不代表崩溃<br/>4. 0xbad22222错误码：当VOIP程序在后台太过频繁的激活时，系统可能会终止此类程序<br/>5. 0xc00010ff错误码：程序执行大量耗费CPU和GPU的运算，导致设备过热，触发系统过热保护被系统终止，意为“cool off”<br/>6. 0xdead10cc错误码：程序退到后台时还占用系统资源，如通讯录被系统终止，意为“dead lock” |
|    Termination Signal    | 终止信号                                                     |
|    Termination Reason    | 终止原因                                                     |
|   Terminating Process    | 终止进程                                                     |
|   Triggered by Thread    | 在某一个线程出了问题导致crash，Thread 0 为主线程、其它的都为子线程 |
|      Binary Images       | 二进制映像                                                   |
| Last Exception Backtrace | 最后异常回溯，一般根据这个代码就能找到crash的具体问题        |



Crash分析工具推荐

- SYM

https://github.com/zqqf16/SYM

- dSYMTools

https://github.com/answer-huang/dSYMTools