InteractionManager allows long-running work to be scheduled after any interactions/animations have completed. In particular, this allows JavaScript animations to run smoothly.

交互管理器允许在任何交互或动画执行完成以后安排长期运行的任务。具体来说，这可以让 JavaScript 动画运行流畅。

Applications can schedule tasks to run after interactions with the following:

在交互完成以后，应用程序可以使用以下代码安排任务执行：

```javascript
InteractionManager.runAfterInteractions(() => {
  // ...long-running synchronous task...
});
```

Compare this to other scheduling alternatives:

* requestAnimationFrame(): for code that animates a view over time.
* setImmediate/setTimeout(): run code later, note this may delay animations.
* runAfterInteractions(): run code later, without delaying active animations.

与其它的调度方案进行比较：

* `requestAnimationFrame()`： 针对随时间推移改变视图的动画代码。
* `setImmediate()`/`setTimeout()`：注意运行代码以后，会推迟动画执行。
* `runAfterInteractions()`：运行代码以后，不会推迟动画执行。

The touch handling system considers one or more active touches to be an 'interaction' and will delay `runAfterInteractions()` callbacks until all touches have ended or been cancelled.

触发处理系统将一个或多个活跃的触发事件标记为一次 'interaction'，并且推迟 `runAfterInteractions()` 回调函数的执行，直到所有的触发操作已完成或者被取消了。

InteractionManager also allows applications to register animations by creating an interaction 'handle' on animation start, and clearing it upon completion:

交互管理器同样允许应用程序在动画开始的时候通过创建一个交互 'handle' 的方式注册动画，然后在动画结束的时候清除它。

```javascript
var handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
```

## Methods

## 方法

static **runAfterInteractions**(callback: Function)

Schedule a function to run after all interactions have completed.

在所有交互完成以后调度函数执行。

static **createInteractionHandle**()

Notify manager that an interaction has started.

通知管理器一次交互操作开始了。

static **clearInteractionHandle**(handle: number)

Notify manager that an interaction has completed.

通知管理器一次交互操作已经完成。
