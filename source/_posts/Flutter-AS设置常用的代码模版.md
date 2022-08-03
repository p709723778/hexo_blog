---
title: Flutter ~ AS设置常用的代码模版
date: 2021-10-23 14:29:02
tags: [Dart, Flutter]
categories: [Flutter开发]
---

> 我们在日常开发中，使用 Android Studio 新建的 dart 文件，里面没有任何代码，类名称都是需要自己在手动写上去，文件名和类名称还不一样，dart文件名是以 `_` 来分割，类名称要遵循`大驼峰命名法`， 比较浪费时间，因此我们要使用代码模版来解决此问题。
>
> 比如：我们创建了一个 `hello_test.dart` 文件，dart代码内容 `class HelloTest { }` 



### 1. 打开 File and Code Templates

> Preferences > Editor > File and Code Templates

{% asset_img 1.png 示例图 %}

<!--more-->

### 2. 设置代码模版

在`File and Code Templates` 里，选择 `Files` 一栏，点击左上角加号，新建名字是 `Dart File For Name` ，扩展名为 dart 的代码模版。

{% asset_img 2.png 示例图 %}

然后在输入框中填入如下模版代码：

```dart
#set( $items = $NAME.split("_") )
#set( $class = "" )
#set( $item = "" )
#foreach($item in $items)
#set( $class = $class + $item.substring(0,1).toUpperCase()+$item.substring(1))
#end
class ${class}{}
```

然后点击右下角的Apply > OK就创建成功了。

### 3. 验证代码模版

> File > New > 模版

模版菜单图如下:

{% asset_img 3.png 示例图 %}

新建类名称图如下:

{% asset_img 4.png 示例图 %}

在打开的页面中会看到刚才创建的class，说明创建成功了。试一下生成代码的效果，如下所示。

```dart
class HelloTest {}
```

总结: 通过参照以上操作可以编写自己特定的代码模版。
