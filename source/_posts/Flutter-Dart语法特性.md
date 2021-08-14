---
title: Flutter ~ Dart语法特性
date: 2020-11-12 11:23:55
tags: [Dart, Flutter]
categories: [Flutter开发]
---

## 一. Dart 的基本语法

### 1、程序入口

- Dart 的入口也是 main 函数，且没有返回值。
- 传递给 main 的命令行参数，会存放在 `List<String> args` 中。
- 定义字符串可以使用单引号或双引号。
- 每行语句必须使用分号结尾。

```dart
main(List<String> args) {
  print("Hello World");
}
```

### 2、声明变量

- 明确声明：变量类型 变量名称 = 赋值;

- 类型推导：`var / dynamic / const / final` 变量名称 = 赋值;

  <!--more-->

```dart
// 1. 明确的声明
String name = "lqr";

// 2. 类型推导(var/final/const)
// 类型推导的方式虽然没有明确的指定变量的类型，但是变量是有自己明确的类型的
// 2.1 var声明变量
var age = 18;
// age = "abc"; // IDE报错：A value of type 'String' can't be assigned to a variable of type 'int'.

// 2.2 final声明常量
final height = 1.88;
// height = 2.00; // IDE报错：The final variable 'height' can only be set once.

// 2.3 const声明常量
const address = "广州市";
// address = "北京市"; // IDE报错：Constant variables can't be assigned a value.

// 2.4 final和const的区别
// const必须赋值 常量值（编译期间需要有一个确定的值）
// final可以通过计算/函数获取一个值（运行期间来确定一个值）
// const date1 = DateTime.now(); // 写法错误
final date2 = DateTime.now();

// 2.5 Object和dynamic的区别
// 分别使用Object 和 dynamic，让父类引用指向子类对象
// Object调用方法时，编译时会报错
Object obj = "lqr";
print(obj.substring(1)); // IDE报错：The method 'substring' isn't defined for the type 'Object'.
// dynamic调用方法时，编译时不报错，但是运行时会存在安全隐患
// dynamic是明确声明（var是类型推导）
dynamic obj = "lqr";
print(obj.substring(1)); // qr
```

### 3、[Dart 没有]非零即真

js 中存在非零即真、非空即真的特性，但在 Dart 中没有！！Dart 要求 `if()` 语句必须传入一个 bool 类型：

```dart
// Dart中没有非零即真，也没有非空即真
var flag = "true";
if (flag) { // IDE报错：Conditions must have a static type of 'bool'.
  print("执行代码");
}

```

> 注意：Dart 不支持非空即真或者非 0 即真，必须有明确的 bool 类型

### 4、字符串类型

Dart 有三种定义字符串的方式：

- 单引号('')：与双引号相同。

- 双引号("")：与单引号相同。

- 三引号("""""")：可以定义多行字符串，不需要借助 `\n`。


  ```dart
  // 1. 定义字符串
  var str1 = 'abc';
  var str2 = "abc";
  var str3 = """
  abc
  cba
  nba
  """;
  
  // 小技巧：如果字符串中存在双引号，又不想转义的话，可以使用单引号来定义该字符串，反之使用双引号。
  print('a"b"c'); // a"b"c
  print("a'b'c"); // a'b'c
  
  ```

Dart 支持字符串插值：

```dart
// 2. 字符串和表达式拼接
var name = "lqr";
var age = 19;
var height = 1.88;
// 强调：${变量}可以省略{}，${表达式}不能省略{}
// var message1 = "my name is ${name}, age is ${age}, height is ${height}";
var message1 = "my name is $name, age is $age, height is $height"; // my name is lqr, age is 19, height is 1.88
var message2 = "name is $name, type is ${name.runtimeType}"; // name is lqr, type is String
print(message1);
print(message2);

```

### 5、集合类型

Dart 集合类型有：

- 列表 List：[ele1, ele2, ele2, ele1]
- 集合 Set：{ele1, ele2, ele3}
- 映射 Map：{"key1" : value1, "key2" : value2}

