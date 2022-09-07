---
title: Flutter ~ GZip解压缩
date: 2021-09-07 19:22:35
tags: [Dart, Flutter]
categories: [Flutter开发]
---

> GZip最早由Jean-loup Gailly和Mark Adler创建，用于UNⅨ系统的文件压缩。我们在Linux中经常会用到后缀为.gz的文件，它们就是GZip格式的。现今已经成为Internet 上使用非常普遍的一种数据压缩格式，或者说一种文件格式

废话不多说，直接上代码。

#### 代码运行示例:

```dart
void main() {
    print("准备 GZip压缩");

    //原始字符串
    const testString = '测试一下GZip压缩字符串';
    //GZip 压缩后的文本
    final zipString = GzipUtil.gzipEncode(testString);

    print("GZip 编码-$zipString");

    //GZip 解压
    final zipString2 = GzipUtil.gzipDecode(zipString);

    print("GZip 解码-$zipString2");
  
}
```

#### 工具类源码:

```dart
import 'dart:convert';
import 'dart:io';

class GzipUtil {
  ///GZIP 压缩
  static String? gzipEncode(String? str) {
    if (str == null) return str;
    //先来转换一下
    final stringBytes = utf8.encode(str);
    //然后使用 gzip 压缩
    final gzipBytes = GZipCodec().encode(stringBytes);
    //然后再编码一下进行网络传输
    final compressedString = base64UrlEncode(gzipBytes);
    return compressedString;
  }

  ///GZIP 解压缩
  static String? gzipDecode(String? str) {
    if (str == null) return str;
    //先来解码一下
    final List<int> stringBytes = base64Url.decode(str);
    //然后使用 gzip 压缩
    final gzipBytes = GZipCodec().decode(stringBytes);
    //然后再编码一下
    final compressedString = utf8.decode(gzipBytes);
    return compressedString;
  }
}
```

