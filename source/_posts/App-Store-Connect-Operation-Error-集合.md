---
title: iOS开发之 ~ App Store Connect Operation Error 集合
date: 2019-12-24 19:10:52
tags: [iOS]
categories: [iOS开发]
---

### 一 、常用的上传工具

###### 1. Transporter GUI工具 推荐

使用说明: https://help.apple.com/itc/transporter/

###### 2. iTMSTransporter (macOS / Windows / Linux都有提供) 推荐

使用说明: https://help.apple.com/itc/transporteruserguide/

###### 3. ~~Application Loader (集成在Xcode内部，但是该工具已经被苹果弱化) ~~   不推荐

使用说明: https://help.apple.com/itc/apploader/

<br/>

<!--more-->

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



- ERROR ITMS-90535:

  "Unexpected CFBundleExecutable Key. The bundle at `'**/*.app/xxxx.bundle'` does not contain a bundle executable. If this bundle intentionally does not contain an executable, consider removing the `CFBundleExecutable` key from its Info.plist and using a CFBundlePackageType of BNDL. If this bundle is part of a third-party framework, consider contacting the developer of the framework for an update to address this issue."

  <span style="color:green">解决方案:找到工程中.bundle对应的plist文件 删除Executable file配置的哪一行，即可</span>

------



- ITMS-90737: 

  "Invalid Document Configuration. Document Based Apps should support either the Document Browser (UISupportsDocumentBrowser = YES) or implement Open In Place (LSSupportsOpeningDocumentsInPlace = YES/NO). Visit https://developer.apple.com/document-based-apps/ for more information."

  <span style="color:green">解决方案:在工程配置文件info.plist中增加Supports Document Browser字段并设置其值为YES</span>

------



- ITMS-90809: 

  此错误会有两种提示,解决方案都是一样的

1. Deprecated API Usage - App updates that use UIWebView will no longer be accepted as of December 2020. Instead, use WKWebView for improved security and reliability. Learn more (https://developer.apple.com/documentation/uikit/uiwebview).

  

2. Deprecated API Usage - New apps that use UIWebView are no longer accepted. Instead, use WKWebView for improved security and reliability. Learn more (https://developer.apple.com/documentation/uikit/uiwebview).

  <span style="color:green">解决方案:把工程里面的UIWebView 都改为 WKWebView</span>

------



- ITMS-90737: 

  Missing Info.plist value - A value for the Info.plist key 'CFBundleIconName' is missing in the bundle 'com.xxx.xxx'. Apps built with iOS 11 or later SDK must supply app icons in an asset catalog and must also provide a value for this Info.plist key. For more information see http://help.apple.com/xcode/mac/current/#/dev10510b1f7.

  <span style="color:green">解决方案:给应用配置Icon图即可</span>

------





