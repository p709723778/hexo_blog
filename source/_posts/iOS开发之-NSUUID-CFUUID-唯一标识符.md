---
title: 'iOS开发之 ~ NSUUID , CFUUID 唯一标识符'
date: 2019-07-02 15:50:09
tags: [UUID]
categories: [iOS开发]
---

### CFUUID

从iOS2.0开始，`CFUUID`就已经出现了。它是**CoreFoundatio**包的一部分，因此API属于C语言风格。CFUUIDCreate 方法用来创建CFUUIDRef，并且可以获得一个相应的NSString字符串

如下代码：
```objc
CFUUIDRef cfuuid = CFUUIDCreate(kCFAllocatorDefault);

NSString *cfuuidString = (NSString 
*)CFBridgingRelease(CFUUIDCreateString(kCFAllocatorDefault, cfuuid));

CFRelease(cfuuid);
```

获得的这个CFUUID值系统并没有存储。每次调用CFUUIDCreate，系统都会返回一个新的唯一标示符。

### NSUUID

`NSUUID`在iOS 6中才出现，这跟`CFUUID`几乎完全一样，只不过它是Objective-C接口。+ (id)UUID 是一个类方法，调用该方法可以获得一个UUID。通过下面的代码可以获得一个UUID字符串：

```objc
NSString *uuid = [[NSUUID UUID] UUIDString];
```

跟`CFUUID`一样，这个值系统也不会存储，每次调用的时候都会获得一个新的唯一标示符。在我读取NSUUID时，注意到获取到的这个值跟CFUUID完全一样（不过也可能不一样）：

格式示例: `973FC752-75EA-4217-BEB3-CF5DD0610FC2`