```dart
// 1. 列表List：[];
var names = ["abc", "cba", "nba", "cba"];
names.add("mba");
// names.remove(value);
// names.removeAt(index);

// 2. 集合Set：{};
var movies = {"星际穿越", "大话西游", "盗梦空间"};
names = Set<String>.from(names).toList(); // 可以用Set来去重
print(names);

// 3. 映射Map
var info = {
  "name": "lqr",
  "age": 18,
};

```

## 二、Dart 的函数使用

### 1、函数的基本使用

Dart 的函数定义：

```Plain
返回值 函数的名称(参数列表) {
  函数体
  return 返回值
}

```

例子：

```dart
main(List<String> args) {
  print(sum(20, 30));
}

int sum(int num1, int num2) {
  return num1 + num2;
}

// 返回值的类型可以省略（开发中不推荐）
// sum(int num1, int num2) {
//   return num1 + num2;
// }

```

### 2、函数的可选参数

Dart 的函数参数有两类：

- 必选参数：必须传
- 可选参数：可不传

```dart
main(List<String> args) {
  sayHello1("lqr");
}

// 必选参数：必须传
void sayHello1(String name) {
  print(name);
}

```

- 可选参数有 2 种：
  - 位置可选参数：[int age, double height]
  - 命名可选参数：{int age, double height}

```dart
main(List<String> args) {
  sayHello2("lqr");
  sayHello2("lqr", 18, 1.88);

  sayHello3("lqr", age: 18, height: 1.88);
  sayHello3("lqr", height: 1.88, age: 18);
  sayHello3("lqr", height: 1.88);
}

// 位置可选参数：[int age, double height]
// 实参和形参在进行匹配时，是根据位置来匹配
void sayHello2(String name, [int age, double height]) {}

// 命名可选参数：{int age, double height}
// 实参和形参在进行匹配时，是根据变量名来匹配
void sayHello3(String name, {int age, double height}) {}

```

### 3、函数的默认参数

Dart 中没有函数重载!!!Dart 中没有函数重载!!!Dart 中没有函数重载!!!但是，Dart 函数支持为可选参数设置默认值：

```dart
void sayHello1(String name) {
  print(name);
}
// Dart中没有函数的重载
// void sayHello1(){ // IDE报错：The name 'sayHello1' is already defined.
//   ...
// }

// 注意：只有可选参数才可以有默认值，必传参数没有默认值
void sayHello4(String name, [int age = 20, double height = 1.7]) {}
void sayHello5(String name, {int age = 20, double height = 1.7}) {}

```

> 注意：只有可选参数才能设置默认值。

### 4、函数是一等公民

`函数是一等公民` 意味着可以将函数赋值给一个变量，也可以将函数作为另外一个函数的参数或返回值来使用。

```dart
main(List<String> args) {
  // 1. 直接找到另外一个定义的函数传进去
  test(bar); // bar函数被调用

  // 2. 匿名函数 (参数列表){函数体};
  test(() {
    print("匿名函数被调用"); // 匿名函数被调用
    return 10;
  });

  // 3. 箭头函数：条件，函数体只有一行代码
  // 注意：Dart中的箭头函数与js中的箭头函数是两回事
  test(() => print("箭头函数被调用")); // 箭头函数被调用
}

// 函数可以作为另外一个函数的参数
void test(Function foo) {
  var result = foo();
}

void bar() {
  print("bar函数被调用");
}

```

上述代码中，当将函数作为另一个函数的参数时，使用 `Function` 来声明这个参数类型，很明显有个问题，那就是无法对传入的函数做限制：

```dart
main(List<String> args) {
  test((name) {
    print(name); // lqr
  });
}

// 封装test函数，要求：传入一个函数
// 缺点：Function无法对传入函数做限制（比如：函数的参数列表、返回值）
void test(Function foo) {
  foo("lqr");
}

```

