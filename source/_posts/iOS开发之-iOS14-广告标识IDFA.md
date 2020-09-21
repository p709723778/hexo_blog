---
title: iOS开发之 ~ iOS14 广告标识IDFA
date: 2020-07-23 16:53:32
tags: [iOS, IDFA]
categories: [iOS开发]

---

> 用手机自带Safari 打开 [iOS14_Beta_Profile](/download/iOS_14_DP_Beta_Profile.mobileconfig) 可以进行下载描述文件安装体验iOS14系统



#### 适配方案:

1.需要在info.plist 中添加 `NSUserTrackingUsageDescription` 对应的描述文案

2.iOS14下新增了IDFA 权限申请 API 添加申请权限的代码,代码如下:



​	首先要导入系统框架

```objc
@import AdSupport;
@import AppTrackingTransparency;
```

​	适配代码

```objc
+ (NSString *)idfa
{
    __block NSString *advertisingId = @"";

    if (@available(iOS 14, *)) {
        if (ATTrackingManager.trackingAuthorizationStatus != ATTrackingManagerAuthorizationStatusAuthorized) {
            [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
                if (status == ATTrackingManagerAuthorizationStatusAuthorized) {
                    NSLog(@"用户选择了同意授权IDFA权限 %@", advertisingId);
                    advertisingId = [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];
                } else {
                    NSLog(@"用户选择了拒绝授权IDFA权限");
                }
            }];
        } else {
            advertisingId = [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];
        }
    } else {
        // ios14以下

        if ([[ASIdentifierManager sharedManager] isAdvertisingTrackingEnabled]) {
            advertisingId = [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];
        } else {
            NSLog(@"请在设置 -> 隐私 -> 广告 -> 限制广告跟踪打开广告跟踪功能");
        }
    }
    return advertisingId;
}
```

<!--more-->

- 查看IDFA的授权状态

   通过 `trackingAuthorizationStatus` 属性获取授权状态

- 获取IDFA权限

   通过 `+ (void)requestTrackingAuthorizationWithCompletionHandler:(void(^)(ATTrackingManagerAuthorizationStatus status))completion;` 函数获取IDFA值

#### IDFA授权提示框

{% asset_img 1.png 预览效果 %}



#### 如何找IDFA授权开关

设置 -> 隐私 - > 跟踪

{% asset_img 2.png 预览效果 %}



#### 总结:

1. 线上AppStore没有适配的包安装到 iOS14 系统下是无法获取IDFA的,系统接口默认返回 `00000000-0000-0000-0000-000000000000`,因为在 iOS14 下IDFA需要单独申请权限, 在xcode12下系统新增了一个系统库 `AppTrackingTransparency.framework` 来专门管理IDFA权限,此库提供了查看IDFA的授权状态及获取IDFA权限的接口

2. IDFA总开关 关闭再打开时,IDFA的值会发生变化
3. IDFA 应用授权列表里面的所有应用全部关闭再打开时,IDFA的值发生变化
4. IDFA 应用授权列表里面的应用超过两个以上,保证其中有一个应用授权没有关闭时,其他应用授权关闭打开,IDFA的值不会发生变化
5. IDFA 应用授权列表里面只有一个应用时, 单独关闭再打开开关时,IDFA的值发生变化

6. IDFA总开关是关闭状态时,应用不会提示让用户授权IDFA权限