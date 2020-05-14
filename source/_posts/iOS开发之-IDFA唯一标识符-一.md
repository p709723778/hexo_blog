---
title: iOS开发之 ~ IDFA唯一标识符 (一)
date: 2019-06-24 20:50:14
tags: [iOS, IDFA]
categories: [iOS开发]
---

通过网上查资料看,我发现有一部分人使用IDFA用来做设备唯一标识,我个人觉的不是很好!有很大的缺陷,为什么呢?下面来详细解说

要使用IDFA首先要导入系统库 `@import AdSupport;`

```objc
@property(nonatomic, readonly) NSUUID *advertisingIdentifier;
```
>IDFA是每个设备特有的字母数字串，仅用于服务广告。(官方解释)<br>
>可以理解为广告标识符，在同一个设备上的所有App都会取到相同的值，是苹果专门给各广告提供商用来追踪用户而设的。广告标示符是由系统存储着的。
>适用于对外：例如广告推广，换量等跨应用的用户追踪等。

此属性与identifierForVendor(IDFV)不同,它在同一个设备上,所有供应商返回的都是同一个值,可能会改变: 例如:手机重置(抹掉所有内容和设置)会导致变化。

有以下几种情况会导致变化:

#### 1.设置 -> 通用 -> 还原 -> 抹掉所有内容和设置

{% asset_img 截图1.png 预览效果 %}

#### 2.设置 -> 隐私 -> 广告 -> 限制广告跟踪(开 / 关)

>下面限制广告跟踪开关
>关闭时: 可以获取到IDFA
>打开时: 无法获取到IDFA

{% asset_img 截图2.png 预览效果 %}


>a.打开限制的情况下

{% asset_img 截图3.png 预览效果 %}



>b.关闭限制的情况下

{% asset_img 截图4.png 预览效果 %}



- **官方也提供了广告跟踪是否可用的接口**

```objc
BOOL isEnabled = [[ASIdentifierManager sharedManager] isAdvertisingTrackingEnabled];
```

>小结:<br>1.限制广告追踪开关切换会导致变化;<br>2.重置手机也会导致变化<br>总结:由于各种不稳定,个人建议使用 `IDFV + KeyChain` 做为用户的设备唯一标识,此方式会在后面继续讲解.

Demo下载地址:

[点击下载](/download/testIDFA.zip)