这时，可以使用 `函数签名` 来代替 `Function`：

```dart
main(List<String> args) {
  test((num1, num2) {
    return num1 + num2;
  });
}

// 可以限制 作为参数的函数的类型
// 缺点：相当于在形参位置上写了一个函数声明（函数签名），整体看起来可读性差
void test(int foo(int num1, int num2)) {
  foo(20, 30);
}

```

为了有更好的可读性，可以使用 `typedef` 来代替 `函数签名`：

```dart
main(List<String> args) {
  test((num1, num2) {
    return num1 + num2;
  });

  var demo1 = demo();
  print(demo1(20, 30)); // 600
}

// 可以使用typedef定义一个函数类型
typedef Calculate = int Function(int num1, int num2);

void test(Calculate calc) {
  calc(20, 30);
}

// 函数作为返回值
Calculate demo() {
  return (num1, num2) {
    return num1 * num2;
  };
}

```

## 三、Dart 的特殊运算符

### 1、运算符

- `??=`：赋值运算符（变量会 null 时才执行赋值操作）
- `??`：条件运算符（有点像简化版的三元运算符）

```dart
// 1. ??=
// 当原来的变量有值时，那么 ??= 不执行
// 原来的变量为null时，那么将值赋值给这个变量
var name = null;
name ??= "lilei";
print(name); // lilei

// 2. ??
// ??前面的数据有值，那么就使用??前面的数据
// ??前面的数据为null，那么就使用后面的值
var name = null;
var temp = name ?? "lilei";
print(temp); // lilei

```

### 2、级联语法(..)

Dart 可以使用级联语法 `(..)` 将任意对象的 `变量赋值` 或 `方法调用` 操作都变成 `链式调用`：

```dart
main(List<String> args) {
  // var p = Person();
  // p.name = "lqr";
  // p.run();
  // p.eat();

  // 级联运算符
  var p = Person()
    ..name = "lqr"
    ..eat()
    ..run();
}

```

### 3、for 循环的使用

Dart 支持 `普通 for 循环`，以及 `for-in 循环`：

```dart
// 1. 基础for循环
for (var i = 0; i < 10; i++) {
  print(i);
}

// 2. 遍历数组
var names = ["why", "cba", "cba"];
for (var i = 0; i < names.length; i++) {
  print(names[i]);
}

for (var name in names) {
  print(name);
}

```

> while 和 do-while 和其他语言一致，break 和 continue 用法也是一致

## 四、Dart 的面向对象

### 1、类的定义

Dart 中使用 `class` 关键字来定义类：

```dart
main(List<String> args) {
  var p1 = new Person("lqr", 18);
  // 从Dart2开始，new关键字可以省略
  var p2 = Person("lqr", 18);
}

class Person {
  String name;
  int age;

  // Person(String name, int age) {
  //   this.name = name;
  //   this.age = age;
  // }

  // 等同于上面的代码
  Person(this.name, this.age);
}

```

### 2、类的构造函数

Dart 不支持函数重载，所以，构造函数也一样不能重载。不过，可以使用 `命名构造函数` 来解决：

```dart
main(List<String> args) {
  var p = Person("lqr", 18);
  var p1 = new Person.withNameAgeHeight("lqr", 18, 1.88);
  var p2 = Person.fromMap({
    "name": "lqr",
    "age": 18,
    "height": 1.88,
  });
}

class Person {
  String name;
  int age;
  double height;

  Person(this.name, this.age);

  // Dart中没有函数重载
  // Person(this.name, this.age, this.height); // IDE报错：The default constructor is already defined.

  // 命名构造函数
  Person.withNameAgeHeight(this.name, this.age, this.height);

  Person.fromMap(Map<String, dynamic> map) {
    this.name = map['name'];
    this.age = map['age'];
    this.height = map['height'];
  }

  @override
  String toString() {
    return "$name $age $height";
  }
}

```

### 3、类的初始化列表

