The most fundamental component for building UI, `View` is a container that supports layout with flexbox, style, some touch handling, and accessibility controls, and is designed to be nested inside other views and to have 0 to many children of any type. `View` maps directly to the native view equivalent on whatever platform react is running on, whether that is a `UIView`, `<div>`, `android.view`, etc. This example creates a `View` that wraps two colored boxes and custom component in a row with padding.

```html
<View style={{flexDirection: 'row', height: 100, padding: 20}}>
  <View style={{backgroundColor: 'blue', flex: 0.3}} />
  <View style={{backgroundColor: 'red', flex: 0.5}} />
  <MyCustomComponent {...customProps} />
</View>
```

`Views` are designed to be used with `StyleSheets` for clarity and performance, although inline styles are also supported.

## Props 

**accessibilityLabel** string 

Overrides the text that's read by the screen reader when the user interacts with the element. By default, the label is constructed by traversing all the children and accumulating all the Text nodes separated by space.

**accessible** bool 

When true, indicates that the view is an accessibility element. By default, all the touchable elements are accessible.

**onMoveShouldSetResponder** function 

For most touch interactions, you'll simply want to wrap your component in `TouchableHighlight` or `TouchableOpacity`. Check out `Touchable.js`, `ScrollResponder.js` and `ResponderEventPlugin.js` for more discussion.

**onResponderGrant** function 

**onResponderMove** function 

**onResponderReject** function 

**onResponderRelease** function 

**onResponderTerminate** function 

**onResponderTerminationRequest** function 

**onStartShouldSetResponder** function 

**onStartShouldSetResponderCapture** function 

**pointerEvents** enum('box-none', 'none', 'box-only', 'auto') 

In the absence of auto property, none is much like `CSS`'s `none` value. `box-none` is as if you had applied the `CSS` class:

```css
.box-none {
  pointer-events: none;
}
.box-none * {
  pointer-events: all;
}
```

`box-only` is the equivalent of

```csss
.box-only {
  pointer-events: all;
}
.box-only * {
  pointer-events: none;
}
```

But since `pointerEvents` does not affect layout/appearance, and we are already deviating from the spec by adding additional modes, we opt to not include `pointerEvents` on `style`. On some platforms, we would need to implement it as a `className` anyways. Using `style` or not is an implementation detail of the platform.

**removeClippedSubviews** bool 

This is a special performance property exposed by RCTView and is useful for scrolling content when there are many subviews, most of which are offscreen. For this property to be effective, it must be applied to a view that contains many subviews that extend outside its bound. The subviews must also have overflow: hidden, as should the containing view (or one of its superviews).

style style 

├─[**Flexbox...**](http://facebook.github.io/react-native/docs/flexbox.html#proptypes)
├─**backgroundColor** string
├─**borderBottomColor** string
├─**borderColor** string
├─**borderLeftColor** string
├─**borderRadius** number
├─**borderRightColor** string
├─**borderTopColor** string
├─**opacity** number
├─**overflow** enum('visible', 'hidden')
├─**rotation** number
├─**scaleX** number
├─**scaleY** number
├─**shadowColor** string
├─**shadowOffset** {h: number, w: number}
├─**shadowOpacity** number
├─**shadowRadius** number
├─**transformMatrix** [number]
├─**translateX** number
└─**translateY** number

**testID** string 

Used to locate this view in end-to-end tests.