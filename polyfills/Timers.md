Timers are an important part of an application and React Native implements the [browser timers](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/Timers).

## Timers

- setTimeout, clearTimeout
- setInterval, clearInterval
- setImmediate, clearImmediate
- requestAnimationFrame, cancelAnimationFrame

`requestAnimationFrame(fn)` is the exact equivalent of `setTimeout(fn, 0)`, they are triggered right after the screen has been flushed.

`setImmediate` is executed at the end of the current JavaScript execution block, right before sending the batched response back to native. Note that if you call `setImmediate` within a `setImmediate` callback, it will be executed right away, it won't yield back to native in between.

The `Promise` implementation uses `setImmediate` as its asynchronicity primitive.


## InteractionManager

One reason why well-built native apps feel so smooth is by avoiding expensive operations during interactions and animations. In React Native, we currently have a limitation that there is only a single JS execution thread, but you can use `InteractionManager` to make sure long-running work is scheduled to start after any interactions/animations have completed.

Applications can schedule tasks to run after interactions with the following:

```javascript
InteractionManager.runAfterInteractions(() => {
   // ...long-running synchronous task...
});
```

Compare this to other scheduling alternatives:

- requestAnimationFrame(): for code that animates a view over time.
- setImmediate/setTimeout/setInterval(): run code later, note this may delay animations.
- runAfterInteractions(): run code later, without delaying active animations.

The touch handling system considers one or more active touches to be an 'interaction' and will delay `runAfterInteractions()` callbacks until all touches have ended or been cancelled.

InteractionManager also allows applications to register animations by creating an interaction 'handle' on animation start, and clearing it upon completion:

```javascript
var handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
```


## TimerMixin

We found out that the primary cause of fatals in apps created with React Native was due to timers firing after a component was unmounted. To solve this recurring issue, we introduced `TimerMixin`. If you include `TimerMixin`, then you can replace your calls to `setTimeout(fn, 500)` with `this.setTimeout(fn, 500)` (just prepend `this.`) and everything will be properly cleaned up for you when the component unmounts.

```javascript
var TimerMixin = require('react-timer-mixin');

var Component = React.createClass({
  mixins: [TimerMixin],
  componentDidMount: function() {
    this.setTimeout(
      () => { console.log('I do not leak!'); },
      500
    );
  }
});
```

We highly recommend never using bare timers and always using this mixin, it will save you from a lot of hard to track down bugs.

定时器是应用程序的重要组成部分.React Native遵循浏览器版本相关规则实现定时器[浏览器定时器参考](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/Timers).

## 定时器

- setTimeout, clearTimeout
- setInterval, clearInterval
- setImmediate, clearImmediate
- requestAnimationFrame, cancelAnimationFrame

`requestAnimationFrame(fn)` 与`setTimeout(fn, 0)`调用效果相同, 二者均在屏幕刷新后立刻触发.

`setImmediate`在当前Javascript代码运行完毕, 与原生线程通信之前执行.请注意,如果你在setImmediate函数的回调函数中调用了setImmediate,那么回调中的setImmediate将立即执行,此过程不与原生线程通信.

`Promise`对象，就是利用`setImmediate` 作为基础，实现ES6 Promise/A+的同步机制.

## InteractionManager对象

设计良好的原生App之所以能够运行平滑，原因在于在动画和交互操作进行时，系统禁止了开销大的指令运行.在React Native中,我们做了限制,程序中时刻只有一个JS运行线程(JavaScript Core VM)，但是你可以通过使用InteractionManager对象来保证一个需要长时间执行的任务,在交互或动画运行完成后开始执行.

程序可以预先计划某项任务在交互完成后执行:

```javascript
InteractionManager.runAfterInteractions(() => {
   // ...需要长时间运行的同步任务...
});
```

比较互动管理器与其他代码计划运行调用方式：
- requestAnimationFrame() 控制一个视图实现一段动画
- setImmediate/setTimeout/setInterval(): 延迟运行计划运行的代码,请注意,这种调用方式会阻塞动画运行.
- runAfterInteractions(): 延迟运行计划运行的代码，不会阻塞正在运行的动画

当屏幕上有一个或更多触点时，触点管理系统会认为当前存在交互，在触点全部消失时自动调用`runAfterInteractions()` 回调函数.

我们也可以通过InteractionManager对象的createInteractionHanle()注册动画，实现动画串联播放

```javascript
var handle = InteractionManager.createInteractionHandle();
// 运行一段动画... (`runAfterInteractions` 中注册的函数目前在队列中)
// 在动画完成时:
InteractionManager.clearInteractionHandle(handle);
// 在所有createInteractionHandle注册的方法完成后，runAfterInteractions注册的任务开始运行
```

## 定时器成员(TimerMixin)

通过React Native创建的应用的大部分运行错误源于定时器在组件卸载后触发.为了解决这个问题,React Native引入了定时器成员(TimerMixin)概念.引入定时器成员后,代码中可以使用有归属对象的定时器'this.setTimeout(fn, 500)'代替掉无归属对象的定时器'setTimeout(fn, 500)'，这样做的好处是当组件卸载时,会自动清理掉定时器,避免出现定时器在卸载后依旧能触发的问题.

```javascript
var TimerMixin = require('react-timer-mixin');
var Component = React.createClass({
  mixins: [TimerMixin],
  componentDidMount: function() {
    this.setTimeout(
      () => { console.log('I do not leak!'); },
      500
    );
  }
});
```

在此强烈建议您不要使用无归属对象的定时器,转而使用定时器成员,这样做能有效规避BUG.