如果类中的属性使用 final 修饰，那么在必须在构造函数中对 final 属性进行赋值，但要注意的是，不能在构造函数的函数体内对 final 属性赋值：

```dart
class Person {
  final String name;
  final int age;

  // Person(this.name, this.age) {} // 必传参数
  Person(this.name, {this.age = 10}) {} // （带默认值的）可选参数

  // Person(this.name) { // IDE报错：All final variables must be initialized, but 'age' isn't.
  //   this.age = 10; // IDE报错：'age' can't be used as a setter because it's final.
  // }
}

```

虽然 Dart 不支持在构造函数函数体中对 final 属性赋值，但是可以用 `初始化列表` 对其赋值：

> 初始化列表一般与命名可选参数配合使用，当外界调用构造函数时，可以对形参对应的属性进行校验，以及设置初始值。

```dart
class Person {
  final String name;
  final int age;

  // function ():xxxxxx {} ，其中xxxxxx部分就是初始化列表
  Person(this.name) : this.age = 10 {} // 初始化列表：可能使用 表达式 来为属性赋值
  // Person(this.name, {this.age = 10}) {} // （带默认值的）可选参数：只能使用 赋值语句 来为属性赋值
  // Person(this.name, {int age}) : this.age = age ?? 10 {} // 初始化列表：可以对形参进行校验，以及设置初始化值

  // 初始化列表有多个时，用逗号隔开
  // Person(this.name, {int age, double height}) : this.age = age ?? 10, this.height = height ?? 1.88 {}
}

```

### 4、重定向构造函数

重定向构造函数，其实就是在一个构造函数中，去调用另一个构造函数：

```dart
class Person {
  String name;
  int age;

  // 构造函数的重定向
  Person(String name) : this._internal(name, 0);

  Person._internal(this.name, this.age);
}

```

### 5、常量构造函数

在某些情况下，我们希望 `传入相同的值` 能 `返回同一个对象`，这时，可以使用常量构造函数：

- 常量构造函数：

  - 在构造函数前加 `const` 进行修改。

  - 拥有常量构造函数的类中，所有的成员变量必须使用 `final` 修饰。

  - 为了可以通过常量构造函数创建出相同的对象，不再使用 new 关键字，而是使用 

    ```
    const
    ```

     关键字。

    - 如果是将结果赋值给 `const` 修饰的标识符时，`const` 可以省略。

```dart
main(List<String> args) {
  // 用 const常量 去接收一个常量构造函数的结果，可以省略 const
  // const p1 = const Person("lqr", 18);
  const p1 = Person("lqr", 18);
  const p2 = Person("lqr", 18);
  print(identical(p1, p2)); // true

  // 如果用 var变量 去接收一个常量构造函数的结果，则这时省略的不是 const，而是 new！！
  var p11 = Person("lqr", 18); // ==> var p11 = new Person("lqr");
  var p22 = Person("lqr", 18);
  print(identical(p11, p22)); // false

  // 必须所有的属性值相同，对象才是同一个
  const p111 = Person("lqr", 18);
  const p222 = Person("lqr", 20);
  print(identical(p111, p222)); // false
}

class Person {
  // 拥有常量构造方法的类中，所有的成员变量必须是final修饰
  final String name;
  final int age;

  // 一个类中只能有一个常量构造方法，命名构造方法也不行
  // const Person(this.name);
  const Person(this.name, this.age);
}

```

### 6、工厂构造函数

为了满足 `传入相同的值，得到相同的对象` 这个需求，除了可以使用常量构造函数外，还可以使用工厂构造函数，而且工厂构造函数更加灵活。

- 工厂构造函数：在构造函数前加 `factory` 关键字进行修改。
- 工厂构造函数与普通的构造函数的区别：
  - 普通的构造函数：会自动返回创建出来的对象，不能手动返回
  - 工厂构造函数最大的特点：可以手动的返回一个对象

