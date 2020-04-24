---
title: iOS开发之 ~ App Store Connect Operation Error 集合
date: 2020-04-24 19:10:52
tags: [iOS]
categories: [iOS开发]
---

### 一 、常用的上传工具

###### 1. Transporter (macOS / Windows / Linux都有提供) 推荐

使用说明: https://help.apple.com/itc/transporteruserguide/

###### 2. ~~Application Loader (集成在Xcode内部，但是该工具已经被苹果弱化) ~~   不推荐

使用说明: https://help.apple.com/itc/apploader/

<br/>

### 二 、上传App Store Connect提示的错误以及解决方案




- ERROR ITMS-90023: 

  "Missing required icon file. The bundle does not contain an app icon for iPad of exactly '76x76' pixels, in .png format for iOS versions >= 7.0. To support older operating systems, the icon may be required in the bundle outside of an asset catalog. Make sure the Info.plist file includes appropriate entries referencing the file. See https://developer.apple.com/documentation/bundleresources/information_property_list/user_interface"

  <span style="color:green">解决方案:给应用添加指定规格大小的png格式的Icon</span>

------


- ERROR ITMS-90704: 

  "Missing App Icon. An app icon measuring 1024 by 1024 pixels in PNG format must be included in the Asset Catalog of apps built for iOS, iPadOS, or watchOS. Without this icon, apps cannot be submitted for review. For details, see https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/app-icon/."

  <span style="color:green">解决方案:给应用添加1024*1024的png格式的Icon</span>

------


- ERROR ITMS-90085: 

  "No architectures in the binary. Lipo failed to detect any architectures in the bundle executable."

  <span style="color:green">解决方案:安装lipo命令工具或者最新Command LIne Tools</span>

------



