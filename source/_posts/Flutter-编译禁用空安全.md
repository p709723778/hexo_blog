---
title: Flutter ~ 编译禁用空安全
date: 2021-11-04 11:26:38
tags: [Dart, Flutter]
categories: [Flutter开发]
---

> 因为把pubspec.yaml文件environment的SDK版本进行了升级，导致一些第三方库报错不支持安全模式.



```dart
Waiting for connection from debug service on Chrome...
Error: Cannot run with sound null safety, because the following dependencies
don't support null safety:

 - package:xxxx
```



### Android 原生禁用空安全

在flutter工程中找到android目录下`gradle.properties`文件,添加 `extra-front-end-options=--no-sound-null-safety`



### iOS 原生禁用空安全

在 `Build Settings` => `User-Defined`  添加 键 `EXTRA_FRONT_END_OPTIONS` ，值 `--no-sound-null-safety`

[参考stackoverflow](https://stackoverflow.com/questions/67304114/how-to-add-flutter-run-time-arguments-to-xcode)
<!--more-->


### Flutter层禁用空安全

##### 1. 命令禁用

```dart
/// 运行
flutter run --no-sound-null-safety

/// 打包
flutter build apk --no-sound-null-safety --release
```

但是我们常用的是使用编辑器启动的，而且使用命令启动不好操作断点，所以我们还是在代码或编辑器里面配置。

##### 2. 使用的是Android Studio配置如下

{% asset_img 1.webp 示例图 %}

{% asset_img 2.webp 示例图 %}

##### 3. 使用的是VSCode配置如下

{% asset_img 3.webp 示例图 %}