```dart
main(List<String> args) {
  final p1 = Person.withName("lqr");
  final p2 = Person.withName("lqr");
  print(identical(p1, p2));
}

class Person {
  String name;
  String color;

  // static 修饰的属性是类属性，可以通过类名直接调用
  static final Map<String, Person> _nameCache = {};

  Person(this.name, this.color);

  // 普通的构造函数：会自动返回创建出来的对象，不能手动返回
  // 工厂构造函数最大的特点：可以手动的返回一个对象
  factory Person.withName(String name) {
    if (_nameCache.containsKey(name)) {
      return _nameCache[name];
    } else {
      final p = Person(name, "default");
      _nameCache[name] = p;
      return p;
    }
  }
}

```

### 7、类的 setter 和 getter

很多时候，我们并不希望外部可以直接访问对象字段，而是经过 `setter` 和 `getter` 中转，这样的好处是，可以在 `setter` 和 `getter` 中做一些额外的工作，Dart 支持为字段增加 `setter` 和 `getter`：

- `setter` ：前面使用 `set` 声明，并且需要用 `()` 接收参数。
- `getter` ：前面使用 `get` 声明，还需要声明返回值类型，但是不能有 `()` 接收参数。
- `setter` 和 `getter` 像字段一样使用。

```dart
main(List<String> args) {
  final p1 = Person();

  // p1.setName("lqr"); // IDE报错：The method 'setName' isn't defined for the type 'Person'.

  // setter 和 getter 像字段一样使用
  p1.setName = "lqr";
  print(p1.getName); // lqr
}

class Person {
  String name;

  // setter
  // set setName(String name) {
  //   this.name = name;
  // }
  set setName(String name) => this.name = name;

  // getter：注意getter没有()
  // String get getName {
  //   return this.name;
  // }
  String get getName => this.name;
}

```

> 严格意义上来说，Dart 中的 `setter` 和 `getter` 已经不能算是方法了吧。

### 8、类的继承

Dart 中的继承使用 `extends` 关键字，子类中使用 `super` 来访问父类。

> 注意：Dart 只支持单继承。

```dart
main(List<String> args) {}

class Animal {
  int age;

  Animal(this.age);

  void run() {
    print("向前跑~");
  }
}

class Person extends Animal {
  String name;

  // 必须在初始化列表中，调用父类的构造函数
  Person(this.name, int age) : super(age);

  @override
  void run() {
    // super.run();
    print("向前走～");
  }
}

// Dart只支持单继承
// class SuperMan extends Runner, Flyer{ // IDE报错：Each class definition can hava at most one extends clause.
// }

```

### 9、抽象类的使用

Dart 可以使用 `abstract` 关键字来定义抽象类：

- 抽象类中的 `抽象方法不需要使用 abstract 关键字修饰`
- 抽象类不能实例化

```dart
main(List<String> args) {
  final s = Shape(); // IDE报错：Abstract classes can't be instantiated.
}

// 注意2：抽象类不能实例化
abstract class Shape {
  // 抽象方法，没有方法体，也不用 abstract 关键字修饰
  int getArea();

  String getInfo() {
    return "形状";
  }
}

// 注意1：继承自抽象类后，必须实现抽象类的抽象方法
class Rectangle extends Shape {
  @override
  int getArea() {
    return 100;
  }
}

```

> 注意：Dart 中抽象类不能实例化，但也有特殊情况，有工厂构造函数(factory)的抽象类可以实例化，比如：Map。

### 10、隐式接口

Dart 使用 `implements` 关键字来实现多个接口，但奇葩的是，Dart 中没有用来定义接口的关键字，默认情况下所有的类都是隐式接口，即可以 `implements` 任意类：

> 注意：要重写被 `implements` 的类中的所有方法！

