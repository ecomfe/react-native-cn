---
id: gesture-responder-system
title: Gesture Responder System
layout: docs
category: Guides
permalink: docs/gesture-responder-system.html
next: nativemodulesios
---

Gesture recognition on mobile devices is much more complicated than web. A touch can go through several phases as the app determines what the user's intention is. For example, the app needs to determine if the touch is scrolling, sliding on a widget, or tapping. This can even change during the duration of a touch. There can also be multiple simultaneous touches.

The touch responder system is needed to allow components to negotiate these touch interactions without any additional knowledge about their parent or child components. This system is implemented in `ResponderEventPlugin.js`, which contains further details and documentation.

### Best Practices

Users can feel huge differences in the usability of web apps vs. native, and this is one of the big causes. Every action should have the following attributes:

- Feedback/highlighting- show the user what is handling their touch, and what will happen when they release the gesture
- Cancel-ability- when making an action, the user should be able to abort it mid-touch by dragging their finger away

These features make users more comfortable while using an app, because it allows people to experiment and interact without fear of making mistakes.

### TouchableHighlight and Touchable*

The responder system can be complicated to use. So we have provided an abstract `Touchable` implementation for things that should be "tappable". This uses the responder system and allows you to easily configure tap interactions declaratively. Use `TouchableHighlight` anywhere where you would use a button or link on web.


## Responder Lifecycle

A view can become the touch responder by implementing the correct negotiation methods. There are two methods to ask the view if it wants to become responder:

 - `View.props.onStartShouldSetResponder: (evt) => true,` - Does this view want to become responder on the start of a touch?
 - `View.props.onMoveShouldSetResponder: (evt) => true,` - Called for every touch move on the View when it is not the responder: does this view want to "claim" touch responsiveness?

If the View returns true and attempts to become the responder, one of the following will happen:

 - `View.props.onResponderGrant: (evt) => {}` - The View is now responding for touch events. This is the time to highlight and show the user what is happening
 - `View.props.onResponderReject: (evt) => {}` - Something else is the responder right now and will not release it

If the view is responding, the following handlers can be called:

 - `View.props.onResponderMove: (evt) => {}` - The user is moving their finger
 - `View.props.onResponderRelease: (evt) => {}` - Fired at the end of the touch, ie "touchUp"
 - `View.props.onResponderTerminationRequest: (evt) => true` - Something else wants to become responder. Should this view release the responder? Returning true allows release
 - `View.props.onResponderTerminate: (evt) => {}` - The responder has been taken from the View. Might be taken by other views after a call to `onResponderTerminationRequest`, or might be taken by the OS without asking (happens with control center/ notification center on iOS)

`evt` is a synthetic touch event with the following form:

 - `nativeEvent`
     + `changedTouches` - Array of all touch events that have changed since the last event
     + `identifier` - The ID of the touch
     + `locationX` - The X position of the touch, relative to the element
     + `locationY` - The Y position of the touch, relative to the element
     + `pageX` - The X position of the touch, relative to the screen
     + `pageY` - The Y position of the touch, relative to the screen
     + `target` - The node id of the element receiving the touch event
     + `timestamp` - A time identifier for the touch, useful for velocity calculation
     + `touches` - Array of all current touches on the screen

### Capture ShouldSet Handlers

`onStartShouldSetResponder` and `onMoveShouldSetResponder` are called with a bubbling pattern, where the deepest node is called first. That means that the deepest component will become responder when multiple Views return true for `*ShouldSetResponder` handlers. This is desirable in most cases, because it makes sure all controls and buttons are usable.

However, sometimes a parent will want to make sure that it becomes responder. This can be handled by using the capture phase. Before the responder system bubbles up from the deepest component, it will do a capture phase, firing `on*ShouldSetResponderCapture`. So if a parent View wants to prevent the child from becoming responder on a touch start, it should have a `onStartShouldSetResponderCapture` handler which returns true.

 - `View.props.onStartShouldSetResponderCapture: (evt) => true,`
 - `View.props.onMoveShouldSetResponderCapture: (evt) => true,`

### PanResponder

For higher-level gesture interpretation, check out [PanResponder](/react-native/docs/panresponder.html).

