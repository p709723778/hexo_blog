---
title: Flutter ~ 如何优雅地使用新版enum功能
date: 2022-12-12 11:11:37
tags: [Dart, Flutter]
categories: [Flutter开发]
---

> Dart 2.17 新增了一些enum的功能，一起来看看吧！
>
> 我也是看大佬 王叔不秃 学到的新姿势, 通过[ChatGPT](https://chat.openai.com/chat)询问，给到的答案也通过switch来写对应方法来转换枚举。🤓

```dart
void main() {
  final p = PortType.fromString('USB-C');
  print(p);
  print(p.isUSB);
}

enum PortType {
  usbA('USB-A', isUSB: true),
  usbC('USB-C', isUSB: true),
  lightning('LIGHTNING'),
  unknown('UNKNOWN');

  final String name;
  final bool isUSB;

  const PortType(this.name, {this.isUSB = false});

  static PortType fromString(String name) {
    return values.firstWhere((v) => v.name == name,
        orElse: () => PortType.unknown);
  }
}
```

注意事项:

`最后一个枚举项后面的 "," 需要改成 ";"   否则无法通过编译。`
