---
title: iOS开发之 ~ IDFA唯一标识符 (二)
date: 2019-07-01 15:45:02
tags: [iOS, IDFA]
categories: [iOS开发]
---

在此浅谈一下App再提交AppStore的时候,需要选择你的应用是否用到了IDFA,用到IDFA的场景是哪一种?下面来描述一下.此文也有参考别人的博客!

{% asset_img 截图1.png 预览效果 %}

以上4项代表的含义：



### 1. 在 App 内投放广告

>服务应用中的广告。如果你的应用中集成了广告的时候，你需要勾选这一项。

<!--more-->

### 2. 将此 App 安装归因于先前投放的特定广告

>跟踪广告带来的安装。如果你使用了第三方的工具来跟踪广告带来的激活以及一些其他事件，但是应用里并没有展示广告你需要勾选这一项。

### 3.将此 App 中发生的操作归因于先前投放的特定广告

>跟踪广告带来的用户的后续行为。如果你使用了第三方的工具来跟踪广告带来的激活以及一些其他事件。

### 4. iOS 中的“限制广告跟踪”设置

>对您的应用使用 IDFA 的目的做下确认，只要您获取了 IDFA，那么这一项都是需要勾选的。

**提交以供审核时：**

- 如果您的应用里只是集成了广告，不追踪广告带来的激活行为，那么选择 `1 和 4`。
- 如果您的应用没有广告，而又获取了 IDFA。我们建议选择 `2 和 4`。

官方文档:
[https://developer.apple.com/documentation/adsupport/asidentifiermanager?language=objc](https://developer.apple.com/documentation/adsupport/asidentifiermanager?language=objc)

### 如何检查项目内部是否使用到了IDFA?

>- 打开终端cd到要检查的文件的根目录。
>- 执行下列语句：grep -r advertisingIdentifier .   （别少了最后那个点号）。