```dart
main(List<String> args) {}

// Dart中没有哪一个关键字是来定义接口的
// 没有这些关键字interface/protocol
// 默认情况下所有的类都是隐式接口
class Runner {
  void running() {
    print("跑吧");
  }
}

class Flyer {
  void flying() {
    print("飞吧");
  }
}

// 当将一个类当做接口使用时，那么实现这个接口的类，必须实现这个接口中所有方法（不可以在这些方法里使用super）
class SuperMan implements Runner, Flyer {
  @override
  void flying() {
    // super.flying(); // IDE报错：The method 'flying' is always abstract in the supertype.
  }

  @override
  void running() {}
}

```

> 建议：用 `abstract class` 来声明接口，反正被 `implements` 的类的所有方法都要重写。

### 11、mixin 混入的使用

当遇到要复用 2 个类中方法的时候，要怎么办？

- 如果使用接口方式则需要重新实现 2 个类中各自的方法，这样根本就不能算是复用；
- 而 Dart 又只能单继承，最多只能复用 1 个类中方法；

Dart 提供了另一个方案：`Mixin 混入`。可以定义 2 个 mixin 类，通过 `with` 关键字把这 2 个 mixin 类混入到一个类 A 中，这时类 A 就拥有了它们的方法，从而达到复用 2 个类中方法的目的：

```dart
main(List<String> args) {
  final superMan = SuperMan();
  superMan.eating(); // 动作吃东西
  superMan.running(); // running（调用的是混入类Runner中的running()）
  superMan.flying(); // flying
}

class Animal {
  void eating() {
    print("动作吃东西");
  }

  void running() {
    print("动物running");
  }
}

mixin Runner {
  void running() {
    print("running");
  }
}

mixin Flyer {
  void flying() {
    print("flying");
  }
}

// Runner中的running()与Animal中的running()冲突，这时会发生覆盖，即SuperMan中的running()调用的是Runner的running()
class SuperMan extends Animal with Runner, Flyer {}

```

### 12、类属性和类方法

类中使用 `static` 关键字修饰的属性(或方法)就是类属性(或类方法)，类属性和类方法可以通过类名直接调用：

```dart
main(List<String> args) {
  Person.courseTime = "8:00";
  print(Person.courseTime);
  Person.gotoCourse();
}

class Person {
  // 成员变量
  String name;

  // 静态属性（类属性）
  static String courseTime;

  // 对象方法
  void eating() {
    print("eating");
  }

  // 静态方法（类方法）
  static void gotoCourse() {
    print("去上课");
  }
}

```

### 13、枚举的使用

枚举使用 `enum` 关键字来定义，枚举有 2 个常见的属性：

- index：用于表示每个枚举的索引，从 0 开始
- values：包含每个枚举值的集合

```dart
// 枚举：enum
main(List<String> args) {
  final color = Colors.red;
  switch (color) {
    case Colors.red:
      print("红色");
      break;
    case Colors.blue:
      print("蓝色");
      break;
    case Colors.green:
      print("绿色");
      break;
  }

  print(Colors.values); // [Colors.red, Colors.blue, Colors.green]
  print(Colors.red.index); // 0
}

enum Colors {
  red,
  blue,
  green,
}

```

### 14、泛型的使用

集合类型使用泛型：

```dart
// List使用时的泛型写法：
var names1 = ["abc", "cba", "nba", 111]; // List<Object>
var names2 = ["abc", "cba", "nba"]; // List<String>
// var names3 = <String>["abc", "cba", "nba", 111]; // IDE报错：The element type 'int' can't be assigned to the list type 'String'.
var names3 = <String>["abc", "cba", "nba"]; // List<String>
List<String> names4 = ["abc", "cba", "nba"]; // List<String>

// Map使用时的泛型写法：
var info1 = {"name": "lqr", "age": 18}; // _InternalLinkedHashMap<String, Object>
var info2 = <String, String>{'name': 'lqr', 'age': '18'}; // _InternalLinkedHashMap<String, String>
Map<String, String> info3 = {'name': 'lqr', 'age': '18'}; // _InternalLinkedHashMap<String, String>

```

类定义使用泛型：

