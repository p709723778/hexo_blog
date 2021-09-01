---
title: Flutter ~ Text 同时添加中划线和下划线
date: 2021-09-01 20:25:59
tags: [Dart, Flutter]
categories: [Flutter开发]
---

{% asset_img 1.png 示例图 %}

<!--more-->

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(title: 'Text Demo', home: ContainerDemo()),
  );
}

class ContainerDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('文本组件'),
      ),
      body: Center(
        child: Column(
          children: <Widget>[
            Text(
              "Hello world!",
              style: TextStyle(
                color: Colors.green,
                fontSize: 23.0,
                decoration: TextDecoration.combine(
                    [TextDecoration.underline, TextDecoration.lineThrough]),
              ),
            ),
            const Text(
              '红色+黑色删除线+25号',
              style: TextStyle(
                color: Color(0xffff0000),
                decoration: TextDecoration.lineThrough,
                decorationColor: Color(0xff000000),
                fontSize: 25.0,
              ),
            ),
            const Text(
              '橙色+下划线+24号',
              style: TextStyle(
                color: Color(0xffff9900),
                decoration: TextDecoration.underline,
                fontSize: 24.0,
              ),
            ),
            const Text(
              '虚线上划线+23号+倾斜',
              style: TextStyle(
                decoration: TextDecoration.overline,
                decorationStyle: TextDecorationStyle.dashed,
                fontSize: 23.0,
                fontStyle: FontStyle.italic,
              ),
            ),
            const Text(
              '24号+加粗',
              style: TextStyle(
                fontSize: 23.0,
                fontStyle: FontStyle.italic,
                fontWeight: FontWeight.bold,
                letterSpacing: 6.0,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

