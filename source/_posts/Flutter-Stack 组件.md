---
title: Flutter ~ Stack 组件
date: 2021-01-04 14:33:49
tags: [Dart, Flutter]
categories: [Flutter开发]
---

层叠布局和Web中的绝对定位、Android中的Frame布局是相似的，子组件可以根据距父容器四个角的位置来确定自身的位置。绝对定位允许子组件堆叠起来（按照代码中声明的顺序）。Flutter中使用`Stack`和`Positioned`这两个组件来配合实现绝对定位。`Stack`允许子组件堆叠，而`Positioned`用于根据`Stack`的四个角来确定子组件的位置。

```dart
/// Creates a stack layout widget.
///
/// By default, the non-positioned children of the stack are aligned by their
/// top left corners.
Stack({
  Key? key,
  this.alignment = AlignmentDirectional.topStart,
  this.textDirection,
  this.fit = StackFit.loose,
  @Deprecated(
    'Use clipBehavior instead. See the migration guide in flutter.dev/go/clip-behavior. '
    'This feature was deprecated after v1.22.0-12.0.pre.'
  )
  this.overflow = Overflow.clip,
  this.clipBehavior = Clip.hardEdge,
  List<Widget> children = const <Widget>[],
})
```

- `alignment`：此参数决定如何去对齐没有定位（没有使用`Positioned`）或部分定位的子组件。所谓部分定位，在这里**特指没有在某一个轴上定位：**`left`、`right`为横轴，`top`、`bottom`为纵轴，只要包含某个轴上的一个定位属性就算在该轴上有定位。

- `textDirection`：和`Row`、`Wrap`的`textDirection`功能一样，都用于确定`alignment`对齐的参考系，即：`textDirection`的值为`TextDirection.ltr`，则`alignment`的`start`代表左，`end`代表右，即`从左往右`的顺序；`textDirection`的值为`TextDirection.rtl`，则alignment的`start`代表右，`end`代表左，即`从右往左`的顺序。

- `fit`：此参数用于确定**没有定位**的子组件如何去适应`Stack`的大小。`StackFit.loose`表示使用子组件的大小，`StackFit.expand`表示扩伸到`Stack`的大小。

- `overflow`：此属性已经废弃,改换为clipBehavior。

- `clipBehavior`：此属性决定裁剪方式。

|          **clipBehavior属性值**           | **说明**                                                     |
| :---------------------------------------: | :----------------------------------------------------------- |
|          clipBehavior: Clip.none          | 不裁剪                                                       |
|        clipBehavior: Clip.hardEdge        | 裁剪但不应用抗锯齿，裁剪速度比none模式慢一点，但比其他方式快。 |
|       clipBehavior: Clip.antiAlias        | 裁剪而且抗锯齿，以实现更平滑的外观。裁剪速度比antiAliasWithSaveLayer快，比hardEdge慢。 |
| clipBehavior: Clip.antiAliasWithSaveLayer | 带有抗锯齿的剪辑，并在剪辑之后立即保存saveLayer。            |

### 初探Stack组件的使用

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(title: 'Stack Demo', home: StackDemo()),
  );
}

class StackDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Stack 组件'),
      ),
      body: Stack(
        children: <Widget>[
          Container(
            width: 100,
            height: 100,
            color: Colors.red,
          ),
          Container(
            width: 90,
            height: 90,
            color: Colors.blue,
          ),
          Container(
            width: 80,
            height: 80,
            color: Colors.green,
          ),
        ],
      ),
    );
  }
}
```

上面的代码Stack做为根布局，叠加的方式展示3个组件，第一个组件比较大100*100，第二个组件稍微小点90***90

，第三个组件最小80*80，显示的方式是能看见第一个和第二个组件的部分区域，第三个组件是能全部显示出来

{% asset_img 1.png 示例图 %}

<!--more-->

### fit 属性使用

如果指定是StackFit.expand，所以的子组件会和Stack一样大的

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(title: 'Stack Demo', home: StackScreen()),
  );
}

class StackScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Stack title"),
      ),
      body: Stack(
        fit: StackFit.expand,
        children: <Widget>[
          Container(
            width: 100,
            height: 100,
            color: Colors.red,
          ),
          Container(
            width: 90,
            height: 90,
            color: Colors.blue,
          ),
          Container(
            width: 80,
            height: 80,
            color: Colors.green,
          ),
        ],
      ),
    );
  }
}
```

显示内容就只最后一个组件，虽然我们给这个组件指定了一个80*80的宽高是不会 生效的，因为我们已经指定了子元素和Stack一样大小，也就是说设置了StackFit.expand，StackFit.expand的效果优先

{% asset_img 2.png 示例图 %}

### Positioned

这个使用控制Widget的位置，通过他可以随意摆放一个组件，有点像绝对布局

```dart
  const Positioned({
    Key? key,
    this.left,
    this.top,
    this.right,
    this.bottom,
    this.width,
    this.height,
    required Widget child,
  })
```

left、top 、right、 bottom分别代表离Stack左、上、右、底四边的距离

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(title: 'Stack Demo', home: PositionScreen()),
  );
}

class PositionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Postion Title"),
      ),
      body: Stack(
        children: <Widget>[
          Positioned(
            top: 100.0,
            child: Container(
              color: Colors.blue,
              child: const Text("第一个组件"),
            ),
          ),
          Positioned(
            top: 200,
            right: 100,
            child: Container(
              color: Colors.yellow,
              child: const Text("第二个组件"),
            ),
          ),
          Positioned(
            left: 100.0,
            child: Container(
              color: Colors.red,
              child: const Text("第三个组件"),
            ),
          ),
        ],
      ),
    );
  }
}
```

这个例子的效果就是

- 第一个组件距离顶部Stack 有100的间距
- 第二个组件距离顶部200，距离右边100间距
- 第三个组件距离左边100间距

{% asset_img 3.png 示例图 %}
