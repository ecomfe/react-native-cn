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