##手势响应器系统
在移动设备上的手势识别比web复杂很多。一个触摸事件需要通过一系列的过程，app才能明白用户的意图。比如，app需要确定触摸事件是滚动屏幕，滑动组件还是轻击。这些事件还可能在触摸过程中进行变化，此外还有多个触摸事件同时发生的情况。
触摸响应器系统可以让各个组件处理它们触摸事件的交互而不必去了解他们的父组件和子组件。这个系统在 `ResponderEventPlugin.js` 中进行了实现，其中还包含了更加详细的文档和资料。


### 最佳实践

用户在使用web应用程序和原生应用程序时，感觉到巨大差异的最重要原因之一就在于手势识别的不同体验。（原生应用的）每一个操作都应该有以下的属性：
- 反馈/高亮 - 告诉用户哪个元素在处理他的触摸事件以及用户结束他们的手势后会得到什么样的结果。
- 撤销能力 - 当进行一个操作的时候，用户可以在触摸事件过程中通过拖拽移开自己的手指来中止本次操作。

这些特性使用户在使用应用的时候更加的舒服，因为它允许用户进行实验性的交互，而不必担心犯错。

### TouchableHighlight组件 和 Touchable*抽象类
响应器系统用起来是非常复杂的。所以我们提供了一个抽象类`Touchable` 来实现可以轻击的对象，它通过使用响应器系统可以让你轻松的以声明的方式进行轻击交互的配置。你可以在web中使用按钮或者链接的任何地方使用 `TouchableHighlight` 组件.


## 响应器的生命周期

一个视图可以通过实现正确的方法成为一个触摸响应器。以下就是询问视图是否要成为一个响应器的2种方法：
 - `View.props.onStartShouldSetResponder: (evt) => true,` - 这个视图想要在触摸的开始成为一个响应器么？
 - `View.props.onMoveShouldSetResponder: (evt) => true,` - 在一个不是响应器视图上面的触摸事件，每一次触摸移动都会询问：这个视图是不是需要声明触摸响应。

如果视图返回true，并且试图成为一个响应器，下面事件中的一个将会发生：
 - `View.props.onResponderGrant: (evt) => {}` - 现在这个视图正在响应这些触摸事件。这时候可以开始反馈用户的触摸事件了。
 - `View.props.onResponderReject: (evt) => {}` - 还有其他的东西在作为触摸的响应器，并且还未释放。
 
如果视图开始响应了，下面这些处理程序就可以被调用了：
 - `View.props.onResponderMove: (evt) => {}` - 用户在移动手指时触发
 - `View.props.onResponderRelease: (evt) => {}` - 在触摸事件结束时触发，即"touchUp"。
 - `View.props.onResponderTerminationRequest: (evt) => true` - 有其他的元素想要成为响应器。这个视图是否释放响应器？返回true则允许释放。
 - `View.props.onResponderTerminate: (evt) => {}` - 作用在这个视图上的响应器已经被其他元素取代。可能是被其他视图的 `onResponderTerminationRequest` 方法调用后所取代，或者是被系统直接取代（经常发生在iOS中的控制中心或通知中心）。

`evt` 是一个合成的触摸事件用于以下形式：

 - `nativeEvent`
     + `changedTouches` - 一个数组，它存储了自从上一个事件后所有发生改变的触摸事件。
     + `identifier` - 触摸事件的ID
     + `locationX` - 相对于元素的触摸X坐标
     + `locationY` - 相对于元素的触摸Y坐标
     + `pageX` - 相对于屏幕的触摸X坐标
     + `pageY` - 相对于屏幕的触摸Y坐标
     + `target` - 接受触摸事件元素的节点id
     + `timestamp` - 一个触摸事件的时间戳，对计算速度很有用
     + `touches` - 一个数组，记录当前屏幕上的触摸事件


### 捕获 ShouldSet 处理程序

`onStartShouldSetResponder` 和 `onMoveShouldSetResponder` 通过冒泡的方式被调用，即 最底层的组件最先被调用。这意味着最底层的组件将成为响应器，即使很多视图都对`*ShouldSetResponder`处理程序返回true。这种设计是非常合理的，它保证了所有组件和按钮都是可用的。 

但是，有时候我们希望确定一个父节点成为响应器，可以通过捕获阶段来处理。在响应器系统开始从最底层开始冒泡之前，执行一个捕获事件`on*ShouldSetResponderCapture`。所以如果一个父视图想要阻止子视图在触摸开始时成为响应器，给父视图添加一个 `onStartShouldSetResponderCapture` 处理程序并返回true。


### PanResponder

想要看更高级的手势说明，请查看 [PanResponder](/react-native/docs/panresponder.html).
