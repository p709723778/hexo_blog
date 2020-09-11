---
title: iOS开发之 ~ 循环引用
date: 2016-12-11 11:32:51
tags: [iOS]
categories: [iOS开发]
---



## 1. 概述

> iOS内存中的分区有：堆、栈、静态区。其中，栈和静态区是操作系统自己管理回收，不会造成循环引用。在堆中的相互引用无法回收，有可能造成循环引用。



- 循环引用的实质：多个对象相互之间有强引用，不能施放让系统回收。

- 解决循环引用一般是将 strong 引用改为 weak 引用。

<!--more-->

## 2. 循环引用场景分析及解决方案



### 2.1 父类与子类

在使用UITableView 的时候，将 UITableView 给 Cell 使用，cell 中的 strong 引用会造成循环引用。

```objc
// Controller中的代码
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    TestTableViewCell *cell =[tableView dequeueReusableCellWithIdentifier:@"UITableViewCellId" forIndexPath:indexPath];
    cell.tableView = tableView;
    return cell;
}

// 自定义Cell中的代码
@interface TestTableViewCell : UITableViewCell
@property (nonatomic, strong) UITableView *tableView; // strong 造成循环引用
@end
```

`解决方案：`

`strong 改为 weak`

```objc
// 自定义Cell中的代码
@interface TestTableViewCell : UITableViewCell
@property (nonatomic, weak) UITableView *tableView; // strong 改为 weak
@end
```



### 2.2 代码块 Block

block在copy时都会对block内部用到的对象进行强引用的。

```objc
self.testObject.testCircleBlock = ^{
   [self doSomething];
};
```

`解决方案:`

`self将block作为自己的属性变量，而在block的方法体里面又引用了 self 本身，此时就很简单的形成了一个循环引用。`

`应该将 self 改为弱引用`

```objc
__weak typeof(self) weakSelf = self;
 self.testObject.testCircleBlock = ^{
      __strong typeof (weakSelf) strongSelf = weakSelf;
      [strongSelf doSomething];
};
```

### 2.3 代理方法 Delegate

delegate 属性的声明为strong会导致循环引用

```objc
// delegate 属性的声明
@property (nonatomic, strong) id <BViewControllerDelegate> delegate;

// self -> AViewController
BViewController *bVc = [BViewController new];
bVc.delegate = self; 
[self.navigationController pushViewController: bVc animated:YES];

// 假如是 strong 的情况
// bVc.delegate ===> AViewController (也就是 A 的引用计数 + 1)
// AViewController 本身又是引用了 <BViewControllerDelegate> ===> delegate 引用计数 + 1
// 导致： AViewController <======> Delegate ，也就循环引用啦

```

`解决方案:`

`将 delegate 属性声明 strong 改为 weak，否则会造成循环引用`

```objc
// delegate 属性的声明
@property (nonatomic, weak) id <BViewControllerDelegate> delegate;
```



### 2.4 计时器NSTimer

NSTimer 的 target 对传入的参数都是强引用（即使是 weak 对象）

```objc
/// 一般使用NSTimer代码如下

- (void)viewDidLoad {
    NSTimer *timer = [[NSTimer alloc] initWithFireDate:[NSDate date] interval:1 target:self selector:@selector(timerFire) userInfo:nil repeats:YES];
    [[NSRunLoop currentRunLoop] addTimer:timer forMode:NSDefaultRunLoopMode];
    self.timer = timer;
}
 
- (void)timerFire {
    NSLog(@"timer fire");
}
```

{% asset_img 1.png 预览效果 %}

`解决方案:` 

1. `在ViewController执行dealloc前释放timer（不推荐）`
2. `对定时器NSTimer封装`
3. `苹果API新增3个接口解决循环引用（iOS 10.0以上可用）`
4. `使用block进行解决`
5. `使用NSProxy进行解决`

此处就介绍第2种方案,后续详细介绍其他方式

```objc
#import <Foundation/Foundation.h>

@interface NSTimer (STBlocks)

+ (NSTimer *)st_scheduledTimeWithTimeInterval:(NSTimeInterval)interval
                                         block:(void(^)())block
                                       repeats:(BOOL)repeats;

@end


#import "NSTimer+STBlocks.h"

@implementation NSTimer (STBlocks)


+ (NSTimer *)st_scheduledTimeWithTimeInterval:(NSTimeInterval)interval
                                         block:(void(^)())block
                                       repeats:(BOOL)repeats
{
    return [self scheduledTimerWithTimeInterval:interval
                                         target:self
                                       selector:@selector(st_blockInvoke:) userInfo:[block copy]
                                        repeats:repeats];
}

- (void)st_blockInvoke:(NSTimer *)timer
{
    void (^block)() = timer.userInfo;
    if(block)
    {
        block();
    }
}

@end
```

封装后如何使用:

```objc
__weak __typeof(self) weakSelf = self;
[NSTimer st_scheduledTimeWithTimeInterval:4.0f
                                     block:^{
                                         __strong __typeof(self) strongSelf = weakSelf;
                                         [strongSelf doSomething];
                                     }
                                   repeats:YES];
```

> 计时器保留其目标对象，反复执行任务导致的循环，确实要注意，另外在dealloc的时候，不要忘了调用计时器中的 invalidate方法。