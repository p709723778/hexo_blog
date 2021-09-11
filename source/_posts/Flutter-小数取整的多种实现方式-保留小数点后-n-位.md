---
title: Flutter ~ 小数取整的多种实现方式 & 保留小数点后 n 位
date: 2020-12-04 18:05:01
tags: [Dart, Flutter]
categories: [Flutter开发]
---

## 1. 舍弃小数部分（取整）

首先我们来看如何只保留整数位，这里有很多方法可以实现：

```dart
void main() {
  double price = 100 / 3;

  //原始值
  print("price = $price");

  //舍弃当前变量的小数部分，结果为 33。返回值为 int 类型。
  print("price.truncate() = ${price.truncate()}");

  //舍弃当前变量的小数部分，浮点数形式表示，结果为 33.0。返回值为 double。
  print("price.truncateToDouble() = ${price.truncateToDouble()}");

  //舍弃当前变量的小数部分，结果为 33。返回值为 int 类型。
  print("price.toInt() = ${price.toInt()}");

  //小数部分向上进位，结果为 34。返回值为 int 类型。
  print("price.ceil() = ${price.ceil()}");

  //小数部分向上进位，结果为 34.0。返回值为 double。
  print("price.ceilToDouble() = ${price.ceilToDouble()}");

  //当前变量四舍五入后取整，结果为 33。返回值为 int 类型。
  print("price.round() = ${price.round()}");

  //当前变量四舍五入后取整，结果为 33.0。返回值为 double 类型。
  print("price.roundToDouble() = ${price.roundToDouble()}");

  //取整: 忽略小数位,返回整数
  final int number = 100 ~/ 3;
  print("number = $number");
}
```

<!--more-->

根据自己的需求，是否需要四舍五入等，选择一个合适的方法即可。

## 2. 保留小数点后 n 位

如果我们想要控制浮点数的精度，想要保留小数点后几位数字，怎么实现？

最简单的是使用 `toStringAsFixed()` 方法：

```dart
void main() {
  double price = 100 / 3;

  //原始值
  print("price = $price");

  //保留小数点后2位数，并返回字符串：33.33。
  print("price.toStringAsFixed = ${price.toStringAsFixed(2)}");

  //保留小数点后5位数，并返回一个字符串 33.33333。
  print("price.toStringAsFixed = ${price.toStringAsFixed(5)}");
}
```

注意，`toStringAsFixed()` 方法会进行四舍五入。

或者也可以使用第三方类库，或者自己写一个函数实现都可以。当然，大多数情况下 `toStringAsFixed()` 方法都可以满足我们的需求了。
