---
id: nativemodulesios
title: Native Modules (iOS)
layout: docs
category: Guides
permalink: docs/nativemodulesios.html
next: testing
---

Sometimes an app needs access to platform API, and React Native doesn't have a corresponding wrapper yet. Maybe you want to reuse some existing Objective-C or C++ code without having to reimplement it in JavaScript. Or write some high performance, multi-threaded code such as image processing, network stack, database or rendering.

We designed React Native such that it is possible for you to write real native code and have access to the full power of the platform. This is a more advanced feature and we don't expect it to be part of the usual development process, however it is essential that it exists. If React Native doesn't support a native feature that you need, you should be able to build it yourself.

This is a more advanced guide that shows how to build a native module. It assumes the reader knows Objective-C (Swift is not supported yet) and core libraries (Foundation, UIKit).

## iOS Calendar module example

This guide will use [iOS Calendar API](https://developer.apple.com/library/mac/documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html) example. Let's say we would like to be able to access iOS calendar from JavaScript.

Native module is just an Objectve-C class that implements `RCTBridgeModule` protocol. If you are wondering, RCT is a shorthand for ReaCT.

```objective-c
// CalendarManager.h
#import "RCTBridgeModule.h"

@interface CalendarManager : NSObject <RCTBridgeModule>
@end
```

React Native will not expose any methods of `CalendarManager` to JavaScript unless explicitly asked. Fortunately this is pretty easy with `RCT_EXPORT`:

```objective-c
// CalendarManager.m
@implementation CalendarManager

- (void)addEventWithName:(NSString *)name location:(NSString *)location
{
  RCT_EXPORT();
  RCTLogInfo(@"Pretending to create an event %@ at %@", name, location);
}

@end
```

Now from your JavaScript file you can call the method like this:

```javascript
var CalendarManager = require('NativeModules').CalendarManager;
CalendarManager.addEventWithName('Birthday Party', '4 Privet Drive, Surrey');
```

Notice that the exported method name was generated from first part of Objective-C selector. Sometimes it results in a non-idiomatic JavaScript name (like the one in our example). You can change the name by supplying an optional argument to `RCT_EXPORT`, e.g. `RCT_EXPORT(addEvent)`.

The return type of the method should always be `void`. React Native bridge is asynchronous, so the only way to pass result to JavaScript is by using callbacks or emitting events (see below).

## Argument types

React Native supports several types of arguments that can be passed from JavaScript code to native module:

- string (`NSString`)
- number (`NSInteger`, `float`, `double`, `CGFloat`, `NSNumber`)
- boolean (`BOOL`, `NSNumber`)
- array (`NSArray`) of any types from this list
- map (`NSDictionary`) with string keys and values of any type from this list
- function (`RCTResponseSenderBlock`)

In our `CalendarManager` example, if we want to pass event date to native, we have to convert it to a string or a number:

```objective-c
- (void)addEventWithName:(NSString *)name location:(NSString *)location date:(NSInteger)secondsSinceUnixEpoch
{
  RCT_EXPORT(addEvent);
  NSDate *date = [NSDate dateWithTimeIntervalSince1970:secondsSinceUnixEpoch];
}
```

As `CalendarManager.addEvent` method gets more and more complex, the number of arguments will grow. Some of them might be optional. In this case it's worth considering changing the API a little bit to accept a dictionary of event attributes, like this:

```objective-c
- (void)addEventWithName:(NSString *)name details:(NSDictionary *)details
{
  RCT_EXPORT(addEvent);
  NSString *location = [RCTConvert NSString:details[@"location"]]; // ensure location is a string
  ...
}
```

and call it from JavaScript:

```javascript
CalendarManager.addEvent('Birthday Party', {
  location: '4 Privet Drive, Surrey',
  time: date.toTime(),
  description: '...'
})
```

> **NOTE**: About array and map
>
> React Native doesn't provide any guarantees about the types of values in these structures. Your native module might expect array of strings, but if JavaScript calls your method with an array that contains number and string you'll get `NSArray` with `NSNumber` and `NSString`. It's developer's responsibility to check array/map values types (see [`RCTConvert`](https://github.com/facebook/react-native/blob/master/React/Base/RCTConvert.h) for helper methods).

# Callbacks

> **WARNING**
>
> This section is even more experimental than others, we don't have a set of best practices around callbacks yet.

Native module also supports a special kind of argument - callback. In most cases it is used to provide function call result to JavaScript.

```objective-c
- (void)findEvents:(RCTResponseSenderBlock)callback
{
  RCT_EXPORT();
  NSArray *events = ...
  callback(@[[NSNull null], events]);
}
```

`RCTResponseSenderBlock` accepts only one argument - array of arguments to pass to JavaScript callback. In this case we use node's convention to set first argument to error and the rest - to the result of the function.

```javascript
CalendarManager.findEvents((error, events) => {
  if (error) {
    console.error(error);
  } else {
    this.setState({events: events});
  }
})
```

Native module is supposed to invoke callback only once. It can, however, store the callback as an ivar and invoke it later. This pattern is often used to wrap iOS APIs that require delegate. See [`RCTAlertManager`](https://github.com/facebook/react-native/blob/master/React/Modules/RCTAlertManager.m).

If you want to pass error-like object to JavaScript, use `RCTMakeError` from [`RCTUtils.h`](https://github.com/facebook/react-native/blob/master/React/Base/RCTUtils.h).

## Implementing native module

The native module should not have any assumptions about what thread it is being called on. React Native invokes native modules methods on a separate serial GCD queue, but this is an implementation detail and might change. If the native module needs to call main-thread-only iOS API, it should schedule the operation on the main queue:


```objective-c
- (void)addEventWithName:(NSString *)name callback:(RCTResponseSenderBlock)callback
{
  RCT_EXPORT(addEvent);
  dispatch_async(dispatch_get_main_queue(), ^{
    // Call iOS API on main thread
    ...
    // You can invoke callback from any thread/queue
    callback(@[...]);
  });
}
```

The same way if the operation can take a long time to complete, the native module should not block. It is a good idea to use `dispatch_async` to schedule expensive work on background queue.

## Exporting constants

Native module can export constants that are instantly available to JavaScript at runtime. This is useful to export some initial data that would otherwise require a bridge round-trip.

```objective-c
- (NSDictionary *)constantsToExport
{
  return @{ @"firstDayOfTheWeek": @"Monday" };
}
```

JavaScript can use this value right away:

```javascript
console.log(CalendarManager.firstDayOfTheWeek);
```

Note that the constants are exported only at initialization time, so if you change `constantsToExport` value at runtime it won't affect JavaScript environment.


## Sending events to JavaScript

The native module can signal events to JavaScript without being invoked directly. The easiest way to do this is to use `eventDispatcher`:

```objective-c
- (void)calendarEventReminderReceived:(NSNotification *)notification
{
  NSString *eventName = notification.userInfo[@"name"];
  [self.bridge.eventDispatcher sendAppEventWithName:@"EventReminder"
                                               body:@{@"name": eventName}];
}
```

JavaScript code can subscribe to these events:

```javascript
var subscription = DeviceEventEmitter.addListener(
  'EventReminder',
  (reminder) => console.log(reminder.name)
);
...
// Don't forget to unsubscribe
subscription.remove();
```
For more examples of sending events to JavaScript, see [`RCTLocationObserver`](https://github.com/facebook/react-native/blob/master/Libraries/Geolocation/RCTLocationObserver.m).

 
有时候，一个 app 需要访问平台 API 能力，而 React Native 并没有对应的本地的包装器接口可供调用。可能你想要重用已经存在的 Objective-C 或 C++ 代码，而不想重新用 JavaScript 实现一遍。或者想写一些高性能、多线程的代码，比如图片处理、网络堆栈、数据库或者渲染。

我们设计了 React Native 使得你可以写真实的本地代码和访问平台全部功能的代码成为可能。这是一个更加高级的特性，我们并不想把它只是作为通常开发流程的一部分，尽管它的存在是必须的。如果 React Native 并不支持你想要的本地功能特性，你应该可以自己构建它。

这是一个更加高级的入门文档，来向你展示如何构建一个本地的模块。我们假设读者都知道 Objective-C (Swift 当前还不支持)和核心的库（Foundation, UIKit）。

## iOS 日历模块例子

这个文档将使用 [iOS Calendar API](https://developer.apple.com/library/mac/documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html) 例子。也就是说，我们想要通过 Javascript 访问 iOS 日历。

本地模块只是一个 Objective-C 类，它实现了 `RCTBridgeModule` 协议。你不用觉得困惑，RCT 是 ReaCT 的简称。

```objective-c
// CalendarManager.h
#import "RCTBridgeModule.h"

@interface CalendarManager : NSObject <RCTBridgeModule>
@end
```

React Native 不会暴露任何 `CalendarManager` 方法给 JavaScript，除非你显示要求。幸运的是，使用 `RCT_EXPORT` 相当简单：

```objective-c
// CalendarManager.m
@implementation CalendarManager

- (void)addEventWithName:(NSString *)name location:(NSString *)location
{
  RCT_EXPORT();
  RCTLogInfo(@"Pretending to create an event %@ at %@", name, location);
}

@end
```

现在从你的 JavaScript 文件你可以这样调用该方法：

```javascript
var CalendarManager = require('NativeModules').CalendarManager;
CalendarManager.addEventWithName('Birthday Party', '4 Privet Drive, Surrey');
```

要注意的是暴露的方法名称是从 Objective-C 选择符的第一部分产生的。有时候会导致一些不符合 JavaScript 语言命名习惯的名称（就像我们这个例子里所见到的）。你可以通过为 `RCT_EXPORT` 提供一个可选的参数来改变这个名称，比如 `RCT_EXPORT(addEvent)` 。

该方法的返回类型应该始终是 `void` 。React Native 桥接是异步的，所以把结果返回给 JavaScript的唯一方式是通过回调或者发射事件形式（具体参见下面）。

## 参数类型

React Native 支持一些参数类型，可以直接从 JavaScript 代码传递给本地模块：

- string (`NSString`)
- number (`NSInteger`, `float`, `double`, `CGFloat`, `NSNumber`)
- boolean (`BOOL`, `NSNumber`)
- array (`NSArray`) 任何来自于这里所列的参数类型的类型都可以
- map (`NSDictionary`) 字符串的 key 和 任何来自于这里所列的参数类型的 value 
- function (`RCTResponseSenderBlock`)

在我们的 `CalendarManager` 例子里，如果我们想要传递事件日期给本地模块，我们得把它转成字符串或者数字：

```objective-c
- (void)addEventWithName:(NSString *)name location:(NSString *)location date:(NSInteger)secondsSinceUnixEpoch
{
  RCT_EXPORT(addEvent);
  NSDate *date = [NSDate dateWithTimeIntervalSince1970:secondsSinceUnixEpoch];
}
```

随着 `CalendarManager.addEvent` 方法变得越来越复杂，参数数量会越来越多。其中一些参数可能是可选的。在这种情况下，是时候考虑改变一下API的设计了，使其可以接受一个事件属性的字典，就像这样： 

```objective-c
- (void)addEventWithName:(NSString *)name details:(NSDictionary *)details
{
  RCT_EXPORT(addEvent);
  NSString *location = [RCTConvert NSString:details[@"location"]]; // ensure location is a string
  ...
}
```

在 JavaScript 里这样调用：

```javascript
CalendarManager.addEvent('Birthday Party', {
  location: '4 Privet Drive, Surrey',
  time: date.toTime(),
  description: '...'
})
```

> **注意**: 关于 array 和 map
>
> React Native 并没有为这些结构里的值的类型提供任何保证。你的本地模块可能期望接收一个字符串的数组，但是如果在 JavaScript 调用你的方法时候，传递了一个既包含数字又包含字符串的数组，你将会得到一个 `NSArray` 同时包含 `NSNumber` 和 `NSString` 类型的数据。这是开发者的职责，应该确保 array/map 的数据类型的正确性(参见 [`RCTConvert`](https://github.com/facebook/react-native/blob/master/React/Base/RCTConvert.h) 的一些助手方法)。

# 回调

> **警告**
>
> 这部分内容相比其它部分更加偏试验性质，关于回调，我们还没有一系列的最佳实践可供参考。

本地模块也支持一种特殊类型的参数 - 回调。在大部分情况，它用来给 JavaScript 提供函数执行结果的回调。

```objective-c
- (void)findEvents:(RCTResponseSenderBlock)callback
{
  RCT_EXPORT();
  NSArray *events = ...
  callback(@[[NSNull null], events]);
}
```

`RCTResponseSenderBlock` 只接受一个参数 -  arguments 数组，用来传递给 JavaScript 回调。在这个例子，我们使用 node 风格来设置参数，第一个参数作为出错的参数信息，其它参数作为该函数的执行结果。

```javascript
CalendarManager.findEvents((error, events) => {
  if (error) {
    console.error(error);
  } else {
    this.setState({events: events});
  }
})
```

本地模块假定回调只被调用一次。当然，它也可以把回调先存储起来，过后再调用这个回调。这种模式经常被用来包装需要代理的 iOS API 接口。参见[`RCTAlertManager`](https://github.com/facebook/react-native/blob/master/React/Modules/RCTAlertManager.m)。

如果你想要传递类似错误对象的参数给 JavaScript，你可以使用 [`RCTUtils.h`](https://github.com/facebook/react-native/blob/master/React/Base/RCTUtils.h) 的 `RCTMakeError` 方法。

## 实现本地模块

本地模块不应该有任何关于当前什么线程正在被调用的假设。React Native 在一个独立的顺序的 GCD 队列里调用本地模块方法，但这只是一个可能发生变化的实现细节。如果本地模块需要调用只是主线程的 iOS API，它应该在主队列里调度该操作：


```objective-c
- (void)addEventWithName:(NSString *)name callback:(RCTResponseSenderBlock)callback
{
  RCT_EXPORT(addEvent);
  dispatch_async(dispatch_get_main_queue(), ^{
    // 调用主线程的 iOS API 
    ...
    // 你可以调用来自任何线程或者队列里的回调
    callback(@[...]);
  });
}
```

同样地，如果该操作可能需要花费比较长的时间去完成，本地模块不应该被阻塞。使用 `dispatch_async` 在后台队列里调度需要耗时更长的工作是一种更好的方式。

## 导出常量

本地模块可以导出给 JavaScript 在运行时环境下立刻能访问的常量。当需要导出一些初始化数据的时候，这通常非常有用，否则需要一个来回的桥接。

```objective-c
- (NSDictionary *)constantsToExport
{
  return @{ @"firstDayOfTheWeek": @"Monday" };
}
```

JavaScript 可以立刻使用这个值：

```javascript
console.log(CalendarManager.firstDayOfTheWeek);
```

注意，导出常量的时机只能在初始化时候，所以如果你在运行时改变 `constantsToExport` 值，并不会影响 JavaScript 环境。


## 向 JavaScript 发送事件

本地模块可以向 JavaScript 发送事件信号，而不需要直接调用 JavaScript。最简单方式是使用 `eventDispatcher`：

```objective-c
- (void)calendarEventReminderReceived:(NSNotification *)notification
{
  NSString *eventName = notification.userInfo[@"name"];
  [self.bridge.eventDispatcher sendAppEventWithName:@"EventReminder"
                                               body:@{@"name": eventName}];
}
```

JavaScript 代码可以订阅这些事件：

```javascript
var subscription = DeviceEventEmitter.addListener(
  'EventReminder',
  (reminder) => console.log(reminder.name)
);
...
// 不要忘记移除订阅
subscription.remove();
```
更多关于向 JavaScript 发送事件的例子，参考 [`RCTLocationObserver`](https://github.com/facebook/react-native/blob/master/Libraries/Geolocation/RCTLocationObserver.m)。