```dart
main(List<String> args) {
  var point = Point<double>(1.23333, 1.9527); // Point<double>
  final pointX = point.setAndGetX(5.55);
  print(pointX); // 5.55
}

// class Point<T> // 泛型T是Object的子类
class Point<T extends num> { // 泛型T是数字类型
  T x;
  T y;

  Point(this.x, this.y);

  T setAndGetX(T x) {
    this.x = x;
    return this.x;
  }
}

```

> `T extends num` 用于限制泛型的类型必须是数字，不限制的话则默认是 Object。

## 五、Dart 中库的使用

- Dart 中你可以导入一个库来使用它所提供的功能。
- Dart 中任何一个 dart 文件都是一个库，即使你没有用关键字 `library` 声明。

库导入的语法：

```dart
import '库所在的 uri';

```

### 1、使用系统的库

系统库的 uri 一般以 `dart:` 开头，常见的系统库有：`dart:io`、`dart:async`、`dart:math` 等：

```dart
// import 'dart:io';
// import 'dart:isolate';
// import 'dart:async';
// import 'dart:math';

// 系统的库：import 'dart:库的名字';

import 'dart:math';

main(List<String> args) {
  final num1 = 20;
  final num2 = 30;
  print(min(num1, num2));
}

```

> 注意：`dart:core` 也是很常用的系统库，不过可以省略不写。

### 2、使用自定义库

自定义库就是自己项目中定义的其他 dart 文件，比如在 utils 目录下创建一个 `math_utils.dart` 文件，内容如下：

```dart
// utils 目录下的 math_utils.dart 文件内容：
int sum(int num1, int num2) {
  return num1 + num2;
}

int mul(int num1, int num2) {
  return num1 * num2;
}

```

这里的 `math_utils.dart` 文件就是一个自定义库，你可以在其他 dart 文件中导入：

```dart
// 导入自定义库可以是相对路径或绝对路径。
import 'utils/math_utils.dart';

main(List<String> args) {
  print(sum(20, 30));

```

### 3、使用第三方库

那些存放在远程仓库上的库文件就是第三方库，如果项目中需要依赖第三方库，就需要在项目目录下创建一个 `pubspec.yaml` 文件，其中 `dependencies` 部分填写要依赖的第三方库的描述（库名: 版本）：

```yaml
name: myProjectName
description: my project name
dependencies:
  http: ^0.12.0+4
environment:
  sdk: ">=2.10.0 <3.0.0"

```

