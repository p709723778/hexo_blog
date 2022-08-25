---
title: Flutter ~ ThemeData通过ThemeExtension扩展自定义属性
date: 2022-08-25 18:46:06
tags: [Dart, Flutter]
categories: [Flutter开发]
---

> ThemeExtensions 是在Flutter 3.0 中ThemeData类中新增的对象，通过它可以扩展自己想自定义的属性字段, Flutter 3.0之前需要编写一个扩展类来处理。
>
> Flutter 3.0之前可以参考 [在 flutter 中如何使用和扩展  ThemeData](https://juejin.cn/post/7065585593786302500) 这篇文章。



下面我们来介绍如果在Flutter 3.0中使用官方方法来扩展。



在ThemeData中 多了一个 [final Map<Object, ThemeExtension> extensions](https://github.com/flutter/flutter/blob/f1875d570e/packages/flutter/lib/src/material/theme_data.dart#L1127) 字段声明

要定义ThemeExtension扩展类，需要将一个或多个ThemeExtension子类传递给[ThemeData.new](https://api.flutter.dev/flutter/material/ThemeData/ThemeData.html) 或 [copyWith](https://api.flutter.dev/flutter/material/ThemeData/copyWith.html) 的extensions属性。

要获取扩展名，请使用[Theme.of(context).extension<MyColors>()](https://api.flutter.dev/flutter/material/ThemeData/extension.html). 

完整的Demo如下:

{% asset_img 1.gif 示例图 %}

<!--more-->

```dart
import 'package:flutter/material.dart';
import 'package:flutter/scheduler.dart';

@immutable
class MyColors extends ThemeExtension<MyColors> {
  const MyColors({
    required this.brandColor,
    required this.danger,
  });

  final Color? brandColor;
  final Color? danger;

  @override
  MyColors copyWith({Color? brandColor, Color? danger}) {
    return MyColors(
      brandColor: brandColor ?? this.brandColor,
      danger: danger ?? this.danger,
    );
  }

  @override
  MyColors lerp(ThemeExtension<MyColors>? other, double t) {
    if (other is! MyColors) {
      return this;
    }
    return MyColors(
      brandColor: Color.lerp(brandColor, other.brandColor, t),
      danger: Color.lerp(danger, other.danger, t),
    );
  }

  // Optional
  @override
  String toString() => 'MyColors(brandColor: $brandColor, danger: $danger)';
}

void main() {
  // Slow down time to see lerping.
  timeDilation = 5.0;
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({Key? key}) : super(key: key);

  static const String _title = 'Flutter Code Sample';

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  bool isLightTheme = true;

  void toggleTheme() {
    setState(() => isLightTheme = !isLightTheme);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: MyApp._title,
      theme: ThemeData.light().copyWith(
        extensions: <ThemeExtension<dynamic>>[
          const MyColors(
            brandColor: Color(0xFF1E88E5),
            danger: Color(0xFFE53935),
          ),
        ],
      ),
      darkTheme: ThemeData.dark().copyWith(
        extensions: <ThemeExtension<dynamic>>[
          const MyColors(
            brandColor: Color(0xFF90CAF9),
            danger: Color(0xFFEF9A9A),
          ),
        ],
      ),
      themeMode: isLightTheme ? ThemeMode.light : ThemeMode.dark,
      home: Home(
        isLightTheme: isLightTheme,
        toggleTheme: toggleTheme,
      ),
    );
  }
}

class Home extends StatelessWidget {
  const Home({
    Key? key,
    required this.isLightTheme,
    required this.toggleTheme,
  }) : super(key: key);

  final bool isLightTheme;
  final void Function() toggleTheme;

  @override
  Widget build(BuildContext context) {
    final MyColors myColors = Theme.of(context).extension<MyColors>()!;
    return Material(
      child: Center(
          child: Row(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Container(width: 100, height: 100, color: myColors.brandColor),
          const SizedBox(width: 10),
          Container(width: 100, height: 100, color: myColors.danger),
          const SizedBox(width: 50),
          IconButton(
            icon: Icon(isLightTheme ? Icons.nightlight : Icons.wb_sunny),
            onPressed: toggleTheme,
          ),
        ],
      )),
    );
  }
}
```

