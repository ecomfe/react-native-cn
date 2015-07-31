`PanResponder` reconciles several touches into a single gesture. It makes single-touch gestures resilient to extra touches, and can be used to recognize simple multi-touch gestures.

It provides a predictable wrapper of the responder handlers provided by the [gesture responder system](http://facebook.github.io/react-native/docs/gesture-responder-system.html). For each handler, it provides a new `gestureState` object alongside the normal event.

A `gestureState` object has the following:

* `stateID` - ID of the gestureState- persisted as long as there at least one touch on screen
* `moveX` - the latest screen coordinates of the recently-moved touch
* `moveY` - the latest screen coordinates of the recently-moved touch
* `x0` - the screen coordinates of the responder grant
* `y0` - the screen coordinates of the responder grant
* `dx` - accumulated distance of the gesture since the touch started
* `dy` - accumulated distance of the gesture since the touch started
* `vx` - current velocity of the gesture
* `vy` - current velocity of the gesture
* `numberActiveTouches` - Number of touches currently on screeen

## Basic Usage 

```javascript
  componentWillMount: function() {
    this._panGesture = PanResponder.create({
      // Ask to be the responder:
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onStartShouldSetPanResponderCapture: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,

      onPanResponderGrant: (evt, gestureState) => {
        // The guesture has started. Show visual feedback so the user knows
        // what is happening!

        // gestureState.{x,y}0 will be set to zero now
      },
      onPanResponderMove: (evt, gestureState) => {
        // The most recent move distance is gestureState.move{X,Y}

        // The accumulated gesture distance since becoming responder is
        // gestureState.d{x,y}
      },
      onResponderTerminationRequest: (evt, gestureState) => true,
      onPanResponderRelease: (evt, gestureState) => {
        // The user has released all touches while this view is the
        // responder. This typically means a gesture has succeeded
      },
      onPanResponderTerminate: (evt, gestureState) => {
        // Another component has become the responder, so this gesture
        // should be cancelled
      },
    });
  },

  render: function() {
    return (
      <View {...this._panResponder.panHandlers} />
    );
  },
```

## Working Example 

To see it in action, try the [PanResponder example in UIExplorer](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/ResponderExample.js)

## Methods 

static **create**(config: object) 

@param {object} config Enhanced versions of all of the responder callbacks that provide not only the typical `ResponderSyntheticEvent`, but also the `PanResponder` gesture state. Simply replace the word Responder with `PanResponder` in each of the typical `onResponder*` callbacks. For example, the config object would look like:

* `onMoveShouldSetPanResponder: (e, gestureState) => {...}`
* `onMoveShouldSetPanResponderCapture: (e, gestureState) => {...}`
* `onStartShouldSetPanResponder: (e, gestureState) => {...}`
* `onStartShouldSetPanResponderCapture: (e, gestureState) => {...}`
* `onPanResponderReject: (e, gestureState) => {...}`
* `onPanResponderGrant: (e, gestureState) => {...}`
* `onPanResponderStart: (e, gestureState) => {...}`
* `onPanResponderEnd: (e, gestureState) => {...}`
* `onPanResponderRelease: (e, gestureState) => {...}`
* `onPanResponderMove: (e, gestureState) => {...}`
* `onPanResponderTerminate: (e, gestureState) => {...}`
* `onPanResponderTerminationRequest: (e, gestureState) => {...}`

In general, for events that have capture equivalents, we update the gestureState once in the capture phase and can use it in the bubble phase as well.

Be careful with onStartShould* callbacks. They only reflect updated `gestureState` for start/end events that bubble/capture to the Node. Once the node is the responder, you can rely on every start/end event being processed by the gesture and `gestureState` being updated accordingly. (numberActiveTouches) may not be totally accurate unless you are the responder.


`PanResponder`将多点触发调节成单点触发动作。该方法使单点触发动作对多点具有弹性，可以用来识别简单的多点触发动作。
PanResponder为响应处理程序提供一个可预测包，其中响应处理程序由动作应答系统提供[gesture responder system](http://facebook.github.io/react-native/docs/gesture-responder-system.html).。对每一个处理程序来说，相对于正常事件都提供了一个新的`gestureState`对象。

一个`gestureState`对象有以下属性：

* `stateID` - gestureState 的 ID- 至少保持屏幕上单点触发动作时间
* `moveX` - 最后移动触发下的最新屏幕横坐标
* `moveY` - 最后移动触发下的最新屏幕纵坐标
* `x0` - 应答器的屏幕横坐标
* `y0` - 应答器的屏幕纵坐标
* `dx` - 触发开始后累积的横向动作距离
* `dy` - 触发开始后累积的纵向动作距离
* `vx` - 当前动作的横向速度
* `vy` - 当前动作的纵向速度
* `numberActiveTouches` - 当前屏幕的触发数量

## 基本用法 

```javascript
  componentWillMount: function() {
    this._panGesture = PanResponder.create({
      // Ask to be the responder:
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onStartShouldSetPanResponderCapture: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,

      onPanResponderGrant: (evt, gestureState) => {
        // The guesture has started. Show visual feedback so the user knows
        // what is happening!

        // gestureState.{x,y}0 will be set to zero now
      },
      onPanResponderMove: (evt, gestureState) => {
        // The most recent move distance is gestureState.move{X,Y}

        // The accumulated gesture distance since becoming responder is
        // gestureState.d{x,y}
      },
      onResponderTerminationRequest: (evt, gestureState) => true,
      onPanResponderRelease: (evt, gestureState) => {
        // The user has released all touches while this view is the
        // responder. This typically means a gesture has succeeded
      },
      onPanResponderTerminate: (evt, gestureState) => {
        // Another component has become the responder, so this gesture
        // should be cancelled
      },
    });
  },

  render: function() {
    return (
      <View {...this._panResponder.panHandlers} />
    );
  },
```

## 工程实例

参考[PanResponder example in UIExplorer](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/ResponderExample.js)，可查看在实际工程中的应用。

## 方法

static **create**(config: object) 

@param {object} 提供所有应答器回调配置的增强版本，应答器回调不仅可以提供典型的`ResponderSyntheticEvent`动作状态，还可以提供`PanResponder`动作状态。在每一个典型的`onResponder*` 回调过程中，可用`PanResponder`简单替换`PanResponder`。
举个例子，config对象可能看起来如下所示：

* `onMoveShouldSetPanResponder: (e, gestureState) => {...}`
* `onMoveShouldSetPanResponderCapture: (e, gestureState) => {...}`
* `onStartShouldSetPanResponder: (e, gestureState) => {...}`
* `onStartShouldSetPanResponderCapture: (e, gestureState) => {...}`
* `onPanResponderReject: (e, gestureState) => {...}`
* `onPanResponderGrant: (e, gestureState) => {...}`
* `onPanResponderStart: (e, gestureState) => {...}`
* `onPanResponderEnd: (e, gestureState) => {...}`
* `onPanResponderRelease: (e, gestureState) => {...}`
* `onPanResponderMove: (e, gestureState) => {...}`
* `onPanResponderTerminate: (e, gestureState) => {...}`
* `onPanResponderTerminationRequest: (e, gestureState) => {...}`

一般来说，对于所捕获的等价事件，我们可在捕获阶段或冒泡阶段更新一次gestureState。
使用过程中要注意onStartShould*回调。该回调只可对节点捕获/冒泡的开始/结束事件更新后的`gestureState`做出变化。一旦节点成为应答器，每一个动作的开始/结束事件后，
`gestureState`都会有相应的更新。(numberActiveTouches)并不完全正确（应答器除外）。
