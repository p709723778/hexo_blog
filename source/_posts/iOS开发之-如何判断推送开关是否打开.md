---
title: iOS开发之 ~ 如何判断推送开关是否打开
date: 2013-02-21 10:41:44
tags: [iOS]
categories: [iOS开发]
---

- iOS8以前

根据 `[[UIApplication sharedApplication] enabledRemoteNotificationTypes]` 的返回值来进行判断，该返回值是一个枚举值，如下：

```objective-c
typedef enum {

  UIRemoteNotificationTypeNone   = 0,

  UIRemoteNotificationTypeBadge  = 1 << 0,

  UIRemoteNotificationTypeSound  = 1 << 1,

  UIRemoteNotificationTypeAlert  = 1 << 2,

  UIRemoteNotificationTypeNewsstandContentAvailability = 1 << 3,

} UIRemoteNotificationType;
```

如果是 `UIRemoteNotificationTypeNone` ，则可以认为推送开关没有打开，反之亦然。



- iOS8以后

```objective-c
// 返回值为BOOL类型,true为打开 false为关闭
[[UIApplication sharedApplication] isRegisteredForRemoteNotifications];
```