> 可以在 [pub.dev](https://link.juejin.cn?target=https%3A%2F%2Fpub.dev%2F) 上查找第三方库的使用说明。

填写好依赖后，需要执行 `dart pub get` 或 `flutter pub get` 命令，让终端从服务器上拉取第三方对应的版本文件，之后就可以在代码中使用这个第三方库了：

```dart
import 'package:http/http.dart' as http;

main(List<String> args) async {
  var url = 'https://example.com/whatsit/create';
  var response = await http.post(url, body: {'name': 'doodle', 'color': 'blue'});
  print('Response status: ${response.statusCode}');
  print('Response body: ${response.body}');
  print(await http.read('https://example.com/foobar.txt'));
}

```

### 4、库方法名冲突

比如：`math_utils.dart` 库中有 `int sum(int num1, int num2)` 方法，当前 dart 文件中又定义了 `void sum(int num1, int num2)` 方法，这 2 个方法名相同，但返回值不同：

```dart
import 'utils/math_utils.dart';

main(List<String> args) {
  print(sum(20, 30)); // IDE报错：This experssion has a type of 'void' so its value can't be used.
}

// void sum(num1, num2) // 参数类型可以不写，默认是 dynamic
void sum(int num1, int num2) {
  print(num1 + num2);
}

```

这时，当前 dart 文件识别到的是自己的 sum()方法，而实际上我们想要使用的是 `math_utils.dart` 库中 sum()方法，这就发生了库方法名冲突，可以使用 `as` 关键字给库起别名来解决：

```dart
import 'utils/math_utils.dart' as mUtils;

main(List<String> args) {
  print(mUtils.sum(20, 30));
}

void sum(sum1, sum2) {
  print(sum1 + sum2);
}

```

### 5、库内容的显示和隐藏

默认 import 进来的库中所有内容都是可见的，但是很多时候我们希望只导入库中的某些内容，或者希望隐藏库中的某些内容，这时可以使用 `show` 和 `hide` 关键字：

- show：显示某个成员（ 屏蔽其他）

```dart
// import "utils/math_utils.dart" show sum, mul;
import "utils/math_utils.dart" show sum;

main(List<String> args) {
  print(sum(20, 30));
  print(mul(20, 30)); // IDE报错：The function 'mul' isn't defined.
}

```

- hide：隐藏某个成员（显示其他）

```dart
// import "utils/math_utils.dart" hide sum, mul;
import "utils/math_utils.dart" hide sum;

main(List<String> args) {
  print(sum(20, 30)); // IDE报错：The function 'sum' isn't defined.
  print(mul(20, 30));
}

```

### 6、批量导入库文件

有些时候，可能需要在一个 dart 文件中，导入 n 个库，这还是能接受的：

```dart
import 'utils/math_utils.dart';
import 'utils/date_utils.dart';
import ...

main(List<String> args) {
  print(sum(20, 30));
  print(dateFormat());
}

```

但如果是 n 个 dart 文件都要导入相同的 m 个库呢？很明显，需要有个统一批量导入库文件的方案：可以创建一个新的 dart 文件，使用 `export` 关键字统一导库。

```dart
// utils 目录下的 utils.dart 文件内容：
export 'math_utils.dart';
export 'date_utils.dart';
export ...

```

这时，其他 dart 文件中，只需要导入这个 utils.dart 就好了：

```dart
import 'utils/utils.dart';

main(List<String> args) {
  print(sum(20, 30));
  print(dateFormat());

  // math_utils.dart中的_min()无法被外界调用
  // print(_min(20, 30)); // IDE报错：The function '_min' isn't defined.
}

```

> 注意：Dart 语言中没有可见性修饰符，即没有 public、protected、private。如果某个成员属性或方法不想被外界（其他 dart 文件）调用，可以在名字前加个下划线 `_`。

## 六、Dart Extension 扩展类功能

Dart 从 2.7 版本开始加入了 Extension 语法，可以给已经存在的类扩展成员方法。扩展类方法需要使用到 `extension` 关键字，语法为：

```dart
extension XXX on YYY{}

```

表示给 YYY 类进行扩展，扩展名是 XXX（名字可以随便写）。

### 1、扩展成员方法

如果你想给 String 扩展一个成员方法的话，可以这么写：

```dart
extension StringExt on String {
  String sayHello() {
    return "hello $this";
  }
}

main(List<String> args) {
  final name = "GitLqr";
  print(name.sayHello()); // hello GitLqr
}

```

### 2、扩展操作符

Extension 也可以扩展操作符, 如：

```dart
extension StringExt on String {
  operator *(int count) {
    String result = "";
    for (var i = 0; i < count; i++) {
      result += this;
    }
    return result;
  }
}

main(List<String> args) {
  final name = "GitLqr";
  print(name * 3); // GitLqrGitLqrGitLqr
}

```

### 3、扩展成员"属性"

Extension 除了可以扩展方法(method)、操作符(operator)，还可以扩展 getter、setter，但无法扩展实例属性（或者叫字段）。不过，因为 getter 在调用时跟调用属性的语法很像，所以这里的"属性"是带引号的：

```dart
extension StringExt on String {
  int get size {
    return this.length;
  }
}

main(List<String> args) {
  final name = "GitLqr";
  print(name.size); // 6
}

```

setter 同理，这里不多举例，更多细节，可通过官方文档查看：https://dart.dev/guides/language/extension-methods
