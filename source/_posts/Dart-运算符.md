---
title: Dart ~ 运算符#
date: 2020-12-02 20:30:05
tags: [Dart]
categories: [Flutter开发]
---

## 1.Dart在线编辑器

- [Repl](https://repl.it/)
- [Dart官方在线编辑器](https://dart.dev/tutorials/web/get-started)

> Dart 运算符和绝大部分编程语言的运算符一样，所以你可以用熟悉的方式去执行程序代码运算。



## 2.运算符

**?. 运算符**

它的意思是左边如果为空返回 null，否则返回右边的值。

```dart
void main() {
  Animal animal = new Animal('cat');
  Animal empty = null;

  //animal 非空，返回 animal.name 的值 cat
  print(animal?.name);
  //empty 为空，返回 null
  print(empty?.name);

  //animal 非空，可以直接访问 animal.name 的值 cat
  print(animal.name);
  //empty 为空，抛出异常
  print(empty.name);
}

class Animal {
  final String name;
  Animal(this.name);
}
```



**?? 运算符**

如果 a 不为 null，返回 a 的值，否则返回 b。在 Java 或者 C++ 中，我们需要通过三元表达式 (a != null)? a : b 来实现这种情况。而在 Dart 中，这类代码可以简化为 a ?? b。

```dart
void main() {
  C c = new C('Case 1');
  B b = new B(c);
  A a = new A(b);

//   C c = new C(null);
//   B b = new B(c);
//   A a = new A(b);

//   C c = new C('Case 2');
//   B b = null;
//   A a = new A(b);

  //直接使用.来最终获取 c 的变量 value
  if (a != null && a.bMember != null && a.bMember.cMember != null) {
    print(a.bMember.cMember.value);
  } else {
    print(null);
  }

  //直接使用.来最终获取 c 的变量 value，为空时返回 unknown
  if (a != null && a.bMember != null && a.bMember.cMember != null) {
    String value = a.bMember.cMember.value;
    if (value == null) {
      value = 'unknown';
    }
    print(value);
  } else {
    print('unknown');
  }

  //dart 使用?.来最终获取 c 的变量 value
  print(a?.bMember?.cMember?.value);
  //dart 使用?.来最终获取 c 的变量 value，为空时使用 ?? 返回 unknown
  print(a?.bMember?.cMember?.value??'unknown');
}

class A {
  final B bMember;
  A(this.bMember);
}

class B {
  final C cMember;
  B(this.cMember);
}

class C {
  final String value;
  C(this.value);
}
```



**??= 运算符**

这种用默认值兜底的赋值语句在 Dart 中我们可以用 a ??= value 表示。如果 a 为 null，则给 a 赋值 value，否则跳过。

```dart
void main() {
  Animal animal = new Animal('cat');
  Animal empty = null;

  print(empty?.name);
  //empty 为空，则给empty赋值为animal
  empty ??= animal;
  print(empty.name);
}

class Animal {
  final String name;
  Animal(this.name);
}
```



? : 运算符 (三目运算)

bool-expr ? value1 : value2;
如果 bool-expr 为 true 就返回 value1, 如果 bool-expr 为 false 就返回 value2

```dart
void main() {
  bool isOpen = false;
	var value1 = "开";
	var value2 = "关";
  print(isOpen ? value1 : value2);
}
```

