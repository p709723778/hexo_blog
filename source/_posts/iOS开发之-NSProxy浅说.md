---
title: iOS开发之 ~ NSProxy浅说
date: 2016-12-29 18:10:24
tags: [iOS]
categories: [iOS开发]
---

### 概要

> NSProxy是一个抽象的超类,它遵守了 NSObject 协议，并且是不继承自NSObject的。可以通过它的API为其它的Object对象或者不存在的对象提供替身。
>



.h头文件声明如下

```objc
@class NSMethodSignature, NSInvocation;

NS_ROOT_CLASS
@interface NSProxy <NSObject> {
    Class	isa;
}

+ (id)alloc;
+ (id)allocWithZone:(nullable NSZone *)zone NS_AUTOMATED_REFCOUNT_UNAVAILABLE;
+ (Class)class;

- (void)forwardInvocation:(NSInvocation *)invocation;
- (nullable NSMethodSignature *)methodSignatureForSelector:(SEL)sel NS_SWIFT_UNAVAILABLE("NSInvocation and related APIs not available");
- (void)dealloc;
- (void)finalize;
@property (readonly, copy) NSString *description;
@property (readonly, copy) NSString *debugDescription;
+ (BOOL)respondsToSelector:(SEL)aSelector;

- (BOOL)allowsWeakReference API_UNAVAILABLE(macos, ios, watchos, tvos);
- (BOOL)retainWeakReference API_UNAVAILABLE(macos, ios, watchos, tvos);

// - (id)forwardingTargetForSelector:(SEL)aSelector;

@end
```

`NSProxy` 的使用一般比较少,没了解之前看到它心里就冒出"这什么鬼,有点深奥呦!"的想法,其实非常简单，通常你只需要实现两个方法：

```objc
- (void)forwardInvocation:(NSInvocation *)invocation;
- (nullable NSMethodSignature *)methodSignatureForSelector:(SEL)sel NS_SWIFT_UNAVAILABLE("NSInvocation and related APIs not available");
```

那么我们通过Demo来给大家演示一下NSProxy的使用.

```objc
@interface MyProxy : NSProxy
{
    id _object;
}

+ (id)proxyForObject:(id)obj;

@end

@implementation MyProxy

+ (id)proxyForObject:(id)obj {
    MyProxy *instance = [MyProxy alloc];
    instance->_object = obj;

    return instance;
}

- (NSMethodSignature *)methodSignatureForSelector:(SEL)sel {
    return [_object methodSignatureForSelector:sel];
}

- (void)forwardInvocation:(NSInvocation *)invocation {
    if ([_object respondsToSelector:invocation.selector]) {
        NSString *selectorName = NSStringFromSelector(invocation.selector);

        NSLog(@"Before calling \"%@\".", selectorName);
        [invocation invokeWithTarget:_object];
        NSLog(@"After calling \"%@\".", selectorName);

        // 调用堆栈的符号
        NSLog(@"%@", [NSThread callStackSymbols]);
    }
}

@end
```

这是我们的 Proxy 简单实现，我们需要持有一个被代理对象的引用，然后将消息转发到这个对象上，在转发之前和以后我们就可以做自己想做的事情了。

`methodSignatureForSelector:` 方法需要获取一个方法签名，用来生成 NSInvocation，我们直接将这个调用转发到被代理对象中。紧接着，`forwardInvocation:` 会被调用，将 NSInvocation 用被代理对象调用。我们就可以在这个方法里做一些手脚，比如埋点计数等。在这个例子中，我只是简单地将对象所调用的方法的 selector 打印出来。

然后我们看看用于测试的主函数：

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.

    NSURL *url = [MyProxy proxyForObject:[NSURL URLWithString:@"https://www.baidu.com/"]];

    NSURLSessionDataTask *task = [[NSURLSession sharedSession] dataTaskWithURL:url completionHandler:^(NSData *_Nullable data, NSURLResponse *_Nullable response, NSError *_Nullable error) {
        
    }];

    [task resume];

    return YES;
}
```

就是简单构造一个 `NSURL`，只不过我们先用了 `MyProxy` 封装代理后传给 `NSURLSession` 去使用，输出结果如下：

```objc
2016-12-29 18:40:24.998735+0800 test[12184:851334] Before calling "absoluteURL".
2016-12-29 18:40:24.998814+0800 test[12184:851334] After calling "absoluteURL".
```

也就是说，系统用 `NSURL` 的 `absoluteURL` 属性来获取真正的 URL 数据，至此我们就已经可以跟踪已有类的行为了，甚至还可以通过 `[NSThread callStackSymbols]` 来跟踪调用改方法的函数调用栈：

```objc
2016-12-29 18:40:25.000404+0800 test[12184:851334] (
	0   test                                0x0000000102575108 -[MyProxy forwardInvocation:] + 296
	1   CoreFoundation                      0x00000001a3042e64 271BBB21-28D6-37CE-894F-43B3352DA6CE + 1162852
	2   CoreFoundation                      0x00000001a304509c _CF_forwarding_prep_0 + 92
	3   CoreFoundation                      0x00000001a3011758 CFURLCopyAbsoluteURL + 68
	4   CFNetwork                           0x00000001a361acb8 CFNetwork + 23736
	5   CFNetwork                           0x00000001a361ab80 CFNetwork + 23424
	6   CFNetwork                           0x00000001a361f7b4 CFNetwork + 42932
	7   test                                0x0000000102575b54 -[AppDelegate application:didFinishLaunchingWithOptions:] + 276
	8   UIKitCore                           0x00000001a588c9dc FBF3D986-2D16-3B6C-BE71-216144F6307F + 11676124
	9   UIKitCore                           0x00000001a588e97c FBF3D986-2D16-3B6C-BE71-216144F6307F + 11684220
	10  UIKitCore                           0x00000001a58940dc FBF3D986-2D16-3B6C-BE71-216144F6307F + 11706588
	11  UIKitCore                           0x00000001a4f6eef4 FBF3D986-2D16-3B6C-BE71-216144F6307F + 2117364
	12  UIKitCore                           0x00000001a589005c FBF3D986-2D16-3B6C-BE71-216144F6307F + 11690076
	13  UIKitCore                           0x00000001a5890464 FBF3D986-2D16-3B6C-BE71-216144F6307F + 11691108
	14  UIKitCore                           0x00000001a5895a08 UIApplicationMain + 164
	15  test                                0x0000000102574ecc main + 332
	16  libdyld.dylib                       0x00000001a2c99588 A333D9F4-8DA0-330D-B17E-0A92EC3DEF07 + 5512
)
```

并借此来跟踪一些系统行为。

