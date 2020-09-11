---
title: iOS开发之 ~ NSTimer 循环引用
date: 2016-12-23 18:10:24
tags: [iOS]
categories: [iOS开发]
---

## 1. 概述

> 在使用NSTimer，如果使用不得当特别会引起循环引用，造成内存泄露。所以怎么避免循环引用问题，下面我提出几种解决NSTimer的几种循环引用。

在iOS中，NSTimer的使用是非常频繁的，但是NSTimer在使用中需要注意，避免循环引用的问题。之前经常这样写

```objc
- (void)setupTimer {
    self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(timerAction) userInfo:nil repeats:YES];
  [[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSDefaultRunLoopMode];
}

- (void)dealloc {
    [self.timer invalidate];
    self.timer = nil;
}
```

由于self强引用了timer，同时timer也强引用了self，所以循环引用造成dealloc方法根本不会走，self和timer都不会被释放，造成内存泄漏。



## 2. 解决方案

- 在ViewController执行dealloc前释放timer（不推荐）
- 对定时器NSTimer封装
- 苹果API接口解决方案（iOS 10.0以上可用）
- 使用block进行解决
- 使用NSProxy进行解决



### 2.1 在ViewController执行dealloc前释放timer（不推荐）

> 在viewWillAppear中创建timer
>
> 在viewWillDisappear中销毁timer

在某些情况下，这种做法是可以解决问题的，但是有时却会引起其他问题，比如控制器push到下一个控制器，viewDidDisappear后，timer被释放，此时再回来，timer已经不复存在了。

所以，这种"方案"并不是合理的。

### 2.2 对定时器NSTimer封装

```objc
//STTimer.h文件
#import <Foundation/Foundation.h>
@interface STTimer : NSObject

//开启定时器
- (void)startTimer;

//暂停定时器
- (void)stopTimer;
@end


#import "STTimer.h"

@implementation STTimer {
    
    NSTimer *_timer;
}

- (void)stopTimer{
    
    if (_timer == nil) {
        return;
    }
    [_timer invalidate];
    _timer = nil;
}


- (void)startTimer{
    
    _timer = [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(work) userInfo:nil repeats:YES];
  [[NSRunLoop currentRunLoop] addTimer:_timer forMode:NSDefaultRunLoopMode];
}

- (void)work{
    
    NSLog(@"正在计时中。。。。。。");
}

- (void)dealloc{
    
   NSLog(@"%s",__func__);
    [_timer invalidate];
    _timer = nil;
}

@end
```

在ViewController中使用代码如下：

```objc
#import "ViewController1.h"
#import "STTimer.h"

@interface ViewController ()

@property (nonatomic, strong) STTimer *timer;

@end

@implementation ViewController

- (void)viewWillDisappear:(BOOL)animated {
    
    [super viewWillDisappear:animated];
}

- (void)viewDidLoad {
    [super viewDidLoad];
    self.title = @"VC1";
    self.view.backgroundColor = [UIColor whiteColor];
    
    //自定义timer
    STTimer *timer = [[STTimer alloc] init];
    self.timer = timer;
    [timer startTimer];
}

- (void)dealloc {
   
    [self.timer stopTimer];
    NSLog(@"%s",__func__);
}

```

运行打印结果：

```objc
-[ViewController dealloc]
-[STTimer dealloc]
```

这个方式主要就是让STTimer强引用NSTimer，NSTimer强引用PFTimer,避免让NSTimer强引用ViewController，这样就不会引起循环引用，然后在dealloc方法中执行NSTimer的销毁，相对的PFTimer也会进行销毁了。

### 2.3 苹果系统API可以解决（iOS10以上）

在iOS 10.0以后，苹果官方新增了关于NSTimer的三个API：

