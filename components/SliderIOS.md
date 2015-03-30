## Props 

**maximumValue** number 

Initial maximum value of the slider. Default value is 1.

**minimumValue** number 

Initial minimum value of the slider. Default value is 0.

**onSlidingComplete** function 

Callback called when the user finishes changing the value (e.g. when the slider is released).

**onValueChange** function 

Callback continuously called while the user is dragging the slider.

**style** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

Used to style and layout the `Slider`. See `StyleSheet.js` and `ViewStylePropTypes.js` for more info.

**value** number 

Initial value of the slider. The value should be between minimumValue and maximumValue, which default to 0 and 1 respectively. Default value is 0.

*This is not a controlled component*, e.g. if you don't update the value, the component won't be reset to it's inital value.