```objc
+ (NSTimer *)timerWithTimeInterval:(NSTimeInterval)interval repeats:
(BOOL)repeats block:(void (^)(NSTimer *timer))block 
API_AVAILABLE(macosx(10.12), ios(10.0), watchos(3.0), tvos(10.0));

+ (NSTimer *)scheduledTimerWithTimeInterval:(NSTimeInterval)interval repeats:
(BOOL)repeats block:(void (^)(NSTimer *timer))block 
API_AVAILABLE(macosx(10.12), ios(10.0), watchos(3.0), tvos(10.0));

- (instancetype)initWithFireDate:(NSDate *)date interval:
(NSTimeInterval)interval repeats:(BOOL)repeats block:(void (^)(NSTimer *timer))block 
API_AVAILABLE(macosx(10.12), ios(10.0), watchos(3.0), tvos(10.0));
```

这三个方法都有一个Block的回调方法。关于block参数，官方文档有说明：

> the timer itself is passed as the parameter to this block when executed 
> to aid in avoiding cyclical references。
>
> 翻译过来就是说，定时器在执行时，将自身作为参数传递给block，来帮助避免循环引用。

使用很简单，但是要注意两点：

> 1.避免block的循环引用，使用 __weak 和  __strong 来避免
> 2.在持用NSTimer对象的类的方法中-(void)dealloc调用NSTimer 的- (void)invalidate方法；

### 2.4 使用block来解决

该方案主要要点：

- 将计时器所应执行的任务封装成"Block"，在调用计时器函数时，把block作为userInfo参数传进去。
- userInfo参数用来存放"不透明值"，只要计时器有效，就会一直保留它。
- 在传入参数时要通过copy方法，将block拷贝到"堆区"，否则等到稍后要执行它的时候，该blcok可能已经无效了。
- 计时器现在的target是NSTimer类对象，这是个单例，因此计时器是否会保留它，其实都无所谓。此处依然有保留环，然而因为类对象（class object）无需回收，所以不用担心。

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

### 2.5 使用NSProxy来解决循环引用

```objc
// STProxy.h
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface STProxy : NSProxy

//通过创建对象
- (instancetype)initWithObjc:(id)object;

//通过类方法创建创建
+ (instancetype)proxyWithObjc:(id)object;

@end

NS_ASSUME_NONNULL_END

// STProxy.m

#import "STProxy.h"

@interface STProxy()

@property (nonatomic, weak) id object;

@end
@implementation STProxy

- (instancetype)initWithObjc:(id)object {
    
    self.object = object;
    return self;
}

+ (instancetype)proxyWithObjc:(id)object {
    
    return [[self alloc] initWithObjc:object];
}

- (void)forwardInvocation:(NSInvocation *)invocation {
    
    if ([self.object respondsToSelector:invocation.selector]) {
        
        [invocation invokeWithTarget:self.object];
    }
}

- (NSMethodSignature *)methodSignatureForSelector:(SEL)sel {
    
    return [self.object methodSignatureForSelector:sel];
}
@end
```

在使用的时候如下代码：

```objc
#import "ViewController.h"
#import "STProxy.h"

@interface ViewController ()

//使用NSProxy
@property (nonatomic, strong) NSTimer *timer2;

@end

@implementation ViewController

- (void)viewWillDisappear:(BOOL)animated {
    
    [super viewWillDisappear:animated];
}

- (void)viewDidLoad {
    [super viewDidLoad];
    self.title = @"VC1";
    self.view.backgroundColor = [UIColor whiteColor];
    
    STProxy *proxy = [[STProxy alloc] initWithObjc:self];
    self.timer2 = [NSTimer scheduledTimerWithTimeInterval:1.0 target:proxy selector:@selector(timerHandle) userInfo:nil repeats:YES];
  [[NSRunLoop currentRunLoop] addTimer:self.timer2 forMode:NSDefaultRunLoopMode];
}

//定时触发的事件
- (void)timerHandle {
     NSLog(@"正在计时中。。。。。。");
}

- (void)dealloc {
    [self.timer2 invalidate];
    self.timer2 = nil;
    NSLog(@"%s",__func__);
}

@end
```

当pop当前viewController时候，打印结果：

```objc
-[ViewController dealloc]
```

通过STProxy这个伪基类（相当于ViewController的复制类），避免直接让timer和viewController造成循环。